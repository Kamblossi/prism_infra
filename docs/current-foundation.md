# Current PrismERP Foundation

## Azure

- VM name: erp-dev-server
- Region: South Africa North
- OS: Ubuntu 24.04 LTS
- VM size: Standard B2as v2
- Storage: Premium SSD
- User: azureuser

## Cloudflare

Cloudflare is the authoritative DNS and edge layer for prismtechco.com.

The tenant domain strategy uses first-level subdomains such as:

- dev-erp.prismtechco.com
- tenant1-dev-erp.prismtechco.com
- tenantname-erp.prismtechco.com

Nested tenant subdomains such as `tenant1.erp.prismtechco.com` are avoided because they are not covered by Cloudflare Free Universal SSL.

## Security posture

- SSH access is key-based.
- SSH should be restricted to trusted source IPs in Azure NSG.
- Application ports should not be opened until the reverse proxy/SSL layer is ready.
- Secrets must not be committed to Git.
