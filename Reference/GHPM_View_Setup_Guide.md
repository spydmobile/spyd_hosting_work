# GitHub Project Views Setup Guide - VPS Infrastructure

**Project**: VPS Infrastructure Tracker (#16)
**URL**: https://github.com/users/spydmobile/projects/16

This guide walks you through configuring custom views for your VPS infrastructure project. Views must be configured through the GitHub web interface (not available via API).

**Estimated time**: 5 minutes

---

## Prerequisites

- GitHub Project created (✓ Project #16)
- Issues created and linked to project (✓ 5 issues)
- Milestones configured (✓ 5 milestones)

---

## View 1: Kanban Board (Status Workflow)

**Purpose**: Daily task management with status-based workflow

### Steps:

1. **Open Project**:
   - Navigate to: https://github.com/users/spydmobile/projects/16
   - You should see the default "Table" view

2. **Create New View**:
   - Click the "+ New view" button (top right, next to current view tabs)
   - Select "Board" as the layout type
   - Name it: `Kanban Board`

3. **Configure Board Grouping**:
   - Click the "Group by" dropdown at the top of the board
   - Select "Status"
   - The board will now show columns: Todo, In Progress, Done

4. **Customize Columns** (optional):
   - You can rename columns by clicking the "..." menu on each column
   - Adjust column order by dragging
   - Add custom statuses if needed via column menu

5. **Set as Default View**:
   - Click the view dropdown (next to view name)
   - Select "Set as default"
   - This view will now open first when accessing the project

---

## View 2: Milestone Roadmap (Timeline Planning)

**Purpose**: Long-term infrastructure planning by milestone

### Steps:

1. **Create New View**:
   - Click "+ New view"
   - Select "Roadmap" as the layout type
   - Name it: `Milestone Roadmap`

2. **Configure Roadmap Grouping**:
   - Click "Group by" dropdown
   - Select "Milestone"
   - The roadmap will show swimlanes for each milestone:
     - Email Migration Complete
     - Security Hardened
     - Backup System Active
     - Multi-Domain Optimized
     - Documentation Complete

3. **Adjust Timeline View**:
   - Use the zoom controls (top right) to adjust time scale
   - Options: Quarters, Months, Weeks
   - Recommended: Start with "Months" view

4. **Set Milestone Dates** (optional but recommended):
   - Go to repository Milestones page: https://github.com/spydmobile/spyd_hosting_work/milestones
   - Edit each milestone to add due dates
   - Return to roadmap view to see timeline visualization

---

## View 3: Service Dashboard (Optional)

**Purpose**: Filter and group issues by service type (web-server, mail-server, dns-server, etc.)

### Steps:

1. **Create New View**:
   - Click "+ New view"
   - Select "Table" as layout type
   - Name it: `Service Dashboard`

2. **Configure Grouping**:
   - Click "Group by" dropdown
   - Select "Labels"
   - The table will group issues by their service labels

3. **Add Useful Columns**:
   - Click "+ Add field" (top right of table)
   - Add columns: Priority, Milestone, Assignees, Status
   - Reorder columns by dragging headers

4. **Apply Filters** (optional):
   - Click "Filter" button (top)
   - Add filter: `label: web-server OR label: mail-server OR label: dns-server OR label: database OR label: control-panel`
   - This shows only service-related issues

---

## View 4: Priority View (Optional)

**Purpose**: Focus on critical and high-priority tasks

### Steps:

1. **Create New View**:
   - Click "+ New view"
   - Select "Table" layout
   - Name it: `Priority Dashboard`

2. **Apply Priority Filter**:
   - Click "Filter"
   - Add filter: `label: "priority: critical" OR label: "priority: high"`

3. **Group by Status**:
   - Click "Group by"
   - Select "Status"
   - You'll see critical/high items organized by their current status

4. **Sort by Priority**:
   - Click "Sort" button
   - Add sort: Labels → Descending (critical will appear first)

---

## Additional Configuration

### Workflow Automation

Enable automatic status updates:

1. Click project "..." menu → Settings
2. Navigate to "Workflows" section
3. Enable these workflows:
   - **Auto-add to project**: Automatically add new issues from repository
   - **Item closed**: Move items to "Done" when closed
   - **Item reopened**: Move items back to "Todo" when reopened

### Field Customization

Add custom fields for VPS-specific tracking:

1. Go to project Settings → Fields
2. Add custom fields:
   - **Impact** (Single select): Critical, High, Medium, Low
   - **Service Affected** (Single select): Web, Mail, DNS, Database, All
   - **Downtime** (Text): Record any service downtime

### Saved Filters

Create quick filters for common queries:

- Click "Filter" → Set filter criteria → Click "Save filter"
- Useful filters:
  - `is:open label:"priority: critical"`
  - `is:open milestone:"Email Migration Complete"`
  - `is:open label:"domain: spyd.com"`

---

## View Recommendations

**Daily Workflow**: Use **Kanban Board** view
- See what's in progress
- Move cards through workflow
- Quick status updates

**Weekly Planning**: Use **Milestone Roadmap** view
- Review milestone progress
- Adjust priorities and timelines
- Plan upcoming work

**Service Incidents**: Use **Service Dashboard** or **Priority Dashboard**
- Quick identification of affected services
- Filter by critical priority
- Cross-service impact assessment

---

## Keyboard Shortcuts

Useful shortcuts in project views:

- `c` - Create new issue
- `?` - Show all keyboard shortcuts
- `Cmd/Ctrl + K` - Open command palette
- Drag & drop issues between columns (Kanban) or timeline (Roadmap)

---

## Accessing Your Project

Multiple access points:

1. **Direct URL**: https://github.com/users/spydmobile/projects/16
2. **Repository**: https://github.com/spydmobile/spyd_hosting_work → "Projects" tab
3. **GitHub Menu**: Profile → "Projects" → "VPS Infrastructure Tracker"
4. **VS Code**: GitHub Pull Requests and Issues extension → Projects section

---

## Troubleshooting

**Issues not showing in views?**
- Check that issues are added to the project (not just created in repo)
- Verify filters aren't excluding them
- Refresh the page

**Can't see milestone groups?**
- Ensure issues have milestone assignments
- Check that milestones exist in repository
- Verify grouping is set to "Milestone"

**Roadmap view empty?**
- Add due dates to milestones for timeline visualization
- Ensure issues are assigned to milestones
- Check zoom level (try "Months" view)

---

## Next Steps

After setting up views:

1. ✓ Set Kanban Board as default view
2. ✓ Add due dates to milestones for roadmap visualization
3. ✓ Enable workflow automation
4. Test dragging issues between status columns
5. Bookmark project URL for quick access
6. Consider installing GitHub mobile app for on-the-go updates

---

**Questions?** Refer to the "GitHub Issues & Project Management" section in CLAUDE.md for detailed workflow documentation.
