# Research Framework: Aim, Research Gap, and Claimed Novelty

The reference numbering in this section follows the [IEEE bibliography](bibliography.md).

## 1. Object, Subject, and Unit of Analysis

**Object of research** — the process of assessing and monitoring credit risk in a portfolio of individual borrowers.

**Subject of research** — machine-learning methods, survival modelling, and explainable artificial intelligence for the early identification of deterioration in a borrower’s credit quality, with decisions that can be verified and audited.

**Unit of analysis** — borrower *i* at observation time *t*, rather than a credit institution, an industry, or the financial system as a whole. This distinction is essential: individual default, corporate financial distress, and systemic risk are different target phenomena; their metrics and risk drivers cannot be transferred directly across levels of analysis [9], [12], [34], [36].

## 2. Aim and Research Question

**Aim of the research** — to develop and empirically test an explainable system for identifying latent deterioration in an individual borrower’s credit quality before formal default. The system should simultaneously provide adequate discriminatory power, a calibrated risk-probability estimate, a useful early-warning horizon, and a documented explanation for each individual decision.

**Main research question:** under what data conditions, validation design, and explanation methods can a dynamic credit-risk model identify a deterioration in borrower quality before formal default without compromising probability calibration or decision auditability?

## 3. Research Tasks

To achieve this aim, the following tasks are proposed:

1. Operationalise formal default and latent deterioration for the selected credit portfolio before model training.
2. Construct a temporal dataset containing only features available at time *t* and eliminate target leakage.
3. Compare an interpretable baseline (logistic regression or scorecard) with ensemble models and, where a temporal structure is available, survival models [4], [6], [7], [10], [37].
4. Assess model quality using sequential temporal validation, including an out-of-time period, rather than only a random train/test split [6].
5. Evaluate discriminatory power, probability-of-default calibration, warning timeliness, and the false-alarm rate within one evaluation framework.
6. Produce global and local explanations of predictions, and assess the stability of the explanations when the time period, sample composition, and class imbalance change [11], [16].
7. Define requirements for model-decision logging, drift monitoring, data control, and the procedure for revising a risk signal [40].

## 4. Technical Problem Formulation

Let *i* = 1, …, *N* denote borrowers and *t* = 1, …, *T* denote discrete observation dates. For each borrower, a feature vector is formed:

```text
x(i, t) = (x¹(i, t), x²(i, t), …, xᵖ(i, t))
```

The vector `x(i, t)` contains only information available no later than time *t*. For a fixed prediction horizon *h*, the model estimates:

```text
p_hat(i, t; h) = P[Y(i, t; h) = 1 | x(i, t)]
```

Here, `Y(i, t; h) = 1` denotes the occurrence of a predefined adverse event between *t* and *t + h*. The event definition must be established in the study design before modelling — for example, as a regulatory or bank-defined delinquency threshold, a default status, or another documented credit event.

In addition, an indicator of latent deterioration, `Z(i, t; h)`, is introduced. It must not arbitrarily substitute for default. It must be defined in advance through an observable risk-transition rule, for example, a move to a riskier segment, a persistent increase in calibrated PD, or a predefined delinquency state. The specific threshold is selected in accordance with portfolio policy and fixed before the results are evaluated.

Where historical data before the event are available, a survival formulation is also considered:

```text
T_i = min {t : Y(i, t) = 1}
δ_i = 1 if the event is observed; δ_i = 0 if the observation is right-censored
```

The indicator `δ_i` accounts for right censoring. This formulation enables the estimation of both event probability and time to event; its use should be determined by the actual data structure, rather than by an algorithmic preference [6], [10], [16].

For each local decision, the explanation is represented by a vector of feature contributions:

```text
phi(i, t) = (phi¹(i, t), …, phiᵖ(i, t))
```

This vector may be obtained, for example, using SHAP or a local surrogate approach. Such contributions explain the behaviour of the trained model; they do not establish a causal effect of a feature on default [2], [11].

## 5. Validation Design and Quality Criteria

The data are split chronologically: training uses earlier periods, tuning uses a subsequent validation period, and final evaluation uses an unused out-of-time period. All feature transformations, oversampling, variable selection, and hyperparameter tuning must be performed within the training window only. Identifiers of future status, features observed after the decision date, and aggregates that use future observations are excluded as potential sources of leakage.

| Measured property | Main measure | Meaning for the credit-risk setting |
|---|---|---|
| Discriminatory power | ROC-AUC, PR-AUC, recall/precision | Shows how well the model separates risky observations; with rare defaults, PR-AUC and recall cannot be replaced by accuracy. |
| PD calibration | Brier score, calibration curve, calibration slope/intercept | Tests whether the prediction can be interpreted as a risk probability rather than only as a ranking score. |
| Early warning | Lead time; share of events warned before the event | Shows the time-related usefulness of the signal for intervention. |
| False alarms | False-positive rate, precision at a fixed threshold, cost-sensitive evaluation | Enables the benefit of early warning to be balanced against operational workload. |
| Explanation stability | Overlap of top-k features, rank correlation of contributions, repeatability across periods | Tests whether an explanation is an artefact of a particular sample or balancing procedure. |

The alert threshold *c* is selected not by maximising accuracy, but with regard to the objective function of the credit process: the acceptable number of false alarms, the cost of missing deterioration, and the required lead time. The final model is therefore selected using the full set of measures, not a single ROC-AUC or F1 score.

## 6. Research Gap

The literature review identifies five distinct streams of research: ensemble credit-risk prediction [4], [7]; explanation of complex models using SHAP and LIME [2], [5], [11]; treatment of imbalanced samples [11], [34]; dynamic and survival modelling [6], [10], [16]; and model governance [40]. However, the reviewed corpus contains a limited number of studies that, within one reproducible framework, simultaneously:

- focus on the **individual-borrower** level rather than corporate distress or systemic risk;
- define latent deterioration before formal default as a distinct, observable outcome;
- measure ranking quality, PD calibration, lead time, and false alarms;
- test the model on an out-of-time period without leakage;
- assess the stability of local explanations over time and under class imbalance; and
- link explanations to monitoring, logging, and audit procedures.

Accordingly, the research gap is not the absence of an individual classification method or XAI tool. It is the insufficient integration of predictive, temporal, interpretive, and governance requirements within one testable early-credit-warning system.

## 7. Testable Research Propositions

The following propositions are hypotheses to be tested empirically, rather than conclusions assumed in advance:

- **H1.** Non-linear ensemble models achieve no lower out-of-time discriminatory power than the logistic baseline, provided that they use the same information set and a valid temporal-validation design.
- **H2.** Explicit modelling of time to event or feature dynamics improves the usefulness of early warning compared with a static model at a fixed horizon.
- **H3.** A model with high discrimination does not necessarily have acceptable PD calibration; calibration requires separate assessment and, where necessary, post-processing.
- **H4.** The stability of local SHAP/LIME explanations declines when class imbalance and the composition of the time period change; it should therefore be evaluated separately from predictive performance.
- **H5.** A threshold calibrated to acceptable false alarms and a minimum lead time produces an early-warning signal that is more operationally useful than a threshold selected solely by accuracy or F1.

## 8. Claimed Scientific Novelty

The scientific novelty is formulated as the novelty of the **proposed integrated design** and is subject to confirmation after empirical results have been obtained:

1. **Operational separation of outcomes.** The study proposes to model formal default and latent deterioration in credit quality separately, fixing the identification rules before training and avoiding the conflation of borrower default, corporate distress, and systemic risk.
2. **A unified temporal evaluation framework.** The study proposes to combine PD estimation, time to event, lead time, and false alarms in one out-of-time procedure in which the temporal order of the data is a design constraint.
3. **Two-level explainability.** The framework combines a global analysis of risk drivers with a local explanation of each individual signal, plus a separate assessment of explanation stability across periods and under changes in class imbalance.
4. **Multi-criteria model selection.** Method selection is based jointly on discrimination, calibration, early-warning performance, false-alarm rate, and explanation stability, rather than on one aggregated measure.
5. **An auditable implementation architecture.** For each decision, the design records the data snapshot date, model version, set of available features, risk estimate, alert threshold, local explanation, and subsequent monitoring result. This provides a basis for reproducibility, drift control, and model governance.


