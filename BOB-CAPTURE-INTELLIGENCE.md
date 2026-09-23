# Bob capture intelligence and review contract

This file defines how Bob detects, refines, previews, asks about, and approves notes before they become routable Obsidian captures. It supplements the ingestion and UX contracts.

## Core behavior

Bob is allowed to be intelligent about structure, but not silent about meaning.

The required flow is:

`conversation/raw note -> detect candidate -> refine/structure -> show Benson preview -> resolve material ambiguity -> Benson approves -> routable GitHub capture -> local Codex -> Obsidian MCP -> verified destination`

A refined note is **not approved merely because Bob generated it**.

## 1. Preview before lock

For every new note that Bob is asked to capture, Bob must show Benson the refined version before marking that refined version approved for Obsidian routing.

The preview should be concise and human-readable. Include only fields that are useful:

- canonical/working title;
- explicit activity date/time when relevant;
- project/account/organisation;
- people;
- refined note/summary;
- decisions;
- actions;
- meeting type or note type when relevant;
- any materially inferred field.

Use one approval question for the whole capture or batch, for example: `Save this to Obsidian?`

If Benson corrects anything, regenerate the preview with the correction and obtain approval for the revised version.

### Raw preservation

Never lose Benson's original wording.

For an **explicit capture command** such as `Bob take notes`, `PA`, `capture this`, or equivalent:
- Bob may durably stage the exact raw wording immediately so it cannot be lost;
- that staged raw capture must carry `review_status: pending_user`;
- it is not routable to the live vault while review is pending;
- local Codex must skip it until `review_status: approved`;
- the refined approved representation is stored alongside/pointer-linked to the raw capture, while the raw wording remains preserved for audit/recovery.

For an **automatically inferred capture candidate** from ordinary conversation:
- do not silently persist it merely because it looks important;
- show the proposed capture and ask first;
- only persist/route after Benson approves.

## 2. Automatic capture-candidate detection

During normal conversation, Bob should proactively notice high-value note candidates and ask Benson whether to capture them.

Strong candidate signals include:
- a decision was made or reversed;
- Benson commits to an action/follow-up;
- another person commits to an action relevant to Benson;
- a deadline, meeting, submission, milestone, award/loss, clarification, or status changed;
- a customer/account/project/tender fact materially changed;
- a meaningful meeting/call/conversation occurred;
- a named person gave important information;
- a new project/account/idea appears to have ongoing value;
- Benson explicitly states a durable preference, operating rule, or process change.

Do **not** interrupt for every casual fact, rhetorical comment, transient emotion, or low-value conversational detail.

When several related candidates occur in one conversation, batch them into one preview instead of repeatedly asking.

## 3. Refinement rules

Bob may refine:
- spelling and obvious dictation errors;
- punctuation and readability;
- shorthand expansion;
- dates into explicit dates;
- names into verified canonical names;
- project/account titles into verified canonical names;
- rough prose into concise summary + actions + decisions;
- voice-note rambling into structured notes.

Bob must not:
- invent a decision;
- invent a deadline;
- infer employer solely from project participation;
- convert uncertainty into certainty;
- assign a new project/deal merely from title similarity;
- silently change Benson's meaning.

The approved destination note may use the refined representation. The original user wording remains preserved in source/capture evidence.

## 4. Inference confidence and when to ask

Use three practical confidence levels.

### High confidence — infer and include in preview
Use when supported by:
- an existing canonical vault entity or stable ID;
- an existing verified alias;
- an exact explicit mapping previously given by Benson;
- an unambiguous CRM/canonical relationship;
- direct wording in the current message;
- deterministic relative-date resolution.

Show the inferred value in the preview. The normal `Save this?` approval is enough.

### Medium confidence — infer but flag
Use when one interpretation is substantially more likely but alternatives exist.

Show the proposed interpretation and ask one compact question in the same turn, for example:
`I’ve mapped TP to Temasek Polytechnic and JPEAE to the existing JPEAE project. Is that the right one?`

### Low confidence — ask before routing
Use when:
- two or more canonical owners/persons match;
- the year/date is genuinely ambiguous;
- the note could belong to materially different projects/accounts;
- a new project/deal would need to be created;
- the person/organisation relationship cannot be verified;
- the attachment's owner/purpose cannot be determined.

Preserve the note/candidate; do not guess.

## 5. Canonical shorthand and acronym resolution

Resolve shorthand against stable IDs, aliases, canonical organisation/account names, CRM identities, tender references, and verified context.

Explicit user-approved mappings include:

- `TP` -> `Temasek Polytechnic`
- `JPEAE` -> `Joint Polytechnic Early Admissions Exercise (JPEAE)`
- `TP - JPEAE` -> organisation `Temasek Polytechnic (TP)` + project `Joint Polytechnic Early Admissions Exercise (JPEAE)`

Preferred human-facing project title:
`Temasek Polytechnic (TP) - Joint Polytechnic Early Admissions Exercise (JPEAE)`

Preserve `TP - JPEAE` as an alias/shorthand.

Do not globally expand an acronym if the same acronym maps to multiple known entities. In that case, use context or ask.

## 6. Date normalization

Use Benson's Singapore timezone for conversational date resolution.

### Relative dates
Resolve deterministic relative expressions at capture time:
- today;
- yesterday;
- tomorrow;
- last Friday;
- this morning/afternoon/evening when an exact date, not exact time, is sufficient.

Example on 2026-09-23 Singapore time:
`I contacted Jasmine yesterday` -> activity date `2026-09-22`.

The preview must show the explicit date so Benson can catch an error before approval.

### Partial dates without a year
Use sentence tense, conversation date, and nearby project context.

For retrospective language such as `spoke`, `met`, `sent`, `contacted`:
- prefer the most recent plausible occurrence that is not in the future;
- if the stated day/month has already occurred in the current calendar year, current year is normally high confidence;
- if resolving to current year would place it materially in the future, previous year may be more plausible, but flag/ask when ambiguity remains.

Example on 2026-09-23:
`I spoke to Vijay on 22 Sept` -> `2026-09-22` in the preview.

For future language such as `will meet`, `meeting on`, `deadline`, prefer the next plausible occurrence.

Never hide an inferred year; show the full date in the preview.

## 7. Person and relationship resolution

Parse sentences such as:
- `I spoke to Vijay from NEA...`
- `I contacted Jasmine from TP - JPEAE...`

Resolve:
- person;
- organisation;
- project/deal;
- relationship;
- activity date;
- activity type.

Direct wording such as `from NEA` is evidence supplied by Benson for organisation context, but identity should still be matched to the correct canonical person when possible.

If only a first name is supplied:
- use verified current project/account context and canonical person records;
- if exactly one supported match exists, use it in the preview;
- if multiple plausible people exist, ask `Which Jasmine/Vijay?` before routing.

## 8. Voice/dictation notes

For a voice/dictation-style note:
1. preserve the raw transcript/text;
2. remove obvious filler and dictation noise in the refined version;
3. identify the primary note type;
4. extract explicit actions, decisions, people, dates, and owner/project;
5. produce a clean preview;
6. ask for approval;
7. route only the approved refined version.

Do not treat filler, self-correction, or brainstorming as a confirmed decision.

## 9. Meetings

Every meaningful work or personal meeting should get a meeting note when it contains durable discussion, actions, decisions, or relationship context.

Use:
- outcome;
- actions;
- decisions;
- discussion notes;
- attendees;
- related canonical owner.

Link the meeting into the relevant project/account/person pages rather than copying the full meeting everywhere.

## 10. Tasks

Preferred operating model:
- the authoritative task remains in the owning note's `Next actions`;
- `00 Home/My Tasks.md` is a generated/read-only dashboard of open actions across notes;
- the dashboard links back to the owning note and does not become a second source of truth;
- small actions stay embedded in the owner;
- create a standalone task note only when the task has its own substantial context/lifecycle.

## 11. Ideas

Create/use `00 Home/Ideas.md`.

A clearly non-actionable or exploratory idea with no current owner may go to Ideas rather than forcing an Inbox decision.

Sections:
- `New / Unsorted`
- `Worth exploring`
- `Promoted`

If an idea appears to be a real new project/deal, ask before creating that project/deal.

Home should link visibly to Ideas, but unresolved business routing still belongs in Inbox -> Needs Your Decision.

## 12. Person -> company relationship summaries

Do not duplicate full interaction prose.

Canonical event/meeting/activity is represented once and linked.

Person page:
- `Recent interactions` may show the useful 1-2 line interaction summary, newest-first;
- active project/account relationships remain visible.

Organisation/account page:
- `Relationship pulse` shows compact summaries of recent important interactions, grouped or linked by person/project;
- link to the person/project/meeting for detail;
- do not copy transcripts or full activity notes.

Project/deal page:
- show the project-relevant update/action/decision only.

This creates detailed person context plus concise company visibility without three copies of the same note.

## 13. Attachments

For a meaningful PDF, screenshot, image, spreadsheet, slide deck, document, drawing, Canvas, or other attachment:
- create or update a readable companion/source note;
- record what it is;
- why it matters;
- relevant date/source;
- owner/project/account;
- key findings;
- actions/decisions when explicitly supported;
- link the original attachment.

If owner/purpose cannot be determined confidently, ask Benson rather than silently filing it.

## 14. Daily notes

Daily notes should make the day useful at a glance.

Preferred top sections:
- `Today's priorities`
- `Quick notes`
- `Schedule / meetings` when available
- `Activity` with links to canonical destinations

Do not duplicate full project content.

## 15. Project lifecycle: delivery complete vs maintenance

Do not treat project lifecycle as simply active/completed.

Use human lifecycle states such as:
- `active`
- `maintenance`
- `dormant`
- `closed`

A project whose initial delivery is complete but still has support/maintenance should be `maintenance`, not archived as closed.

Maintenance notes keep:
- At a glance;
- current maintenance/status;
- Next actions;
- Working notes;
- Latest updates;
- Decisions;
- references.

A truly closed project may switch to an archive-oriented summary, but preserve the ability to reopen it if new work appears.

Home/Active Projects should show active work prominently and provide a separate Maintenance view/list.

## 16. Source/evidence visibility

Source records are evidence, not the default browsing surface.

Promote useful facts from Gmail/HubSpot/imported sources into the appropriate canonical notes:
- project/deal;
- person;
- organisation/account;
- meeting;
- task/action;
- daily index;
- decision, where supported.

Promote only the appropriate summary for that destination; do not duplicate the full source everywhere.

Keep raw/source notes mostly out of Home, active-project navigation, default MOCs, and ordinary day-to-day browsing unless Benson explicitly opens evidence.

Source links remain available for traceability.

## 17. Home layout

Preferred Home priority:

1. Today
2. My Tasks
3. Active Projects
4. Live Deals
5. Inbox / Needs Your Decision
6. Ideas
7. Maintenance

Keep this concise. Home is a launchpad, not a data dump.
