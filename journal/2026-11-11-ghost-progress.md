# Day 11 — GHOST: From Linux Server to AI Infrastructure

**Date:** November 11, 2026  
**Project:** FRAYTRACE — GHOST  
**Focus:** Linux Administration, Network Security, Remote Access, AI Infrastructure  
**Status:** GHOST v1.1 operational / GHOST v2.0 in progress

---

## Overview

Today, I continued developing GHOST, my first physical Linux server.

My goal was to turn an ordinary laptop into a secure, remotely accessible server capable of running AI agents and automation workflows.

The day involved configuring secure remote access, testing the infrastructure, preparing an AI automation platform, and troubleshooting installation errors.

## 1. GHOST v1.0 — Server Stabilization

Completed the initial infrastructure stabilization.

**Hardware:**
- Intel Core i3-1115G4
- 8 GB RAM
- 256 GB SSD

**Operating system:** Ubuntu Server 24.04 LTS

**Completed:**
- Updated 34 Ubuntu packages.
- Updated the Linux kernel.
- Configured SSH authentication using ED25519.
- Disabled SSH password authentication and root login.
- Configured UFW firewall.
- Enabled automatic security updates.
- Configured monitoring and local backups.
- Verified Wi-Fi reconnection after reboot.
- Verified remote management with the laptop lid closed.

**Verification results:**
- 30/30 successful SSH logins.
- 200/200 network responses without packet loss.
- SSD SMART diagnostics passed.
- System services started successfully after reboot.

Some ACPI/i915 warnings and Wi-Fi latency issues remain unresolved.

## 2. GHOST v1.1 — Secure Remote Access

Configured Tailscale to access GHOST from outside my home network.

**What I accomplished:**
- Installed Tailscale on MacBook and GHOST.
- Connected both devices to the same private tailnet.
- Resolved Tailscale authentication problems.
- Verified encrypted connectivity between devices.
- Successfully connected to GHOST using SSH over Tailscale.
- Tested SSH access using my iPhone's mobile internet instead of home Wi-Fi.

**Result:** Remote SSH access from another network — PASS.

The centralized Tailscale access policy remains unverified. A security audit also found that the intended MacBook-only restriction cannot be considered enforced solely by UFW.

## 3. GHOST v2.0 — AI Automation Infrastructure

Started preparing an AI automation environment using:

- PostgreSQL — database storage.
- Python — scripts and automation agents.
- n8n — workflow automation.
- Ollama — optional small local AI model.
- systemd — service management and resource controls.

Prepared three example workflows:

1. GHOST Health Monitor.
2. Security Log Analyzer.
3. AI Task Processor.

The services are designed to expose application interfaces only through localhost and authenticated SSH tunnels.

## 4. Installation and Troubleshooting

The AI infrastructure deployment encountered several technical problems.

**Preflight issues:**

Some legitimate system network sockets were incorrectly classified as unsafe because of interface suffixes such as `%lo` and `%tailscale0`.

The validation logic was improved to inspect process ownership, executable paths, services and socket metadata.

**macOS launcher issue:**

The temporary-log filename template was incompatible with macOS `mktemp` behavior.

This was corrected, and the launcher subsequently passed its preliminary checks.

**n8n installation failure:**

The installation stopped while starting `ghost-n8n-build.service` with systemd exit status `203/EXEC`.

A permission defect was identified: the Node.js directory was created with mode `0700`, preventing the dedicated build user from traversing the directory.

An installer fix and guarded recovery process were prepared.

**Partial installation recovery:**

The recovery process detected existing systemd services and configurations from GHOST v1.0–v1.1 and stopped to avoid modifying unknown state.

Fourteen existing systemd objects were identified for review.

No automatic deletion or unsafe overwrite was performed.

## 5. What I Learned

Today I gained practical exposure to:

- SSH and public-key authentication.
- VPN networking with Tailscale.
- Linux firewalls and network interfaces.
- TCP/UDP socket inspection.
- systemd services, permissions and execution failures.
- Ubuntu package management.
- Safe installation recovery.
- Infrastructure testing and documentation.

Most troubleshooting and implementation were performed with AI assistance.

I can follow and reproduce many commands, but I still need to improve my independent understanding of Linux administration, networking and system security.

## 6. Current Project Status

| Component | Status |
|---|---|
| Ubuntu Server | Operational |
| Home Wi-Fi | Working |
| SSH / ED25519 | Working |
| Tailscale remote access | Tested successfully |
| UFW | Active; VPN access restrictions require review |
| Monitoring | Operational |
| Local backups | Available |
| PostgreSQL / Python for v2.0 | Partially installed; final verification pending |
| n8n | Installation incomplete |
| Ollama | Not deployed |
| AI workflows | Prepared, not running |

## 7. Next Steps

- Review the 14 systemd objects blocking installation recovery.
- Complete safe installation of n8n.
- Verify PostgreSQL, Python and Node.js.
- Test actual workflow execution.
- Validate isolation and resource usage.
- Review centralized Tailscale ACL/grants.
- Set up encrypted backups on separate storage.
- Deploy a small local AI model only if server resources permit.

## Reflection

This was one of my most technically demanding days.

I encountered networking issues, authentication failures, restrictive preflight checks, filesystem permission problems and incomplete installation states.

The most important lesson was that infrastructure engineering is not simply installing software.

It requires understanding how systems interact, diagnosing failures and protecting working services while making changes.

I still rely heavily on AI guidance, but each problem gives me an opportunity to understand the technologies more deeply.

**GHOST v1.1 is operational. GHOST v2.0 is under construction.**

*Learn. Build. Break. Understand. Secure.*
