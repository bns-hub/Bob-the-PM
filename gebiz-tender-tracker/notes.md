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