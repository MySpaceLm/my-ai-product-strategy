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

**Scope:** Covers Chartguard's flagging engine (safety + denial-risk), the Preference/suppression personalization layer, and the cross-hospital Network Intelligence pattern-propagation system. Explicitly does not cover Seha Hub's other native AI features outside the Chartguard module.

**Autonomy boundaries:**
- **OK solo (no human needed):** denial-risk flags surfacing for clinician review (not auto-corrected); Preference-loop suppression of non-safety flags; Network Intelligence pattern *detection* across hospitals.
- **Needs human:** any safety-flag dismissal (already signature-gated); Network Intelligence pattern *rollout* to a new hospital the first time (detection is automatic, but a pattern going live and changing what fires for clinicians at a new site requires clinical safety sign-off); any future agentic extension that would draft-and-send anything externally (see Agent Topology).

**Escalation triggers:** Reuses the Module 4 Reliability Contract thresholds directly — confidence <60%, suspected safety-flag false negative, hallucination >1%, latency >1500ms, drift >1%/wk. All route to the rotating clinical safety/CDI on-call.

**Audit cadence:** Not purely calendar-based — event-triggered first, calendar as the floor. Re-review is mandatory whenever: the flag-generation model changes, prompts change, a new data source or tool is integrated (e.g. a new EHR module feeds the reconciliation engine), or a regulatory change lands (e.g. an EU AI Act update). On top of that, a weekly cadence matches the Module 4 gold-set audit and LangSmith regression run; real-time for the safety-flag-engine auto-rollback conditions already defined in the Reliability Contract; quarterly for the Network Intelligence pattern library and for this governance policy itself.

**Regulatory exposure (EU AI Act / other):** HIPAA (patient data; see Memory below). Likely EU AI Act high-risk tier if deployed in EU health systems, given the safety-adjacent decision-support nature. Open question, not yet resolved: whether the safety-flag engine's degree of autonomy could bring it into Software-as-a-Medical-Device (SaMD/FDA) territory — flagged here rather than assumed either way. This is an initial product-level assessment, not a legal determination.

**Risk tier:** High.

**Immediate stop authority:** The rotating clinical safety/CDI on-call (the same role named throughout the Reliability Contract and HITL Architecture) can halt the safety-flag engine immediately — this is the same auto-rollback mechanism already wired to the Reliability Contract's alert thresholds, but it is also a standing manual authority, not only an automated one. No engineering approval or deploy cycle is required to pull that switch.

## Agent Topology
<!-- If using agents: what can each agent do? What can't it do? Who approves what? -->

*Chartguard today flags, it doesn't act — no agentic extension is live. This topology is written now so any future agentic feature (e.g., auto-drafting a physician query or a denial appeal) has to be built inside these boundaries, not decided after the fact. Structured on the M5 agent-governance framework: Autonomy, Tool Calls, Memory, Chain.*

**Autonomy — draft ≠ send:** A future agentic feature may draft a physician query or a denial-appeal letter. It may never submit, send, or message anything externally on its own. Draft-to-send is always a human action.

**Tool Calls — whitelist, not open access:** Read-only access to pharmacy, lab, orders, and allergy data for cross-module reconciliation. No write access to the medical record. No external messaging or payer-submission API access, now or in any planned agentic extension, without a separate governance review.

**Memory — what persists, TTL, who reads:** Per-clinician/unit suppression patterns and the validated cross-hospital pattern library persist. Suppression patterns are reviewed and expire if unused for 90 days. Only the flagging engine and compliance/audit exports can read this memory — no other Seha Hub module has access.

**Chain — named owner per handoff:** If a hospital's live flag output depends on an upstream validated pattern from the Network Intelligence loop, the named owner is the clinical safety lead who approved that pattern's promotion to that hospital — not "the system."

## Shadow AI Audit

*Repo columns per the exercise: Tool = workaround · Owner = signal source · Risk = frequency · Decision = build / partner / ignore (the keep/govern/kill placeholder is overwritten below, per the M5 instructions).*

| Tool (workaround) | Owner (signal) | Risk (frequency) | Decision |
|------|-------|-----------|----------|
| Clinicians pasting notes into ChatGPT/Claude directly for a quick second-opinion rewrite or summary | Capability gap — Chartguard flags issues but doesn't draft or rewrite notes | H | Build (and urgently ban raw PHI in consumer AI tools by policy until native summarization ships — this is a Samsung-path risk, not just a feature gap) |
| CDI/coding staff keeping a parallel spreadsheet/macro-based denial tracker to cross-verify Chartguard's flags | Trust gap — a new tool without an established track record yet | M | Ignore for now — revisit once the Reliability Contract's weekly accuracy numbers accumulate a real history |
| Nurses using ChatGPT or a Zapier recipe to auto-summarize long notes into shift-handoff bullets | Workflow gap — Chartguard has no handoff-summary output today | H | Build — natural extension of the existing Filler tier from Module 3 |
| Physicians using a personal ambient-scribe subscription (DAX or similar) for dictation, since Chartguard only analyzes notes, it doesn't generate them | Capability gap — narrower scope than DAX by design (Module 2 positioning) | M/H | Partner — integrate the scribe's output as an input to Chartguard's flags instead of competing head-on; reinforces the "occupy the gap DAX doesn't fill" strategy |
| Revenue-cycle/finance team building a manual spreadsheet to track how many denials Chartguard actually prevented | Capability gap — no native "confirmed catch" reporting dashboard, which the Module 3 outcome-based pricing model depends on | H | Build, urgently — this isn't optional; the hybrid pricing model can't be trusted or billed without it |

**Total tools found:** 5
**Tools after triage (build candidates):** 3 (note-summarization, shift-handoff summaries, confirmed-catch reporting dashboard)
**Estimated hidden spend:** ~$3,250/mo (illustrative) — ~$1,250/mo in informal ChatGPT/Claude seats among clinicians (~50 of 500 seats x ~$25/mo) + ~$2,000/mo in personal ambient-scribe subscriptions among physicians (~20 physicians x ~$100/mo); excludes the harder-to-quantify staff time lost to manual spreadsheet tracking
