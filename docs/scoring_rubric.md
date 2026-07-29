# Scoring Rubric

Applied per document, per domain (Attention Regulation, Executive Functioning,
Self-Regulation, Everyday Functioning, Environmental Support, Wait-list Support). A single ICB
score per domain is the highest score reached by any passage across all its included
documents.

| Score | Definition | Worked anchor example |
|---|---|---|
| 0 | Absent | Domain not mentioned anywhere in the pathway's documentation. |
| 1 | Signposted | A one-line mention or link out (e.g. "patients may also find support via [charity]") with no detail on what the support involves. |
| 2 | Generic Provision | Pathway names a non-specific offer (e.g. "psychoeducation is available," a general self-help leaflet) without describing a structured or individualised method. |
| 3 | Targeted Intervention | Pathway names a specific structured intervention, programme, or referral route addressing the domain (e.g. a named CBT-for-ADHD group, a structured coaching programme, a defined Access to Work referral pathway). |

## Notes for coders

- Score the *pathway documentation*, not assumed clinical practice — this project's unit of
  analysis is the commissioned pathway (see Ethical Consideration #3 in the original research
  plan), so a domain scores 0 if it isn't documented even if it's plausible that it happens in
  practice.
- `dictionary/keywords.yaml`'s `targeted_signals` field gives domain-specific pointers on what
  tends to separate a 2 from a 3 — use judgement, these are prompts not hard rules.
- If a document contradicts an earlier one for the same ICB (e.g. an outdated PDF vs a current
  webpage), score from the most recent/currently active source per the inclusion criteria.
