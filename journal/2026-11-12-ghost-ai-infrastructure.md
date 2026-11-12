# Day 12 — GHOST: Building My Own AI Infrastructure

**Date:** November 12, 2026  
**Project:** FRAYTRACE — GHOST  
**Focus:** Linux Administration, DevOps, AI Infrastructure, Automation, System Security  
**Status:** GHOST v2.0 partially operational / Ollama deployment pending

---

## Overview

Today, I made significant progress toward transforming GHOST from a secure Linux server into a self-hosted AI automation platform.

After extensive troubleshooting, I successfully verified that n8n, PostgreSQL and GHOST Agent were running on my physical Realme server.

I also accessed the n8n web interface remotely from my MacBook through an encrypted SSH tunnel over Tailscale.

The final goal is to run automation workflows and a small local language model directly on GHOST, independently of my MacBook.

## 1. GHOST v2.0 — Infrastructure Deployment

The infrastructure uses:

- Ubuntu Server 24.04 LTS.
- PostgreSQL 16 for database storage.
- Python for background automation services.
- Node.js for running n8n.
- n8n for workflow automation.
- systemd for service management and resource limitations.
- SSH and Tailscale for secure remote administration.

### Verified services

| Component | Result |
|---|---|
| Ubuntu Server | Operational |
| SSH / ED25519 | PASS |
| Tailscale remote access | PASS |
| n8n service | ACTIVE |
| GHOST Agent | ACTIVE |
| PostgreSQL 16 | ACTIVE |
| PostgreSQL cluster `ghost` | ONLINE |
| n8n web interface | Accessible through SSH tunnel |
| Ollama | Not installed |
| AI workflows | Execution not fully verified |

The PostgreSQL cluster uses port `5433`.

The main `postgresql.service` showed an inactive state, but the actual cluster service `postgresql@16-ghost` was confirmed active.

## 2. First Remote Access to n8n

One of today's biggest achievements was opening the n8n interface on my MacBook while the application was running on GHOST.

The connection architecture:

**MacBook → Tailscale → SSH Tunnel → GHOST → n8n**

Local browser address:

`http://localhost:15678`

Remote application endpoint:

`127.0.0.1:5678`

This helped me understand how SSH port forwarding works.

The browser runs on my MacBook, but the n8n application runs on the physical Realme server.

Closing the browser does not necessarily stop the application. The server can continue executing activated workflows independently, provided the required services remain running.

## 3. Installation Recovery and Troubleshooting

A large part of the day was dedicated to safely recovering the partially completed GHOST v2.0 installation.

### systemd compatibility checks

The recovery system initially blocked existing systemd units from GHOST v1.0–v1.1.

The diagnostic implementation was improved to inspect:

- Unit files and dependencies.
- Service execution properties.
- File permissions and ownership.
- D-Bus responses.
- Systemd object lifetimes.
- Known recovery services.

The recovery check eventually returned:

`resume_status: PASS`

This allowed the original installation transaction to continue without repeating the initial deployment.

### n8n build permissions

An earlier deployment failure returned:

`203/EXEC`

The cause was traced to restrictive directory permissions created under `umask 077`.

The installation tooling was corrected to allow the dedicated build user to access the Node.js executable without unnecessarily weakening system isolation.

### SSH tunnel verification

The automated final verification later stopped at:

`mac_ui_tunnel exit=255`

I manually tested the local port and SSH forwarding.

The MacBook successfully opened the n8n setup page through the SSH tunnel.

This demonstrated that a failed automated verification does not always mean the underlying application is broken.

## 4. Server Resource Monitoring

I used Linux commands including:

- `free -h`
- `df -h /`
- `top`
- `systemctl`
- `pg_lsclusters`

### Observed system resources

| Metric | Result |
|---|---|
| Physical RAM | Approximately 7.5 GiB |
| RAM used | Approximately 1.2 GiB |
| RAM available | Approximately 6.4 GiB |
| Swap | 4 GiB, unused |
| Root filesystem | 98 GiB |
| Root filesystem used | 13 GiB |
| Root filesystem available | 81 GiB |
| CPU idle | 99.6% |
| Load average | 0.00 / 0.00 / 0.00 |
| Uptime | 1 day, 2 hours, 30 minutes |

These results were captured during an idle period.

They show that the server had substantial spare resources at that moment, but they do not establish performance under AI inference workloads.

## 5. Ollama — Local AI Preparation

I continued preparing the local AI inference component.

The selected model is:

**`qwen2.5:1.5b-instruct-q4_K_M`**

Approximate model characteristics:

- 1.54 billion parameters.
- Quantized Q4_K_M format.
- Approximately 986 MB model download.
- Intended for CPU inference.

The planned Ollama service configuration includes:

- RAM limit: 3 GB.
- CPU quota: 150%.
- Local API: `127.0.0.1:11434`.
- One loaded model at a time.
- Restricted systemd service account.
- Filesystem and network isolation.

### Problems encountered

The Ollama systemd unit originally contained an unsupported parameter:

`ConditionPathIsExecutable`

It was corrected in the prepared installer to:

`ConditionFileIsExecutable`

The executable file was confirmed absent from:

`/opt/ghost-ai/ollama/bin/ollama`

A separate recovery package was prepared and locally tested.

However, the first recovery attempt stopped during the systemd dependency security check.

Unexpected effective dependencies included:

- `tmp.mount`
- `systemd-remount-fs.service`

These may originate from systemd's automatic dependency handling for service isolation settings.

The recovery process correctly stopped without installing Ollama.

**Ollama and the Qwen model are not yet running on GHOST.**

## 6. Security Considerations

The deployment continues to prioritize:

- Key-based SSH authentication.
- Restricted Linux service accounts.
- Localhost-only application APIs.
- Firewall protection.
- Tailscale-based remote access.
- Resource limitations.
- Preservation of existing services and data.
- Guarded installation recovery.

Remaining concerns include:

- Centralized Tailscale ACL/grants not fully verified.
- External encrypted backups not completed.
- Some ACPI/i915 warnings remain unresolved.
- Long-term continuous stability not yet established.
- Final workflow execution and recovery tests remain incomplete.

## 7. What I Learned

Today I gained practical experience with:

- Linux process and resource monitoring.
- PostgreSQL cluster management.
- systemd service states and dependencies.
- SSH local port forwarding.
- Remote web application administration.
- Filesystem permissions.
- Installation recovery and transaction safety.
- AI inference resource planning.

I also learned the importance of distinguishing three different states:

1. Software is installed.
2. The service is running.
3. The application has passed real functional testing.

These are not the same thing.

Most configuration and troubleshooting were performed with AI assistance. I still need to develop the ability to diagnose and manage these systems independently.

## 8. Next Steps

- Complete the guarded Ollama installation.
- Verify a real Qwen model response.
- Measure inference speed and RAM/CPU consumption.
- Test all three n8n workflows.
- Verify PostgreSQL integration with n8n.
- Test backup and restoration procedures.
- Validate application isolation and network exposure.
- Review the centralized Tailscale access policy.
- Complete final GHOST v2.0 documentation.

## Reflection

Today I finally saw the practical result of building my own server.

Instead of only connecting to a Linux terminal, I accessed a real application running on my own hardware.

The system is not finished yet, and the installation process has been more complicated than expected.

However, this experience showed me how infrastructure components interact and why engineering requires careful testing, debugging and recovery planning.

My next goal is to complete GHOST v2.0 and understand the infrastructure well enough to operate it with less AI assistance.

**GHOST v2.0 is partially operational. The local AI deployment remains in progress.**

*Learn. Build. Break. Understand. Secure.*
