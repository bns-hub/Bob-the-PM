---
repair_version: 1
repair_id: obsidian-repair-20260921-inbox-kanban-root-sweep
status: awaiting_local_writer
created_at: "2026-09-21T00:18:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
drive_mode: read_only
priority: urgent-user-request
---

# Inbox, Kanban, and root-cleanup sweep — 2026-09-21

## User request

Benson asked for an immediate sweep, a visible project Kanban, cleanup of newly duplicated root folders/files, and correct CRM-backed person classification.

Do not apply this manifest blindly. Re-read the live vault through authenticated Obsidian MCP, obtain the shared processor lock, and verify every target before changing it.

## Current writer state

At cloud inspection time, both registered local writers were stale:
- LAPTOP-96G8839H last_seen: 2026-09-14T10:23:43Z
- PC1 last_seen: 2026-09-14T12:39:30Z

No current authenticated local writer / active-vault validation for exactly `Ben` was established by the cloud coordinator. Therefore this sweep is staged but NOT locally applied.

## Kanban finding

No `Active Projects` or `Live Deals` Base/Kanban file was found in the synchronized vault view. The currently visible `E Efforts/Efforts MOC.md` is a static Markdown MOC, not the requested Kanban.

Create through local Obsidian MCP, without adding another top-level folder:

1. `00 Home/Active Projects.base`
   - first/default view: Work
   - additional views: Personal, All
   - source only canonical project notes
   - do not mix HubSpot deal objects into project objects
   - Work projects live under `E Efforts/Work/`
   - Personal projects live under `E Efforts/Personal/`

2. `00 Home/Live Deals.base`
   - source canonical HubSpot-backed deal notes
   - HubSpot remains source of truth for CRM-owned fields
   - show at least: deal name, account/customer, deal owner, deal type
   - group/order by the exact HubSpot deal-stage sequence already documented in OBSIDIAN-GRAPH-HYGIENE.md
   - provide full-pipeline view plus filtered active view
   - hide/filter terminal stages only at view level; never delete/rewrite data merely to hide a column

3. Patch `00 Home/Home.md` to link clearly to both Base files.

Re-open both Base files in Obsidian and verify that cards resolve to the intended canonical notes.

## Root hygiene finding

The synchronized vault root currently contains the established folders plus a second cluster created in one burst around 2026-09-20T16:04Z (2026-09-21 00:04 SGT).

Established/canonical candidates:
- `00 Home/` — created 2026-09-17
- `01 Inbox/` — created 2026-09-16
- `A Atlas/`
- `C Calendar/`
- `E Efforts/`
- `S Sources/`
- `X Filtered Out/`
- `Z System/`

New duplicate/stray cluster:
- `00. Home/` — created 2026-09-20T16:04Z; contains a newer `One-Click Note Guide.md`
- `01. Inbox/` — created 2026-09-20T16:04Z; currently empty
- `02. Excalidraw/` — created 2026-09-20T16:04Z; currently empty
- `Excalidraw/` — created 2026-09-20T16:04Z; currently empty
- root `Untitled.md` — empty
- root `UsersBensonAppDataLocalTempclaude...scratchpaddeadlinks.json` — scratch-like artifact whose filename points to a Claude temp path
- root `Fold5 to iPhone Migration Plan.md`
- root `GeBIZ Tender Tracker — Pipeline Workflow.md`

Do NOT delete the duplicate cluster solely from this manifest. Through local MCP:
1. compare `00. Home/One-Click Note Guide.md` with `00 Home/One-Click Note Guide.md`;
2. preserve any newer substantive content in the canonical `00 Home` copy;
3. verify `01. Inbox`, `02. Excalidraw`, and `Excalidraw` are still empty before removal;
4. classify/move the root Markdown files to their canonical owners;
5. move scratch/debug artifacts out of the live knowledge graph only after confirming they are not user knowledge;
6. remove empty duplicates only after the above and then run a root-level broken-link check.

The timing strongly indicates a bulk local write/export created the duplicate cluster. The cloud coordinator cannot prove which local process created every item without local logs. The scratch JSON filename explicitly contains a Claude temp path and must be treated as a leaked scratch artifact unless local inspection proves otherwise.

## Canonical Inbox sweep

Canonical live Inbox observed:
- `01 Inbox/Capture Here.md`
- `01 Inbox/Inbox.md`
- `01 Inbox/Fold5 to iPhone CHECKLIST.md`
- `01 Inbox/Fold5 to iPhone Migration Plan.md`
- `01 Inbox/Fold5 to iPhone Runbook.md`

Persistent control files:
- `Capture Here.md` stays as the input buffer.
- `Inbox.md` stays as the read/triage dashboard.
Never delete either file.

### iPhone migration

Treat all three iPhone files as one Personal project:
- canonical project: `iPhone Migration`
- scope: personal
- target: `E Efforts/Personal/iPhone Migration/`
- create/reuse canonical hub: `E Efforts/Personal/iPhone Migration/iPhone Migration.md`
- children: CHECKLIST, Migration Plan, Runbook
- assign one stable `project_id` shared by the hub/child ownership metadata
- link all children back to the canonical hub

There is also a root-level `Fold5 to iPhone Migration Plan.md`. Diff it against the Inbox version and merge unique substantive content before removing any duplicate. Never choose solely by file size or modified time.

### Capture Here entries observed

The live `Capture Here.md` still contains unprocessed material dated 14–18 Sep 2026. Process every entry; preserve captured dates in the daily index.

Verified CRM-backed routing found during this cloud sweep:

- `NEA - AMS3`
  - HubSpot deal ID: 340928313029
  - current CRM stage ID: 3366006495
  - stage label: CAT 5 - Submission in Progress
  - deal type: Open Tender
  - deal owner: Benson Foo (owner ID 86653749)
  - route AMS3 tender/proposal/status captures to the canonical deal/bid owner; do not fabricate a delivery project before the deal/project transition is explicit.

- `DSO - HPMS2`
  - HubSpot deal ID: 320212975348
  - stage: CAT 3 - Completed Presentation / On-going Clarifications / Negotiation
  - deal type: Open Tender
  - owner: Benson Foo
  - route the Cheok Hong follow-up as an action/activity under this canonical opportunity owner unless a separate delivery project already exists and is verified.

- `BreadTalk - AI Initiative`
  - HubSpot deal ID: 347393181393
  - stage: CAT 7 - Oppty Identified
  - deal type: Initial Engagement
  - owner: Benson Foo

- `LTA - Development and Maintenance of LTA.PROMPT 2.0`
  - HubSpot deal ID: 348349823687
  - stage: Tender Published
  - deal type: Open Tender
  - owner: Benson Foo
  - user shorthand alias: `LTA.PROMPT 2.0`

- `SIT - SITAR` should be the user-facing canonical shorthand only after local owner verification.
  - HubSpot deal ID: 348238350062
  - CRM deal name: `Maintenance and Support Service for SITAR App (ITQ-26-0151)`
  - stage: Tender Published
  - deal type: Non-Tender
  - owner: Benson Foo
  - user shorthand alias: `SITAR`
  - verify the canonical organisation is SIT before final renaming to `SIT - SITAR`.

- `Bhutan ACC - ICT Roadmap`
  - HubSpot deal ID: 344950500079
  - stage: CAT 5 - Submission in Progress
  - deal type: Non-Tender
  - owner: Benson Foo
  - Capture Here says submission was completed at 5:42pm; preserve that as dated activity, but do not overwrite HubSpot's CRM stage from Obsidian.

- `Corporate Outreach`
  - no HubSpot DEAL match found by that phrase in this sweep.
  - likely related to existing `E Efforts/Projects/Lead Generation and Outreach.md`, but do not merge by title similarity alone. Verify content/aliases before routing.

For struck-through/completed tasks, preserve the activity/date history and remove them from the active capture buffer only after the destination/daily index is verified.

## Date index

For each processed 14–18 Sep capture, create/update the relevant `C Calendar/Work/YYYY-MM-DD.md` or Personal daily index entry with a concise link to the final owner/child note. Do not duplicate full note content.

## Kok Tiong identity correction

Before any AMS3/person routing involving Kok Tiong Goh:
- HubSpot active owner: 163267515
- HubSpot internal user record resolves to the Ecquaria corporate email
- CRM formal job title: `Technical Consultant`
- Benson-confirmed functional role: `presales`
- canonical organisation relationship: TOPPAN Ecquaria / internal
- functional relationship: presales
- do NOT classify him as NEA/customer simply because he participates in NEA AMS3

If multiple HubSpot CONTACT provider records resolve to the same real person, preserve provider IDs under one canonical person entity rather than creating duplicate people.

## Verification before completion

The local processor must:
1. refresh authoritative Bob instructions;
2. obtain the shared processor lock;
3. authenticate Obsidian MCP and validate active vault exactly `Ben`;
4. re-read every target before modification;
5. make the smallest safe changes;
6. re-read every changed note/Base through MCP;
7. verify stable IDs, aliases, links, and intended content;
8. run broken-link checks for root, Inbox, iPhone project, and affected work owner clusters;
9. update this manifest with exact changed paths and unresolved items;
10. only then mark `status: completed`.

