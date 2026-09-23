---
repair_version: 1
repair_id: obsidian-repair-20260923-viewer-friendly-note-normalization
status: queued_for_local_writer
created_at: "2026-09-23T15:57:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
drive_mode: read_only
priority: user-request
ux_standard: OBSIDIAN-NOTE-UX-STANDARD.md
---

# Viewer-friendly working-note normalization — 2026-09-23

## Goal

Make Benson's day-to-day Obsidian working notes fast to scan and easy to type into without weakening ingestion, deduplication, or audit safety.

The machine may process captures oldest-first. Human-facing working notes must display current information first.

## Required target set

On the next authenticated local Codex run, after obtaining the shared processor lock, inspect and normalize only the following bounded set:

1. canonical project/deal/campaign/effort notes that contain one or more `capture_id` / `capture_ids` values from Bob ingestion;
2. any normal working note containing a visible heading or section named `Inbox Activity`, `Processed Captures`, `Capture Ledger`, or an equivalent ingestion-only ledger;
3. canonical active project/deal/campaign notes linked from the Home, Active Projects, or Live Deals working views when they were materially updated by Bob/Codex since 2026-09-14.

Do not mass-rewrite untouched historical/audit/source notes merely for cosmetics.

## Presentation standard

Follow `OBSIDIAN-NOTE-UX-STANDARD.md`.

Preferred visible flow:

```markdown
# <Title>

> [!summary] At a glance
> **Status:** ...
> **Current focus:** ...
> **Next milestone:** ...

## Next actions
- [ ] ...

## Working notes
...

## Latest updates
### YYYY-MM-DD
- ...

## Decisions
- ...

## Meetings & notes
- ...

## References
- ...
```

Omit empty sections except `Working notes` may remain intentionally blank on active notes.

## Ordering

- `Latest updates`: newest first.
- `Decisions`: newest first when the note uses a running decision list.
- `Meetings & notes`: newest first.
- Do not reorder the user's free-form `Working notes`.
- Daily calendar notes and source/audit records may keep chronological ordering where that is their purpose.

## Inbox Activity cleanup

For normal working notes:
- remove the visible `Inbox Activity`/processed-capture ledger only after confirming its facts are preserved in the correct human-facing section or linked source/audit note;
- retain capture IDs, hashes, source IDs, and verification metadata in YAML/frontmatter or existing source/audit records;
- do not delete unique user content;
- do not create a replacement visible audit section.

## Note-taking protection

`Working notes` is user-owned free-form space:
- preserve exact wording;
- never auto-sort it;
- never rewrite paragraphs for style;
- never convert every bullet into a task;
- promote only clearly supported durable actions, decisions, or updates to structured sections, while preserving the original note unless cleanup was explicitly requested.

## At-a-glance generation

For an active working note, derive at most:
- current status;
- current focus;
- next milestone.

Use existing verified note/CRM/project evidence only. Do not infer missing status or dates.

## Safety and verification

1. Refresh all authoritative Bob instructions including `OBSIDIAN-NOTE-UX-STANDARD.md`.
2. Acquire the repository-wide processor lock.
3. Authenticate Obsidian MCP and confirm active vault exactly `Ben`.
4. Re-read each target before editing.
5. Make the smallest safe presentation patch.
6. Re-read every changed note through Obsidian MCP.
7. Verify stable IDs, capture IDs/hashes, user content, links, and destination path.
8. Run affected-cluster broken-link checks.
9. Record exact changed paths and unresolved items in this manifest.
10. Set status to `completed` only after MCP verification.

No Google Drive writes. No direct filesystem writes.

## Completion report requested

When complete, record:
- number of working notes normalized;
- number of visible Inbox Activity / capture-ledger sections removed;
- number of Latest updates sections converted to newest-first;
- number of Working notes sections created/preserved;
- any notes skipped because their structure or ownership was ambiguous;
- broken-link result.
