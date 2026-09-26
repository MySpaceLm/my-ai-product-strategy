# Compounding System Design

*Maps directly onto the three loops already scored in Module 2's Data Flywheel (13/20). Recursive Learning = Correction + Preference loops; Cross-Domain Transfer = Domain Context loop; Network Intelligence = Network loop.*

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| Recursive Learning | Signed safety-flag dismissals (compliance log) + HITL reviewer corrections (Module 4) | Per-clinician/unit flag suppression adjusts live (works); the underlying flag-generation model itself is retrained/tuned — it doesn't (no pipeline yet) | N | broken |
| Cross-Domain Transfer | A pattern validated and flagged in one unit (e.g. ED) | The same pattern applied, policy-gated, in a different unit (e.g. OR) | N | missing |
| Network Intelligence | Validated patterns detected automatically per hospital, raw patient data never shared | Pattern proposed and rolled out to other hospitals in the network | Y | active |

**Broken loop identified (self-identified — no live partner pass; a real one is still an open item):** Recursive Learning at the model level → **fix:** build the pipeline that feeds the signed-dismissal compliance log plus HITL reviewer corrections into a periodic (weekly, matching the Module 4 gold-set audit cadence) retraining/threshold-tuning job. The raw signal already exists — it's the reuse mechanism that's missing, exactly as Module 2's Correction loop score (2/5) flagged.

## Context Connectivity
<!-- How does knowledge flow across teams and domains? Where does it silo? -->

**Where knowledge silos:** Counterintuitively, the silo sits *inside* a single hospital, not between hospitals. Network Intelligence already moves validated patterns across an entire hospital network without ever exposing raw data — but Cross-Domain Transfer, moving a validated pattern between two units *in the same building* (ED to OR), doesn't exist yet. Knowledge crosses organizational boundaries more easily than it crosses unit boundaries within one org, because the Network loop was built as real infrastructure while cross-unit transfer was only ever designed on paper (the policy-gating model exists; the pipeline doesn't).

**The freeze test:** freeze all engineering for 3 months — does Chartguard still win? Partial pass. The two active loops (Preference suppression, Network Intelligence) are live infrastructure, not manual iteration — they keep detecting and propagating patterns on their own with zero engineering touching code, so they'd keep compounding through a freeze. That's real evidence of compounding, not just scaling. But the two broken/missing pieces (model-level Recursive Learning, Cross-Domain Transfer) would be exactly as broken after 3 months as before — they were never running, so a freeze changes nothing for them. The honest read: Chartguard is compounding on two of three loops, and stalled-flat on the third.

## Governance Policy

**Scope:**
**Autonomy boundaries:**
**Escalation triggers:**
**Audit cadence:**
**Regulatory exposure (EU AI Act / other):**

## Agent Topology
<!-- If using agents: what can each agent do? What can't it do? Who approves what? -->

## Shadow AI Audit

| Tool | Owner | Risk Level | Decision |
|------|-------|-----------|----------|
| | | H / M / L | keep / govern / kill |
| | | H / M / L | keep / govern / kill |
| | | H / M / L | keep / govern / kill |

**Total tools found:**
**Tools after triage:**
**Estimated hidden spend:**
