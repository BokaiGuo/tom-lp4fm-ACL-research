# LP4FM 2026 Manuscript

Target venue: NeurIPS 2026 Workshop on Linguistic Principles for Foundation
Models (LP4FM).

## Submission mode

- Primary target: Full Paper, at most 9 main-text pages.
- Fallback: Short Paper, at most 4 main-text pages.
- References and appendices are outside the stated main-text limit.
- The submission is anonymous and non-archival according to the workshop CFP.

## Template status

LP4FM requests the official NeurIPS 2026 template. The NeurIPS 2026 author kit
was not publicly downloadable when this manuscript directory was assembled.
`neurips_2025.sty` is therefore an official but provisional compile-only
placeholder, loaded in neutral `preprint` mode so the PDF does not claim to be
a NeurIPS 2025 submission. Replace it with `neurips_2026.sty`, remove the
temporary `preprint` option, and follow the LP4FM anonymity settings before
submission.

## Build

```bash
cd /home/gbk/code/ToM/paper/lp4fm2026
latexmk -pdf -interaction=nonstopmode main.tex
```

Regenerate all evidence tables before compiling:

```bash
cd /home/gbk/code/ToM
python scripts/build_lp4fm2026_experiment_tables.py
python scripts/audit_lp4fm2026_manuscript.py
```

The current draft uses 8 conservatively counted main-text pages and 15 pages in
total. The main text is organized as a four-part scientific argument rather
than an experiment inventory: pooled effects fail to identify the controller;
typed conflict reveals source routing; a reportative reversal motivates an
accessibility bottleneck; held-out composition and frozen checkpoint transfer
test the resulting law. The appendix retains the complete evidence inventory,
pooled and cross-pool results, same-world topology matrix, cross-realization
matrix, construction factorial, frozen compound predictions, saturation-model
comparison, checkpoint boundary, statistics, and exact scoring configuration.

## Scientific claim

The manuscript claims a controlled behavioral source-accessibility saturation
law in Qwen2.5-7B-Instruct and a checkpoint-sensitive composition boundary:
Qwen3-8B and matched Qwen2.5-7B Base preserve partial control-relative shape but
not the directional residual. Affine-gain invariance rules out scalar logit
compression as a complete explanation. The manuscript does not claim a
hidden-state circuit, a unique aggregation operator, a causal effect of
instruction tuning, human psycholinguistic universality, or broad model-family
transfer.
