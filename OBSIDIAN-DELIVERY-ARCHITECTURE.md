# Obsidian delivery architecture

This file is the authoritative device-and-delivery model for Bob-the-PM captures and Obsidian. It overrides any older or conflicting wording elsewhere in this repository about where a capture is finally written, what counts as successful Obsidian delivery, or when a GitHub handoff may be cleared.

## Canonical flow

You -> Bob capture -> GitHub source queue -> approved local writer -> local `Ben` vault filesystem -> Obsidian indexes the file -> Google Drive syncs the same local file -> cloud verification -> only then clear the transient GitHub capture.

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
- for a new vault note, the final write must be made by an approved local writer with direct filesystem access to the actual `Ben` vault;
- approved local writers are local Codex and local Claude Code sessions running on a device that can directly access that device's `Ben` vault filesystem;
- local verification must re-open the exact written `.md` and confirm the expected stable capture/source ID, content hash, and intended content;
- Google Drive then synchronizes that same local file to the cloud;
- the cloud `Obsidian Export` task may use the Drive view for context, deduplication, and post-sync verification, but not as the authoritative new-note writer;
- the transient GitHub handoff remains authoritative until delivery is verified.

## Approved local writers

### Local Codex

Local Codex running on a PC/laptop/desktop with filesystem access to `Ben` is the preferred primary writer.

### Local Claude Code

Claude Code running locally on a PC/laptop/desktop with filesystem access to `Ben` is an approved fallback writer and follows exactly the same claim, write, verify, and reporting rules as local Codex.

A Claude cloud session is not a local writer. A ChatGPT/Codex cloud task is not a local writer. The distinction is direct access to the device filesystem containing the real local `Ben` vault.

### Writer claim / race-prevention rule

Local Codex and local Claude Code may both be available, but they must not independently process the same queued item.

Before a local writer writes a routable capture/source item, it must create or update a deterministic GitHub claim keyed by the item's stable capture/source ID plus content hash. The claim must identify the writer/device and start time. Another writer that sees a current valid claim must skip that item. A stale claim may be recovered only according to the shared Bob rules, with the source re-read before retry.

After a successful local write and local verification, the writer records completion for that capture/source ID and hash and releases/completes the claim. The transient GitHub source itself is still not cleared until the synced-vault verification rule below passes.

## Device roles

### PC / laptop / desktop

Local Codex is the preferred primary writer; local Claude Code is an approved fallback writer.

Before writing, either local writer must:
1. resolve the actual local path of the `Ben` vault on that device;
2. verify the expected vault structure is present and the target is writable;
3. never touch `.obsidian/`, lock/cache/workspace/plugin-state files, or hidden Obsidian application state;
4. read the current Bob instructions and GitHub source queue;
5. obtain the deterministic GitHub claim for the capture/source item;
6. deduplicate by stable capture/source ID and content hash;
7. write or minimally patch the appropriate `.md` inside the local vault;
8. re-read and verify the local file contains the expected ID/hash and content;
9. record writer completion/heartbeat status in the shared GitHub writer-status area when available;
10. allow the normal sync layer to propagate the file.

### Android

Android Obsidian is a consumer of a local vault and may also be a writer only when an explicitly approved on-device local process has direct filesystem access to the same local `Ben` vault and can perform the same claim/write/verification rules.

A local Codex or Claude Code environment on Android counts as a writer only if it genuinely has filesystem permission to the local `Ben` vault and can satisfy these rules. Do not assume the ChatGPT or Claude mobile cloud session itself has arbitrary local filesystem write access.

If no approved local writer exists on Android, leave the GitHub handoff queued and let an available PC/laptop/desktop local writer process it.

### iPhone / iPad

Treat iPhone/iPad as a synced vault consumer unless an explicitly approved local filesystem process exists that can satisfy the same claim/write/verification rules.

Do not assume a ChatGPT or Claude mobile cloud session can write directly into the iOS Obsidian vault. With the current Google Drive-backed architecture, PC/laptop/desktop remains the preferred authoritative writer.

## Local writer discovery / status

Approved local writers should publish/update a small status or heartbeat file under `obsidian-local-writers/` in this repository so cloud orchestration can distinguish an available writer from an unavailable one.

A writer status should include at least:
- `device_id`
- `device_type`
- `writer` (`codex` or `claude-code`)
- `writer_enabled`
- `last_seen`
- `vault_validation_status`
- `resolved_local_vault_path` or a privacy-safe path fingerprint
- `last_successful_capture_id`
- `last_successful_content_hash`
- `last_error`
- `sync_status` when known

The cloud task cannot discover local paths by itself. It can only observe these status records and later verify the synchronized vault result.

## Cloud `Obsidian Export` role

`Obsidian Export` runs daily and orchestrates the queue. Every run must inspect:
- all staged content under `obsidian-temp-notes/`, including `01. Inbox/Capture Here.md`;
- `daily-agenda/notes.md` when present;
- every project folder's `notes.md`; and
- `obsidian-local-writers/` status/heartbeat records when present.

The cloud task may classify, route, enrich, deduplicate, assign stable IDs/hashes, compare against existing vault/checkpoint state, discover reported local-writer availability, and verify a synced result. It must not create a replacement work-owned `.md` inside the live `Ben` vault just to complete a capture.

If a new local note is required and no valid local writer is available, status is `awaiting local writer`.

If a writer exists but cannot find or validate its local `Ben` vault, status is `local vault not found`.

If local write and local verification succeeded but the synchronized destination is not yet visible/verified to cloud, status is `awaiting sync verification`.

None of those states count as successful Obsidian delivery.

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
- an approved local Codex or local Claude Code writer creates/updates the `.md` in `Ben`;
- the writer re-opens and verifies the local file;
- Obsidian notices/indexes the local file;
- Google Drive for desktop or the approved device sync mechanism uploads/synchronizes it;
- cloud verification observes the synchronized copy afterward;
- only then may a transient GitHub capture be cleared.

Do not invert this into `cloud Drive API creates file -> assume Obsidian received it`.
