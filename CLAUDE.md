# PM Agent Flow v2 — System Instructions

## What this is
A multi-agent workflow for product managers. Given a customer problem,
the system produces a complete, dev-ready PRD that QA can write tests
against before development begins.

---

## Agent Routing

| Step | Agent | Mode | Waits for |
|------|-------|------|-----------|
| 00 | Intake Agent | INTERACTIVE | — |
| 01 | Persona Builder | PARALLEL | intake.md |
| 02 | Problem Definer | PARALLEL | intake.md |
| 03 | Market & Expert Intelligence | SEQUENTIAL | persona.md + problem.md |
| 04 | Solution Architect | SEQUENTIAL | market_intel.md |
| 05 | UX/UI Agent | CONDITIONAL | solution.md (only if UI required) |
| 06 | PRD Writer | SEQUENTIAL | solution.md + ux_brief.md (if exists) |
| 07 | Dev Critic | SEQUENTIAL | prd.md |
| 08 | QA Validator | SEQUENTIAL | prd.md + dev_feedback.md |
| 09 | Iteration Agent | ON-DEMAND | final_report.md + human feedback |

---

## Token Efficiency Protocol — MANDATORY

### The Context Summary
Every agent MUST:
1. Read `outputs/context_summary.md` FIRST — this contains all decisions already made
2. Read ONLY the specific input files listed in its "Inputs" section
3. After writing its output, APPEND a summary of its decisions to `context_summary.md`

### What goes in context_summary.md
Each agent appends a section like:
```
## [Agent Name] — decisions made
- [key decision 1]
- [key decision 2]
- Skipped: [anything explicitly ruled out]
```

### Why this matters
- Agents never re-read full files from earlier steps
- Decisions are never re-derived — they're cached in context_summary.md
- No content is repeated between agents

---

## Iteration Protocol

When the Iteration Agent is triggered:
1. Read `outputs/context_summary.md` to understand what was decided
2. Read the human feedback
3. Identify the MINIMUM set of agents that need to re-run
4. Re-run only those agents, in order
5. Update context_summary.md with revised decisions
6. Never re-run an agent whose output is unaffected by the change

---

## File Conventions
- All outputs go to: `outputs/`
- User attachments go to: `inputs/attachments/`
- Never overwrite existing output — append _v2 if revision needed
- context_summary.md is the single source of truth for decisions

---

## System Context (fill in before running)
- Product: [YOUR PRODUCT NAME]
- Type: [B2B / B2C]
- Tech stack: [e.g. React, Node, PostgreSQL]
- Key constraints: [e.g. no backend changes, mobile-first]
