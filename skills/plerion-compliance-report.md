---
name: Pull a Plerion compliance framework report
description: Check compliance framework posture for an integration and download a generated compliance report.
api: openapi/plerion-openapi-original.yml
operations: [listTenantComplianceFrameworks, requestComplianceFrameworkReport, downloadComplianceFramework]
---

# Pull a Plerion compliance framework report

Review compliance posture (e.g. ISO 27001, SOC 2, CIS) and fetch a downloadable report.

## Auth
- Send `Authorization: Bearer $PLERION_API_KEY` on every request (HTTPS only).
- Base URL: `https://{region}.api.plerion.com` (default region `au`).

## Steps
1. `listTenantComplianceFrameworks` — list frameworks and their compliance posture across the tenant; note the `integrationId` and `complianceFrameworkId` you care about.
2. `requestComplianceFrameworkReport` — start generation of the report for that integration + framework.
3. `downloadComplianceFramework` — poll this endpoint to receive a pre-signed download URL (valid ~1 hour), then fetch the report with an ordinary HTTP GET.

## Conventions
- Report generation is asynchronous: request, then poll the download endpoint until the URL is available.
- Errors are `application/json`; handle `403`, `404` (tenant/integration not found), and `429`.
