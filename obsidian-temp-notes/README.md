# Obsidian temp notes

This is a holding area, not the real vault. Files here mirror the target path inside the user's actual Obsidian vault, named `Ben`, so it is obvious where each one belongs once it is copied across.

Target inside the vault, for reference:
`obsidian://open?vault=Ben&file=01.%20Inbox%2FCapture%20Here`

Google Drive was tried instead of this and dropped, direct-write file sharing did not work reliably. GitHub is the working mechanism for getting new content INTO the vault, keep it that way unless something concrete changes.

**Reading the vault, as of 2026-09-14, is a separate matter and is allowed.** The user has given a Claude cloud session standing permission to read the real Drive-backed vault directly, for context, via the Google Drive connector. This is read-only: no renaming, moving, editing, or deleting anything in the vault, and `.obsidian/` is never touched. Writing new content still only ever goes through this GitHub `obsidian-temp-notes` folder, never a direct Drive write. Vault root folder ID, at time of writing: `1hn2zMhML2HZLu5L77Vt422D3nnk0dDi-`. Drive lists the owner on these files as `bnsn4ull@gmail.com` — this is correct and expected, not a mismatch: the user owns the vault on their personal account and has shared it into the work account (bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com) specifically so Claude and Codex can read it there. Confirmed by the user 2026-09-14.

## Convention

- Folder and file names under this directory match the vault's own path exactly, folder for folder, so `01. Inbox/Capture Here.md` here is meant for `01. Inbox/Capture Here` in the vault.
- A desktop or laptop local Codex processor checks this folder and files routable content through the local Obsidian Model Context Protocol connector. It must hold the shared processor lock first.
- Once an entry has been written and re-read successfully through Obsidian Model Context Protocol, local Codex deletes only that matching entry here and pushes the change. If filing or verification fails, the entry stays here for retry.

## Lossless capture rule

Every Bob capture is written here immediately before any PA/PM classification follow-up. If classification is not yet known, prefix the exact text with `pending-classification:`. Pending entries stay in GitHub and must not be filed or deleted by Obsidian Export until they are classified. This makes GitHub the durable handoff queue instead of relying on chat history.

Every new entry also carries the stable version 1 metadata comment defined in [`../OBSIDIAN-INGESTION-CONTRACT.md`](../OBSIDIAN-INGESTION-CONTRACT.md). Older plain entries are kept valid and receive metadata before a local processor claims them.

## Two kinds of entries land here

- **PA captures**, meeting notes, tasks, ideas, anything, plain prose or an existing accepted prefix (`meeting:`, `task:`, `idea:`, `source:`), whichever fits. Filed using Codex's normal categorisation rules.
- **PM decisions**, project scope or phase outcomes that also live in that project's own `notes.md` elsewhere in this repository. These carry a `project: [<project name>]` prefix here, so Codex files them under that project rather than Unresolved Routing.
