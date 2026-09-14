# Local Obsidian writers

This folder contains shared coordination records for the Bob to Obsidian workflow.

- `processor-lock.md` exists only while one desktop or laptop local Codex processor owns the queue.
- `<device_id>-codex.md` records the last confirmed status of that device.
- `claims/` contains durable per-capture claim and completion records. Completed claims are never deleted.

The exact lock, claim, expiry, recovery, and cleanup rules are in [`../OBSIDIAN-INGESTION-CONTRACT.md`](../OBSIDIAN-INGESTION-CONTRACT.md).

These files coordinate writers. They do not prove that Obsidian received a note. Only a successful authenticated Obsidian Model Context Protocol write followed by an Obsidian Model Context Protocol re-read of the exact destination proves local delivery.
