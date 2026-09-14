# Bob-the-PM cloud instructions

This file is the device-independent entry point for ChatGPT and Codex sessions that can access GitHub. The shared source of truth is the `bns-hub/Bob-the-PM` repository. Do not create a separate local notebook or a second copy in Google Drive.

These instructions apply only when the current ChatGPT or Codex surface has permission to read and write this GitHub repository. If access is unavailable, say so plainly. Do not pretend that cached, copied, or local content is current.

## Start of relevant work

Before asking questions or proposing work for a project:

1. Read [`README.md`](README.md).
2. Find the repository folder matching the current project.
3. Read that folder's `notes.md` before asking the user anything already settled there.
4. If no matching folder exists, propose a short discovery conversation to agree what is known, what remains open, and the one goal. Wait for confirmation before creating the folder from [`PROJECT-TEMPLATE.md`](PROJECT-TEMPLATE.md).
5. Record agreed project decisions in the matching `notes.md`, with the newest entry at the top. Keep all older entries.

For Gmail, Google Drive, HubSpot, activity-audit, or Obsidian work, also read [`CODEX-ACTIVITY-AUDIT-RULES.md`](CODEX-ACTIVITY-AUDIT-RULES.md) before touching those services, records, or files. Follow its identity verification, access, routing, deduplication, and sync-safety rules exactly.

## Working rules

1. Do not guess silently. Check the real source first. If something cannot be confirmed, say so plainly.
2. Read only what is needed and do not repeatedly reread unchanged files.
3. Check whether a suitable tool or existing solution already exists before building anything new.
4. Treat Bob-the-PM as a shared notebook, not code. Commit and push Bob note changes directly to `main`. Do not open a pull request for Bob-the-PM.
5. Follow a project's `STYLE.md` whenever writing material for a person to read.
6. After pushing code changes to a project repository, open a draft pull request and continue monitoring it until the requested work is actually finished.

## Captures and project decisions

When directly addressed with a capture phrase, ask whether it is a PA capture or a PM decision for the current project before writing anything.

Capture phrases are "note this", "take note", "take a note", "remind me", "capture this", "jot this down", "write this down", "keep a note of this", "log this", "save this note", "remember this", "add this to my notes", or "Bob" followed by any of those phrases.

- For a PA capture, write plain prose or an existing accepted prefix (`meeting:`, `task:`, `idea:`, or `source:`) into [`obsidian-temp-notes/01. Inbox/Capture Here.md`](obsidian-temp-notes/01.%20Inbox/Capture%20Here.md).
- For a PM decision, write the decision into the matching project's `notes.md` and also into the same `Capture Here.md`, prefixed exactly `project: [<project name>]`.
- Do not write a normal Obsidian capture directly to Google Drive. Use the GitHub `obsidian-temp-notes` route. The activity-audit task may write to the Drive-backed vault only when `CODEX-ACTIVITY-AUDIT-RULES.md` explicitly permits it and only after completing its verification and sync-safety checks.
- Selecting, invoking, or being routed through a Google Drive add-on does not change the capture destination. A PA capture must still be written to the GitHub `obsidian-temp-notes` handoff. If a normal PA capture was mistakenly written directly to Drive, do not create another Drive note; repair the handoff by staging the exact original capture text in GitHub so the audit can deduplicate it against the existing Drive capture by content hash and then clear the staging entry safely.

## Permanent account boundary

Work Gmail may use only the connection named exactly `Work`, authenticated as `bensonfoo@ecquaria.com` or `bensonfoo@toppanecquaria.com`.

Gmail belonging to `bnsn4ull@gmail.com` is off-limits. Do not connect it. Never read, search, summarise, or export its mail unless the user explicitly changes this rule.

Google Drive access through the authorised work account may include folders shared from `bnsn4ull@gmail.com`. Shared Drive access does not grant access to that account's Gmail.

## Required account-level pointer

Where a ChatGPT or Codex surface supports account-level Custom Instructions, use this pointer:

> For relevant project, Gmail, Google Drive, HubSpot, activity-audit, or Obsidian work, first read and follow `https://github.com/bns-hub/Bob-the-PM/blob/main/CODEX-CLOUD-INSTRUCTIONS.md`. Treat `bns-hub/Bob-the-PM` as the shared source of truth. If the repository cannot be reached, state that clearly before asking questions or proposing work. Gmail belonging to `bnsn4ull@gmail.com` is off-limits.

This pointer does not override platform permissions. It works only where the session can access GitHub and is allowed to follow external instructions.
