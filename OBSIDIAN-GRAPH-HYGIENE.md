# Obsidian graph hygiene and consolidation contract

This file defines how the Bob / Codex pipeline keeps the `Ben` vault useful as a knowledge graph instead of merely accumulating source records. It supplements `OBSIDIAN-DELIVERY-ARCHITECTURE.md`, `OBSIDIAN-INGESTION-CONTRACT.md`, and `CODEX-ACTIVITY-AUDIT-RULES.md`. The stricter safety rule wins if wording conflicts.

## Goal

Every substantive imported item should become useful context under a verified canonical owner. The vault must not treat every email, RSVP, HubSpot batch, or capture as an equally important standalone knowledge node.

The desired pattern is:

`raw/audit evidence -> one canonical activity/event when needed -> one canonical project/tender/account/deal owner -> broader account/entity/MOC`

The pipeline must preserve evidence without turning evidence containers into the main graph structure.

## Canonical-owner rule

For each new or changed substantive note, resolve exactly one primary owner when the evidence supports it:

- Tender
- Service Request
- Change Request
- Project Activity
- Campaign / Account / Sales
- Unresolved Routing

When a specific project, tender, service request, change request, deal, or account owner already exists, prefer that owner over a broad umbrella such as `NEA Engagement`, `TOPPAN Ecquaria`, `Benson Foo`, a source MOC, or an audit log.

A broad account or engagement note remains useful as a parent, but it must not be the only project link when a more specific verified owner is known.

## Explicit project captures

A capture with `project: [<name>]` is not allowed to remain only as plain text `Project: <name>` in the destination note.

Before creating a standalone note, local Codex must search for:

1. an existing canonical project note or MOC with the same verified project identity;
2. an existing HubSpot deal/project owner that clearly represents that project;
3. an existing tender/SR/CR owner when the capture is actually about one of those;
4. the matching Bob-the-PM project folder and its known aliases.

If a verified owner exists, patch that owner or create a small linked child note only when the content genuinely deserves its own lifecycle. If no canonical project hub exists but the project identity is explicit and supported, create one project hub in the established `E Efforts` structure rather than leaving a freestanding unlinked capture.

## Required project linkage

A project-owned note should carry a stable canonical owner reference in frontmatter when the vault's existing conventions allow it, for example `project`, `owner`, `project_link`, or an equivalent established field. The human-readable body must also contain a verified wikilink to the owner.

Do not invent near-duplicate tags. Reuse the vault's controlled taxonomy. Tags support retrieval; wikilinks define graph relationships.

## Consolidation rule

Consolidation means reducing conceptual duplication, not deleting evidence.

Prefer to append or minimally patch an existing owning note when a new source item only adds:

- a status update;
- a personnel update;
- a milestone;
- a decision;
- an action or outcome;
- a billing/acceptance/handover fact;
- a tender clarification or award milestone;
- a service/change request update.

Keep raw/source records only as long as needed for auditability and recovery. Raw evidence does not have to remain inside the live Obsidian vault forever when a verified archive copy exists outside the vault and the canonical knowledge has already been promoted.

Repeated calendar acceptance/decline messages for one event should point to one canonical event/project owner. The source messages may remain as evidence during processing, but they should not each become the only visible representation of the event.

## Audit-source graph rule

Batch manifests, filtered-audit notes, HubSpot record-range files, Gmail evidence batches, checkpoints, and source MOCs are evidence/navigation nodes. They are not business-project owners.

They should link primarily within the source/audit layer and to canonical owners only where needed for traceability. Do not fan every batch note directly into Benson Foo, TOPPAN Ecquaria, and multiple broad MOCs when the same relationship is already represented through the canonical owner.

Historical source batches must never be destroyed merely to make the graph prettier. However, the user has explicitly requested that pure Gmail/HubSpot batch containers stop cluttering the live `Ben` graph. After the source-batch compaction checks below pass, a pure raw batch may be archived outside the live vault and removed from the live vault. This is an evidence-preserving migration, not cosmetic deletion.

## Source-batch compaction and live-graph retention

The live vault should contain business knowledge plus compact source navigation, not thousands of raw range/batch containers.

Examples of pure batch containers include:

- `* Gmail Batch * Filtered Audit.md` and equivalent Gmail batch/progress/evidence range files;
- `HubSpot Emails - ... Records ...md`;
- `HubSpot Notes - ... Records ...md`;
- `HubSpot Contacts - ... Records ...md`;
- `HubSpot Companies - ... Records ...md`;
- other HubSpot object-range files whose primary purpose is raw historical audit storage rather than an independent business record.

Do not archive canonical business owners, project/tender/SR/CR/deal notes, useful MOCs, or human-authored notes simply because they originated from Gmail or HubSpot.

For each pure batch container, use this sequence:

1. Re-read it and identify all substantive records or facts it contains.
2. Verify that substantive business knowledge has already been promoted or linked to the correct canonical project/tender/SR/CR/deal/account owner. If not, promote/repair that knowledge first.
3. Verify stable provider IDs, record counts, source range, content hash, and current vault path.
4. Create a durable archive copy outside the live `Ben` vault in the authorized audit/checkpoint storage, preserving the full raw/readable content plus original path, stable IDs, before-hash, archive timestamp, and archive file ID/path.
5. Re-read the archive copy and verify its hash/content before changing the live vault.
6. Replace direct business-note links to the raw batch with the appropriate compact source MOC or canonical owner where needed so that removing the raw batch will not create broken knowledge navigation.
7. Through the approved local Codex writer and Obsidian MCP, remove the verified pure batch file from the live `Ben` vault. Do not use the Google Drive connector to delete the live-vault file.
8. Update the compact source MOC/checkpoint with the archived batch's source range, provider IDs, original vault path, archive location, and hash so audit retrieval remains possible.
9. Re-scan the affected vault area for broken wikilinks and verify the canonical owner chain remains intact.

If the archive copy cannot be created or verified, the raw batch stays in the live vault and the repair remains pending. No archive failure is permission to delete evidence.

This compaction is bounded and resumable. Process at most one source family (for example Gmail batches, HubSpot Emails, HubSpot Notes, HubSpot Contacts, or HubSpot Companies) or 50 batch files per maintenance run, whichever is smaller. Persist the next resume position.

## Daily graph-hygiene pass

`Obsidian Export` performs a read-only graph-hygiene audit on the synced Drive view every daily run. It must inspect newly created or materially changed notes since the previous successful checkpoint and look for:

- explicit project names present only as plain text with no canonical link;
- substantive activity linked only to a broad account/engagement MOC;
- duplicate standalone notes that should update an existing owner;
- repeated event/RSVP notes lacking a canonical event/project link;
- project/tender/SR/CR notes missing controlled category/owner metadata;
- source/audit records incorrectly promoted as knowledge owners;
- newly created canonical owners that older related notes should now link to;
- pure Gmail/HubSpot batch containers that are eligible for evidence-preserving source-batch compaction.

The cloud task does not edit the live vault. It creates deterministic repair manifests under `obsidian-local-writers/repairs/` for an approved local Codex processor.

Each daily pass is bounded: repair one coherent project/account cluster or one source-batch family, with at most 50 target notes/files, whichever is smaller. Persist the next resume position. This prevents mass rewriting and keeps repairs reviewable.

## Weekly ownership and link pass

Once per seven-day cycle, inspect every note created or materially changed since the previous successful weekly checkpoint. Include ordinary Markdown notes, Excalidraw drawings, Canvas files, and attachment companion notes. Do not exclude a note merely because its file type or folder is non-standard.

For each substantive item:

1. resolve the verified canonical project, tender, service request, change request, deal, account, person, or subject owner;
2. prefer the most specific verified owner over a broad MOC or account umbrella;
3. add only the smallest useful controlled property, tag, or wikilink required to make the relationship visible;
4. preserve all user-authored content, drawing data, source IDs, capture IDs, and hashes;
5. confirm the new link resolves and check the affected owner cluster for broken links;
6. use `unresolved_routing` and create a bounded repair entry when the owner cannot be proved.

The 2026-09-14 AMS3 Excalidraw repair is the reference pattern: the drawing was left intact, given one `project` wikilink to the verified AMS3 HubSpot deal owner, and then checked for link resolution and backlinks.

## WRMS canonical grouping

WRMS is a concrete example of the required behavior.

Current evidence shows many notes titled or describing `WRMS`, `NEA WRMS`, `WRMS Tender Presentation`, WRMS maintenance, and WRMS project activity. These should not remain grouped only under the broad `NEA Engagement` note.

The local writer must first search the live `Ben` vault for an existing canonical WRMS project hub. If a suitable verified hub already exists, reuse it. If none exists, create a canonical umbrella project hub named `NEA WRMS` in the established `E Efforts/Projects` structure.

The umbrella hub should represent the WRMS programme/project identity and may link to more specific owners such as:

- the existing `NEA - WRMS App Maintenance & Support Renewal` HubSpot/deal note;
- WRMS tender/re-procurement activity;
- WRMS change requests;
- WRMS maintenance/kickoff/handover activity;
- WRMS personnel/project updates.

Do not merge unrelated NEA opportunities into WRMS merely because they involve the same customer.

For existing WRMS calendar/email/activity notes, add the canonical WRMS project link when the content explicitly concerns WRMS. Preserve existing `NEA Engagement` links when they remain useful as a broader parent; the fix is to add the missing specific owner, not erase valid context.

The current `WRMS Personnel Update` capture is an example of a note that should not remain with only the plain text `Project: WRMS`. It must be linked to the canonical WRMS owner, or its content should be minimally incorporated into that owner if that is the cleaner representation.

## Personal vs work domain routing

`Personal` and `Work` are domains, not replacements for the ACES note types. Keep the established ACES structure and express the domain inside it instead of creating a second parallel vault architecture.

Preferred personal structure:

- `A Atlas/Personal/Personal MOC.md` — personal homepage / map of content;
- `A Atlas/Personal/Games/` — durable game/reference notes such as Honkai: Star Rail, Reverse: 1999, and Chaos Zero Nightmare;
- `C Calendar/Personal/` — personal schedules and calendar-like notes, including banner/event schedules when they are primarily temporal;
- `E Efforts/Personal/` — active personal projects and decision efforts such as the Gacha Calendar, iPhone migration, or Apple Watch selection;
- `S Sources/Personal/` — personal reference/source material;
- `Z System/Personal Automations.md` — personal recurring AI/automation workflows and their links.

Use a controlled frontmatter field such as `domain: personal` or `domain: work` when the note's purpose is clear and the vault's current metadata conventions allow it. The domain is determined by what the note/task is for, not by which AI, application, account, or automation created it.

Examples that should normally be treated as `domain: personal` when the content matches:
- Gacha Calendar and related schedule-checking work;
- Reverse: 1999 notes and banner/event tracking;
- Honkai: Star Rail and Chaos Zero Nightmare personal game notes;
- Apple Watch / personal-device purchase research;
- personal phone migration, travel, household, and lifestyle planning.

Examples that remain `domain: work`:
- GeBIZ tender pipeline and tender-review work;
- Activity Audit / knowledge sweep work;
- ACC ICT Roadmap;
- NEA AMS3 and other customer/project work;
- TOPPAN Ecquaria work skills and source refreshes.

An AI task does not become work merely because ChatGPT, Codex, Claude, or an automation created it. Personal recurring tasks belong under the Personal domain and should link back to `Personal MOC` and the relevant personal project/game note.

For existing legacy notes, do not mass-move or rename the vault. First re-read the note, verify its actual purpose, add the smallest useful `domain` metadata and Personal MOC/project links, and move it only when the destination is clear and the change is verified through Obsidian MCP. Ambiguous notes remain in place and are marked for `unresolved_routing` rather than guessed.

## Safety and verification

All graph repairs and live-vault removals are performed only by the approved local Codex writer through Obsidian MCP while holding the shared processor lock.

Before every patch or removal:

- re-read the current note through Obsidian MCP;
- preserve stable IDs, capture IDs, content hashes, provider IDs, user-authored text, and valid frontmatter;
- never touch `.obsidian/` or application state;
- never bulk move/rename/delete files without a verified repair manifest and bounded batch;
- never resolve an ambiguous owner by title similarity alone;
- for raw batch removal, require a verified external archive copy and compact index/checkpoint first.

After every patch or removal, re-read the exact destination/owner through Obsidian MCP, verify preserved source identity, and run a broken-link check for the affected cluster.

If the owner is ambiguous, use `unresolved_routing` and leave the evidence intact.

## User-approved operating model — 2026-09-21

This section records Benson's approved operating model and supersedes older wording in this file where there is a conflict.

### Mandatory Personal / Work routing

For every project, effort, capture, meeting, change request, service request, decision, task, source note, drawing, Canvas, or other substantive item that is routed into `E Efforts`, use an explicit controlled field:

```yaml
scope: personal # or work
```

Never infer `scope` from the fact that an item looks project-like, was created by an AI task, appears in a work-shaped folder, or has `classification: pa`. The existing PA/PM capture classification is independent of Personal/Work scope.

The approved effort roots are:

- `E Efforts/Personal/<canonical project>/...`
- `E Efforts/Work/<canonical project>/...`

Known personal examples include the Fold5 -> iPhone migration and Apple Watch / personal-device purchase work. Known work examples include customer projects, tenders, HubSpot sales work, ACC ICT Roadmap, NEA AMS3, and other TOPPAN Ecquaria work.

If the scope cannot be proved from the capture, an existing canonical owner, or explicit user wording, set/record `scope: unresolved` in the routing manifest, leave the item in `01 Inbox`, and do not guess.

### Inbox must be exhaustively processed

The Inbox is a queue, not a permanent filing area.

On every routing sweep, enumerate **every file and routable item under the live vault folder `01 Inbox/`**, not only `Capture Here.md`. Also enumerate every staged file under `obsidian-temp-notes/01. Inbox/` in GitHub.

This includes Markdown notes, Excalidraw drawings, Canvas files, and supported attachment companion notes. Binary/source attachments may remain in an attachment/source location when moving them would be unsafe, but they must be linked from the canonical owner or a companion source note.

For every Inbox item:

1. Preserve the original text/content and any stable source/capture identity.
2. Resolve `scope` first.
3. Resolve the most specific verified canonical owner.
4. If it belongs to an existing project, tender, deal, service request, change request, person, account, or subject, file it under or link it to that owner.
5. Add only controlled properties/tags and verified wikilinks; never invent near-duplicate tags.
6. Meetings, CRs, SRs, decisions, and other durable sub-items may remain separate child notes, but they must live under or link to the canonical project rather than becoming unrelated top-level notes.
7. Simple updates that do not deserve a separate lifecycle should minimally patch the canonical owner instead of creating a duplicate note.
8. Re-read and verify the destination through Obsidian MCP, then run the affected-cluster broken-link check.
9. Only after successful local verification may the original transient Inbox item be moved/cleared. Unresolved items stay in Inbox.

A routing sweep is not complete while a routable file remains silently stranded in `01 Inbox`.

### Unresolved routing / user decision

Do not interrupt Benson every day for ambiguous routing. Maintain a visible `## Needs Your Decision` section in the Inbox index (or the vault's equivalent Inbox decision note) that lists unresolved items with the minimum question required, for example:

- `Personal or Work?`
- `Existing project or new project?`
- `Which canonical project/deal does this belong to?`

The weekly ownership/link pass must surface these unresolved decisions to Benson. If no unresolved items exist, do not prompt.

A genuinely new project/deal must **not** be created merely because a capture looks new. Create it only when Benson explicitly says/implies that it is a new project/deal. Otherwise keep the item in Inbox as `unresolved_routing`.

### Canonical project identity and rename rule

Every canonical project should have a stable, non-name identity such as `project_id`. Human-readable names and file paths may change; the stable ID must not.

A project rename is initiated at the **canonical project hub**, not by renaming child notes individually. When Benson renames the canonical project hub (or changes its canonical project-name property), the locked local writer must propagate the rename safely:

1. preserve the same `project_id`;
2. update the canonical hub name/path through Obsidian MCP;
3. preserve the old name as an alias when useful;
4. update child-note project links/properties that reference the old canonical name;
5. rely on Obsidian link updates where available, then verify the links explicitly;
6. update relevant views/MOCs;
7. run a broken-link check.

Child notes are related to the project by stable owner identity + verified wikilink, not by a duplicated free-text project name.

### Date-oriented retrieval

Routing content into canonical owners must not destroy the ability to answer "what did I write last Tuesday?"

Preserve the source/capture timestamp for each ingested item. Use explicit date/time properties when appropriate (for example `captured_at`, `activity_date`, `created_at`, and `updated_at`) rather than relying only on filesystem creation time, which can change during sync/move operations.

Maintain a lightweight daily activity index under `C Calendar/Personal/` or `C Calendar/Work/` (or the vault's established daily-note location) for each day that has routed activity. The daily index should contain concise entries linking to the final canonical destination, not duplicate the full note. Example:

```markdown
- 14:32 [[NEA AMS3]] — Section 7 discussion routed to [[2026-09-17 Meeting with Kok Tiong]]
- 21:10 [[iPhone Migration]] — migration checklist updated
```

This gives two valid retrieval paths:
- by project/deal: open the canonical owner to see all related material;
- by date: open the daily index / date view to see what was captured or changed that day and jump to its canonical destination.

### Kanban / Base operating model

Do not use the Kanban itself as the source of truth. Cards represent canonical notes or CRM-backed deal notes.

Create one Active Projects Base with multiple views. The first/default view is **Work**; also provide **Personal** and **All** views. Work and Personal projects remain physically separated under `E Efforts/Work/` and `E Efforts/Personal/`.

Keep **Live Deals** as a separate HubSpot-derived Base/Kanban from delivery projects. A closed/won deal may later create or relate to a project with the same human-readable name, but the deal object and project object remain separate and use separate stable IDs.

The Live Deals Kanban uses HubSpot deal stages as its columns, preserving the user's chosen business ordering:

1. Tender Published
2. CAT 7 - Oppty Identified
3. CAT 6 - BQ Submitted
4. CAT 5 - Submission in Progress
5. CAT 4 - Submitted
6. CAT 3 - Completed Presentation / On-going Clarifications / Negotiation
7. CAT 2 - Negotiation / Pending LOA
8. CAT 1 - Won
9. Lost / Potential Lost
10. Dropped
11. Blocked
12. No Award
13. Customer Engagement
14. Lead Identified

Deal cards must expose at least: deal name, deal owner, deal type, and account/customer when available. Preserve HubSpot as the source of truth for CRM-owned fields; local Obsidian annotations remain user-owned.

Views may hide terminal or low-interest stages via per-view filters. Preserve a separate full-pipeline view so hidden stages are never mistaken for deleted data. Where the current Obsidian Kanban implementation supports manual view/group ordering or collapsing, preserve Benson's chosen ordering; otherwise use filtered views to achieve the same practical result without rewriting CRM stage values.

## Inbox file roles and canonical naming — 2026-09-21

### `01 Inbox/Capture Here.md` versus `01 Inbox/Inbox.md`

These two files have different permanent roles and must not be treated as disposable notes.

- **`Capture Here.md` is the user input buffer.** Benson may type free-form capture text there. Bob/ChatGPT may also stage captures into the GitHub handoff equivalent. The processor reads each capture, assigns/retains stable capture identity, resolves scope and owner, then routes the information to its canonical destination.
- **`Inbox.md` is the read/triage dashboard.** It is for Benson to read. It shows unresolved routing, `Needs Your Decision`, recent processing status, and links to items still waiting for action. It is not the normal place Benson must type raw captures.
- Neither `Capture Here.md` nor `Inbox.md` is ever deleted as part of normal processing.
- After a capture is verified in its destination, remove only that processed capture block from `Capture Here.md`; preserve the file itself and every concurrent/unprocessed entry.
- After an Inbox decision is resolved, remove/update only the corresponding dashboard row/section in `Inbox.md`; preserve the file itself.
- Any other note/file placed in `01 Inbox/` is a transient inbox item. Read its content, preserve source identity, and route it. If the note itself is a durable meeting/CR/SR/source/decision child, move it under the canonical owner. If its content is merged into an existing owner, remove the transient source note only after the destination has been locally re-read and verified. Never delete information merely because the Inbox sweep completed.
- If Benson accidentally types substantive content directly into `Inbox.md`, do not discard it. Treat that user-authored content as a routable Inbox item, preserve it, and then restore `Inbox.md` to its dashboard role after verified filing.

### Canonical naming for Work projects and deals

Every **work** project and every **work** deal must have one canonical human-readable name in this exact pattern:

`<Company Name> - <Project Name>`

Examples:
- `NEA - AMS3`
- `NEA - WRMS App Maintenance & Support Renewal`
- `ACC - ICT Roadmap`

The canonical organisation name comes from the verified organisation/account owner (prefer the existing canonical organisation note or HubSpot account name, including an established short name/acronym when that is the vault's canonical display name). Do not create a second organisation simply because Benson typed an acronym, abbreviation, spelling variant, or informal name.

Benson may type a project/deal name in any reasonable form. The router must normalize the wording against:
1. stable `project_id` or `deal_id`;
2. HubSpot/provider IDs when applicable;
3. the canonical organisation owner;
4. existing aliases;
5. verified context such as linked contacts, tender reference, or source record.

Once matched, route to the canonical `Company Name - Project Name` owner. Do not create a duplicate because the capture used different capitalization, punctuation, spacing, acronym, or shorthand.

Recommended canonical metadata:

```yaml
type: project # or deal
scope: work
project_id: project-...
deal_id: hubspot:deal:...
company: "[[NEA]]"
project_name: "AMS3"
canonical_name: "NEA - AMS3"
aliases:
  - "NEA AMS3"
  - "AMS3"
```

The deal object and project object remain separate even when they share the same `canonical_name`; their `type` and stable IDs distinguish them.

For **personal** projects, do not fabricate a company. Keep them under `E Efforts/Personal/` with a stable `project_id` and a natural project name such as `iPhone Migration`.

### Contact/person relationship model

A person is a canonical entity independent of any single organisation or project. Do not put the project company into the person's filename merely to force grouping.

Each person note should have one stable `contact_id`/provider identity when available, and controlled relationships such as:

```yaml
type: person
contact_id: person-...
organisations:
  - "[[Partner Company]]"
projects:
  - "[[NEA - AMS3]]"
relationship_types:
  - partner
tags:
  - entity/person
  - relationship/partner
```

Use controlled relationship values such as `customer`, `partner`, `subcontractor`, `vendor`, `internal`, `agency`, or another already-approved value. A person may relate to multiple organisations and multiple projects, and their organisation does **not** need to be the same as the project's company.

Where the relationship is project-specific, also make it explicit in the body, for example:

```markdown
## Project relationships
- [[NEA - AMS3]] — partner / subcontractor — via [[Partner Company]]
```

Use tags for broad retrieval and wikilinks/properties for the actual graph relationship. Never duplicate a person because they participate in more than one company/project.

### CRM identity verification before person classification

Before assigning a work person's relationship as customer, partner, subcontractor, vendor, agency, or internal, check the available authoritative work identity sources first when the person can be resolved there. At minimum:

1. search HubSpot CONTACT for matching provider records;
2. search HubSpot USER / owner identity when the person may be a TECQ employee;
3. prefer verified corporate email/domain, active owner/user identity, organisation association, and current role/function over assumptions from the project they appear in;
4. if the person is an active TECQ user/owner, classify the organisation relationship as `internal` unless stronger evidence proves otherwise;
5. functional role (for example `presales`) is separate from formal CRM job title and may be stored as a controlled function/role field when explicitly provided by Benson or verified from source material;
6. do not infer that someone belongs to the customer merely because they attend or contribute to that customer's project.

Known correction: **Kok Tiong Goh** is a TOPPAN Ecquaria internal presales participant. HubSpot currently resolves him as active owner `163267515` and an internal user on the Ecquaria corporate domain; the CRM job-title field reads `Technical Consultant`. Preserve Benson's explicitly supplied functional role as `presales`, while keeping the CRM job title separately as source data. Do not classify him as an NEA/customer contact merely because he works on NEA AMS3.

### Immediate priority override — 2026-09-21

Until completed or blocked by the absence of a valid local writer, process `obsidian-local-writers/repairs/2026-09-21-inbox-kanban-root-sweep.md` **before** the older source-batch compaction and WRMS maintenance backlogs. This is a direct user-requested repair covering the visible Inbox, Kanban/Base creation, accidental root duplicates/stray files, iPhone personal-project routing, canonical deal/project naming, and Kok Tiong identity correction.

Normal safety rules still apply: no cloud Drive writes to the live vault, no deletion before local MCP verification, preserve all user knowledge, and run the specified broken-link checks.

### Mandatory per-contact verification gate

For **every work person/contact encountered during capture routing, project/deal enrichment, meeting processing, or graph cleanup**, verify identity before writing organisation or relationship metadata.

Minimum gate:
1. search the canonical person entity first;
2. search HubSpot CONTACT for matching provider records;
3. if the person could be internal TECQ staff, also search HubSpot USER / owner identity;
4. compare corporate email/domain, provider IDs, organisation associations, existing aliases, and project context;
5. preserve formal CRM job title separately from functional/project role;
6. only after those checks write controlled organisation/relationship values such as `internal`, `customer`, `partner`, `vendor`, or `subcontractor`;
7. if records conflict or identity is ambiguous, leave `relationship_status: unresolved` and surface it in `01 Inbox/Inbox.md -> Needs Your Decision` instead of guessing.

This gate applies even when a person's organisation appears obvious from the project name. Project participation is never sufficient evidence of employer/organisation.

For already-existing person notes touched by a sweep, re-verify the relationship before preserving or changing it. Do not mass-reclassify untouched historical contacts without a reason to process them.

### Hubspot Live Deals compatibility and public-version view — 2026-09-21

Benson renamed the existing Live Deals folder to exactly `Hubspot Live Deals`. Detect and reuse that existing folder; do not recreate a parallel `Live Deals` folder.

For each HubSpot-backed deal note, hydrate and preserve these view properties from HubSpot/provider data:

```yaml
hubspot_deal_id:
deal_name:
account_name:
account:
deal_owner_name:
deal_owner_id:
deal_type:
hubspot_stage:
show_in_live_deals: true
```

Rules:
- `deal_name` comes from HubSpot `dealname`.
- `deal_owner_name` must be resolved from HubSpot `hubspot_owner_id` through the owner/user data; never display only the numeric owner ID.
- `account_name` must come from the associated HubSpot COMPANY when one exists. Also preserve an `account` wikilink to the canonical organisation note when it can be verified.
- If HubSpot has no associated company, do not fabricate one from the deal title; leave account unresolved and queue it for enrichment/review.
- Keep these CRM-owned fields read-only from Obsidian's perspective. A refresh may update them from HubSpot.
- `show_in_live_deals` is a local Obsidian view-control property only and must never write back to HubSpot.

The user's current Obsidian public build does not have access to the early-access native Kanban view. Do not require Obsidian 1.14/Catalyst for the operating dashboard. Use a compatible Base view now:

1. `Pipeline` — Cards (or Table if Cards cannot group cleanly), grouped by `hubspot_stage`, sorted first by `account_name` A→Z and then `deal_name` A→Z.
2. `By Account` — grouped/sorted by `account_name`, then `deal_name`.
3. `My Active` — filter `show_in_live_deals != false` and exclude terminal stages (CAT 1 - Won, Lost / Potential Lost, Dropped, No Award) unless Benson explicitly wants them.
4. `All Live Deals` — no local hide filter, for audit/recovery.

Ensure the displayed property order starts with the deal identity (prefer `file.name` or `deal_name`) followed by Account / customer, Deal owner, Deal type, HubSpot stage. Do not allow blank card headers merely because a display property is missing.

When Benson wants to hide a specific deal only from Obsidian, set `show_in_live_deals: false` on that deal note. This is view state, not CRM state.

When a future public Obsidian release includes native Kanban, the same hydrated properties can be reused without restructuring the deal notes.

### Active Work dashboard semantics — 2026-09-21

Benson expects the default work dashboard to show **current work**, not only delivery/internal project notes. Therefore the existing `00 Home/Active Projects.base` must keep its filename for continuity but its first/default `Work` view should include:

- active `type: project` notes under `E Efforts/Work/`; and
- active `type: deal` notes from `Hubspot Live Deals`.

Do not duplicate a HubSpot deal into a fake project note merely to make it visible. The same deal note should surface in the Work view. Show a visible `work_type` / Type field so users can distinguish `Deal` from `Project`.

Maintain additional views:
- `Projects Only`
- `Personal`
- `All`

The default Work view should sort by canonical account/company when present, then canonical name. Internal projects without an account should still remain visible.

Required current-work visibility (verified 2026-09-21):
- `NEA - AMS3` — deal
- `SIT - SITAR` — deal
- `LTA - LTA.PROMPT 2.0` — deal
- `BreadTalk - AI Initiative` — deal
- `PUB - AMS` — deal (HubSpot deal ID 348946112222; full CRM title: `PUB - Provision of Software Update and Maintenance Services for PUB's Asset Management System (AMS) PUB000ETT26000110`)
- `Lead Generation and Outreach` — project, with `Corporate Outreach` preserved as an alias when verified as the same effort.

For live-deal visibility, terminal CRM stages are excluded from normal active views but remain available in `All Live Deals`. HubSpot stage changes remain source-of-truth updates; local notes must never silently rewrite CRM lifecycle state.
