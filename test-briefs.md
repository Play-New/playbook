# Framework stress test — 5 briefs

Test the 6-field model (Layer, Evolution, Metric/Signal, Graduation, Loop, Feeds) across diverse domains. For each: decompose, check if Feeds adds insight or is just noise.

---

## Brief 1: CashGuard — personal cash flow protection (Block/Dorsey type)

**Client:** Cash App users living paycheck to paycheck.
**Value expected:** Never go to zero. Rent, Spotify, kids' allowance — all sequenced so the user always has a cushion.

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Transaction ingester | Enrichment | commodity | coverage (% accounts linked), freshness | if a bank API breaks → build custom connector | N/A (buy) | — |
| Spending pattern detector | Inference | custom | prediction accuracy (actual vs predicted balance 7d out) | >90% accuracy 4 weeks → reduce model complexity | autoresearch | — |
| Cash flow advisor | Interpretation | **genesis** | % users who follow sequencing advice, balance improvement at 30d | when advice patterns stabilize for an income bracket → auto-sequence | manual review | — |
| Conversational interface | Delivery | **genesis** | queries resolved without escalation, feature requests that don't exist | when >80% queries composable from existing capabilities → reduce human fallback | manual review | unresolved queries → spending pattern detector (unknown patterns), feature requests → capability gap (roadmap), ignored advice → cash flow advisor (advice wasn't useful) |

### What Feeds reveals
The conversational interface is genesis — not because chat is hard, but because it **composes responses from capabilities in real time** and builds a world model from every interaction. Two genesis nodes (interpretation + delivery) with the Delivery feeding signal back to both Inference and Interpretation. The compound loop IS the product. Without Feeds, this looks like "a chatbot." With Feeds, it's clear the interface is the moat.

### Feeds useful? **Yes — this is the Block pattern. Without Feeds, the decomposition misses the core insight.**

---

## Brief 2: Playbook itself — the plugin

**Client:** Founders, product leaders, AI agents building intelligence-era products.
**Value expected:** Know where to invest, when to change approach, and what to leave alone.

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Brief scanner | Enrichment | product | % of brief content successfully parsed and used in decomposition | if a format breaks parsing → add parser | N/A | — |
| Web research (5 lenses) | Enrichment | custom | % of mapping fields filled with research evidence (not intuition) | when research consistently produces 4/5 lenses useful → freeze query structure | autoresearch | — |
| Node classifier | Inference | custom | accuracy of evolution classification vs expert judgment | when users override <10% of classifications → reduce research depth | autoresearch | — |
| Strategic assessment | Interpretation | **genesis** | user acceptance rate (confirms without major changes), quality of challenge (did it surface something the user hadn't considered?) | when assessments for a product category follow stable patterns → template for that category | manual review | — |
| CLAUDE.md generator | Delivery | product | context fidelity score at review time (does CLAUDE.md match the actual product?) | when fidelity consistently >90% → reduce review frequency | manual review | review findings → node classifier (evolution drifted), user overrides of mapping → strategic assessment (challenge was wrong), stale CLAUDE.md detected → brief scanner (re-trigger strategy) |

### What Feeds reveals
The CLAUDE.md is not just output — it's the artifact that gets tested against reality at every `/playbook:review`. Review findings feed back to Inference (was the evolution classification right?), user overrides feed back to Interpretation (was the challenge useful?). **The plugin itself is a compound loop: strategy → build → review → strategy.** Each cycle improves the mapping.

### Feeds useful? **Yes — makes the strategy/build/review cycle explicit as a feedback loop, not just three separate commands.**

---

## Brief 3: FarmSense — crop disease early warning for smallholder farmers

**Client:** Smallholder farmers in sub-Saharan Africa (1-5 hectares).
**Value expected:** Know about crop disease before it's visible, through a channel that doesn't require a smartphone.

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Satellite imagery collector | Enrichment | commodity | coverage (% fields monitored), revisit frequency | if resolution insufficient → add drone imagery | N/A (buy) | — |
| Farmer reports (SMS/USSD) | Enrichment | custom | report rate (% farmers submitting weekly), data quality | when report quality stable → reduce prompt frequency | manual review | — |
| Disease predictor | Inference | **genesis** | prediction accuracy (days before visible symptoms), false alarm rate | when false alarm rate <5% for a disease → publish as regional alert rule | autoresearch | — |
| Action advisor | Interpretation | custom | advice clarity score (tested with sample farmers), applicability (% advice actionable with available resources) | when advice for a disease is stable → SMS template per disease | manual review | — |
| SMS/USSD alerts | Delivery | commodity | delivery rate, time from prediction to farmer receiving alert | if SMS fails >10% → add voice IVR | N/A | farmer replies → disease predictor (ground truth — "yes I see it" or "no my crop is fine"), no-reply patterns → SMS alerts (timing or language wrong), questions about diseases not in system → satellite collector (coverage gap) |

### What Feeds reveals
The SMS reply is the cheapest, most honest ground truth in the system. A farmer texting "no my crop is fine" after a disease alert is direct feedback to the genesis node (disease predictor). This is production autoresearch through the most basic interface possible — USSD. No smartphone, no app, no dashboard. The Delivery node is commodity infrastructure (SMS) but the signal it captures is irreplaceable.

### Feeds useful? **Yes — the insight that SMS replies are ground truth for the genesis node would be invisible without Feeds.**

---

## Brief 4: LegalDraft — contract clause risk assessment

**Client:** In-house legal teams at mid-market companies (50-500 employees).
**Value expected:** First draft of contract review in minutes, not days. Flag the clauses that actually matter.

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Contract parser | Enrichment | product | extraction accuracy (% clauses correctly identified) | when accuracy >98% → freeze parser, monitor only | autoresearch | — |
| Clause risk scorer | Inference | product | agreement with senior lawyers on risk ranking | when agreement >90% → reduce human review sample | autoresearch | — |
| Risk narrative | Interpretation | custom | lawyer edit rate (% of narrative changed before sending), time saved vs manual review | when edit rate <15% for a contract type → auto-generate for that type | manual review | — |
| Review dashboard + redline | Delivery | product | time from upload to first review, % contracts reviewed same day | when >90% same-day → shift focus to quality metrics | N/A | lawyer edits to narrative → risk narrative (what the system got wrong), clauses marked "not a risk" that were flagged → clause risk scorer (false positive), contract types uploaded that parser fails on → contract parser (coverage gap) |

### What Feeds reveals
No genesis node. This is an efficiency product — all nodes are product or custom. The value is in speed, not in novel intelligence. But Feeds still matters: lawyer edits to the risk narrative are the training signal for the Interpretation node. Without declaring Feeds, this looks like a one-shot tool. With Feeds, it's clear the system can improve with use.

### Feeds useful? **Yes, even without genesis. The lawyer-edit-as-training-signal pattern is the same as TalentVoice's recruiter edits.**

---

## Brief 5: NeighborLoop — hyperlocal community intelligence

**Client:** Residents of a neighborhood (5K-50K people).
**Value expected:** Know what's happening around you without checking 15 apps and 3 WhatsApp groups.

| Node | Layer | Evolution | Metric / Signal | Graduation | Loop | Feeds |
|------|-------|-----------|-----------------|------------|------|-------|
| Multi-source aggregator | Enrichment | custom | source coverage (% local channels ingested), dedup rate | when a new local source appears → add connector | manual review | — |
| Event/issue detector | Inference | custom | detection precision (% flagged items that residents confirm as real), recall | when a pattern recurs monthly → auto-categorize | autoresearch | — |
| Relevance personalizer | Interpretation | **genesis** | per-user engagement rate, % items marked "not relevant" | when irrelevance rate <5% for a user segment → freeze personalization for that segment | autoresearch (slow — weekly) | — |
| Conversational digest + chat | Delivery | **genesis** | questions answered, topics surfaced that weren't in any source, resident contributions triggered | when >60% of queries answerable from existing data → reduce human curation | manual review | "I didn't know about X" → multi-source aggregator (coverage gap), "this isn't happening anymore" → event detector (staleness), resident-contributed info → enrichment (the user IS the source), questions without answers → relevance personalizer (the model doesn't know what matters here) |

### What Feeds reveals
Two genesis nodes. The conversational interface is genesis because it's hyperlocal — what matters in Trastevere is different from what matters in Islington, and only the accumulated interactions reveal the difference. The richest Feed: **residents contributing information make them enrichment sources, not just consumers.** The Delivery node literally becomes an Enrichment node. The user is the sensor.

### Feeds useful? **Yes — reveals that in community products, the user IS enrichment. The Delivery→Enrichment loop is the product.**

---

## Summary: what the 5 briefs test

| Brief | Genesis layer | Delivery evolution | Feeds adds insight? | Pattern revealed |
|-------|-------------|-------------------|--------------------|----|
| CashGuard | Interp + Delivery | **genesis** (composable) | **Yes** | Interface is the moat (Block pattern) |
| Playbook | Interpretation | product | **Yes** | Strategy/build/review is a compound loop |
| FarmSense | Inference | commodity | **Yes** | SMS reply is cheapest ground truth |
| LegalDraft | none (efficiency) | product | **Yes** | Lawyer edits train the system even without genesis |
| NeighborLoop | Interp + Delivery | **genesis** (hyperlocal) | **Yes** | User IS enrichment in community products |

### Framework stress test result

Feeds adds insight in **5/5 cases**, including:
- A product with Delivery genesis (CashGuard, NeighborLoop) — captures the compound moat
- A product with no genesis (LegalDraft) — still shows how the system learns
- A product with commodity Delivery (FarmSense) — SMS replies as ground truth
- The plugin itself (Playbook) — the strategy/review cycle as a feedback loop
- Different domains: fintech, dev tools, agriculture, legal, community

The field never produced noise. In every case it surfaced a relationship that was invisible with 5 fields.

**One weakness found:** the template says "Every Delivery node should declare this" but in CashGuard and NeighborLoop, the Enrichment node "farmer reports" and "multi-source aggregator" could also have Feeds (farmer reports that mention a disease not in the system feed back to the satellite collector's coverage priorities). The current guidance says Feeds is primarily for Delivery. It should say: **every node that touches humans should declare Feeds.** The human interaction is the sensor, regardless of layer.
