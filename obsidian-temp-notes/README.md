# Obsidian temp notes

This is a holding area, not the real vault. Files here mirror the target path inside the user's actual Obsidian vault, named `Ben`, so it is obvious where each one belongs once it is copied across.

Target inside the vault, for reference:
`obsidian://open?vault=Ben&file=01.%20Inbox%2FCapture%20Here`

Google Drive was tried instead of this and dropped, direct-write file sharing did not work reliably. GitHub is the working mechanism, keep it that way unless something concrete changes.

## Convention

- Folder and file names under this directory match the vault's own path exactly, folder for folder, so `01. Inbox/Capture Here.md` here is meant for `01. Inbox/Capture Here` in the vault.
- Codex checks this folder on every run and is responsible for filing new content into the real vault, since it can reach the local device and Drive-backed vault directly, a cloud session like Claude's cannot.
- Once an entry has been filed successfully into the real vault, Codex deletes it from here and pushes the change, it does not stay as a record. If filing fails, the entry stays here for retry on the next run.
