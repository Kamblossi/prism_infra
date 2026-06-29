# PrismERP AWS Server Notes

## Server role

AWS PrismERP starter server.

## Region

Europe (Ireland), eu-west-1.

## Storage

- Root disk: 50 GiB
- Data disk: 200 GiB mounted at /mnt/prismerp-data
- /opt/prismerp symlinked to /mnt/prismerp-data/prismerp
- Docker data root: /mnt/prismerp-data/docker

## Current policy

- No RDS yet
- No Load Balancer yet
- No NAT Gateway
- No Elastic IP
- No Route 53 hosted zone
- Cloudflare remains DNS/edge later

## 2026-06-05 PrismERP AWS recovery checkpoint

Working public dev URL:
- https://aws-dev-erp.prismtechco.com

Current Frappe internal site:
- aws-dev-erp.localhost

Current public access design:
- Caddy handles public HTTP/HTTPS.
- Frappe frontend is bound to 127.0.0.1:8080.
- Caddy proxies to 127.0.0.1:8080.
- Docker Compose env uses FRAPPE_SITE_NAME_HEADER=aws-dev-erp.localhost.

Backups:
- Manual working-site backup completed.
- Post-HTTPS backup completed.
- Automated daily backup script created:
  /opt/prismerp/gitops/prism-aws-dev/bin/backup-site.sh
- Backup log:
  /opt/prismerp/logs/backups/backup-site.log
- Backup destination:
  /opt/prismerp/backups/aws-dev-erp.localhost
- Retention:
  14 days on host

Recovery documentation:
- /opt/prismerp/RECOVERY_RUNBOOK.md

Do not delete:
- /opt/prismerp/backups
- /opt/prismerp/secrets
- /opt/prismerp/gitops/prism-aws-dev/env/prism-aws-dev.env
- /etc/caddy/Caddyfile

## 2026-06-09 Phase 4A-Final Desk card label fix

The legacy /desk ERPNext Settings card is now renamed visually via prism_brand JavaScript only.

Visible UI:
- ERPNext Settings -> PrismERP Settings

Internal Frappe/ERPNext routing records intentionally remain unchanged:
- tabWorkspace: ERPNext Settings
- tabWorkspace Sidebar: ERPNext Settings
- tabDesktop Icon: ERPNext Settings

Reason:
Renaming those internal records broke the legacy Desk grid and made the card disappear. The final fix is UI-only and bounded.

No ERPNext or Frappe core files were edited.
No database records were renamed as part of the final fix.
No MutationObserver or continuous DOM rewrite loop was introduced.

Commit:
- 9d113c9 Fix Desk card label for PrismERP Settings

## 2026-06-09 GitHub PAT authentication setup

AWS server prism-aws-dev was configured to push PrismERP repositories to GitHub over HTTPS using GitHub CLI credential integration.

Repos intended for this workflow:
- Kamblossi/prism-brand
- Kamblossi/prism_saas
- Kamblossi/prism-erp
- Kamblossi/prism_infra

Security note:
- Do not place PATs in Git remotes.
- Do not paste PATs into commands that are stored in shell history.
- Use `gh auth status` to verify authentication.
- Rotate the PAT if it is exposed or no longer needed.

## Phase 4B Platform outbound email setup

PrismERP platform outbound email has been configured on the AWS site.

Sender identity:
- PrismERP <erp@prismtechco.com>

Provider:
- Zoho Mail

Purpose:
- Platform/system email
- Password resets
- User invitations
- Frappe/ERPNext notifications
- Future platform onboarding messages

Scope:
- This is platform email for the current PrismERP AWS site.
- This is not tenant business email.
- Tenant organisations will configure their own business email accounts later for invoices, quotations, purchase orders, supplier/customer communication, and organisation-specific notifications.

Configuration:
- Outgoing email enabled.
- Incoming email disabled for now.
- Default outgoing enabled.
- Always use account email address as sender enabled.
- SMTP secrets are not stored in SERVER_NOTES.md.

Verification:
- Test email sent successfully.
- Password reset email tested.
- User invitation tested if applicable.
- Email Queue checked.

## Phase 5B Basic security hardening

Basic host hardening was applied after the Phase 5A read-only security audit.

Changes:
- UFW enabled.
- UFW default incoming policy set to deny.
- UFW outgoing policy set to allow.
- UFW allowed inbound ports:
  - 22/tcp for SSH
  - 80/tcp for Caddy HTTP
  - 443/tcp for Caddy HTTPS
- SSH password authentication remains disabled.
- SSH public key authentication remains enabled.
- Direct root SSH login disabled.
- X11 forwarding disabled.
- SSH idle session controls added:
  - ClientAliveInterval 3600
  - ClientAliveCountMax 2
- SSH MaxAuthTries set to 3.
- Caddyfile formatted and validated.
- Caddy reloaded.

No Docker networking changes were made.
No AWS Security Group changes were made.
No secrets were printed or modified.
No GitHub authentication changes were made.

Current expected public entry points:
- 22/tcp SSH
- 80/tcp Caddy HTTP
- 443/tcp Caddy HTTPS

Expected internal-only app binding:
- 127.0.0.1:8080 -> Frappe frontend
## 2026-06-10 Phase 6A AWS cost/resource inventory summary

AWS staging host cost/resource inventory completed.

Observations:
- Instance type: m7i-flex.large
- Host disk: 48G root volume at ~51% used; /mnt/prismerp-data at ~1% used
- /opt/prismerp total size: 15G
- /opt/prismerp/backups: ~9.7M on disk
- Docker build cache and image layers are the main storage driver
- No additional public ports beyond 22/80/443
- App access is routed through Caddy; Docker frontend remains bound to 127.0.0.1:8080

Limitations:
- No AWS Cost Explorer/Billing data collected from this host (AWS CLI/role not available here)
- EBS/infra inventory could not be enumerated remotely without AWS CLI/credentials

No AWS resources were changed.
## Production URL activation

Production public URL activated:
- https://erp.prismtechco.com

Fallback/dev URL retained:
- https://aws-dev-erp.prismtechco.com

Internal Frappe site name retained:
- aws-dev-erp.localhost

Routing:
- DNS points erp.prismtechco.com to AWS EC2 public IP 52.209.194.199.
- DNS points aws-dev-erp.prismtechco.com to AWS EC2 public IP 52.209.194.199.
- Caddy terminates HTTPS for both public hostnames.
- Caddy proxies traffic to 127.0.0.1:8080.
- Frappe continues using the internal site aws-dev-erp.localhost.

Frappe public host_name:
- https://erp.prismtechco.com

Reason:
- Avoided risky internal Frappe site/database rename.
- Production URL is now the canonical public URL for generated links and platform use.
- Existing aws-dev-erp.prismtechco.com remains available as a fallback during transition.

Status:
- DNS resolution verified.
- Caddy configuration updated and validated.
- Caddy reloaded successfully.
- HTTPS verified for production URL.
- HTTPS verified for fallback/dev URL.
- Login page verified.
- Desk verified.
- PrismERP branding verified.
- Email/password-reset link domain should use https://erp.prismtechco.com after host_name update.

Important:
- This is a production URL activation, not a full production launch.
- Off-server backup remains deferred until client/revenue stage.
- SaaS tenant lifecycle, Clerk authentication, and automated tenant provisioning remain future phases.

## 2026-06-12 Prism SaaS Admin Desk – aws-009 checkpoint

Milestone: Prism SaaS Admin Workspace deployed and verified on aws-009.

### Image

- Tag: `prismerp:erpnext-16.15.1-frappe-version-16-aws-009`
- Image ID: `sha256:786a66e75f60674817cd319c73ab75140aa614e1c524fdc2cc1c9b99b618b2e3`
- Base image: `prismerp:erpnext-16.15.1-frappe-version-16-aws-005`
- Source commit: `a800020` (prism_saas main)
- Previous live image: `aws-008`

### Build context

- Fresh deterministic context from `/opt/prismerp/src/prism_saas`
- Excluded `.git`, `__pycache__`, `*.pyc`, stale tarballs
- No stale aws-006/007/008 contexts used

### Changes (vs aws-008)

- Workspace JSON fixed: top-level `"doctype": "Workspace"` added
- Workspace JSON at correct module path:
  `prism_saas/prism_saas/workspace/prism_saas_admin/prism_saas_admin.json`
- Wrong app-root path removed:
  `prism_saas/workspace/prism_saas_admin/` (absent)
- Old workspace-creating patch neutralized (`clear_cache` only)
- New role-assignment patch added:
  `prism_saas.patches.v0_1_0.assign_prism_platform_admin_roles`

### GitOps Compose file updated

- `/opt/prismerp/gitops/prism-aws-dev/compose/docker-compose.yml`

### App services reconciled (running aws-009)

- backend
- frontend
- websocket
- queue-short
- queue-long
- scheduler

### DB/Redis services untouched

- db
- redis-cache
- redis-queue

### Migration result

- Command: `bench --site aws-dev-erp.localhost migrate`
- Result: successful

### Patch log

- `prism_saas.patches.v0_1_0.create_prism_saas_admin_workspace`
- `prism_saas.patches.v0_1_0.assign_prism_platform_admin_roles`

### Prism SaaS Admin Workspace verification

- Workspace exists
- Module: `Prism SaaS`
- Title: `Prism SaaS Admin`
- All 8 shortcut `link_to` targets present:
  - Prism Platform Settings
  - Prism Tenant Plan
  - Prism Tenant Request
  - Prism Tenant
  - Prism Tenant Domain
  - Prism Tenant Member
  - Prism Provisioning Job
  - Prism Tenant Lifecycle Event

### Access verification

- Administrator has `Prism Platform Admin` role
- `newton.ochieng97@gmail.com` has `Prism Platform Admin` role
- `has_app_permission()` allows platform admin
- Guest and non-platform users denied

### Runtime health

- 2 workers online, scheduler active
- Control-plane tenant count: 0 (clean)

### Latest automated backup observed

- `20260627_051502-aws-dev-erp_localhost-database.sql.gz`
- `20260627_051502-aws-dev-erp_localhost-files.tar`
- `20260627_051502-aws-dev-erp_localhost-private-files.tar`
- `20260627_051502-aws-dev-erp_localhost-site_config_backup.json`

### Prohibited actions not performed

- No tenant provisioning
- No tenant sites created
- No manual Workspace creation
- No manual tenant records
- No Caddy/site_config changes
- No volume deletion
- No Docker prune
- No force push
- No README.md modification
