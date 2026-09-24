# Claude instruction: Obsidian deal ownership

This is an instruction update only. Do not edit files, refresh data, change dashboards, or take any other action. Learn the behaviour below for future Bob and Obsidian work, then only acknowledge that you understand it.

For every user-facing Obsidian deal dashboard and view, use the current human-readable `deal_owner_name` as the primary ownership and filtering field.

- For Benson's deal visibility, use `deal_owner_name == "Benson Foo"` in both `Hubspot Live Deals.base` -> `My Active` and the deal branch of `Active Projects.base` -> `Work`.
- On future HubSpot deal refreshes, update both `deal_owner_name` and `deal_owner_id` from current HubSpot data before evaluating dashboard visibility.
- Retain `deal_owner_id` only for reconciliation, audit, and provider identity checks. Do not use it as the primary user-facing inclusion filter.
- If the owner name is missing, resolve it before deciding whether the deal should appear.
- If two owners ever have the same display name, flag the ambiguity for Benson instead of silently switching back to raw owner-ID filtering.

Do not take action now. Reply only to confirm that you understand and will follow this behaviour in future.
