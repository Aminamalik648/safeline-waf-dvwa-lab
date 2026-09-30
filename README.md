# safeline-waf-dvwa-lab

**A hands-on Web Application Firewall home lab: SafeLine WAF protecting a deliberately vulnerable web app (DVWA), with a full attack/defense demonstration.**

---

## Overview

This project stands up a self-hosted Web Application Firewall (WAF) home lab across two virtual machines — one acting as attacker, one as victim — to practice and demonstrate practical web application security skills: attack simulation, WAF policy configuration, and log-based verification.

**Architecture:**
```
Kali Linux (attacker, GUI)  →  SafeLine WAF (reverse proxy, :443)  →  Apache + DVWA (:8080)
```

SafeLine inspects and filters all traffic before it reaches DVWA, blocking common web attacks (SQL injection, HTTP flood, etc.) while still allowing legitimate traffic through to the application.

## Objectives

- Deploy a vulnerable web application (DVWA) in a controlled environment
- Configure a production-grade WAF (SafeLine) as a reverse proxy in front of it
- Demonstrate SQL injection being exploited, then blocked, with evidence
- Configure and test additional WAF defenses: HTTP flood/rate limiting, authentication gateway

## Tech Stack

| Component | Tool |
|---|---|
| Hypervisor | VMware Workstation |
| Attacker | Kali Linux (GUI) |
| Victim host | Ubuntu Server 22.04 (headless) |
| Web stack | Apache, PHP, MySQL 8.0 |
| Vulnerable app | DVWA (Damn Vulnerable Web Application) |
| WAF | SafeLine WAF 9.4.1 (Docker-based) |
| Encryption | Self-signed SSL/TLS certificate (OpenSSL) |

## Lab Phases

1. **Network Setup** — Kali + Ubuntu on a shared subnet, connectivity verified
2. **LAMP Stack + DVWA** — Apache/PHP/MySQL installed, DVWA deployed on port 8080
3. **DNS Resolution** — `/etc/hosts` mapping for a friendly domain (`dvwa.local`)
4. **Self-Signed SSL Certificate** — generated via OpenSSL for HTTPS
5. **SafeLine WAF Installation** — Docker-based deployment
6. **Certificate Import + DVWA Onboarding** — reverse proxy configuration
7. **SQL Injection Demonstration** — blocked → exploited (protection off) → blocked again
8. **Advanced WAF Configurations** — HTTP flood defense, authentication gateway

## Key Findings & Challenges

- Diagnosed and patched a real **MySQL vs. MariaDB compatibility bug** in DVWA's own setup script (`ADD COLUMN IF NOT EXISTS` — a MariaDB-only clause)
- Traced a **missing `<?php` tag** causing a silent PHP parsing failure
- Solved a **reverse-proxy misconfiguration** where an Apache redirect was inadvertently routing traffic around the WAF entirely
- Diagnosed a **login issue that looked like a session bug** but was resolved to stale browser cookies, using DevTools network tracing, PHP session file inspection, and source-code tracing to systematically rule out each layer

## Result

Demonstrated a fully working WAF deployment with evidenced attack/defense cycles: SQL injection blocked by default, successfully exploited with protection disabled (full user table dumped), then blocked again once protection was restored — plus working HTTP flood rate-limiting and an authentication gateway in front of the application.

## Author

Amina Malik — Cybersecurity student, SOC analyst portfolio project.
