# OSF Digital — Sprint Capacity Planner

A lightweight, zero-dependency sprint capacity planner built for OSF Digital delivery teams. It bridges the gap between **PSA** (the team's source of truth for leave and resource allocation) and **Jira** (sprint planning), giving delivery leads a fast, sprint-aware view of available capacity.

> **How it fits in:** PSA tracks official leave (fed from Zoho) and project-level resource allocation by week. Jira manages sprint tickets and story points. Neither tool answers "how much capacity does this team actually have across sprint boundaries, accounting for leave?" — that's what this tool does.

---

## How It Works

The app stores all data as a single JSON file in a GitHub repository of your choice. Changes are auto-saved via the GitHub API — no backend or database required.

**Setup takes ~2 minutes:**
1. Create a GitHub Personal Access Token (PAT) with `repo` scope
2. Create a repo (or use an existing one) to store the data file
3. Open the app and enter your PAT, repo path (`owner/repo`), and branch
4. Configure your sprints and add team members

---

## Features

### ✅ MVP (Shipped)

| Feature | Description |
|---|---|
| **Leave Calendar** | Monthly view — click to mark leave, right-click to clear. Weekends and today highlighted automatically. |
| **Leave Types** | Annual, Sick, Public Holiday, Other — each colour-coded. |
| **Sprint Capacity** | Calculates available team capacity per sprint, deducting leave days. Shows utilisation % with green/amber/red status. |
| **Planned work tracking** | Enter planned story points/hours per sprint to surface surplus or deficit. |
| **Per-member capacity overrides** | Override default capacity for individual team members per sprint. |
| **Configuration panel** | Set project name, sprint length, start date, number of sprints, and capacity units (hours/days/points). |
| **GitHub persistence** | Data stored as JSON in your own GitHub repo — no backend needed. Auto-saves with debounce. |
| **Demo mode** | `demo.html` with realistic dummy data for onboarding new teams without touching live data. |

---

### 🗓 Upcoming

#### PSA / Zoho Leave Sync
Pull **approved leave** directly from PSA (which is already fed by Zoho People) into the capacity planner, eliminating double-entry. PSA is the team's source of truth for leave — this sync would make that data available to sprint planners automatically.

- Triggered on a schedule (e.g. daily via GitHub Actions — no infrastructure changes required)
- Leave type mapping: PSA/Zoho categories → Annual, Sick, Public Holiday, Other
- The app picks up changes on next load with no code changes

#### Planned Leave Type
A dedicated **"Planned"** leave type (distinct from PSA-approved leave) so sprint planners can model anticipated or unconfirmed absence without overwriting official data. Important because:
- PSA/Zoho only reflects *approved* leave — planners often need to account for requests not yet approved
- Teams want to run "what if" scenarios (e.g. "if Alice takes that week, what's our capacity?")
- Planned entries are visually distinct and excluded from any PSA sync overwrites

These two features are designed to work together: PSA sync populates approved leave automatically, and the Planned type preserves manual planning capability alongside it.

#### Other Candidates
- iCal / Google Calendar export of leave per member
- Slack or Teams notification when capacity drops below a threshold

---

## Data Structure

```json
{
  "members": [
    {
      "name": "Sarah Mitchell",
      "capacity": null,
      "leaves": {
        "2026-03-17": "annual",
        "2026-03-18": "annual",
        "2026-04-02": "sick"
      }
    }
  ],
  "config": {
    "projectName": "Commerce Cloud — Phase 2",
    "sprintWeeks": 3,
    "sprintStart": "2026-03-02",
    "numSprints": 4,
    "capUnit": "hours",
    "hoursPerDay": 8
  }
}
```

---

## Tech Stack

- Vanilla HTML/CSS/JS — no build step, no framework, no dependencies
- GitHub API v2022-11-28 for data persistence
- Google Fonts (Lato) — loaded from CDN
