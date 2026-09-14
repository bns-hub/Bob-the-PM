# Obsidian temp notes

This is a holding area, not the real vault. Files here mirror the target path inside the user's actual Obsidian vault, named `Ben`, so it is obvious where each one belongs once it is copied across.

Target inside the vault, for reference:
`obsidian://open?vault=Ben&file=01.%20Inbox%2FCapture%20Here`

Google Drive was tried instead of this and dropped, direct-write file sharing did not work reliably. GitHub is the working mechanism for getting new content INTO the vault, keep it that way unless something concrete changes.

**Reading the vault, as of 2026-09-14, is a separate matter and is allowed.** The user has given a Claude cloud session standing permission to read the real Drive-backed vault directly, for context, via the Google Drive connector. This is read-only: no renaming, moving, editing, or deleting anything in the vault, and `.obsidian/` is never touched. Writing new content still only ever goes through this GitHub `obsidian-temp-notes` folder, never a direct Drive write. Vault root folder ID, at time of writing: `1hn2zMhML2HZLu5L77Vt422D3nnk0dDi-`. Drive lists the owner on these files as `bnsn4ull@gmail.com` — this is correct and expected, not a mismatch: the user owns the vault on their personal account and has shared it into the work account (bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com) specifically so Claude and Codex can read it there. Confirmed by the user 2026-09-14.

## Convention

- Folder and file names under this directory match the vault's own path exactly, folder for folder, so `01. Inbox/Capture Here.md` here is meant for `01. Inbox/Capture Here` in the vault.
- Codex checks this folder on every run and is responsible for filing new content into the real vault, since it can reach the local device and Drive-backed vault directly, a cloud session like Claude's cannot.
- Once an entry has been filed successfully into the real vault, Codex deletes it from here and pushes the change, it does not stay as a record. If filing fails, the entry stays here for retry on the next run.

## Two kinds of entries land here

- **PA captures**, meeting notes, tasks, ideas, anything, plain prose or an existing accepted prefix (`meeting:`, `task:`, `idea:`, `source:`), whichever fits. Filed using Codex's normal categorisation rules.
- **PM decisions**, project scope or phase outcomes that also live in that project's own `notes.md` elsewhere in this repository. These carry a `project: [<project name>]` prefix here, so Codex files them under that project rather than Unresolved Routing.
