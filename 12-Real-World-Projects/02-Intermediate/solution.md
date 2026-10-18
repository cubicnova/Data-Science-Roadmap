# Worked Solutions — Intermediate Real-World Projects

These are reference answers. Use them to review your reasoning, not as a substitute for defining the problem and checking assumptions in your own dataset.

## Part A — Business framing and data audit

### 1. Business brief
Example: “Each week, the retention team needs a ranked list of active subscribers who may cancel in the next 30 days, so it can offer relevant support or retention options. Success will be assessed by incremental retained customers and intervention cost, not model accuracy alone. Predictions must use only information available at scoring time.”

### 2. Target definition
A clear label could be `1` if a currently active account's service terminates within the next 30 days, otherwise `0`, using a specified observation cutoff. Clarify paused accounts, non-payment suspensions, trial expirations, voluntary versus involuntary churn, and whether customers without a full 30-day follow-up are excluded.

### 3. Identify leakage
`cancellation_date` and `refund_after_cancel` are likely post-outcome or direct proxies and should not be predictors for a pre-cancellation model. `support_tickets_last_30d` is valid only if the complete window ends before the prediction timestamp. Always define the prediction time first.

### 4. Audit code
```python
def audit_dataframe(df, target=None):
    print("Shape:", df.shape)
    print("\\nDtypes:\\n", df.dtypes)
    print("\\nMissing:\\n", df.isna().sum().sort_values(ascending=False))
    print("\\nDuplicate rows:", df.duplicated().sum())
    if target and target in df:
        print("\\nTarget distribution:\\n", df[target].value_counts(dropna=False))
        print("\\nTarget proportions:\\n", df[target].value_counts(normalize=True, dropna=False))
```

### 5. Data dictionary
Include a row for each column. The most important field is “available at prediction time?” because a statistically useful feature can still be invalid if it is not known when predictions are made.

### 6. Sampling bias
Customers who contact support may be more frustrated, more engaged, or experiencing specific product issues than the overall population. The resulting model may perform poorly for customers who do not contact support. Compare the sample with the scoring population and document selection criteria.

### 7. Train/test split
Imputers and scalers learn values from data. If fit before splitting, information from the holdout set influences the training transformation. Use a pipeline so each cross-validation fold fits preprocessing only on that fold's training portion. Prefer chronological splits when deployment predicts future outcomes or when data drifts over time.

### 8. Missingness
- Median: robust numeric default when missingness is not itself the central signal.
- Most frequent: simple categorical imputation.
- Drop rows: acceptable only when missingness is limited and deletion will not create substantial bias or waste valuable examples.
Consider adding missingness indicators where justified, and investigate why values are absent.

## Part B — EDA and feature engineering

### 9. Numeric distributions
Example observations should reference visible patterns, units, and sample size: “Tenure is concentrated among newer accounts” or “monthly charges show a broad spread.” Do not infer that low tenure causes churn from a histogram alone.

### 10. Categorical comparison
```python
summary = (
    df.groupby("contract_type", dropna=False)["churn"]
      .agg(customers="size", churn_rate="mean")
      .sort_values("churn_rate", ascending=False)
)
```
Always show group sizes alongside rates and investigate whether the groups differ in other ways.

### 11. Correlation caution
A third variable, selection effects, or reverse timing can explain an association. Support tickets might indicate an existing issue rather than cause cancellation. Causal claims require an appropriate design and assumptions.

### 12. Outliers
Check units, allowed ranges, duplicates, and source records. Correct demonstrable data errors; preserve legitimate extreme customers or use a justified robust method. Do not delete observations only because they hurt model performance.

### 13. Feature engineering
```python
df["average_monthly_spend"] = (
    df["total_spend"] / df["tenure_months"].clip(lower=1)
)
```
Check zero or missing tenure explicitly. `total_spend` may contain future information if calculated beyond the prediction date, so enforce a time cutoff before using it.

### 14. Class balance
```python
prevalence = df["churn"].mean()
print(f"Churn prevalence: {prevalence:.1%}")
```
A model that always predicts the majority class can have high accuracy and zero recall for churn. Report precision, recall, F1, ROC-AUC and, for rare positives, precision-recall behavior.

### 15. Segment stability
Fit with multiple seeds, compare cluster sizes and feature profiles, and test on later periods. Cluster labels can be permuted, so compare membership with a label-invariant metric or align clusters by their profiles. Stability is evidence, not proof of business usefulness.

### 16. EDA summary
A good summary separates:
- Finding: a measured pattern with context.
- Caveat: missingness, sample selection, uncertainty, or possible confounding.
- Next question: what additional data or test could help make a decision?

## Part C — Modeling and evaluation

### 17. Baseline classifier
```python
from sklearn.dummy import DummyClassifier
from sklearn.metrics import accuracy_score, recall_score

baseline = DummyClassifier(strategy="most_frequent")
baseline.fit(X_train, y_train)
pred = baseline.predict(X_test)
print("Accuracy:", accuracy_score(y_test, pred))
print("Positive-class recall:", recall_score(y_test, pred, zero_division=0))
```
The baseline is a reference, not a useful churn intervention if it predicts no customers will churn.

### 18. Logistic regression pipeline
```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.linear_model import LogisticRegression

numeric_features = ["tenure_months", "monthly_fee", "support_tickets"]
categorical_features = ["contract_type", "payment_method"]

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore")),
])
preprocess = ColumnTransformer([
    ("num", numeric_pipe, numeric_features),
    ("cat", categorical_pipe, categorical_features),
])
model = Pipeline([
    ("preprocess", preprocess),
    ("classifier", LogisticRegression(max_iter=1000, class_weight="balanced")),
])
model.fit(X_train, y_train)
```
Use the features actually present in the dataset, and tune class weighting/threshold using validation data rather than the final test set.

### 19. Confusion matrix
TP=42, FP=18, FN=28, TN=212; total = 300.

- Precision = TP / (TP + FP) = 42 / 60 = **0.70**
- Recall = TP / (TP + FN) = 42 / 70 = **0.60**
- Accuracy = (TP + TN) / total = 254 / 300 ≈ **0.847**

### 20. Threshold trade-off
Raising the threshold usually labels fewer cases positive, often increasing precision and decreasing recall. Higher recall can be important when missing a high-risk case is expensive and the intervention is low-cost, provided the outreach volume remains manageable.

### 21. ROC-AUC vs PR-AUC
ROC-AUC summarizes ranking performance across true-positive and false-positive rates. Precision-recall analysis focuses on positive-class precision and recall and is often more revealing when positives are rare. Always interpret metrics with prevalence and the business action in mind.

### 22. Cross-validation
Stratified folds approximately preserve class proportions in each fold. Keeping transformations in the pipeline ensures the imputer, scaler, and encoder are fit only on each fold's training data. For grouped or temporal data, use group-aware or time-aware validation instead.

### 23. Regression baseline
```python
from sklearn.dummy import DummyRegressor
baseline = DummyRegressor(strategy="median")
baseline.fit(X_train, y_train)
predictions = baseline.predict(X_test)
```
Computing the median from the entire dataset leaks information from the test labels into the baseline.

### 24. Regression metrics
- **MAE:** average absolute error, in target units.
- **RMSE:** square root of average squared error, in target units; large errors have extra influence.
- **R²:** relative fit compared with predicting the test-set mean; it can be negative on held-out data.
If one property is predicted ₹50 lakh away while most are close, RMSE penalizes that large miss more strongly than MAE.

### 25. Error analysis
Compare MAE by price band, region, or property type, and inspect sample sizes. A gap may indicate missing features, data-quality issues, distribution shift, or uneven model performance. It does not by itself prove unfair treatment or a causal explanation.

### 26. Calibration
Among cases assigned probability 0.20, roughly 20% should experience the event over the defined outcome window for a well-calibrated model. Calibration matters when a probability is compared with intervention costs or used for expected-value calculations.

### 27. Model comparison
A suitable table includes the baseline, model name, validation metrics, runtime/complexity, and limitations. Compare models on the same splits and metrics. Do not select solely by one metric if operational or interpretability constraints matter.

### 28. Avoid overfitting the test set
The test set becomes part of the iterative decision process if you repeatedly tune against it. Use validation/cross-validation for selection and reserve the test set for a final evaluation. If substantial iteration already occurred, obtain a new untouched test period or dataset when possible.

## Part D — Unsupervised learning and business analysis

### 29. Prepare clustering features
Scale features because K-means uses distance; a high-magnitude feature can dominate the result. Fit scaling on the data used for clustering and document the chosen feature set.

### 30. Interpret clusters
Report cluster size and original-unit summaries. Use neutral labels such as “higher recent activity / lower tenure,” not unsupported labels such as “bad customers.” Check whether segments suggest a feasible and ethical action.

### 31. Choose cluster count
Inertia decreases as k grows, while silhouette scores summarize separation/cohesion under a particular distance measure. Combine these with stability, segment size, business interpretability, and actionability.

### 32. Sales KPI definitions
- Revenue: define whether it is gross or net and the currency/time period.
- Net revenue: gross sales less defined discounts, returns, and allowances.
- Units sold: state how returned/cancelled units are treated.
- Gross margin: revenue minus cost of goods sold, with a documented cost basis.
Apply consistent definitions across products and regions.

### 33. Time-aware validation
Random splits can train on future patterns and test on the past, giving an unrealistic estimate. Train on earlier periods and validate/test on later periods, using rolling-origin evaluation when appropriate.

### 34. Campaign conversion
- Campaign A: 36 / 600 = **6%**
- Campaign B: 28 / 350 = **8%**

Campaign B has a higher observed rate, but sampling uncertainty and allocation differences matter. If recipients were not randomly assigned, differences in audience or timing can confound the comparison. A randomized test with a pre-specified metric is stronger evidence of causal lift.

### 35. Simpson's paradox
An aggregate rate combines segment-level rates using segment weights. If campaigns reach different proportions of high- and low-conversion segments, the overall ranking can differ from the within-segment ranking. Compare like with like and inspect allocation.

## Part E — Delivery, governance, and communication

### 36. Model card
Include model name/version, owner, intended users, intended and prohibited uses, target definition, training/evaluation data description, metrics and splits, limitations, subgroup performance, privacy considerations, monitoring, and update cadence.

### 37. Privacy review
Examples: minimize collected fields, remove direct identifiers when not needed, restrict access, set retention limits, aggregate reporting, redact logs, use approved storage, document purpose, and review legal/organizational requirements.

### 38. Reproducibility
Record the Python/package versions, random seeds, data-generation process or approved data source, exact run command, expected outputs, and known nondeterminism. Keep large/private data out of version control.

### 39. Executive summary
A good summary states the decision and time horizon, the approach and holdout evaluation, the result in plain language, the recommended next action, the main caveat, and what should be tested before full rollout. Avoid unexplained acronyms and unsupported causal claims.

### 40. Capstone review
A strong submission contains a leakage-safe split, a baseline, a pipeline, appropriate metrics, threshold rationale, error analysis, a privacy/limitations section, and reproducible instructions. A useful final step is a small controlled pilot to measure actual intervention impact rather than assuming model performance translates directly to business value.

## Final project review checklist

- [ ] Target and prediction time are explicit.
- [ ] Data provenance and data dictionary are documented.
- [ ] Leakage and sampling bias have been considered.
- [ ] Preprocessing is fit within the training pipeline.
- [ ] Baseline and validation strategy are justified.
- [ ] Metrics match the decision and class balance.
- [ ] Error analysis includes meaningful segments.
- [ ] Recommendations distinguish correlation from causation.
- [ ] Privacy, fairness, and limitations are addressed.
- [ ] Another person can reproduce the results.
