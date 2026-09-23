# Obsidian note UX standard

This is the human-facing layout standard for canonical project, deal, campaign, effort, and other working notes in the `Ben` vault. It governs presentation only. Identity, deduplication, locking, routing, and verification remain governed by `OBSIDIAN-INGESTION-CONTRACT.md`.

## Design goal

A note should answer these questions within a few seconds:

1. What is this?
2. What is happening now?
3. What do I need to do next?
4. What changed most recently?
5. Where can I type a quick note without fighting the automation?

Machine/audit metadata must not dominate the visible note.

## Default working-note layout

Use this order when the note type supports it:

```markdown
# <Canonical note title>

> [!summary] At a glance
> **Status:** <current human-readable status>
> **Current focus:** <one short sentence>
> **Next milestone:** <date or milestone when known>

## Next actions
- [ ] <action> — <owner/date only when useful>

## Working notes
<User-authored free-form notes. Preserve wording and placement. Bob/Codex must not auto-sort or rewrite this section merely for presentation.>

## Latest updates
### YYYY-MM-DD
- <most recent concise update>
- <link to meeting/source when useful>

### YYYY-MM-DD
- <older update>

## Decisions
- **YYYY-MM-DD — <decision>:** <concise decision and consequence>

## Meetings & notes
- [[YYYY-MM-DD Meeting title]] — <one-line outcome>

## References
- [[Relevant canonical note]]
- [[Supporting source]]
```

Do not create empty sections merely to fill a template. Omit sections with no useful content, except `Working notes` may remain as an intentionally empty capture area on active working notes.

## Human-facing ordering

- `Latest updates`: newest date first.
- `Next actions`: open/actionable items first; completed items may be collapsed, moved to dated history, or retained only when context matters.
- `Decisions`: newest material decision first unless the note already uses a deliberate decision-log structure.
- `Meetings & notes`: newest first.
- Daily calendar/source/audit notes may remain chronological when chronology is the purpose of the note.
- Internal queue processing may still run oldest-first for deterministic resume behavior. Processing order must never dictate reading order.

## Safe note-taking zone

`## Working notes` is the default human scratch area.

Rules:
- Preserve the user's exact wording.
- Do not reorder it automatically.
- Do not silently convert every bullet into a task, decision, or activity.
- When a working note clearly becomes a durable action/decision/update, copy or minimally promote the useful fact into the appropriate structured section while preserving the original note unless the user explicitly asks for cleanup.
- Never overwrite a user's free-form paragraph just to make the note prettier.

## At-a-glance rule

For active project/deal/campaign notes, keep `At a glance` compact. Target three lines:
- status,
- current focus,
- next milestone.

Do not repeat full CRM metadata, long summaries, provider IDs, capture IDs, or audit evidence here.

## Audit/provenance placement

Keep capture IDs, content hashes, provider IDs, sync state, and verification metadata in YAML/frontmatter, claims, checkpoints, or source/audit notes.

Do not create a visible `Inbox Activity`, `Processed Captures`, `Capture Ledger`, or equivalent section in a normal working note solely for ingestion bookkeeping.

If source provenance materially helps the user, surface only a concise source link in `References` or beside the relevant update. Full evidence belongs in the linked source note or a collapsed audit block on source/audit records.

## Update-writing rule

When Bob/Codex files a new capture into an existing working note:
1. preserve the canonical identity and user-authored content;
2. update `At a glance` only when the capture materially changes current status/focus/milestone;
3. add/merge actionable work under `Next actions`;
4. add the dated fact under the correct date in `Latest updates`, newest-first;
5. add a durable decision under `Decisions` only when a decision is actually supported;
6. link a meeting/source rather than duplicating its full contents;
7. keep provenance out of the visible body unless it helps the reader.

If the capture is only a quick thought and does not safely map to a structured fact yet, append it to `Working notes` without inventing meaning.

## Existing-note preservation

Do not rewrite a whole note merely to enforce this standard. Normalize incrementally:
- preserve custom headings that carry useful meaning;
- map obvious equivalents (for example `Action Items` -> `Next actions`) only when safe;
- never discard historical prose;
- do not duplicate the same update into multiple sections;
- prefer the smallest readable patch.

This standard is for ease of viewing and note taking, not cosmetic churn.

## Vault-wide coverage — 2026-09-23

This standard applies to **all human-readable notes in the live `Ben` vault**, not only projects and deals. Apply the appropriate note-type pattern below. The goal is consistency without forcing every note into the same template.

### Universal rules for every note

1. Put what Benson most needs to see near the top.
2. Prefer meaningful human language over machine fields in the visible body.
3. Preserve stable IDs and machine metadata in YAML/frontmatter.
4. Preserve all user-authored content.
5. Do not create empty decorative headings.
6. Prefer links over duplicated content.
7. When a note is time-oriented, make the most useful time order explicit.
8. Never let ingestion/audit mechanics become the main reading experience.
9. Every note must remain easy to append to manually without breaking automation.
10. A note may opt out of a standard section when its existing structure is clearly more useful.

### Note-type patterns

#### Project / deal / campaign / active effort
Use:
- `At a glance`
- `Next actions`
- `Working notes`
- `Latest updates` newest-first
- `Decisions`
- `Meetings & notes`
- `References`

The default working-note layout earlier in this file applies.

#### Meeting note
Optimize for what happened and what follows.

Preferred order:
```markdown
# YYYY-MM-DD — <Meeting title>

> [!summary] Outcome
> <1-3 sentence outcome>

## Actions
- [ ] <action> — <owner/date when known>

## Decisions
- <decision>

## Discussion notes
<User notes; preserve wording>

## Attendees
- [[Person]]

## Related
- [[Canonical project/deal/account]]
- [[Source]]
```

Do not bury actions below a transcript. If raw transcript/evidence exists, keep it below the readable note or in a linked source note.

#### Person / contact
Optimize for recognition and relationship context.

Preferred visible order:
- short identity/role summary;
- current organisation(s);
- relationship to Benson/TOPPAN Ecquaria when verified;
- active projects/deals;
- recent interactions newest-first;
- notes;
- references.

Do not expose CRM/provider IDs prominently in the body. Keep formal CRM title separate from project/function role.

#### Organisation / account
Optimize for account context.

Preferred visible order:
- what the organisation is / relationship;
- active deals/projects;
- key people;
- recent activity newest-first;
- working/account notes;
- references.

Do not turn the organisation page into a dump of every email or CRM record.

#### Daily / calendar note
Optimize for date retrieval.

Preferred order:
- date/title;
- open items or important events for that day;
- activity entries in time order when timestamps matter;
- links to canonical destinations.

Daily indexes may remain chronological because chronology is their function. They should link rather than duplicate full project content.

#### Task / action note
When a task deserves a standalone note, put:
- task/status/due date at the top;
- context/project link;
- next step;
- notes/history newest-first where useful.

Do not create a standalone task note for every checkbox. Prefer `Next actions` in the owning note unless the task has its own lifecycle or substantial context.

#### Decision note
Put:
- decision statement;
- date/status;
- rationale/evidence;
- consequences / follow-up;
- related project/meeting/source.

Do not rewrite uncertainty as a decision. A proposed choice is not a confirmed decision.

#### Tender / bid note
Treat as an active work note with tender-specific fields near the top when useful:
- agency/customer;
- tender/reference number;
- submission deadline;
- current stage;
- current focus;
- next milestone/action.

Then use the normal working-note pattern with latest activity newest-first.

#### Service Request / Change Request
Put:
- identifier/status;
- owner/project;
- impact/summary;
- next action;
- latest updates newest-first;
- decisions/approvals;
- evidence/references.

#### Personal project / purchase / planning note
Use the same working-note pattern as work projects, but do not add corporate/account fields. Keep practical next steps and comparison/decision context near the top.

#### Reference / evergreen knowledge note
Optimize for understanding, not activity tracking.

Preferred order:
- concise summary/definition;
- key points;
- details;
- examples/how-to;
- related notes;
- sources.

Do not add `Latest updates`, `Next actions`, or `Working notes` unless they are genuinely useful.

#### Source / evidence / imported email / CRM record
Optimize for readable evidence while preserving provenance.

Preferred order:
- human-readable subject/title;
- date/source/participants;
- concise summary;
- decisions/actions/outcomes if explicitly present;
- related canonical owner links;
- readable content;
- collapsed raw source/audit evidence when required.

Chronology and exact evidence take priority over dashboard styling. Do not fabricate an `At a glance` status for evidence-only notes.

#### MOC / index / dashboard
Optimize for navigation.

Preferred order:
- purpose/scope;
- most-used or active links first;
- grouped navigation;
- unresolved/attention items when relevant;
- lower-priority/reference links later.

Do not fill MOCs with duplicated prose from child notes.

#### Inbox control notes
- `Capture Here.md`: input first; newest raw captures at top; minimal instructions; no processed ledger.
- `Inbox.md`: decision/triage dashboard; unresolved items and `Needs Your Decision` near the top; recent processing status concise; no raw archive dump.

#### System / automation / runbook note
Optimize for operation:
- purpose;
- current status;
- how to use/run;
- inputs/outputs;
- failure/recovery;
- change log newest-first when useful;
- references.

Machine configuration may remain detailed when it is the point of the note, but still keep a readable operational summary first.

#### Archive / historical record
Preserve historical integrity. Add only minimal navigation/summary if needed. Do not reorder original user-authored chronology merely to match current working-note style.

### Automatic note-type detection

Use verified frontmatter, canonical folder/owner, stable IDs, and note purpose together. Do not decide note type from title alone.

If a note fits multiple roles, choose the primary role and preserve useful secondary sections. For example:
- a tender can also be a deal, but its primary working view may be tender;
- a meeting belongs to a project but remains a meeting note;
- a source email may contain actions, but remains evidence linked to the working owner.

If the primary note type is genuinely ambiguous, preserve the note and record `ux_type: unresolved` in the repair manifest rather than guessing.

### Global date-order rule

Use the order that best supports the note's job:
- active updates, recent interactions, decisions, meeting lists, change logs: **newest first**;
- raw captures: **newest first**;
- daily timestamped activity: **time order within that date** unless an existing deliberate reverse chronology is clearer;
- instructions/how-to/reference: logical order, not date order;
- immutable historical/source records: preserve source order unless a separate readable summary/index is added.

Internal processing order remains independent.

## Benson capture and home preferences — 2026-09-23

These are user-approved defaults:

- Bob uses **review by exception**: refine and auto-approve routine high-confidence notes; show a preview only when Benson requests one or when a material ambiguity needs confirmation.
- Bob proactively detects important durable decisions/actions/status changes/meetings in conversation and may capture high-confidence low-risk items automatically; ask only when uncertainty materially affects the result.
- Voice/dictation notes are cleaned into structured notes with raw wording preserved.
- Meaningful meetings receive dedicated meeting notes.
- Tasks remain authoritative in the owning note; Home exposes a central `My Tasks` dashboard linking to open actions.
- `00 Home/Ideas.md` is the low-friction home for unowned exploratory ideas.
- Person notes show detailed `Recent interactions`; organisation/account notes show a concise `Relationship pulse` linking to people/projects/meetings.
- Meaningful attachments get readable companion/source notes; ambiguity is surfaced to Benson.
- Daily notes include `Today's priorities`, `Quick notes`, schedule/meetings when useful, and activity links.
- Project lifecycle distinguishes `active`, `maintenance`, `dormant`, and `closed`. Delivery-complete projects with ongoing support are `maintenance`, not closed.
- Source/evidence notes stay mostly invisible in normal browsing; their useful facts are promoted into the appropriate canonical notes.
- Home priority is: Today, My Tasks, Active Projects, Live Deals, Inbox / Needs Your Decision, Ideas, Maintenance.

For detailed capture/inference rules, use `BOB-CAPTURE-INTELLIGENCE.md`.
## Relationship-note display preference — 2026-09-23

For people and organisations:

- **Person note:** full `Recent interactions` detail.
- **Organisation/company note:** short `Relationship pulse` summary only.
- Put a `Full notes →` wikilink to the person's canonical note immediately after each shortened company interaction summary.
- **Project/deal note:** only the project-relevant consequence/action/update.

Example company entry:

`- **22 Sep 2026 — Jasmine:** Followed up on JPEAE requirements and next steps. [[Jasmine Tan|Full notes →]]`

Do not duplicate the person's full interaction prose into the company page.
