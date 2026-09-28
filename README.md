# Dissertation Literature Review

## Research Topic

**Explainable Artificial Intelligence for the Early Identification of Credit Risk and Latent Deterioration in Borrower Quality**

This repository contains the literature review for a dissertation focused on explainable artificial intelligence, credit-risk modelling, early warning, and the identification of latent deterioration in borrower quality before formal default.

---

## Research Question

> **How effectively can explainable machine-learning models identify latent deterioration in borrower quality before formal default under an out-of-time validation design, when assessed jointly for discrimination, calibration, warning lead time, false-alert burden, and the temporal stability of explanations?**

The study treats latent deterioration as a pre-specified observable transition that is independent of the candidate model being evaluated.

---

## Project Structure

```text
literature review/
│
├── README.md
│
└── docs/
    ├── bibliography.md
    │
    ├── docs_en/
    │   └── literature_review_en.md
    │
    └── docs_ru/
        └── literature_review_ru.md
```

## Files

### English Literature Review

[`docs/docs_en/literature_review_en.md`](docs/docs_en/literature_review_en.md)

Contains the English version of the dissertation literature review.

### Russian Literature Review

[`docs/docs_ru/literature_review_ru.md`](docs/docs_ru/literature_review_ru.md)

Contains the Russian version of the dissertation literature review.

### Bibliography

[`docs/bibliography.md`](docs/bibliography.md)

Contains the complete bibliography used by both versions of the literature review.

The bibliography currently includes **54 sources**.

---

## Literature Review Structure

The literature review is organised into the following sections:

1. **Scope of the Review and Formulation of the Problem**
2. **From Statistical Scoring to Machine-Learning Ensembles: Evidence and Its Limits**
3. **Explainability in Credit-Risk Models**
4. **Class Imbalance and the Stability of Explanations**
5. **Temporal Risk and Survival Modelling**
6. **Early Warning and Latent Deterioration Before Default**
7. **Calibration and Decision Usefulness**
8. **Temporal Validation, Drift, Governance, and Auditability**
9. **Synthesis, Research Gaps, and the Present Study**

---

## Main Research Focus

The review focuses on credit-risk modelling at the level of the individual borrower.

It distinguishes between:

* borrower default;
* delinquency;
* corporate financial distress;
* bank default;
* systemic financial risk;
* early-warning signals;
* latent deterioration in borrower quality.

These phenomena are treated as related but non-equivalent and are not compared directly without considering differences in target definition, data, borrower population, time horizon, and validation design.

---

## Research Areas Covered

The literature review covers:

* logistic regression and traditional credit scoring;
* Random Forest, XGBoost, LightGBM, and ensemble models;
* explainable artificial intelligence;
* SHAP and LIME;
* intrinsic and post-hoc interpretability;
* class imbalance;
* explanation stability;
* dynamic credit-risk modelling;
* survival analysis;
* time-to-event modelling;
* early-warning systems;
* borrower deterioration before default;
* probability calibration;
* ROC-AUC and PR-AUC;
* recall and precision;
* Brier score and calibration analysis;
* false-alert burden;
* warning lead time;
* out-of-time validation;
* target leakage;
* temporal distribution shift;
* model monitoring;
* auditability and model governance.

---

## Central Literature Review Conclusion

The reviewed literature shows that credit-risk models should not be evaluated solely on predictive discrimination.

A model may achieve strong ROC-AUC while still:

* producing poorly calibrated probabilities;
* generating excessive false alerts;
* identifying deterioration too late;
* losing predictive quality over time;
* producing unstable local explanations.

Therefore, credit-risk monitoring requires consideration of several interconnected dimensions:

**predictive performance, probability calibration, temporal robustness, warning timeliness, false-alert burden, and explanation stability.**

The literature review consequently treats early identification of borrower deterioration as a temporal monitoring problem rather than only as a conventional binary default-classification task.

---

## Citation Convention

Both literature-review versions use numerical citations:

```text
[1], [2], [3], ... [47]
```

All citation numbers correspond to entries in:

```text
docs/bibliography.md
```

The citation numbering must remain identical in the Russian and English versions.
