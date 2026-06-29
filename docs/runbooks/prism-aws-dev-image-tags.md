Prism AWS Dev Image Tags
Current runtime image
As of 2026-06-29, the PrismERP AWS development/runtime stack uses:
prismerp:erpnext-16.15.1-frappe-version-16-aws-012
Image ID:
f1879590e06a
The aws-012 image contains the verified Prism SaaS Admin Workspace route, permissions, roles,
Frappe v16 Workspace content layout, and manual tenant request conversion workflow.
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
Current app services on aws-012
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
b5d24f8 Add manual tenant request conversion workflow
Latest checkpoint
See:
docs/runbooks/aws-012-checkpoint.md
