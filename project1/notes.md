# Project1 notes

---

## 2026-09-14 Local writer setup guide delivered; access key rotation required before use

**What we already know, confirmed, not guessed:**
- A setup guide, `Obsidian-Local-Writer-and-Sweep-Setup.md`, was produced covering the local-writer role named in `OBSIDIAN-DELIVERY-ARCHITECTURE.md`: `Bob/Claude capture -> GitHub queue -> one local Codex writer -> Obsidian connection -> local Ben vault -> Google Drive sync`. It specifies one active writer at a time (desktop preferred, laptop as fallback) coordinated by a shared GitHub lock, and gives the exact per-computer setup steps (Obsidian + MCP connector plug-in, Node.js check, `OBSIDIAN_MCP_TOKEN` Windows user variable, Codex Desktop endpoint config, authenticated vault-identity check, heartbeat registration under `obsidian-local-writers/`).
- On the laptop specifically: Node.js `v24.19.0` confirmed installed, Codex's Obsidian connection to `http://127.0.0.1:27200/mcp` authenticated and identified the server as `Obsidian - Ben`, and the new Excalidraw drawing `Drawing NEA AMS3, 2026-09-14 17.11.28.excalidraw.md` was linked to the verified AMS3 HubSpot deal owner (link confirmed working).
- **Security issue, action required:** the Obsidian connector access key was pasted directly into a chat during setup. The key must be rotated in the Obsidian connector, the private Windows `OBSIDIAN_MCP_TOKEN` variable updated on each local-writer computer, then Obsidian and Codex fully restarted, before the local-writer flow is trusted for real writes. This is a task only doable on the actual Windows machines, not from this cloud session.
- Claude's role in this flow stays capture/project-context only: never connect to, amend, or take the processor lock on the local `Ben` vault or its Drive-synced copy.

**What we are choosing to leave open, or unsure of, for now:**
- Whether the desktop has completed the same setup and rotation, or only the laptop has been verified so far.
- One older missing AMS3 evidence link was found during setup and logged as an unresolved repair, not yet fixed.

**The one goal for this phase:**
- Same as the prior entry below: get a real local writer running end-to-end (now largely specified and partly verified on the laptop), with the access-key rotation completed first.

**Anything the next session should NOT re-ask, because it is already settled:**
- Do not re-ask whether Claude needs the Obsidian connector — it does not; local Codex is the only final vault writer.
- Do not treat the pre-rotation key as still valid; assume it must be rotated before relying on the local writer for real deliveries.

---

## 2026-09-14 Obsidian delivery architecture corrected; local writer still missing

**What we already know, confirmed, not guessed:**
- The Roy Li/WRMS capture failure (marked "filed" by Codex, but not actually found in the vault on direct search) was caused by a real architecture problem, not a one-off bug: a cloud write into the shared Google Drive vault folder is owned by the work account and does not reliably reach the personal-account local filesystem mirror that Obsidian actually watches.
- The corrected, now-permanent flow is recorded in `OBSIDIAN-DELIVERY-ARCHITECTURE.md` in the main Bob-the-PM folder: Bob capture -> GitHub queue -> a local writer with real filesystem access to the `Ben` vault -> Obsidian indexes it -> Google Drive for Desktop syncs that local file up -> cloud `Obsidian Export` verifies the synced copy -> only then the transient GitHub entry is cleared. A cloud-created Drive file alone no longer counts as delivery.
- `CODEX-CLOUD-INSTRUCTIONS.md` now points to this architecture file, and the daily `Obsidian Export` task reads it on every run. `daily-agenda/notes.md` and project `notes.md` are explicitly permanent history, never deleted after export.

**What we are choosing to leave open, or unsure of, for now:**
- The one piece this doesn't yet solve: **there is no local writer set up.** The architecture names "PC/laptop local Codex" as the preferred authoritative writer, but that's a local install/schedule/trigger the user still needs to set up on their own machine with access to the real `Ben` vault folder. Until it exists, captures will queue correctly in GitHub but sit at "awaiting local vault write" rather than actually reaching Obsidian.

**The one goal for this phase:**
- Get a real local writer running on the PC/laptop so captures actually complete the chain end to end, not just queue.

**Anything the next session should NOT re-ask, because it is already settled:**
- Do not re-debate the architecture; `OBSIDIAN-DELIVERY-ARCHITECTURE.md` is authoritative and overrides older wording elsewhere about cloud Drive writes counting as delivery.
- Do not treat a cloud-created Drive `.md` as proof a capture reached Obsidian.

---

## 2026-09-14 Documented Bob's existing access; Drive-ownership question resolved; Bob going into Codex next

**What we already know, confirmed, not guessed:**
- Bob already has full access to: work Gmail (connection "Work", bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com), Google Drive (shared into the work account, including the Obsidian vault folder), the local drive (Codex reaches this directly), and HubSpot (same work account's portal). None of this needed setting up, it was already in place.
- A separate Claude session had already written the full account-boundary rules into `CODEX-ACTIVITY-AUDIT-RULES.md` in the main Bob-the-PM folder, before this session got to it. This session added a short "Bob's existing access" section to the main `README.md` that points to that file, instead of writing the rules a second time in a different place.
- The personal Gmail account, bnsn4ull@gmail.com, is unchanged, still off-limits, no Gmail connection to it at all. The user did not ask to remove that wall, only confirmed the other four systems.
- The user's next step: get Codex to test the GitHub interaction, then install Bob into Codex.
- Resolved: Drive listing the vault's owner as `bnsn4ull@gmail.com` is correct, not a mismatch. The user owns the vault on their personal account and shared it into the work account (bensonfoo@ecquaria.com / bensonfoo@toppanecquaria.com) specifically so Claude and Codex can read it there. `obsidian-temp-notes/README.md` updated to reflect this, no longer flagged as open.

**What we are choosing to leave open, or unsure of, for now:**
- Nothing outstanding from this entry.

**The one goal for this phase:**
- Get Codex actually running with the current Bob-the-PM setup, GitHub interaction tested first, then Bob installed into Codex properly.

**Anything the next session should NOT re-ask, because it is already settled:**
- Do not ask again whether work Gmail, Drive, local drive, or HubSpot access exists, they do, see `README.md` and `CODEX-ACTIVITY-AUDIT-RULES.md` in the main Bob-the-PM folder.
- Do not remove or loosen the bnsn4ull@gmail.com personal-Gmail wall without the user explicitly saying so first.

---

## 2026-09-13 Set-up phase, writing CLAUDE.md and AGENTS.md

**What we already know, confirmed, not guessed:**
- Two instruction files are wanted, `AGENTS.md` as the main, full set of rules, `CLAUDE.md` as a short pointer to it plus one Claude-only note.
- The files are meant to be a reusable template, not written only for Project1.
- The user is not a technical person, so anything explained to them, in these files or in conversation, must use plain, everyday language, no jargon.
- Past problems, in the user's own words, were, too much back and forth from guessing wrong, wasted computing cost from too much text or too many connected tools, and inconsistent quality between Claude and Codex.

**What we are choosing to leave open, or unsure of, for now:**
- Exact wording of the trimmed, shorter version of AGENTS.md and CLAUDE.md, a first draft exists but needs revising down in length.
- How big a task has to be before it counts as a new "phase" or "sprint," left as a judgement call, the helper proposes, the user confirms.

**The one goal for this phase:**
- Agree a working template for AGENTS.md and CLAUDE.md, and a simple, working filing system in Bob-the-PM, before either gets used for real work.

**Anything the next session should NOT re-ask, because it is already settled:**
- Bob-the-PM's folder rule, one folder per project, only shared template files loose in the main folder, is settled, do not re-propose a different filing system.
- The instruction files stay split into two, do not merge them into one file.
