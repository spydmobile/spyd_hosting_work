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

## GitHub Issues & Project Management

This repository uses a dual-track documentation system combining comprehensive Markdown documentation with active GitHub Issues for task tracking.

### Project Structure

**Project Board**: VPS Infrastructure Tracker (#16)
- **URL**: https://github.com/users/spydmobile/projects/16
- **Repository**: https://github.com/spydmobile/spyd_hosting_work

**Milestones** (all issues MUST have milestone assignments):
1. **Email Migration Complete** - All email services functional and properly routed
2. **Security Hardened** - SSL certificates, firewall rules, security patches applied
3. **Backup System Active** - Automated backup and restore procedures in place
4. **Multi-Domain Optimized** - All hosted domains properly configured and optimized
5. **Documentation Complete** - Full infrastructure documentation and runbooks

**Project Views** (configured via web interface):
- **Kanban Board** (default) - Status-based workflow for daily task management
- **Milestone Roadmap** - Timeline view of infrastructure goals
- **Service Dashboard** - Issues grouped by service type (optional)
- **Priority Dashboard** - Critical and high-priority tasks (optional)

**View Setup**: See `GHPM_View_Setup_Guide.md` for step-by-step configuration instructions.

### Issue Label System

**Issue Types**:
- `dns-issue` - DNS configuration problems and routing issues
- `email-config` - Email server configuration and routing
- `ssl-certificate` - SSL/TLS certificate issues
- `security-update` - Security patches and updates (critical)
- `maintenance` - Routine maintenance tasks
- `configuration` - Server configuration changes
- `troubleshooting` - Active investigation of problems
- `backup` - Backup and restore operations
- `performance` - Performance optimization tasks
- `migration` - Service migrations and transfers

**Priorities** (always include one):
- `priority: critical` - Service down or security critical
- `priority: high` - Important but service still operational
- `priority: medium` - Should be addressed soon
- `priority: low` - Nice to have or future enhancement

**Services** (specify affected service):
- `web-server` - Apache/Nginx web server related
- `mail-server` - Mail server (SMTP/IMAP) related
- `dns-server` - DNS zone management
- `database` - MySQL/PostgreSQL database related
- `control-panel` - cPanel/WHM related

**Domains** (specify if domain-specific):
- `domain: spyd.com` - Issues specific to spyd.com
- (Add labels for other domains as needed)

### When to Create GitHub Issues

Create issues for:
- **DNS/Email Configuration**: Changes or problems with DNS, email routing, MX records
- **SSL Certificates**: Renewals, issues, or audit tasks
- **Security Updates**: Patches, vulnerabilities, access control changes
- **Service Outages**: Any service degradation or downtime
- **Maintenance Tasks**: Routine or scheduled maintenance
- **Performance Issues**: Slow responses, resource constraints
- **Configuration Changes**: Server or service configuration updates
- **Troubleshooting**: Active investigation of problems
- **Migrations**: Moving services or domains
- **Documentation**: Creating runbooks or updating docs

### Issue Creation Workflow

**⚠️ MANDATORY REQUIREMENTS**:
1. Every issue MUST have a milestone assignment (no exceptions)
2. Every issue MUST be added to the project board
3. Include appropriate labels (type + priority + service/domain)

**Step-by-step**:

```bash
# 1. Create issue with labels, milestone, and assignee
gh issue create --repo spydmobile/spyd_hosting_work \
  --title "Your issue title" \
  --body "Detailed description..." \
  --label "issue-type,priority: X,service-name" \
  --milestone "Milestone Name" \
  --assignee spydmobile

# 2. Get issue number from output (e.g., #6)

# 3. Add issue to project board (MANDATORY)
ISSUE_ID=$(gh issue view 6 --repo spydmobile/spyd_hosting_work --json id --jq '.id')
gh api graphql -f query='
mutation {
  addProjectV2ItemById(input: {
    projectId: "PVT_kwHOABBD-s4BIrDk"
    contentId: "'$ISSUE_ID'"
  }) {
    item { id }
  }
}'
```

**If no milestone fits**: Create a new milestone first:
```bash
gh api repos/spydmobile/spyd_hosting_work/milestones -X POST \
  -f title="New Milestone Name" \
  -f description="Description of this goal" \
  -f state="open"
```

### Integration Between Issues and Documentation

**Cross-referencing**:
- Reference issues in documentation: `See issue #5 for details`
- Reference documentation in issues: `Details in INFRASTRUCTURE.md`
- Link related issues: `Related to #3` or `Closes #1`

**Workflow**:
1. **When problem occurs**: Create GitHub issue for tracking
2. **Document details**: Add comprehensive info to INFRASTRUCTURE.md
3. **Link them**: Reference issue # in docs, reference docs in issue
4. **On resolution**: Update issue with solution, update docs with final state
5. **Close issue**: Mark complete on project board

### Common gh CLI Commands

**List issues**:
```bash
# All open issues
gh issue list --repo spydmobile/spyd_hosting_work

# By label
gh issue list --repo spydmobile/spyd_hosting_work --label "priority: critical"

# By milestone
gh issue list --repo spydmobile/spyd_hosting_work --milestone "Email Migration Complete"

# With full details
gh issue list --repo spydmobile/spyd_hosting_work --json number,title,milestone,labels
```

**View issue details**:
```bash
gh issue view 1 --repo spydmobile/spyd_hosting_work
```

**Update issue**:
```bash
gh issue edit 1 --repo spydmobile/spyd_hosting_work --add-label "troubleshooting"
```

**Close issue**:
```bash
gh issue close 1 --repo spydmobile/spyd_hosting_work --comment "Resolved: email routing fixed"
```

**List milestones**:
```bash
gh api repos/spydmobile/spyd_hosting_work/milestones --jq '.[] | {number, title, open_issues}'
```

**Create milestone**:
```bash
gh api repos/spydmobile/spyd_hosting_work/milestones -X POST \
  -f title="Milestone Name" \
  -f description="Description"
```

### Issue Lifecycle

1. **Created**: New issue with milestone + labels + assigned to project
2. **Triaged**: Priority set, service identified
3. **In Progress**: Status updated on Kanban Board
4. **Resolved**: Solution implemented, documented
5. **Closed**: Issue marked complete, moved to Done column

### Best Practices

**Issue Titles**:
- Be specific: ❌ "Email problem" ✅ "Email routing fails for franco@spyd.com"
- Include affected service/domain when relevant
- Start with verb: "Fix...", "Implement...", "Document...", "Investigate..."

**Issue Bodies**:
- **Background**: What's the context?
- **Problem/Objective**: What needs to be done?
- **Impact**: What's affected? (service, users, domains)
- **Steps to reproduce**: For bugs/issues
- **Expected outcome**: What does success look like?
- **References**: Link to logs, docs, related issues

**Labels**:
- Include type, priority, and service
- Add domain label if domain-specific
- Example: `dns-issue, priority: high, dns-server, domain: spyd.com`

**Milestones** (⚠️ MANDATORY):
- Every issue must have a milestone
- Choose the milestone that best fits the work
- If none fit, create a new milestone
- Helps with roadmap planning and prioritization

**Comments**:
- Document progress and findings
- Include commands run and results
- Link to commits that address the issue
- Explain resolution when closing

### Milestone Assignment Guidelines

**Email Migration Complete**:
- DNS configuration for email routing
- MX record updates
- Mail server configuration
- Email account management
- SPF/DKIM/DMARC setup

**Security Hardened**:
- SSL certificate management
- Firewall configuration
- Security patches
- Access control
- Vulnerability remediation

**Backup System Active**:
- Backup configuration
- Restore testing
- Disaster recovery planning
- Backup monitoring

**Multi-Domain Optimized**:
- Domain inventory
- Multi-domain configuration
- Performance optimization
- Resource allocation

**Documentation Complete**:
- Runbook creation
- Procedure documentation
- Architecture documentation
- Knowledge base articles

**Create new milestone if needed**: For work that doesn't fit existing categories

### Verification Commands

**Check all issues have milestones**:
```bash
gh issue list --repo spydmobile/spyd_hosting_work --json number,title,milestone | \
  jq '.[] | select(.milestone == null) | {number, title, milestone: "MISSING"}'
```

Should return empty if all issues have milestones.

**Check project board items**:
```bash
gh project view 16 --owner spydmobile --format json | \
  jq '.items | length'
```

Should match number of open issues.

## Multi-Domain Context

The VPS hosts multiple domains beyond spyd.com. When making changes:
- Verify which domain you're modifying
- Check for dependencies between domains
- Consider impact on other hosted sites
- Each domain may have different hosting arrangements (some fully on VPS, others split like spyd.com)
