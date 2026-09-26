# Three-Horizon Roadmap & Board Pitch

*Pitch audience: CTO + Head of Product. This audience cares most about reliability and cost — the pitch below leads with the Reliability Contract, the Kill Switch, and margin architecture, not board-level narrative alone.*

## AI Evaluation

*Run against the assembled repo, using the six lenses from the AI Bet Evaluator.*

**Bet Validation — evidence-backed or just conviction?** Evidence-backed. The M1 diagnostic named specific attackers (Oracle Health, Nuance DAX, Waystar/R1 RCM) rather than "Big Tech," and the bet was sharpened twice (Epic → Seha Hub rename, feature scope narrowed from generic coding automation to a cross-module safety+denial wedge) based on real competitive analysis, not just conviction.

**Capability Gaps — what needs to be built? Realistic?** Two concrete, scoped gaps: the Correction loop (signed dismissals captured but not reused — a pipeline problem, not a research problem) and Cross-Domain Transfer (designed with the right governance model, not yet built). Both are realistic H1/H2 engineering work, not open research questions.

**Defensibility — platform-proof or copyable?** Partially platform-proof. The Preference and Network Intelligence loops (5/5 each) are genuinely hard to copy quickly — they depend on accumulated per-unit and cross-hospital validated pattern data. Platform Exposure remains the named top vulnerability: Seha Hub itself could absorb this natively, which is a real, not hypothetical, risk.

**Pricing Alignment — economics hold under stress?** Not under the original flat-seat model — that model failed its own stress test (37.6% margin in the combined worst case, below the 40% floor). The redesigned hybrid model ($19 base + $15/confirmed catch) both fixes the stress-test failure and raises baseline margin, which is a stronger result than simply protecting the original number.

**Trust & Reliability — contract explicit and measurable?** Yes. Every metric in the Reliability Contract has a numeric target, a measurement method, and an alert threshold with a named consequence (page, rollback, audit, human queue) — not vague statements like "high accuracy."

**Impact & Scale — what impact? What breaks at 10x?** At 10x scale (multi-hospital, high adoption), the two things that break first are exactly the two open gaps: Cross-Domain Transfer (knowledge stays siloed per unit even as hospitals multiply) and the unresolved SaMD/FDA regulatory question (more autonomy, more scale, more regulatory exposure). Both are already on the roadmap below, not surprises.

## Roadmap

### Horizon 1 — Now (0-3 months)
*Quick wins. Ship with existing capabilities.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Run the Kill Switch 48-hour live cutover drill (Claude to ChatGPT) | Successful cutover, zero flag-quality degradation | H |
| Ship semantic caching on re-analysis triggers | Blended cost/request drops toward closing the gap to the 40% margin floor | H |
| Build the Correction-loop retraining pipeline (signed dismissals + HITL corrections to weekly threshold tuning) | Correction loop score rises from 2/5 toward 4-5/5 at next flywheel review | H |
| Ship the confirmed-catch reporting dashboard | Live before the first billing cycle under the new hybrid pricing model | H |
| Expand golden dataset with rare-drug-interaction adversarial cases | +10 rows toward the ~150-row v1 target | H |

### Horizon 2 — Next (3-9 months)
*Bets. Requires new capabilities or integrations.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Build Cross-Domain Transfer pipeline (policy-gated pattern application across units) | 1+ validated pattern successfully transferred and measured. Kill criteria: no measurable improvement in target unit within 60 days | M |
| Ship native incident-reporting system integration (e.g. RL Datix/Midas) | First hospital partner live. Kill criteria: no adoption within 90 days of launch | M |
| Ship physician-query drafting (agentic, pre-approved: draft-only, human sends) | Reviewer override rate <30%. Kill criteria: override rate exceeds 30%, pause and retrain | M |
| Resolve SaMD/FDA regulatory determination | Formal legal/regulatory opinion obtained | M |

### Horizon 3 — Bet (9-18 months)
*Moonshots. High uncertainty, high potential.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Multi-facility Network Intelligence rollout beyond the pilot hospital (requires H2's Cross-Domain Transfer + regulatory resolution first) | Live at 3+ facilities in one network | L |
| Nuance/Microsoft DAX partnership on phrasing-pattern alignment (requires partner conversation initiated) | Signed partnership or integration agreement | L |
| Vision: Chartguard as the reference safety+denial layer across the entire Seha Hub network | Adopted as a default module in new Seha Hub deployments | L |

## Board Pitch

**Thesis (1 sentence):** Chartguard is already differentiated on reliability infrastructure, not just model quality — the fastest way to protect margin as usage scales is closing two already-scoped engineering gaps and shipping the pricing fix already designed.

**The case:**
1. **Why now:** Denial costs and safety liability are present, measurable costs today — not hypothetical. The infrastructure to address them (eval harness, Kill Switch, cascading architecture, signature-gated safety flags) is already built and running, not speculative.
2. **What's defensible:** Not the model — the compliance-grade accountability (signature-gated dismissals, physician-query ownership) and the validated cross-hospital Network Intelligence loop, both things Nuance DAX and CodaMetrix don't have today. Reinforced by a Ready-rated Kill Switch (abstraction + routing + eval already live; only the live cutover drill is pending).
3. **The economics:** Baseline flat-seat margin looks strong (86.1%) but fails its own stress test — 37.6% in the combined worst case, below the 40% floor. The fix is already designed, not theoretical: semantic caching plus a hybrid confirmed-catch pricing model that raises margin to 89.4% and adds $7,500/mo gross at 500 seats — this isn't just risk mitigation, it's a net improvement.

**The risks:**
1. **Trust / failure modes:** The Correction loop is broken (signal captured, never reused) and Cross-Domain Transfer doesn't exist yet. Both are scoped, sequenced (H1 and H2 respectively), and have defined success metrics — known gaps with a plan, not open unknowns.
2. **Scale / governance:** No live agentic extension today, but the governance topology (draft-not-send, whitelisted tool calls, TTL'd memory, named-owner chain handoffs) is pre-built, so any future agentic feature ships inside existing guardrails instead of triggering a new policy debate later. The SaMD/FDA determination is still open — flagged honestly, not glossed over.
3. **Competitive:** Microsoft/Nuance DAX and CodaMetrix are the named attackers; Platform Exposure (Seha Hub itself absorbing this natively) remains the top vulnerability from Module 1. Mitigated by owning workflow depth and validated pattern data that's genuinely hard to replicate quickly — not by assuming the incumbents won't try.

**The ask:** 2 engineers + 1 ML engineer for 3 months to execute the full Horizon 1 list — Kill Switch drill, caching, the Correction-loop pipeline, the confirmed-catch dashboard, and golden dataset expansion. Estimated cost ~$180K (3 FTEs x 3 months, blended loaded rate). Return: closes the exact gap that drops margin below the 40% floor under stress, and unblocks billing under the new pricing model before it needs to go live.

## Board Q&A / Anticipated Objections

*From the Module 6 board simulation, audience: CTO + Head of Product. Every answer ties back to something already built or already sequenced above — not a promise.*

**"You say the Kill Switch is 'Ready' — but you've never actually cut over in production. Why should I believe that, and what if it fails?"**
That's precisely why the 48-hour live cutover drill is the first item in Horizon 1, not the last. Today it's Ready on paper — abstraction, routing, and eval all exist and run daily. What's never been proven is a live cutover under real traffic, and that's exactly the gap this ask closes in week one, not month three.

**"Your current pricing model fails its own stress test. Why wasn't this caught before launch, and how do I know the next gap isn't already sitting somewhere unfound?"**
The pricing model wasn't broken at launch — it was reasonable for a single-hospital pilot at moderate usage. The stress test is the discipline that caught this before a customer complained or usage actually tripled — we found it ourselves, in a strategy exercise, not in a support ticket. That's the actual point of the Reliability Contract and Eval Dashboard: continuous weekly testing and drift alerts exist to surface the next gap early too. There are no guaranteed-zero unknown risks left, but there is a system built specifically to find them before they're a crisis, and this is proof it works.

**"The Correction loop has been broken since Module 2. What's actually different this time?"**
It's been correctly identified since Module 2, not neglected. The moat and margin fundamentals were fixed first deliberately — building a retraining pipeline on top of an unstable pricing model or an unproven Kill Switch would have been wasted engineering. What's different now: the design already exists. This is wiring two systems that already work independently (the signed-dismissal log and the existing LangSmith eval harness) on a weekly cadence — integration work, not research. The commitment is explicit: Correction loop score rises from 2/5 to 4-5/5, checked at the next flywheel review.

**"Why 3 FTEs and not 1?"**
The five Horizon 1 items split cleanly by skill: the Kill Switch drill and the confirmed-catch dashboard are core engineering; the Correction-loop pipeline and golden-dataset expansion need ML/eval expertise; semantic caching is systems work that runs in parallel with both. Three people working in parallel compresses five initiatives into 3 months instead of 6-9 months serial with one person — the headcount is sized to what's actually parallelizable, not padded.

**"Platform Exposure is still your named top vulnerability from Module 1. None of Horizon 1 touches it. Why isn't it there?"**
Because it's a multi-quarter risk, not a 3-month one — Seha Hub building this natively takes real engineering time even starting today, which is exactly why the M1 diagnostic scored it Medium confidence, not Critical. The real mitigation isn't a single engineering task — it's the moat compounding across Horizon 1 and 2: the Preference and Network Intelligence loops are already 5/5 and get harder to replicate the longer they run. An explicit H2 item to monitor Seha Hub's own roadmap as a tripwire is a reasonable addition — but the honest answer is that this defense is compounding, not a box to check this quarter.

## M1 Baseline vs. Now
*Your 3-sentence AI strategy from Module 1 vs. what you'd say now:*

**M1 baseline:** "AI is clearly transforming healthcare, and there's real opportunity for us to use it to improve patient outcomes and reduce administrative burden. I don't have a specific plan yet, but I want us to explore where AI capabilities can genuinely help our products and our market. The first step is figuring out which of AI's strengths actually apply to what we do."

**Now (drafted from the repo — adjust to your own voice before using):** We're not betting on a smarter model, we're betting on infrastructure competitors don't have yet: compliance-grade accountability, a validated cross-hospital learning loop, and a reliability contract with real teeth. The economics work, and we already found and fixed the one pricing flaw that would have broken them under real growth. The two things that could still hurt us — a stalled feedback loop and a knowledge silo between hospital units — are scoped, sequenced, and already on the roadmap, not surprises waiting to happen.
