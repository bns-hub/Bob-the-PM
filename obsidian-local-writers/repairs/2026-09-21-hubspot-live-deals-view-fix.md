---
repair_version: 1
repair_id: obsidian-repair-20260921-hubspot-live-deals-view-fix
status: awaiting_local_writer
created_at: "2026-09-21T14:39:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
priority: urgent-user-visible
---

# Hubspot Live Deals + Active Projects view fix

## Verified current bug

The synced `00 Home/Active Projects.base` still has a global filter:

- `node_type == "project"`
- folder restricted to `E Efforts/Work` or `E Efforts/Personal`

Therefore active HubSpot deal notes can never appear in the default Work view.

The synced `00 Home/Hubspot Live Deals.base` still binds legacy fields:

- `company`
- `deal_owner`
- `deal_stage`

The user-visible cards therefore show blank Account/customer, Deal owner, and deal identity even though Deal type and stage are populated.

## Required exact repair

Use authenticated Obsidian MCP against active vault exactly `Ben` and acquire the single-writer lock before any change.

### 1. Normalize all open HubSpot deal notes

Use these exact properties:

```yaml
hubspot_deal_id:
deal_name:
canonical_name:
account_name:
account:
deal_owner_id:
deal_owner_name:
deal_type:
hubspot_stage:
show_in_live_deals: true
work_type: deal
```

Hydrate missing CRM-owned values from HubSpot read-only provider data. Resolve owner IDs to human-readable names. Use associated HubSpot COMPANY where present. Do not write back to HubSpot.

### 2. Fix Hubspot Live Deals Base

Preserve exact filename:

`00 Home/Hubspot Live Deals.base`

Public Obsidian 1.13.7 only; use Cards/Table, not native Kanban.

Display order:
1. `canonical_name` or file name
2. `account_name`
3. `deal_owner_name`
4. `deal_type`
5. `hubspot_stage`

Views:
- `My Active` first/default; exclude `show_in_live_deals: false` and terminal stages CAT 1 - Won, Lost / Potential Lost, Dropped, No Award.
- `By Account`; sort account A→Z, then canonical name A→Z.
- `Pipeline`; group/sort by hubspot stage where supported.
- `All Live Deals`; no local hide filter.

Local hide behavior:
- `show_in_live_deals: false` hides a deal from My Active only.
- never write this flag to HubSpot.

### 3. Fix Active Projects as current-work dashboard

Preserve exact filename:

`00 Home/Active Projects.base`

Remove the global project-only restriction.

Default `Work` view must include both:
- active work projects; and
- active nonterminal HubSpot deal notes.

Do not clone deals into project notes. Show `work_type` so Deal vs Project is obvious.

Retain/add:
- Work (first/default)
- Projects Only
- Personal
- All

Sort Work by account/company then canonical name where supported.

Required Work visibility:
- `NEA - AMS3`
- `SIT - SITAR`
- `LTA - LTA.PROMPT 2.0`
- `BreadTalk - AI Initiative`
- `PUB - AMS`
- `Lead Generation and Outreach` / alias `Corporate Outreach`

### 4. Specific verified HubSpot records

- NEA - AMS3: deal 340928313029; owner Benson Foo; account fallback National Environment Agency; CRM company association currently missing.
- SIT - SITAR: deal 348238350062; owner Benson Foo; account fallback Singapore Institute of Technology; CRM company association currently missing.
- LTA - LTA.PROMPT 2.0: deal 348349823687; account Land Transport Authority; owner Benson Foo.
- BreadTalk - AI Initiative: deal 347393181393; account BreadTalk Group; owner Benson Foo.
- PUB - AMS: deal 348946112222; account Public Utility Board (PUB); owner Benson Foo; full CRM title `PUB - Provision of Software Update and Maintenance Services for PUB's Asset Management System (AMS) PUB000ETT26000110`; preserve full CRM title as source/alias while canonical display remains `PUB - AMS`.
- Corporate Outreach: no HubSpot deal by that phrase; verify as alias of existing `Lead Generation and Outreach` project, not a duplicate.

### 5. Verification gate

Before completion:
- re-read every changed deal note and both Base files through Obsidian MCP;
- open each Base and confirm the six requested Work items are returned by the default Work view;
- confirm visible deal title, account, owner, type and stage in Hubspot Live Deals;
- verify By Account ordering;
- verify `show_in_live_deals: false` hides only locally;
- run affected broken-link checks;
- update this manifest with exact changed paths and counts;
- refresh local writer heartbeat;
- confirm sync state if available.
