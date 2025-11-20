# Infrastructure Documentation - spyd.com

## VPS Details

**Provider**: HostPapa
**Hostname**: vwes7374.hostpapavps.net
**Primary IP**: 45.56.217.65
**Management**: WHM & cPanel
**SSH Access**: `ssh spydcom@vwes7374.hostpapavps.net`

## Domain Configuration

### Primary Domain: spyd.com

**Hosting Setup**:
- Website: GitHub Pages (185.199.108-111.153)
- Email: VPS mail server (45.56.217.65)
- DNS Management: cPanel/WHM

## Email Routing Configuration

### Problem (November 2025)
After migrating spyd.com website to GitHub Pages, email routing broke because:
- MX record pointed to `spyd.com`
- `spyd.com` A record pointed to GitHub Pages IPs (which don't handle email)
- `mail.spyd.com` was a CNAME to `spyd.com` (also GitHub Pages)

Result: Email to addresses like `franco@spyd.com` had nowhere to go.

### Solution Implemented
Created proper DNS separation between web and mail services:

**DNS Records**:
```
spyd.com              A      185.199.108.153  (GitHub Pages - website)
mail.spyd.com         A      45.56.217.65     (VPS - mail server)
spyd.com              MX     0 mail.spyd.com. (mail routing)
```

**SPF Record**:
```
spyd.com  TXT  "v=spf1 +mx +a +ip4:216.108.149.0/24 +ip4:45.56.217.65/32 ~all"
```

### Changes Made
1. Removed: `mail.spyd.com CNAME → spyd.com`
2. Added: `mail.spyd.com A → 45.56.217.65`
3. Updated: MX record destination from `spyd.com` to `mail.spyd.com`

## Mail Server

**Server**: Running on VPS (45.56.217.65)
**Status**: Active and accepting mail
**Ports**: Standard SMTP (25), Submission (587), IMAP/POP3
**Webmail Access**: Available via cPanel

## Active Email Accounts
- franco@spyd.com
- (other team members using spyd.com email)

## Access & Management

**cPanel Access**:
- URL: Via VPS hostname or IP
- Zone Editor: For DNS management
- Email Accounts: For mailbox management

**API Access**:
- UAPI available via SSH for automation
- DNS module: `uapi DNS parse_zone`, `mass_edit_zone`
- Email module: Available for account management

## Multi-Domain Hosting

The VPS hosts multiple domains beyond spyd.com. Each requires separate DNS configuration when using external hosting (like GitHub Pages) while maintaining email on the VPS.

## Notes

- DNS changes propagate immediately (verified working)
- Website and email services can be split across providers
- Always create dedicated A records for mail servers (mail.domain.com)
- Never point MX records directly to domains using external hosting
- SPF records must include VPS IP for proper email authentication

## Date
Configuration verified and documented: November 20, 2025
