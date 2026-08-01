---
name: Triage Plerion security findings
description: Authenticate to the Plerion API and pull the tenant's failed security findings and top risks so an agent can triage cloud security posture.
api: openapi/plerion-openapi-original.yml
operations: [getTenantDetails, listTenantFindings, listTenantRisks, listTenantAlerts]
---

# Triage Plerion security findings

Use the Plerion tenant API to review current cloud security posture and prioritize what to fix.

## Auth
- Send `Authorization: Bearer $PLERION_API_KEY` on every request (HTTPS only; plain HTTP fails).
- Manage keys at https://app.plerion.com/settings/api-keys.
- Base URL: `https://{region}.api.plerion.com` where region is one of `au`, `sg1`, `in1`, `us1` (default `au`).

## Steps
1. `getTenantDetails` — confirm the tenant and read its current risk score for baseline context.
2. `listTenantFindings` — filter to `statuses=FAILED` to get only open failures. Page with `cursor` + `perPage`; keep requesting while `hasNextPage` is true.
3. `listTenantRisks` — pull prioritized risks to focus on what matters most.
4. `listTenantAlerts` — review risk-based alerts for anything already surfaced to the team.

## Conventions
- Pagination is cursor-based: pass `perPage` (positive integer within the documented max) and the returned `cursor`; stop when `hasNextPage` is false.
- Errors return `application/json`. Handle `403` (bad/absent key), `422` (invalid params like `perPage`), and `429` (rate limited — back off and retry).
