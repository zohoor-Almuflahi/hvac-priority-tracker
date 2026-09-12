# AI Collaboration and Role-Evolution Brief
## HVAC Maintenance Priority Tracker

Tools used: **Cursor AI** (code generation), **Claude** (planning, research, judgment, review, verification), **Gemini** (planning support, comparison and cross-checking).

---

## AI Uses Across Planning, Coding, Debugging, and Testing

**Planning.** Before any code was written, I used Claude and Gemini to pressure-test the product scope against the assignment brief. This included deciding between a frontend-only build and a full-stack approach (frontend-only won, since it matched the "no private data" requirement and kept the build achievable in the time available), and deciding to skip a separate Figma prototyping stage in favor of designing directly in code, since the product was small enough that a prototype-then-rebuild step would have doubled the work for no real benefit.

**Coding.** Cursor generated the actual application code. Rather than requesting the entire app in one prompt, the build was staged: static layout first, then the isolated status-calculation logic, then the interactive wiring (add/edit/delete/sort, validation, local storage persistence). Staging the requests kept each piece small enough to actually review before moving to the next.

**Debugging and testing.** The core maintenance-status logic (`getStatus()`) was tested manually against hardcoded example dates for each status boundary (Overdue, Due Soon, Within Interval) before being trusted inside the UI. Later, a full code review pass (done with Claude, acting as a senior-developer reviewer) caught several smaller issues: incorrect recommended-action wording for healthy units, a missing validation rule (service date preceding install date), and leftover dead code from an earlier version of the delete logic.

**Research and alignment.** After the MVP was functional, I used Claude to research Trane Technologies' actual public product lines, published HVAC maintenance guidance, and sustainability commitments (the Gigaton Challenge), and used that research to refine the product — replacing generic unit-type labels with real Trane product lines and grounding the maintenance-interval logic in a verified public source rather than an assumed number.

---

## Code Accepted, Modified, or Rejected

The majority of Cursor-generated code was reviewed and accepted as-is, particularly the layout, form structure, and event-handling boilerplate, which were straightforward and low-risk.

Several pieces were **modified** after review:
The "recommended action" for a healthy unit originally suggested unnecessary maintenance (e.g., "Belt inspection" for a unit in good standing) — changed to show no action for healthy units.
- Form validation was missing a check preventing a last-service date from being entered before a unit's install date added.
- A small piece of dead code (destructuring a field that active units never carried) was cleaned up rather than left in.

One change was **caught, then deliberately accepted rather than rejected outright**: when asked for a narrow, single-value color change, Cursor also changed the entire visual theme (dark to light) and swapped the title font neither of which were requested. Rather than either blindly accepting the extra changes or automatically reverting them, I evaluated the result on its own merits and chose to keep the new theme, since a lighter, more familiar look better matched the goal of feeling recognizable to Trane reviewers.

No generated code was rejected outright and rebuilt from scratch — the review process was mostly about refinement, not replacement.

---

## Security, Licensing, Quality, or Comprehension Risks

No security or licensing risks were identified. The product's architecture (no backend, no login, no external data transmission, local-storage-only persistence) inherently limits security exposure, and no third-party dependencies with licensing concerns were introduced.

The main risk pattern that did surface was **AI-stated claims being inaccurate or going beyond what was asked, without flagging it as such** and this came from more than one AI tool involved in the project, not just the code-generation tool:
- Cursor made an unrequested design change (see above) while completing a requested one.
- Separately, while verifying source citations for the maintenance-guidance section, Claude itself stated that a link was broken based on an assumption rather than an actual check. This was caught only because I clicked the link myself and found it worked correctly.

Both instances were resolved through manual verification rather than trusting the AI's stated confidence. This reinforced a practical rule for the rest of the project: **treat AI output, including AI's claims about its own output, as a draft to verify, not a fact to accept.**

---

## How AI May Change Junior Developer Responsibilities

Working this way clarified, for me, what the role of a junior developer is shifting toward. Writing syntax from scratch matters less than it used to AI tools can produce working code quickly. What matters more is the ability to see the bigger picture of the application: how a given change fits into the whole product, not just whether it runs in isolation.

A junior developer today needs to know how to evaluate AI output critically what to accept, what to modify, and what to reject outright and that includes recognizing good design and architectural decisions, not just functional code, because AI can generate something that works while still being the wrong choice for the product. It also means not extending automatic trust to an AI tool's own claims about its work, the same way you wouldn't extend it to unverified code. The skill isn't typing the code anymore it's knowing enough to judge it, and being willing to verify it, even when the AI sounds confident.