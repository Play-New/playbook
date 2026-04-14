```
    ┌───────────────────────────────────────────────┐
    │                                               │
    │   ██████╗  ██╗      █████╗ ██╗   ██╗         │
    │   ██╔══██╗ ██║     ██╔══██╗╚██╗ ██╔╝         │
    │   ██████╔╝ ██║     ███████║ ╚████╔╝          │
    │   ██╔═══╝  ██║     ██╔══██║  ╚██╔╝           │
    │   ██║      ███████╗██║  ██║   ██║            │
    │   ╚═╝      ╚══════╝╚═╝  ╚═╝   ╚═╝            │
    │                                               │
    │   ██████╗  ██████╗  ██████╗ ██╗  ██╗         │
    │   ██╔══██╗██╔═══██╗██╔═══██╗██║ ██╔╝         │
    │   ██████╔╝██║   ██║██║   ██║█████╔╝          │
    │   ██╔══██╗██║   ██║██║   ██║██╔═██╗          │
    │   ██████╔╝╚██████╔╝╚██████╔╝██║  ██╗         │
    │   ╚═════╝  ╚═════╝  ╚═════╝ ╚═╝  ╚═╝         │
    │                                               │
    │   the real game is changing the game         │
    │                                               │
    │   enrichment ── inference ──┐                  │
    │                             │                  │
    │              interpretation ─── delivery       │
    │                                               │
    │   data in ·· value out ·· signal back          │
    │                                               │
    └───────────────────────────────────────────────┘
```

A Claude Code plugin that decomposes intelligence-era products into a value chain, identifies where the real value sits, and produces structured context that tells both people and agents where to invest effort.

### Install

1. Register the marketplace (once):

```bash
claude plugin marketplace add Play-New/playbook
```

2. Install the plugin:

```bash
claude plugin install playbook@playbook
```

3. Verify:

```bash
claude plugin list
```

You should see `playbook@playbook — Status: ✔ enabled`. Restart Claude Code after installing. Three commands become available: `/playbook:strategy`, `/playbook:build`, `/playbook:review`.

---

## 1. The problem

The architecture of most software products does not reflect that intelligence is now a commodity input. Teams ship dashboards, CRUD, and forms — the same things they shipped a decade ago, just faster. The question "how do I build this?" has a cheap answer. The question "where does the intelligence go, and what is it worth?" does not.

Three failure modes recur:

1. **Building commodity as custom.** Teams invest engineering effort in components that exist as services. Embedding search, document parsing, notification routing — these are solved problems. Building them is waste.
2. **No inference layer.** The product collects data and presents it, but never asks what patterns, predictions, or anomalies live inside it. The intelligence layer is absent.
3. **No way to know if it works.** The most valuable component — the one that creates the product's reason to exist — has no metric, no signal, no way to tell if it is improving or degrading.

## 2. The model

Every intelligence-era product moves data from raw input to user value through four layers:

| Layer | Role | Example |
|-------|------|---------|
| **Enrichment** | Where data enters. Normalizes inputs from every channel the user already uses. | Price scraping, document parsing, voice transcription |
| **Inference** | Pattern detection, prediction, anomaly flagging. The compute is cheap; knowing what questions to ask is not. | Anomaly detection, matching, classification |
| **Interpretation** | Turns raw inference into something a person can act on. "Anomaly score 0.87" means nothing. "Orders dropped 30%, likely budget freeze, renewal in 45 days" is an insight. | Recommendations, explanations, suggested actions |
| **Delivery** | Returns the right insight through the right channel at the right moment AND captures signal from how people respond. Every delivery surface is also a sensor. | Push notifications, digests, dashboards, conversational interfaces |

The value flow is not a pipeline — it is a cycle:

```
    Enrichment → Inference → Interpretation → Delivery
        ↑                                        │
        └──────── Feeds (backward signal) ────────┘
```

What the user accepts, ignores, modifies, or asks for and doesn't get flows back as signal to the earlier layers. A dashboard where users search for a competitor not tracked feeds signal back to Enrichment (coverage gap). An alert consistently ignored feeds signal back to Inference (precision is wrong). A recommendation edited before acceptance feeds signal back to Interpretation (the system got it partially wrong — the edit shows what).

This applies to products and to organizations. An organization's delivery surfaces — client reports, internal dashboards, team communications — are sensors too. What people ask for that the system can't answer is the roadmap.

The compound loop is the moat. Every capability in isolation is replicable — card processing, lending, document parsing, notification routing. What is not replicable is the accumulated understanding of customers built from every interaction. This is the world model: a living intelligence that gets deeper with every use. The product with the richest Feeds builds the deepest world model. The deepest world model composes the best responses. The best responses generate more use. The cycle compounds. A competitor can copy the capabilities. They cannot copy the world model — it only exists because of the history of interactions that built it.

Playbook decomposes a product into **nodes**, each belonging to exactly one layer. Every node carries six fields:

| Field | What it captures |
|-------|-----------------|
| **Layer** | Enrichment, inference, interpretation, or delivery |
| **Evolution** | Where the node sits: genesis (new, uncertain — invest here), custom (understood but not standardized), product (multiple approaches exist), commodity (buy it). A static dashboard is product. A conversational interface that composes itself from capabilities is genesis — it accumulates a world model and creates a compound moat |
| **Metric / Signal** | What you measure and the target. Metric for things with fast, automatic feedback (accuracy, precision, latency). Signal for things requiring human observation (acceptance rate, fatigue, time to action) |
| **Graduation** | Two parts: *when* to change approach (the trigger) and *what* to change to (the direction). "Accuracy >95% for 2 weeks → replace with deterministic rules" is complete. "Accuracy >95%" alone is not — you know when to act but not what to do |
| **Loop** | Whether the node can be optimized automatically (autoresearch), requires human judgment (manual review), or is commodity (N/A) |
| **Feeds** | Which nodes this enriches through use, against the normal EIID direction, and with what signal. A node with no backward signal is terminal (`—`). Every Delivery node should declare this |

The decomposition is then challenged. A pitch that says "we'll build a RAG system" gets: RAG is how, what is the what? A brief with six features but no inference layer gets: where are the patterns? A commodity node built custom gets: this exists as a service, buy it. No genesis nodes gets: where are you creating new value? A Delivery node with no Feeds gets: your product doesn't learn from use.

## 3. The method

Playbook operates in three phases. Each is a command; together they form a cycle.

### 3.1 Strategy (`/playbook:strategy`)

Takes any input — a raw brief, a pitch deck, an idea in conversation, an existing codebase. Reads what exists, researches the problem space (searching for what would make the brief *wrong*, not for confirmation), and produces the decomposition.

Two outputs. A **strategic assessment** for the people deciding: where the value is, where the risk is, what to do first, what not to do. And a **CLAUDE.md** for the agents building: structured context that says exactly which nodes are genesis (invest here), which are commodity (buy, don't build), what to measure, and when to change approach.

This is context engineering. If an AI agent reads the CLAUDE.md tomorrow with zero prior knowledge, it knows where to invest effort and where not to. Stale context is worse than no context, so the document is treated as a living artifact, not a one-time deliverable.

### 3.2 Build (`/playbook:build`)

Starts with a vision conversation: what does the user experience when this product is perfect? Then encodes that vision as tests before writing any product code. The test suite *is* the plan — it tracks progress and gates quality.

For nodes marked "autoresearch" in the mapping, build sets up an automated optimization loop based on Karpathy's framework:

1. **One mutable file.** The agent can only change one file per node (prompt, config, or logic). Everything else is frozen. This prevents the agent from changing the evaluation to make the metric look better.
2. **One metric.** Computed automatically against an evaluation set. One number, no ambiguity.
3. **Fixed time budget.** Each experiment completes within a fixed window, making experiments comparable.
4. **Git as keep/discard.** `git commit` when the metric improves. `git reset --hard` when it doesn't. Improvements accumulate. Failures vanish.

An Enrichment node matching products to SKUs can run 12 experiments per hour this way. An Interpretation node generating pricing recommendations cannot — the signal (did the seller accept it?) takes days. The mapping captures this distinction explicitly.

A second form of autoresearch operates in production through Delivery:

```
    Sandbox autoresearch          Production autoresearch
    (Karpathy)                    (through Delivery)

    agent changes file            customer uses interface
         │                              │
    run evaluation                system composes response
         │                              │
    metric improves?              customer got what they wanted?
      ┌───┴───┐                     ┌───┴───┐
     yes     no                    yes     no
   commit   reset               signal    signal
                              (reinforce) (gap → roadmap)
         │                              │
    converge                    feeds back to
    or stop                     other nodes
```

The customer is the agent, the interface is the mutable surface, the usage is the measurement. The product improves itself through use. This applies to products (a dashboard that reveals coverage gaps) and to organizations (a team communication channel that reveals where information doesn't flow).

### 3.3 Review (`/playbook:review`)

Measures the product against the mapping. For each node: is the metric above, at, or below target? Has a graduation trigger fired? For autoresearch nodes: is the loop converging, diverging, or not set up? For the mapping as a whole: does it still match the product?

When a node is below target and has an autoresearch loop, review doesn't just report — it runs an optimization cycle. The system improves itself where it can.

Two skills fire automatically between explicit commands. **Playbook awareness** activates during planning: does this change serve a node? Is the evolution classification still accurate? Has a graduation trigger fired? **Build awareness** activates during implementation: does this code trace to a node? Does it change what the CLAUDE.md says about the product?

### 3.4 Graduation

Nothing in the mapping is permanent. Nodes evolve in both directions:

- **Up.** A simple approach hits limits, edge cases justify complexity. A rules-based classifier needs a model.
- **Down.** Patterns stabilize, what was experimental becomes routine. A model-based classifier becomes a lookup table.

Each node documents both the condition ("when accuracy exceeds 95% for 2 weeks") and the direction ("replace with deterministic rules"). A trigger without a direction is incomplete.

## 4. Examples

Three decompositions across different domains (full detail in `reference/example.md`).

### 4.1 PriceScope — pricing intelligence for e-commerce

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Price scraper | Enrichment | commodity | coverage 80%+ | if coverage drops, build custom | N/A (buy) | — |
| Product matcher | Enrichment | custom | accuracy on test set | >95% stable → rules | autoresearch | — |
| Anomaly detector | Inference | custom | precision + recall | >10K SKUs → rules for known patterns | autoresearch | — |
| Price recommendation | Interpretation | **genesis** | acceptance rate | patterns repeat → auto-rules per category | manual review | — |
| Alert dispatcher | Delivery | product | fatigue rate <40% | fatigue >40% → reduce frequency | manual review | ignored alerts → anomaly detector, action timing → routing rules |

Genesis is in Interpretation. Autoresearch applies to two Enrichment/Inference nodes with fast feedback. Enrichment is commodity — buy it. The alert dispatcher feeds signal back to Inference — ignored alerts mean the anomaly wasn't worth flagging.

### 4.2 TalentVoice — AI-augmented recruiting language

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Document ingester | Enrichment | commodity | ingestion success rate | new ATS breaks it → custom parser | N/A (buy) | — |
| Outcome linker | Enrichment | custom | linkage rate | >95% for an ATS → freeze connector | autoresearch | — |
| Bias pattern detector | Inference | custom | precision + recall | pattern universally known → blocklist | autoresearch | — |
| Rewrite recommendation | Interpretation | **genesis** | acceptance rate + outcome delta (30-90d) | >90% acceptance → auto-apply | manual review | — |
| Editor overlay | Delivery | product | time to action, dismissal rate | >50% auto-accepted → auto-apply | manual review | edits before accepting → rewrite recommendation, dismissed suggestions → bias detector |

Genesis is in Interpretation, but feedback is structurally slow (30-90 days). The real moat is in Enrichment — the outcome linker is a data asset. The editor overlay is the hidden training loop: when a recruiter edits a rewrite before accepting, the edit shows exactly what the recommendation got wrong.

### 4.3 BrandLens — content governance and brand voice

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Content collector | Enrichment | commodity | coverage, freshness | no API → custom scraper | N/A (buy) | — |
| Style guide encoder | Enrichment | custom | rule coverage, conflict rate | conflicts at zero → stable | manual review | — |
| Compliance scorer | Inference | product | accuracy vs experts, FP rate | FP >15% → simplify rules | autoresearch | — |
| Consistency analyzer | Inference | **genesis** | cross-channel variance, drift latency | stable 6mo → reduce monitoring | autoresearch (weekly) | — |
| Writer guidance | Delivery | product | time to correction, acceptance rate | pass rate >95% → advisory mode | manual review | corrections before flags → compliance scorer, rejected suggestions → style guide encoder, new compliant patterns → consistency analyzer |

Genesis is in Inference, not Interpretation. The value is in detecting brand drift — the detection itself is the insight. Two autoresearch loops at different speeds. Writer guidance is the densest sensor — it feeds three nodes despite being product-stage.

### What the three examples show together

| Pattern | PriceScope | TalentVoice | BrandLens |
|---------|-----------|-------------|-----------|
| Genesis layer | Interpretation | Interpretation | Inference |
| Feedback speed | days | 30-90 days | weekly |
| Enrichment | commodity | **strategic asset** | mixed |
| Autoresearch nodes | 2 (both fast) | 2 (not the genesis) | 2 (different speeds) |
| Where the moat is | interpretation quality | enrichment data | inference detection |
| Delivery feeds | 1 node (anomaly detector) | 2 nodes (rewrite rec, bias detector) | **3 nodes** (scorer, encoder, analyzer) |

The genesis node is not always in the same layer. The moat is not always where you'd expect. Autoresearch helps where feedback is fast, but the most valuable node often has the slowest feedback. The Delivery layer — even when product-stage — is often the richest sensor in the system. A product where Delivery feeds no other node doesn't learn from use.

## 5. References

- **Wardley, S.** Value chain mapping and evolution. The foundation for positioning components on the genesis-to-commodity axis and making build-vs-buy decisions based on evolutionary stage rather than intuition.
- **Choudary, S. P.** Platform dynamics and AI-driven industry restructuring. The lens for understanding how intelligence as a commodity input changes where value accumulates in a product.
- **Karpathy, A.** Autoresearch: automated experimentation with one mutable file, one metric, fixed time budget, and git as keep/discard mechanism. The operational model for nodes with clean metrics and fast feedback.

## License

MIT
