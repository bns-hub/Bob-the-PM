---
repair_version: 1
repair_id: obsidian-repair-20260914-wrms-graph-normalization
status: pending
created_at: "2026-09-14T16:40:00+08:00"
created_by: chatgpt-cloud
scope: wrms-project-cluster
max_target_notes: 50
source_of_truth: OBSIDIAN-GRAPH-HYGIENE.md
---

# WRMS graph normalization

## Why this repair exists

The synced `Ben` vault currently contains a clear WRMS cluster that is only partially connected. The graph therefore shows many WRMS notes as individual activity nodes around broad `NEA Engagement` or source/audit hubs instead of around a specific WRMS project owner.

Read `OBSIDIAN-GRAPH-HYGIENE.md` before processing this job. Do not apply this document as a blind patch. Re-read the live notes through Obsidian MCP first.

## Verified observations from the synced Drive view

1. `WRMS Personnel Update.md` explicitly says `Project: WRMS` and records Roy Li's departure, but it contains no WRMS wikilink or canonical project-owner field. Its stable capture metadata must be preserved.
2. Several explicit `NEA WRMS - Tender` / `WRMS Tender Presentation` calendar-email notes link to the broad `[[E Efforts/Projects/NEA Engagement|NEA Engagement]]` owner but do not link to a more specific WRMS owner.
3. `NEA Engagement.md` itself contains a Batch 007 evidence list with many WRMS-specific notes, confirming the relationship but leaving them grouped only at the broad NEA-engagement level.
4. A specific WRMS HubSpot/deal note already exists with stable node ID `hubspot:deal:318728919771` and title `NEA - WRMS App Maintenance & Support Renewal`. It contains substantial WRMS maintenance, award, CR, kickoff, handover, and project context. Treat it as a verified WRMS sub-owner, not automatically as the umbrella for every historical WRMS tender item.
5. The Bob-the-PM repository has an explicit `WRMS/notes.md` project folder, confirming `WRMS` is an intentional project identity rather than an incidental keyword.

## Required outcome

### 1. Resolve or create the canonical WRMS umbrella

Search the live `Ben` vault for an existing canonical project/MOC representing the WRMS programme/project.

- If a suitable verified hub already exists, reuse it.
- If none exists, create `E Efforts/Projects/NEA WRMS.md` as the canonical umbrella project hub, using the vault's established project-note structure and controlled taxonomy.
- Link the umbrella to the verified NEA organisation/account and the broader `NEA Engagement` parent where useful.
- Link the existing HubSpot/deal note `NEA - WRMS App Maintenance & Support Renewal` as a more specific WRMS owner/sub-record.
- Do not merge unrelated NEA work into WRMS.

### 2. Repair the WRMS Personnel Update

Target title: `WRMS Personnel Update.md`.

- Preserve `capture_id`, `content_hash`, source repository/path/blob metadata, exact original capture, and user-authored content.
- Replace the plain-text-only project relationship with a verified canonical WRMS owner link in the note's established metadata style and human-readable body.
- If the cleaner representation is to append this personnel fact into the canonical WRMS project hub, do so minimally while preserving the source note/evidence identity required by the ingestion contract; do not silently discard the original capture.

### 3. Repair the explicit WRMS calendar/email cluster

Re-read and patch only notes whose content explicitly concerns WRMS. Initial candidates include:

- `C Calendar/Email/2025-09-19 NEA WRMS - Tender`
- `C Calendar/Email/2025-09-19 NEA WRMS - Tender b68a6d`
- `C Calendar/Email/2025-09-19 NEA WRMS - Tender 89abc2`
- `C Calendar/Email/2025-09-24 WRMS Tender Presentation Slides`
- `C Calendar/Email/2025-09-24 NEA WRMS - Tender`
- `C Calendar/Email/2025-09-24 Invitation Tender Presentation WRMS`
- `C Calendar/Email/2025-09-24 Tender Presentation - WRMS`
- `C Calendar/Email/2025-09-24 NEA WRMS - Tender cd0fda`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation 46b115`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation Declined`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation 75a420`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation - 9ec261`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation - a4f76a`
- `C Calendar/Email/2025-09-25 WRMS Tender Presentation - 46c1bc`
- later calendar items whose titles explicitly contain WRMS, including dry runs, slides, internal syncs, rehearsals, and presentation-training notes.

For each verified WRMS note:

- preserve source IDs and existing valid links;
- add the canonical WRMS owner link using the vault's established metadata/body-link style;
- keep `NEA Engagement` when it remains valid as a broader parent;
- do not relabel unrelated `Provision Application...`, `Next Generation Integrated Field...`, or other NEA notes as WRMS merely because they appear in the same batch.

### 4. Keep source/audit evidence subordinate

Do not delete Gmail batch files, HubSpot range files, filtered-audit notes, MOCs, or checkpoints. They remain evidence/navigation nodes. Do not add extra direct links from every source batch into WRMS unless required for traceability.

## Verification

After processing:

- re-read every changed note through Obsidian MCP;
- verify the canonical WRMS link is present and stable source/capture IDs are unchanged;
- verify the WRMS umbrella links to the existing WRMS deal/sub-owner and broader NEA context without absorbing unrelated NEA work;
- update this file to `status: completed` with `completed_at`, processor device, canonical WRMS destination path, list/count of changed notes, unresolved candidates, and verification result.

If the canonical WRMS owner cannot be resolved safely, set `status: unresolved_routing`, record the ambiguity, and make no speculative mass changes.