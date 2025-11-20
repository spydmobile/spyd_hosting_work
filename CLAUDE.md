# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a VPS infrastructure management workspace for spyd.com hosted on HostPapa. This directory serves as the working location for managing server configurations, DNS settings, email routing, and documentation related to the VPS infrastructure.

## VPS Environment

**Role**: Subject matter expert on VPS management and infrastructure

**Server Access**:
```bash
ssh spydcom@vwes7374.hostpapavps.net
```

**Key Details**:
- Provider: HostPapa
- Hostname: vwes7374.hostpapavps.net
- Primary IP: 45.56.217.65
- Management Tools: WHM & cPanel
- Multi-domain hosting environment

## Common Operations

### DNS Management

**View DNS zone for a domain**:
```bash
ssh spydcom@vwes7374.hostpapavps.net "uapi --output=json DNS parse_zone zone=spyd.com"
```

**Access cPanel Zone Editor**:
- Log into cPanel
- Navigate to "Zone Editor" or "Advanced Zone Editor"
- Select domain and manage DNS records

### Email Configuration

**Email accounts are managed via**:
- cPanel Email Accounts interface
- UAPI Email module via SSH

**Mail server**: Runs on VPS (45.56.217.65)
- Ports: SMTP (25), Submission (587), IMAP/POP3
- Webmail: Available via cPanel

### Server Information

**Get server IP**:
```bash
ssh spydcom@vwes7374.hostpapavps.net "hostname -I | awk '{print \$1}'"
```

**Check domain configuration**:
```bash
ssh spydcom@vwes7374.hostpapavps.net "uapi --output=json DomainInfo domains_data domain=spyd.com"
```

## Architecture: Split Hosting Setup

**spyd.com operates on a split hosting architecture**:

- **Website**: GitHub Pages (185.199.108-111.153)
- **Email**: VPS mail server (45.56.217.65)
- **DNS**: Managed in cPanel/WHM

This split requires careful DNS configuration:
- Main domain A records point to GitHub Pages for website
- Dedicated `mail.domain.com` A record points to VPS
- MX records point to `mail.domain.com` (not main domain)

**Critical DNS pattern for split hosting**:
```
domain.com        A      <github-pages-ip>     # Website
mail.domain.com   A      <vps-ip>              # Mail server
domain.com        MX     0 mail.domain.com.    # Mail routing
```

**Never**:
- Point MX records to domains using external hosting (GitHub Pages, Netlify, etc.)
- Use CNAMEs for mail subdomains
- Forget to include VPS IP in SPF records

## API Access

The cPanel UAPI is available via SSH for automation:

**Available modules**:
- `DNS`: parse_zone, mass_edit_zone
- `Email`: Account management
- `DomainInfo`: Domain configuration queries

**Example**:
```bash
uapi --output=json DNS parse_zone zone=example.com
```

## Documentation

**INFRASTRUCTURE.md**: Contains detailed VPS configuration, DNS setup, email routing solutions, and historical issues/resolutions. Reference this file for:
- Complete DNS record configurations
- Email routing troubleshooting
- SPF/authentication setup
- Multi-domain hosting patterns

## Multi-Domain Context

The VPS hosts multiple domains beyond spyd.com. When making changes:
- Verify which domain you're modifying
- Check for dependencies between domains
- Consider impact on other hosted sites
- Each domain may have different hosting arrangements (some fully on VPS, others split like spyd.com)
