# Leakage-Audited CVD Prediction with Domain-Shift Diagnostics
## Overview

Clinical ML pipelines often skip three checks: whether the "external" cohort is truly disjoint from the training data, whether an explainability finding holds beyond one model, and whether an internal-vs-external gap reflects covariate shift. This case study uses cardiovascular disease (CVD) prediction to show how each omission produces misleading conclusions.

**Key findings**

| Check | Without it | With it |
|---|---|---|
| Leakage audit | 85.52% external accuracy; 3 significant subgroup disparities | All 297 UCI Cleveland patients were already in the fedesoriano pool. After removal (621 patients, zero overlap), external accuracy is **73.40%** (ROC-AUC 0.8152) vs. **91.30% ± 4.02%** internal 5-fold CV; no subgroup disparity survives |
| Domain-shift diagnostic | Gap unexplained | Domain classifier AUC 0.90; shift driven by RestingECG, MaxHR and Cholesterol missingness. Feature-level remedies with threshold re-tuning raise external accuracy to 77.78% (about a quarter of the gap) |
| Multi-model explanation check | Single-model SHAP–LIME result | Confidence–agreement correlation is weak: r = 0.22 (Random Forest), r = 0.14 (XGBoost), p < 0.01, 536 explained instances |

## Methodology (summary)

1. Identify and remove all fedesoriano rows matching the Cleveland cohort (exact feature match; 8-field and 11-field keys give identical results).
2. Train a soft-voting ensemble (LR, RF, GB, XGBoost, calibrated SVM) with scaling, SMOTE and tuning confined to each training fold.
3. Explain fold-tuned RF and XGBoost with SHAP and LIME; compare rankings with Kendall's τ, stratified by prediction confidence.
4. Statistical testing: Wilcoxon, corrected resampled t-test (Nadeau–Bengio), chi-square/Fisher with Holm–Bonferroni correction, logistic regression.
5. External validation on the 297-patient Cleveland cohort, plus a domain-classifier diagnostic.

## Data

Both datasets are public and not redistributed here:

- fedesoriano Heart Failure Prediction dataset (Kaggle)
- UCI Heart Disease (Cleveland) dataset, DOI: [10.24432/C52P4X](https://doi.org/10.24432/C52P4X)

## Repository contents

```
main.tex          Manuscript (Springer Nature sn-jnl template, two-column)
references.bib    Bibliography
sn-jnl.cls        Springer Nature journal class
sn-mathphys-num.bst  Numbered reference style
figures/          Figure files (see below)
```

Figures expected by `main.tex` (place them next to `main.tex`, or adjust paths):
`methodology_flowchart.pdf`, `confusion_roc.png`, `calibration_curve.png`, `disagreement_analysis.png`, `domain_shift_analysis.png`.

## Building

**Overleaf:** upload the folder as a project and compile with pdfLaTeX.

**Locally:**

```bash
latexmk -pdf main.tex
```

## Citation

If you use this work, please cite (details to be updated after publication):

```bibtex
@article{rashid2026leakage,
  author  = {Rashid, Muhammad Anamur and Chowdhury, Antu},
  title   = {Framework for Cardiovascular Disease Prediction with Leakage-Audited External Validation and Domain-Shift Diagnostics},
  journal = {Health Information Science and Systems},
  year    = {2026},
  note    = {Under review}
}
```
