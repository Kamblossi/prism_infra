# aws-012 Checkpoint - Manual Tenant Request Conversion Workflow

Date: 2026-06-29  
Runtime target: `prism-aws-dev` on AWS  
Control site: `aws-dev-erp.localhost`  
Public platform URL: `https://erp.prismtechco.com`

## Image

`prismerp:erpnext-16.15.1-frappe-version-16-aws-012`

Image ID:

`f1879590e06a`

## Source commit

`prism_saas`:

`b5d24f8 Add manual tenant request conversion workflow`

## Purpose

This image makes the Prism SaaS v1 manual tenant workflow permanent in the runtime image.

The workflow supports:

1. Approving a `Prism Tenant Request`
2. Converting the request into a `Prism Tenant`
3. Creating a primary `Prism Tenant Domain`
4. Creating a `Prism Tenant Member`
5. Creating a queued `Prism Provisioning Job`
6. Creating default `Prism Provisioning Step` rows
7. Creating `Prism Tenant Lifecycle Event` records

## Safety boundary

This checkpoint is still control-plane only.

It does not:

- run `bench new-site`
- create a tenant Frappe site
- install apps on tenant sites
- change Caddy
- change DNS
- change `site_config.json`
- provision real customer runtime databases

## Autoname fix

The following DocType autoname values were corrected:

| DocType | Autoname |
|---|---|
| Prism Tenant Request | `format:TREQ-{#####}` |
| Prism Tenant Domain | `format:TDOM-{#####}` |
| Prism Tenant Member | `format:TMEM-{#####}` |
| Prism Provisioning Job | `format:PJOB-{#####}` |
| Prism Tenant Lifecycle Event | `format:LEV-{#####}` |

This replaced the previous invalid pattern style such as `format:LEV-.#####`, which Frappe treated literally.

## Deployment validation

Deployed runtime verification passed:

```text
DEPLOYED_AUTONAME_VALIDATION=PASS
DEPLOYED_WORKFLOW_TEST_RESULT=PASS
DEPLOYED_ROLLBACK_RESULT=PASS
DEPLOYED_AWS012_RESULT=PASS
```

Rollback verification created and rolled back:

* `Prism Tenant Request`
* `Prism Tenant`
* `Prism Tenant Domain`
* `Prism Tenant Member`
* `Prism Provisioning Job`
* 9 provisioning steps
* 3 lifecycle events

Post-rollback counts were all zero.

## Promotion backup

Pre-promotion backup:

```text
20260629_185912-aws-dev-erp_localhost-site_config_backup.json
20260629_185912-aws-dev-erp_localhost-database.sql.gz
20260629_185912-aws-dev-erp_localhost-files.tar
20260629_185912-aws-dev-erp_localhost-private-files.tar
```

## Final container state

App services promoted to `aws-012`:

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

## Next milestone options

Recommended next step:

1. Add Desk buttons/client UX for approving and converting Tenant Requests.

Then:

2. Add dry-run provisioning executor that marks provisioning steps without running bench commands.
3. Add real provisioning execution only after dry-run is proven.
