# Chartguard — AI Operating Policy

*What our AI can do, what it can't, and what happens when something goes wrong.*

*This is the customer-facing one-pager — written for an enterprise buyer's leadership, not for engineering. It is not a legal document. Full working detail, rationale, and cross-references to the diagnostic, flywheel, and reliability contract live in `05-the-guardrails/compounding-system.md`.*

## Scope
Chartguard covers three things: the safety-flag engine (cross-checking notes against pharmacy, lab, and order data for patient-safety conflicts), the denial-risk flag engine (documentation-gap detection for insurance denial prevention), and the cross-hospital pattern-sharing system that improves both over time.

**Not covered:** any other AI feature in Seha Hub outside Chartguard. Chartguard does not draft clinical notes, does not modify the medical record, and does not communicate with anyone outside the hospital on its own.

## What It Can Do Without a Human
- Surface a denial-risk flag for a clinician to review
- Suppress a denial-risk flag it has learned is a false positive for a specific clinician or unit
- Detect a pattern worth sharing across hospitals

## What Always Requires a Human
- Dismissing a safety flag — requires a signed acknowledgment, every time, no exceptions
- Rolling out a newly detected pattern to a hospital for the first time — requires clinical safety sign-off
- Anything that would draft-and-send a message, query, or appeal outside the system (not built today; pre-approved boundary for if it ever is)

## When We Escalate to a Human On-Call
- System confidence drops below 60% on any flag
- A real safety conflict may have been missed
- Hallucination rate exceeds 1%, or latency exceeds 1.5 seconds
- Weekly accuracy trend is declining by more than 1% a week

## How Often We Review the System
Not just on a calendar. We re-review any time the model changes, the prompts change, a new data source is connected, or a regulation changes. On top of that: weekly automated testing, real-time monitoring for safety-critical failures, and a quarterly full review.

## Who Can Shut It Off
The on-call clinical safety lead can halt the safety-flag engine immediately — no engineering deploy required. This authority exists whether or not an automated alert has fired.

## Regulatory Position
Governed as HIPAA-regulated patient data. We treat it as likely high-risk under the EU AI Act for any EU deployment. Whether it falls under medical-device regulation is still an open question we're tracking, not one we've resolved. *(This is our own product-level assessment, not a legal opinion.)*
