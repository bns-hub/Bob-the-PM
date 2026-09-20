# Bob to Obsidian ingestion contract

This is the repository-side contract for phone capture, queueing, claiming, local Obsidian writing, verification, and cleanup. It supplements `OBSIDIAN-DELIVERY-ARCHITECTURE.md`. If wording elsewhere conflicts with this contract, the stricter safety rule applies.

## One flow

`Phone or chat capture -> GitHub queue -> one local Codex processor -> Obsidian MCP -> local Ben vault -> verification -> GitHub completion`

Bob is the capture layer. GitHub is the durable queue. A local Codex session is the only final writer. Obsidian MCP is the only approved write route. Google Drive is a later sync layer, not a write target for Bob, Claude, cloud ChatGPT, or cloud Codex.

Claude may add captures to this GitHub queue. Claude must never write, edit, rename, move, or delete a file in the Drive-backed vault. Claude must never create or take a processor lock.

## Capture record version 1

Every new entry under `## New captures` must have one metadata comment immediately before the exact captured text.

```markdown
<!-- bob-capture:v1
capture_id: bob-20260914-example-01
captured_at: 2026-09-14T16:30:00+08:00
classification: pa
content_sha256: sha256:0123456789abcdef
-->
task: Exact words supplied by the user.
```

Rules:

1. `capture_id` is created once, when Bob first stages the capture. It is lowercase and unique. Use `bob-YYYYMMDD-<short-readable-slug>-<two-digit-occurrence>`.
2. `captured_at` is an International Organization for Standardization 8601 timestamp with the capture device's known offset. Use `unknown` only when migrating an older entry.
3. `classification` is exactly `pending`, `pa`, or `pm`.
4. `content_sha256` is the SHA-256 hash of the exact user-supplied words, encoded as UTF-8, with line endings normalised to line feed and without an added final line break. Bob-added routing prefixes and metadata are not included in the hash.
5. The exact user-supplied words must remain present in the visible entry. Existing prefixes remain valid. A pending item keeps `pending-classification:`. A project decision keeps `project: [<project name>]`.
6. Separate entries with a line containing only `---`. Newest entries remain at the top.
7. Classification may change, but `capture_id`, `captured_at`, `content_sha256`, and the user's exact words must not change.
8. Older plain entries remain valid. Before claiming one, local Codex must add version 1 metadata in a separate GitHub commit while preserving its exact text.

Identical words captured twice receive different capture IDs and the same content hash. This preserves genuine repeated captures while still exposing a possible duplicate for review.

## Single processor lock

Desktop and laptop must never process the queue at the same time. Before inspecting a routable item for writing, local Codex must obtain the repository-wide lock at `obsidian-local-writers/processor-lock.md`.

The lock contains:

```yaml
---
lock_version: 1
status: active
device_id: "DEVICE-NAME"
writer: codex
started_at: "2026-09-14T08:30:00Z"
heartbeat_at: "2026-09-14T08:30:00Z"
expires_at: "2026-09-14T08:45:00Z"
---
```

Lock acquisition is deterministic:

1. Fetch and fast-forward from `origin/main`.
2. If an unexpired active lock exists, stop and report `another local processor is active`.
3. Otherwise create or replace the lock, commit it directly to `main`, and push.
4. If the push is rejected, fetch again. The device whose lock is now on `origin/main` owns the run. Every other device stops without writing to Obsidian.
5. Refresh `heartbeat_at` and `expires_at` before each item. The lease is 15 minutes.
6. A stale lock may be replaced only after re-reading `origin/main`, confirming `expires_at` has passed, and recording the previous owner and `recovery_reason: stale_lock` in the replacement lock.
7. Delete the lock, commit, and push when the run finishes or stops safely.

This shared lock prevents simultaneous queue processing. Each item also has its own durable claim and completion record.

## Item claim

For each routable item, use this exact path:

`obsidian-local-writers/claims/<capture_id>-<first-16-hex-of-content-sha256>.md`

The claim starts with `claim_status: active` and records the capture ID, full hash, device, start time, source path, and source Git blob identity. Local Codex must commit and push the active claim before any Obsidian write. A completed claim is never deleted.

If the claim path already says `completed` with the same full hash, treat that occurrence as delivered and do not write it again. If the same capture ID appears with a different hash, stop with `capture identity conflict`. If another active claim exists, stop unless it is stale and the current device also holds the processor lock.

Use both identity checks. An exact capture ID and hash pair is the same occurrence. A matching hash under a different capture ID is a possible repeated capture. Do not create duplicate prose automatically. Reuse the verified existing owning note and add the new capture ID to its metadata when the evidence shows the same real-world item is already represented. If that cannot be confirmed, preserve the new occurrence and mark it `unresolved_routing` rather than silently discarding it.

## Local Codex processing order

After taking the processor lock, local Codex must:

1. Confirm an authenticated Obsidian MCP round trip with `get_server_info` or the connector's equivalent. Confirm that the active vault is exactly `Ben`. A saved setting, open port, or visible local folder is not enough.
2. Read only through the Obsidian MCP tools needed for search, note inspection, writing, and verification. Never write the vault by direct filesystem access or a Google Drive connector.
3. Read all queue entries. Skip `classification: pending` and legacy `pending-classification:` entries.
4. Process routable entries oldest first by `captured_at`, then `capture_id`. Treat `captured_at: unknown` as older than known timestamps and break ties by `capture_id`.
5. Create and push the item claim.
6. Search Obsidian frontmatter for the exact `capture_id` and `content_sha256`. Also search verified project, person, organisation, tender, service request, change request, and account identifiers supported by the capture.
7. Prefer the existing owning note when evidence supports it. Make the smallest content-preserving append or patch. Preserve existing frontmatter, links, headings, and user text. Do not choose a note by title similarity alone.
8. If no supported owner exists, create one note in the established ACES folder structure. If routing is unclear, leave the queue item and claim as `unresolved_routing`. Do not guess.
9. Store the capture ID and content hash in the destination note frontmatter. For a note that represents several captures, use `capture_ids` and `content_hashes` lists and preserve any existing singular fields.
10. Re-read the exact destination through Obsidian MCP. Verify the capture ID, full hash, exact intended content, and target path.
11. Mark the claim `completed` only after that MCP re-read succeeds. Record `destination_path`, `local_verified_at`, and the verification result.
12. Re-fetch `origin/main`, re-read the current staging file, remove only the matching entry, and preserve every concurrent capture. Commit and push the cleanup. If the push is rejected, repeat the safe re-read and targeted removal. Never replace the whole staging file from a stale copy.
13. Update the device status, then release the processor lock.

An optional later sync check may update `sync_status`, but it is not allowed to write the note or change the verified destination. The local Obsidian MCP re-read is the delivery proof.

## Failure states

Use these plain states and keep the queue entry intact unless the claim was already completed and locally verified:

- `mcp_unavailable`
- `mcp_authentication_failed`
- `wrong_vault`
- `another_local_processor_active`
- `capture_identity_conflict`
- `unresolved_routing`
- `write_failed`
- `verification_failed`
- `github_conflict`

No failure state counts as delivery. Never clear a capture merely because a claim exists, a local file path exists, or a Drive copy exists.

## Scope, Inbox-file, and new-project rules — 2026-09-21

The capture classification field (`pending|pa|pm`) does not encode Personal/Work scope.

For each routable item, resolve an independent controlled scope:

- `personal`
- `work`
- `unresolved`

Never guess this value from PA/PM classification. Route effort-owned content to `E Efforts/Personal/` or `E Efforts/Work/` only after scope is supported.

The processor must also enumerate every file in the live `01 Inbox/` folder, not only block captures inside `Capture Here.md`. Each file receives the same canonical-owner, deduplication, link/tag/property, verification, and cleanup protections as a block capture.

If an Inbox item clearly belongs to an existing canonical project, all durable child objects (including meetings, CRs, SRs, decisions, tasks, and source notes) must be linked to that project and normally live under its project structure. Do not create an unrelated standalone owner.

If an item appears to describe a new project/deal but the user has not explicitly created/implied one, leave it in Inbox as `unresolved_routing`. Add the minimal question to the Inbox `Needs Your Decision` section and surface it during the weekly review.

When a project is renamed, preserve its stable `project_id`; rename the canonical project hub and propagate references through the approved local Obsidian MCP workflow rather than renaming children independently.

## Canonical name resolution and people relationships — 2026-09-21

For work projects and work deals, the destination owner's canonical display name is `<Company Name> - <Project Name>`. Benson may type any reasonable shorthand. Do not rewrite the user's captured words; instead resolve the capture to the existing canonical owner using stable IDs, provider IDs, organisation/account identity, aliases, tender/reference context, and verified linked people.

Do not create a duplicate project/deal solely because punctuation, capitalization, acronym, spacing, or word order differs.

For person/contact routing, treat the person as an independent entity. A contact may belong to a partner, vendor, subcontractor, customer, internal organisation, or another organisation while still being related to the project. Maintain explicit organisation wikilinks plus project wikilinks and controlled relationship types. Do not assume that every project-related person belongs to the project's customer organisation.

The two persistent Inbox control files are not queue items to delete:
- `Capture Here.md` persists; only verified processed capture blocks are removed.
- `Inbox.md` persists; only resolved dashboard/decision entries are removed or updated.
