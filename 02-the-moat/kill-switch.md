# Kill Switch Audit

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | 48-Hour Action |
|-----------|--------------|------------|---------------|
| **Provider** | Claude (Anthropic) is the core model in production; ChatGPT (OpenAI) is a proven technical alternative but has never actually run live | M | Run a real 48-hour drill — route a defined slice of production traffic to ChatGPT and confirm it holds, turning "technically could switch" into "have switched" |
| **Abstraction** | All model calls route through a single internal interface — no direct provider API calls scattered in product code | L | Maintain discipline: every new feature must call through the interface, never a provider SDK directly |
| **Routing** | Multi-model routing already live, routing tasks by cost, latency, and quality, with automatic failover to a fallback provider | L | None needed — keep the fallback provider's quota/capacity healthy so failover doesn't fail silently under real load |
| **Eval** | Automated eval harness (LangSmith) tests provider outputs against the quality bar before rollout | L | Confirm the golden-note test set explicitly covers safety-critical flags (e.g., allergy/order conflicts), not just general accuracy |

## Portability Score
Ready — three of four dimensions are structurally solid; the only real gap is that Provider portability has been designed and supported (abstraction + routing + eval all exist) but never proven under real conditions. The 48-hour drill above closes that gap.

## If Claude doubles pricing tomorrow:
Reweight the router's cost thresholds — a config change, not a rewrite — shifting routine/low-stakes flag checks (e.g., stylistic denial-risk language) to a cheaper model already in the routing pool, while keeping Claude reserved for the safety-critical cross-checks (pharmacy/lab/order conflicts) where the quality bar can't flex. Because routing and the abstraction layer already exist, this is hours of work, not weeks.

## If Claude (Anthropic) ships a competing product:
The provider relationship isn't the moat — the hospital-specific denial-outcome and safety-incident feedback loops, ownership of the physician-query workflow, signature-gated safety accountability, and the per-unit/per-clinician Preference tuning already built into Seha Hub are. A foundation-model vendor shipping a generic competing product would start with none of that data, none of that workflow lock-in, and none of the validated cross-hospital pattern library — the same structural argument that applies against Nuance DAX in the encroachment plan.

## Fire-Drill Timeline (Extra Practice Exercise 2)

*The concrete 48-hour drill referenced in the Provider row above, and in the Module 6 Horizon 1 roadmap. This is what "Ready" actually looks like exercised, not just designed.*

| Time | Action | Owner |
|---|---|---|
| T+0 | Drill declared (simulated trigger, not a real outage) — incident commander assigned | Engineering lead |
| T+0 to T+2h | Router config updated to send a defined slice of frontier-tier traffic to ChatGPT instead of Claude; abstraction layer means this is a config change, not a code change | On-call engineer |
| T+2h to T+8h | LangSmith eval harness scores live ChatGPT-routed traffic against the golden dataset in real time; safety-critical flag accuracy watched specifically, not just overall accuracy | ML engineer |
| T+8h to T+24h | If quality holds against the Reliability Contract's thresholds: expand the cutover to all tiers. If it doesn't: roll back to Claude immediately and log exactly why | Engineering lead + clinical safety lead |
| T+24h to T+48h | Extended monitoring under full alternate-provider load — confirm no drift, no latency regression, no rise in safety-flag false negatives over a sustained window, not just a snapshot | Clinical safety on-call |
| T+48h | Drill closeout: actual cutover time achieved (vs. the 48-hour target), any quality delta observed, Portability Score updated from "Ready (untested)" to "Ready (proven)" if the drill succeeded | Clinical safety lead, signs off |

**What this closes:** today the Provider row says "technically could switch, never has." After this drill runs once, it says "has switched, under monitored conditions, with a measured cutover time" — the exact gap named in the Kill Switch assessment above and in the Module 6 board simulation's first objection.
