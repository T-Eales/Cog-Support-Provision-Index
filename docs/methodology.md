# Methodology

Expands the dissertation proposal's Method section into an executable protocol. See the proposal
for full Background, Theory, Rationale, and Hypotheses — reproduced here only where they inform a
methodological or implementation decision.

## 1. Design

A supervised computational validation design, comparing automated text classifiers against a
human reference baseline, run in three phases:

1. **Development** — build the human-annotated reference standard; train the baseline and
   TF-IDF/logistic-regression models.
2. **Calibration** — evaluate the trained models on a held-out set to fix each model's
   confidence/abstention threshold.
3. **Test** — frozen models classify the test set; results are compared to the human reference
   standard.

## 2. Document identification & inclusion criteria

Materials are publicly accessible adult ADHD pathway documents: NHS trust service pages, regional
ICB referral guidance, patient information leaflets, and commissioned third-sector provision
frameworks. Target: 150–250 documents split across these four sources.

**Inclusion**: publicly accessible, currently active, and specifically detailing adult ADHD
diagnostic or support pathways in an English Integrated Care Board (ICB).

**Exclusion**: paediatric ADHD pathways; general ADHD material with no regional service detail;
private-sector documentation outside the Right to Choose scheme.

Retrieval (`scripts/`) uses BeautifulSoup and DuckDuckGo search (e.g. `Adult ADHD pathway
site:nhs.uk`, `Adult ADHD service specification CMHT NHS`) and:

- checks each site's `robots.txt` before requesting a page,
- rate-limits requests (3s between calls) to avoid loading public NHS/third-sector servers,
- logs source authors, URL, publication date, and document type per document in
  `data/manifest.csv`, for an auditable evidence trail (Gebru et al., 2021).

## 3. Passage extraction & filtering

Extracted raw text is split into paragraph-length passages and filtered for ADHD keywords
(`dictionary/keywords.yaml`), expected to yield 500–750 candidate passages. Passages are split at
the **provider level** — not the passage level — into development (~290), calibration (~85), and
test (~125) sets, so no single provider's passages leak across sets.

## 4. Coding rubric & reliability procedure

Passages are coded against [`docs/scoring_rubric.md`](scoring_rubric.md), an operationalisation of
Brown's (2013) model (ATT, PER, ESR, WM) and a 4-level provision scale (No Support / Signposting /
General Support / Targeted Support).

- The primary researcher and one independent rater (background in research methods, trained on
  the rubric) code independently, blind to each other's decisions and to all model outputs.
- **Stage 1**: both code a development subsample (~95 passages), then meet to discuss
  disagreements.
- **Stage 2**: the rater independently codes the full test set (~125 passages) plus a third of
  the development set — strictly blind, with no visibility into the primary researcher's codes
  or any model output.
- **Stage 3**: disagreements between the two are reconciled into a single consensus standard.
- **Cohen's Kappa** is used for the binary "is a deficit mentioned" judgement; **Cohen's Weighted
  Kappa** for agreement across the ordinal provision levels.

## 5. Models

Three classifiers, all trained on the development set:

1. **Keyword/context-rule baseline** — transparent and rule-based, using
   `dictionary/keywords.yaml`.
2. **Regularised logistic regression over TF-IDF features.**
3. **Dense sentence embeddings** (Sentence-BERT-style) — captures semantic similarity rather
   than surface keyword matching, so it can generalise across public-facing vs clinical phrasing
   of the same construct (e.g. "help with organisation and planning" vs "working memory").

## 6. Calibration & thresholds

Because model confidence scores don't reliably track accuracy (Guo et al., 2017), each model is
evaluated on the calibration partition to fix an abstention threshold: classifications below
threshold are routed to human review rather than accepted automatically. Once thresholds are set,
the models, coding rubric, and keyword dictionary are frozen (Kapoor & Narayanan, 2023) before the
test set is touched.

## 7. Evaluation

- **H1** (the supervised model significantly outperforms the keyword baseline) is tested with
  McNemar's test on paired classification outcomes.
- Precision, recall, and macro-F1 are reported per category and overall, alongside confusion
  matrices against the human reference standard.
- A structured, descriptive qualitative error analysis is performed on misclassified passages.
- Success criterion: the best-performing model reaches **≥90% precision** on the **≥50%** of test
  passages it classifies above its confidence threshold.

## 8. Limitations (carried from the proposal)

- Documented provision is not the same as clinical practice — a service may deliver support
  that's absent from its published specification, or publish a specification it cannot sustain
  (Magon et al., 2015).
- Providers with more administrative capacity tend to publish more detailed documentation, so
  detection may correlate with resourcing rather than actual provision quality.
- The primary researcher is not blind to the study's aims; dual coding mitigates but does not
  remove this bias (O'Connor & Joffe, 2020).
- Category imbalance may depress macro-F1 and destabilise kappa for rarer categories (McHugh,
  2012; Sokolova & Lapalme, 2009).
- Mapping administrative language onto psychological constructs (e.g. "help with organisation
  and planning" → working memory) is interpretive, not exact (Flake & Fried, 2020).
- Findings generalise only to English NHS/third-sector provision — ADHD services are commissioned
  differently elsewhere in the UK.
