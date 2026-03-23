# CLAUDE.md Template

The plugin generates this file via `/super:strategy`. It contains stable project instructions that Claude reads at session start. Target: under 100 lines. Findings go to `.superskills/`, not here.

---

```markdown
# [Project Name]

## Business
**Client:** [who is paying]
**Industry:** [sector, size, geography]

## User
**End user:** [role, daily context]
**Need:** [outcome, not feature]
**Target feeling:** [the emotional state when the product works perfectly — e.g. "calm control", "warm discovery", "precise confidence"]

## Stack
[detected or recommended, with rationale]

## EIID

### Enrichment (collect)
**Have:** [sources already connected]
**Human input:** [how people naturally feed data — photos, voice, chat, email, manual entry]
**Missing:** [gaps to fill]
**Connect:** [systems to integrate]
**Approach:** [automate / differentiate / innovate — per component] via [LLM call / workflow / agent / code / buy]. Graduation: [trigger, optional]

### Inference (patterns)
**Detect:** [what to spot]
**Predict:** [what to forecast]
**Flag:** [anomalies to catch]
**Approach:** [automate / differentiate / innovate — per component] via [LLM call / workflow / agent / code / buy]. Graduation: [trigger, optional]

### Interpretation (insights)
**Surface:** [what to tell the user]
**Frame as:** [comparison, trend, explanation, recommendation]
**Approach:** [automate / differentiate / innovate — per component] via [LLM call / workflow / agent / code / buy]. Graduation: [trigger, optional]

### Delivery (reach)
**Channels:** [same channels used for input — the system is invisible, not a destination]
**Triggers:** [when to send]
**Timing:** [optimal moment]
**Approach:** [automate / differentiate / innovate — per component] via [LLM call / workflow / agent / code / buy]. Graduation: [trigger, optional]

## Technology Constraints
[detected constraints, one per line: "Use X, NOT Y, Z."]

## Code Architecture
[Derive from the EIID mapping and stack. What matters for THIS project's structure? Examples of project-specific decisions:
- A CLI tool: single entry point, no routing, output to stdout
- A visual SaaS: page-per-route, server components for data, client components for interaction
- A WhatsApp-first product: message handlers organized by intent, shared formatting layer
- A multi-agent system: one directory per agent, shared tool registry, isolated state per agent
Write 3-5 architectural decisions specific to this project. Do not include generic coding advice.]

If EIID mapping includes components via agent, workflow, or LLM call:
- **Parity:** every action available in the UI is also available as an atomic tool for agents.
- **Granularity:** tools are primitives (look up product, check inventory), not features (process order).
- **Tests as reward signal:** tests define the outcome, the agent iterates until they pass. Write tests first, then let the agent work.
- **External state:** progress and decisions persist to filesystem or database, never to conversation history.
- **Graduation markers:** comments in code with trigger conditions. `// via workflow, graduate to code when >500 recipes/day`

## Design System
**Framework:** [detected or recommended]
**Token source:** [globals.css / theme.ts / etc.]
```
