# Machine Learning Interview Preparation

An interview-focused module for candidates preparing for data scientist, machine learning engineer, and applied ML roles. It combines ML fundamentals, model selection, evaluation, feature engineering, leakage prevention, interpretability, and practical Python exercises.

## Learning outcomes

By completing this module, you should be able to:

- Explain supervised, unsupervised, and reinforcement learning at a high level.
- Choose appropriate baselines, algorithms, metrics, and validation strategies.
- Explain bias–variance trade-offs, regularization, overfitting, and underfitting.
- Identify data leakage, class imbalance, distribution shift, and evaluation pitfalls.
- Build reproducible preprocessing and modeling pipelines.
- Interpret classification and regression metrics in the context of business costs.
- Explain feature importance, model interpretability, and error analysis.
- Discuss deployment monitoring and retraining triggers.
- Solve common ML coding and scenario questions in an interview.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Study roadmap, key concepts, and interview strategy |
| `examples.ipynb` | Hands-on examples with scikit-learn and synthetic datasets |
| `practice.md` | Theory questions, scenarios, and coding exercises |
| `solution.md` | Worked answers, implementation patterns, and explanations |

## Suggested 7-day study plan

### Day 1 — Fundamentals
Supervised vs unsupervised learning, train/validation/test splits, baselines, generalization, bias–variance, and overfitting.

### Day 2 — Algorithms
Linear and logistic regression, decision trees, random forests, gradient boosting, k-nearest neighbors, SVMs, and clustering.

### Day 3 — Evaluation
Confusion matrix, precision, recall, F1, ROC-AUC, PR-AUC, calibration, regression metrics, and threshold selection.

### Day 4 — Data preparation
Missing values, categorical encoding, scaling, feature engineering, class imbalance, and scikit-learn pipelines.

### Day 5 — Validation and tuning
Cross-validation, stratification, grouped/time-based splits, hyperparameter tuning, and leakage prevention.

### Day 6 — Interpretability and production
Permutation importance, error analysis, drift, monitoring, reproducibility, and retraining criteria.

### Day 7 — Mock interview
Answer conceptual questions aloud, solve two coding exercises, and walk through an end-to-end ML case.

## Core concepts checklist

- **Baseline first:** establish a simple, meaningful baseline before complex models.
- **Split to match deployment:** random split for suitable IID data; group split for repeated entities; time-based split for temporal prediction.
- **Fit transforms on training data only:** use pipelines to prevent preprocessing leakage.
- **Choose metrics from the problem:** consider class imbalance, ranking, calibration, and business costs.
- **Tune on validation data:** keep the final test set isolated until final evaluation.
- **Inspect errors:** aggregate metrics can hide failure on important subgroups.
- **Prefer reproducibility:** record data versions, features, code, parameters, and random seeds where applicable.
- **Monitor after deployment:** data quality, feature drift, prediction distribution, latency, and delayed labels/performance.

## Metric selection quick guide

| Problem | Useful metrics | Important caveat |
|---|---|---|
| Binary classification | Precision, recall, F1, ROC-AUC, PR-AUC, log loss, calibration | Accuracy can be misleading with imbalanced classes |
| Regression | MAE, RMSE, \(R^2\), residual plots | RMSE penalizes large errors more heavily than MAE |
| Ranking | Precision@k, Recall@k, NDCG@k, MAP | Evaluate at the ranking depth that matters |
| Clustering | Silhouette score, stability, domain validation | Internal scores do not prove clusters are useful |
| Probability estimates | Log loss, Brier score, calibration curve | Ranking quality and calibration are different |

## Interview answer framework

For an algorithm question:
1. Explain the intuition.
2. State the assumptions or conditions where it works well.
3. Discuss key hyperparameters.
4. Explain strengths, weaknesses, and alternatives.
5. Describe how you would validate it.

For a case study:
1. Clarify the business objective and prediction unit.
2. Define the target, prediction horizon, and success metric.
3. Identify leakage, sampling, and data-quality risks.
4. Establish a baseline and validation design.
5. Compare candidate models and inspect errors.
6. Explain deployment, monitoring, and retraining.

## Further reading

- [scikit-learn User Guide](https://scikit-learn.org/stable/user_guide.html)
- [scikit-learn model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [scikit-learn pipelines and composite estimators](https://scikit-learn.org/stable/modules/compose.html)
- [Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course)
