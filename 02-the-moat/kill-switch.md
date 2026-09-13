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
