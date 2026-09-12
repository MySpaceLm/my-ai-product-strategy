# The Prototype Bet

## What I Built
<!-- One sentence: what does this prototype demonstrate? -->
Chartguard — a two-pane clinical workstation mockup where a sample ED note is analyzed on demand, surfacing patient-safety conflicts (e.g., a documented allergy vs. an active conflicting order) and denial-risk documentation gaps (e.g., vague diagnostic language, missing specificity) as severity-ranked flags with suggested fixes.

## Tool Used
<!-- v0 / Cursor / Lovable / other -->
Claude (Artifacts)

## Prototype Link
<!-- Paste the shareable URL -->
https://claude.ai/code/artifact/98b72420-d7fa-4d75-99a6-66f7ea532b47

## AI Value Archetype
Copilot (augments clinicians and CDI/compliance staff at the point of documentation) with an Oracle component (surfaces hidden safety/denial signal from the note).

## The Bet in One Sentence
A cross-module AI copilot bolted onto Seha Hub that improves clinical note quality in real time, catching patient-safety gaps (via pharmacy/lab/order cross-checks) and denial-causing documentation gaps before they happen — for hospital clinicians, CDI specialists, and coders, because the leading alternative (Nuance DAX) only writes the note and doesn't check it against the rest of the chart.

## Kill Criteria
Kill this bet if, after a pilot on one unit for 60–90 days: (1) the denial-outcome and safety-incident feedback loops don't produce a statistically significant reduction in denials or safety near-misses vs. baseline (3M/Solventum alone), or (2) Seha Hub's own team ships a native equivalent within the pilot window, or (3) the hospital's payer mix and incident volume are too low to generate a meaningful training signal within two quarters.
