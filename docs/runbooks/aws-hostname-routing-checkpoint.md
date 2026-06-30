# AWS Hostname Routing Checkpoint

Date: 2026-06-30
Runtime target: AWS PrismERP dev
Compose project: prism-aws-dev
Current image: prismerp:erpnext-16.15.1-frappe-version-16-aws-014

## Purpose

This checkpoint records the change from fixed-site frontend routing to hostname-based Frappe site routing.

## Previous State

The frontend container used:

    FRAPPE_SITE_NAME_HEADER=aws-dev-erp.localhost

This forced all hostnames to the control site.

## New State

The frontend container now uses:

    FRAPPE_SITE_NAME_HEADER=$host

The generated frontend config now contains:

    server_name $host;
    proxy_set_header X-Frappe-Site-Name $host;
    proxy_set_header Host $host;

## Control Site Compatibility

The physical control site directory remains:

    aws-dev-erp.localhost

Two symlinks were added in the sites volume:

    erp.prismtechco.com -> aws-dev-erp.localhost
    aws-dev-erp.prismtechco.com -> aws-dev-erp.localhost

This allows public/control hostnames to route to the existing control site without renaming the site directory.

## Verification

Local frontend checks:

    erp.prismtechco.com/login -> HTTP 200
    aws-dev-erp.prismtechco.com/login -> HTTP 200
    testco-erp.prismtechco.com/login -> HTTP 404

The testco result is expected because the tenant Frappe site has not been created yet.

## Safety

No tenant site was created during this routing change.

The frontend remains bound to:

    127.0.0.1:8080

## Remaining Work

Before public tenant access works:

1. Create DNS record for testco-erp.prismtechco.com.
2. Add testco-erp.prismtechco.com to Caddy routing.
3. Create the tenant site through the real provisioning executor.
