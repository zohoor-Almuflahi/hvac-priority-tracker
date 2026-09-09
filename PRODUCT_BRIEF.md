# Product Brief: HVAC Maintenance Priority Tracker

## User

Facility managers responsible for commercial HVAC systems across one or more buildings, who currently track maintenance status using spreadsheets, memory, or informal notes.

## Problem

There is no quick, reliable way for a facility manager to see which HVAC units need service soonest. Maintenance timing is scattered across spreadsheets or relies on someone remembering when each unit was last serviced. This leads to missed maintenance windows, reactive (rather than preventive) repairs, and no clear way to prioritize limited time and budget across multiple units.

## Solution

A lightweight, single-page web tool where a facility manager enters basic information for each HVAC unit — location, unit type, install date, and last service date — and the tool automatically calculates and displays a maintenance priority status: **Overdue**, **Due Soon**, or **Fine**. Status is calculated using standard maintenance intervals based on unit type, informed by Trane's publicly available maintenance guidance (e.g., annual service for air handlers and central AC systems; twice-yearly service for packaged rooftop units).

## Core Workflow

1. Manager adds a unit via a simple form (location, unit type, install date, last service date).
2. The tool calculates each unit's status automatically based on time elapsed since last service, relative to the standard interval for that unit type.
3. All units display in a single sortable table, with the most urgent (Overdue) units surfaced first by default.
4. Manager can edit a unit's information (e.g., after service is completed) using the same form, which updates its status immediately.

## Success Criteria

- A manager can add a new unit and see an accurate status (Overdue / Due Soon / Fine) within seconds, with no manual calculation required.
- The table can be sorted so the most urgent units are immediately visible.
- The tool requires no login and stores no private or sensitive data — only unit-level maintenance metadata that the manager enters themselves.
- The product runs entirely in the browser with no backend or database dependency, and can be accessed via a single shared link.

### Evaluation

The system evaluates elapsed time against standard 90-day preventive maintenance intervals aligned with Trane Technologies commercial maintenance guidelines:

- **Critical (Overdue):** ≥ 90 days since last service.
- **Warning (Due Soon):** 60–89 days since last service.
- **Optimal (Fine):** < 60 days since last service.

### Output

The application renders a sorted, high-contrast priority list displaying status flags, elapsed days, and recommended maintenance actions (filter replacement, coil cleaning, belt inspection).

## Out of Scope

- User accounts, authentication, or role-based access
- Multi-building or multi-tenant data aggregation
- Automated notifications, emails, or reminders
- Integration with real building management systems or live sensor data
- Work order creation, technician scheduling, or service history beyond "last service date"
- Mobile app (this is a responsive web tool, not a native app)
