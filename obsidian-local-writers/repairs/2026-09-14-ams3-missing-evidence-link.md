---
repair_version: 1
repair_id: obsidian-repair-20260914-ams3-missing-evidence-link
status: unresolved_routing
created_at: "2026-09-14T18:20:00+08:00"
created_by: local-codex
scope: ams3-owner-broken-link
max_target_notes: 2
source_of_truth: OBSIDIAN-GRAPH-HYGIENE.md
---

# AMS3 missing evidence-link repair

## Verified state

- `Excalidraw/Drawing NEA AMS3, 2026-09-14 17.11.28.excalidraw.md` was linked through its `project` property to the verified owner `E Efforts/HubSpot Deals/NEA Consolidated Application Maintenance Services (AMS3)(NEA000ETT26000073).md`.
- The drawing's link resolves and the owner reports the drawing as a backlink.
- Google Drive later showed the synced drawing as file ID `1Xv7SjVPD_NpSyOUK64Kfe2eb7tYzvdNU`, updated at `2026-09-14T10:16:13.893Z`, with the same AMS3 `project` link present. This confirms the local amendment propagated to Drive.
- The affected-cluster check found one older unrelated broken link in the AMS3 owner: `[[S Sources/NEA AMS3 Management Price Review Acceptance|NEA AMS3 Management Price Review Acceptance]]`.
- The expected target was not resolved during the bounded check. The link was left unchanged to preserve evidence and avoid guessing.

## Next action

Search the live vault and authorised source evidence by stable Gmail message IDs `1a0984b61081394e`, `1a0984b54ab4e42c`, `1a09a8b9bd805ec8`, and `1a09d86e6a52a5ec`. If the intended evidence note exists under a different verified path, repair the link. If it does not exist, recreate a human-readable evidence note only from those exact source records, preserving their IDs and hashes. Re-run the AMS3 cluster broken-link check before marking this repair completed.
