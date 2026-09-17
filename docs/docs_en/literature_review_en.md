# Literature Review
## Explainable Artificial Intelligence for Early Identification of Credit Risk and Latent Deterioration in Borrower Quality


---

## 1. Credit-risk problem formulation and unit of analysis

Credit scoring is fundamentally a problem of estimating and ranking borrower risk under incomplete information. Classical scorecards and logistic-regression models remain important because they offer a relatively transparent mapping from borrower characteristics to a risk estimate and provide a strong benchmark against which more complex models can be evaluated. Large benchmarking studies have also shown that methodological sophistication does not automatically imply dominant performance in all credit datasets: simple statistical classifiers can remain competitive with more complex methods, and model comparison must therefore be empirical rather than assumed a priori [51].

The unit of analysis is critical. Individual-borrower default, loan delinquency, corporate financial distress, bank default, and systemic financial risk are related but non-equivalent outcomes. They differ in the economic event being predicted, observation frequency, available covariates, class prevalence, action horizon, and error costs. Consequently, a model that performs well for listed-company distress or systemic-risk prediction cannot be treated as direct evidence of effectiveness for borrower-level early warning [9], [12], [14], [26], [34], [36], [46], [50]. The present study therefore fixes the unit of analysis as borrower \(i\) at observation time \(t\), with the objective of monitoring changes in that borrower's credit quality over time.

A second distinction concerns the outcome itself. **Formal default** is an observed adverse credit event defined by an institution, regulation, or documented portfolio policy. **Latent deterioration** is an earlier observable worsening in borrower quality that precedes, but is not identical to, formal default. **Early warning** refers not merely to predicting an adverse event, but to generating a sufficiently timely signal for intervention. These concepts should not be collapsed into a single binary label because a model may predict eventual default accurately while providing no operationally useful warning before the event.

Accordingly, the central problem is not simply whether a borrower will default. It is whether an observable deterioration can be detected sufficiently early, with calibrated risk estimates, an acceptable false-alert burden, and explanations that remain sufficiently stable to support monitoring and audit.

---

## 2. From statistical credit scoring to machine-learning ensembles

The shift from conventional statistical scoring toward machine learning has been motivated by the ability of non-linear methods to represent interactions, heterogeneous borrower profiles, and non-additive relationships. Random Forest provided a foundational ensemble formulation [1], while subsequent credit-risk studies have compared logistic regression with Random Forest, XGBoost, LightGBM and other ensemble architectures [4], [7], [15], [30], [37], [44]. However, Breiman's methodological contribution should not itself be interpreted as empirical evidence of superiority in credit portfolios: algorithmic capacity and portfolio-specific effectiveness are separate questions.

This distinction is supported by the broader benchmarking tradition. Baesens et al. compared multiple classifiers across eight real-life credit-scoring datasets and found that advanced classifiers could perform strongly while logistic regression and linear discriminant analysis remained competitive [51]. The implication is methodological: model complexity must be justified through a controlled comparison under the same information set, target definition, sampling scheme, and validation protocol. It is not sufficient to compare headline AUC or accuracy values taken from different studies.

More recent studies report advantages of ensemble or hybrid approaches in particular settings. Ariza-Garzón et al. use machine-learning models and SHAP in P2P lending [4]; Yang et al. compare logistic regression, Random Forest, XGBoost, and LightGBM in credit-default prediction [15]; Choi and Cha propose a LightGBM scorecard based on SHAP values [37]; and Lee et al. examine a CNN–Transformer architecture using real-world credit-transaction data [35]. These studies demonstrate that non-linear models can be useful in credit-risk prediction, but they do not establish a universal ranking of algorithms because their datasets, targets, prevalence rates, borrower populations, and validation designs differ.

The appropriate synthesis is therefore not that machine learning has replaced statistical scoring. Rather, the literature supports a **conditional trade-off**: flexible models may capture relationships missed by linear scorecards, while transparent baselines retain value for interpretation, benchmarking, calibration comparison, and governance. A credible dissertation design should therefore compare an interpretable baseline and more flexible models under an identical temporal information set and evaluation protocol.

---

## 3. Explainability in credit-risk models

As predictive models become more complex, interpretability and explainability become separate methodological requirements. LIME introduced a general local-surrogate framework for explaining individual predictions [2]. SHAP later formalised additive feature attribution using a unified framework based on Shapley values [54]. These approaches address different explanatory questions and should not be treated as interchangeable labels for "interpretability".

An **intrinsically interpretable model**, such as a conventional scorecard or suitably specified logistic regression, exposes a relatively direct model structure. A **post-hoc explanation** describes the behaviour of an already trained model. Within post-hoc explanation, **global explanation** concerns overall model behaviour or feature influence across a population, while **local explanation** concerns the contribution of features to one prediction. Neither SHAP nor LIME establishes that a feature causally affects default; they explain model behaviour under their respective assumptions [2], [54].

Credit-risk studies have increasingly adopted these methods. SHAP has been applied to P2P lending, SME risk, default prediction, scorecard construction, feature selection, network-augmented scoring, fairness, and complex architectures [4], [5], [7], [11], [15], [37], [42], [47], [48]. Bücker et al. explicitly frame transparency, auditability, and explainability as distinct requirements for machine-learning credit scoring and show why high predictive performance alone does not satisfy model-understanding requirements [55]. This is particularly relevant to financial institutions, where model outputs may need to be traced, reviewed, challenged, and documented.

The key limitation of the common "model + SHAP" pattern is that an explanation generated at one point in time is often treated as if it were a stable property of the borrower or the economic mechanism. That inference is not justified. Feature attributions are conditional on the trained model, background distribution, feature dependence structure, and data composition. The relevant question for monitoring is therefore not only whether an explanation exists, but whether the explanation is sufficiently stable when the model is applied across periods and changing portfolio conditions.

---

## 4. Class imbalance and explanation stability

Credit default is commonly a minority event, making class imbalance both a predictive and an interpretive problem. Resampling, class weighting, threshold optimisation, and feature selection can alter the fitted decision boundary. They can therefore affect not only recall or precision but also the explanations attached to predictions.

Chen, Calabrese, and Martin-Barragan provide direct evidence that class imbalance can affect the stability of SHAP and LIME interpretations in credit scoring [11]. This finding changes the role of XAI in model evaluation: explanation quality should not be assessed solely by visual plausibility. Instead, explanation stability becomes an empirical property that can be tested across resamples, periods, and model refits. Studies of oversampling [17], adaptive imbalance-aware ensembles [33], dynamic financial-distress prediction under severe imbalance [34], and SHAP-based feature selection [47] reinforce the broader point that data treatment and explanation cannot be analysed independently.

For rare events, accuracy is particularly weak as a primary performance measure. ROC-AUC provides a ranking measure but can remain optimistic when the negative class dominates. PR-AUC, recall, precision, false-positive burden, and threshold-dependent measures provide additional information. However, even these discrimination-oriented measures do not establish that estimated probabilities are calibrated or that an alert occurs early enough to be operationally useful.

This leads to a first substantive gap in the literature: **predictive robustness and explanatory robustness are frequently evaluated as separate dimensions**. The systematic review by Bahadori and Hassanzadeh identifies persistent limitations in XAI stability and practical actionability, alongside limited large-scale treatment of adaptive systems and concept drift [18]. This corpus-level evidence supports treating explanation stability under changing data conditions as a research problem rather than as a presentation feature.

---

## 5. Dynamic credit risk and survival modelling

Credit risk is inherently temporal. A static classifier estimates an outcome over a fixed horizon, but it does not explicitly model the timing of that outcome or censoring. Survival analysis provides a natural framework for modelling time to event and has a longer history in credit scoring than recent machine-learning survival models suggest.

Stepanova and Thomas applied survival-analysis methods to personal-loan data and examined extensions of the Cox proportional-hazards model in credit scoring [52]. Bellotti and Crook later used survival analysis to model time to default on credit-card accounts and incorporated macroeconomic variables as time-varying covariates [53]. These studies establish that survival-based credit modelling predates modern gradient-boosting implementations and that time-varying risk information is a foundational rather than newly introduced concern.

Recent work extends this tradition with machine-learning methods. Xia et al. combine survival analysis and gradient boosting in SurvXGBoost and compare models in out-of-sample and out-of-time settings [6]. Bone-Winkel and Reichenbach connect survival modelling with explainable machine learning in P2P lending [10]. JointLIME extends local interpretation to black-box survival models with endogenous time-varying covariates [16]. Together, these studies demonstrate that temporal risk modelling and explanation can be combined, but they do not remove the need to evaluate whether explanations remain stable across time or whether an earlier signal is operationally useful.

Survival models also introduce design requirements that a fixed-horizon classifier may avoid. A study must define the time origin, event, censoring mechanism, treatment of early repayment or account closure, handling of repeated loans, and whether the unit is a borrower or an exposure. Therefore, survival modelling should be selected because the observed event process and portfolio history support it, not because it is intrinsically more advanced.

---

## 6. Early warning and latent deterioration before default

Early-warning research should distinguish three questions:

1. **Will an adverse event occur within a horizon?**
2. **When is the event likely to occur?**
3. **Can a preceding deterioration be identified early enough to support intervention?**

These are not equivalent tasks. Financial-distress forewarning [9], dynamic distress prediction [34], industry-risk monitoring [36], profitability deterioration [50], and borrower-level default prediction [4], [6], [10], [15] use related methods but different targets. Evidence from corporate distress or systemic risk may inform methodology, yet it cannot substitute for borrower-level validation.

For the present study, latent deterioration should be defined **independently of the candidate model being evaluated**. A deterioration label must therefore be based on a pre-specified observable transition, such as movement into a documented delinquency state, a worsening external or pre-existing internal risk grade, or another portfolio-defined adverse transition. A rise in the candidate model's own predicted PD must not be used to define the target that the same model is trained to predict, because that would introduce circularity.

This distinction provides a stronger basis for early-warning evaluation. The relevant outcome is not simply whether the final adverse event is classified correctly. A useful system must also measure:

- whether an alert is generated before the event;
- the distribution of lead time;
- the share of events detected within an actionable horizon;
- the number or rate of false alerts;
- the persistence or reversibility of warnings; and
- whether the explanation for the signal is stable enough to support review.

The literature contains examples addressing parts of this problem. Wigantiyoko and Atok explicitly select an early-warning model with attention to recall of defaults [44]. Distress and forewarning studies emphasise predictive horizons [9], [34], [50], while survival studies formalise time to event [6], [10], [16]. The unresolved issue is therefore not the absence of algorithms for prediction. It is the **joint evaluation of warning timeliness, probability quality, false-alert burden, and explanation stability at the borrower level**.

---

## 7. Calibration and decision usefulness

Discrimination and calibration answer different questions. A model may rank risky borrowers correctly while producing probabilities that systematically overstate or understate observed event frequencies. For credit-risk monitoring, this distinction matters because a PD-like output may be used for risk segmentation, threshold setting, provisioning, escalation, or downstream decision rules.

ROC-AUC, PR-AUC, recall, precision, F1, and related metrics evaluate aspects of classification or ranking; they do not establish probabilistic calibration. Calibration curves, the Brier score, and calibration slope/intercept are therefore required when predictions are interpreted as probabilities. Recent credit-risk literature has explicitly noted that high predictive accuracy does not by itself imply reliable probability estimates [24].

Threshold selection should likewise be separated from model training. Maximising accuracy or F1 imposes an implicit trade-off that may not match the cost structure of an early-warning process. An operational threshold should reflect the cost of missed deterioration, the acceptable false-alert workload, and the minimum useful lead time. Accordingly, early-warning performance should be evaluated as a constrained decision problem rather than reduced to one global ranking metric.

This observation also limits claims of model superiority. A model with higher AUC may be inferior for a particular monitoring process if it is poorly calibrated, produces excessive false alerts at the required sensitivity, or generates warnings too late for intervention. Model selection should therefore be multi-criteria, but the criteria and decision rule must be stated explicitly rather than combined informally after observing results.

---

## 8. Temporal validation, drift, governance, and auditability

Temporal validation is essential when the intended use is ongoing portfolio monitoring. Random train/test splits can mix observations from different dates and may overstate performance when borrower behaviour, portfolio composition, macroeconomic conditions, or institutional policies change over time. Out-of-time evaluation reduces this risk and is explicitly used in dynamic credit-scoring work such as Xia et al. [6].

A rigorous temporal design should separate earlier training data, a subsequent validation period, and a final unused out-of-time test period. All preprocessing learned from data—including variable selection, scaling, oversampling, calibration, and hyperparameter tuning—must be fitted within the corresponding training window. Future-status indicators, post-decision variables, and aggregates using future observations must be excluded.

The intended generalisation target must also be stated. If the same borrower can appear in earlier and later periods, the test evaluates **future monitoring of an existing portfolio**. If the objective is generalisation to previously unseen borrowers, a borrower-disjoint design is also required. These are different evaluation questions and should not be conflated.

Governance adds further requirements. Bücker et al. emphasise transparency and auditability as model properties in credit scoring [55]. The European Banking Authority's Guidelines on Loan Origination and Monitoring require institutions using automated models in creditworthiness assessment and decision-making to address data quality, traceability, auditability, robustness, performance assessment, documentation, controls, overrides, and monitoring [56]. These requirements support the inclusion of model versioning, data-snapshot dates, feature availability, threshold records, explanations, and subsequent outcomes in an auditable monitoring architecture.

Governance, however, should not be presented as the scientific novelty of the dissertation by itself. Its role is to constrain the experimental design and ensure that the resulting model and explanations can be reproduced and reviewed.

---

## 9. Cross-study synthesis

The literature can be organised into five established streams.

**First, predictive modelling.** Statistical and machine-learning models provide alternative ways of estimating borrower risk. Ensemble methods often improve flexibility, but their relative performance depends on the dataset, target, class distribution, and validation design [4], [15], [35], [37], [44], [51].

**Second, temporal risk modelling.** Survival analysis and dynamic models explicitly incorporate time to event, censoring, or time-varying information [6], [10], [16], [52], [53]. These approaches are more directly aligned with changing borrower risk than a purely static score, but temporal modelling does not automatically provide an operational early-warning rule.

**Third, explainability.** LIME, SHAP, and related frameworks provide local or global descriptions of model behaviour [2], [4], [5], [7], [54]. In credit scoring, explanation is increasingly linked to transparency and auditability [55]. However, the existence of a local explanation does not establish that the explanation is stable across periods or robust to class imbalance.

**Fourth, evaluation under imbalance and distribution change.** Class imbalance affects both predictive metrics and explanation stability [11], while adaptive and imbalance-aware studies address changing data conditions [17], [33], [34], [47]. Recent systematic reviews identify concept drift, XAI stability, and operationalisation as continuing challenges [18], [38].

**Fifth, governance and deployment.** Recent literature and regulatory guidance extend model evaluation beyond predictive accuracy toward monitoring, documentation, fairness, traceability, and controls [18], [38], [40], [55], [56].

The central synthesis is that these streams answer complementary questions but are often evaluated with different experimental objectives. The strongest evidence for a remaining gap does not come from the claim that "no study combines every component". Instead, it arises from the unresolved relationship between them: **a model that remains discriminative over time may still become poorly calibrated, issue warnings too late, produce too many false alerts, or generate unstable local explanations**. Current systematic reviews explicitly report unresolved issues around XAI stability, adaptive systems, concept drift, operational deployment, and governance [18], [38]. Dynamic and survival studies show how time can be modelled [6], [10], [16], [52], [53], while explanation-stability research shows that explanations can change under imbalance [11]. What remains insufficiently established is how these properties interact in one borrower-level out-of-time early-warning setting.

---

---

## 10. Conclusions from the Literature Review

The reviewed literature shows that contemporary credit-risk research develops along several interconnected directions: predictive modelling, temporal risk modelling, explainable artificial intelligence, treatment of class imbalance, and increasingly strict requirements for model validation, monitoring, and auditability.

The evidence indicates that ensemble and other non-linear models can improve the ability to capture complex relationships in credit data, but their advantage is not universal and depends on the dataset, target definition, borrower population, class distribution, and validation design [4], [15], [35], [37], [44], [51]. Logistic regression and other interpretable approaches therefore remain important as baseline models for comparing discrimination, calibration, and interpretability.

A separate stream of research focuses on dynamic and survival-based approaches that explicitly model time to event, censoring, and changes in risk factors over time [6], [10], [16], [52], [53]. These methods are particularly relevant to early-warning applications because they make it possible to study not only whether an adverse event may occur, but also the temporal structure of deterioration preceding that event.

The XAI literature demonstrates extensive use of SHAP and LIME for explaining complex credit-risk models [2], [4], [5], [7], [11], [15], [37], [42], [54]. At the same time, the existence of an explanation does not guarantee its stability. The reviewed studies indicate that class imbalance, changes in sample composition, and model-training procedures may affect the stability of local explanations [11], [17], [33], [34], [47]. This issue is particularly important in ongoing credit-portfolio monitoring, where models are applied to data whose distribution may change over time.

The literature also shows that high discriminatory performance is not equivalent to reliable probability estimation and does not by itself establish the practical usefulness of an early-warning system. Credit-risk monitoring therefore requires attention not only to ROC-AUC or F1, but also to PR-AUC, recall, precision, Brier score, calibration characteristics, false-alert burden, and the time interval between an alert and the observed adverse event [6], [11], [24], [44].

Overall, the literature supports a multi-dimensional approach to credit-risk model evaluation. Predictive performance, temporal robustness, probability calibration, warning timeliness, and explanation stability should be considered jointly rather than treated as interchangeable indicators of model quality. For the subsequent empirical study, it is therefore essential to define the unit of analysis and target event precisely, exclude future-information leakage, apply out-of-time validation, and distinguish clearly between default prediction, early identification of borrower deterioration, and explanation of model behaviour.

# Bibliography

[1] Breiman, L. (2001). *Random Forests*. *Machine Learning*, 45(1), 5–32. https://doi.org/10.1023/A:1010933404324.

[2] Ribeiro, M. T., Singh, S., & Guestrin, C. (2016). *“Why Should I Trust You?”: Explaining the Predictions of Any Classifier*. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 1135–1144. https://doi.org/10.1145/2939672.2939778.

[3] Giudici, P., Hadji-Misheva, B., & Spelta, A. (2019). *Network based credit risk models*. *Quality Engineering*. https://doi.org/10.1080/08982112.2019.1655159.

[4] Ariza-Garzón, M., Arroyo, J., Caparrini, A., & Segovia-Vargas, M. (2020). *Explainability of a Machine Learning Granting Scoring Model in Peer-to-Peer Lending*. *IEEE Access*. https://doi.org/10.1109/ACCESS.2020.2984412.

[5] Bussmann, N., Giudici, P., Marinelli, D., & Papenbrock, J. (2020). *Explainable AI in Fintech Risk Management*. *Frontiers in Artificial Intelligence*. https://doi.org/10.3389/frai.2020.00026.

[6] Xia, Y., He, L., Li, Y., Fu, Y., & Xu, Y. (2021). *A dynamic credit scoring model based on survival gradient boosting decision tree approach*. *Technological and Economic Development of Economy*. https://doi.org/10.3846/tede.2020.13997.

[7] Bussmann, N., Giudici, P., Marinelli, D., & Papenbrock, J. (2021). *Explainable Machine Learning in Credit Risk Management*. *Computational Economics*. https://doi.org/10.1007/s10614-020-10042-0.

[8] Sowmiya, M. N., Jaya Sri, S., Deepshika, S., & Hanushya Devi, G. (2024). *Credit Risk Analysis Using Explainable Artificial Intelligence*. *Journal of Soft Computing Paradigm*. https://doi.org/10.36548/jscp.2024.3.004.

[9] Deng, S., Luo, Q., Zhu, Y., Ning, H., & Shimada, T. (2024). *Financial risk forewarning with an interpretable ensemble learning approach: An empirical analysis based on Chinese listed companies*. *Pacific-Basin Finance Journal*. https://doi.org/10.1016/j.pacfin.2024.102393.

[10] Bone-Winkel, G. F., & Reichenbach, F. (2024). *Improving credit risk assessment in P2P lending with explainable machine learning survival analysis*. *Digital Finance*. https://doi.org/10.1007/s42521-024-00114-3.

[11] Chen, Y., Calabrese, R., & Martin-Barragan, B. (2024). *Interpretable machine learning for imbalanced credit scoring datasets*. *European Journal of Operational Research*. https://doi.org/10.1016/j.ejor.2023.06.036.

[12] Tang, P., Tang, T., & Lu, C. (2024). *Predicting systemic financial risk with interpretable machine learning*. *The North American Journal of Economics and Finance*. https://doi.org/10.1016/j.najef.2024.102088.

[13] Yu, D., & Xiang, B. (2025). *Customized integrated decision model for CBEC enterprise credit evaluation: The fusion of multi-source features and machine learning*. *Electronic Markets*. https://doi.org/10.1007/s12525-025-00793-9.

[14] Tang, P., Peng, H., Luo, S., & Liu, Y. (2025). *Forecasting Bank Default Risk with Interpretable Machine Learning: The Study of Chinese Banks*. *Emerging Markets Finance and Trade*. https://doi.org/10.1080/1540496X.2024.2415337.

[15] Yang, S., Huang, Z., Xiao, W., & Shen, X. (2025). *Interpretable Credit Default Prediction with Ensemble Learning and SHAP*. *2025 International Conference on Artificial Intelligence, Human-Computer Interaction and Natural Language Processing (ICAHN)*. https://doi.org/10.1109/icahn67688.2025.00027.

[16] Chen, Y., Calabrese, R., & Martin-Barragan, B. (2025). *JointLIME: An interpretation method for machine learning survival models with endogenous time-varying covariates in credit scoring*. *Risk Analysis*. https://doi.org/10.1111/risa.17679.

[17] Tang, X., Yu, T., Chen, X., & Lu, H. (2026). *A boundary-transcending synthetic oversampling algorithm based on vector decomposition for imbalanced credit scoring*. *Omega*. https://doi.org/10.1016/j.omega.2026.103626.

[18] Bahadori, S., & Hassanzadeh, A. (2026). *A comprehensive literature review on AI-based credit assessment in digital lending: Exploring models, transparency, alternative data, and real-time learning*. *Expert Systems with Applications*. https://doi.org/10.1016/j.eswa.2026.131872.

[19] Wang, Z., Li, Y., Cui, Z., Zheng, W., & Wang, T. (2026). *A machine learning-based study of credit risk in supply chain finance of listed service-oriented enterprises in China*. *Pacific-Basin Finance Journal*, 96, 103043. https://doi.org/10.1016/j.pacfin.2025.103043.

[20] NoorMohammadzadehMaleki, D., Baghaei Oskouei, M., Taheri, A., Farhadi, A., & Zamanifar, A. (2026). *A structurally sparse and robust XAI framework*. *AI and Ethics*. https://doi.org/10.1007/s43681-026-01322-w.

[21] Kannan, J., & Pruseth, S. (2026). *An Explainable Machine Learning Framework for Dynamic Credit Portfolio Risk Assessment and Default Prediction Using Random Forest Optimization*. *International Journal For Multidisciplinary Research*. https://doi.org/10.36948/ijfmr.2026.v08i02.73602.

[22] Ginting, V. S., Hizria, R., & Takhir, S. (2026). *Analysis Of The Determinants Of Financial Resilience Among Retiree Customers Using An Explainable AI (XAI) Approach*. *Journal of Computer Networks, Architecture and High Performance Computing*. https://doi.org/10.47709/cnahpc.v8i3.9459.

[23] Azima, A., & Subramanian, P. (2026). *Application of Explainable AI (XAI) in Credit Risk Assessment: Current Challenges and Further Research Direction*. *2026 Innovations in Machine, Engineering, and Digital Conference (IMED)*. https://doi.org/10.1109/IMED68921.2026.11484450.

[24] Rambauli, M., Ravele, T., & Sigauke, C. (2026). *Application of Explainable AI and Uncertainty Quantification in Credit Risk Assessment*. *Risks*. https://doi.org/10.3390/risks14070152.

[25] Liu, J., Zhang, X., & Xiong, H. (2024). *Credit risk prediction based on causal machine learning: Bayesian network learning, default inference, and interpretation*. *Journal of Forecasting*, 43(5), 1625–1660. https://doi.org/10.1002/for.3080.

[26] Wang, P., & Borjigin, S. (2026). *Bank systemic risk prediction based on text mining and explainable machine learning*. *The North American Journal of Economics and Finance*. https://doi.org/10.1016/j.najef.2025.102577.

[27] Baraniuk, M., & Hrabariev, A. (2026). *Credit Scoring Models in Bank Credit Risk Management: Comparative Assessment and Application in Ukrainian Banking Practice*. *Baltic Journal of Economic Studies*. https://doi.org/10.30525/2256-0742/2026-12-3-299-311.

[28] Krishnamoorthy, S. (2026). *Complexity-Budgeted, Interaction-Aware Interpretable Model for Tabular Data*. *arXiv*. https://doi.org/10.48550/arXiv.2607.07060.

[29] Duricova, L., & Labosova, V. (2026). *Corporate Loan Default Prediction in the Slovak Banking Context: An Interpretable and Ensemble CRISP-DM Pipeline for Credit Risk Assessment*. *Systems*. https://doi.org/10.3390/systems14070738.

[30] Musiige, A. J., Naing, M. H., & Wattanapongsakorn, N. (2026). *Credit Risk Assessment through Multiple Machine Learning Models and SHAP-based Interpretability*. *International Joint Conference on Computer Science and Software Engineering*. https://doi.org/10.1109/JCSSE68839.2026.11597053.

[31] Ireddy, R. (2026). *Explainable Machine Learning Frameworks for Robust Credit Risk Assessment and Regulatory Compliance in Digital Banking*. *2026 2nd International Conference on Sustainable Computing and Integrated Communication in Changing Landscape of AI (ICSCAI)*. https://doi.org/10.1109/icscai68849.2026.11648766.

[32] Sahani, S. K., Lee, T.-F., Pandey, D., Pandey, B., Jha, R., & Labh, S. (2026). *Explainable Machine Learning for Credit Risk Management and Intelligent Lending Decisions in Nepalese Cooperative Banks: A Mathematical Review*. *Journal of Intelligent Decision Making and Information Science*. https://doi.org/10.59543/jidmis.v3.1549.

[33] Liu, W., Zou, Y., Liu, B., Tao, J., Lan, X., & Xia, M. (2026). *Explainable adaptive ensemble learning with imbalance mitigation for manufacturing sector financial risk warning*. *Chaos, Solitons & Fractals*, 202, 117577. https://doi.org/10.1016/j.chaos.2025.117577.

[34] Zhao, M., Sun, J., Li, N., & Yang, X.-Y. (2026). *Highly class-imbalanced dynamic financial distress prediction based on Bagging-XGBoost*. *Risk Management*. https://doi.org/10.1057/s41283-026-00242-7.

[35] Lee, J., Lee, Y., Hong, J., Chung, Y., & Kim, E. (2026). *Hybrid CNN-transformer architecture for personal credit risk prediction with comparative insights into model explainability*. *Journal of Supercomputing*. https://doi.org/10.1007/s11227-026-08486-6.

[36] Xu, Y. (2026). *Industry-Specific credit risk identification for bank lending risk monitoring: An explainable machine learning approach based on Chinese listed firms*. *International Journal of Applied Economics, Finance and Accounting*. https://doi.org/10.33094/ijaefa.v23i2.2459.

[37] Choi, Y., & Cha, E. (2026). *LightGBM Scorecard based on SHAP values*. *Computational Economics*. https://doi.org/10.1007/s10614-025-11194-7.

[38] Zhang, B., Luo, J., Wu, R., Wei, J., Luo, Z., & Shen, H. (2026). *Machine Learning for Individual Credit Risk Assessment: A Systematic Literature Review of State-of-the-Art Methods, Challenges and Perspectives*. *Journal of Risk and Financial Management*. https://doi.org/10.3390/jrfm19080607.

[39] Mondal, A., Mahesh Kumar, K. R., Shyam Sundar, I., Pattanaik, S., Rohini, P., & Jayaram, B. (2026). *Machine Learning—Based Credit Risk Assessment Model for Commercial Financial Institutions*. *2026 2nd International Conference on Sustainable Computing and Integrated Communication in Changing Landscape of AI (ICSCAI)*. https://doi.org/10.1109/icscai68849.2026.11649056.

[40] Abugri, C., Pallapati, J., & Nketiah, T. (2026). *Model Risk Management and Validation Frameworks for Machine Learning Models in Banking*. *International Journal For Multidisciplinary Research*. https://doi.org/10.36948/ijfmr.2026.v08i01.66703.

[41] Ngwenya, M. (2026). *Modeling Bias and Transparency in Credit Scoring Algorithms: A Differentially Private SHAP-Based Framework*. *2026 International Conference on Artificial Intelligence, Computer, Data Sciences and Applications (ACDSA)*. https://doi.org/10.1109/ACDSA67686.2026.11467840.

[42] Hegde, A., & Bhowmik, B. (2026). *Network Centrality Feature Augmentation and SHAP Analysis of Inlier and Outlier Borrowers in Credit Scoring*. *Computational Economics*. https://doi.org/10.1007/s10614-025-11292-6.

[43] Salvad’e, N., & Hillel, T. (2026). *ParamBoost: Gradient Boosted Piecewise Cubic Polynomials*. *arXiv*. https://doi.org/10.48550/arXiv.2604.18864.

[44] Wigantiyoko, G. P., & Atok, R. M. (2026). *Perbandingan Prediksi Loan Default Menggunakan Pemodelan Algoritma Random Forest dan XGBoost untuk Mitigasi Risiko Kredit Bank*. *Jurnal Teknologi dan Manajemen Industri Terapan*. https://doi.org/10.55826/jtmit.v5i3.2101.

[45] Ariza-Garzón, M.-J., Arroyo, J., Segovia-Vargas, M.-J., & Caparrini, A. (2024). *Profit-sensitive machine learning classification with explanations in credit risk: The case of small businesses in peer-to-peer lending*. *Electronic Commerce Research and Applications*, 67, 101428. https://doi.org/10.1016/j.elerap.2024.101428.

[46] Sun, J., Yang, Y., Chen, S., Zhang, B., Li, R., & Dai, L. (2026). *Recognizing distressed finances of China A-share listed companies: Insights from interpretable machine learning approach*. *International Journal of Accounting Information Systems*. https://doi.org/10.1016/j.accinf.2026.100785.

[47] Gonlachanvit, P., Senjuntichai, A., & Senjuntichai, T. (2026). *SHAP-McNemar Stepwise Feature Selection for Machine Learning in Credit Risk Modeling*. *Machine Learning with Applications*. https://doi.org/10.1016/j.mlwa.2026.100977.

[48] Adegun, A., Li, W., & Fawale, S. (2026). *Structural Causal Model and Explainable AI for Detecting Bias and Fairness in Automated Credit Decisions System*. *IEEE International Conference on Engineering of Complex Computer Systems*. https://doi.org/10.1109/ICECCS70432.2026.11651164.

[49] Navya, S. D., Sreekanth, D., & Sankari, S. (2026). *Structural Gender Bias in Credit Scoring: Proxy Leakage*. *arXiv*. https://doi.org/10.48550/arXiv.2601.18342.

[50] Khezri, M. G., & Giouvris, E. (2026). *The Conditional Role of Corporate Governance and Explainable Machine Learning in Predicting Severe Profitability Deterioration: Evidence from the S&P 500—An Early Warning System*. *Risks*. https://doi.org/10.3390/risks14090200.

[51] Baesens, B., Van Gestel, T., Viaene, S., Stepanova, M., Suykens, J., & Vanthienen, J. (2003). *Benchmarking state-of-the-art classification algorithms for credit scoring*. *Journal of the Operational Research Society*, 54(6), 627–635. https://doi.org/10.1057/palgrave.jors.2601545.

[52] Stepanova, M., & Thomas, L. C. (2002). *Survival Analysis Methods for Personal Loan Data*. *Operations Research*, 50(2), 277–289. https://doi.org/10.1287/opre.50.2.277.426.

[53] Bellotti, T., & Crook, J. (2009). *Credit scoring with macroeconomic variables using survival analysis*. *Journal of the Operational Research Society*, 60(12), 1699–1707. https://doi.org/10.1057/jors.2008.130.

[54] Lundberg, S. M., & Lee, S.-I. (2017). *A Unified Approach to Interpreting Model Predictions*. *Advances in Neural Information Processing Systems*, 30, 4765–4774.

[55] Bücker, M., Szepannek, G., Gosiewska, A., & Biecek, P. (2022). *Transparency, auditability, and explainability of machine learning models in credit scoring*. *Journal of the Operational Research Society*, 73(1), 70–90. https://doi.org/10.1080/01605682.2021.1922098.

[56] European Banking Authority. (2020). *Guidelines on loan origination and monitoring (EBA/GL/2020/06)*. Applicable from 30 June 2021.
