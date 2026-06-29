# aws-013 Checkpoint - Tenant Request Desk Workflow Buttons

Date: 2026-06-29  
Runtime target: `prism-aws-dev` on AWS  
Control site: `aws-dev-erp.localhost`  
Public platform URL: `https://erp.prismtechco.com`

## Image

`prismerp:erpnext-16.15.1-frappe-version-16-aws-013`

Image ID:

`619571a8a702`

## Source commit

`prism_saas`:

`3e1bb22 Add Tenant Request desk workflow buttons`

## Purpose

This checkpoint adds Desk usability for the Prism SaaS tenant request workflow.

The `Prism Tenant Request` form now exposes workflow actions:

- Approve
- Convert to Tenant
- Approve and Convert
- Open Tenant

These actions call the server-side Prism SaaS workflow added in `aws-012`.

## Browser validation

A real control-plane test was completed from the Desk UI.

Created request:

`TREQ-00001`

Converted tenant:

`testco`

Created records:

| Record | Value |
|---|---|
| Prism Tenant Request | `TREQ-00001` |
| Prism Tenant | `testco` |
| Primary Domain | `testco-erp.prismtechco.com` |
| Prism Tenant Domain | `TDOM-00002` |
| Prism Tenant Member | `TMEM-00003` |
| Prism Provisioning Job | `PJOB-00004` |

Lifecycle events:

| Event | Type |
|---|---|
| `LEV-00005` | Tenant Created |
| `LEV-00006` | Provisioning Job Queued |
| `LEV-00007` | Tenant Request Converted |

Provisioning steps:

- 9 steps created
- all steps currently `Pending`
- job status currently `Queued`
- tenant status currently `Provisioning Queued`

## Safety boundary

This checkpoint still does not create real tenant sites.

It does not:

- run `bench new-site`
- install ERPNext on a tenant site
- install `prism_brand` on a tenant site
- create tenant-site users
- change Caddy
- change DNS
- change tenant `site_config.json`

## Promotion backup

Pre-promotion backup:

```text
20260629_194655-aws-dev-erp_localhost-site_config_backup.json
20260629_194655-aws-dev-erp_localhost-database.sql.gz
20260629_194655-aws-dev-erp_localhost-files.tar
20260629_194655-aws-dev-erp_localhost-private-files.tar
```

## Final runtime state

App services promoted to `aws-013`:

* backend
* frontend
* websocket
* queue-short
* queue-long
* scheduler

Persistent services unchanged:

* `mariadb:11.8`
* `redis:8.6-alpine` cache
* `redis:8.6-alpine` queue

## Next milestone

Build the dry-run provisioning executor for `Prism Provisioning Job`.

The dry-run executor should process `PJOB-00004` and mark steps as simulated without running tenant creation commands.
