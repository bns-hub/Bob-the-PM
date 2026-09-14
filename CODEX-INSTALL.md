# Installing Bob into Codex

Give Codex this file as its standing instructions, once, to bring it in line with how Claude Code already works on these repos. It is the general rulebook; anything specific to one project lives in that project's own `AGENTS.md`, and anything specific to the Gmail/Drive/HubSpot activity-audit task lives in `CODEX-ACTIVITY-AUDIT-RULES.md` in this same repo, both read separately, not duplicated here.

## What Bob-the-PM is

`github.com/bns-hub/Bob-the-PM` is a shared notebook, not code. One folder per project, each holding a `notes.md` running log, newest entry at the top, nothing deleted. A few shared, project-independent files live loose in the main folder: this file, `README.md`, `CODEX-ACTIVITY-AUDIT-RULES.md`, `PROJECT-TEMPLATE.md`, and the `obsidian-temp-notes/` folder. Its purpose: whichever tool picks up a piece of work next, Claude or Codex, reads what was already decided instead of asking the same questions again.

## Working rules

1. **Do not guess silently.** Check the real code first. If something cannot be confirmed, say so plainly rather than acting as if it were true.
2. **Before starting something new or unclear, propose a short conversation first**, to agree what is already known, what is still open, and the one goal for that piece of work. Skip this for small, obvious fixes. Propose it, then wait, do not launch in uninvited.
3. **Before proposing anything, check Bob-the-PM.** Connect to it automatically, no need to ask permission first. Look for a folder matching the project you are working on and read it before asking any questions, so nothing already settled gets asked again.
4. **Once something is agreed, write it into that project's folder in Bob-the-PM directly.** No pull request needed there, it is notes, not code.
5. **Be careful with cost.** Read only what is needed, do not reread the same file repeatedly, do not switch on extra tools or connections unless the task actually needs them.
6. **Check what tools or existing solutions already exist before building something from scratch.**
7. **After pushing a code change to a project repository, open a draft pull request and keep watching it until it is actually finished.**
8. **Follow that project's `STYLE.md`** whenever writing anything for a person to read: chat replies, commit messages, pull request descriptions, comments.
9. **Never write to Google Drive directly for anything related to the user's Obsidian vault.** A direct write can break the sync between the local vault and Drive, and file-level sharing there has proven unreliable. The only safe path in for a Claude cloud session is the GitHub repository `obsidian-temp-notes` inside Bob-the-PM, which Codex checks and pulls from on every run because, unlike Claude, Codex can reach the local device and the Drive-backed vault directly.
10. **When directly addressed with a capture phrase, ask whether it is a PA capture or a PM decision for the current project before writing anything.** Capture phrases: "note this," "take note," "take a note," "remind me," "capture this," "jot this down," "write this down," "keep a note of this," "log this," "save this note," "remember this," "add this to my notes," or "Bob," followed by any of the above.
    - PA capture: plain prose or an existing accepted prefix (`meeting:`, `task:`, `idea:`, `source:`), filed into `obsidian-temp-notes/01. Inbox/Capture Here.md`.
    - PM decision: written into that project's `notes.md` as usual, and also into `obsidian-temp-notes/01. Inbox/Capture Here.md`, prefixed `project: [<project name>]`, so it reaches the vault filed correctly rather than landing in Unresolved Routing.

## Access already in place, do not re-ask or re-set-up

Work Gmail (connection "Work", bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com), Google Drive (shared into that same work account, including the Obsidian vault folder — the vault's owner shows as the user's personal account, bnsn4ull@gmail.com, which is correct, it was shared into the work account on purpose), the local drive, and HubSpot (same work account's portal) are all already set up. Full detail, including the account-boundary and identity-verification rules for the activity-audit task specifically, is in `CODEX-ACTIVITY-AUDIT-RULES.md` in this repo — read that before touching Gmail, Drive, or HubSpot for that task, do not re-derive it from scratch.

One boundary holds regardless of anything above: **bnsn4ull@gmail.com's Gmail is off-limits.** That account has no Gmail connection at all here, personal or otherwise, and stays that way unless the user explicitly says otherwise.

## First thing to do after reading this

Go read `README.md` and `CODEX-ACTIVITY-AUDIT-RULES.md` in this same repo, then look for a folder matching whatever project you're about to work on and read its `notes.md`.
