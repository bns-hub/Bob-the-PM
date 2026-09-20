## 2026-09-21 TenderBoard Work Gmail source added and mailbox evidence reconciled

**One goal:** close the coverage gap between TenderBoard's public Open Deals page and the tenders delivered to the TECQ TenderBoard mailing group, without exposing personal or unrelated work email.

### Account and privacy boundary

- Gmail connection used: `Work`.
- Verified account: `bensonfoo@ecquaria.com`.
- Allowed scope: messages from `noreply@tenderboard.biz` addressed to the `TECQ_tenderboard` distribution group. Known group variants include `TECQ_tenderboard@ecquaria.com` and `TECQ_tenderboard@toppanecquaria.com`.
- Personal Gmail `bnsn4ull@gmail.com` remains forbidden.
- Unrelated work email remains forbidden.
- The mailbox investigation was read-only. No message, label, archive state, draft, or reply was changed.

### Mailbox evidence

- A Drive-wide and tracker investigation had already shown that the public TenderBoard page is incomplete. Work Gmail now provides the missing second TenderBoard source.
- Search scope covered all mail from `noreply@tenderboard.biz`. There were **1,779 messages**.
- **192** were daily digest messages titled `Updated tenders notification for TECQ_tenderboard from TenderBoard`, covering 23 January to 19 September 2026.
- The digests contained **7,989 tender appearances** and **7,431 distinct normalised title-and-buyer pairs**. Every digest's advertised count matched the parsed count, with zero count mismatches.
- As at 20 September 2026, **599** digest tenders were still open: **270 GeBIZ** and **329 Non-GeBIZ**.
- The other **1,587 messages** included **1,510** messages with structured tender fields and **422 amendment messages**, plus clarifications, competitive-data newsletters and one reminder.
- Competitive-data newsletters are not tender rows. Clarifications must update or alert on an existing tender, not create duplicates. Reliable award messages may feed Awarded (Intel) only when the supplier and award fields are clear.

### Evidence for the three tenders raised by Benson

1. `TO2026014`, Singapore Institute of Technology cloud data platform: present in the 14 August daily digest and closes 5 October 2026. This agrees with the first-party source and the repaired tracker row.
2. Singapore University of Technology and Design Generative Artificial Intelligence optical character recognition Software as a Service tender: present in the 15 August daily digest and closed 28 August 2026. This resolves the former Unknown closing date.
3. Singapore University of Technology and Design IT outsource support: present in the 16 September daily digest and closes 2 October 2026. This agrees with the first-party Singapore University of Technology and Design opportunity.

The optical character recognition row was corrected in the canonical tracker. It was removed from EPU/CMP/10 and added to Closed Tenders with source `TenderBoard Email`, closing date 28 August 2026, Move Date 20 September 2026 and its existing `Look at` verdict, reason, review date and fingerprint preserved. The write was re-read and verified. It is no longer incorrectly shown as Live.

### Comparison with the canonical tracker

- Canonical tracker ID remains `1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU`.
- Of the 599 email tenders still open on 20 September, **226** exact-title matched a current or closed tracker row.
- **373** did not exact-title match: **264 Non-GeBIZ** and **109 GeBIZ**.
- A deliberately broad information and communications technology keyword screen identified **136** of those 373 for TECQ relevance review.
- These 136 are a review queue, not 136 confirmed missing tracker rows. Many will be false positives. They must be judged against the actual primary deliverable before insertion.
- The tracker Coverage & Method tab now records the mailbox counts, comparison limits, review queue and optical character recognition correction.

### Scheduled task change

The existing ChatGPT scheduled task `GeBIZ Tender Pipeline — Full Run` was updated in place and re-opened to verify the saved wording. Its schedule remains daily at 11:45 AM Singapore time.

- The old blanket `Never read email` rule was removed.
- Work Gmail is allowed only for the TenderBoard scope stated above.
- The first email-enabled run performs one historical backfill and reconciles against the verified 1,779-message baseline.
- Later runs use a seven-day overlapping search so delayed messages and amendments are caught.
- The task stores the newest processed Gmail message ID and internal message date in Coverage & Method and Run Ledger Notes.
- Messages are deduplicated by Gmail message ID.
- Tenders are deduplicated by normalised reference number, or by normalised title plus buyer or closing date when the reference is absent.
- The latest dated amendment overrides earlier digest or message fields.
- Source is recorded as `TenderBoard Email`.
- Missing reference numbers are allowed. Unknown closing dates never auto-close.
- Unmatched `Not relevant` tenders are recorded as screened out and are not inserted.
- Unmatched `Look at` and `Possible` tenders are inserted into the correct EPU tab with their verdict. The task must never insert every unmatched email tender blindly.
- After deduplication, every distinct currently open deal captured from public TenderBoard, TenderBoard Email or a first-party source receives a full TECQ relevance decision. The task must not stop at keyword screening. Closed historical email deals are not repeatedly re-reviewed unless they are needed for an existing tracker row or reliable award intelligence.
- A Work Gmail failure is non-fatal. It is logged and the other sources continue.
- The final report includes the Gmail checkpoint, message counts, digests, amendments, clarification alerts, parse failures, screened-out rows, inserted relevant rows and urgent deadlines.

### Final source and merge workflow

1. Verify the Work Google Drive identity, canonical tracker ID and archive folder. Abort tracker writes if the read and write file IDs differ.
2. Read the canonical tracker once and cache the task-owned tabs.
3. Collect GeBIZ Rich Site Summary feeds and the richer published GeBIZ crawl.
4. Read the public TenderBoard crawl only when its status is fresh and its exported count matches the advertised public total.
5. Read the first-party institution handoff and prefer its nonblank fields over TenderBoard sources.
6. Read Work Gmail only within the TenderBoard boundary. Perform the one-off history backfill, then a seven-day overlap from the saved checkpoint.
7. Parse daily digests, structured tender notices and amendments. Do not turn clarification or competitive-data messages into duplicate opportunities.
8. Apply source priority to nonblank fields: `MANUAL_TENDERS`, GeBIZ, first-party institution, TenderBoard Email, then public TenderBoard. A later TenderBoard amendment overrides an earlier TenderBoard digest.
9. Deduplicate by normalised reference, or by normalised title plus buyer, agency or closing date as available.
10. Review the actual deliverable for TECQ fit. Screen out irrelevant mail without adding it. Add only relevant unmatched `Look at` or `Possible` rows.
11. Keep an existing open row when a source later omits it. Close only after a confirmed closing time passes or a reliable source confirms closure or cancellation. Never auto-close Unknown dates.
12. Back up when practical, update only the canonical spreadsheet, verify the changed ranges, update Run Ledger and Coverage & Method, and issue the short phone-readable report.

This email source closes the main structural gap. The public page remains useful and independently count-checked, but Work Gmail now catches TenderBoard catalogue items and amendments that the public Open Deals page cannot expose.

## 2026-09-20 Missing TenderBoard tenders: cause confirmed, tracker repaired, and coverage workflow corrected

**One goal:** explain why three user-found TenderBoard tenders were absent, restore them safely, and change the collection workflow so the same gaps are detected or covered.

### Confirmed findings

- All three tenders were absent from the canonical tracker `1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU`. Searches covered EPU/CMP/10, EPU/SER/34, and Closed Tenders.
- They were not filtered out by the tracker. A search of every committed `TenderBoard_Raw_latest.csv` snapshot from 29 August through 20 September found none of the three. The tracker never received them from its TenderBoard handoff.
- The previous public TenderBoard crawler had `maxAgeDays = 14`. It stopped after encountering older publication dates. On 20 September the public Open Deals page advertised 187 live tenders, but the crawler exported only 160. It therefore discarded 27 older-published tenders even though they were still open.
- Removing the publication-age rule and walking every public page produced 187 of 187 advertised live tenders across four pages. This recovered the Singapore Institute of Technology cloud data platform tender.
- The public TenderBoard Open Deals page is not the full TenderBoard catalogue. The page describes public, TenderBoard-exclusive open deals, while TenderBoard's paid service offers browsing of all currently live deals. An unauthenticated public crawler therefore cannot reproduce everything a signed-in user can find.
- This second limitation explains why the two Singapore University of Technology and Design items were not on the public TenderBoard page, even after all 187 public rows were scanned.
- The official Singapore University of Technology and Design procurement page currently lists the IT outsource support tender. It does not currently list the Generative Artificial Intelligence optical character recognition tender. Exact searches also found no reliable public source for that second item, so its reference, closing date, and public link remain unconfirmed.

### The three tenders and what was done

1. **TO2026014, cloud data platform for Singapore Institute of Technology**
   - First-party source: https://www.singaporetech.edu.sg/procurement-opportunities/to2026014
   - Published 14 August 2026.
   - Closes 5 October 2026 at 4 PM.
   - Added to EPU/CMP/10 with `Look at`.
   - Reason: direct cloud data platform design, development, implementation, and maintenance is a strong match for TECQ's application, integration, cloud, and data capabilities.

2. **Provision of SaaS solution for a Generative Artificial Intelligence optical character recognition system for Singapore University of Technology and Design**
   - The user confirmed this was found in TenderBoard.
   - No reference number, closing date, or public link could be independently verified.
   - Added to EPU/CMP/10 with source `TenderBoard (user confirmed)`, status `Live`, closing date `Unknown`, and verdict `Look at`.
   - The Unknown closing date is deliberate. The daily workflow must never auto-close it.
   - Reason: SaaS, Generative Artificial Intelligence, optical character recognition, design, implementation, and support are a strong capability match.

3. **Doc3312966712, IT outsource support services for Singapore University of Technology and Design**
   - First-party source: https://www.sutd.edu.sg/about/partnering-with-sutd/suppliers/opportunities/
   - Opens 15 September 2026 and closes 2 October 2026.
   - Mandatory Zoom briefing is 25 September at 10 AM.
   - Registration with the signed non-disclosure agreement is due 22 September before 5 PM.
   - Added to EPU/CMP/10 with `Possible`.
   - Reason: IT support is relevant, but the public notice does not expose enough scope detail to confirm a strong fit.

The live tracker write was re-read after saving. All three rows, verdicts, dates, reasons, and verified links persisted. The canonical EPU/CMP/10 tab now has 618 data rows. The repair and diagnosis are also recorded in Run Ledger row 49 and Coverage & Method rows 43 to 52.

### Code and handoff changes

- Pull request 12: https://github.com/bns-hub/Projects/pull/12
- Branch: `fix/tender-source-coverage`
- Commit: `650a18b2093fd4fca51f6a4a2a9b5b01cbe10f96`
- Merged to Projects main as `be7e7a751298ae8276dbec8e16530840661889b4` after all checks completed.
- Pull request checks completed cleanly. The unrelated Netlify jobs skipped or passed as expected.
- The TenderBoard crawler no longer applies a publication-age cut-off. It scans all public live pages.
- It now reads TenderBoard's advertised public total and fails instead of publishing if the exported unique-row count differs.
- A new first-party institution crawler reads current Singapore University of Technology and Design opportunities and publishes `Institution_Raw_latest.csv` plus `Institution_Raw_status.json`.
- The known IT outsource support tender is a required regression check through its 2 October closing date. The check expires after the tender closes so it will not break future runs permanently.
- The GitHub workflow publishes both the TenderBoard and institution handoffs on the `tenderboard-data` branch.
- Local tests passed with 187 of 187 TenderBoard public rows across four pages and six current Singapore University of Technology and Design rows.
- GitHub Actions run 35484778586 also passed and published the corrected handoffs at 10:45 SGT. The live status files report 187 TenderBoard rows with coverage passed and six institution rows with the known tender present.
- Pull request 12 is merged. The corrected data is live from the successful manual workflow run, and the next normal scheduled crawl will use the same corrected code from Projects main.

### Live scheduled task changes

The active daily task `GeBIZ Tender Pipeline — Full Run`, scheduled for 11:45 AM Singapore time, was updated in place.

- It still reads and writes only the canonical spreadsheet ID.
- It now reads the institution status and CSV handoffs in addition to GeBIZ and TenderBoard.
- First-party institution fields take priority over TenderBoard fields when both describe the same tender.
- GeBIZ remains the highest-priority source when it has an official matching record.
- The final report now includes the institutional-source count and failures.
- Run Ledger Notes and Coverage & Method must record institutional totals and new rows.
- An existing open tracker row must not be deleted merely because it is absent from a later daily handoff. It remains until a confirmed closing date passes, or a reliable source explicitly says it is closed or cancelled.
- Unknown closing dates must never be auto-closed.
- Source failures remain non-fatal and must be reported plainly.

### Final daily workflow for future runs

1. Verify the Work Google Drive account, the canonical tracker ID, and the fixed archive folder ID.
2. Read the canonical tracker once and cache the active state.
3. Collect the official GeBIZ sources and the richer GitHub GeBIZ handoff.
4. Read the TenderBoard status first. Accept the TenderBoard CSV only when it is current and the status says the exported count matched TenderBoard's advertised public count.
5. Read the institution status first, then the institution CSV. This currently covers the official Singapore University of Technology and Design opportunity page.
6. Apply optional `MANUAL_TENDERS` corrections last. This remains the safe route for tenders visible only inside signed-in TenderBoard, including items with an Unknown closing date.
7. Deduplicate by normalised reference, or by normalised title plus agency or closing date. Source priority is GeBIZ, then first-party institution, then TenderBoard. Nonblank manual corrections remain authoritative.
8. Keep existing open rows even when a current source omits them. Close only after a known closing time passes or a reliable source confirms closure or cancellation.
9. Review every new retained row. Keep uncertain but plausible work as `Possible` instead of excluding it.
10. Back up before writing, write only to the canonical file, then re-read the changed ranges to confirm persistence.
11. Record source counts, coverage checks, new rows, failures, manual corrections, and direct post-write counts in Run Ledger and Coverage & Method.
12. Report new `Look at` and `Possible` tenders, urgent deadlines, source gaps, backup result, and the canonical tracker link.

### Remaining limitation and next extension

- The public TenderBoard crawler can prove completeness only for TenderBoard's public Open Deals page. It cannot prove completeness against TenderBoard's full signed-in catalogue.
- The Generative Artificial Intelligence optical character recognition tender demonstrates that gap. Its tracker row is safe, but its missing reference and deadline still need a signed-in TenderBoard detail or a first-party source.
- The durable coverage approach is to add first-party institution feeds for important repeat buyers and keep `MANUAL_TENDERS` for signed-in-only discoveries. Do not describe the public TenderBoard crawl as full TenderBoard coverage.

---

## 2026-09-20 Recovery and automation repair completed

**Outcome confirmed from the live canonical Google Sheet after the run:**
- Canonical tracker ID remains `1UpmDSHvuZ8IZ9VOMCOvIAizdzv6T9fDgPF9tippwZhU`. Its current title is `GeBIZ Tender Tracker — Current (20/09/26, 6:28 AM)`.
- EPU/CMP/10 has 615 data rows and all 615 have a TECQ Review.
- EPU/SER/34 has 81 data rows and all 81 have a TECQ Review.
- The two open tabs therefore contain 696 reviewed rows and zero blank review decisions.
- Closed Tenders has 413 data rows, confirming that two expired rows moved there during this run.
- All 368 saved decisions were rechecked against the source CSV files and the live tracker. The result is 368 matched, zero missing, zero ambiguous, and 368 exact matches across TECQ Review, Why, and Reviewed On.
- Of the 368 restored decisions, 367 remain on the open tabs and one is now in Closed Tenders. Their fingerprints are 367 from `TECQ_REVIEWS_2026-09-18.csv` and one from its supplement.
- The fresh second-phase review added 95 decisions, comprising 94 in EPU/CMP/10 and one in EPU/SER/34.
- Tender HDB000ETT26000166 is present in EPU/CMP/10 with the exact title `MAINTENANCE OF THE ESTATE MANAGEMENT SYSTEM` and verdict `Look at`.
- The canonical file was updated in place. Forecasted Tenders was left untouched.

**Live scheduled task changes completed:**
- The active `GeBIZ Tender Pipeline — Full Run` task remains scheduled daily at 11:45 AM.
- It now uses the Work Google Drive connection only and locks every read and write to the canonical tracker ID.
- It aborts if its read and write spreadsheet IDs differ.
- It must not create or rotate another Current spreadsheet.
- It now requires RFC 4180-compatible CSV parsing, row-by-row validation, reporting of malformed rows without discarding the valid batch, and a pre-write match/conflict report.
- The recovery spreadsheet `1b8PEr3Rq1jqgbMQS65Una0Jjht5pZemfY4UeBp4nbMU` remains a read-only recovery source.
- A manual Run now completed successfully.

**Other checks and record correction:**
- Apps Script currently has one trigger only: `runWeeklyForecast`. There is no `runDailyTracker` trigger to delete, so nothing was removed.
- Pull request 11 received a correction comment explaining that its old root-cause account was false and that the actual fault was the failed CSV handoff. Comment: https://github.com/bns-hub/Projects/pull/11#issuecomment-5745774329
- No Projects repository code change was needed because the live generator and merge instructions are held in the scheduled task, not in the dormant old Apps Script copy.

**Remaining non-blocking housekeeping:**
- GeBIZ RSS was unavailable in this runner, but the richer 320-row GeBIZ crawl and the 160-row TenderBoard handoff completed.
- Direct folder inspection confirmed that `Archived`, folder ID `1TPg44swiYi14FD3rciZx-WNCsFE8Qyve`, is the established archive. It already contains the dated TenderBoard raw CSV files and historical tracker copies.
- A post-recovery backup was created there as `GeBIZ Tender Tracker — Archived Backup (20/09/26, 10:10 AM)`, file ID `1MMxykVBARK0BEW4O8lI_bi-pekphA-eba_hpLxqnmPU`.
- The scheduled task now uses that exact Archived folder ID for future backups, raw TenderBoard CSV files, and manual tender lookup. It must not create another Archive or Archived folder.
- Coverage & Method has the correct new 20 September run block, with older 19 September status lines retained above it as history.
- The missing 20 September Run Ledger row was added manually and re-read successfully. It records the 320-row GeBIZ crawl, the 160-row TenderBoard handoff, zero new retained tenders, 368 restored reviews, 95 fresh reviews, and two rows moved to Closed Tenders.
- The scheduled task wording was repaired after verification. It now requires a clearly dated Coverage & Method block with final direct-read counts, and exactly one Run Ledger row for the current day before the final report.
- The duplicated `runWeeklyForecast` sentence was removed.

---

## 2026-09-20 Retraction of the truncated-export match correction

**This entry supersedes both match-count entries immediately below.**

**Confirmed from direct live Google Sheets range reads:**
- EPU/CMP/10 currently contains 617 data rows, not 164.
- EPU/SER/34 currently contains 81 data rows.
- Closed Tenders currently contains 411 data rows.
- The earlier 108 matched and 259 unmatched result came from treating a truncated export ending near row 165 as the whole tracker.
- Rows below that point are part of the live populated sheet. They were not proven to be stale or outside an active table.
- Matching TECQ_REVIEWS_2026-09-18.csv plus its supplement against the complete live ranges gives 368 matches, zero missing rows, zero ambiguous matches, and zero conflicting verdicts.
- The 368 matches comprise 328 EPU/CMP/10 rows and 40 EPU/SER/34 rows.
- Matching uses reference number for 274 rows and normalised title for 94 rows.
- All 368 target TECQ Review cells are currently blank, so applying the batch is additive and overwrites no verdict.
- Current non-blank TECQ Review counts are 194 in EPU/CMP/10 and 40 in EPU/SER/34, totalling 234 reviewed open rows.
- Therefore the claim that 17 relevant tenders are missing from the tracker is withdrawn. Those tenders are present in the complete live range.
- The corrected CSV file remains 367 rows, with the one-row supplement producing 368 decisions in total.

**Expected post-merge arithmetic:**
- 234 existing reviewed open rows plus 368 new verdicts equals 602 reviewed rows.
- With 698 open rows, that leaves 96 awaiting review.
- The current Coverage & Method text says 234 of 698 reviewed but 530 awaiting review. That displayed awaiting count is arithmetically inconsistent and should not be used as the verification source. Recount the cells directly after the run.

**Remaining engineering work:**
- Fix the decision-file generator to use a proper CSV writer so quotation marks cannot break a future file.
- Make the merge reject or report an individual malformed row instead of discarding the entire batch.
- The merged pull request 11 in bns-hub/Projects contains a disproved explanation about the old Apps Script. Its four-line header change is inert, but the permanent pull request record should receive a correction comment if the user authorises it.
- The dead Apps Script trigger may be removed separately. It is unrelated to applying the 368 decisions.

**No production changes:**
- No Google Drive file, spreadsheet, Apps Script, trigger, or scheduled task was changed during this verification.

---

## 2026-09-20 Correction to active-table match audit

**This entry supersedes the 367-decision match counts in the entry immediately below.**

**Confirmed correction:**
- The earlier claim that all 367 decisions matched the active canonical tracker was wrong.
- The cause was an over-broad read of EPU/CMP/10 through row 1000. That included old populated cells below the active table and incorrectly treated them as current tracker rows.
- Repeating the match against the current active bounds gives exactly 108 matched and 259 unmatched: 69 matches in EPU/CMP/10, 39 in EPU/SER/34, and none in Closed Tenders.
- The reproduced active bounds are 164 EPU/CMP/10 data rows, spreadsheet rows 2 to 165, 80 EPU/SER/34 data rows, spreadsheet rows 2 to 81, and 205 Closed Tenders data rows, spreadsheet rows 2 to 206.
- Of the 259 unmatched decisions, 243 are Not relevant, 12 are Possible, and 5 are Look at.
- Therefore 17 relevant tenders are absent from the active tracker.
- The 273 and 94 figures previously reported were the decision file's own split between rows with and without a reference number. They were not verified match counts.

**Important additional finding:**
- All 17 relevant missing tenders still survive as old cells below the active EPU/CMP/10 table, at spreadsheet rows 170 to 498. Their review fields are blank.
- They are therefore absent operationally from the active tracker, but their source row data has not vanished from the raw sheet grid.
- This points to an active-table rebuild or retention failure. The refresh retained only the top current block and excluded still-open older tenders while leaving the old cells below the active range.
- The exact writer logic still needs inspection before stating the software cause as settled.

**Time-sensitive items:**
- Three Possible tenders close on 21 September 2026: Adobe Acrobat Standard subscription, Delinea Privileged Access Management renewal, and MPA fuel-data collection licence.
- Four Possible tenders close on 23 September 2026: IT outsource support, Veeam renewal, Microsoft Office LTSC licences, and NYP Deep Freeze Cloud renewal.
- This corrects the supplied summary, which mentioned three on 23 September.

**Revised recovery design, not yet authorised for writing:**
1. Matched active rows: fill only the review-owned fields.
2. Unmatched Not relevant rows: keep a screening record and do not add them to the active tracker.
3. Unmatched relevant rows: restore or recreate an active tracker row from the surviving source data, then apply the saved verdict.
4. Before writing, report exact counts, conflicts, deadlines, and the handling of the stale lower grid.
5. Do not treat the three recovery-copy verdicts as separate inputs because they are already part of the 367-decision file.

**No production changes:**
- No Google Drive file, spreadsheet, Apps Script, trigger, or scheduled task was changed during this correction.

---

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
