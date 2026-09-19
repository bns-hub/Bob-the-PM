## 2026-09-20 Completed 367-decision lineage audit

**What we now know, confirmed from the files and revision history:**
- TECQ_REVIEWS_2026-09-18.csv contains exactly 367 completed decisions, all stamped 2026-09-18 17:00 Singapore time: 343 Not relevant, 18 Possible, and 6 Look at.
- All 367 match rows in the canonical tracker 1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU. Matching used Tender/Ref No. for 273 rows and normalised Title plus Agency for the 94 TenderBoard rows with no reference number.
- The 367 split across 328 EPU/CMP/10 rows and 39 EPU/SER/34 rows.
- Canonical revision 1494 at 2026-09-18 16:34 Singapore time, before the decision file was created, had all 367 matching tracker rows with blank review fields.
- Canonical revision 1546 at 2026-09-18 17:12 Singapore time, immediately after the decision file was created, still had all 367 review fields blank. Revisions 1598, 1601, 1627, 1653, and the current state also have all 367 blank.
- Therefore the 367 were created in the CSV handoff file and never merged into the canonical tracker. None were later erased from that tracker.
- The recovery spreadsheet 1b8PEr3Rq1jqgbMQS65Una0Jjht5pZemfY4UeBp4nbMU contains only three of the 367 decisions. The other 364 remain only in the CSV handoff file.
- TECQ_REVIEWS_2026-09-18_supplement.csv adds one further completed decision for ACR000ETQ26000005 at 17:35. It also remains unmerged. The correct combined totals are 368 decisions: 344 Not relevant, 18 Possible, and 6 Look at.
- The run note's statement that the combined total was 343 Not relevant, 18 Possible, and 7 Look at is an arithmetic and category error. The supplement row itself says Not relevant.
- The run note explicitly says the live tracker was not updated because the intended full workbook upload exceeded the available tool size limit. This agrees with the revision evidence.
- The approximately 97 newer blanks are a separate set and were not examined or changed.
- No spreadsheet, Apps Script, trigger, scheduled task, or Drive file was changed during this audit.

**Correct classification of the original 367:**
- Previously present in the canonical tracker and later lost: 0.
- Completed elsewhere but never merged into the canonical tracker: 367.
- Genuinely never reviewed: 0.

**Open point:**
- The Apps Script attribution for the 19 September 23:01 event remains unresolved. The source visible now was saved later and cannot prove which code ran at that earlier time.

**Next safe step, not yet authorised:**
- Merge the 367 original decisions and the one supplement decision into the canonical tracker by stable row key, after taking a backup and running a dry-run that confirms 368 matches, zero missing rows, zero ambiguous matches, zero conflicts, and no changes to source-owned fields.
- Separately merge the three 19 September decisions from the recovery copy only if the merge input does not already include them. They are already among the 367, so a correct deduplicating merge must not apply them twice.
- Keep 1b8PEr3 as a recovery source and do not delete it.
- Add a hard read-ID versus write-ID guard before any future automation writes.

---

## 2026-09-20 Corrected root cause from revision evidence

**What we already know, confirmed, not guessed:**
- The earlier loss premise was incorrect because it compared two different spreadsheet file IDs after the workflow forked.
- Long-lived file 1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU revision 1627, modified 2026-09-19 03:04:08 UTC, already had blank TECQ Review, Why, Reviewed On, and Review Fingerprint cells for MOESCHETQ26003993, MOESCHETQ26003900, and WSG000ETT26000002.
- Separate file 1b8PEr3Rq1jqgbMQS65Una0Jjht5pZemfY4UeBp4nbMU was created about 40 minutes later and contains the three reviews.
- The three reviews were therefore written only to the new file. They were not later cleared from the long-lived file.
- Comparing the two current file states shows all 163 CMP/10 row keys and every source-owned value are identical. Only the four review-owned fields on those three rows differ.
- The live spreadsheet uses the same 15-column header for EPU/CMP/10 and EPU/SER/34.
- EPU/SER/34 preserved 40 reviews because those reviews pre-dated the file fork and were already present on both branches. There is no evidenced CMP/10-specific writer difference.
- The scheduled-task specification caused the fork at Phase 1 Step 12 by creating a new Current spreadsheet, then Phase 2 and Phase 3 wrote reviews into that newly created file. The older long-lived file continued to be updated separately and later appeared as Current again.
- Apps Script execution history confirms runDailyTracker completed at 2026-09-19 23:01 Singapore time. No Apps Script code or trigger was changed during this investigation.

**What we are choosing to leave open, or unsure of, for now:**
- Which exact rename or title-selection event made the long-lived file appear as the authoritative Current after the fork. This does not affect the proven fact that the three reviews never existed in that file.
- Production changes remain out of scope until the user approves implementation.

**The one goal for the next phase:**
- Prevent split-brain tracker files by locking every collection, review, and refresh step to one verified spreadsheet ID, preserving review-owned fields by normalised Tender/Ref No., and rejecting any write that would reduce review counts without an explicit authorised reason.

**Anything the next session should NOT re-ask, because it is already settled:**
- Do not describe the incident as CMP/10 clearing three reviews. The correct description is that the three new reviews were written to a separate forked spreadsheet and never reached the long-lived file.
- Do not restore historical verdicts or perform new reviews until the single-file fix and regression checks are verified.

---
# GeBIZ Tender Tracker

## 2026-09-20 Narrowed forensic investigation

**What we already know, confirmed, not guessed:**
- There are exactly two relevant Google Sheets files named GeBIZ Tender Tracker - Current.
- The long-lived live tracker has file ID 1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU.
- The frozen pre-loss snapshot has file ID 1b8PEr3Rq1jqgbMQS65Una0Jjht5pZemfY4UeBp4nbMU and must not be changed.
- EPU/CMP/10 retained all 163 business rows but lost TECQ Review, Why, Reviewed On, and Review Fingerprint.
- EPU/SER/34 retained its 40 reviews and is the reference implementation.
- The user confirmed this phase on 2026-09-20.

**What we are choosing to leave open, or unsure of, for now:**
- Where the EPU/CMP/10 and EPU/SER/34 refresh implementations are stored or run.
- The precise data structures, stable key, schemas, merge operations, and write operations used by each refresh path.
- The exact operation at which the four EPU/CMP/10 review fields cease to exist.
- The safest non-production test location for the corrected refresh logic.

**The one goal for this phase:**
- Identify the exact EPU/CMP/10 operation that drops the four review fields, then produce the smallest safe fix and a regression test using the frozen snapshot, without making any production changes or recovery writes.

**Anything the next session should NOT re-ask, because it is already settled:**
- This phase is read-only forensic investigation and fix design.
- Do not restore the 367 historical verdicts, perform the approximately 97 new judgements, modify the frozen snapshot, create another tracker, rename or archive tracker files, change Apps Script, revive runDailyTracker, or touch Forecasted Tenders.
- Preserve review-owned fields by Tender/Ref No., never by row number or sort position.