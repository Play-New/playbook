---
description: Autonomous build from strategy and design. Vision conversation, then tests that encode the vision, then autonomous construction until the tests pass. Stops only on hard failure.
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, WebSearch, WebFetch
---

# Build

Read `reference/build-principles.md` before every build decision.

## Rule Zero

Before adding anything — a screen, a component, an endpoint, a table, a setting, a dependency — ask:

1. **Does this trace to the EIID mapping?** If not, don't build it.
2. **Is there a simpler way?** Fewer screens. Fewer components. Fewer abstractions. The simplest version that delivers the value.
3. **Does this earn its place?** Every element a user sees, every endpoint, every table column must justify its existence against the user need. If removing it wouldn't hurt the outcome, remove it.

This is not a UI guideline. It applies to everything: data models, API routes, background jobs, agent tools, visual surfaces. A product with 3 screens that each do one thing perfectly beats a product with 12 screens. A schema with 4 tables that model the real relationships beats 10 tables that model hypothetical ones.

Rule zero overrides all other build decisions. When in doubt, build less.

## Detect Mode

**1. Does a strategy exist?** Check for CLAUDE.md with an EIID mapping section.

**2. Route:**
- **No EIID mapping** → stop. "No strategy found. Run `/super:strategy` first."
- **EIID mapping exists + no built code** (empty src/ or app/) → run **init mode**
- **EIID mapping exists + built code + user provided a target** → run **extend mode**
- **EIID mapping exists + built code + no target** → "Project has code. Provide a target to build."

---

## Init Mode

### 1. Vision

Before anything technical, have a conversation. Ask the user:

**"Describe what this product should do. Not features — the experience. What does the user see, feel, do? When is this product perfect? Use your own words. Examples, references, screenshots, links — anything that helps me see what you see."**

Do not interrupt with technical questions. Do not ask about frameworks, databases, or architecture — the strategy already covers that. This is about the experience.

Collect everything the user shares:
- Descriptions in their own words
- Examples of products they admire (and what specifically they admire)
- Screenshots, Figma links, references
- User flows described as stories ("the user opens the app, sees X, does Y, then Z happens")
- What "perfect" means to them — the bar, not the floor
- What they explicitly do not want

**But do not just listen. Challenge.** As the user describes their vision, push back where it matters:

- **Complexity that doesn't serve the user.** The user described a flow with 4 steps? Maybe 2 would work. They want a dashboard? Maybe a single notification at the right moment delivers the same value. They described a settings page? Maybe the behavior can be inferred from usage.
- **Missing pieces.** What happens when something fails? What does the first experience look like with no data? What about the user who doesn't speak the primary language? These are moments the user will notice — propose how to handle them.
- **Better patterns.** The user described a visual surface, but the EIID mapping shows conversational delivery. Maybe a WhatsApp message at the right time beats a dashboard the user checks once a week. Or the user described a chatbot, but the real value is a single well-timed notification.
- **Contradictions.** They want it minimal but described 6 different surfaces. They want it fast but described a complex agent loop. Which matters more?
- **Things they take for granted.** What does "good" mean for an agent response? How should errors feel? What's the voice? These are design decisions, not afterthoughts.

The goal is not to override the user's vision. The goal is to make the vision sharper. The user brings the intent and the taste. Build brings the critical eye and the experience of what works.

When the conversation reaches alignment: "Anything else, or should I show you what I'd build?"

### 2. Experience Proposal

Show the user what you understood. Not a technical plan — a description of the experience you will build.

**For each touchpoint the user encounters** — a screen, a conversation, a CLI command, a notification, an agent interaction, a workflow output:
- What the user perceives when they arrive
- What they can do
- How it feels — density, pace, character, tone
- The shape of the interaction: if visual, what's where and what's prominent. If conversational, how the exchange flows. If CLI, what the output looks like. If notification, what it says and when.

**For each flow the product has:**
- Step by step, what happens from the user's perspective
- Where intelligence shows up (LLM calls, agents, workflows) and how the user perceives it
- Waiting moments, transitions, feedback — described as experience, not as components

**For what you would NOT build:**
- Surfaces the user might expect but that aren't needed
- Settings that can be inferred
- Features that don't trace to EIID

**For the signature:**
- The one thing that makes this product recognizable and different from a generic version of the same idea

Write this as a narrative, not as a bulleted plan. The user should be able to read it and say "yes, that's what I want" or "no, you missed the point."

The user corrects, adds, removes. Iterate until they say it's right. **This is the creative checkpoint.** Everything after this is autonomous.

### 3. Readiness

Now check the technical foundation. Read CLAUDE.md, `.superskills/design-system.md`, `.superskills/decisions.md`, `.superskills/report.md` (if they exist).

Three gates:

- **Strategy:** EIID mapping with Approach fields on all layers.
- **Stack:** package.json with framework installed. If missing, install from CLAUDE.md.
- **Design:** if any EIID layer needs a visual surface, `.superskills/design-system.md` must have Direction and Tokens. If no visual layers, skip.

Fail → tell the user what's missing and which command to run. Stop.

**Human prerequisites.** Check once:
- Git repo, Node.js, package manager on PATH
- Service credentials in `.env.local` for each service in the EIID mapping (database, APIs, LLM provider)
- Missing? Tell the user what to set up. Don't proceed without it.

### 4. Tests First

Before writing a single line of product code, write the complete test suite that encodes the vision.

**Install what the project needs.** Read the EIID mapping and the experience proposal. The product's shape determines the testing infrastructure:
- Visual surfaces need browser-based tests that verify what the user sees and does.
- API routes, business logic, and data pipelines need integration tests that verify behavior.
- Agent and LLM behavior needs tests that verify response structure, tone, and content — not prompt internals.
- CLI products need tests that verify output format, exit codes, and cross-channel consistency.
- Conversational products need tests that verify message structure, timing, and interaction flow.

Detect existing test infrastructure first. Install only what's missing. Use whatever test runners fit the project's stack.

**Write tests from the experience proposal, not from implementation plans.** Each test describes what the user experiences — across any modality:

```
// Visual product — test the experience, not the component
test('user arrives at fleet overview, sees vehicles sorted by urgency, most critical first')
test('when no vehicles need attention, the overview says so clearly — no empty table')

// Conversational product — test the interaction, not the prompt
test('user sends a pantry photo, receives recipe suggestions within 30 seconds')
test('user asks to substitute an ingredient, agent responds with alternative and adjusted recipe')
test('agent response leads with the recommendation, not with an explanation')

// CLI product — test the output and behavior, not the internals
test('scan command outputs risk table sorted by severity, fits in 80 columns')
test('when no breaking changes found, exits with code 0 and a single line confirmation')

// Bad: tests the implementation
test('VehicleTable component renders with sorting prop')
test('API returns 200 with vehicle array')
test('prompt includes system message with role definition')
```

The tests are the contract. They encode everything the user described in the vision phase. Every flow, every touchpoint, every interaction that matters. If a test doesn't trace to something the user said in the vision conversation, it doesn't belong.

**Run the tests.** They should all fail. This is correct — the product doesn't exist yet. But they must be syntactically valid and structurally sound. Fix any test that fails for the wrong reason (import errors, config issues, bad selectors).

Write the test plan to `.superskills/build-plan.md` — this is the ground truth. The plan is the tests.

### 5. Build Loop (autonomous)

The tests define the work. Build until they pass. All of them.

Skills (eiid-awareness, design-awareness, build-awareness) fire during the loop. They are advisory — the build incorporates their feedback automatically and logs observations, but does not stop for them. Only hard failures stop the loop.

**Order:** infrastructure → intelligence → surfaces. Build the foundation first, then the smart parts, then what the user sees. But always with the end experience in mind — the infrastructure exists to serve the surface, not the other way around.

**For each piece of the product:**

1. **Build** with the full context: EIID mapping, design system, experience proposal, target feeling. For visual surfaces, build with the design tokens, typography, composition, the direction's character from line one. Every visual element earns its place.

2. **Run the full test suite** after each piece. Track which tests flip from failing to passing. This is the progress indicator.

3. **On regression** (a previously passing test breaks): revert, analyze, try a different approach. Maximum 5 attempts. If all 5 regress, skip the piece and report. Never leave a regression in place.

4. **On stuck** (a test won't pass after 5 iterations): log what was tried, skip to the next piece. The build continues.

5. **Verify beyond tests:**
   - Types check (`npx tsc --noEmit`)
   - If visual: structural verification — correct tokens, components, layout patterns. Flag screens that need the user to see the running product.
   - **Feeling check:** re-read the target feeling from CLAUDE.md and the experience patterns from `.superskills/design-system.md`. For every touchpoint the user perceives, does it carry the product's character? Would removing any element hurt the experience? If not, remove it.

6. **Log** to `.superskills/decisions.md`. Re-read the log before the next piece — if earlier pieces had issues or patterns, use that context.

**The loop does not stop until:**
- All tests pass, OR
- All remaining failing tests have been attempted 5 times each

**Context drift is real.** On builds with 5+ pieces, re-read the experience proposal, CLAUDE.md, and `.superskills/design-system.md` before each piece. For very long builds, consider splitting into independent sessions per piece: each session reads from disk, builds one piece, commits, exits. The filesystem is the memory, not the conversation.

### 6. Report

After all tests pass (or all skipped pieces are logged), present the report and write it to `.superskills/report.md`:

```
## Build Report

Tests: [pass]/[total]
Skipped: [count] (with reasons)
EIID coverage: [which layers are implemented]
Visual review needed: [list screens that need the user to see the running product]

Skipped pieces:
- [piece]: [what failed, what was tried, proposed fix]
```

---

## Extend Mode

Add a feature or implement a piece on top of existing code.

### 1. Vision

Same conversation as init mode, but scoped. Ask the user:

**"Describe what this change should do. What does the user experience today, and what should they experience after? When is this change perfect?"**

Collect their description, examples, references. Challenge the same way as init mode: is this the simplest path? Are there missing edge cases? Is there a better experience pattern? Push back where it makes the change sharper. Confirm understanding when aligned.

### 2. Context

Read CLAUDE.md, `.superskills/decisions.md`, `.superskills/design-system.md`, `.superskills/report.md` (if they exist). Understand what exists and what review flagged.

Map the target to EIID. If it doesn't trace to any layer, flag it. Don't build scope creep silently.

### 3. Experience Proposal

Show what you'd change. Not a diff — a description of the new experience. What the user will see differently. What stays the same. What you'd remove.

Rule zero: is this the simplest way to deliver this value? Challenge the scope before committing.

The user approves before building.

### 4. Tests First

Write new e2e and integration tests that encode the change. Run the full existing suite to confirm it passes before you start — establish the baseline.

Write (or update) `.superskills/build-plan.md`.

### 5. Build

Same loop as init mode. Same autonomy. Same rollback rules. Build until the new tests pass and no old tests break.

### 6. Report

Full test suite results. Log to `.superskills/decisions.md`. Report.

---

## Rules

- **Rule zero above all.** Build less. Every piece earns its place.
- **Vision before plan.** The user describes perfection in their own words. Build understands it before proposing anything.
- **Tests are the plan.** The experience proposal becomes e2e tests. The tests are the spec, the progress tracker, and the quality gate. All in one.
- **Install what you need.** Browser tests, integration tests, CLI tests — whatever this product requires. The test infrastructure is not optional — set it up automatically.
- **Autonomous after vision approval.** No checkpoints during the build. Log, don't ask.
- **Don't stop until it's right.** The build loop runs until all tests pass. Not "mostly pass." All of them.
- **Five iterations max per piece.** Then skip and report. Don't block the whole build.
- **The strategy is truth.** If what you're building contradicts CLAUDE.md, stop and report. Don't deviate silently.
- **Design is not decoration.** The direction, the signature, the character — present in every piece from line one.
- **Prompts are visible.** Every LLM prompt is discoverable, inspectable, and if user-facing, editable. No buried prompts in business logic.
- **The vision is the bar.** Not "it works." Not "it's close." The result matches what the user described — whether that's a screen, a conversation, a CLI output, or an agent interaction. If it doesn't, iterate until it does or report why it can't.
