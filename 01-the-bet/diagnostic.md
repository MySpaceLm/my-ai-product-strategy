# Three-Axis Vulnerability Diagnostic

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Seha Hub Safety & Denial Copilot — a bolt-on AI module for Seha Hub (the hospital's existing EHR) that improves clinical note quality in real time and, from that same signal, (1) cross-checks the note against pharmacy, lab, and orders data for patient-safety gaps, and (2) flags documentation gaps before they become insurance denials.
**Your Role:** Product leader for AI features across Seha Hub's clinical modules (ED, inpatient, OR, pharmacy, lab, radiology, registration) at a hospital already running Seha Hub and 3M/Solventum 360 Encompass for coding.

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

**Score rationale:** Better than a head-on note-writing tool, because this occupies a gap the leading ambient-scribe vendors (e.g., Nuance DAX) explicitly don't fill (cross-module safety + denial prevention). But two encroachment paths are real: Seha Hub's own product team could extend its native clinical models into this exact space since it already owns the cross-module data; and Oracle Health (Cerner) — a rival incumbent EHR — already ships comparable cross-module clinical AI, setting a market bar Seha Hub has to clear or risk hospitals favoring the rival platform instead. Not immediate, but real on both fronts.

**Named attacker (from partner challenge):** Oracle Health (Cerner) — proof this kind of cross-module clinical model is already shipping natively on a rival EHR, and the competitive bar Seha Hub has to clear.

---

## Top Vulnerability
Platform Exposure: Seha Hub owns the underlying data plumbing across every module this product touches, so a well-differentiated wedge is one roadmap decision away from being absorbed natively — and a rival EHR has already proven the cross-module model works, raising the bar for how good Seha Hub's version has to be.

## Confidence Level
Medium — the moat and data advantage are real once the two feedback loops (denial outcomes, safety incidents) are actually built and running, but that build is not yet done, and platform absorption risk is a long-term threat rather than an immediate one.
