# Methodology

Expands the original research plan's Analysis Plan into an executable protocol. See the
original plan for Research Aims, Research Question, and Hypotheses (H1/H2/H0) — reproduced
here only where they inform a methodological decision.

## 1. Sampling frame

Propose **8 ICBs total, 2 per area category**:

- 2 under-resourced
- 2 better-resourced
- 2 rural
- 2 urban

Categories are not mutually exclusive (an ICB can be, e.g., both rural and under-resourced) —
record both classifications per ICB in `data/manifest.csv`.

Classification criteria to fix before selection:
- **Resourcing**: published ICB funding allocation per capita and/or adult ADHD service
  staffing levels (source: NHS England ICB allocations, published board papers).
- **Rurality**: ONS Rural-Urban Classification for the ICB's constituent local authorities
  (majority-rural vs majority-urban).

This N is a starting proposal, not fixed by the original plan — revisit with your supervisor
before Week 4 (see `docs/timeline.md`).

## 2. Document identification procedure

Per ICB, systematically search and log in `data/manifest.csv`:
1. ICB website commissioning/policies section
2. Named ADHD service provider website(s) commissioned by that ICB
3. Any publicly available shared-care protocol or service specification (FOI disclosure logs
   are a fallback if not otherwise published)

For every candidate document, record in the manifest: ICB name, area type(s), document title,
source URL, publication/last-reviewed date, and an include/exclude decision with a one-line
reason against the original plan's inclusion/exclusion criteria (e.g. "excluded — CAMHS
pathway," "excluded — pre-2020, no evidence of current use").

## 3. Keyword dictionary construction

`dictionary/keywords.yaml` is seeded from the domain justifications in the original plan.
Before full-scale extraction:
1. Run `scripts/extract_passages.py` on 2–3 pilot documents.
2. Manually review the candidate passages for false positives/negatives per domain.
3. Revise the keyword list accordingly and record what changed and why (a short changelog at
   the top of `keywords.yaml` is sufficient) — this transparency is part of what makes the
   retrieval step auditable rather than a black box.

## 4. Coding & reliability procedure

- One primary coder scores every candidate passage against `docs/scoring_rubric.md`.
- A second coder independently double-codes a **20% random subsample** of passages (stratified
  across the 4 area types so no category is left unchecked).
- `scripts/reliability.py` computes **weighted Cohen's kappa** (appropriate here because the
  0–3 rubric is ordinal, not nominal) between the two coders on that subsample.
- Disagreements of more than 1 rubric point are resolved by discussion between coders; if
  unresolved, a third rater (e.g. supervisor) breaks the tie. Log resolutions in the coding
  sheet's notes column.

## 5. Statistical analysis, mapped to hypotheses

| Hypothesis | Test | Rationale |
|---|---|---|
| H1: better-resourced areas offer more targeted interventions | Mann-Whitney U comparing domain scores, better-resourced vs under-resourced ICBs | Ordinal outcome, small independent-groups N → non-parametric |
| H2: rural areas provide a wider variety of non-pharmacological treatment | Count of domains scoring ≥2 per ICB; compare rural vs urban distributions (Mann-Whitney U) | "Variety" operationalised as breadth of domains reaching at least generic provision |
| H0: all areas lack support in ≥2 domains | Proportion of the 8 sampled ICBs with ≥2 domains scoring 0–1, reported descriptively | Directly matches the stated null — no inferential test needed, just the proportion |

`scripts/analyse.py` implements all three and exports summary tables/figures.

## 6. Limitations (carried from the original plan)

The unit of analysis is the **commissioned pathway**, not lived patient experience or clinical
delivery — documented policy may not reflect practice. This is an accepted scientific
limitation of a document-based design (Ethical Consideration #3), not something the analysis
can correct for; it should be stated explicitly in the discussion.
