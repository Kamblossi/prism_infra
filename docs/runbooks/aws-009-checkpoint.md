# aws-009 Checkpoint

## Date

2026-06-12

## Image

- Tag: `prismerp:erpnext-16.15.1-frappe-version-16-aws-009`
- ID: `sha256:786a66e75f60674817cd319c73ab75140aa614e1c524fdc2cc1c9b99b618b2e3`
- Base: `aws-005` (`e2ba56acfe46`)
- Source: `prism_saas` commit `a800020` (main)
- Previous live: `aws-008`

## Context

`aws-008` was built and promoted but migration failed because the Prism SaaS Admin
workspace JSON was missing the top-level `"doctype": "Workspace"` key. That source
issue was fixed in commit `a800020`, and `aws-009` was built as a clean replacement.

## Changes (vs aws-008)

- Workspace JSON: added top-level `"doctype": "Workspace"`
- Workspace JSON placed at correct module path:
  `prism_saas/prism_saas/workspace/prism_saas_admin/prism_saas_admin.json`
- Wrong app-root workspace path absent:
  `prism_saas/workspace/prism_saas_admin/` (removed)
- Old workspace-creating patch neutralized to `frappe.clear_cache()` only
- New role-assignment patch added:
  `prism_saas.patches.v0_1_0.assign_prism_platform_admin_roles`

## Build

- Fresh deterministic build context from `/opt/prismerp/src/prism_saas`
- Excluded `.git`, `__pycache__`, `*.pyc`, stale tarballs
- No aws-006/007/008 build contexts reused

## Migration

- Command: `bench --site aws-dev-erp.localhost migrate`
- Result: success
- Patches logged:
  - `prism_saas.patches.v0_1_0.create_prism_saas_admin_workspace` (neutralized)
  - `prism_saas.patches.v0_1_0.assign_prism_platform_admin_roles`

## Post-migration verification

- `Prism SaaS Admin` workspace exists, module=`Prism SaaS`
- Workspace shortcuts:
  - Prism Platform Settings (DocType)
  - Prism Tenant Plan (DocType, List)
  - Prism Tenant Request (DocType, List)
  - Prism Tenant (DocType, List)
  - Prism Tenant Domain (DocType, List)
  - Prism Tenant Member (DocType, List)
  - Prism Provisioning Job (DocType, List)
  - Prism Tenant Lifecycle Event (DocType, List)
- Administrator has `Prism Platform Admin` role
- `newton.ochieng97@gmail.com` has `Prism Platform Admin` role
- App visibility helper allows platform admin, denies Guest/non-platform users
- Control-plane tenant count: 0 (clean)

## Services reconciled

- backend, frontend, websocket, queue-short, queue-long, scheduler → aws-009
- DB, redis-cache, redis-queue untouched

## Runtime health

- 2 workers online, scheduler active
