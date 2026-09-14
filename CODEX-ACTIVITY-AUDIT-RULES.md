# Codex activity-audit rules (permanent)

This is a standing operating spec for the Codex/ChatGPT cloud activity-audit task. On the Codex/ChatGPT side this recurring job is named **"Obsidian Export"** — that is the schedule this file governs. It runs entirely in ChatGPT cloud, not here. The one part that involves a Claude cloud session is called out explicitly below: Claude hands off notes only through the GitHub `obsidian-temp-notes` folder in this repository, and never writes to Google Drive or Gmail directly.

**"Obsidian Export" must run daily.** Every run reads Bob-the-PM's GitHub inbox (`obsidian-temp-notes/01. Inbox/Capture Here.md`) and the `daily-agenda/notes.md` and per-project `notes.md` content queued there, and files anything new into the real Drive-backed vault, verifying the write actually landed before removing the source entry from GitHub (see the note added 2026-09-14 after a capture was marked filed but was not actually found in the vault). A less-than-daily schedule lets daily-agenda items go stale before they ever reach Obsidian.

Recorded here verbatim, as supplied by the user, for reference by Codex and any future session.

---

Permanent account-boundary rule: Before any source read or Drive write, verify connector identities independently:
Google Drive access is via the work account, bensonfoo@ecquaria.com, connected intentionally, with shared access into bnsn4ull@gmail.com's personal Drive space. This Codex task itself continues to read from and write to that Drive space as specified below, the vault root and the checkpoint folder, that is unchanged and expected. What is off-limits is a separate Claude cloud session writing to Google Drive directly, that has proven unreliable there, file-level sharing consistently failed, so Claude hands off notes through the GitHub folder described below instead, never through a direct Drive write. bnsn4ull@gmail.com itself has no connection here at all, only its Drive folder is shared in.

Gmail work activity must use only the connection named exactly "Work", authenticated exactly as bensonfoo@ecquaria.com or its alternative login bensonfoo@toppanecquaria.com, both are the same person, Benson Foo.

HubSpot activity must use only the authorized work portal for bensonfoo@ecquaria.com or bensonfoo@toppanecquaria.com.

Never read, search, summarize, or export Gmail belonging to bnsn4ull@gmail.com, that account has no Gmail connection here at all, personal or otherwise. If a write target cannot be confirmed as inside the shared personal Drive space, pause the write and report which folder failed to resolve. If the Gmail connection name or identity mismatches, abort Gmail processing before reading any message content and report a security issue.

Run this activity-audit export entirely in ChatGPT cloud with no dependency on any separate ingestion process.

Incrementally export only new or changed authorized work activity from:
- Gmail connection whose nickname is exactly "Work" and whose authenticated account is exactly bensonfoo@ecquaria.com or bensonfoo@toppanecquaria.com;
- connected HubSpot, treating owner ID 86653749 as canonical Benson Foo;
- authorized ChatGPT/Codex activity available to this cloud task; and
- Claude handoffs separately supplied in the existing Google Drive structure.

Additional source: Bob-the-PM temp notes. On every run, check the GitHub repository bns-hub/Bob-the-PM, folder obsidian-temp-notes. Its structure mirrors the real vault's own folder paths, so obsidian-temp-notes/01. Inbox/Capture Here.md corresponds to 01. Inbox/Capture Here.md in the vault. It may contain zero, one, or several files, process every file present, not just the first, in a deterministic order, oldest created first, so a run that stops partway resumes predictably next time.

For each file, apply the same one-click capture rule defined below, same parsing, same categorisation and routing, same deduplication by stable ID and content hash against existing vault notes.

Once a file's content has been fully and successfully filed into the real vault, remove that entry from the GitHub copy and push the change. If filing fails, leave the entry in place for retry on the next run, and continue on to the other entries rather than stopping the whole run.

This is the one safe place a separate Claude session is permitted to write directly, for handing off notes, since it cannot reach the local device or write to Drive.

## Google-Drive/Obsidian sync-safety rule

Treat the Drive-backed vault as a live synchronized filesystem. Never modify anything under .obsidian/ and never rewrite, rename, move, or delete existing vault files merely for tidying, formatting, or presentation. Prefer creating a new Markdown note at the intended stable path when importing a new item. Before changing any existing Markdown note, re-read the current Drive version immediately before the write and compare its stable Drive ID, modified timestamp when available, and content hash against the version previously inspected in the same run. If the existing note changed after inspection, do not overwrite it; classify the item as a sync conflict, leave the GitHub staging source intact, checkpoint the conflict, and retry on a later run. Avoid bulk folder moves, mass renames, or mass rewrites. Do not touch lock, cache, workspace-state, plugin-state, or hidden Obsidian application files. For GitHub handoff files, delete the staging copy only after the destination Drive write has succeeded and the written destination can be re-read and verified to contain the expected capture ID/content hash. If verification fails, retain the GitHub staging file for retry. When updating an existing owner/project/tender note is required, make the smallest content-preserving patch needed and preserve user-authored content and frontmatter unless the rule explicitly requires a specific metadata field.

Use Google Drive folder ID 1hn2zMhML2HZLu5L77Vt422D3nnk0dDi- as the primary Drive-backed Obsidian vault root. At the start of each run, verify that the connected Drive account can resolve and write into this folder. Preserve its existing A Atlas, C Calendar, E Efforts, S Sources, and Z System tree exactly. Do not rename, move, delete, flatten, or recreate those top-level folders. Route final-form, human-readable Activity Audit Schema Markdown and related evidence metadata into the appropriate existing vault folders so the cloud output is complete and directly usable without a separate ingestion pass.

Continue writing a compact manifest and successful checkpoint copy to Google Drive folder ID 1T2X6ngN73ZgOOCqGVXoG0XIdybJAo93h, preserving the existing checkpoint lineage. Do not use the handoff folder as the primary destination for final-form notes when the vault root is writable. If the vault root cannot be resolved or is not writable, keep the recurring task active, do not create a replacement tree elsewhere, write only the compact failure/checkpoint state to the handoff folder when permitted, and notify an actionable file-sync/access failure stating that the remaining bridge cannot yet be removed.

Process only deltas since the last successful checkpoint, using a seven-day overlap where appropriate. Compare saved source cursors, stable provider IDs, modified timestamps, Drive file IDs, counts, and content hashes before fetching bodies or attachments. Hydrate only new, changed, previously failed, or unresolved records and attachments. Deduplicate by stable source ID plus hash and preserve retry state for failed or unresolved items.

Treat Benson Foo as the canonical self identity and preserve canonical Benson Foo and TOPPAN Ecquaria entity links. Preserve explicit verified roles and relationships such as mailbox_owner, sender, recipient, author, actor, attendee, crm_owner, and source-to-evidence links. Never infer authorship, ownership, or relationships merely from mailbox presence or name similarity. Keep uncertain associations unresolved until supported by exact provider IDs or source evidence.

Keep routine work as retrieval_status audit-only and do not promote it into narrative knowledge unless the Activity Audit Schema explicitly requires that. Use human-readable contextual filenames and headings; keep provider IDs and machine identifiers in YAML/frontmatter, manifests, checkpoints, or evidence metadata rather than filenames and headings.

On an unchanged run, remain quiet; update only the minimal checkpoint data needed to preserve cursor integrity if necessary. Notify only when there is a newly completed export, an actionable failure, a security issue, or confirmed completion of the historical backlog. In any notification, report concise counts, changed outputs, failures, and unresolved items without exposing unnecessary message content.

## Permanent readability and historical normalization rule

For every newly imported Markdown note and every existing Markdown note that contains JSON, escaped HTML, a raw API payload, or an opaque source dump from authorized Gmail, HubSpot, ChatGPT/Codex, or authorized Claude exports, make the human-readable representation the primary view.

For each applicable note:
- Use a descriptive human-readable title.
- Present clear fields when available: date, source, sender/from, recipients/to/cc, subject, record type, Benson Foo's role, related people, companies, deals, and tasks, concise summary, decisions, actions, outcomes, attachments or artifacts, stable source ID, and verified wikilinks.
- Convert email HTML and escaped HTML into clean readable text while preserving meaningful structure, links, lists, and tables.
- Preserve the complete original payload losslessly after the readable view inside a collapsed HTML details block whose summary is exactly "Raw source (audit evidence)". Put the original JSON, HTML, API response, or opaque source dump inside an appropriate fenced code block within that collapsed section. Verify the preserved raw payload against its saved hash.
- Keep every existing source ID, record identity, relationship, role, and verified wikilink stable. Do not replace stable notes or create new IDs merely for presentation changes.
- Do not create isolated person, company, deal, task, message, or artifact nodes merely to improve display. Reuse verified canonical entities and create graph links only when supported by exact IDs or evidence.

Apply this rule immediately to every new addition before considering that addition complete. On every weekly run, also convert a bounded, resumable batch of the historical JSON-first or opaque-first Markdown backlog. Bound the batch to fit the run safely; process in deterministic stable order. Persist an exact checkpoint listing each file and source record converted, its before-and-after hash, failures or unresolved items, and the next resume position. Continue from that position on subsequent weekly runs until a full deterministic scan confirms that no JSON-first notes remain, then record and notify historical completion.

## Permanent one-click capture and vault enrichment rule

On each run, inspect the vault note at 01. Inbox/Capture Here.md. Process only unprocessed content in the New captures section. If the marker is missing or the section boundaries are ambiguous, do not guess or consume content; record an actionable failure.

Parse captures as follows:
- A line containing only --- is an explicit entry separator.
- Also accept natural prose and the optional case-insensitive prefixes meeting:, task:, idea:, project:, and source:.
- Respect explicit separators first. Within a capture, split clearly unrelated entries only when they have distinct subjects or actions; do not fragment one coherent item merely because it has multiple paragraphs.
- Assign every entry a stable capture ID and content hash. Deduplicate against existing note frontmatter, output-note source metadata, manifests, and prior checkpoints; never process the same capture twice and never use title matching alone as identity.

For each successfully processed capture:
- Create a separate, human-readable note in the appropriate existing ACES folder, preserving the vault's established A Atlas, C Calendar, E Efforts, S Sources, and Z System organization and its current naming conventions.
- Use only controlled tags from the existing vault taxonomy; reuse canonical tags and avoid near-duplicates or speculative tags.
- Add canonical Benson Foo, TOPPAN Ecquaria, entity, project, and MOC wikilinks only when supported by the capture or verified existing relationships. Reuse existing canonical notes and links rather than creating isolated graph nodes for display.
- Preserve the exact original capture text while processing it. Store the stable capture ID and content hash in the destination note's own frontmatter, not in a running ledger inside Capture Here.md. Once every output note for an entry has been written successfully, remove that entry from New captures entirely; do not append it to a Processed captures section. If any write fails, leave the entry in New captures unprocessed and checkpoint the failure for retry. Before processing an entry, check for its capture ID/content hash across existing note frontmatter (not an inbox log) to avoid reprocessing.
- Keep processed entries deterministic and resumable. When identical text appears more than once, preserve occurrence identity so a genuinely separate repeated capture is neither lost nor accidentally reprocessed.

### Mandatory categorization and routing

- Tender-related items (RFI, RFQ, BQ, EOI, procurement, briefings, clarifications, submissions, pricing, evaluations, awards/losses, and meetings): category Tender; file under the corresponding tender/bid.
- Service Requests (SR): category Service Request; file under the corresponding project/deal.
- Change Requests (CR): category Change Request; file under the corresponding project/deal.
- Sales, campaign, account, or partnership activity not yet a formal tender (relationship-building, outreach, follow-up, partnership development): category Campaign / Account / Sales; file under the verified account, campaign, or sales record, do not force it into an unrelated tender or project.
- Other project-specific delivery, support, billing, issues, milestones, handover, acceptance, or closure: category Project Activity; file under the corresponding project/deal. Use verified local-vault or authorized HubSpot evidence to resolve the owner. Preserve identifiers, dates, parties, status, decisions, actions/outcomes, and financial milestones. Update the owning record, not only an audit log; do not create a duplicate freestanding email note. Link supporting source/calendar evidence.
- Every substantive item must be assigned to exactly one of: Tender, Service Request, Change Request, Project Activity, Campaign / Account / Sales, or Unresolved Routing. If ambiguous, mark Unresolved Routing, list candidate targets, and defer instead of guessing.
