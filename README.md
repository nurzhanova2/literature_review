# Dissertation Literature Review

## Research Topic

**Explainable artificial intelligence for the early identification of credit risk and latent deterioration in an individual borrower’s credit quality.**

## Research Goal and Rationale

**The main goal** is to develop and empirically test an explainable system that identifies latent deterioration in an individual borrower’s credit quality before formal default.

The study considers more than classification accuracy. A practical credit decision requires a calibrated risk probability, useful warning time, an acceptable number of false alerts, an explanation of the individual signal, and the ability to audit the decision.

The technical problem is formulated as follows:

```text
p_hat(i, t; h) = P[Y(i, t; h) = 1 | x(i, t)]
```

Here, `x(i, t)` denotes borrower features available no later than date `t`; `Y(i, t; h)` is a formally defined adverse event over horizon `h`; and `p_hat(i, t; h)` is the estimated risk probability. The design also defines `Z(i, t; h)`, an observable indicator of latent deterioration that does not substitute for formal default.

## Project Navigation

| Material | Russian version | English version | Contents |
|---|---|---|---|
| Full literature review | [literature_review_ru.md](docs/docs_ru/literature_review_ru.md) | [literature_review_en.md](docs/literature_review_en.md) | Critical review, a comparative table of 50 sources, and bibliography. |
| Detailed comparative table | [comparative_table_ru.md](docs/docs_ru/comparative_table_ru.md) | [comparative_table_en.md](docs/comparative_table_en.md) | Detailed comparison of 26 key studies with the proposed dissertation design. |
| Goal, research gap, and novelty | [goal_gap_novelty_ru.md](docs/docs_ru/goal_gap_novelty_ru.md) | [goal_gap_novelty_en.md](docs/goal_gap_novelty_en.md) | Object and subject, formalisation, hypotheses, research gap, claimed novelty, and limitations. |
| Bibliography | [bibliography.md](docs/bibliography.md) | [bibliography.md](docs/bibliography.md) | 50 sources with clickable DOI links. |
| Locally recovered PDFs | [source_pdfs/README.md](credit-risk-xai/source_pdfs/README.md) | [source_pdfs/README.md](credit-risk-xai/source_pdfs/README.md) | Register of local PDFs, DOI links, and access statuses. |

## What the Dissertation Design Evaluates

| Property | Example measures | Practical meaning |
|---|---|---|
| Discriminatory power | ROC-AUC, PR-AUC, recall/precision | Separates risky from non-risky observations. |
| PD calibration | Brier score, calibration curve | Tests whether a prediction can be interpreted as a risk probability. |
| Early warning | Lead time; share of events warned in advance | Measures the time available for credit intervention. |
| False alerts | False-positive rate; precision at a fixed threshold | Aligns the risk signal with operational workload. |
| Explanation stability | Overlap of top-k factors; repeatability across periods | Tests whether an explanation is an artefact of a particular sample. |

The data should be split chronologically: training on earlier periods, tuning on a subsequent period, and final evaluation on a separate **out-of-time** period. Features using future information are excluded as leakage.

## Evidence Base

| Measure | Count | Interpretation |
|---|---:|---|
| Sources in the bibliography | 50 | Complete bibliographic corpus of the review. |
| Studies in the detailed comparative table | 26 | Selected key studies for substantive comparison. |

## Recommended Review Order Before the Defense

1. Begin with the full [Russian](docs/docs_ru/literature_review_ru.md) or [English](docs/literature_review_en.md) literature review.
2. Read [Goal, Research Gap, and Novelty](docs/docs_ru/goal_gap_novelty_ru.md) to review the scientific framework and testable hypotheses.
3. Use the [detailed comparative table](docs/docs_ru/comparative_table_ru.md) to compare the proposed design with solutions proposed by other authors.
4. For questions about sources, open the [bibliography](docs/bibliography.md) and the [PDF register](credit-risk-xai/source_pdfs/README.md).
