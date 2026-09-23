# Claude handoff — Bob / Obsidian behavior update

Last updated: 2026-09-23

Claude: read this file before handling Benson's Bob/Obsidian captures, notes, routing, or project handoffs.

This is a summary only. The canonical rules remain in:

- `BOB-CAPTURE-INTELLIGENCE.md`
- `OBSIDIAN-NOTE-UX-STANDARD.md`
- `OBSIDIAN-INGESTION-CONTRACT.md`
- `OBSIDIAN-DELIVERY-ARCHITECTURE.md`
- `OBSIDIAN-GRAPH-HYGIENE.md`
- `CODEX-CLOUD-INSTRUCTIONS.md`

## Important architecture boundary

Claude is a **capture / staging / reasoning layer**, not the final Obsidian writer.

Allowed flow:

`Benson -> Claude/Bob capture -> GitHub staging -> local Codex -> authenticated Obsidian MCP -> live Ben vault -> verification`

Do not:
- write directly to the live Obsidian vault;
- write the live vault through Google Drive;
- claim a note reached Obsidian merely because it exists in GitHub or Drive;
- create or take the local processor lock.

Claude may stage approved captures into GitHub. If Benson has not approved the refined note, use `review_status: pending_user`.

## New capture behavior

Bob/Claude should be smarter than literal transcription.

### Required flow

`raw conversation/note -> infer context -> refine/format -> show Benson preview -> ask/confirm material assumptions -> Benson approves -> stage as approved`

A refined note is not approved merely because Claude/Bob created it.

For an explicit capture request:
- preserve Benson's original wording;
- it may be staged immediately for durability as `review_status: pending_user`;
- show the refined version to Benson before the refined form is routable;
- after Benson approves, mark it `review_status: approved`.

For ordinary conversation:
- detect important decisions, actions, meetings, status changes, commitments, deadlines, and ideas;
- proactively propose a capture;
- do not silently persist an inferred ordinary-conversation note without approval.

## Read between the lines

Infer the likely meaning from the whole statement and recent context before asking Benson to repeat information.

Use verified aliases, canonical entities, CRM context, project context, dates, and people.

Example:

`TP - JPEAE`

should resolve to:

`Temasek Polytechnic (TP) - Joint Polytechnic Early Admissions Exercise (JPEAE)`

Preserve `TP - JPEAE` as an alias.

If the inference is new/material, tell Benson what was inferred and ask him to confirm.

Example:

`I read "TP - JPEAE" as Temasek Polytechnic (TP) - Joint Polytechnic Early Admissions Exercise (JPEAE). Correct?`

Once Benson has explicitly confirmed an alias/person/relationship, reuse it later without asking the same question again unless new evidence conflicts.

## Dates

Benson is in Singapore time.

- explicit relative date: resolve it and show the explicit date;
- missing date for a past/present interaction: assume today's Singapore date and flag it;
- partial past date without year: infer the most recent plausible year and show it explicitly;
- never hide an assumed date.

Example on 2026-09-23:

`I spoke to Vijay from NEA`

-> `Date: 23 Sep 2026 (assumed today) — correct?`

`I spoke to Vijay from NEA on 22 Sept`

-> `22 Sep 2026` in the preview.

## People

Before creating a new work person:

1. check existing canonical person notes;
2. check authorised HubSpot CONTACT;
3. if they may be TOPPAN Ecquaria internal, check HubSpot USER/owner;
4. use project/company context only as supporting evidence, not proof of employer.

If a likely existing match is found, ask Benson whether it is the same person before creating a duplicate.

If no supported match exists, propose creating a new person.

## Person / company / project display

Benson's preferred model:

### Person note
Keep the **full interaction notes** in `Recent interactions`, newest-first.

Include useful detail such as:
- date;
- project/account;
- discussion;
- actions;
- commitments;
- outcome/decision;
- related meeting/source.

### Company/organisation note
Keep only a short `Relationship pulse` summary.

Immediately after each short summary, add a link to the person's full notes.

Example:

`- **22 Sep 2026 — Jasmine:** Followed up on JPEAE requirements and next steps. [[Jasmine Tan|Full notes →]]`

Do not duplicate the full interaction text on the company page.

### Project/deal note
Keep only the project-relevant consequence/action/update, with person/meeting links where helpful.

## Tasks

Recognise task intent automatically.

Examples:
- `I need to call Tracy`
- `I should follow up with TP`
- `remind me to send the proposal`

These are task candidates even without the word `task`.

Authoritative task location:
- owning note's `Next actions`.

`00 Home/My Tasks.md` is a generated navigation/dashboard, not a second task source of truth.

## Ideas

Exploratory language such as:
- `maybe`
- `what if`
- `could we`
- `idea:`

may be proposed for `00 Home/Ideas.md`.

Do not turn an idea into a committed task/project/deal without Benson's approval.

Preferred Ideas sections:
- `New / Unsorted`
- `Worth exploring`
- `Promoted`

## Meetings

Every meaningful meeting with durable discussion/actions/decisions should get a meeting note.

Preferred structure:
- Outcome
- Actions
- Decisions
- Discussion notes
- Attendees
- Related project/account/source

Link the meeting into the relevant person/project/account rather than duplicating the full meeting everywhere.

## Voice/dictation

For rambling or dictated notes:
- preserve raw wording/transcript;
- remove filler in the refined version;
- infer note type;
- extract actions, decisions, people, dates, and project/account;
- show the refined preview;
- ask for approval before routing.

## Attachments

For meaningful PDFs, screenshots, images, spreadsheets, documents, slide decks, Excalidraw, Canvas, etc.:

create/update a readable companion/source note containing:
- what it is;
- why it matters;
- date/source;
- owner/project/account;
- key findings;
- actions/decisions when supported;
- link to the original attachment.

If ownership/purpose is unclear, ask Benson.

## Note UX

Human-facing notes should be easy to scan and easy to type into.

For active working notes, prefer:

- At a glance
- Next actions
- Working notes
- Latest updates — newest first
- Decisions
- Meetings & notes
- References

`Working notes` is Benson's free-form scratch area. Do not auto-sort or rewrite it.

Do not expose `Inbox Activity`, `Processed Captures`, capture ledgers, hashes, or provider IDs prominently in normal working notes. Keep machine/audit data in frontmatter/source/audit layers.

## Home

Preferred Home launch order:

1. Today
2. My Tasks
3. Active Projects
4. Live Deals
5. Inbox / Needs Your Decision
6. Ideas
7. Maintenance

## Project lifecycle

Do not treat everything as only active/completed.

Use:
- `active`
- `maintenance`
- `dormant`
- `closed`

A delivery-complete project with ongoing support/maintenance remains `maintenance`, not closed.

## Source/evidence notes

Gmail/HubSpot/imported evidence should stay mostly invisible during normal browsing.

Promote the useful fact into the relevant:
- project/deal;
- person;
- organisation/account;
- meeting;
- task;
- decision;
- daily index.

Keep the raw source available for traceability, but do not make source batches the main reading surface.

## Current full-vault normalization status

Benson instructed on 2026-09-23: **process it all**.

The full-vault repair is:

`obsidian-local-writers/repairs/2026-09-23-viewer-friendly-note-normalization.md`

Status is currently `awaiting_local_writer`.

The cloud side has already identified first-batch stale UX in:
- `00 Home/Home.md`
- `01 Inbox/Capture Here.md`
- `01 Inbox/Inbox.md`

Final live-vault changes must still be made by local Codex through authenticated Obsidian MCP.

## Claude handoff rule

When Claude creates a handoff for Bob/Obsidian:

- preserve original Benson wording/provenance;
- apply the smart inference/refinement rules above;
- show Benson the refined form when the interaction permits;
- if Benson approves, stage as `review_status: approved`;
- otherwise stage as `review_status: pending_user`;
- never treat Claude's own inference as Benson's approval;
- never write the live vault directly.

If in doubt, preserve the information and ask the smallest useful clarification rather than guessing.
