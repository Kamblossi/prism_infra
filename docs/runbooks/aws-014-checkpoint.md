# aws-014 Checkpoint - Dry-run Provisioning Executor

Date: 2026-06-30  
Runtime target: AWS PrismERP dev  
Compose project: `prism-aws-dev`  
Frappe site: `aws-dev-erp.localhost`

## Runtime Image

`prismerp:erpnext-16.15.1-frappe-version-16-aws-014`

Image ID:

`ee1553732674`

Build log:

`/opt/prismerp/logs/builds/prismerp-image-build-aws-014-20260630T050924Z.log`

## Source Commit

`prism_saas`:

`d59e4fd Add dry-run provisioning job executor`

## What Changed

`aws-014` introduced a safe dry-run provisioning executor for Prism SaaS control-plane jobs.

New source:

`prism_saas/services/provisioning_executor.py`

Updated controller:

`prism_saas/prism_saas/doctype/prism_provisioning_job/prism_provisioning_job.py`

New whitelisted controller methods:

- `run_dry_run(name)`
- `run_next_queued_dry_run()`

## Safety Boundary

The dry-run executor only updates control-plane records.

It does not:

- create a tenant Frappe site
- run `bench new-site`
- install ERPNext on a tenant site
- install `prism_brand` on a tenant site
- create tenant-site users
- change Caddy
- change DNS
- change tenant `site_config.json`

The executor returns:

- `site_created: false`
- `commands_executed: false`

## Deployment Result

App services promoted to `aws-014`:

- backend
- frontend
- websocket
- queue-short
- queue-long
- scheduler

Persistent services were unchanged:

- MariaDB `mariadb:11.8`
- Redis cache
- Redis queue

Migration completed successfully. The existing non-fatal warning remained:

`/home/frappe/frappe-bench/apps/prism_saas/prism_saas/fixtures/role.json missing`

## Backups

Pre-aws-014 deployment backup:

- `20260630_081823-aws-dev-erp_localhost-site_config_backup.json`
- `20260630_081823-aws-dev-erp_localhost-database.sql.gz`
- `20260630_081823-aws-dev-erp_localhost-files.tar`
- `20260630_081823-aws-dev-erp_localhost-private-files.tar`

Pre-dry-run execution backup:

- `20260630_082417-aws-dev-erp_localhost-site_config_backup.json`
- `20260630_082417-aws-dev-erp_localhost-database.sql.gz`
- `20260630_082417-aws-dev-erp_localhost-files.tar`
- `20260630_082417-aws-dev-erp_localhost-private-files.tar`

## Verified Test Tenant State

Tenant:

`testco`

Tenant Request:

`TREQ-00001`

Provisioning Job:

`PJOB-00004`

After permanent dry-run execution:

- `PJOB-00004` status: `Succeeded`
- worker ID: `prism-saas-dry-run`
- attempt count: `1`
- all 9 provisioning steps: `Succeeded`
- tenant `testco` status: `Provisioning Queued`
- tenant was not marked `Active`
- primary domain remained `Pending DNS`
- tenant member remained `Invited`

Lifecycle events added:

- `LEV-00008` Dry Run Provisioning Started
- `LEV-00009` Dry Run Provisioning Succeeded

## Verification Results

Image validation:

`IMAGE_AWS014_VALIDATION=PASS`

Deployment validation:

`DEPLOYED_AWS014_RESULT=PASS`

Permanent dry-run:

`PERMANENT_DRY_RUN_RESULT=PASS`

Final tenant verification:

`TESTCO_AFTER_DRY_RUN_RESULT=PASS`

## Next Milestone

Build the real provisioning executor.

The real executor should begin with a carefully controlled single-tenant path for `testco`, with rollback checks and explicit safeguards before allowing broader tenant provisioning.
