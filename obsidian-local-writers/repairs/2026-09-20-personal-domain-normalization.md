---
repair_version: 1
repair_id: obsidian-repair-20260920-personal-domain-normalization
status: pending
created_at: "2026-09-20T04:17:00+08:00"
created_by: chatgpt-cloud
scope: personal-domain-cluster
max_target_notes: 50
source_of_truth: OBSIDIAN-GRAPH-HYGIENE.md
---

# Personal domain normalization

## Why this repair exists

The user wants personal material clearly separated from work material without replacing the established ACES vault architecture. Some AI-created tasks and notes are personal even though they were created or maintained by ChatGPT/Codex/Claude.

Read `OBSIDIAN-GRAPH-HYGIENE.md` before processing. Re-read the live `Ben` vault through Obsidian MCP and do not apply this manifest as a blind patch.

## Required outcome

### 1. Establish the Personal domain

Prefer the following structure, reusing existing equivalent folders/notes when they already exist:

- `A Atlas/Personal/Personal MOC.md`
- `A Atlas/Personal/Games/`
- `C Calendar/Personal/`
- `E Efforts/Personal/`
- `S Sources/Personal/`
- `Z System/Personal Automations.md`

Do not create a second parallel vault architecture. Personal remains a domain inside ACES.

The Personal MOC should link to useful personal areas such as Tech, Games, Purchases, Travel, Finance, Household, Personal Projects, and Personal Automations, but create only areas supported by actual notes.

### 2. Classify by purpose, not by tool

Where supported by the note's content and current metadata conventions, add:

```yaml
domain: personal
```

or:

```yaml
domain: work
```

Do not classify a note as work merely because it came from ChatGPT, Codex, Claude, GitHub, or an automation.

### 3. Initial personal candidates

Search the live vault for existing notes/tasks/projects related to these explicit personal areas and normalize only verified matches:

- Gacha Calendar / game schedule project;
- Reverse: 1999 / RE1999 notes, banners, events, selector/rerun tracking;
- Honkai: Star Rail / HSR banner and event tracking;
- Chaos Zero Nightmare / CZN banner and event tracking;
- Apple Watch selection / purchase research;
- Fold5 -> iPhone Duo migration and related personal-device setup;
- clearly personal travel, purchase, lifestyle, game, or household notes.

Treat scheduling/automation notes for these subjects as personal too.

### 4. Keep work material separate

Do not move or relabel these as personal:

- GeBIZ tender pipeline / tender review;
- Activity Audit / knowledge sweep;
- ACC ICT Roadmap;
- NEA AMS3 and customer/project delivery notes;
- TOPPAN Ecquaria source/skill refreshes;
- other clearly corporate Gmail/HubSpot/customer/project records.

### 5. Minimal-change migration

For existing notes:
1. re-read the current note through Obsidian MCP;
2. identify the verified domain from content;
3. add the smallest useful domain metadata and Personal MOC/project link;
4. move/rename only where the destination is clear and consistent with existing vault conventions;
5. preserve user-authored content, capture IDs, source IDs, hashes, links, and frontmatter;
6. re-read the changed note and verify all links;
7. keep ambiguous notes in place and record `unresolved_routing`.

Do not mass-move the vault. Process at most 50 target notes in this repair.

## Verification

When complete, update this manifest with:
- `status: completed`;
- `completed_at`;
- processor device;
- Personal MOC path;
- folders/notes created or reused;
- count/list of notes classified or moved;
- unresolved candidates;
- verification result.

If the Personal MOC or note routing cannot be resolved safely, leave the repair pending or mark `unresolved_routing` rather than guessing.
