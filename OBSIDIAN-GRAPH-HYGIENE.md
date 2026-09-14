# Obsidian graph hygiene and consolidation contract

This file defines how the Bob / Codex pipeline keeps the `Ben` vault useful as a knowledge graph instead of merely accumulating source records. It supplements `OBSIDIAN-DELIVERY-ARCHITECTURE.md`, `OBSIDIAN-INGESTION-CONTRACT.md`, and `CODEX-ACTIVITY-AUDIT-RULES.md`. The stricter safety rule wins if wording conflicts.

## Goal

Every substantive imported item should become useful context under a verified canonical owner. The vault must not treat every email, RSVP, HubSpot batch, or capture as an equally important standalone knowledge node.

The desired pattern is:

`raw/audit evidence -> one canonical activity/event when needed -> one canonical project/tender/account/deal owner -> broader account/entity/MOC`

The pipeline must preserve evidence without turning evidence containers into the main graph structure.

## Canonical-owner rule

For each new or changed substantive note, resolve exactly one primary owner when the evidence supports it:

- Tender
- Service Request
- Change Request
- Project Activity
- Campaign / Account / Sales
- Unresolved Routing

When a specific project, tender, service request, change request, deal, or account owner already exists, prefer that owner over a broad umbrella such as `NEA Engagement`, `TOPPAN Ecquaria`, `Benson Foo`, a source MOC, or an audit log.

A broad account or engagement note remains useful as a parent, but it must not be the only project link when a more specific verified owner is known.

## Explicit project captures

A capture with `project: [<name>]` is not allowed to remain only as plain text `Project: <name>` in the destination note.

Before creating a standalone note, local Codex must search for:

1. an existing canonical project note or MOC with the same verified project identity;
2. an existing HubSpot deal/project owner that clearly represents that project;
3. an existing tender/SR/CR owner when the capture is actually about one of those;
4. the matching Bob-the-PM project folder and its known aliases.

If a verified owner exists, patch that owner or create a small linked child note only when the content genuinely deserves its own lifecycle. If no canonical project hub exists but the project identity is explicit and supported, create one project hub in the established `E Efforts` structure rather than leaving a freestanding unlinked capture.

## Required project linkage

A project-owned note should carry a stable canonical owner reference in frontmatter when the vault's existing conventions allow it, for example `project`, `owner`, `project_link`, or an equivalent established field. The human-readable body must also contain a verified wikilink to the owner.

Do not invent near-duplicate tags. Reuse the vault's controlled taxonomy. Tags support retrieval; wikilinks define graph relationships.

## Consolidation rule

Consolidation means reducing conceptual duplication, not deleting evidence.

Prefer to append or minimally patch an existing owning note when a new source item only adds:

- a status update;
- a personnel update;
- a milestone;
- a decision;
- an action or outcome;
- a billing/acceptance/handover fact;
- a tender clarification or award milestone;
- a service/change request update.

Keep the raw/source record in `S Sources`, `X Filtered Out`, or the existing evidence structure when auditability requires it, but do not promote that source container as a new project node unless it has independent meaning.

Repeated calendar acceptance/decline messages for one event should point to one canonical event/project owner. The source messages may remain as evidence, but they should not each become the only visible representation of the event.

## Audit-source graph rule

Batch manifests, filtered-audit notes, HubSpot record-range files, Gmail evidence batches, checkpoints, and source MOCs are evidence/navigation nodes. They are not business-project owners.

They should link primarily within the source/audit layer and to canonical owners only where needed for traceability. Do not fan every batch note directly into Benson Foo, TOPPAN Ecquaria, and multiple broad MOCs when the same relationship is already represented through the canonical owner.

Never delete historical source batches solely to reduce graph density.

## Daily graph-hygiene pass

`Obsidian Export` performs a read-only graph-hygiene audit on the synced Drive view every daily run. It must inspect newly created or materially changed notes since the previous successful checkpoint and look for:

- explicit project names present only as plain text with no canonical link;
- substantive activity linked only to a broad account/engagement MOC;
- duplicate standalone notes that should update an existing owner;
- repeated event/RSVP notes lacking a canonical event/project link;
- project/tender/SR/CR notes missing controlled category/owner metadata;
- source/audit records incorrectly promoted as knowledge owners;
- newly created canonical owners that older related notes should now link to.

The cloud task does not edit the live vault. It creates deterministic repair manifests under `obsidian-local-writers/repairs/` for an approved local Codex processor.

Each daily pass is bounded: repair one coherent project/account cluster or at most 50 target notes, whichever is smaller. Persist the next resume position. This prevents mass rewriting and keeps repairs reviewable.

## WRMS canonical grouping

WRMS is a concrete example of the required behavior.

Current evidence shows many notes titled or describing `WRMS`, `NEA WRMS`, `WRMS Tender Presentation`, WRMS maintenance, and WRMS project activity. These should not remain grouped only under the broad `NEA Engagement` note.

The local writer must first search the live `Ben` vault for an existing canonical WRMS project hub. If a suitable verified hub already exists, reuse it. If none exists, create a canonical umbrella project hub named `NEA WRMS` in the established `E Efforts/Projects` structure.

The umbrella hub should represent the WRMS programme/project identity and may link to more specific owners such as:

- the existing `NEA - WRMS App Maintenance & Support Renewal` HubSpot/deal note;
- WRMS tender/re-procurement activity;
- WRMS change requests;
- WRMS maintenance/kickoff/handover activity;
- WRMS personnel/project updates.

Do not merge unrelated NEA opportunities into WRMS merely because they involve the same customer.

For existing WRMS calendar/email/activity notes, add the canonical WRMS project link when the content explicitly concerns WRMS. Preserve existing `NEA Engagement` links when they remain useful as a broader parent; the fix is to add the missing specific owner, not erase valid context.

The current `WRMS Personnel Update` capture is an example of a note that should not remain with only the plain text `Project: WRMS`. It must be linked to the canonical WRMS owner, or its content should be minimally incorporated into that owner if that is the cleaner representation.

## Safety and verification

All graph repairs are performed only by the approved local Codex writer through Obsidian MCP while holding the shared processor lock.

Before every patch:

- re-read the current note through Obsidian MCP;
- preserve stable IDs, capture IDs, content hashes, provider IDs, user-authored text, and valid frontmatter;
- never touch `.obsidian/` or application state;
- never bulk move/rename/delete files for cosmetic cleanup;
- never resolve an ambiguous owner by title similarity alone.

After every patch, re-read the exact destination through Obsidian MCP and verify the expected owner link/metadata and preserved source identity.

If the owner is ambiguous, use `unresolved_routing` and leave the evidence intact.