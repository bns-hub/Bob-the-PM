# Obsidian delivery architecture

This file is the authoritative device-and-delivery model for Bob-the-PM captures and Obsidian. It overrides any older or conflicting wording elsewhere in this repository about where a capture is finally written, what counts as successful Obsidian delivery, or when a GitHub handoff may be cleared.

## Canonical flow

You -> Bob capture -> GitHub source queue -> one locked local Codex processor -> Obsidian Model Context Protocol -> local `Ben` vault -> local verification -> clear the matching transient GitHub capture -> normal vault sync.

GitHub source queues include:
- `obsidian-temp-notes/01. Inbox/Capture Here.md` and other staged files under `obsidian-temp-notes/`;
- `daily-agenda/notes.md` when present; and
- every project folder's `notes.md`.

`daily-agenda/notes.md` and project `notes.md` are immutable history. Exporting an entry never deletes or rewrites that source history. Only transient handoff entries such as successfully delivered entries under `Capture Here.md` may be cleared.

## Authoritative write surface

The authoritative destination for new Obsidian notes is the actual local Obsidian vault named `Ben`. The only approved write route for this ingestion workflow is an authenticated local Obsidian Model Context Protocol connection.

A new `.md` file created only through the cloud Google Drive connector is not proof of Obsidian delivery, even if its parent folder is inside the Drive view of the vault. A cloud-created file can be owned by the work Google account and fail to materialize in the personal-account local filesystem that Obsidian is watching.

Therefore:
- cloud ChatGPT/Codex/Claude must not claim a new capture reached Obsidian merely because a Drive file exists;
- for a new vault note, the final write must be made by local Codex through Obsidian Model Context Protocol while that device holds the shared GitHub processor lock;
- a visible folder, saved connection setting, or open port is not proof of access. Local Codex must complete an authenticated Model Context Protocol round trip and confirm that the active vault is `Ben`;
- local verification must re-open the exact written `.md` through Obsidian Model Context Protocol and confirm the expected stable capture/source ID, content hash, and intended content;
- Google Drive then synchronizes that same local file to the cloud;
- the cloud `Obsidian Export` task may use the Drive view for read-only context and optional post-sync confirmation, but not for final deduplication or writing;
- the transient GitHub handoff remains authoritative until delivery is verified.

## Approved local processor and writer

### Local Codex

Local Codex running on a desktop or laptop is the sole approved processor and writer for this workflow. It must use Obsidian Model Context Protocol for both the write and the verification read.

### Bob, Claude, and cloud tasks

Bob, Claude, ChatGPT cloud, and Codex cloud may stage or coordinate captures through GitHub. They are not vault writers. Claude must never write directly to Google Drive or to the Obsidian vault, whether it runs locally or in the cloud.

### Writer claim / race-prevention rule

Desktop and laptop local Codex may both be available, but only one may process the queue at a time. Before inspecting routable content for a write, local Codex must obtain the repository-wide lease defined in `OBSIDIAN-INGESTION-CONTRACT.md`. A rejected GitHub push means that device did not obtain the lock and must stop without writing to Obsidian.

Before the lock owner writes a routable capture/source item, it must also create and push a deterministic GitHub claim keyed by the item's stable capture/source ID plus content hash. The claim identifies the writer, device, source identity, and start time. Another writer that sees a current valid claim must skip that item. A stale claim may be recovered only while holding the repository-wide lock and after re-reading the source.

After a successful local write and local verification, the writer records completion for that capture/source ID and hash and releases/completes the claim. The transient GitHub source itself is still not cleared until the synced-vault verification rule below passes.

## Device roles

### PC / laptop / desktop

Local Codex is the only approved processor and writer for this workflow.

Before writing, local Codex must:
1. complete an authenticated Obsidian Model Context Protocol round trip and confirm the active vault is exactly `Ben`;
2. confirm the expected vault structure through Obsidian Model Context Protocol and that the required write tools are available;
3. never touch `.obsidian/`, lock/cache/workspace/plugin-state files, or hidden Obsidian application state;
4. read the current Bob instructions and GitHub source queue;
5. obtain the deterministic GitHub claim for the capture/source item;
6. deduplicate by stable capture/source ID and content hash;
7. write or minimally patch the appropriate `.md` through Obsidian Model Context Protocol;
8. re-read through Obsidian Model Context Protocol and verify the note contains the expected ID, hash, and content;
9. record writer completion/heartbeat status in the shared GitHub writer-status area when available;
10. allow the normal sync layer to propagate the file.

### Android

Android Obsidian is a synced vault consumer for this workflow. Bob may capture from Android into GitHub. Android must not claim or process the queue. Leave the handoff queued for a desktop or laptop local Codex processor.

### iPhone / iPad

Treat iPhone and iPad as capture devices and synced vault consumers. Bob may capture from them into GitHub. A mobile ChatGPT or Claude session must not write directly into the iOS Obsidian vault or Google Drive.

## Local writer discovery / status

Desktop and laptop local Codex processors publish a small status or heartbeat file under `obsidian-local-writers/` so cloud orchestration can distinguish an available processor from an unavailable one.

A writer status should include at least:
- `device_id`
- `device_type`
- `writer` (`codex`)
- `writer_enabled`
- `last_seen`
- `vault_validation_status`
- `resolved_local_vault_path` or a privacy-safe path fingerprint
- `last_successful_capture_id`
- `last_successful_content_hash`
- `last_error`
- `sync_status` when known

The cloud task cannot discover local paths by itself. It can only observe these status records and later verify the synchronized vault result.

## Cloud `Obsidian Export` role

`Obsidian Export` runs daily and orchestrates the queue. Every run must inspect:
- all staged content under `obsidian-temp-notes/`, including `01. Inbox/Capture Here.md`;
- `daily-agenda/notes.md` when present;
- every project folder's `notes.md`; and
- `obsidian-local-writers/` status/heartbeat records when present.

The cloud task may classify, route, assign stable IDs and hashes, inspect GitHub coordination state, and report queue status. It must not create or edit a `.md` inside the live `Ben` vault. Final deduplication, enrichment, writing, and verification belong to the locked local Codex processor because those steps depend on an authenticated Obsidian Model Context Protocol round trip.

If a new local note is required and no valid local writer is available, status is `awaiting local writer`.

If a writer exists but cannot find or validate its local `Ben` vault, status is `local vault not found`.

If local write and local verification succeeded but the synchronized destination is not yet visible/verified to cloud, status is `awaiting sync verification`.

None of those states count as successful Obsidian delivery.

## Verification and cleanup

A transient GitHub capture may be removed only after all of these are true:
1. the intended destination note exists in the local `Ben` vault and was re-read through Obsidian Model Context Protocol;
2. the destination contains the expected stable capture/source ID and content hash;
3. the content matches the intended capture/export;
4. the result is not merely a cloud-created work-owned Drive file or a direct filesystem write masquerading as an Obsidian Model Context Protocol result;
5. the current GitHub staging file is re-read immediately before removal so concurrent edits are preserved.

If any condition fails, leave the GitHub handoff intact.

For `daily-agenda/notes.md` and project `notes.md`, never delete source entries after export. Record their exported source identity in destination/checkpoint metadata instead.

## Sync responsibilities

Obsidian itself indexes files already present in its local vault. The sync layer is responsible for moving those local files between devices/cloud storage.

Under the current architecture:
- the one locked local Codex processor creates or updates the `.md` in `Ben` through Obsidian Model Context Protocol;
- local Codex re-opens and verifies the note through Obsidian Model Context Protocol;
- Obsidian notices/indexes the local file;
- Google Drive for desktop or the approved device sync mechanism uploads/synchronizes it;
- optional cloud verification may observe the synchronized copy afterward;
- the transient GitHub capture is cleared only after the local Model Context Protocol verification and safe source re-read defined in `OBSIDIAN-INGESTION-CONTRACT.md`.

Do not invert this into `cloud Drive API creates file -> assume Obsidian received it`.

## Mandatory full-Inbox sweep — 2026-09-21

In addition to the GitHub source queues above, the daily/weekly organization workflow must account for **every file under the live vault's `01 Inbox/` folder**. The cloud task may inspect the synchronized view read-only and create deterministic repair manifests, but only the locked local Codex writer may move, rename, patch, link, tag, or clear those live-vault files through authenticated Obsidian MCP.

Do not equate "read Capture Here.md" with "Inbox processed." A complete sweep enumerates all live Inbox files plus all staged GitHub Inbox files, resolves Personal/Work scope and the canonical owner, applies controlled links/properties/tags, verifies the destination locally, and leaves unresolved items in Inbox for Benson's decision.

Ambiguous scope/ownership is surfaced in the Inbox's `Needs Your Decision` section and in the weekly review; it is never guessed.

## Persistent Inbox control files — 2026-09-21

Treat the live-vault files `01 Inbox/Capture Here.md` and `01 Inbox/Inbox.md` as persistent control surfaces:

- `Capture Here.md` = capture/input buffer;
- `Inbox.md` = read/triage dashboard and `Needs Your Decision` surface.

Do not delete either file during processing. Clear only an individually verified capture block from `Capture Here.md`, and only an individually resolved dashboard entry from `Inbox.md`.

Other files in `01 Inbox/` are transient until routed. After reading and preserving their content, either move the durable note under its verified canonical owner or merge its information into the correct owner. Remove the transient source only after Obsidian MCP re-read verifies that the destination preserves the intended content and relationships.

For work routing, use the canonical owner naming pattern `<Company Name> - <Project Name>`. User-entered shorthand is input, not identity. Resolve it to the existing canonical project/deal by stable IDs, organisation owner, aliases, and verified context.
