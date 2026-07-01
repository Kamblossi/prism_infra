# AWS-015 First Tenant and Wildcard TLS Checkpoint

Date: 2026-07-01

## Summary

This checkpoint records the first successful real PrismERP tenant provisioning on the AWS runtime.

The tenant `testco` was provisioned as a real Frappe/ERPNext tenant site, exposed publicly through wildcard DNS and wildcard Caddy TLS, then marked Active in the PrismERP control plane.

## Runtime

Current runtime target:

- Cloud: AWS
- Hostname: ip-172-31-38-199
- Public server IP: 52.209.194.199
- Runtime root: /opt/prismerp
- Compose path: /opt/prismerp/gitops/prism-aws-dev/compose
- Compose project: prism-aws-dev
- Control site: aws-dev-erp.localhost
- Public control URL: https://erp.prismtechco.com
- Dev/fallback URL: https://aws-dev-erp.prismtechco.com

## Image checkpoint

Application services were promoted to:

- prismerp:erpnext-16.15.1-frappe-version-16-aws-015
- Image ID observed during validation: sha256:ac4c53f6cf31a34b445366405559f17886e78ce31309e125e279fb9f944f8366

Application services using aws-015:

- backend
- frontend
- websocket
- queue-short
- queue-long
- scheduler

Database and Redis services were not recreated.

Database and Redis remained on:

- mariadb:11.8
- redis:8.6-alpine

## Storage correction

During the aws-015 image build, the root disk filled because containerd was still using /var/lib/containerd on the root filesystem.

Corrective action completed:

- Docker root: /mnt/prismerp-data/docker
- Containerd data moved to: /mnt/prismerp-data/containerd
- /var/lib/containerd is now a symlink to /mnt/prismerp-data/containerd

Observed result:

- Root disk recovered from approximately 98 percent used to approximately 32 percent used.
- Data disk holds Docker/containerd build/runtime storage.

No docker prune was run.

## Prism SaaS real provisioning executor

aws-015 includes the real tenant provisioning executor.

Runtime checks passed for:

- prism_saas.services.real_provisioning_executor
- Prism Provisioning Job controller real provisioning methods
- create_real_job
- plan_real
- run_real

Real provisioning worker ID:

- prism-saas-real-v1

The executor created the first real tenant site through bench commands.

## First active tenant

Tenant:

- Tenant key: testco
- Tenant name: Test Co
- Primary contact: Tester One
- Primary contact email: testerone@email.com
- Tenant site: testco-erp.prismtechco.com
- Public URL: https://testco-erp.prismtechco.com
- Tenant status after activation: Active

Tenant site apps:

- frappe 16.24.3
- erpnext 16.15.1
- prism_brand 0.0.1

The control-plane tenant record shows apps_installed as:

- erpnext
- prism_brand

The tenant credential file exists under the protected server-side secret path:

- /home/frappe/frappe-bench/sites/.prism_secrets/tenant_initial_credentials/testco-erp.prismtechco.com.json

The credential file is mode 600. Do not print or commit its contents.

## Provisioning job

Real provisioning job:

- Job: PJOB-00010
- Job type: Create Tenant Site
- Status: Succeeded
- Tenant: testco
- Idempotency key: tenant-create-real:testco
- Worker ID: prism-saas-real-v1
- Attempt count: 1

Historical dry-run job retained:

- Job: PJOB-00004
- Status: Succeeded
- Worker ID: prism-saas-dry-run
- Idempotency key: tenant-create:testco

Do not reuse PJOB-00004 for real provisioning.

## Tenant domain

Primary tenant domain:

- Record: TDOM-00002
- Domain: testco-erp.prismtechco.com
- Domain type: PrismERP Subdomain
- Is primary: 1
- Status after activation: Active

## DNS

Cloudflare is the DNS manager for prismtechco.com.

Wildcard DNS was configured so tenant-style subdomains resolve to the AWS PrismERP server.

Observed DNS behavior:

- testco-erp.prismtechco.com resolves to 52.209.194.199
- anotherco-erp.prismtechco.com resolves to 52.209.194.199
- thirdco-erp.prismtechco.com resolves to 52.209.194.199
- erp.prismtechco.com resolves to 52.209.194.199

Existing service subdomains remained protected by exact DNS records:

- riekocloud.prismtechco.com resolved to Cloudflare proxied addresses
- billing.prismtechco.com resolved to Cloudflare proxied addresses

Design note:

- Exact DNS records override the wildcard record.
- Tenant naming uses first-level hostnames such as testco-erp.prismtechco.com.
- Because DNS wildcard labels must be whole labels, the wildcard DNS record is *.prismtechco.com rather than *-erp.prismtechco.com.

## Caddy wildcard TLS

Caddy version observed:

- v2.11.4

Caddy Cloudflare DNS module installed:

- dns.providers.cloudflare
- Module version observed: v0.2.4

Caddy was changed from exact tenant hostnames to wildcard TLS.

Current Caddyfile model:

- Global ACME DNS challenge provider: cloudflare
- Site block: *.prismtechco.com
- Reverse proxy target: 127.0.0.1:8080

Caddy obtained a wildcard certificate successfully for:

- *.prismtechco.com

Observed public routing after wildcard TLS:

- https://erp.prismtechco.com/login returned HTTP 200
- https://testco-erp.prismtechco.com/login returned HTTP 200
- https://anotherco-erp.prismtechco.com/login returned HTTP 404 because no such tenant site exists

The 404 for an unprovisioned tenant is expected and desirable.

## Caddy backups

Caddyfile backups created during the transition:

- /etc/caddy/Caddyfile.pre-testco-20260701T071655Z.bak
- /etc/caddy/Caddyfile.pre-wildcard-20260701T082344Z.bak

Caddy binary backup created before adding the Cloudflare DNS module:

- /opt/prismerp/backups/caddy/caddy-pre-cloudflare-20260701T073535Z

## Backups

Post-activation backups were completed and copied to the host.

Backup root:

- /opt/prismerp/backups/post-testco-activation-20260701T093941Z

Control site backup files:

- control-site/20260701_123942-aws-dev-erp_localhost-database.sql.gz
- control-site/20260701_123942-aws-dev-erp_localhost-files.tar
- control-site/20260701_123942-aws-dev-erp_localhost-private-files.tar
- control-site/20260701_123942-aws-dev-erp_localhost-site_config_backup.json

Testco tenant backup files:

- testco-site/20260701_150945-testco-erp_prismtechco_com-database.sql.gz
- testco-site/20260701_150945-testco-erp_prismtechco_com-files.tar
- testco-site/20260701_150945-testco-erp_prismtechco_com-private-files.tar
- testco-site/20260701_150945-testco-erp_prismtechco_com-site_config_backup.json

## Validation results

Final validation passed:

- Control site public login returned HTTP 200.
- Testco tenant public login returned HTTP 200.
- Unprovisioned tenant returned HTTP 404.
- Testco tenant was marked Active.
- Testco primary domain was marked Active.
- Control site backup completed.
- Testco tenant backup completed.

## Security follow-up

A Cloudflare API token was used by Caddy for DNS-01 wildcard certificate issuance.

Security follow-up still required:

- Rotate the Cloudflare API token.
- Keep /etc/caddy/cloudflare.env mode 600.
- Avoid printing Caddy journal output until the token is rotated, because an earlier journal command displayed the token environment variable.
- After rotating, remove startup environment logging from the Caddy systemd service if still present.

Do not commit Cloudflare tokens, tenant credentials, site_config secrets, database passwords, or backup files.

## Operational notes

For future PrismERP tenant signups using PrismERP subdomains:

1. Do not manually create Cloudflare DNS records per tenant.
2. Do not manually edit Caddy per tenant.
3. Create the tenant record and provisioning job in the control plane.
4. Run or automate the real provisioning executor.
5. Frappe hostname routing serves provisioned tenant sites.
6. Unknown wildcard hostnames return 404 until a matching tenant site exists.

