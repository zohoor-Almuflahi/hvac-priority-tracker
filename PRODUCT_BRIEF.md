# Product Brief: HVAC Maintenance Priority Tracker

## User

Facility managers responsible for commercial HVAC systems across one or more buildings, who currently track maintenance status using spreadsheets, memory, or informal notes.

## Problem

There is no quick, reliable way for a facility manager to see which HVAC units need service soonest. Maintenance timing is scattered across spreadsheets or relies on someone remembering when each unit was last serviced. This leads to missed maintenance windows, reactive (rather than preventive) repairs, and no clear way to prioritize limited time and budget across multiple units.

## Solution

A lightweight, single-page web tool where a facility manager enters basic information for each HVAC unit — location, unit type, install date, and last service date — and the tool automatically calculates and displays a maintenance priority status: Overdue, Due Soon, or Within planned interval. Status is calculated from the time elapsed since the last service, using a uniform 182-day (~6-month) interval derived from Trane's publicly available guidance recommending a minimum of twice-annual preventive service.

## Core Workflow

1. Manager adds a unit via a simple form (location, unit type, install date, last service date).
2. The tool calculates each unit's status automatically based on time elapsed since last service, relative to a uniform 182-day preventive-service interval.
3. All units display in a single sortable table, with the most urgent (Overdue) units surfaced first by default.
4. Manager can edit a unit's information (e.g., after service is completed) using the same form, which updates its status immediately.

## Success Criteria

- A manager can add a new unit and see an accurate status (Overdue / Due Soon / Within planned interval) within seconds, with no manual calculation required.
- The table can be sorted so the most urgent units are immediately visible.
- The tool requires no login and stores no private or sensitive data — only unit-level maintenance metadata that the manager enters themselves.
- The product runs entirely in the browser with no backend or database dependency, and can be accessed via a single shared link.

### Evaluation

The system evaluates elapsed time since last service against a uniform 182-day preventive maintenance interval, derived from Trane's published recommendation of at least twice-annual service (365 ÷ 2):

- **Overdue:** ≥ 182 days since last service.
- **Due Soon:** 152–181 days since last service (a 30-day warning buffer, chosen as an MVP planning choice rather than a Trane-published threshold).
- **Within planned interval:** < 152 days since last service.

### Output

The application renders a sorted, high-contrast priority list displaying status badges, elapsed days since service, and a generic recommended action for units that need attention ("Schedule preventive service" for Overdue, "Plan preventive service" for Due Soon). No action is shown for units within their interval.

## Out of Scope

- User accounts, authentication, or role-based access
- Multi-building or multi-tenant data aggregation
- Automated notifications, emails, or reminders
- Integration with real building management systems or live sensor data
- Work order creation, technician scheduling, or service history beyond "last service date"
- Mobile app (this is a responsive web tool, not a native app)
- Equipment-specific maintenance intervals or task lists (one uniform interval is used for all unit types in this MVP)
- Manufacturer-specific maintenance instructions
