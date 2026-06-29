# aws-011 Prism SaaS Admin Workspace checkpoint
Date: 2026-06-29
## Milestone
`aws-011` is the current verified PrismERP AWS development/runtime image.
This checkpoint completes the Prism SaaS Admin Desk exposure and rendering milestone.
The Prism SaaS Admin Workspace is now reachable at:
```text
https://erp.prismtechco.com/desk/prism-saas-admin
Runtime image
prismerp:erpnext-16.15.1-frappe-version-16-aws-011
Image ID:
6bfad23c219b
Previous runtime image:
prismerp:erpnext-16.15.1-frappe-version-16-aws-010
Source commit
prism_saas:
6520980 Add Prism SaaS Admin workspace content layout
Relevant preceding fixes:
9b5a764 Fix Prism SaaS Admin workspace visibility and roles
a800020 Fix Prism SaaS Admin workspace JSON doctype
cbda806 Fix Prism SaaS Admin workspace delivery
Build log
/opt/prismerp/logs/builds/prismerp-image-build-aws-011-20260629T132342Z.log
Pre-promotion backup
Backup taken before promoting app services to aws-011:
20260629_163458-aws-dev-erp_localhost-database.sql.gz
20260629_163458-aws-dev-erp_localhost-files.tar
20260629_163458-aws-dev-erp_localhost-private-files.tar
20260629_163458-aws-dev-erp_localhost-site_config_backup.json
Services promoted
The following app services were promoted to aws-011:
backend
frontend
websocket
queue-short
queue-long
scheduler
The following services were intentionally not recreated:
db
redis-cache
redis-queue
Migration
Command:
bench --site aws-dev-erp.localhost migrate
Result: successful.
Non-fatal warning observed:
/home/frappe/frappe-bench/apps/prism_saas/prism_saas/fixtures/role.json missing
What aws-011 fixed
aws-011 added the Frappe v16 Workspace content layout for Prism SaaS Admin.
Before this fix, the Workspace route opened but the page body showed empty grey loading placeholders
because the live Workspace had:
CONTENT_TYPE=NoneType
CONTENT_LENGTH=0
After aws-011, the live Workspace verification showed:
CONTENT_TYPE=str
CONTENT_LENGTH=919
CONTENT_BLOCKS=9
CONTENT_BLOCK_TYPES=["header", "shortcut", "shortcut", "shortcut", "shortcut", "shortcut",
"shortcut", "shortcut", "shortcut"]
Final Workspace verification
The live Workspace verification passed:
PUBLIC=1
IS_HIDDEN=0
APP='prism_saas'
CONTENT_LENGTH=919
CONTENT_BLOCKS=9
SHORTCUT_TABLE_COUNT=8
VERIFY_RESULT=PASS
Rendered shortcuts confirmed in browser:
Platform Settings
Tenant Plans
Tenant Requests
Tenants
Tenant Domains
Tenant Members
Provisioning Jobs
Lifecycle Events
Current status
Prism SaaS Admin is now usable as the PrismERP control-plane administration Workspace.
This completes the control-plane UI exposure milestone.
Not completed yet
The following multi-tenant SaaS lifecycle features remain future work:
Tenant Request approval workflow
Tenant record creation workflow
Provisioning Job execution logic
Provisioning Step logs
Dry-run provisioning mode
Real bench new-site tenant provisioning
Tenant admin creation
Tenant domain/host_name setup
Tenant lifecycle event automation
Tenant backup/restore tracking
Clerk/public signup
Billing integration
Prohibited actions not performed
No tenant sites created
No tenant provisioning executed
No Caddy/site_config changes
No DB/Redis recreation
No volume deletion
No Docker prune
No force push
No README.md modification
