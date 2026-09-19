# My AI Product Strategy

> A living strategy built across 6 sessions. Each module adds one component. By Module 6, this repo IS your strategy — version-controlled, board-ready, portable.

---

## Strategy at a Glance

| Component | Module | Status | Key Artifact |
|-----------|--------|--------|-------------|
| **The Bet** | M1 | [x] complete | `01-the-bet/` |
| **The Moat** | M2 | [x] complete | `02-the-moat/` |
| **The Margin** | M3 | [x] complete | `03-the-margin/` |
| **The Contract** | M4 | [x] complete | `04-the-contract/` |
| **The Guardrails** | M5 | [ ] | `05-the-guardrails/` |
| **The Pitch** | M6 | [ ] | `06-the-pitch/` |

---

## The Bet (M1)

**What we're building, for whom, why now.**

- **Product:** Seha Hub Safety & Denial Copilot — cross-module AI bolt-on for Seha Hub that improves note quality, catches patient-safety gaps, and prevents insurance-denial-causing documentation errors
- **AI Value Archetype:** Copilot + Oracle
- **Vulnerability Scores:** Moat 4/5 · Data 4/5 · Platform 3/5
- **Top Risk:** Platform Exposure — Seha Hub owns the cross-module data plumbing and could build this natively, and Oracle Health (Cerner) already ships a comparable capability on a rival EHR
- **Confidence:** M
- **Prototype:** [Chartguard](https://claude.ai/code/artifact/98b72420-d7fa-4d75-99a6-66f7ea532b47) — clinical note copilot flagging safety conflicts + denial-risk gaps
- **Kill Criteria:** No significant denial/safety-incident reduction after a 60–90 day pilot, Seha Hub ships a native equivalent within the pilot window, or the hospital's data volume can't support a meaningful signal within two quarters

→ Details: [`01-the-bet/`](01-the-bet/)

---

## The Moat (M2)

**Why this won't get copied in 6 months.**

- **Data Flywheel Score:** 13/20 (Correction 2 · Preference 5 · Domain Context 1 · Network 5)
- **Weakest Loop:** Domain Context — cross-unit transfer designed (policy-gated) but not yet built
- **Competitive Position:** Strong Preference + Network loops (per-unit/clinician tuning, validated cross-hospital pattern promotion); weak on cross-unit transfer within a hospital
- **Encroachment Defense:** Vs. Microsoft/Nuance DAX — Chartguard already owns compliance-grade accountability (signature-gated safety, physician-query workflow) that DAX would have to build from scratch; validated-pattern network effect beats DAX's unvalidated breadth
- **Vendor Portability:** Ready — Claude (Anthropic) is the core model behind a single abstraction interface, with live multi-model routing + failover and an automated eval harness (LangSmith); only gap is a live cutover drill, not yet run

→ Details: [`02-the-moat/`](02-the-moat/)

---

## The Margin (M3)

**Will this make money or bleed it?**

- **Gross Margin (flat-seat baseline):** 86.1% ($42.20/seat), drops to 58.4% at 3x usage and 37.6% in the combined worst case (3x usage + 50% cost/request) — below the 40% floor
- **Gross Margin (AI-adjusted, hybrid pricing):** 89.4% ($28,600/mo gross at 500 seats) — usage-based pricing raises margin % and gross $ simultaneously
- **Pricing Model:** Hybrid — $19/seat/mo base + $15/confirmed catch (Maximize strategy); replaces a flat $49/seat baseline that left value on the table as usage grew
- **Cascading Strategy:** 60% small / 25% mid / 15% frontier, routed by cost/latency/quality; blended cost $0.0034/request
- **Break-even at:** Biggest margin risk is flat seat pricing against usage-scaling COGS — first lever is semantic caching on re-analysis triggers, second is the confirmed-catch pricing redesign

→ Details: [`03-the-margin/`](03-the-margin/)

---

## The Contract (M4)

**Why users will trust a probabilistic system.**

- **Reliability Target:** 95% overall accuracy · safety-flag false-negative rate <0.1% (dedicated metric, tighter than generic accuracy) · hallucination <0.5% · latency p95 <1000ms · drift <0.5%/wk
- **Golden Dataset:** 10 rows, 3 adversarial (v1 ship target ~150) — spans both flag types, a clean-note negative case, and cases testing the known M2 gaps (Preference, Domain Context, Network loops)
- **Confidence UX:** Tiered confidence + human-in-loop trigger, combined; safety-flag thresholds locked (non-adjustable by users), denial-risk thresholds user-adjustable; reasoning/evidence always shown
- **HITL Architecture:** Confidence <60% OR safety-flag uncertainty OR suspected false negative → rotating clinical safety/CDI on-call; corrections feed the weekly gold-set audit, closing the M2 Correction-loop gap
- **Failure Mode Coverage:** Prompt injection via note text, contradictory allergy documentation, buried conflict in a long/complex note — real partner red-team pass still an open item

→ Details: [`04-the-contract/`](04-the-contract/)

---

## The Guardrails (M5)

**What breaks when this scales — and what compounds.**

- **Compounding System:** [describe feedback loops]
- **Governance Posture:** [approach]
- **Shadow AI Status:** __ tools found, __ triaged
- **Agent Boundaries:**
- **Regulatory Exposure:**

→ Details: [`05-the-guardrails/`](05-the-guardrails/)

---

## The Pitch (M6)

**How you get this funded, shipped, and adopted.**

- **Horizon 1 (Now):**
- **Horizon 2 (Next):**
- **Horizon 3 (Bet):**
- **Board Narrative:** [1-sentence thesis]
- **Key Metric:**

→ Details: [`06-the-pitch/`](06-the-pitch/)
