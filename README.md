# ADHD Pathway Review

MSc Psychology dissertation project: a mixed-methods analysis of how consistently NHS and
third-sector adult ADHD pathways in England provide non-pharmacological support, across four
ICB area types (under-resourced, better-resourced, rural, urban), scored against a 0–3 rubric
over six psychological domains — Attention Regulation, Executive Functioning, Self-Regulation,
Everyday Functioning, Environmental Support, and Wait-list Support.

Full design: [`docs/methodology.md`](docs/methodology.md). Milestones: [`docs/timeline.md`](docs/timeline.md).
Rubric with worked anchors: [`docs/scoring_rubric.md`](docs/scoring_rubric.md).
Seed keyword dictionary: [`dictionary/keywords.yaml`](dictionary/keywords.yaml).

## Repo layout

- `docs/` — methodology, timeline, and scoring rubric.
- `dictionary/keywords.yaml` — seed keyword list per domain, to be refined during piloting.
- `data/manifest.csv` — log of identified ICBs/documents (template, not yet populated).
- `data/raw/<area_type>/` — where source ICB documents go once collected.
- `data/extracted/`, `data/coded/` — placeholders for pipeline output once it exists.
- `scripts/`, `tests/` — placeholders; the extraction/anonymisation/analysis code isn't
  written yet, to be built out later.

## License

Copyright (c) 2026 Theodore Eales. All Rights Reserved — see [`LICENSE`](LICENSE). This
repository is public for transparency/review purposes only; no permission to reuse, copy, or
redistribute is granted. Contact the author for permission.
