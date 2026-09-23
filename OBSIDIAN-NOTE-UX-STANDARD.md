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
