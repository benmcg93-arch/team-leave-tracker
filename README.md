# OSF Digital — Sprint & Leave Planner

A lightweight, zero-dependency team leave tracker and sprint capacity planner built for OSF Digital delivery teams. Leave is entered manually against a calendar and the app automatically calculates available capacity across configured sprints.

---

## How It Works

The app stores all data as a single JSON file in a GitHub repository of your choice. Changes are auto-saved via the GitHub API — no backend or database required.

**Setup takes ~2 minutes:**
1. Create a GitHub Personal Access Token (PAT) with `repo` scope
2. Create a repo (or use an existing one) to store the data file
3. Open the app and enter your PAT, repo path (`owner/repo`), and branch
4. Start adding team members and marking leave

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

#### Zoho Leave Integration (Sync)
Automatically pull **approved leave** from Zoho People into the calendar, removing the need for double-entry. Leave approved by a manager in Zoho would sync into the planner on a daily schedule (via GitHub Actions — no infrastructure changes required to the app itself).

- Requires a Zoho OAuth service account and a leave-type mapping (e.g. "Casual Leave" → Annual, "Medical Leave" → Sick)
- Sync would run on a schedule and commit updated JSON to the repo
- The app picks up changes on next load with no code changes

#### Manual Planning Leave Type
A dedicated **"Planned"** leave type (distinct from approved leave types) so sprint planners can model anticipated or unconfirmed absence without it being treated as authoritative data. This is important because:
- Zoho only reflects *approved* leave — planners often need to account for requests not yet approved
- Teams want to run "what if" scenarios (e.g. "if Alice takes that week, what's our capacity?")
- Planned entries would be visually distinct and excluded from Zoho sync overwrites

These two features are designed to work together: Zoho sync populates approved leave automatically, and the Planned type preserves manual planning capability alongside it.

#### Other Candidates
- iCal / Google Calendar export of leave per member
- Public holiday auto-population by country/region
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
