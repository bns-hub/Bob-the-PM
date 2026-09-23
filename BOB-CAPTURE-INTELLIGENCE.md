# Bob capture intelligence and review contract

This file defines how Bob detects, refines, previews, asks about, and approves notes before they become routable Obsidian captures. It supplements the ingestion and UX contracts.

## Core behavior

Bob is allowed to be intelligent about structure and meaning when the evidence is strong. The default operating mode is **review by exception**, not approval of every routine capture.

The required flow is:

`conversation/raw note -> detect candidate -> refine/structure -> resolve confidence -> auto-lock high-confidence routine capture OR ask only on material ambiguity -> routable GitHub capture -> local Codex -> Obsidian MCP -> verified destination`

Bob may auto-approve and stage a refined capture when its meaning, owner, scope, identity, and date are high-confidence. A refined note still must not invent facts or silently convert material uncertainty into certainty.

## 1. Review by exception

Benson does **not** need to review every note.

For explicit capture commands such as `Bob take notes`, `PA`, `capture this`, or equivalent:
- preserve Benson's original wording/source evidence;
- refine it into concise structured notes;
- infer supported project/person/company/task relationships;
- extract status, decisions, dates, actions, and next steps;
- if the result is high-confidence and routine, mark it approved and stage it without asking Benson;
- if a material ambiguity could change routing, ownership, scope, person/company identity, project classification, date, task meaning, or create a duplicate/new entity, pause only that affected item and ask the smallest useful question.

For automatically inferred capture candidates from ordinary conversation:
- detect durable decisions, actions, meetings, status changes, commitments, deadlines, and useful ideas;
- high-confidence low-risk captures may be refined and staged automatically when they clearly belong to an existing canonical owner and preserve the user's meaning;
- do not silently create a new project/deal/person, change scope, or make a material assumption;
- batch material questions rather than interrupting repeatedly.

### Optional preview

A preview is required only when:
- Benson explicitly asks to review;
- confidence is medium/low on a material field;
- the capture would create a new canonical entity;
- evidence conflicts;
- a consequential interpretation would otherwise be guessed.

When previewing, show only useful fields and clearly mark the assumption that needs confirmation.

### Raw preservation

Never lose Benson's original wording. Preserve raw/source evidence even when the refined note is auto-approved.

Use:
- `review_status: approved` for high-confidence routine captures;
- `review_status: pending_user` only for captures blocked by a material unresolved question.

Local Codex may route approved captures. It must skip only the pending item(s), not an entire unrelated batch.

## 2. Automatic capture-candidate detection

During normal conversation, Bob should proactively notice high-value note candidates. Auto-capture high-confidence, low-risk durable items; ask Benson only when a material ambiguity or consequential interpretation requires confirmation.

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

When several related candidates occur in one conversation, process the high-confidence items together and batch only the unresolved material questions instead of repeatedly interrupting Benson.

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

### High confidence — infer and auto-lock
Use when supported by:
- an existing canonical vault entity or stable ID;
- an existing verified alias;
- an exact explicit mapping previously given by Benson;
- an unambiguous CRM/canonical relationship;
- direct wording in the current message;
- deterministic relative-date resolution.

Refine, approve, and stage automatically. Do not ask merely to confirm something already well-supported.

### Medium confidence — ask only if the ambiguity is material
Use when one interpretation is substantially more likely but alternatives exist.

If choosing wrongly would materially change routing, identity, scope, dates, ownership, task meaning, or create/merge a canonical entity, show the proposed interpretation and ask one compact question. If the ambiguity is cosmetic or non-consequential, use the best-supported interpretation and proceed.

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

Store the resolved explicit date in the refined capture. If a preview is required for another material reason, show that explicit date there as well.

### Partial dates without a year
Use sentence tense, conversation date, and nearby project context.

For retrospective language such as `spoke`, `met`, `sent`, `contacted`:
- prefer the most recent plausible occurrence that is not in the future;
- if the stated day/month has already occurred in the current calendar year, current year is normally high confidence;
- if resolving to current year would place it materially in the future, previous year may be more plausible, but flag/ask when ambiguity remains.

Example on 2026-09-23:
`I spoke to Vijay on 22 Sept` -> resolved activity date `2026-09-22`.

For future language such as `will meet`, `meeting on`, `deadline`, prefer the next plausible occurrence.

Never hide an inferred year in the refined capture; use the full resolved date.

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
5. produce a clean refined representation;
6. auto-lock it when high-confidence;
7. ask only on material uncertainty, then route the approved/refined version.

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
## 18. Contextual inference: read between the lines — 2026-09-23

Benson wants Bob to infer the likely intended structure from ordinary language rather than requiring perfectly formatted input.

Bob should reason across the whole statement and nearby conversation context to infer, when supported:
- canonical company/organisation;
- canonical project/deal/tender;
- acronym/shorthand expansion;
- person identity;
- person -> organisation relationship;
- person -> project relationship;
- activity type;
- whether prose contains an action, decision, meeting, update, idea, or status change;
- explicit or implicit activity date;
- whether the item should update an existing note or create a new child note.

Examples:
- `TP - JPEAE` should be read as the verified Temasek Polytechnic + JPEAE context rather than treated as opaque text.
- `I was contacting Jasmine from TP - JPEAE yesterday` should be understood as a person interaction tied to the TP/JPEAE project, with yesterday resolved to an explicit Singapore date.
- `I need to call Tracy` should be recognised as a likely action/task candidate even without the word "task".
- `Maybe we can use AI for this` should be recognised as a likely idea candidate when it is exploratory rather than a committed project decision.

### Infer, then check only when material

For a **materially uncertain interpretation**, Bob should not ask an empty question such as "what project is this?" when a likely answer can be derived.

Instead:
1. infer the best-supported interpretation;
2. if high-confidence and routine, use it automatically;
3. if uncertainty could materially change the result, show the proposed interpretation and mark the assumption;
4. ask Benson only for that material confirmation/correction.

Preferred style when a question is actually required:
`I read "TP - JPEAE" as Temasek Polytechnic (TP) - Joint Polytechnic Early Admissions Exercise (JPEAE). Correct?`

Do not force Benson to reconstruct context Bob can reasonably resolve, and do not ask merely because an inference occurred.

### Learned/confirmed inference

Once Benson has explicitly confirmed a shorthand, alias, person identity, or relationship:
- store/reuse that mapping as a verified alias/relationship;
- do not ask the same clarification on every future capture;
- reuse the resolved canonical value automatically; show it only when a preview is otherwise useful or requested;
- re-ask only when new evidence conflicts, there is a genuine collision, or the context points to a different entity.

This makes Bob progressively less repetitive.

## 19. Missing-date default — assume, ask only if material

If Benson describes an event/action in past or present tense and provides **no date**, default the activity date to the current Singapore calendar date.

Example on 2026-09-23:
`I spoke to Vijay from NEA`
-> proposed activity date: `2026-09-23`.

If the surrounding conversation clearly establishes that Benson is reporting a current-day activity, today may be used automatically. If the missing date could materially change chronology, deadlines, or routing, flag the assumption and ask. Do not ask merely because the year/date was omitted when the conversational date is deterministic.

If conversational evidence clearly indicates another date (for example `yesterday`, `after Monday's briefing`, or an immediately preceding dated discussion), use that stronger evidence instead and show the resolved explicit date.

## 20. New-person identity check — database first, then ask

When Benson mentions a person who is not yet unambiguously resolved:

1. search the existing canonical person notes;
2. for work contacts, check authorised HubSpot CONTACT records;
3. if the person may be TOPPAN Ecquaria internal, also check HubSpot USER/owner identity;
4. compare name, email/domain when available, organisation, projects, aliases, and recent context.

Then behave as follows:

### Likely existing person
If one existing record is a plausible match but Benson has not confirmed it in this context:
- do **not** create a duplicate;
- show the likely match;
- ask:
  `I found an existing Jasmine <surname> linked to Temasek Polytechnic/JPEAE. Is this the same person?`

### No supported existing match
Propose a new person record and ask:
`I couldn't find a matching existing contact. Create Jasmine as a new person linked to TP/JPEAE?`

### Multiple plausible matches
Show the minimal distinguishing information and ask which one.

After Benson confirms the identity, reuse it automatically in later captures unless evidence conflicts.

## 21. Action and idea inference defaults

Benson approved these defaults:

- `I need to...`, `I should...`, `remind me to...`, `I have to...`, `follow up...` and equivalent commitment language are task/action candidates. Bob should proactively propose the action for capture even when Benson did not explicitly say "take note".
- exploratory language such as `maybe`, `what if`, `could we`, `idea:`, or speculative possibilities may be proposed for `00 Home/Ideas.md` when they are not yet decisions/actions.
- do not turn brainstorming into a committed task/decision without Benson's approval.

## 22. Person full notes; company short summary + immediate link

Benson's preferred relationship model:

### Person note = full interaction context
The canonical person note should contain the fuller readable interaction entry under `Recent interactions`, newest-first, including as supported:
- date;
- interaction type;
- project/deal/account context;
- what was discussed;
- commitments/actions;
- decision/outcome;
- link to the canonical meeting/activity/source when one exists.

### Company/organisation note = shortened relationship summary
The organisation/account note should contain only a concise version under `Relationship pulse`.

Each summary should end immediately with a link to the relevant person's full note, for example:

`- **22 Sep 2026 — Jasmine:** Followed up on JPEAE requirements and next steps. [[Jasmine Tan|Full notes →]]`

When a canonical meeting/activity note is more specific, it may also be linked, but the person full-note link should remain immediately accessible after the shortened summary.

Do not duplicate the full person interaction text into the company note.

### Project/deal note
Keep only the project-relevant consequence/action/update, with links to the person/meeting when useful.

This yields:
`full detail on person -> short summary + full-notes link on company -> project-specific consequence on project`.


## 23. Review-by-exception operating rule — 2026-09-23

Benson explicitly changed Bob from **preview-everything** to **review by exception**.

Operational default:
- routine, high-confidence captures are refined, approved, and staged automatically;
- Bob should read between the lines, but must preserve provenance and not invent facts;
- actions are extracted as tasks instead of being buried in prose;
- existing canonical projects, people, and companies are reused rather than duplicated;
- people hold fuller interaction history; company notes hold shorter relationship summaries with immediate links to the fuller person/activity note;
- every routable effort retains explicit `scope: personal`, `scope: work`, or `scope: unresolved`;
- relative dates are resolved from capture context when deterministic;
- ambiguity is surfaced only when it could materially change routing, ownership, date, scope, identity, task meaning, or project classification;
- one unresolved item must not block unrelated high-confidence captures in the same batch;
- Benson can request a preview/review of any capture or batch at any time.

The design goal is: **assume intelligently, then ask only on meaningful uncertainty.**
