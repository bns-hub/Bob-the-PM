# Obsidian delivery architecture

This file is the authoritative device-and-delivery model for Bob-the-PM captures and Obsidian. It overrides any older or conflicting wording elsewhere in this repository about where a capture is finally written, what counts as successful Obsidian delivery, or when a GitHub handoff may be cleared.

## Canonical flow

You -> Bob capture -> GitHub source queue -> local vault writer -> local `Ben` vault filesystem -> Obsidian indexes the file -> Google Drive syncs the same local file -> cloud verification -> only then clear the transient GitHub capture.

GitHub source queues include:
- `obsidian-temp-notes/01. Inbox/Capture Here.md` and other staged files under `obsidian-temp-notes/`;
- `daily-agenda/notes.md` when present; and
- every project folder's `notes.md`.

`daily-agenda/notes.md` and project `notes.md` are immutable history. Exporting an entry never deletes or rewrites that source history. Only transient handoff entries such as successfully delivered entries under `Capture Here.md` may be cleared.

## Authoritative write surface

The authoritative destination for new Obsidian notes is the actual local filesystem of the Obsidian vault named `Ben`.

A new `.md` file created only through the cloud Google Drive connector is not proof of Obsidian delivery, even if its parent folder is inside the Drive view of the vault. A cloud-created file can be owned by the work Google account and fail to materialize in the personal-account local filesystem that Obsidian is watching.

Therefore:
- cloud ChatGPT/Codex/Claude must not claim a new capture reached Obsidian merely because a Drive file exists;
- for a new vault note, the final write should be made by a local writer with direct filesystem access to the actual `Ben` vault;
- local verification must re-open the exact written `.md` and confirm the expected stable capture/source ID, content hash, and intended content;
- Google Drive then synchronizes that same local file to the cloud;
- the cloud `Obsidian Export` task may use the Drive view for context, deduplication, and post-sync verification, but not as the authoritative new-note writer;
- the transient GitHub handoff remains authoritative until delivery is verified.

## Device roles

### PC / laptop

PC/laptop local Codex is the preferred authoritative writer.

Before writing, local Codex must:
1. resolve the actual local path of the `Ben` vault;
2. verify the expected vault structure is present and the target is writable;
3. never touch `.obsidian/`, lock/cache/workspace/plugin-state files, or hidden Obsidian application state;
4. read the current Bob instructions and GitHub source queue;
5. deduplicate by stable capture/source ID and content hash;
6. write or minimally patch the appropriate `.md` inside the local vault;
7. re-read and verify the local file;
8. allow the normal sync layer to propagate it.

### Android

Android Obsidian is a consumer of a local vault and may also be a writer only when an approved on-device automation has direct filesystem access to the same local `Ben` vault and can perform the same verification rules as PC/laptop local Codex.

Do not assume the ChatGPT mobile cloud session itself has arbitrary local filesystem write access. If no approved local writer exists on Android, leave the GitHub handoff queued and let the PC/laptop local writer process it.

### iPhone / iPad

Treat iPhone/iPad as a synced vault consumer unless an explicitly approved local filesystem automation exists that can satisfy the same local-write and verification rules.

Do not assume a ChatGPT mobile cloud session can write directly into the iOS Obsidian vault. With the current Google Drive-backed architecture, PC/laptop remains the preferred authoritative writer.

## Cloud `Obsidian Export` role

`Obsidian Export` runs daily and orchestrates the queue. Every run must inspect:
- all staged content under `obsidian-temp-notes/`, including `01. Inbox/Capture Here.md`;
- `daily-agenda/notes.md` when present; and
- every project folder's `notes.md`.

The cloud task may classify, route, enrich, deduplicate, assign stable IDs/hashes, compare against existing vault/checkpoint state, and verify a synced result. It must not create a replacement work-owned `.md` inside the live `Ben` vault just to complete a capture.

If a new local note is required and no verified local result is visible yet, status is `awaiting local vault write`. That is not success and not failure.

## Verification and cleanup

A transient GitHub capture may be removed only after all of these are true:
1. the intended destination note exists as part of the synced `Ben` vault workflow;
2. the destination contains the expected stable capture/source ID and content hash;
3. the content matches the intended capture/export;
4. the result is not merely a cloud-created work-owned Drive file masquerading as a local-vault result;
5. the current GitHub staging file is re-read immediately before removal so concurrent edits are preserved.

If any condition fails, leave the GitHub handoff intact.

For `daily-agenda/notes.md` and project `notes.md`, never delete source entries after export. Record their exported source identity in destination/checkpoint metadata instead.

## Sync responsibilities

Obsidian itself indexes files already present in its local vault. The sync layer is responsible for moving those local files between devices/cloud storage.

Under the current architecture:
- local writer creates/updates the `.md` in `Ben`;
- Obsidian notices/indexes the local file;
- Google Drive for desktop or the approved device sync mechanism uploads/synchronizes it;
- cloud verification observes the synchronized copy afterward.

Do not invert this into `cloud Drive API creates file -> assume Obsidian received it`.
