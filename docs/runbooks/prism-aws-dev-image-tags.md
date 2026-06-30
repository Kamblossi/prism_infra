Prism AWS Dev Image Tags
Current runtime image
As of 2026-06-29, the PrismERP AWS development/runtime stack uses:
prismerp:erpnext-16.15.1-frappe-version-16-aws-013
Image ID:
619571a8a702
The aws-013 image contains the verified Prism SaaS Admin Workspace route, permissions, roles,
Frappe v16 Workspace content layout, manual tenant request conversion workflow, and Desk workflow buttons.
Current runtime target
Site: aws-dev-erp.localhost
Public URL: https://erp.prismtechco.com
Prism SaaS Admin: https://erp.prismtechco.com/desk/prism-saas-admin
Compose project: prism-aws-dev
Compose file: /opt/prismerp/gitops/prism-aws-dev/compose/docker-compose.yml
Compose command
Always use:
docker compose -p prism-aws-dev -f /opt/prismerp/gitops/prism-aws-dev/compose/dockercompose.yml
Image tag policy
Use a new immutable release tag for each verified runtime image promotion.
Do not overwrite an existing release tag after it has been documented and used by Compose.
App service reconcile
When changing only the app image tag, reconcile app services only:
docker compose -p prism-aws-dev -f /opt/prismerp/gitops/prism-aws-dev/compose/dockercompose.yml up -d --no-deps backend frontend websocket queue-short queue-long scheduler
Do not recreate DB or Redis for image-tag-only app releases.
Current app services on aws-013
backend
frontend
websocket
queue-short
queue-long
scheduler
Persistent services not recreated during app image promotion
db
redis-cache
redis-queue
Latest verified source
prism_saas:
3e1bb22 Add Tenant Request desk workflow buttons
Latest checkpoint
See:
docs/runbooks/aws-013-checkpoint.md

## aws-014 - Dry-run provisioning executor

- Image: `prismerp:erpnext-16.15.1-frappe-version-16-aws-014`
- Image ID: `ee1553732674`
- Build log: `/opt/prismerp/logs/builds/prismerp-image-build-aws-014-20260630T050924Z.log`
- Source commit: `prism_saas d59e4fd Add dry-run provisioning job executor`
- Runtime result: app services promoted to `aws-014`; DB and Redis remained unchanged.
- Verification: `IMAGE_AWS014_VALIDATION=PASS`, `DEPLOYED_AWS014_RESULT=PASS`, `TESTCO_AFTER_DRY_RUN_RESULT=PASS`.
- Dry-run result: `PJOB-00004` succeeded in dry-run mode; no tenant site was created; `testco` remained `Provisioning Queued`.
