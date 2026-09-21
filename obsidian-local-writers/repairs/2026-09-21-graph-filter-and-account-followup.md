# Ben graph filter and deal account follow-up

Date: 2026-09-21
Device: LAPTOP-96G8839H
Writer: Codex
Vault: `Ben` only
Status: local Obsidian MCP verified; cloud sync not independently checked

## Graph

The existing graph filter omitted `X Filtered Out` and `S Sources/HubSpot/Audit Batches`. A sampled small node opened an existing filtered audit note under `X Filtered Out/Emails`, confirming that filtered evidence files appeared in the graph. The audit-batch folder contains historical source containers, not canonical business owners.

In Obsidian 1.13.7 graph settings, set `Existing files only` on and `Orphans` off. Extended the live graph search and `00 Home/Default View filter.md` with:

```
-path:"X Filtered Out" -path:"S Sources/HubSpot/Audit Batches"
```

The live graph was inspected after applying the search and became visibly smaller. No notes, links, or source evidence were deleted.

## Account fallback

The following five deal notes lacked a current HubSpot COMPANY association in the previous repair. Their local deal notes contained a company link and HubSpot company ID, and the corresponding company notes contained a reciprocal link to the same deal. Used those provider-ID-backed local associations to populate `account_name` and `account`:

- `E Efforts/HubSpot Deals/Ayodia Co., Ltd (2).md` -> `Ayodia Co., Ltd.` (company ID 344104103670)
- `E Efforts/HubSpot Deals/Betimes Solutions Co., Ltd (2).md` -> `Betimes Solutions Co., Ltd.` (company ID 344082588392)
- `E Efforts/HubSpot Deals/Digital Dialogue Co., Ltd (2).md` -> `Digital Dialogue Co., Ltd.` (company ID 344152931046)
- `E Efforts/HubSpot Deals/Finema Co., Ltd (2).md` -> `Finema Co., Ltd.` (company ID 344160080615)
- `E Efforts/HubSpot Deals/SDLT Company Limited (2).md` -> `SDLT Company Limited` (company ID 344154725050)

Current HubSpot company associations remain absent for these five. The remaining unresolved deal accounts and the prior 14 multi-company and three unavailable-record cases remain in `2026-09-21-hubspot-live-deals-view-fix.md`. Do not infer an account solely from a deal title.

## Verification

- Re-read all six changed vault files through Obsidian MCP; the graph query and the five account labels matched the intended values.
- Obsidian MCP broken-link scan of all six changed files returned zero broken links.
- Live graph still showed the updated query, `Existing files only` enabled, and `Orphans` disabled after the view settled.

Changed vault paths: `00 Home/Default View filter.md` and the five deal notes listed above. Graph settings were changed through the Obsidian user interface.
