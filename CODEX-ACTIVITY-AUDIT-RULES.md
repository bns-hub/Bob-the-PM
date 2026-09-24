# Codex activity-audit rules (permanent)

This is a standing operating spec for the Codex/ChatGPT cloud activity-audit task. On the Codex/ChatGPT side this recurring job is named **"Obsidian Export"** — that is the schedule this file governs. It runs entirely in ChatGPT cloud, not here. The one part that involves a Claude cloud session is called out explicitly below: Claude hands off notes only through the GitHub `obsidian-temp-notes` folder in this repository, and never writes to Google Drive or Gmail directly.

**"Obsidian Export" must run daily as a cloud queue coordinator.** Every run reads Bob-the-PM's GitHub inbox (`obsidian-temp-notes/01. Inbox/Capture Here.md`), `daily-agenda/notes.md`, per-project `notes.md`, and local-writer status. It may stage, classify, and report. It must not write Bob captures into Google Drive or claim that a capture reached Obsidian. Final capture processing belongs to the one locked desktop or laptop local Codex processor through Obsidian Model Context Protocol, as defined in `OBSIDIAN-DELIVERY-ARCHITECTURE.md` and `OBSIDIAN-INGESTION-CONTRACT.md`.

The original operating rules are retained below with the Bob capture sections updated to match the authoritative local ingestion contract.

---

Permanent account-boundary rule: Before any source read or Drive write, verify connector identities independently:
Google Drive access is via the work account, bensonfoo@ecquaria.com, connected intentionally, with shared access into bnsn4ull@gmail.com's personal Drive space. This Codex task itself continues to read from and write to that Drive space as specified below, the vault root and the checkpoint folder, that is unchanged and expected. What is off-limits is a separate Claude cloud session writing to Google Drive directly, that has proven unreliable there, file-level sharing consistently failed, so Claude hands off notes through the GitHub folder described below instead, never through a direct Drive write. bnsn4ull@gmail.com itself has no connection here at all, only its Drive folder is shared in.

Gmail work activity must use only the connection named exactly "Work", authenticated exactly as bensonfoo@ecquaria.com or its alternative login bensonfoo@toppanecquaria.com, both are the same person, Benson Foo.

HubSpot activity must use only the authorized work portal for bensonfoo@ecquaria.com or bensonfoo@toppanecquaria.com.

Never read, search, summarize, or export Gmail belonging to bnsn4ull@gmail.com, that account has no Gmail connection here at all, personal or otherwise. If a write target cannot be confirmed as inside the shared personal Drive space, pause the write and report which folder failed to resolve. If the Gmail connection name or identity mismatches, abort Gmail processing before reading any message content and report a security issue.

Run the cloud activity-audit collection in ChatGPT cloud. Bob capture delivery is a separate local ingestion step and must remain queued until local Codex completes and verifies it through Obsidian Model Context Protocol.

Incrementally export only new or changed authorized work activity from:
- Gmail connection whose nickname is exactly "Work" and whose authenticated account is exactly bensonfoo@ecquaria.com or bensonfoo@toppanecquaria.com;
- connected HubSpot, resolving current deal owners to human-readable names and treating `deal_owner_name: "Benson Foo"` as the primary user-facing ownership value for Benson's deal dashboards. Retain owner ID `86653749` only as reconciliation/audit metadata and an identity-check value, not as the dashboard inclusion key;
- authorized ChatGPT/Codex activity available to this cloud task; and
- Claude handoffs separately supplied in the existing Google Drive structure.

Additional sources: Bob-the-PM staged and running notes. On every run, check the GitHub repository bns-hub/Bob-the-PM. Always inspect the folder obsidian-temp-notes, always inspect daily-agenda/notes.md when present, and enumerate every project folder's notes.md rather than relying only on previously known paths. Its structure mirrors the real vault's own folder paths, so obsidian-temp-notes/01. Inbox/Capture Here.md corresponds to 01. Inbox/Capture Here.md in the vault. It may contain zero, one, or several files, process every file present, not just the first, in a deterministic order, oldest created first, so a run that stops partway resumes predictably next time.

For each staged temp file, validate the version 1 metadata and leave final routing, deduplication, and writing to the locked local Codex processor. For `daily-agenda/notes.md` and project `notes.md`, treat each new or changed log entry as immutable history identified by GitHub path, source commit or blob identity, and content hash. Never delete or rewrite daily-agenda or project history merely because it was exported.

Entries prefixed exactly `pending-classification:` are durable but not yet routable. Do not file them into the vault and do not remove them from GitHub. Preserve them for a later run after Bob replaces the pending marker with PA content or `project: [<project name>]`. Report the pending count only when it is newly blocking a requested capture or when the user explicitly asks.

The locked local Codex processor removes a transient entry only after its Obsidian Model Context Protocol write and verification read succeed. The cloud task does not clear the entry. If filing fails, the entry stays in place for the next local run.

This GitHub queue is the one safe place a separate Claude session may write for note handoff. Claude must never write directly to Google Drive or the Obsidian vault.

## Google-Drive/Obsidian sync-safety rule

Treat the Drive-backed vault as a live synchronized filesystem. Never modify anything under `.obsidian/` and never rewrite, rename, move, or delete existing vault files merely for tidying, formatting, or presentation. For Bob captures, Drive is read-only context and optional later sync confirmation. The cloud task must not create or edit the destination note. The local Codex processor must search, write, and re-read through Obsidian Model Context Protocol, make the smallest content-preserving change, and preserve user-authored content and frontmatter. If verification fails, retain the GitHub staging entry for retry.

Use Google Drive folder ID 1hn2zMhML2HZLu5L77Vt422D3nnk0dDi- only as the read-only synced view of the local `Ben` vault. At the start of each cloud run, verify that the connected work account can resolve and read this folder. Never create, edit, rename, move, or delete live-vault files through Google Drive. Preserve its existing A Atlas, C Calendar, E Efforts, S Sources, and Z System tree exactly. All final live-vault amendments belong to the locked local Codex writer through authenticated Obsidian Model Context Protocol.

Continue writing a compact manifest and successful checkpoint copy to Google Drive folder ID 1T2X6ngN73ZgOOCqGVXoG0XIdybJAo93h, preserving the existing checkpoint lineage. Do not use the handoff folder as the primary destination for final-form notes when the vault root is writable. If the vault root cannot be resolved or is not writable, keep the recurring task active, do not create a replacement tree elsewhere, write only the compact failure/checkpoint state to the handoff folder when permitted, and notify an actionable file-sync/access failure stating that the remaining bridge cannot yet be removed.

Process only deltas since the last successful checkpoint, using a seven-day overlap where appropriate. Compare saved source cursors, stable provider IDs, modified timestamps, Drive file IDs, counts, and content hashes before fetching bodies or attachments. Hydrate only new, changed, previously failed, or unresolved records and attachments. Deduplicate by stable source ID plus hash and preserve retry state for failed or unresolved items.

Once per seven-day cycle, apply the weekly ownership and link pass in `OBSIDIAN-GRAPH-HYGIENE.md` to all newly created or materially changed notes. This includes Excalidraw drawings, Canvas files, and attachment companion notes. Cloud analysis may propose deterministic repairs, but only the locked local writer may apply them. Each applied relationship must use the most specific verified canonical owner, preserve the original content and identifiers, and pass a link-resolution and affected-cluster broken-link check.

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

On each cloud run, inspect the GitHub staging note at `obsidian-temp-notes/01. Inbox/Capture Here.md`. On each local run, the locked local Codex processor reads that same queue and processes only routable content in `## New captures`. If the marker is missing or the section boundaries are ambiguous, do not guess or consume content. Record an actionable failure.

Parse captures as follows:
- A line containing only --- is an explicit entry separator.
- Also accept natural prose and the optional case-insensitive prefixes meeting:, task:, idea:, project:, and source:.
- Respect explicit separators first. Within a capture, split clearly unrelated entries only when they have distinct subjects or actions; do not fragment one coherent item merely because it has multiple paragraphs.
- Assign every entry a stable capture ID and content hash. Deduplicate against existing note frontmatter, output-note source metadata, manifests, and prior checkpoints; never process the same capture twice and never use title matching alone as identity.

For each successfully processed capture, local Codex must:
- Prefer updating the verified existing owning note. Create a separate, human-readable note in the appropriate existing ACES folder only when no supported owner exists. Preserve the vault's established A Atlas, C Calendar, E Efforts, S Sources, and Z System organisation and its current naming conventions.
- Use only controlled tags from the existing vault taxonomy; reuse canonical tags and avoid near-duplicates or speculative tags.
- Add canonical Benson Foo, TOPPAN Ecquaria, entity, project, and MOC wikilinks only when supported by the capture or verified existing relationships. Reuse existing canonical notes and links rather than creating isolated graph nodes for display.
- Preserve the exact original capture text while processing it. Store the stable capture ID and content hash in the destination note's own frontmatter, not in a running ledger inside Capture Here.md. Once every output note for an entry has been written and re-read successfully through Obsidian Model Context Protocol, remove only that matching entry from `## New captures`. Do not append it to a Processed captures section. If any write or verification fails, leave the entry unprocessed for retry. Before processing an entry, check for its capture ID and content hash across existing note frontmatter, not an inbox log, to avoid reprocessing.
- Keep processed entries deterministic and resumable. When identical text appears more than once, preserve occurrence identity so a genuinely separate repeated capture is neither lost nor accidentally reprocessed.

### Mandatory categorization and routing

- Tender-related items (RFI, RFQ, BQ, EOI, procurement, briefings, clarifications, submissions, pricing, evaluations, awards/losses, and meetings): category Tender; file under the corresponding tender/bid.
- Service Requests (SR): category Service Request; file under the corresponding project/deal.
- Change Requests (CR): category Change Request; file under the corresponding project/deal.
- Sales, campaign, account, or partnership activity not yet a formal tender (relationship-building, outreach, follow-up, partnership development): category Campaign / Account / Sales; file under the verified account, campaign, or sales record, do not force it into an unrelated tender or project.
- Other project-specific delivery, support, billing, issues, milestones, handover, acceptance, or closure: category Project Activity; file under the corresponding project/deal. Use verified local-vault or authorized HubSpot evidence to resolve the owner. Preserve identifiers, dates, parties, status, decisions, actions/outcomes, and financial milestones. Update the owning record, not only an audit log; do not create a duplicate freestanding email note. Link supporting source/calendar evidence.
- Every substantive item must be assigned to exactly one of: Tender, Service Request, Change Request, Project Activity, Campaign / Account / Sales, or Unresolved Routing. If ambiguous, mark Unresolved Routing, list candidate targets, and defer instead of guessing.
## Human-facing working-note presentation — 2026-09-23

Cloud collection order and local queue processing order are implementation details. They must not leak into the visible ordering of canonical project/deal/campaign/effort notes.

For any newly created or materially changed human-facing working note, use `OBSIDIAN-NOTE-UX-STANDARD.md` as the presentation rule:
- compact `At a glance` when useful;
- `Next actions` for actionable checkboxes;
- `Working notes` as a stable user-owned scratch area;
- `Latest updates` newest-first;
- `Decisions` newest-first when applicable;
- meeting/reference links instead of duplicated source dumps;
- no visible `Inbox Activity` or processed-capture ledger solely for audit purposes.

Keep capture IDs, hashes, provider IDs, source identities, and verification state in YAML/frontmatter, manifests, claims, checkpoints, or source/audit records. Preserve source evidence and chronology in source/audit notes; optimize canonical working notes for fast reading and note taking.

