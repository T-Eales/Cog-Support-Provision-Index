# An Investigation into the Feasibility of Automated Classification of ADHD Care Pathways

MSc Psychology (University of Essex Online) dissertation project: builds and evaluates a
supervised NLP text classifier that distinguishes recognition of an ADHD-related deficit from
documented provision of support for it, across public-facing adult ADHD pathway documents in
England — NHS trust service pages, ICB referral guidance, patient information leaflets, and
commissioned third-sector provision frameworks.

Passages are coded against **Brown's (2013)** model of ADHD as executive-function impairment,
using four of its six clusters — Attention (ATT), Persistence (PER), Emotional Self-Regulation
(ESR), and Working Memory (WM) — the four most frequently documented barriers requiring
intervention in adult ADHD populations. Each passage is also scored on a 4-level provision scale:
No Support, Signposting, General Support, Targeted Support.

**H1:** the supervised model will significantly outperform a transparent keyword/rule-based
baseline; the best-performing model should reach ≥90% precision on the ≥50% of test passages it
classifies above its confidence threshold.

Full design: [`docs/methodology.md`](docs/methodology.md). Timeline: [`docs/timeline.md`](docs/timeline.md).
Coding rubric: [`docs/scoring_rubric.md`](docs/scoring_rubric.md). Seed keyword dictionary:
[`dictionary/keywords.yaml`](dictionary/keywords.yaml).

## Design in brief

- **Retrieval** — a rate-limited, robots.txt-respecting Python scraper (`scripts/`) searches for
  and downloads adult ADHD pathway documents, logs each in `data/manifest.csv` (source, URL,
  publication date, document type) for an auditable evidence trail, and extracts readable text.
- **Passage extraction** — raw text is split into paragraph-length passages and filtered for
  ADHD keywords, expected to yield ~500–750 candidate passages from 150–250 documents.
- **Human reference standard** — passages are split at the provider level into development
  (~290), calibration (~85), and test (~125) sets to prevent data leakage. The primary
  researcher and one independent rater code passages against `docs/scoring_rubric.md`, blind to
  each other's decisions; disagreements are reconciled into a consensus standard. Inter-rater
  reliability uses Cohen's Kappa (binary: is a deficit mentioned) and Cohen's Weighted Kappa
  (ordinal: provision level).
- **Models** — three classifiers are trained on the development set and compared: a
  keyword/rule-based baseline, regularised logistic regression over TF-IDF features, and a dense
  sentence-embedding model. Confidence/abstention thresholds are set on the calibration set, then
  everything is frozen before the test set is classified.
- **Evaluation** — precision, recall, macro-F1, and per-category confusion matrices against the
  human reference standard; McNemar's test for H1; a qualitative error analysis of misclassified
  passages.

## Repo layout

- `docs/` — methodology, timeline, and scoring rubric.
- `dictionary/keywords.yaml` — seed keyword list per Brown's-model domain and per provision
  level, refined during piloting.
- `data/manifest.csv` — log of identified documents (template, populated during collection).
- `data/raw/<source_type>/` — source documents once collected, split by document type: NHS trust
  pages, ICB referral guidance, patient leaflets, third-sector frameworks.
- `data/coded/` — placeholder for the human-coded reference standard once coding is complete.
- `scripts/` — retrieval, cleaning, and modelling code.
- `tests/` — placeholder for automated tests on the extraction/scoring pipeline.

## Related tooling

The human coding itself — blind dual-rater annotation, consensus reconciliation, Cohen's
Kappa/Weighted Kappa export, and the human-reference export used to score the three classifiers —
is done in a separate private web app, not part of this repo.

## License

Copyright (c) 2026 Theodore Eales. All Rights Reserved — see [`LICENSE`](LICENSE). This
repository is public for transparency/review purposes only; no permission to reuse, copy, or
redistribute is granted. Contact the author for permission.
