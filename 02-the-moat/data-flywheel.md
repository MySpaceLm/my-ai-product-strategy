# Data Flywheel Map

> Score each loop 1-5. Your weakest loop is where competitors attack first.
> The four loops below are the M2 starting point - adapt if your product has 2 or 6 loops instead of 4.

## Flywheel Loops

| Loop | What It Measures | Score 1 | Score 5 | Score |
|------|------------------|---------|---------|-------|
| **Correction** | Do users fix AI outputs? Is that signal captured and reused? | No capture | Automated retraining | 2/5 |
| **Preference** | Does the product learn individual / team preferences over time? | Stateless | Deep personalization | 5/5 |
| **Domain Context** | Does usage in one area improve quality in adjacent areas? | Siloed | Cross-domain transfer | 1/5 |
| **Network** | Does each new user / team make the product better for everyone? | Isolated | Strong network effects | 5/5 |

### Correction Loop - 2/5
**What you capture today:** When a clinician dismisses a safety flag, an acknowledgment + signature is required and stored — but only as a compliance/audit record.
**How it compounds:** It currently doesn't. No pipeline mines the signed-dismissal log for patterns (e.g., which flags get overridden most, cross-referenced with actual outcomes) or feeds it back into flag tuning or retraining. The raw material for this loop already exists; the reuse mechanism doesn't.

### Preference Loop - 5/5
**What you capture today:** Per-clinician and per-unit override/dismissal patterns (e.g., a unit's typical documentation style, a clinician's recurring false-positive dismissals).
**How it compounds:** Live today — the system actively suppresses recurring non-critical (denial-risk/stylistic) flags based on learned per-unit and per-clinician patterns. Safety-critical flags are deliberately exempt from suppression: they always display, and dismissing one requires an acknowledgment + signature, so alert fatigue can never silently erode a safety catch.

### Domain Context Loop - 1/5
**What you capture today:** Nothing — each unit (ED, ICU, OR, etc.) currently runs on siloed logic with no transfer between them.
**How it compounds:** It doesn't yet. The intended design is policy-gated cross-domain transfer — a pattern validated in one unit could be evaluated against another unit's own policy before being applied there, rather than copied blindly. That governance model is the right instinct, but it hasn't been built.

### Network Loop - 5/5
**What you capture today:** Each hospital's raw patient/note data stays fully walled off from every other hospital — no cross-hospital data pooling, by design (privacy + competitive separation).
**How it compounds:** Live today, and it's the strongest loop. The system automatically detects a validated pattern from real-world results at one hospital and proposes it for rollout to other hospitals — sharing the *pattern*, never the underlying data. Because detection and proposal are automated rather than manually curated, this loop scales as fast as hospitals adopt the product, not as fast as a review team can work.

**Total Flywheel Score: 13/20**
**Weakest Loop:** Domain Context (1/5)
**Fix for weakest loop:** Build the cross-unit transfer pipeline the product already has the right governance model for — policy-gated pattern application from one unit's validated learnings into another unit, instead of leaving every unit fully siloed.

---

## Encroachment Threat Assessment

### 1. Platform Encroachment
**Attacker:** Microsoft (Nuance DAX Copilot)
**Vector:** DAX is already the dominant ambient-documentation layer across many EHRs, not just Epic — Microsoft could extend it with cross-module safety/denial checks and plug it into Seha Hub the same way it plugs into others.
**Time-to-threat:** Fast — infrastructure and hospital relationships already exist; this is a feature extension, not a new product.
**% of value at risk:** High — competes directly for the core AI moment (the flag itself), even without controlling Seha Hub as a platform.

### 2. Vertical Competitor
**Attacker:** CodaMetrix (Iodine Software)
**Vector:** An AI-native company whose entire product is "read the note, catch documentation gaps that cause denials" — deeper in this one niche than a hospital-specific bolt-on, with more cross-hospital training data.
**Time-to-threat:** Already selling to hospitals today — not hypothetical.
**% of value at risk:** Moderate-high, concentrated on the denial-risk half of Chartguard specifically; less overlap on the patient-safety half.

### 3. Adjacent Expansion
**Attacker:** 3M/Solventum 360 Encompass
**Vector:** Already embedded in this hospital's coding/billing workflow today — could add "safety flag" as one more module bolted onto a tool coders already open daily, leveraging distribution they already have rather than building from scratch.
**Time-to-threat:** Moderate — requires 3M to decide to expand scope, but they already have the workflow foothold.
**% of value at risk:** Moderate — mainly threatens the coding/billing side rather than the clinical-safety side.

---

## 90-Day Encroachment Plan

*Your partner played the Big Tech attacker. What was their plan to kill you?*

**Attacker:** Microsoft (Nuance DAX Copilot) — playing Head of Product

**Attack vector (target the weakest loop):** Domain Context (1/5) — Chartguard's per-unit siloing, vs. DAX's cross-hospital, cross-unit pattern library that never had that silo to begin with.

**Weeks 1-4 - what they ship:** A "Cross-Unit Insight" feature: DAX pools its own de-identified pattern library across every hospital and unit it's already deployed in — including specialties Chartguard hasn't piloted yet — and starts surfacing denial-risk and safety-adjacent flags that transfer instantly across departments. Marketed directly at the weak point: "Chartguard learns in silos. DAX learns everywhere, from day one."

**Weeks 5-8 - how they poach users:** Approach the CDI and compliance teams — the second sticky user group Chartguard worked to own — with a free 30-day pilot running DAX's cross-unit flags alongside Chartguard. Zero switching cost, since DAX already handles their note-writing. Target OR and radiology specifically, where Chartguard's single-unit tuning is thinnest.

**Weeks 9-12 - why users don't come back:** CDI staff get used to DAX surfacing insights learned from other hospitals entirely — breadth a brand-new, hospital-walled Network loop can't match yet. Chartguard's Domain Context gap becomes visible daily: it's still not catching the OR-specific pattern ED already learned weeks ago. DAX doesn't need to win on flag quality — it just needs "already integrated, already learning across everything" to feel like less friction than "best of breed, but narrow."

**Your defense:**

*Why their plan is weaker than it looks:*

1. **DAX is bolting cross-unit insight onto a note-writing tool — Chartguard is bolting it onto a system that already does compliance-grade accountability.** DAX has never owned the physician-query workflow, never done real-time pharmacy/lab/order reconciliation, and has no signature-gated dismissal architecture. Adding "Cross-Unit Insight" to DAX is a bolt-on feature; adding cross-unit transfer to Chartguard is completing a system it already has the governance model for. Chartguard's engineering lift to close Domain Context is smaller than DAX's lift to build what Chartguard already has.
2. **Breadth without validation is a liability, not an advantage — and that's the pitch to compliance.** DAX's cross-hospital pattern library surfaces everything the model correlates across a huge undifferentiated corpus. Chartguard's Network loop only ships a pattern once it's validated against real practice and real outcomes at a live hospital. To a CDI/compliance team, that's the difference between a tool they can defend in an audit and one they can't — and alert-fatigue-from-an-unvalidated-firehose is exactly the failure mode Chartguard's signature-gated safety design was built to prevent.
3. **Chartguard already owns the switching cost DAX is trying to buy.** DAX's free 30-day pilot only works if CDI/compliance have nothing to lose by trying it — but Chartguard already routes their physician-query workflow and audit trail (Contextual Moat, 4/5). Ripping that out mid-workflow to test a scribe add-on is a bigger ask than DAX assumed.

*The 90-day counter-plan:*

- **Weeks 1-4:** Ship the cross-unit pipeline fast by reusing what already exists — point the exact detection-and-proposal mechanism powering the Network loop (5/5) inward, at units within one hospital, instead of only across hospitals. Not a new capability, an existing one repointed.
- **Weeks 5-8:** Go to compliance with the validation story, not just the feature list. While DAX pitches "learns everywhere," Chartguard pitches "every pattern you see was proven safe and effective before it reached you — here's the audit trail." Get one documented case where a suppressed-false-positive or a caught safety conflict shows up favorably in an actual compliance review, and use it as reference.
- **Weeks 9-12:** Close the OR/radiology gap DAX specifically targeted — prioritize per-unit tuning for exactly the units DAX picked as weakest. Killing their stated wedge directly is more convincing to users than a generic roadmap update.

---

## Extra Practice — Competitor Flywheel Comparison

*Module 2 Extra Practice Exercise 1. Scores are informed estimates from public information about Nuance DAX Copilot (Microsoft) — not internal DAX data, which isn't public. Flagged as illustrative, same discipline as every other estimate in this repo.*

| Loop | Chartguard | Nuance DAX (estimated) | Notes |
|---|---|---|---|
| Correction | 2/5 | 4/5 | DAX generates full note drafts, so every clinician edit is a high-volume, natural correction signal — and Microsoft's enterprise scale makes reuse for model improvement plausible, even though the exact mechanism isn't public. Chartguard has the same raw material (signed dismissals) but no reuse pipeline yet — this is the gap Horizon 1 closes. |
| Preference | 5/5 | 4/5 | DAX adapts to a clinician's speech patterns and phrasing over time — real personalization. But Chartguard's mechanism is arguably more rigorous: a hard, non-negotiable exemption for safety flags from any suppression, which DAX's architecture (a scribe, not a safety-flag engine) doesn't need to solve at all. |
| Domain Context | 1/5 | 3/5 | DAX's broad specialty coverage comes from model scale and general training, not a specific validated cross-unit transfer mechanism — so it's a different kind of advantage (breadth via scale) rather than a direct win on this exact loop. Still ahead of Chartguard's missing pipeline. |
| Network | 5/5 | 5/5 | DAX likely wins on raw scale — it's deployed across a much larger hospital base than Chartguard's pilot. But Chartguard's mechanism (validated-pattern-only propagation, raw data never shared) is arguably more defensible to a compliance team than "the model learned from everywhere," which is exactly the counter-argument built into the 90-Day defense above. |
| **Total** | **13/20** | **~16/20** | |

**Honest read:** DAX likely outscores Chartguard on this scorecard today, mostly on scale and maturity, not because Chartguard's design choices are wrong. The two loops where Chartguard's *mechanism* is arguably stronger (Preference's safety exemption, Network's validation-before-sharing) are also the two loops the 90-Day defense above already leans on — this comparison confirms the defense is aimed at the right place, not a new finding that changes it.
