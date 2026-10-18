# Machine Learning Interview Practice

Try to answer without notes first. For scenario questions, state assumptions, evaluation design, risks, and the trade-offs behind your choice.

## Part A — Conceptual questions

1. Explain supervised, unsupervised, and reinforcement learning with examples.
2. What is the difference between a parameter and a hyperparameter?
3. Explain overfitting, underfitting, and the bias–variance trade-off.
4. Why do we need separate training, validation, and test sets?
5. What is cross-validation? When would ordinary random K-fold validation be inappropriate?
6. Explain data leakage and give three examples.
7. Compare linear regression and logistic regression.
8. Compare decision trees, random forests, and gradient boosting.
9. How does regularization work? Compare L1 and L2 regularization.
10. Explain precision, recall, F1, ROC-AUC, and PR-AUC.
11. Why can accuracy be misleading for an imbalanced dataset?
12. What is the difference between a decision threshold and a model's learned parameters?
13. How would you handle missing numerical and categorical values?
14. When should features be scaled? Which algorithms are sensitive to scale?
15. How do one-hot encoding and ordinal encoding differ?
16. What is feature engineering, and how would you validate that a new feature helps?
17. What is class imbalance? Compare class weighting, resampling, and threshold tuning.
18. What is calibration, and why can it matter when predicted probabilities drive decisions?
19. Explain feature importance. What are limitations of impurity-based importance?
20. What is the difference between model interpretability and causal inference?
21. Explain bagging and boosting.
22. What is a baseline model, and why is it useful?
23. How would you detect concept drift and data drift?
24. What should be monitored after a model is deployed?
25. How would you decide whether to retrain a model?

## Part B — Scenario questions

1. A fraud model has 99.5% accuracy but misses most fraud cases. Diagnose the issue and propose better metrics and actions.
2. A model performs well in training but poorly in production. Give a structured investigation plan.
3. A dataset contains multiple records per customer. How should you split it if the goal is generalization to unseen customers?
4. You predict next month's demand using historical records. Why can a random train/test split be misleading?
5. A feature has suspiciously high predictive power and is only known after the target event. What should you do?
6. A model's ROC-AUC improves but business conversion declines. List possible explanations.
7. A team wants to use a complex model when a simple linear model performs similarly. How would you make the decision?
8. A model has strong overall performance but poor recall for one important customer segment. What would you investigate?
9. The positive class is rare and the business has a fixed review capacity. How would you choose a threshold?
10. A new model improves offline metrics but increases inference latency substantially. What trade-offs and measurements matter?

## Part C — Coding exercises

1. Split a dataset into train and test sets using scikit-learn. Explain when to use `stratify`.
2. Build a preprocessing pipeline that imputes and scales numeric columns and imputes/one-hot encodes categorical columns.
3. Train a logistic regression classifier using a pipeline and evaluate precision, recall, F1, and ROC-AUC.
4. Train a regression baseline and report MAE and RMSE.
5. Use cross-validation to estimate performance without leaking preprocessing statistics.
6. Compare a decision tree and random forest on the same fixed split.
7. Find a threshold that achieves at least 90% recall on validation predictions, if one exists.
8. Calculate a confusion matrix and explain false positives and false negatives in the context of the task.
9. Inspect permutation importance and explain why correlated features can complicate interpretation.
10. Create a simple error-analysis table showing false positives and false negatives with their feature values.

## Part D — Debugging prompts

1. A scaler is fitted on the entire dataset before the split. Explain the problem and fix it.
2. A model is tuned repeatedly against the test set. Explain why the test score is no longer an unbiased final estimate.
3. A classifier reports 99% accuracy on a dataset where 99% of labels are negative. Explain a useful baseline and better metrics.
4. A time-series model uses future rolling averages to predict the past. Identify the leakage and propose a time-safe feature construction.
5. One-hot encoding is performed separately on train and test data. What can go wrong, and how can a pipeline help?

## Part E — Mock interview case

**Case: customer churn prediction**

Prepare a 5–10 minute explanation covering:
- Business objective and prediction unit
- Target definition and prediction horizon
- Data quality, leakage, and sampling risks
- Baseline model and validation strategy
- Candidate algorithms and metric choice
- Threshold selection and intervention capacity
- Error analysis and subgroup performance
- Deployment, monitoring, and retraining triggers

## Self-review checklist

- [ ] I can explain the intuition behind common algorithms.
- [ ] I can select a split strategy based on how predictions will be used.
- [ ] I can identify target leakage and preprocessing leakage.
- [ ] I can choose metrics based on class balance and business costs.
- [ ] I can implement preprocessing with a pipeline.
- [ ] I can explain the limits of feature importance.
- [ ] I can describe a production monitoring plan.
