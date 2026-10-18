# Machine Learning Interview Practice — Solutions

These are reference answers. Good interview responses should adapt the method to the data, business objective, and deployment setting rather than relying on a universal recipe.

## Part A — Conceptual answer guide

1. **Supervised learning** learns from labeled examples (classification/regression); **unsupervised learning** finds structure without a target (clustering/dimensionality reduction); **reinforcement learning** learns actions from rewards and interaction.
2. Parameters are learned from training data (for example, regression coefficients); hyperparameters configure the learning process (for example, tree depth or regularization strength).
3. Underfitting is too-simple modeling and poor performance on both training and validation data. Overfitting is learning noise or idiosyncrasies, producing a generalization gap. Bias–variance describes different sources of prediction error and motivates appropriate model complexity and validation.
4. Training data fits model parameters; validation data supports selection and tuning; test data estimates final generalization after decisions are complete. Repeatedly using the test set leaks information into model selection.
5. K-fold validation trains on subsets and evaluates on held-out folds. Random K-fold can be inappropriate for time series, grouped entities, repeated measures, or other dependent data; use time-aware or group-aware splitting.
6. Leakage occurs when training uses information unavailable at prediction time or information from validation/test data. Examples: a post-outcome field, fitting a scaler before splitting, and randomly splitting repeated users across train and test when evaluating unseen-user generalization.
7. Linear regression predicts a numeric outcome; logistic regression models class probabilities through a logistic link for classification. Both can provide interpretable coefficients, but their target distributions and output interpretation differ.
8. A decision tree partitions features and is easy to explain but can be unstable. Random forests average many randomized trees, usually reducing variance. Gradient boosting builds learners sequentially to correct residual errors and can be highly effective but needs careful tuning.
9. Regularization penalizes model complexity. L1 can drive some coefficients to zero; L2 shrinks coefficients smoothly. The best choice depends on features, scale, objective, and model.
10. Precision is the fraction of predicted positives that are true positives; recall is the fraction of actual positives found; F1 is their harmonic mean. ROC-AUC measures ranking across thresholds; PR-AUC focuses on precision/recall and is often informative with rare positives. Metrics should be interpreted with prevalence and costs in mind.
11. A majority-class classifier may achieve high accuracy while failing on the minority class. Compare against a baseline and inspect confusion matrix, recall, precision, PR-AUC, and business-weighted costs.
12. Parameters define the fitted model. A threshold maps a score/probability to a class decision and can often be changed without retraining the model.
13. Fit imputation on training data only. Numeric choices include median/mean or model-based methods; categorical choices include most frequent or an explicit missing category. Missingness itself can carry information, so investigate why values are missing.
14. Scaling matters for distance- and gradient-based methods such as k-NN, SVMs, and many linear models with regularization. Tree-based models are generally much less sensitive to monotonic feature scaling.
15. One-hot encoding represents categories with indicator columns and avoids imposing an order. Ordinal encoding assigns integer order and is appropriate only when a meaningful order exists or the model can handle the encoding appropriately.
16. Feature engineering constructs useful predictors from raw data. Validate it with a leakage-safe pipeline and an appropriate held-out or cross-validation comparison; check stability and operational availability.
17. Options include class weights, over/under-sampling within training folds, collecting more positive examples, and threshold tuning. Never resample before splitting in a way that contaminates evaluation. Choose based on data volume and error costs.
18. Calibration asks whether predictions assigned probability \(p\) occur about \(p\) of the time. It matters for risk, pricing, resource allocation, and expected-value decisions. A model can rank well yet be poorly calibrated.
19. Permutation importance measures performance degradation after shuffling a feature. Impurity-based importance can favor high-cardinality features and may distribute importance unpredictably among correlated predictors. Importance is not proof of causality.
20. Interpretability explains how a model makes predictions. Causal inference estimates effects of interventions under design and assumptions; explaining a prediction does not establish a causal effect.
21. Bagging trains learners independently on resampled data and aggregates them, often reducing variance. Boosting adds learners sequentially to improve errors according to its objective.
22. A baseline provides a simple reference score and checks whether the task, data, and metric are sensible. It helps quantify the value of added complexity.
23. Data drift is a change in input distribution; concept drift is a change in the relationship between inputs and outcomes. Monitor input/score distributions and, when labels arrive, performance and calibration over time.
24. Monitor input schemas, missingness, distributions, prediction rates, latency, errors, resource use, subgroup metrics, and delayed-label performance. Alert thresholds should be tied to operational impact.
25. Retrain when evidence shows performance or data quality has degraded, the data-generating process changed, or scheduled refresh is justified. Compare against the incumbent on leakage-safe data and keep rollback options.

## Part B — Scenario answer guide

1. The majority class dominates accuracy. Inspect confusion matrix, fraud recall, precision, PR-AUC, and cost-weighted outcomes. Verify labels and split design, then consider class weights/resampling in training, threshold tuning, and human-review capacity.
2. Check schema and feature availability, data quality, training-serving skew, drift, label delay, leakage, split realism, changes in user behavior, and latency. Compare input and prediction distributions, then evaluate labeled production outcomes when available.
3. Use group-aware splitting so each customer appears in only one partition, such as `GroupShuffleSplit` or `GroupKFold`, if the goal is generalization to unseen customers.
4. Random splitting can train on later time periods and test on earlier ones, allowing future patterns to leak. Use chronological splits or rolling-origin validation.
5. Exclude the feature if it is unavailable at prediction time, redefine the prediction timestamp, and audit all features for post-outcome information.
6. The threshold may have changed, calibration or prevalence may have shifted, the operating metric may differ from ROC-AUC, false positives may have increased, or product factors may dominate. Inspect threshold-specific metrics and end-to-end outcomes.
7. Compare incremental lift, uncertainty, latency, maintainability, interpretability, robustness, fairness, and operational cost. Prefer the simpler model if the added complexity does not justify itself.
8. Check data quality and sample size for the segment, label definitions, distribution shift, error types, threshold effects, and whether the metric is stable. Avoid drawing strong conclusions from tiny subgroups.
9. Rank cases by score and select a threshold or top-k policy matching capacity. Estimate precision/recall at that capacity and consider the cost of missed positives and review workload.
10. Measure latency percentiles, throughput, memory, infrastructure cost, failure rate, and business impact. Consider distillation, caching, batching, or a simpler model if the offline gain is not worth the operational penalty.

## Part C — Coding reference patterns

### 1. Train/test split and stratification
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42,
    stratify=y  # useful for classification when preserving class proportions is appropriate
)
```
For time-dependent or grouped data, use a split that respects time or groups rather than a random split.

### 2. Preprocessing pipeline
```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "income"]
categorical_features = ["region", "plan"]

numeric_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore")),
])

preprocessor = ColumnTransformer([
    ("numeric", numeric_pipeline, numeric_features),
    ("categorical", categorical_pipeline, categorical_features),
])

model = Pipeline([
    ("preprocess", preprocessor),
    ("classifier", LogisticRegression(max_iter=1000, class_weight=None)),
])
```

### 3. Classification evaluation
```python
from sklearn.metrics import (
    classification_report, roc_auc_score, average_precision_score
)

model.fit(X_train, y_train)
pred = model.predict(X_test)
prob = model.predict_proba(X_test)[:, 1]

print(classification_report(y_test, pred, zero_division=0))
print("ROC-AUC:", roc_auc_score(y_test, prob))
print("PR-AUC:", average_precision_score(y_test, prob))
```
Use `predict_proba` only when the estimator supports it. Do not report ROC-AUC or PR-AUC without both classes in the evaluation sample.

### 4. Regression evaluation
```python
from sklearn.dummy import DummyRegressor
from sklearn.metrics import mean_absolute_error, mean_squared_error
from math import sqrt

baseline = DummyRegressor(strategy="median")
baseline.fit(X_train, y_train)
pred = baseline.predict(X_test)

print("MAE:", mean_absolute_error(y_test, pred))
print("RMSE:", sqrt(mean_squared_error(y_test, pred)))
```

### 5. Cross-validation with a pipeline
```python
from sklearn.model_selection import StratifiedKFold, cross_val_score

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=cv, scoring="average_precision")
print("Fold scores:", scores)
print("Mean PR-AUC:", scores.mean())
```
For grouped or temporal data, replace this splitter with a group-aware or time-aware strategy.

### 6. Threshold selection for recall
```python
import numpy as np
from sklearn.metrics import recall_score

prob = model.predict_proba(X_valid)[:, 1]
thresholds = np.linspace(0.01, 0.99, 99)
eligible = []
for threshold in thresholds:
    pred = (prob >= threshold).astype(int)
    recall = recall_score(y_valid, pred, zero_division=0)
    if recall >= 0.90:
        eligible.append((threshold, recall, pred.mean()))

# Example policy: among thresholds meeting recall target, choose the highest
# threshold to reduce predicted positives. Confirm precision/capacity as well.
choice = max(eligible, key=lambda item: item[0]) if eligible else None
print(choice)
```
Threshold selection belongs on validation data, not on the final test set.

## Part D — Debugging solutions

1. **Scaler before split:** test-set distribution influences training transformation. Split first and fit transforms only within a training pipeline.
2. **Tuning on test repeatedly:** decisions adapt to test-set quirks. Keep a final untouched test set or use nested validation when model-selection bias is important.
3. **99% majority baseline:** predict all negatives and report accuracy, but also examine recall, precision, PR-AUC, and confusion matrix. Select metrics based on the actual cost of each error.
4. **Future rolling features:** they contain information unavailable at prediction time. Construct lagged and rolling features using only values available before each prediction timestamp; validate chronologically.
5. **Separate one-hot encoding:** train and test may have different columns or ordering. Fit one encoder on training data and transform both partitions through the same pipeline, with unknown categories handled deliberately.

## Suggested answer-quality rubric

Score each category from 1–5:
- Conceptual correctness
- Validation and leakage awareness
- Metric choice and business reasoning
- Implementation and edge-case handling
- Communication of trade-offs and limitations

A strong answer explains not only what model to use, but how to prove it generalizes and how to detect when it stops working.
