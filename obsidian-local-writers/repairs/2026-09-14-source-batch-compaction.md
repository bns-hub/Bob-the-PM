---
repair_version: 1
repair_id: obsidian-repair-20260914-source-batch-compaction
status: pending
created_at: "2026-09-14T17:05:00+08:00"
created_by: chatgpt-cloud
scope: gmail-hubspot-raw-batch-clutter
max_target_files_per_run: 50
source_of_truth: OBSIDIAN-GRAPH-HYGIENE.md
archive_drive_folder_id: "1T2X6ngN73ZgOOCqGVXoG0XIdybJAo93h"
---

# Gmail + HubSpot source-batch compaction

## User-visible problem

The global Obsidian graph is still dominated by raw audit containers such as:

- `2025-09-25 onward Gmail Batch 008 Filtered Audit` and the many date-range Gmail Batch 008 filtered-audit files;
- `HubSpot Emails - <date/range> - Records <range>` files;
- `HubSpot Notes - <date/range> - Records <range>` files;
- similar HubSpot Contacts and Companies range files.

These files are evidence containers, not business knowledge owners. They must not continue to dominate the live `Ben` graph once their evidence has been safely promoted and archived.

Read `OBSIDIAN-GRAPH-HYGIENE.md` before processing. Do not delete raw evidence simply because it looks cluttered.

## Required outcome

Keep the live `Ben` vault focused on canonical business/project/tender/SR/CR/account/deal knowledge plus compact source navigation. Preserve raw historical evidence outside the live vault in the authorized audit/checkpoint storage.

## Processing order

Process one family per run, maximum 50 files:

1. Gmail Batch / Filtered Audit files
2. HubSpot Emails range files
3. HubSpot Notes range files
4. HubSpot Contacts range files
5. HubSpot Companies range files
6. other pure HubSpot object-range files only after the above are complete

Persist exact resume position after each bounded run.

## Per-file safety sequence

For every candidate raw batch file:

1. Re-read the live file through Obsidian MCP and confirm it is a pure source/audit batch, not a canonical business owner.
2. Identify substantive records/facts and verify each is already represented by a canonical owner or useful activity note. If any substantive knowledge exists only inside the batch, promote/link it before archiving the batch.
3. Record original vault path, stable provider/source range, record/message IDs when available, record count, and full content hash.
4. Create a complete archive copy in the authorized audit/checkpoint Drive storage outside the live `Ben` vault. Include original path and metadata in the archive copy or companion manifest.
5. Re-read and hash-verify the archive copy. If verification fails, stop for that file and leave the live batch untouched.
6. Repair direct wikilinks so business/project notes point to the proper canonical owner or compact source MOC instead of depending on the raw batch file.
7. Through the locked local Codex processor and Obsidian MCP, remove the verified raw batch file from the live `Ben` vault.
8. Update the appropriate compact source MOC/checkpoint with original path, source range, archive location/file ID, provider IDs, and content hash.
9. Re-scan the affected area for broken wikilinks and verify the canonical knowledge chain is intact.

## Never archive/remove

Do not archive/remove merely because a note originated from Gmail/HubSpot if it is actually one of these:

- canonical project/tender/SR/CR/deal/account owner;
- human-readable promoted activity with independent business meaning;
- useful MOC required for navigation;
- user-authored note/capture;
- unresolved item whose business ownership has not yet been safely determined.

## Verification / completion

After each bounded family run, update this repair file with:

- `status`: pending / blocked / completed
- family processed
- files examined
- files archived and removed from live vault
- files retained and why
- archive Drive IDs/paths
- before/archive hashes
- promoted/linked canonical owners
- broken-link check result
- exact next resume file/family
- processor device
- completed/checked timestamp

This repair remains pending until Gmail, HubSpot Emails, HubSpot Notes, HubSpot Contacts, and HubSpot Companies raw range/batch clutter has been processed across the full live vault.
