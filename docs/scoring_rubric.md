# Scoring Rubric

Operationalises Brown's (2013) model of ADHD as executive-function impairment. Applied per
passage, across two dimensions: which domain a difficulty or support statement targets, and how
much provision (if any) is documented for it.

## Step 1 — Domain: does the passage mention a deficit, and which one?

| Code | Domain | Operational definition | Example |
|---|---|---|---|
| ATT | Attention | Focusing, sustaining focus, and shifting attention. | "Adults with ADHD may find it difficult to maintain concentration during long meetings." |
| PER | Persistence | Regulating alertness, sustaining effort, and maintaining processing speed. | "Individuals often struggle to see long-term work projects through to completion." |
| ESR | Emotional Self-Regulation | Managing frustration, modulating emotional responses. | "A substantial proportion of patients experience severe emotional dysregulation." |
| WM | Working Memory | Utilising working memory, holding information in mind, and accessing recall. | "Forgetfulness frequently acts as a barrier to managing daily household routines." |

A passage may touch more than one domain — code each domain instance separately, since the unit
of analysis for reliability and scoring is the coded span, not the whole passage.

## Step 2 — Provision level: what does the passage document for that deficit?

| Level | Label | Operational definition | Example |
|---|---|---|---|
| 1 | No Support | The document recognises a specific deficit, but no corresponding intervention or pathway is listed. | "Adults with ADHD often struggle with emotional regulation." |
| 2 | Signposting | Directing the individual to external, third-party, or uncommissioned resources. | "For strategies on improving daily focus, patients can be directed to the ADHD UK charity website." |
| 3 | General Support | A vague, non-specific mention of help without clearly documented support. | "Psychological therapies may be beneficial for managing executive functioning." |
| 4 | Targeted Support | Specific, locally provided, and formally commissioned interventions targeting a mentioned deficit. | "The service provides an 8-week structured CBT course designed to build emotional regulation skills." |

## Notes for coders

- Code what is **documented**, not what you infer might happen in practice — a passage that never
  mentions provision for a deficit is coded 1 (No Support) even if it seems plausible that support
  exists.
- `dictionary/keywords.yaml` is a retrieval aid for the automated pipeline, not a coding
  shortcut — code from the passage's actual meaning, not keyword presence.
- If a passage's phrasing is ambiguous between two provision levels (e.g. General vs Targeted
  Support), prefer the lower level unless the passage names a specific structured
  intervention/programme.
- Binary agreement (Cohen's Kappa) is calculated on Step 1 — whether *any* deficit is mentioned at
  all — and Weighted Kappa on Step 2's ordinal provision level, per `docs/methodology.md`.
