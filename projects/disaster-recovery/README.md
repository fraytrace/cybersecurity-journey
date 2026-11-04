
# Disaster Recovery — Production Infrastructure

**Date:** November 4, 2026  
**Category:** Infrastructure Security / Disaster Recovery  
**Status:** Recovery Completed

## Overview

A production web application became unavailable following a cyberattack affecting its hosting provider's infrastructure.

I participated in the recovery and migration of the application to a new VPS environment, working through network connectivity issues, database recovery, service configuration, and payment integration.

The objective was to restore the production service while preserving data integrity and minimizing further operational risks.

## Technology Stack

- **Operating System:** Ubuntu Server 24.04 LTS
- **Web Server:** Nginx
- **Runtime:** Node.js 24 / Next.js
- **Database:** PostgreSQL 16
- **Security:** SSH, UFW, TLS, Let's Encrypt
- **Networking:** DNS, TCP/IP, HTTP/2
- **Deployment:** Linux, systemd
- **Backup:** Encrypted PostgreSQL backups

## Recovery Process

### 1. Incident Assessment

- Investigated the infrastructure outage.
- Identified unavailable hosting services.
- Reviewed existing database backups.
- Evaluated alternative hosting environments.

### 2. Infrastructure Migration

- Provisioned a new Ubuntu VPS.
- Configured SSH access and firewall rules.
- Installed the application runtime and database.
- Deployed and verified the production application.

### 3. Network Troubleshooting

Investigated several connectivity problems involving:

- SSH connection timeouts
- DNS resolution
- TCP connectivity
- VPN routing
- TLS certificate validation
- External payment gateway connectivity

### 4. Database and Application Recovery

- Verified encrypted backup integrity.
- Tested database restoration in an isolated environment.
- Applied database migrations.
- Reconfigured application services.
- Tested booking capacity and concurrent reservations.

### 5. Production Validation

- Configured HTTPS and HTTP/2.
- Updated DNS records.
- Verified application health endpoints.
- Tested authentication and authorization.
- Validated payment integration and booking workflows.
- Confirmed a successful real payment and booking.

## Challenges

The most significant challenges included:

1. Network failures in the initial VPS environment.
2. Unreachable payment gateway endpoints from another provider.
3. Incomplete historical booking data.
4. SSH connectivity issues involving VPN routing.
5. Preventing overbooking during concurrent reservations.

## Results

**Production service successfully restored.**

- Public website operational
- HTTPS and HTTP/2 enabled
- Database services operational
- Online booking restored
- Payment processing restored
- First successful payment verified
- Encrypted backups configured

## Lessons Learned

- Backups must be stored outside the production server.
- Backup restoration should be tested regularly.
- TCP connectivity should be verified before application deployment.
- Payment gateways require independent network and TLS testing.
- Disaster recovery plans should include alternative hosting providers.
- Monitoring and rollback procedures are essential.

## Security Note

This case study intentionally excludes credentials, private keys, customer information, payment identifiers, infrastructure addresses, and proprietary application code.

---

**Personal takeaway:** Infrastructure security is not only about preventing incidents. It is also about restoring systems safely when incidents happen.
