# AI Collaboration & Role-Evolution Brief

## Purpose

This document tracks how AI-assisted development is used on the HVAC Maintenance Priority Tracker project, including role boundaries, decision ownership, and iteration workflow.

## Current Phase

**Phase 1 — Static UI shell**

Build the single-file HTML layout and styling with no application logic. Establish visual language (high-contrast dark theme, status badges) and DOM structure ready for vanilla JS in the next step.

## AI Role Boundaries

| Area | AI handles | Human owns |
|------|------------|------------|
| Layout & CSS | Structure, responsive table, badge styling | Brand preferences, field labels |
| Business rules | Implement thresholds from product brief | Validate intervals against org policy |
| Data persistence | localStorage implementation (future) | Whether to export/backup data |
| Deployment | Static hosting suggestions | Final hosting URL & access |

## Iteration Workflow

1. **Define** — Product brief sets scope and success criteria
2. **Structure** — Static HTML/CSS shell (current step)
3. **Logic** — Status calculation, CRUD, localStorage
4. **Polish** — Sorting, recommended actions, empty states
5. **Validate** — Manual test cases from technical documentation

## Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| — | Single-file `index.html`, no frameworks | Zero build step; easy to share via one link |
| — | 90-day universal interval for v1 | Simplifies MVP; unit-type intervals can follow |
| — | Dark high-contrast theme | Status colors (red/yellow/green) remain readable |

## Handoff Notes for Next Step

When adding JavaScript:

- Wire `#toggle-form-btn` to show/hide `#unit-form-panel`
- Replace static table rows with dynamic rendering from unit array
- Compute days since service and map to badge classes (`badge--overdue`, etc.)
- Persist units in `localStorage` under a stable key
