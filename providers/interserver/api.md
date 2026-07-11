# InterServer MCP API

> API reference and MCP mapping based on the official InterServer Management API.

**API Version:** `0.9.0`  
**OpenAPI Version:** `3.0.3`

---

# Official Documentation

InterServer provides a modern REST API documented using OpenAPI.

- Interactive API documentation: `https://my.interserver.net/api-docs/elements.html#/`
- OpenAPI JSON specification: `https://my.interserver.net/spec/openapi.json`
- OpenAPI YAML specification: `https://my.interserver.net/spec/openapi.yaml`

---

# Base URL

All REST API requests use the following base URL:

```text
https://my.interserver.net/apiv2
```

Example:

```text
https://my.interserver.net/apiv2/vps
```

---

# Authentication

Most API endpoints require authentication.

InterServer supports two main authentication methods.

## API Key

API-key authentication is the preferred method for MCP integrations.

Generate an API key from the **Account Security** page in the InterServer customer portal.

Send the key using the following request header:

```http
X-API-KEY: YOUR_API_KEY
```

Example:

```bash
curl \
  -H "X-API-KEY: YOUR_API_KEY" \
  https://my.interserver.net/apiv2/vps
```

## Session Authentication

Session authentication can also be used.

First, submit account credentials to:

```http
POST /login
```

The returned session identifier must then be included in future requests:

```http
sessionid: YOUR_SESSION_ID
```

API-key authentication is generally more suitable for MCP servers because it avoids repeated login and session-management steps.

---

# Supported API Categories

The official InterServer API includes support for:

- Account Management
- Billing
- Backups
- DNS
- Domains
- Floating IPs
- Licenses
- Mail
- QuickServers
- Dedicated Servers
- SSL Certificates
- VPS
- Web Hosting
- Support Tickets
- Scrub IPs and DDoS Protection

---

# Account API

## Get Account Information

```http
GET /account
```

Returns account-profile, contact, billing-address, locale, timezone, security, and feature-setting information.

### MCP Tool

```text
get_account_info
```

---

## Update Account Information

```http
POST /account
```

Updates supported account-profile and contact fields.

### MCP Tool

```text
update_account_info
```

---

## Rotate API Key

```http
POST /account/apikey
```

Generates a new API key and immediately invalidates the previous key.

### MCP Tool

```text
rotate_api_key
```

> This is a sensitive operation. The new key should be stored in a secure secrets manager and must never be placed in an AI prompt.

---

## Manage Account SSH Key

```http
POST /account/sshkey
```

Stores the account-level SSH public key that can be installed on future VPS or server orders.

### MCP Tool

```text
update_account_ssh_key
```

---

## Account Safety Features

```http
POST /account/features
```

Supports account-wide controls such as:

- Disabling root-password resets
- Disabling operating-system reinstalls

### MCP Tool

```text
update_account_safety_features
```

---

# VPS API

## List VPS Instances

```http
GET /vps
```

Returns the VPS services associated with the authenticated account.

### MCP Tool

```text
list_vps
```

Example:

```bash
curl \
  -H "X-API-KEY: YOUR_API_KEY" \
  https://my.interserver.net/apiv2/vps
```

---

## Get VPS Information

```http
GET /vps/{id}
```

Returns detailed information about a specific VPS.

### MCP Tool

```text
get_vps
```

Example input:

```json
{
  "id": 12345
}
```

---

## Get VPS Ordering Options

```http
GET /vps/order
```

Returns available:

- Platforms
- Operating-system templates
- Locations
- Slice options
- Pricing

### MCP Tool

```text
get_vps_order_options
```

---

## Start VPS

```http
GET /vps/{id}/start
```

Powers on a stopped VPS.

### MCP Tool

```text
start_vps
```

---

## Stop VPS

```http
GET /vps/{id}/stop
```

Powers off a running VPS.

Stopping a VPS does not cancel billing.

### MCP Tool

```text
stop_vps
```

---

## Restart VPS

```http
GET /vps/{id}/restart
```

Queues a restart action for the VPS.

### MCP Tool

```text
restart_vps
```

---

## Reinstall VPS Operating System

```http
GET /vps/{id}/reinstall_os
```

Returns compatible operating-system templates.

The corresponding submission endpoint can then be used to perform the reinstall.

### MCP Tools

```text
list_vps_reinstall_templates
reinstall_vps
```

> Reinstallation is destructive and must require explicit user confirmation.

---

## VPS Root Password

The API supports VPS root-password management.

### MCP Tools

```text
generate_vps_root_password
change_vps_root_password
```

> Password values must never be shown in logs or stored in prompts.

---

## VPS Bandwidth Usage

```http
GET /vps/{id}/traffic_usage
```

Returns bandwidth-consumption information for the VPS.

### MCP Tool

```text
get_vps_traffic_usage
```

---

## VPS Console

```http
GET /vps/{id}/setup_vnc
```

Returns VNC console information for supported VPS instances.

### MCP Tool

```text
get_vps_console
```

---

## VPS ISO Management

```http
GET /vps/{id}/insert_cd
POST /vps/{id}/insert_cd
```

Lists available ISO images and allows an ISO to be mounted in the virtual CD drive.

### MCP Tools

```text
list_vps_iso_images
mount_vps_iso
eject_vps_iso
```

---

## VPS Invoices

```http
GET /vps/{id}/invoices
```

Returns billing invoices associated with a specific VPS.

### MCP Tool

```text
get_vps_invoices
```

---

## VPS Slice Management

```http
GET /vps/{id}/slices
```

Returns the current slice count, supported range, and pricing information.

### MCP Tools

```text
get_vps_slices
update_vps_slices
```

---

# Dedicated Server API

## List Dedicated Servers

```http
GET /servers
```

Returns all dedicated servers owned by the authenticated account.

### MCP Tool

```text
list_dedicated_servers
```

---

## Get Dedicated Server Information

```http
GET /servers/{id}
```

Returns detailed hardware, network, service-status, and management information.

### MCP Tool

```text
get_dedicated_server
```

---

## Get Dedicated Server Order Options

```http
GET /servers/order
```

Returns available:

- Processors
- Memory options
- Drives
- RAID options
- Operating systems
- Control panels
- Bandwidth
- IP allocations
- Regions
- Pricing

### MCP Tool

```text
get_server_order_options
```

---

## Dedicated Server Invoices

```http
GET /servers/{id}/invoices
```

Returns invoices linked to a specific dedicated server.

### MCP Tool

```text
get_server_invoices
```

---

## Dedicated Server Power Management

The API includes IPMI-related server power operations.

### Proposed MCP Tools

```text
get_server_power_status
power_on_server
power_off_server
restart_server
power_cycle_server
```

> Power operations must require confirmation before execution.

---

# QuickServers API

QuickServers are rapidly deployable preconfigured dedicated servers.

Supported operations include:

- Listing QuickServers
- Ordering services
- Viewing service details
- Operating-system reinstall
- Backup management
- VNC access
- Invoice access

### MCP Tools

```text
list_quickservers
get_quickserver
order_quickserver
restart_quickserver
reinstall_quickserver
get_quickserver_vnc
get_quickserver_backups
```

---

# Web Hosting API

## List Hosting Services

```http
GET /webhosting
```

Returns shared and reseller hosting services associated with the account.

### MCP Tool

```text
list_webhosting_accounts
```

---

## Get Hosting Information

```http
GET /webhosting/{id}
```

Returns information about a hosting service.

### MCP Tool

```text
get_webhosting_account
```

---

## Web Hosting Ordering

The web-hosting order process includes:

1. Retrieving package and order options
2. Validating the order
3. Placing the order
4. Paying the generated invoice
5. Waiting for provisioning

### MCP Tools

```text
get_webhosting_order_options
validate_webhosting_order
order_webhosting
```

---

## Hosting Control Panel and Welcome Information

The API supports access to hosting account information and welcome details.

### MCP Tools

```text
get_hosting_control_panel
get_hosting_welcome_email
```

---

## Hosting Backups

The API includes hosting backup-management capabilities.

### MCP Tools

```text
list_hosting_backups
restore_hosting_backup
```

> Restoring a backup is destructive and must require confirmation.

---

# Domain API

## List Domains

```http
GET /domains
```

Returns domain services owned by the account.

### MCP Tool

```text
list_domains
```

---

## Get Domain Information

```http
GET /domains/{id}
```

Returns detailed information for a domain service.

### MCP Tool

```text
get_domain
```

---

## Register or Transfer a Domain

The domain-order workflow supports:

- Domain availability checks
- Registration
- Transfers
- Pricing
- Validation
- Invoice creation

### MCP Tools

```text
check_domain_availability
get_domain_order_options
register_domain
transfer_domain
```

---

## Domain Renewal

```http
GET /domains/{id}/renew
```

Returns renewal pricing, expiry information, and invoice status.

The renewal submission endpoint can then create or process the renewal.

### MCP Tools

```text
get_domain_renewal_info
renew_domain
```

---

## Domain Invoices

```http
GET /domains/{id}/invoices
```

Returns invoices associated with a domain.

### MCP Tool

```text
get_domain_invoices
```

---

## Nameservers

The API supports viewing and updating domain nameservers.

### MCP Tools

```text
get_domain_nameservers
update_domain_nameservers
```

---

## WHOIS and Privacy

Supported domain-management capabilities include:

- WHOIS information
- WHOIS contact updates
- WHOIS privacy
- Domain locking
- DNSSEC configuration

### MCP Tools

```text
get_domain_whois
update_domain_whois
update_whois_privacy
lock_domain
unlock_domain
get_domain_dnssec
update_domain_dnssec
```

---

# DNS API

## List DNS Zones

```http
GET /dns
```

Returns DNS zones associated with the account.

### MCP Tool

```text
list_dns_zones
```

---

## Add DNS Zone

```http
POST /dns
```

Creates a DNS zone.

### MCP Tool

```text
create_dns_zone
```

---

## Get DNS Zone Records

```http
GET /dns/{id}
```

Returns the records in a DNS zone.

### MCP Tool

```text
list_dns_records
```

---

## Add DNS Record

The DNS API supports creating records such as:

- A
- AAAA
- CNAME
- MX
- TXT
- CAA
- SRV

### MCP Tool

```text
create_dns_record
```

Example input:

```json
{
  "domainId": 472,
  "name": "www",
  "type": "A",
  "content": "192.0.2.10",
  "ttl": 3600
}
```

---

## Update DNS Record

```http
POST /dns/{domainId}/{recordId}
```

Updates an existing record's:

- Name
- Type
- Content
- TTL
- Priority

### MCP Tool

```text
update_dns_record
```

---

## Delete DNS Record

```http
DELETE /dns/{domainId}/{recordId}
```

Deletes a DNS record.

### MCP Tool

```text
delete_dns_record
```

> DNS changes can make websites or email services unavailable. Write operations should require explicit confirmation.

---

# Billing API

The billing API supports:

- Listing invoices
- Viewing individual invoices
- Viewing unpaid invoices
- Shopping-cart management
- Prepay balances
- Payment methods
- Processing invoice payments
- Affiliate reporting

### MCP Tools

```text
list_invoices
get_invoice
list_unpaid_invoices
get_account_balance
get_payment_methods
pay_invoice
list_cart_items
```

Example payment path:

```http
GET /billing/pay/{method}/{invoices}
```

> MCP implementations should never collect or expose complete payment-card details.

---

# Support Ticket API

## List Tickets

```http
GET /tickets
```

Returns support tickets and supports filtering by:

- Status
- Date period
- Page

### MCP Tool

```text
list_tickets
```

---

## Search Tickets

```http
POST /tickets
```

Searches tickets by information such as:

- Subject
- Email
- Ticket mask ID

### MCP Tool

```text
search_tickets
```

---

## New Ticket Options

```http
GET /tickets/new
```

Returns service and department options for creating a ticket.

### MCP Tool

```text
get_ticket_options
```

---

## Create Ticket

The ticket API supports opening a new support request.

### MCP Tool

```text
create_ticket
```

---

## View and Reply to Ticket

Supported actions include:

- Reading ticket details
- Viewing reply history
- Posting a reply
- Closing a resolved ticket

### MCP Tools

```text
get_ticket
reply_ticket
close_ticket
```

---

# Mail API

The Mail API covers InterServer Mail Baby services.

Capabilities include:

- Listing mail services
- Ordering mail services
- Managing delivery settings
- Viewing delivery logs
- Configuring alerts
- Configuring deny rules
- Monitoring delivery performance
- Accessing invoices

### MCP Tools

```text
list_mail_services
get_mail_service
get_mail_logs
get_mail_statistics
create_mail_deny_rule
delete_mail_deny_rule
update_mail_alerts
get_mail_invoices
```

---

# Backup API

The backup API supports cloud-storage and backup services.

Capabilities include:

- Listing backup services
- Ordering backup storage
- Viewing quota and service information
- Managing credentials
- Viewing invoices
- Cancelling backup services

### MCP Tools

```text
list_backup_services
get_backup_service
order_backup_service
get_backup_credentials
get_backup_usage
get_backup_invoices
cancel_backup_service
```

---

# SSL Certificate API

The SSL API supports:

- Listing certificate services
- Ordering certificates
- Viewing certificate information
- Renewing certificates
- Accessing invoices
- Managing certificate lifecycle

### MCP Tools

```text
list_ssl_certificates
get_ssl_certificate
order_ssl_certificate
renew_ssl_certificate
get_ssl_invoices
```

---

# License API

The license API supports software-license provisioning for products such as hosting control panels and related software.

Capabilities include:

- Listing licenses
- Ordering licenses
- Viewing license details
- Changing assigned IP addresses
- Viewing invoices
- Cancelling licenses

### MCP Tools

```text
list_licenses
get_license
order_license
change_license_ip
get_license_invoices
cancel_license
```

---

# Floating IP API

The API supports portable IP-address services for failover and high-availability use cases.

### MCP Tools

```text
list_floating_ips
get_floating_ip
order_floating_ip
update_floating_ip_target
get_floating_ip_invoices
cancel_floating_ip
```

---

# DDoS Scrub IP API

InterServer provides Scrub IP and DDoS-protection management through the API.

Capabilities include:

- Listing protected IP services
- Enabling scrubbing
- Managing firewall rules
- Geographic filtering
- Viewing traffic logs
- Accessing invoices

### MCP Tools

```text
list_scrub_ips
get_scrub_ip
enable_scrubbing
create_scrub_rule
delete_scrub_rule
update_geo_filter
get_scrub_traffic_logs
```

---

# Example MCP Tool Mapping

| MCP Tool | InterServer API |
|---|---|
| `list_vps` | `GET /vps` |
| `get_vps` | `GET /vps/{id}` |
| `restart_vps` | `GET /vps/{id}/restart` |
| `start_vps` | `GET /vps/{id}/start` |
| `stop_vps` | `GET /vps/{id}/stop` |
| `get_vps_traffic_usage` | `GET /vps/{id}/traffic_usage` |
| `list_dedicated_servers` | `GET /servers` |
| `list_domains` | `GET /domains` |
| `get_domain_renewal_info` | `GET /domains/{id}/renew` |
| `list_dns_zones` | `GET /dns` |
| `update_dns_record` | `POST /dns/{domainId}/{recordId}` |
| `list_webhosting_accounts` | `GET /webhosting` |
| `list_tickets` | `GET /tickets` |
| `search_tickets` | `POST /tickets` |
| `get_ticket_options` | `GET /tickets/new` |
| `get_account_info` | `GET /account` |
| `rotate_api_key` | `POST /account/apikey` |

---

# MCP Safety Requirements

## Read-Only Tools

Read operations may execute after authentication without additional confirmation.

Examples:

- Listing VPS instances
- Viewing invoices
- Viewing DNS records
- Reading ticket history
- Checking bandwidth usage

## Confirmation-Required Tools

The following operations should require clear confirmation:

- Restarting or stopping a server
- Reinstalling an operating system
- Resetting a root password
- Deleting a DNS record
- Changing nameservers
- Renewing or purchasing services
- Paying an invoice
- Cancelling a service
- Rotating an API key
- Restoring a backup

## Sensitive Data

The MCP server must never expose or store:

- API keys
- Session identifiers
- Passwords
- Private SSH keys
- Full payment-card details
- Root credentials

---

# Error Handling

The API uses standard HTTP response codes.

Common responses include:

| Status | Meaning |
|---|---|
| `200` | Request completed successfully |
| `400` | Invalid request |
| `401` | Authentication failed or missing |
| `404` | Service or resource not found |
| `409` | Resource state prevents the requested action |
| `422` | Validation error |

Example error:

```json
{
  "success": false,
  "text": "Invalid VPS Passed"
}
```

---

# Rate Limiting and Retries

The MCP server should:

- Avoid unnecessary repeated requests
- Retry only temporary network or server failures
- Never automatically retry destructive operations
- Use request timeouts
- Log operation IDs and timestamps
- Apply its own per-user rate limits

---

# Implementation Notes

- Treat the OpenAPI specification as the source of truth.
- Use the documented HTTP method exactly, even where an action uses `GET`.
- Validate account ownership before exposing a resource to the AI client.
- Store credentials only in environment variables or a secrets manager.
- Redact secrets and passwords from logs.
- Add confirmation prompts before destructive operations.
- Prefer API-key authentication for long-running MCP integrations.
- Keep the generated client synchronized with the official OpenAPI specification.

---

# Disclaimer

This document maps the official InterServer Management API to possible MCP tools.

Available endpoints, request fields, and responses may change. Always verify implementation details against the current official InterServer OpenAPI documentation before developing or deploying an integration.

InterServer is a trademark of its respective owner.
