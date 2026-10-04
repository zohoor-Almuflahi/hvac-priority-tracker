# HVAC Maintenance Priority Tracker

A lightweight, browser-based tool that helps facility managers see which commercial HVAC units need service soonest. Built for the Trane Technologies × IBM SkillsBuild micro-internship.

**Live demo:** https://zohoor-almuflahi.github.io/hvac-priority-tracker/

> This tool is for maintenance prioritization only. It is not made, approved, endorsed, or operated by Trane Technologies.

## What it does

A manager enters each unit's location, unit type, install date (optional), and last service date. The app calculates a priority status from the time since last service:

| Status | Days since last service |
|--------|-------------------------|
| Overdue | 182 or more |
| Due Soon | 152 to 181 |
| Within planned interval | Fewer than 152 |

The 182-day interval comes from Trane's public guidance recommending at least twice-annual preventive service (365 ÷ 2). The 30-day "Due Soon" buffer is an MVP planning choice, not a Trane-published threshold.

## Features

- Automatic status calculation with color-coded badges and a summary count per status
- Sortable priority table (Overdue units first by default), including keyboard sorting
- Add, edit, and delete units, with a confirmation dialog before deleting
- 30-day soft-delete with restore
- Form validation (required fields, no future service dates, service date cannot precede install date)
- Data saved in the browser with `localStorage` (no backend, no login, no data sent anywhere)
- Responsive layout and accessibility support (ARIA roles, focus management, Escape to close dialogs)

## Run locally

No install or build step is needed.

1. Clone or download this repository.
2. Open `index.html` in any modern browser.

Or serve it locally:

```bash
npx serve .
```

## Tech stack

Plain HTML5, CSS3, and vanilla JavaScript in a single file (`index.html`). No frameworks, bundlers, or dependencies.

## Documentation

- [Product Brief](PRODUCT_BRIEF.md): the user, problem, solution, and scope
- [Technical Documentation](TECHNICAL_DOCUMENTATION.md): architecture, setup, known limitations, and test cases
- [AI Collaboration Brief](AI_COLLABORATION.md): how AI tools were used and verified

## Known limitations

- One uniform interval for all unit types; Unit Type is a label only
- Data is stored in one browser and is not synced across devices
- No notifications and no user accounts

See the Technical Documentation for the full list.
