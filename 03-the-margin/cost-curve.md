# Cost Curve & Pricing Strategy

## Packaging Decision

*Leader / Filler / Killer — the 70% rule: if Killer usage is under 70%, it's probably an add-on.*

**Leader:** Safety flag engine — cross-module conflict detection (documented allergy vs. active order, hypoxia vs. missing O2 order, etc.). This is why a hospital buys Chartguard at all: compliance-mandated, signature-gated dismissal, liability-driven, non-negotiable.

**Filler:** Denial-risk / CDI language flags (vague diagnostic language, specificity queries). Lighter-weight NLP checks, real value (denial reduction), bumps ARPU — the upsell reason to expand usage, not the reason the deal gets signed.

**Killer:** Cross-hospital Network Intelligence engine — the automated pattern-detection-and-propagation loop (Module 2's Network loop, 5/5). Continuous pattern-mining and validation across every connected hospital, not a per-note check. Value scales specifically with how many hospitals you're part of — giving it away free to every customer regardless of size would break unit economics, since a single-site customer barely benefits from it but the inference cost runs regardless.

**Killer usage %:** ~25-30% — realistically only multi-facility hospital systems (not standalone hospitals) get meaningful value from cross-hospital pattern propagation. Well under the 70% threshold.

**Bundle or add-on:** Add-on. Leader (safety) + Filler (denial-risk) ship bundled in the base plan for everyone. Network Intelligence (Killer) sells as a premium tier for hospital systems with 2+ facilities.

## Cost Model

*Features -> Tiers -> Blended COGS (refines the initial 2-tier estimate into 3 tiers, one per feature):*

| Feature | Complexity | Model Tier | Cost/Req | Volume % | Weighted |
|---|---|---|---|---|---|
| Denial-risk / CDI language flags | Simple | Small | $0.0010 | 60% | $0.0006 |
| Preference/suppression check (per-clinician, per-unit pattern match) | Medium | Mid | $0.0040 | 25% | $0.0010 |
| Cross-module safety reconciliation (allergy/order/lab/pharmacy) | Complex | Frontier | $0.0120 | 15% | $0.0018 |
| **Blended** | | | | **100%** | **$0.0034** |

*Margin Calculator baseline (per clinician seat, avg 2,000 requests/month) — updated to the refined $0.0034 blended rate:*

| Input | Your Number | Stress (3x usage) |
|---|---|---|
| Avg requests/user/month | 2,000 | 6,000 |
| Cost per request (blended) | $0.0034 | $0.0034 (unchanged — see Stress Tests) |
| Monthly COGS per user | $6.80 | $20.40 |
| Revenue per user/month | $49 | $49 (unchanged — flat seat price) |
| Gross margin per user | $42.20 | $28.60 |
| Gross margin % | 86.1% | 58.4% |

| Cost Category | Per-User/Month | Notes |
|--------------|----------------|-------|
| Inference (frontier tier) | $1.80 | 15% of requests (300/mo) x ~$0.012/request — safety-critical cross-module reconciliation |
| Inference (mid tier) | $2.00 | 25% of requests (500/mo) x ~$0.004/request — preference/suppression pattern checks |
| Inference (triage/small tier) | $1.20 | 60% of requests (1,200/mo) x ~$0.001/request — routine denial-risk/stylistic checks |
| Infrastructure | TBD | Not modeled in this exercise — calculator scoped to AI inference COGS only |
| Data/storage | TBD | Not modeled in this exercise |
| Human-in-the-loop | TBD | Not modeled in this exercise (signature/compliance logging is automated, not a per-request human cost) |
| **Total AI COGS** | **$6.80** | (Note: the earlier $6.40 two-tier estimate is superseded by this 3-tier breakdown) |

## Cascading Strategy
<!-- Cheap model → frontier model routing logic -->

**Triage model:** Small/cheap tier (Claude Haiku-class) — handles routine denial-risk and stylistic-language checks
**Frontier model:** Claude Sonnet-class — reserved for safety-critical cross-module reconciliation (pharmacy/lab/order conflicts)
**Routing rule:** Route by cost/latency/quality (per Module 2's live multi-model routing) — low-stakes checks default to triage tier; preference/suppression pattern checks route to a mid tier; anything touching a safety-critical flag routes to frontier
**Expected cascade ratio:** 60% triage (small) / 25% mid / 15% frontier

## Pricing Model

**Pricing strategy:** Maximize (Microsoft-style) — not Skim (too narrow for a hospital-wide rollout) or Penetrate (gives away margin on a genuinely differentiated, liability-reducing product). Chartguard needs broad adoption across all clinical seats, then monetize depth via the Network Intelligence add-on.

**Unit of work metered:** A "confirmed catch" — a safety flag formally acknowledged + signed as real, or a denial-risk flag later confirmed (via the denial-outcome feedback loop) to have prevented an actual claim rejection. Chosen specifically to fix the margin risk below: billing on confirmed catches means revenue grows exactly when the product is proving its value, instead of staying flat while usage (and COGS) grow.

**Current pricing:** N/A — pre-launch, no prior pricing to reference
**Proposed AI pricing (Before, for comparison):** $49/user/month flat seat price (bundles Leader + Filler)
**Proposed AI pricing (After, redesigned):** $19/seat/month base + $15/confirmed catch — illustrative, needs pilot validation on actual catch volume. Network Intelligence (Killer) sold as a separate premium add-on tier for multi-facility hospital systems.
**Model:** Hybrid — base fee + usage-based component tied to a real outcome, not raw request volume.

## Stress Tests

*Updated to the refined 3-tier blended cost ($0.0034/request):*

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Inference costs 3x (usage grows 3x, price flat) | Gross margin/user drops from $42.20 (86.1%) to $28.60 (58.4%) — healthy, but revenue doesn't grow with usage | Deploy semantic caching on re-analysis triggers — skip re-analysis when a note hasn't materially changed since the last save, blunting volume growth before it hits COGS |
| Heaviest segment doubles (frontier-tier share grows 15% -> 30% of requests, redistributed from small/mid) | Blended cost/req rises to ~$0.0049; COGS/user rises to $9.80/mo; margin drops to $39.20 (80.0%) | Tighten cascade routing thresholds and re-validate via the LangSmith eval harness so more cases can confidently stay on the triage/mid tiers |
| Model provider raises prices 50% | Blended cost/req rises to $0.0051; COGS/user rises to $10.20/mo; margin drops to $38.80 (79.2%) | Exercise the Kill Switch (Module 2) — abstraction layer + eval harness are already built; run the 48-hour cutover drill to shift to an alternate provider rather than absorb the increase |
| **Combined worst case: 3x usage + 50% cost/request** | **COGS/user rises to $30.60/mo; margin drops to $18.40/user (37.6%) — below the 40% floor** | Same two levers (caching + Kill Switch) apply together, plus the pricing redesign below — this is the scenario that actually threatens the architecture, not either factor alone |

**Biggest margin risk:** Revenue is flat per seat while COGS scales directly with usage — there's no pricing mechanism that captures more value as a clinician relies on Chartguard more. Product success (higher usage, more trust) erodes margin instead of growing revenue, because the base plan is seat-based, not usage- or outcome-based. The combined worst-case stress crossing below 40% is the concrete proof of this risk.

**First lever to pull:** Semantic caching on re-analysis triggers. Since most of the 2,000 requests/month come from re-analyzing on every save (including minor edits), caching unchanged-content re-analyses directly attacks the volume side of the equation before touching pricing or model tier at all. The pricing redesign (below) is the second, complementary lever.

## Board One-Pager
<!-- Before/After: Old SaaS revenue vs. AI usage revenue for your product -->

*Assumptions flagged as illustrative, not measured data: 500 clinician seats (representative mid-size hospital), ~3 confirmed catches/seat/month.*

**Before (traditional SaaS — flat seat pricing):**
- Revenue: $49/seat x 500 seats = $24,500/mo
- COGS: $6.80/seat x 500 = $3,400/mo (fixed, at baseline usage)
- Gross margin: $21,100/mo (86.1%)

**After (AI-enabled — hybrid base + confirmed-catch usage):**
- Revenue: ($19 base + 3 catches x $15) x 500 seats = $64/seat x 500 = $32,000/mo
- COGS: $3,400/mo (variable, same usage volume as Before — pricing model change doesn't change underlying cost structure)
- Gross margin: $28,600/mo (89.4%)

**Net margin shift:**
- Δ margin %: +3.3pp (86.1% -> 89.4%)
- Δ gross $: +$7,500/mo (+35.5%)

**Narrative:** Margin % moves because usage-based pricing on confirmed catches captures value proportional to reliance, not a flat fee that leaves money on the table as adoption deepens. Gross $ and NRR both win — expansion revenue grows automatically as clinicians trust the product more, with no renegotiation needed. Hedge: this requires reliable "confirmed catch" telemetry and hospital comfort with a variable bill — worth a capped-usage pilot period to ease the transition.
