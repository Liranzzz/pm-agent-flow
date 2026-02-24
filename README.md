# PM Agent Flow v2

A multi-agent AI workflow for product managers.

You describe a customer problem → 9 specialized agents run in sequence →
you get a dev-ready PRD that QA can write tests against before development begins.

---

## What's new in v2

- **Market & Expert Intelligence agent** — competitive analysis, behavioral design,
  UX patterns — before any solution is proposed
- **UX/UI agent** — conditional: runs only when the solution requires UI.
  Produces UX flows, design brief, wireframe descriptions.
- **Industry-grade PRD** — written for devs and QA simultaneously.
  Every requirement has acceptance criteria, dev notes, and pre-written QA test scenarios.
- **Iteration agent** — smart re-run routing. Change one thing, re-run only
  what's affected.
- **Token efficiency** — a `context_summary.md` caches all decisions.
  Agents read only what they need.

---

## What you'll need

- A Mac or PC with internet access
- A free [Anthropic account](https://claude.ai)
- Node.js ([nodejs.org](https://nodejs.org) — install the LTS version)

---

## Setup

**Install Claude Code:**
```bash
npm install -g @anthropic-ai/claude-code
claude login
```

**Clone the repo:**
```bash
git clone https://github.com/Liranzzz/pm-agent-flow.git
cd pm-agent-flow
```

**Fill in your product context** — open `CLAUDE.md` and complete the System Context section at the bottom.

---

## How to run

**Optional: drop files into `inputs/attachments/`** before running.
The intake agent reads them and skips questions they already answer.
Supported: PNG, JPG, PDF, MD, TXT, DOCX, CSV.

```bash
claude
```

Then type:
```
Run the intake agent and start the PM workflow
```

---

## Agent flow

```
[00] Intake Agent
         ↓
[01] Persona Builder ──┐  parallel
[02] Problem Definer ──┘
         ↓
[03] Market & Expert Intelligence
     (competitive analysis, behavioral design, UX patterns)
         ↓
[04] Solution Architect
     (directions + recommendation + UI gate decision)
         ↓
     UI required?
     ├── Yes/Partial → [05] UX/UI Agent (flows, design brief, wireframes)
     └── No ─────────────────────────────┐
                                         ↓
                                   [06] PRD Writer
                                   (dev + QA ready)
                                         ↓
                                   [07] Dev Critic
                                   (hidden work surfaced)
                                         ↓
                                   [08] QA Validator
                                   (score + handoff package)
                                         ↓
                              [09] Iteration Agent ← on-demand
                              (smart re-run routing)
```

---

## Output files

| File | Agent | Contents |
|------|-------|----------|
| `context_summary.md` | All agents | Decision log — what's been decided |
| `intake.md` | 00 | Approved problem summary |
| `attachment_summary.md` | 00 | What was extracted from files |
| `persona.md` | 01 | 1–3 user personas |
| `problem.md` | 02 | Problem statement, scope, metrics, assumptions |
| `market_intel.md` | 03 | Competitive analysis, behavioral design, UX patterns |
| `solution.md` | 04 | Solution directions + recommendation + UI gate |
| `ux_brief.md` | 05 | UX flows, design brief, wireframe descriptions |
| `prd.md` | 06 | Full PRD with requirements, acceptance criteria, QA scenarios |
| `dev_feedback.md` | 07 | Per-requirement critique + hidden complexity |
| `risk_register.md` | 07 | BLOCK / WARN / OK per requirement |
| `final_report.md` | 08 | Score, gaps, handoff package |

---

## Iterating

After QA validation, if you get feedback:
```
Run the iteration agent. Feedback: [paste feedback here]
```

The iteration agent will:
1. Classify the feedback
2. Identify the minimum agents to re-run
3. Show you the plan before executing
4. Update only what changed

---

## Handoff

When the QA Validator returns **READY FOR HANDOFF ✅**, share with your team:
- `outputs/prd.md`
- `outputs/ux_brief.md` (if it exists)
- `outputs/risk_register.md`

QA can start writing test cases from `prd.md` immediately.
Dev can start from any P0 requirement.

---

## Fine-tuning

Each agent file in `agents/` has a **Fine-tune hints** section.
If an agent's output is weak, add the relevant hint to that agent's prompt and re-run.
