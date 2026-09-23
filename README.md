# Bob-the-PM

This is a shared notebook for AI coding helpers, Claude and Codex, working across several projects. It is not code, it is notes.

## How it is organised, in plain terms

- Each project gets its own folder, named after the project, for example `project1/`.
- Inside a project folder there is one file, `notes.md`, that holds a running log for that project. Newest entry goes at the top, older entries stay below it, nothing gets deleted.
- Only shared, project-independent files live loose in this main folder, including the Obsidian delivery and ingestion contracts, and `PROJECT-TEMPLATE.md`, which is the blank template a helper copies when starting notes for a brand new project.

## Why this exists

Before an AI helper starts real work, or starts a new phase or sprint of work, it has a short conversation with you to pin down what is actually known versus assumed, and what the one goal for that phase is. The outcome of that conversation gets written here, so the next session, whichever tool runs it, can read what was already decided instead of asking the same questions again.

## Bob's existing access

This is settled, already in place, not something a new session needs to ask about or set up again: work Gmail (connection "Work", bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com), Google Drive (via the work account, shared into the user's private Drive folder including the Obsidian vault), local drive (Codex only, reaches the device directly), and HubSpot (the authorized work portal for the same work account).

Full rules, including the account-boundary and identity-verification detail, are recorded in `CODEX-ACTIVITY-AUDIT-RULES.md` in this folder — read that before touching Gmail, Drive, or HubSpot, do not re-derive these rules from scratch. One boundary in there stays in place regardless of anything above: bnsn4ull@gmail.com, the user's personal Gmail, has no Gmail connection here and is off-limits; only its Drive folder is shared in.

## Claude handoff

Claude must read [`CLAUDE-HANDOFF.md`](CLAUDE-HANDOFF.md) before handling Benson's Bob/Obsidian captures, note refinement, routing, or handoff work.

The handoff summarises the current user-approved capture intelligence, preview/approval gate, note UX, person/company linking model, and full-vault normalization status. Canonical safety and routing rules remain in the referenced contract files.
