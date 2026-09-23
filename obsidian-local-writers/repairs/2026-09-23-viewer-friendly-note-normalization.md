---
repair_version: 2
repair_id: obsidian-repair-20260923-viewer-friendly-note-normalization
status: awaiting_local_writer
created_at: "2026-09-23T15:57:00+08:00"
expanded_at: "2026-09-23T16:10:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
drive_mode: read_only
priority: user-request-all-notes
ux_standard: OBSIDIAN-NOTE-UX-STANDARD.md
scope: full-vault
run_requested_at: "2026-09-23T17:03:00+08:00"
run_requested_by: Benson Foo
cloud_preflight: drive_root_verified_read_only_no_obsidian_mcp_write_surface
---

# Full-vault viewer-friendly normalization — 2026-09-23

## User direction

Apply the viewer-friendly note standard to **ALL NOTES** in the live `Ben` vault, while preserving note purpose, user-authored content, stable IDs, links, provenance, and safe writing rules.

This supersedes the earlier bounded target set in version 1 of this repair.

## Goal

Make the entire vault consistently easy to:
- scan;
- understand;
- act from;
- add quick notes to;
- navigate by project/account/person/date;
- maintain without exposing ingestion machinery.

The machine may process files deterministically in any safe order. Human-facing order follows `OBSIDIAN-NOTE-UX-STANDARD.md`.

## Scope

Enumerate the entire live vault through authenticated Obsidian MCP, including all supported human-readable notes under:

- `00 Home/`
- `01 Inbox/`
- `A Atlas/`
- `C Calendar/`
- `E Efforts/`
- `S Sources/`
- `X Filtered Out/` when a note remains intentionally human-readable
- `Z System/`
- any valid legacy/root Markdown notes not yet routed
- Markdown companion notes associated with drawings, Canvas files, or attachments where applicable

Do not modify `.obsidian/`, plugin state, caches, hidden application data, or binary attachment internals.

Excalidraw/Canvas content must not be reformatted merely to imitate Markdown templates. Apply metadata/navigation improvements only when safe and supported.

## Note-type-aware normalization

For every note, determine its primary purpose using verified folder context, frontmatter, stable IDs, owner relationships, and actual content. Then apply the corresponding pattern in `OBSIDIAN-NOTE-UX-STANDARD.md`.

Covered types include:
- project;
- deal;
- campaign/account/sales effort;
- tender/bid;
- service request;
- change request;
- personal project/planning/purchase;
- meeting;
- person/contact;
- organisation/account;
- daily/calendar;
- task/action;
- decision;
- reference/evergreen knowledge;
- source/evidence/imported email/CRM record;
- MOC/index/dashboard;
- Inbox control;
- system/automation/runbook;
- archive/historical.

If primary note type cannot be proved, leave content intact and record `ux_type: unresolved` in this repair report. Do not guess.

## Universal visible-content rules

For every note where applicable:

1. Put the information most useful to Benson near the top.
2. Use human-readable wording in the visible body.
3. Keep machine IDs, hashes, capture IDs, provider IDs, sync/claim state, and other ingestion metadata in frontmatter/source/audit records unless the note's purpose is system operation.
4. Remove visible `Inbox Activity`, `Processed Captures`, `Capture Ledger`, or equivalent ingestion-only sections from normal human-facing notes only after their unique facts are preserved appropriately.
5. Preserve all user-authored prose and notes.
6. Do not create empty headings just to satisfy a template.
7. Prefer wikilinks to duplication.
8. Preserve canonical IDs and all valid relationships.
9. Keep active/recent human-facing lists newest-first where the UX standard says so.
10. Keep `Working notes` as a stable free-form space on active working notes; do not auto-sort or rewrite it.

## Date/order behavior

- Active project/deal/tender/campaign updates: newest first.
- Meeting lists and recent interactions: newest first.
- Decision/change logs: newest first unless the note deliberately uses another documented structure.
- Capture Here: newest raw capture first.
- Daily note activity: preserve useful time order for that day.
- Reference/how-to/runbook: logical step order.
- Source/audit/history: preserve evidentiary/source order; add a readable summary rather than destructively reordering evidence.
- Processing order remains independent.

## Migration strategy

This is a full-vault goal but must remain safe and resumable.

Process in deterministic batches of at most **50 notes per local run**. Continue on subsequent local runs until every eligible note has been inspected.

Recommended stable sweep order:
1. `00 Home/`
2. `01 Inbox/`
3. `E Efforts/`
4. `A Atlas/`
5. `C Calendar/`
6. `S Sources/`
7. `Z System/`
8. `X Filtered Out/`
9. supported legacy/root notes

Within each folder, use stable path order for inspection. Record the last inspected path and next resume path after every batch.

Do not repeatedly rewrite already-compliant notes. Mark them inspected/no-change.

## Priority cleanup

During the first passes, prioritize:
- visible `Inbox Activity` or processed-capture ledgers in working notes;
- ascending activity logs in active project/deal/tender/campaign notes;
- action items buried below long prose;
- meeting notes where actions/outcomes are hard to find;
- person/account notes dominated by provider metadata;
- orphaned human notes lacking an obvious canonical owner/navigation link;
- active notes with no safe place for Benson to type quick working notes.

## Safety and verification

For every batch:

1. Refresh authoritative repository rules, including `OBSIDIAN-NOTE-UX-STANDARD.md`.
2. Acquire the shared processor lock.
3. Authenticate Obsidian MCP and validate active vault exactly `Ben`.
4. Enumerate target notes through MCP.
5. Re-read each note immediately before editing.
6. Make the smallest content-preserving UX patch.
7. Re-read every changed note through MCP.
8. Verify frontmatter/stable IDs/capture IDs/hashes/user content/links/path.
9. Run broken-link checks on all affected clusters.
10. Record results and resume cursor here.
11. Release the processor lock.

No Google Drive writes to the live vault. No direct filesystem writes.

## Completion criteria

Set `status: completed` only after one full deterministic scan of every eligible note confirms each note is either:

- compliant;
- intentionally exempt because its original/source/archive structure is the correct UX;
- or explicitly recorded as unresolved with the minimum user decision required.

## Completion metrics

Track:
- total eligible notes inspected;
- total notes changed;
- already-compliant notes;
- project/deal/tender/campaign notes normalized;
- meeting notes normalized;
- person/contact notes normalized;
- organisation/account notes normalized;
- daily/calendar notes inspected;
- reference notes normalized;
- source/evidence notes normalized or intentionally preserved;
- MOC/dashboard notes normalized;
- system/runbook notes normalized;
- visible Inbox Activity / capture-ledger sections removed;
- reverse-chronology sections fixed;
- Working notes areas created/preserved;
- unresolved UX types;
- broken links before/after;
- last inspected path;
- next resume path.

## User preference questions

Do not block the first safe normalization batches on unanswered preference questions. Apply the universal defaults above. Where a preference would materially affect information loss, note structure, or whether a section should exist, preserve the current content and add the question to the repair report / Inbox decision surface for Benson.

## Home and capture surfaces — user-approved 2026-09-23

During the first safe full-vault batches, create/reuse and verify these human-facing surfaces through Obsidian MCP:

- `00 Home/My Tasks.md` — generated navigation view of open `Next actions` across owning notes. Do not make dashboard copies authoritative.
- `00 Home/Ideas.md` — sections `New / Unsorted`, `Worth exploring`, `Promoted`.
- `00 Home/Maintenance.md` or an equivalent compatible Home/Base view — active maintenance obligations and projects with lifecycle `maintenance`.
- update `00 Home/Home.md` so the primary launch order is:
  1. Today
  2. My Tasks
  3. Active Projects
  4. Live Deals
  5. Inbox / Needs Your Decision
  6. Ideas
  7. Maintenance

Also update active project/deal/person/organisation surfaces as defined in `BOB-CAPTURE-INTELLIGENCE.md`:
- person `Recent interactions`;
- organisation/account `Relationship pulse`;
- source/evidence mostly excluded from ordinary Home/default navigation;
- maintenance lifecycle remains visible and actionable rather than archived away.

Do not depend on Obsidian 1.14-only Kanban behavior. Use the current public-build-compatible layout already established in this vault.
## Smart inference and relationship-display migration — 2026-09-23

As the full-vault normalization touches person and organisation notes:

1. person pages should retain/create full `Recent interactions` entries when supported;
2. organisation/company pages should use shortened `Relationship pulse` entries;
3. each shortened organisation interaction should end with an immediate `[[Person|Full notes →]]` link;
4. remove duplicated long-form interaction prose from organisation pages only after verifying it remains preserved in the person/meeting/activity/source record;
5. preserve project-specific updates separately on the project/deal owner;
6. do not create duplicate people: check canonical person identity and authorised HubSpot records first.

For new captures, use the updated inference defaults in `BOB-CAPTURE-INTELLIGENCE.md`: infer first, flag assumptions, then ask/approve.

## Execution request / cloud preflight — 2026-09-23 17:03 SGT

Benson explicitly instructed: **process it all**.

Cloud preflight completed:

- authoritative live-vault read-only Drive root resolved successfully;
- expected roots were present: `00 Home`, `01 Inbox`, `A Atlas`, `C Calendar`, `E Efforts`, `S Sources`, `X Filtered Out`, `Z System`;
- `.obsidian` was visible and remains excluded from all modification;
- no Obsidian MCP action/tool is exposed to this cloud chat session;
- no compatible Obsidian ChatGPT plugin was available to replace the approved local MCP write path;
- no repository-defined remote trigger for the local Codex writer was found;
- latest laptop writer heartbeat observed: `2026-09-21T07:48:00Z` (15:48 SGT), previously authenticated to vault `Ben`;
- latest PC1 writer heartbeat observed: `2026-09-14T12:39:30Z`;
- therefore cloud did **not** perform prohibited Google Drive live-vault writes and did not claim the normalization was completed.

### Required next writer action

At the next local writer execution, this repair is an explicit run request, not merely a suggestion:

1. refresh `BOB-CAPTURE-INTELLIGENCE.md`, `OBSIDIAN-NOTE-UX-STANDARD.md`, `OBSIDIAN-GRAPH-HYGIENE.md`, `OBSIDIAN-INGESTION-CONTRACT.md`, and `CODEX-CLOUD-INSTRUCTIONS.md`;
2. acquire the processor lock;
3. authenticate Obsidian MCP and confirm active vault exactly `Ben`;
4. begin/continue the full-vault deterministic batches defined above;
5. do not stop after Bob-created notes; inspect every eligible note;
6. create/update Home surfaces, person full notes, company Relationship pulse summaries + immediate Full notes links, Ideas, My Tasks, Maintenance, daily note UX, source invisibility, newest-first human activity ordering, and removal of visible ingestion ledgers as specified;
7. record batch metrics and resume cursor after each verified batch;
8. continue on subsequent local writer runs until the completion criteria are met.

If the local writer starts and cannot authenticate to `Ben`, update this repair with the exact failure state instead of changing the vault by another route.

