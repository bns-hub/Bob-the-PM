---
repair_version: 2
repair_id: obsidian-repair-20260923-viewer-friendly-note-normalization
status: queued_for_local_writer
created_at: "2026-09-23T15:57:00+08:00"
expanded_at: "2026-09-23T16:10:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
drive_mode: read_only
priority: user-request-all-notes
ux_standard: OBSIDIAN-NOTE-UX-STANDARD.md
scope: full-vault
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
