# Scaleway MCP

> An MCP (Model Context Protocol) server specification for integrating AI assistants with the Scaleway European Cloud Platform.

**Status:** 🚧 Planned

---

# Overview

Scaleway MCP aims to provide a secure interface between AI assistants and the Scaleway platform, allowing users to manage cloud infrastructure using natural language.

Instead of manually navigating the Scaleway Console, users can simply ask an AI assistant to deploy cloud resources, manage networking, monitor infrastructure, and automate common administrative tasks.

---

# Why Scaleway MCP?

Scaleway provides a European cloud platform offering:

- Compute Instances
- Bare Metal Servers
- Kubernetes (Kapsule)
- Serverless Functions
- Object Storage
- Block Storage
- Load Balancers
- Private Networks
- DNS
- Managed Databases

An MCP server makes these services accessible through AI assistants while maintaining secure authentication and permission controls.

---

# Goals

The Scaleway MCP project aims to:

- Simplify cloud infrastructure management
- Reduce repetitive administrative tasks
- Enable AI-powered cloud automation
- Standardize cloud operations using MCP

---

# Planned Features

## Compute

### Information

- List Instances
- Instance Details
- CPU Usage
- Memory Usage
- Storage Usage
- Public & Private IP Addresses

### Management

- Create Instance
- Restart Instance
- Shutdown Instance
- Delete Instance
- Resize Instance

---

## Kubernetes

- List Clusters
- Create Cluster
- Delete Cluster
- Manage Node Pools

---

## Networking

- Private Networks
- Load Balancers
- Public IPs
- DNS Management

---

## Storage

- Object Storage
- Block Storage
- Snapshots
- Backups

---

## Managed Databases

- List Databases
- Create Database
- Resize Database Cluster
- Backups

---

# Authentication

Scaleway MCP should support secure authentication using:

- API Key
- Access Token

---

# Security

The MCP server should:

- Require authentication
- Use HTTPS
- Respect account permissions
- Log administrative actions
- Support read-only mode

---

# Example Prompts

## Compute

```
Show my cloud instances.
```

```
Deploy a new Ubuntu instance.
```

```
Restart my instance.
```

---

## Kubernetes

```
Show my Kubernetes clusters.
```

```
Create a Kubernetes cluster.
```

---

## DNS

```
Create an A record.
```

```
Show my DNS records.
```

---

# Example Workflow

```
User

↓

Deploy Ubuntu 24.04

↓

MCP Client

↓

Scaleway MCP

↓

Authenticate

↓

Scaleway API

↓

Deploy Instance

↓

Success Response

↓

AI Assistant

↓

Your cloud instance has been deployed successfully.
```

---

# Proposed MCP Tools

## Compute

| Tool | Description |
|------|-------------|
| list_instances | List cloud instances |
| get_instance | Show instance details |
| create_instance | Create cloud instance |
| restart_instance | Restart instance |
| shutdown_instance | Shutdown instance |
| delete_instance | Delete instance |

---

## Kubernetes

| Tool | Description |
|------|-------------|
| list_clusters | List Kubernetes clusters |
| create_cluster | Create cluster |
| delete_cluster | Delete cluster |

---

## Networking

| Tool | Description |
|------|-------------|
| list_dns | List DNS records |
| add_dns_record | Create DNS record |
| update_dns_record | Update DNS record |
| delete_dns_record | Delete DNS record |
| list_load_balancers | List load balancers |

---

## Storage

| Tool | Description |
|------|-------------|
| list_object_storage | List object storage buckets |
| create_bucket | Create object storage bucket |
| list_snapshots | List snapshots |
| list_backups | List backups |

---

# Suggested Directory Structure

```text
providers/scaleway/

README.md
api.md
authentication.md
examples.md
roadmap.md
```

---

# Development Roadmap

## Phase 1

- Research APIs
- Authentication
- Documentation

## Phase 2

- Compute
- Networking
- Storage

## Phase 3

- Kubernetes
- Managed Databases
- Advanced Automation

---

# Contributing

Contributions are welcome.

Please submit a Pull Request or open an Issue.

---

# Disclaimer

This project is an independent community effort intended to explore MCP integrations for hosting platforms.

Scaleway is a trademark of its respective owner. This project is not affiliated with, endorsed by, or maintained by Scaleway unless explicitly stated.
