---
name: Scan Infrastructure-as-Code with Plerion
description: Upload IaC to Plerion Code Security, then retrieve the resulting findings and vulnerabilities for a scan.
api: openapi/plerion-openapi-original.yml
operations: [postIaCScan, listIaCScansByTenant, getIaCFindingsByScanId, getIaCVulnerabilitiesByScanId]
---

# Scan Infrastructure-as-Code with Plerion

Run a Plerion Code Security IaC scan (Terraform, CloudFormation, Kubernetes, and more) and read its results.

## Auth
- Send `Authorization: Bearer $PLERION_API_KEY` on every request (HTTPS only).
- Base URL: `https://{region}.api.plerion.com` (default region `au`).

## Steps
1. `postIaCScan` — upload the IaC bundle for scanning. Keep uploads within the documented size limit; oversized uploads return `413 Request Entity Too Large`.
2. `listIaCScansByTenant` — list scans to obtain the `scanId` of the scan you just started.
3. `getIaCFindingsByScanId` — retrieve misconfiguration/compliance findings for that `scanId`.
4. `getIaCVulnerabilitiesByScanId` — retrieve software-composition vulnerabilities for that `scanId`.

## Conventions
- Cursor pagination (`cursor` + `perPage`, `hasNextPage`) applies to the list/results endpoints.
- Errors are `application/json`; handle `403`, `413` (upload too large), `422`, and `429`.
