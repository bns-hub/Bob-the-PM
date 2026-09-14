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

## 2026-09-14 analysis checkpoint

- Cloud synced-view count: 29 Gmail filtered-audit Drive objects, representing 28 distinct logical filenames because one title occurs twice.
- HubSpot source MOCs report 87 Email batches, 39 Note batches, 27 Contact batches, and 11 Company batches.
- Canonical source MOCs exist for Gmail Activities, Email Sources, HubSpot Emails, HubSpot Notes, HubSpot Contacts, and HubSpot Companies.
- Current source MOCs still link directly to raw batch files. Archive pointers must replace those links before any corresponding local file is removed.
- The duplicate title `2025-09-17 to 2025-09-25 Filtered Audit.md` has two different Drive IDs and different hashes. Match the single local-vault file by full content hash before selecting the archive source. Never merge or delete by title alone.
- No live source-batch file was removed during this checkpoint.

### Next bounded batch: Gmail 1 of 3

Process these ten Drive objects in this exact order, stopping on the first unresolved archive or ownership prerequisite:

| Order | Title | Drive ID | Synced-view SHA-256 |
|---:|---|---|---|
| 1 | 2025-09-17 to 2025-09-25 Filtered Audit.md | `1OG-kXHmK85iAl-up2adbbUxd3YyVaaCw` | `81a6639cf478f725c6605a7ffada84da5cd067cdb7335833b36207a03c7f0bb0` |
| 2 | 2025-09-17 to 2025-09-25 Filtered Audit.md | `1WNBZ6BTFs1dyJGCzqtzuyCRBjCHRapTE` | `90a9bc7d10a2c7c78c0100827c2bf68d51dfa504d1977b6ec8018e36a4b63bcc` |
| 3 | 2025-09-25 onward Gmail Batch 008 Filtered Audit.md | `1MmEHMp1OeLCCEr_BaXPuZzvPeHLmoQ-a` | `fbe59e94f6d8a37a3ef60129a2994bccebf9c0ecf61686ede178dd7d183c120d` |
| 4 | 2025-10-01 to 2025-10-14 Gmail Batch 008 Filtered Audit.md | `1vvrEcoU8SVE4JBwp6Y8yjwKx4Mbu01Ch` | `faea770d3ce90ae9b14076a773f21758faab0b42ad282d753aa95f75424c0476` |
| 5 | 2025-10-14 to 2025-10-28 Gmail Batch 008 Filtered Audit.md | `1Nfkli_vrIqveDzUgo2015QlSFnX0pQ6T` | `8e3e0afa6a56a7c900ff9971acf38a51980f61fe5c2f8f09060c9f7cdb51e7da` |
| 6 | 2025-10-28 to 2025-11-04 Gmail Batch 008 Filtered Audit.md | `1iHR6Y6SeqxUI5Bmy8dSf0HVqCVYB8K4P` | `e8e348715763962dd462cadefb185a464bb38b38726f30180c93ca87aca6ec1b` |
| 7 | 2025-11-05 to 2025-11-18 Gmail Batch 008 Filtered Audit.md | `1h_dLJwy3PXlxUJSx6W-Sc5YVns8QkQfv` | `940856b90923a2c3c81a2b4945768b54eb0c0713a0ba1a339d5b2c2de510f5ef` |
| 8 | 2025-11-19 to 2025-12-02 Gmail Batch 008 Filtered Audit.md | `1PCfcJvwA6V1cuC50nxneV1LR_eRo_qas` | `37ae99f1e067dbf154001f33a498764f39aa3051fbe15beff1cc8541a0a0d99e` |
| 9 | 2025-12-02 to 2025-12-16 Gmail Batch 008 Filtered Audit.md | `1fimVKUhqqXDffNNJjajsX7UtBYwsnULC` | `2c56320a7008d0594c62737d6b4b4fe4a80c0aa0ae7d5d69bb64f64be83e73c9` |
| 10 | 2025-12-16 to 2025-12-31 Gmail Batch 008 Filtered Audit.md | `1Qmig1GZCGFMujhISJXth22zMi12ZP8YN` | `7dc300dce156f9cc1ed963eea034b4556b3e080e7d6b444956828743c64359ab` |

Resume checkpoint after this batch: order 11, `2025-12-31 to 2026-01-15 Gmail Batch 008 Filtered Audit.md`, Drive ID `1513x1ukOrUFd8FwVxTJAuUgcY-IoAwB2`.
