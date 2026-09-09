# Technical Documentation
## HVAC Maintenance Priority Tracker

---

## Architecture and Technology Choices

**Stack:** Plain HTML5, CSS3, and vanilla ES6+ JavaScript in a single file (`index.html`). No frameworks, no bundler, no build step, no backend.

**Why frontend-only:** The assignment brief requires no private/sensitive data and no login. A backend and database would have added real complexity (hosting, environment setup, additional failure points) without unlocking any capability the product actually needs. All unit data is entered by the user and stored locally in the browser via `localStorage` — nothing is transmitted to a server.

**Why a single file, no build tooling:** With one screen and no complex component tree, a build pipeline (React/bundlers) would add setup overhead without a corresponding benefit. A single static HTML file can be opened directly or deployed to any static host (e.g., GitHub Pages) with zero configuration.

**Core logic design:** The maintenance-status calculation is isolated into small, pure functions (`getStatus()`, `getRecommendedAction()`) that take input data and return a result with no side effects. This makes the core business logic testable independently of the UI — a hardcoded date can be passed in and the output checked directly, without needing to interact with the DOM.

**Maintenance interval logic:** Status is calculated from elapsed days since last service, using a uniform 182-day interval (derived from Trane's published guidance recommending a twice-annual minimum preventive service schedule — 365 ÷ 2). A 30-day buffer defines the "Due Soon" warning window before the 182-day threshold (152 days). This interval is intentionally uniform across all unit types for the MVP (see Known Limitations).

**Data persistence:** Two `localStorage` keys are used — one for active units, one for soft-deleted units (see below). Dates are stored as raw ISO strings, not pre-calculated day counts, so status remains accurate on every page load regardless of how much time has passed.

**Soft-delete / restore:** Deleting a unit moves it to a separate "deleted" store for 30 days rather than erasing it immediately, allowing recovery from accidental deletion. This was a deliberate addition beyond the original scoped workflow, made to reduce the risk of unrecoverable data loss in a tool with no backend/undo history.

**Accessibility:** Form fields use `aria-required`; validation errors use `role="alert"`; the delete-confirmation dialog uses `role="alertdialog"` with focus management and Escape-to-close; sortable table headers are keyboard-operable (`tabindex`, Enter/Space handling), not mouse-only.

---

## File Structure

```
/
├── index.html                    # Complete application: HTML structure, CSS, and JS in one file
├── package.json                  # Project metadata; "start" script serves the file locally
├── README.md                     # Setup and access instructions, live demo link
├── PRODUCT_BRIEF.md              # Product brief deliverable
├── AI_COLLABORATION.md           # AI collaboration and role-evolution brief deliverable
└── TECHNICAL_DOCUMENTATION.md    # This document
```

No `src/` directory, build config, or dependency manifest beyond `package.json` metadata is required, since there is no build step. 

---

## Setup Instructions

**To run locally:**
1. Clone or download the repository.
2. Open `index.html` directly in any modern browser — no installation required.
3. Alternatively, run `npx serve .` from the project root (per the `start` script in `package.json`) to serve it over `localhost` instead of the `file://` protocol.

**To access the deployed version:** *(live link to be added once deployed via GitHub Pages)*

No environment variables, API keys, or external services are required.

---

## Known Limitations

- **Maintenance interval is uniform across all unit types.** The 182-day interval does not currently vary by equipment type (e.g., chillers vs. rooftop units), even though the app collects Unit Type. This was a deliberate MVP scope decision (see Product Brief, Out of Scope) rather than an oversight — real-world intervals vary by equipment and service agreement, which a future version could account for.
- **Unit Type does not affect calculated status.** Because the interval is uniform, the Unit Type field (Performance Climate Changer, CenTraVac, or Precedent) is currently informational/labeling only, not a driver of the business logic.
- **Data is local to one browser.** `localStorage` does not sync across devices or browsers; clearing browser data will erase saved units. There is no cloud backup.
- **No manufacturer-specific maintenance instructions.** Recommended actions ("Schedule preventive service," "Plan preventive service") are intentionally generic, not equipment-specific task lists, per the product brief's scope boundary.
- **No notifications.** The tool is a passive dashboard a manager checks manually; it does not send emails, texts, or push alerts when a unit becomes overdue.
- **Single-user, no accounts.** There is no way to distinguish between different managers' data on a shared device, since there is no login by design.
- **The "Due Soon" 30-day buffer is an MVP planning choice**, not a Trane-published threshold — only the 182-day interval itself is sourced from Trane's public guidance.

---

## Test Cases and Results

Testing was performed manually against the live app, primarily by supplying specific hardcoded dates and confirming the resulting status/behavior.

### Status calculation

| ID | Test | Steps | Expected Result | Result |
|----|------|-------|-----------------|--------|
| TC-02 | Overdue status | Last service date ≥ 182 days ago | Status = "Overdue," red badge, "Schedule preventive service" shown | Pass |
| TC-03 | Due Soon status | Last service date 152–181 days ago | Status = "Due Soon," yellow badge, "Plan preventive service" shown | Pass |
| TC-04 | Within interval status | Last service date < 152 days ago | Status = "Within planned interval," green badge, no action text shown | Pass |

### Add, edit, delete, and restore

| ID | Test | Steps | Expected Result | Result |
|----|------|-------|-----------------|--------|
| TC-01 | Add unit — valid data | Fill all required fields, save | New row appears with correct status badge | Pass |
| TC-05 | Edit unit after service | Update an Overdue unit's last-service date to today | Status updates to "Within planned interval" immediately | Pass |
| TC-06 | Delete → soft-delete → restore | Delete a unit, confirm, then click Restore in Last Deleted | Unit moves to Last Deleted table, then returns to the active list on restore | Pass |

### Persistence and sorting

| ID | Test | Steps | Expected Result | Result |
|----|------|-------|-----------------|--------|
| TC-07 | Persistence | Add units, refresh the browser | All units and correct statuses reload from `localStorage` | Pass |
| TC-08 | Default sort | Add units with mixed statuses | Overdue units appear first by default | Pass |
| TC-09 | Sort by other columns | Click each sortable column header | Table re-sorts by that column, indicator arrow updates | Pass |
| TC-15 | Keyboard sort | Tab to a column header, press Enter/Space | Column sorts without requiring a mouse click | Pass |

### Validation

| ID | Test | Steps | Expected Result | Result |
|----|------|-------|-----------------|--------|
| TC-10 | Required field validation | Submit form with empty Location, Unit Type, or Last Service Date | Inline error shown per missing field, submission blocked | Pass |
| TC-11 | Future-date validation | Enter a Last Service Date after today | Error: "Last service date cannot be in the future" | Pass |
| TC-12 | Install-date-order validation | Enter a Last Service Date earlier than Install Date | Error: "Last service date cannot be before install date" | Pass |

### Layout and empty state

| ID | Test | Steps | Expected Result | Result |
|----|------|-------|-----------------|--------|
| TC-13 | Empty state | View the table with zero units | "No units yet" message shown instead of a blank table | Pass |
| TC-14 | Responsive layout | Resize viewport to ≤ 640px | Layout adjusts; table remains usable via horizontal scroll | Pass |

**Browser support:** Tested in current Chrome; expected to work in any modern evergreen browser (Firefox, Safari, Edge) given standard HTML5/CSS3/ES6+ features and no non-standard APIs beyond `localStorage` and `crypto.randomUUID()` (with a fallback ID generator if unavailable).
