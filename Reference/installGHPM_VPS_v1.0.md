# GitHub Project Management Setup for VPS Infrastructure v1.0

**Purpose**: This document provides a complete prompt for Claude Code to set up a comprehensive GitHub-based project management system for your VPS hosting infrastructure.

**What This Sets Up**:

- GitHub CLI installation and authentication
- Custom issue labels for VPS/hosting infrastructure tracking
- GitHub Project board for visual task management (linked to repository)
- Kanban Board and Milestone Roadmap views
- Milestones for tracking major infrastructure goals
- Initial issues for pending work (all assigned to milestones AND project board)
- Documentation in your CLAUDE.md file
- View setup guide for project configuration
- **Mandatory milestone assignment policy**
- **Mandatory project board addition policy**

**Version History**:
- v1.0 (2025-11-20): Initial VPS infrastructure adaptation from drone GHPM v1.05

---

## Prerequisites

Before starting, ensure:

- Your VPS management workspace is in a Git repository
- The repository has a GitHub remote configured
- You have a CLAUDE.md file (or are willing to create one)
- You're working on macOS (for Homebrew installation)
- You have SSH access to your VPS

---

## The Prompt

Copy the text below and paste it into your Claude Code session:

---

### START OF PROMPT

I want you to set up a GitHub project management system for my VPS hosting infrastructure. Please follow these steps:

### Step 1: Install and Configure GitHub CLI

1. Check if GitHub CLI is installed, if not install via Homebrew: `brew install gh`
2. Check authentication status: `gh auth status`
3. If needed, authenticate with GitHub: `gh auth login`
4. Request additional scopes for project management: `gh auth refresh -s project,read:project --hostname github.com`

### Step 2: Create Issue Labels

Create the following labels for my repository (replace `OWNER/REPO` with my actual GitHub repo):

**Issue Type Labels**:

- `dns-issue` (color: #d73a4a) - DNS configuration problems and routing issues
- `email-config` (color: #0075ca) - Email server configuration and routing
- `ssl-certificate` (color: #fbca04) - SSL/TLS certificate issues
- `security-update` (color: #b60205) - Security patches and updates
- `maintenance` (color: #d4c5f9) - Routine maintenance tasks
- `configuration` (color: #a2eeef) - Server configuration changes
- `troubleshooting` (color: #e99695) - Active investigation of problems
- `backup` (color: #c2e0c6) - Backup and restore operations
- `performance` (color: #bfd4f2) - Performance optimization tasks
- `migration` (color: #f9d0c4) - Service migrations and transfers

**Priority Labels**:

- `priority: critical` (color: #b60205) - Service down or security critical
- `priority: high` (color: #d93f0b) - Important but service still operational
- `priority: medium` (color: #fbca04) - Should be addressed soon
- `priority: low` (color: #0e8a16) - Nice to have or future enhancement

**Service Labels** (for multi-domain/service VPS):

- `web-server` (color: #c5def5) - Apache/Nginx web server related
- `mail-server` (color: #bfdadc) - Mail server (SMTP/IMAP) related
- `dns-server` (color: #d4c5f9) - DNS zone management
- `database` (color: #fef2c0) - MySQL/PostgreSQL database related
- `control-panel` (color: #e4e669) - cPanel/WHM related

**Domain Labels** (customize based on your hosted domains):

- `domain: spyd.com` (color: #ededed) - Issues specific to spyd.com

Use the `gh label create` command for each label.

### Step 3: Create GitHub Project Board

1. Create a new project called "VPS Infrastructure Tracker" for my repository
2. Note the project number for future reference
3. Update the project with description and README:
   ```graphql
   mutation {
     updateProjectV2(input: {
       projectId: "PROJECT_ID"
       shortDescription: "Task tracking and milestone roadmap for VPS infrastructure management"
       readme: "# VPS Infrastructure Project\n\nTrack configurations, incidents, maintenance, and infrastructure goals.\n\n## Server Details\n- **Provider**: HostPapa\n- **Hostname**: vwes7374.hostpapavps.net\n- **Primary IP**: 45.56.217.65\n- **Management**: WHM & cPanel\n\n## Views\n- **Kanban Board**: Status-based workflow\n- **Milestone Roadmap**: Timeline view of infrastructure goals\n- **All Items**: Complete table view"
     }) {
       projectV2 { id }
     }
   }
   ```
4. **Link the project to the repository** using the GitHub GraphQL API:
   - Get the repository node ID: `gh api graphql -f query='{repository(owner: "OWNER", name: "REPO") {id}}'`
   - Get the project node ID from the project creation output
   - Link them: `gh api graphql -f query='mutation {linkProjectV2ToRepository(input: {projectId: "PROJECT_NODE_ID", repositoryId: "REPO_NODE_ID"}) {repository {name}}}'`

### Step 4: Create Milestones

Create the following milestones using the GitHub API (`gh api`):

1. **Email Migration Complete** - All email services functional and properly routed
2. **Security Hardened** - SSL certificates, firewall rules, and security patches applied
3. **Backup System Active** - Automated backup and restore procedures in place
4. **Multi-Domain Optimized** - All hosted domains properly configured and optimized
5. **Documentation Complete** - Full infrastructure documentation and runbooks

**Important**: These milestones will be used for ALL issues. Every issue MUST be assigned to a milestone.

### Step 5: Create Initial Issues

Based on the current VPS status (reference INFRASTRUCTURE.md), create GitHub issues for:

- Recent configuration changes that need documentation
- Known issues or pending fixes
- Planned maintenance tasks
- Security updates needed
- DNS or email configurations requiring attention

For each issue:

- Assign appropriate labels (type + priority + service/domain)
- **⚠️ MANDATORY: Assign to a relevant milestone** (every issue MUST have a milestone, no exceptions)
- Assign to me (the repository owner)
- **⚠️ MANDATORY: Add to the project board** (every issue MUST be on the project board, no exceptions)

**CRITICAL RULES**:
1. If you need to create an issue that doesn't fit any existing milestone, create a new milestone first, then create the issue with that milestone assignment. Never create an issue without a milestone.
2. **IMMEDIATELY after creating each issue**, add it to the project board using the GraphQL API:
   ```bash
   # Get the issue node ID
   ISSUE_ID=$(gh issue view ISSUE_NUMBER --repo OWNER/REPO --json id --jq '.id')

   # Add to project board
   gh api graphql -f query='
   mutation {
     addProjectV2ItemById(input: {
       projectId: "PROJECT_ID"
       contentId: "'$ISSUE_ID'"
     }) {
       item { id }
     }
   }'
   ```
3. **NEVER skip the project board addition step**. All issues must be visible on the project board.

### Step 6: Create Project View Setup Guide

Create a file called `GHPM_View_Setup_Guide.md` that provides step-by-step instructions for configuring project views through the GitHub web interface:

1. **Kanban Board View**: Instructions for creating a Board layout grouped by Status
2. **Milestone Roadmap View**: Instructions for creating a Roadmap layout grouped by Milestone
3. **Service Dashboard View**: Optional table view filtered/grouped by service labels
4. **View Management**: Tips for filters, sorting, and default view settings
5. **Workflow Automation**: Optional auto-archive and auto-add configurations

**Note**: GitHub Projects v2 does not expose view creation via GraphQL API, so views must be configured through the web interface. The guide should be comprehensive enough for quick setup.

### Step 7: Update CLAUDE.md

Add a comprehensive "GitHub Issues & Project Management" section to CLAUDE.md that documents:

1. **Dual-Track Documentation System**: Explain that we use both Markdown docs (INFRASTRUCTURE.md = comprehensive archive) and GitHub Issues (active task tracking)

2. **GitHub Project Structure**: Document the project board number, milestones, column structure, and available views (Kanban Board, Milestone Roadmap, Service Dashboard)

3. **Issue Label System**: List all labels and their purposes

4. **When to Create GitHub Issues**: Define when issues should be created:
   - DNS/email configuration changes or problems
   - SSL certificate renewals or issues
   - Security updates needed
   - Service outages or degradations
   - Maintenance tasks
   - Performance optimization needs
   - Configuration changes
   - Troubleshooting active problems

5. **Issue Creation Workflow**: Step-by-step process for creating and documenting issues
   - **⚠️ CRITICAL**: Include mandatory milestone assignment requirement
   - Document how to create new milestones if needed
   - Emphasize that every issue MUST have a milestone with NO exceptions
   - Include labeling guidelines (type + priority + service/domain)

6. **Integration Between Issues and Docs**: How to cross-reference between GitHub issues and INFRASTRUCTURE.md

7. **Common gh CLI Commands**: Provide a reference of frequently used commands including:
   - Creating issues with labels, assignee, and milestone
   - Listing issues by label, service, or priority
   - Updating issues
   - Closing issues
   - Adding issues to project board
   - Listing milestones
   - Creating new milestones

8. **Issue Lifecycle**: Explain the typical flow from creation to completion

9. **Best Practices**: Guidelines for issue titles, bodies, labels, and comments
   - **⚠️ MANDATORY**: Emphasize that milestones are required for every issue
   - Include server/service context in issue descriptions
   - Reference relevant log files or error messages
   - Document resolution steps in issue comments

10. **Milestone Assignment Guidelines**: Which types of issues go to which milestones
    - Include instructions for creating new milestones when needed

11. **Project Views**: Document available views and when to use each
    - Kanban Board: Daily task workflow
    - Milestone Roadmap: Long-term infrastructure planning
    - Service Dashboard: Service-specific issue tracking

Also update the "Future Claude Instances" section to:
- Remind future Claude sessions to check open GitHub issues when starting work
- **Add verification step**: Check that all issues have milestone assignments
- Provide command: `gh issue list --repo OWNER/REPO --json number,title,milestone,labels`
- Suggest checking for critical priority issues first

### Step 8: Commit Changes

Create a git commit documenting the GitHub project management setup with an appropriate commit message.

### Step 9: Provide Summary

Give me a summary of:

- What was set up
- Project board number and URL
- Number of labels created
- Number of milestones created
- Number of initial issues created
- **Confirm**: All issues have milestone assignments ✓
- How to access the project in VS Code or GitHub web
- **Reminder**: Configure Kanban Board and Milestone Roadmap views using GHPM_View_Setup_Guide.md

### END OF PROMPT

---

## Expected Outcome

After running this prompt, you will have:

✓ GitHub CLI installed and authenticated
✓ Custom issue labels tailored for VPS infrastructure management
✓ A GitHub Project board with description and README
✓ Project board linked to your repository for better integration
✓ Five milestones tracking major infrastructure goals
✓ Initial issues created for your pending work
✓ **All issues assigned to milestones (mandatory policy)**
✓ **All issues added to project board (mandatory policy)**
✓ Complete documentation in CLAUDE.md for future Claude sessions
✓ GHPM_View_Setup_Guide.md for quick view configuration
✓ All changes committed to your repository

**Next Step**: Follow `GHPM_View_Setup_Guide.md` to configure Kanban Board, Milestone Roadmap, and Service Dashboard views via web interface.

---

## Using the System for VPS Management

Once set up, Claude Code will automatically:

- Create GitHub issues when you report server problems, configuration changes, or maintenance needs
- Assign issues with appropriate labels (type + priority + service + domain)
- **Ensure every issue has a milestone assignment (mandatory policy)**
- Update INFRASTRUCTURE.md alongside GitHub issues
- Cross-reference between issues and documentation
- Track progress on the project board
- Help prioritize critical vs. non-critical tasks

You can access your issues and project board:

- **In VS Code**: GitHub Pull Requests and Issues extension (built-in)
- **On the Web**: <https://github.com/OWNER/REPO/issues> and <https://github.com/users/OWNER/projects/PROJECT_NUMBER>
- **Via CLI**: `gh issue list --repo OWNER/REPO` or `gh project view PROJECT_NUMBER --owner OWNER`

### Recommended Workflow

1. **Daily**: Check Kanban Board view for active tasks and critical issues
2. **Weekly**: Review Milestone Roadmap to track progress toward infrastructure goals
3. **On Issue/Problem**: Create GitHub issue + document in INFRASTRUCTURE.md (assign milestone!)
4. **On Resolution**: Close issue + update documentation with solution details
5. **Monthly**: Review and update milestones, archive completed tasks

---

## VPS-Specific Issue Examples

**DNS Configuration Change**:
- Labels: `dns-issue`, `configuration`, `priority: high`, `dns-server`, `domain: spyd.com`
- Milestone: Email Migration Complete
- Title: "Configure mail.spyd.com A record for email routing"

**Security Update**:
- Labels: `security-update`, `priority: critical`, `control-panel`
- Milestone: Security Hardened
- Title: "Apply cPanel security patch CVE-2024-XXXXX"

**Email Troubleshooting**:
- Labels: `email-config`, `troubleshooting`, `priority: high`, `mail-server`, `domain: spyd.com`
- Milestone: Email Migration Complete
- Title: "Investigate email delivery failures for franco@spyd.com"

**Routine Maintenance**:
- Labels: `maintenance`, `priority: medium`, `web-server`
- Milestone: Multi-Domain Optimized
- Title: "Update Apache configuration for performance optimization"

---

## Milestone Assignment Policy

**⚠️ MANDATORY REQUIREMENT**: Every issue MUST be assigned to a milestone. No exceptions.

**Why This Matters for VPS Management**:
- Milestones provide clear infrastructure goals
- Helps prioritize work toward operational readiness
- Milestone Roadmap view requires milestone assignments
- Tracks progress toward fully hardened, documented infrastructure
- Helps plan maintenance windows and update schedules

**VPS-Specific Milestone Guidelines**:
- **Email Migration Complete**: DNS, email routing, mailbox configuration issues
- **Security Hardened**: SSL, firewall, security patches, access control
- **Backup System Active**: Backup configuration, restore testing, disaster recovery
- **Multi-Domain Optimized**: Domain-specific configurations, performance tuning
- **Documentation Complete**: Infrastructure documentation, runbooks, procedures

**How to Handle Edge Cases**:
- If no existing milestone fits, create a new one first
- Use `gh api repos/OWNER/REPO/milestones -X POST -f title="Name" -f description="Desc"`
- Then assign the new milestone to your issue
- Future Claude instances will enforce this policy automatically

---

## Maintenance

The system is self-maintaining through Claude Code, but you should:

- Review and close completed issues promptly
- Update issue status as work progresses
- Keep milestone progress current
- Add new labels for new services or domains as needed
- Create filtered views for specific workflows
- **Verify all new issues have milestone assignments**
- Archive old milestones when infrastructure goals are met

---

## Support

This VPS infrastructure management system is adapted from the drone project GHPM system.

For questions or improvements, refer to the CLAUDE.md file in your repository after setup.

---

**Pro Tip**: After setup and view configuration, ask Claude Code to "provide current status of VPS infrastructure" to get a comprehensive status report including open GitHub issues, milestone progress, service health, and critical priority items from both the Kanban Board and Milestone Roadmap perspectives.
