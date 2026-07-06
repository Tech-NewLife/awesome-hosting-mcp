# InterServer MCP

> An MCP (Model Context Protocol) server specification for integrating AI assistants with InterServer hosting services.

**Status:** 🚧 Planned

---

# Overview

InterServer MCP aims to provide a secure interface between AI assistants and the InterServer platform, allowing users to manage hosting services using natural language.

Instead of manually navigating the control panel, users can simply ask an AI assistant to perform common hosting tasks such as restarting a VPS, checking invoices, managing DNS records, or opening support tickets.

---

# Why InterServer MCP?

InterServer offers a wide range of hosting services including:

- VPS
- Windows VPS
- Linux VPS
- Dedicated Servers
- Shared Hosting
- Domains
- Email Hosting
- DNS Management
- Support
- Billing

An MCP server makes these services accessible through AI assistants while maintaining secure authentication and permission controls.

---

# Goals

The InterServer MCP project aims to:

- Simplify server management
- Reduce repetitive administrative tasks
- Provide AI-powered infrastructure management
- Standardize hosting automation
- Demonstrate how hosting providers can adopt MCP

---

# Planned Features

## VPS Management

### Information

- List VPS
- VPS Details
- Resource Usage
- Operating System
- Assigned IP Addresses
- Hostname
- Bandwidth Usage

### Power Operations

- Restart VPS
- Shutdown VPS
- Power On VPS
- Force Reboot

### Administration

- Reset Root Password
- Change Hostname
- Reinstall Operating System
- View Console Information

---

# Dedicated Servers

### Information

- List Servers
- Hardware Specifications
- Public IP Addresses
- Bandwidth Usage

### Power

- Reboot Server
- Power Cycle
- Shutdown

---

# Shared Hosting

### Account

- List Hosting Accounts
- Disk Usage
- Bandwidth Usage
- PHP Version
- SSL Status

### Management

- Suspend Account
- Unsuspend Account
- Reset Password
- View Account Information

---

# Domain Management

Supported operations include:

- List Domains
- Register Domain
- Renew Domain
- Transfer Domain
- Domain Lock
- WHOIS Information
- Expiry Date

---

# DNS Management

Supported record types:

- A
- AAAA
- CNAME
- MX
- TXT
- CAA
- SRV
- PTR

Supported actions:

- List Records
- Create Record
- Edit Record
- Delete Record

---

# Billing

Features planned:

- List Services
- List Invoices
- Outstanding Balance
- Payment History
- Renew Services
- Upgrade Services

---

# Support

Supported features:

- Create Ticket
- View Tickets
- Reply to Ticket
- Close Ticket
- Search Knowledgebase

---

# Email

Planned features:

- List Mailboxes
- Reset Password
- Create Mailbox
- Delete Mailbox
- Forwarders

---

# Authentication

InterServer MCP should support secure authentication using one or more of the following methods:

- API Key
- OAuth (if available)
- Access Tokens
- Session Authentication

No credentials should ever be stored in prompts.

---

# Security

The MCP server should:

- Require authentication before executing actions.
- Use HTTPS for all API communication.
- Respect account permissions.
- Log administrative actions.
- Never expose sensitive credentials.
- Support read-only and administrative modes.

---

# Example Prompts

## VPS

```
Restart my VPS.
```

```
Show my VPS list.
```

```
How much bandwidth has my VPS used this month?
```

```
Reset the root password for my VPS.
```

```
Reinstall Ubuntu 24.04 on my VPS.
```

---

## Dedicated Server

```
Reboot my dedicated server.
```

```
Show hardware information.
```

```
Display server bandwidth usage.
```

---

## Domains

```
List my domains.
```

```
Renew example.com.
```

```
Show domains expiring this month.
```

---

## DNS

```
Create an A record.
```

```
Delete TXT record.
```

```
Update MX records.
```

```
Show DNS records for example.com.
```

---

## Billing

```
Show unpaid invoices.
```

```
List active services.
```

```
Renew my VPS.
```

---

## Support

```
Create a support ticket.
```

```
Show open tickets.
```

```
Reply to ticket #12345.
```

---

# Example Workflow

```
User

↓

Restart my VPS

↓

MCP Client

↓

InterServer MCP

↓

Authenticate

↓

InterServer API

↓

Restart VPS

↓

Success Response

↓

AI Assistant

↓

Your VPS has been restarted successfully.
```

---

# Proposed MCP Tools

## VPS

| Tool | Description |
|------|-------------|
| list_vps | List all VPS |
| get_vps | Show VPS details |
| restart_vps | Restart VPS |
| shutdown_vps | Shutdown VPS |
| poweron_vps | Start VPS |
| reinstall_vps | Reinstall OS |
| reset_password | Reset root password |

---

## DNS

| Tool | Description |
|------|-------------|
| list_dns | List DNS records |
| add_dns_record | Create record |
| update_dns_record | Update record |
| delete_dns_record | Delete record |

---

## Billing

| Tool | Description |
|------|-------------|
| list_services | List services |
| list_invoices | List invoices |
| renew_service | Renew hosting |

---

## Support

| Tool | Description |
|------|-------------|
| list_tickets | View tickets |
| create_ticket | Create ticket |
| reply_ticket | Reply to ticket |

---

# Suggested Directory Structure

```
providers/interserver/

README.md
api.md
installation.md
authentication.md
examples.md
roadmap.md
```

---

# Development Roadmap

## Phase 1

- Research available APIs
- Define MCP tool schema
- Authentication model
- Documentation

## Phase 2

- VPS Management
- Billing
- Support

## Phase 3

- DNS Management
- Domains
- Shared Hosting

## Phase 4

- Dedicated Servers
- Email Management
- Advanced Automation

---

# Contributing

Contributions are welcome.

You can help by:

- Improving documentation
- Adding supported API endpoints
- Creating MCP tool definitions
- Writing installation guides
- Sharing example prompts

Please submit a Pull Request or open an Issue.

---

# Disclaimer

This project is an independent community effort intended to explore MCP integrations for hosting platforms.

InterServer is a trademark of its respective owner. This project is not affiliated with, endorsed by, or maintained by InterServer unless explicitly stated.
