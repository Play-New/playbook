# Concepts

Canonical definitions. Every concept defined once. Commands and skills reference this file.

---

## The Four Layers (EIID)

Four layers that structure every intelligence-era product. Value flows from raw data to user outcome. Every component (node) traces to exactly one layer.

**Enrichment** — where data enters. Photos, voice notes, chat messages, forwarded emails, spreadsheets, APIs, scraped sources. People should not change how they work. Enrichment accepts data from every channel they already use and normalizes it for inference.

**Inference** — pattern detection, prediction, anomaly flagging. The compute is cheap. The hard part is knowing what questions to ask.

**Interpretation** — turns raw inference into something a person can act on. "Anomaly score 0.87" means nothing. "Orders dropped 30%, likely due to budget freeze, renewal in 45 days" is an insight. Context, comparison, explanation, recommended action.

**Delivery** — returns results through the channels people already use. Triggered by conditions: threshold crossed, schedule, event, user request. The critical design choice is timing.

## Node

A component of the product that belongs to exactly one layer. The unit of decomposition, analysis, and measurement. Each node carries five fields:

- **Layer** — which layer it serves
- **Evolution** — where it sits on the Wardley axis (genesis, custom, product, commodity)
- **Metric / signal** — what you measure and the target value
- **Graduation** — when to change approach and what to change to (condition + direction)
- **Loop** — whether the node is optimizable automatically or requires human judgment

## Evolution

Four stages on the Wardley axis. Each carries strategic meaning for the node.

- **Genesis** — new, uncertain, no established approach. This is where you create value. Invest here.
- **Custom** — understood in your domain but not standardized. Build it, but expect it to evolve.
- **Product** — well-understood, multiple approaches exist. Use existing solutions, adapt to your context.
- **Commodity** — standardized, many providers. Buy it. Building commodity is waste.

## Metric vs Signal

**Metric** — measurable automatically with fast feedback. Accuracy, precision, recall, latency, coverage, cost per unit. Enables autoresearch: change the implementation, measure, keep or discard. Typical of Enrichment and Inference nodes.

**Signal** — observable by humans with slow feedback. Acceptance rate, time to action, satisfaction, fatigue rate. Requires manual review. Typical of Interpretation and Delivery nodes.

Every node has one or the other. A node with neither metric nor signal is flying blind.

## Autoresearch

Automated optimization loop for nodes with a clear metric and fast feedback. Based on Karpathy's autoresearch framework, extended by Shopify (Cortés, Lütke).

Autoresearch does not do human work faster. It explores optimization spaces too large and tedious for humans to attempt. Small improvements of 1% compound over hundreds of iterations into results no human would find. Shopify found a 65% build speedup and 300x faster unit tests this way — optimizations that nobody would have searched for manually.

The mechanism:

1. **One mutable file.** The agent can only change one file per node: the prompt, the config, or the logic file. Everything else is frozen. This prevents the agent from changing the evaluation to make the metric look better.
2. **One metric.** The node's metric from the playbook mapping, computed automatically against an evaluation set. One number, no ambiguity.
3. **Fixed time budget.** Each experiment must complete within a fixed time (evaluation included). This makes experiments comparable regardless of what the agent changed.
4. **Confidence scoring.** A single improvement means nothing. Each experiment runs 3+ times. Improvement is accepted only if statistically significant — Median Absolute Deviation separates real gains from noise.
5. **Backpressure checks.** After a benchmark passes, run correctness checks (tests, types, lint). If correctness breaks, reject the improvement even if the metric improved. The metric gets better AND the system stays correct.
6. **Git as keep/discard.** `git commit` when the improvement is both real (confidence) and correct (backpressure). `git reset --hard` when it isn't. Improvements accumulate. Failures vanish.
7. **Autonomous loop.** The agent reads the code, forms a hypothesis, makes one change, measures, validates, keeps or discards. No human in the loop. Runs until convergence (improvements < 1% over 10 experiments) or budget exhausted.

Works well on Enrichment and Inference nodes (clean metrics, fast evaluation). Partially applicable to Interpretation (prompt clarity, measurable against evaluation set).

The strategy identifies which nodes are autoresearch candidates. Build sets up the loop: creates the evaluation set, designates the mutable file, configures the metric. Review evaluates convergence and triggers cycles when a node is below target.

## Interface

The Interface is where Enrichment and Delivery coincide. It is not a layer. It is not a node. It is the surface where the product touches the user.

Every interaction through the Interface is simultaneously delivery (the product responds) and enrichment (the product learns). A recruiter editing a rewrite before accepting it is receiving value and generating the richest training signal in the system. A dashboard where a user searches for a competitor not yet tracked is delivering what exists and revealing what is missing.

When the Interface captures what users accept, ignore, modify, and ask for that doesn't exist, every interaction feeds Enrichment and Inference. When the Interface captures nothing, the product is blind to its own users.

When the Interface is both sensor and surface, autoresearch happens naturally at the speed of customer interaction. The customer uses the product, the product observes the response, the observation feeds earlier layers. This is not a separate mechanism from sandbox autoresearch — it is the same loop (change, measure, keep or discard) operating through use rather than through controlled experiment.

A static dashboard is product-stage — replaceable. A conversational interface that composes responses from capabilities in real time is genesis-stage — it accumulates a world model. When a customer asks for something the system can't compose from its capabilities, that gap is the roadmap. The judgment is whether filling it aligns with what the product should be.

## World Model

The World Model is the accumulated understanding of users built from every interaction that passes through the Interface. It is not a layer. It is not a field. It is the state that the product accumulates over time.

Every capability in isolation is replicable — document parsing, scoring, notification routing. The World Model is not. It exists only because of the specific history of interactions that built it. The product with the richest Interface builds the deepest World Model. The deepest World Model produces better responses. Better responses generate more use. The loop compounds.

The strategic question is not about individual nodes. It is: does this product build a World Model? A product where every interaction deepens understanding has a compound advantage. A product that pushes value out and captures nothing back is replicable regardless of how sophisticated its inference is.

## Graduation

Nodes evolve. When a metric consistently exceeds its target, simplify the implementation. When volume exceeds a threshold, make it more robust. Graduation goes both directions:

- **Up** — simple approach hits limits, edge cases justify complexity
- **Down** — patterns stabilize, what was experimental becomes routine

Each node documents its graduation trigger: both the condition (when) and the direction (what changes). "When accuracy exceeds 95% for 2 weeks, replace with deterministic rules" is a complete trigger. "When accuracy exceeds 95%" is not — it says when but not where to go.

## Context Engineering

Every output of this plugin is structured context for AI agents. CLAUDE.md is the strategic context document of the product. If an agent reads it, it knows: where the value is, what to build, what to measure, when to change approach.

Context must stay faithful to reality (context fidelity). When the product evolves, the context updates. Stale context is worse than no context — it leads agents to optimize the wrong things.

## Plugin Learning

The plugin applies its own framework to itself. Every interaction with a user is signal:

- **User overrides a classification** → the node classifier was wrong. Log the override and the reasoning to `.playbook/report.md`. Over time, these overrides reveal which archetypes are stable ("document ingester" is always commodity Enrichment) and which require judgment.
- **A challenge changes the user's plan** → the challenge worked. Log which challenge pattern fired and what changed. Over time, the most effective challenges surface.
- **Review finds stale context** → the CLAUDE.md generator or the strategy missed something. Log what drifted and why. Over time, the template and research improve.
- **A research lens produces no useful signal** → the lens was wrong for this domain. Log which lens failed and what the domain was. Over time, research adapts to domain.

These signals persist in `.playbook/report.md` within each project. The plugin reads prior reports when running strategy refresh or review, learning from its own history. The compound loop is: strategy → build → review → strategy, and each cycle through it makes the mapping more faithful and the challenges sharper.
