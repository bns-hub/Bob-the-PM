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