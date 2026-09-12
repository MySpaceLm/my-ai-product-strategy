# Three-Axis Vulnerability Diagnostic

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Epic Safety & Denial Copilot — a bolt-on AI module for Epic (the hospital's existing EHR) that improves clinical note quality in real time and, from that same signal, (1) cross-checks the note against pharmacy, lab, and orders data for patient-safety gaps, and (2) flags documentation gaps before they become insurance denials.
**Your Role:** Product leader for AI features across Epic's clinical modules (ED, inpatient, OR, pharmacy, lab, radiology, registration) at a hospital already running Epic and 3M/Solventum 360 Encompass for coding.

---

## Scores

### Contextual Moat — 4/5
*Workflow depth × switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:** Unlike a single-purpose ambient scribe, this sits across multiple modules at once: it reconciles the note against pharmacy dispensing and lab results in real time (e.g., documented allergy vs. an active conflicting order), and it owns the physician-query workflow that CDI specialists and compliance route through daily. That's two sticky user groups (clinicians *and* CDI/compliance staff), not one, plus per-unit tuning (ED vs. ICU vs. OR order sets) that took real implementation effort. Displacing it means re-wiring three modules and retraining two departments, not just swapping a note-writing tool.

**Named attacker (from partner challenge):** Nuance DAX Copilot (Microsoft) — still the entrenched note-writing tool, but it does not do cross-module safety reconciliation or own the query workflow, so it isn't a full substitute even though it competes for the same moment of attention.

---

### Data Advantage — 4/5
*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:** The model is fed by two closed feedback loops nobody outside this hospital has: (1) actual denial outcomes — which claims were rejected, why, and the appeal result, tied back to the specific documentation gap that caused it, using this hospital's real payer mix; and (2) actual post-hoc safety incidents (falls, adverse drug events, missed diagnoses) tied back to the documentation gap that preceded them. Both compound with every case and are specific to this hospital's patients and payers — not available to a generic model trained on public data.

**Named attacker (from partner challenge):** Waystar / R1 RCM — large multi-hospital revenue-cycle vendors who could build an equivalent denial-feedback loop faster, at greater scale, across many hospitals at once, out-compounding a single-hospital effort over time.

---

### Platform Exposure — 3/5
*Encroachment risk × pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:** Better than a head-on note-writing tool, because this occupies a gap Nuance DAX explicitly doesn't fill (cross-module safety + denial prevention). But Epic already builds cross-module clinical models (e.g., the Epic Deterioration Index pulls vitals + labs across modules), so Epic's own Cognitive Computing platform is a plausible future entrant into this exact space if it decides to extend into denial-linked safety checks. Not immediate, but real and platform-owned.

**Named attacker (from partner challenge):** Epic Systems — via an extension of its existing Cognitive Computing / Deterioration Index framework into denial-linked documentation checks.

---

## Top Vulnerability
Platform Exposure: Epic owns the underlying data plumbing across every module this product touches, so even a well-differentiated wedge is one roadmap decision away from being absorbed natively.

## Confidence Level
Medium — the moat and data advantage are real once the two feedback loops (denial outcomes, safety incidents) are actually built and running, but that build is not yet done, and platform absorption risk is a long-term threat rather than an immediate one.
