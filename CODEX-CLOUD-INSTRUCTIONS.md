# Bob-the-PM cloud instructions

This file is the device-independent entry point for ChatGPT and Codex sessions that can access GitHub. The shared source of truth is the `bns-hub/Bob-the-PM` repository. Do not create a separate local notebook or a second copy in Google Drive.

`bns-hub/Bob-the-PM` is a shared notebook, not code: one folder per project, each holding a `notes.md` running log, newest entry at the top, nothing deleted. A few shared, project-independent files live loose in the main folder: this file, `README.md`, `CODEX-ACTIVITY-AUDIT-RULES.md`, `OBSIDIAN-DELIVERY-ARCHITECTURE.md`, `PROJECT-TEMPLATE.md`, and the `obsidian-temp-notes/` folder. Its purpose: whichever tool picks up a piece of work next, Claude or Codex, reads what was already decided instead of asking the same questions again.

These instructions apply only when the current ChatGPT or Codex surface has permission to read and write this GitHub repository. If access is unavailable, say so plainly. Do not pretend that cached, copied, or local content is current.

## Start of relevant work

Before asking questions or proposing work for a project:

1. Read [`README.md`](README.md).
2. Find the repository folder matching the current project.
3. Read that folder's `notes.md` before asking the user anything already settled there.
4. If no matching folder exists, propose a short discovery conversation to agree what is known, what remains open, and the one goal. Wait for confirmation before creating the folder from [`PROJECT-TEMPLATE.md`](PROJECT-TEMPLATE.md).
5. Record agreed project decisions in the matching `notes.md`, with the newest entry at the top. Keep all older entries.

For Gmail, Google Drive, HubSpot, activity-audit, or Obsidian work, also read [`CODEX-ACTIVITY-AUDIT-RULES.md`](CODEX-ACTIVITY-AUDIT-RULES.md) before touching those services, records, or files. Follow its identity verification, access, routing, deduplication, and sync-safety rules exactly.

For any Obsidian capture, export, sync, delivery, or vault-write question, also read [`OBSIDIAN-DELIVERY-ARCHITECTURE.md`](OBSIDIAN-DELIVERY-ARCHITECTURE.md). That file is authoritative for the device roles, final write surface, verification standard, and GitHub cleanup rules, and it overrides older conflicting wording elsewhere in this repository about cloud Drive writes counting as successful Obsidian delivery.

## Working rules

1. Do not guess silently. Check the real source first. If something cannot be confirmed, say so plainly.
2. Read only what is needed and do not repeatedly reread unchanged files.
3. Check whether a suitable tool or existing solution already exists before building anything new.
4. Treat Bob-the-PM as a shared notebook, not code. Commit and push Bob note changes directly to `main`. Do not open a pull request for Bob-the-PM.
5. Follow a project's `STYLE.md` whenever writing material for a person to read.
6. After pushing code changes to a project repository, open a draft pull request and continue monitoring it until the requested work is actually finished.

## Captures and project decisions

When directly addressed with a capture phrase, **never leave the note only in chat while waiting for classification**. First stage the exact capture text immediately in `obsidian-temp-notes/01. Inbox/Capture Here.md` under `## New captures`, prefixed `pending-classification:` unless the user already made PA/PM intent explicit. Commit that staging write to `main` before asking any follow-up. Then ask whether it is a PA capture or a PM decision for the current project. If the user answers PA, replace only the `pending-classification:` marker with the appropriate plain/accepted PA form. If the user answers PM, replace it with `project: [<project name>]` and also record the decision in that project's `notes.md`. If classification is never answered, keep the pending entry in GitHub so it cannot be lost; Obsidian Export must leave pending-classification entries staged rather than filing or deleting them.

Capture phrases are "note this", "take note", "take a note", "remind me", "capture this", "jot this down", "write this down", "keep a note of this", "log this", "save this note", "remember this", "add this to my notes", or "Bob" followed by any of those phrases.

- For a PA capture, write plain prose or an existing accepted prefix (`meeting:`, `task:`, `idea:`, or `source:`) into [`obsidian-temp-notes/01. Inbox/Capture Here.md`](obsidian-temp-notes/01.%20Inbox/Capture%20Here.md).
- For a PM decision, write the decision into the matching project's `notes.md` and also into the same `Capture Here.md`, prefixed exactly `project: [<project name>]`.
- Do not write a normal Obsidian capture directly to Google Drive. Use the GitHub `obsidian-temp-notes` route. Final delivery must follow `OBSIDIAN-DELIVERY-ARCHITECTURE.md`; a cloud-created Drive file alone is not proof that Obsidian received the note.
- Selecting, invoking, or being routed through a Google Drive add-on does not change the capture destination. A PA capture must still be written to the GitHub `obsidian-temp-notes` handoff. If a normal PA capture was mistakenly written directly to Drive, do not create another Drive note; repair the handoff by staging the exact original capture text in GitHub so the local-vault workflow can deduplicate it and complete delivery safely.

## Permanent account boundary

Work Gmail may use only the connection named exactly `Work`, authenticated as `bensonfoo@ecquaria.com` or `bensonfoo@toppanecquaria.com`.

Gmail belonging to `bnsn4ull@gmail.com` is off-limits. Do not connect it. Never read, search, summarise, or export its mail unless the user explicitly changes this rule.

Google Drive access through the authorised work account may include folders shared from `bnsn4ull@gmail.com`, including the Obsidian vault folder — the vault's owner shows as `bnsn4ull@gmail.com`, which is correct, it was shared into the work account on purpose. Shared Drive access does not grant access to that account's Gmail.

Work Gmail, Google Drive (as above), the local drive (Codex reaches this directly), and HubSpot (the authorised work portal for the same work account) are all already set up — do not re-ask about or re-set-up any of these. Full detail, including the identity-verification rules for the activity-audit task specifically, is in `CODEX-ACTIVITY-AUDIT-RULES.md`.

## Required account-level pointer

Where a ChatGPT or Codex surface supports account-level Custom Instructions, use this pointer:

> For relevant project, Gmail, Google Drive, HubSpot, activity-audit, or Obsidian work, first read and follow `https://github.com/bns-hub/Bob-the-PM/blob/main/CODEX-CLOUD-INSTRUCTIONS.md`. Treat `bns-hub/Bob-the-PM` as the shared source of truth. If the repository cannot be reached, state that clearly before asking questions or proposing work. Gmail belonging to `bnsn4ull@gmail.com` is off-limits.

This pointer does not override platform permissions. It works only where the session can access GitHub and is allowed to follow external instructions.
