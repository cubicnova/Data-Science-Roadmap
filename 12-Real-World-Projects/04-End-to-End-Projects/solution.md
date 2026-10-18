# Worked Solutions — End-to-End Data Science Projects

These reference solutions show patterns that can be adapted to the capstone. The correct design depends on the target, data timing, business costs, and deployment environment.

## Phase 1 — Problem framing and planning

### 1. Project charter
Example: “At the start of each week, help the retention team prioritize active subscription accounts that may cancel within the following 30 days. Evaluate the system on a temporally appropriate holdout using recall, precision, and expected intervention value. A score supports human review; it does not automatically trigger an adverse action.”

Include owner, target definition, prediction timestamp, outcome window, intended action, success metric, and prohibited uses.

### 2. Acceptance criteria
Examples:
- Required data schema passes validation.
- No known post-outcome features are used.
- Pipeline beats the baseline on the preselected validation metric.
- Final holdout report includes uncertainty and error analysis.
- Prediction function rejects missing required features clearly.
- Secrets are not committed or embedded in artifacts.
- Monitoring and rollback owner are documented.

Criteria should be measurable and chosen before reviewing final results.

### 3. Lifecycle
A practical sequence is data ingestion → schema/quality checks → EDA → split → preprocessing/model pipeline → validation → evaluation → artifact versioning → batch/API serving → monitoring → review/retraining. Data access approval, target definition, release approval, and incident response may require human review.

### 4. Risk register
Example risks and mitigations:
- Leakage — define prediction time; audit every feature.
- Missing or invalid data — schema/range checks and a documented failure policy.
- Privacy exposure — minimize fields, restrict access, redact logs.
- Drift — monitor data and outcome metrics.
- Unreproducible artifacts — pin dependencies, record seeds and versions.
- Excessive cost — budgets, resource limits, and cleanup.
- Silent release failure — smoke tests, alerts, and tested rollback.
Assign a named role/owner and likelihood/impact score in the actual project.

## Phase 2 — Data acquisition and validation

### 5. Data dictionary
Document each column's business meaning, type, units, source, valid values, sensitivity, and availability at prediction time. Mark identifiers and exclude them from predictors unless there is a justified reason.

### 6. Schema validation
```python
def validate_required_columns(df, required_columns):
    missing = sorted(set(required_columns) - set(df.columns))
    if missing:
        raise ValueError(f"Missing required columns: {missing}")
    return True
```

### 7. Quality report
```python
def quality_report(df, target=None):
    report = {
        "rows": len(df),
        "columns": len(df.columns),
        "duplicate_rows": int(df.duplicated().sum()),
        "missing_rate": df.isna().mean().sort_values(ascending=False),
        "dtypes": df.dtypes.astype(str),
    }
    if target is not None and target in df:
        report["target_distribution"] = df[target].value_counts(dropna=False)
        report["target_prevalence"] = df[target].mean()
    return report
```
For numeric ranges and categorical cardinality, add domain-specific checks instead of assuming every value is valid.

### 8. Data leakage review
Write the scoring timestamp and label window first. A feature is eligible only if it would be known at that timestamp in production. Exclude post-outcome actions, future aggregates, and target-derived fields.

### 9. Reproducible split
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```
Use time-aware splits when forecasting future observations and group-aware splits when related records from the same person, account, device, or site must not appear in both train and test.

### 10. Data lineage
Record source or generator, extraction/generation timestamp, data version/hash where feasible, transformation steps, filters, target definition, owner, and approval/retention constraints.

## Phase 3 — EDA and feature engineering

### 11. EDA report
Include target distribution, numeric distributions, and grouped rates with sample sizes. Captions should state the unit and population. Write findings as descriptions, not causes, unless a suitable causal design supports the claim.

### 12. Missingness
Inspect whether missingness varies by time, source, target, or subgroup. Median imputation is a simple numeric baseline; most-frequent imputation can work for categories. Dropping rows can waste data and introduce selection bias. Fit transformations on training data only.

### 13. Outlier policy
Check units, source systems, and plausible ranges. Correct demonstrable errors; preserve valid extremes or use a robust method with documented reasoning. Do not remove records only because they are difficult to predict.

### 14. Feature engineering
Example: average monthly spend = cumulative spend up to the prediction timestamp divided by observed tenure. Validate zero tenure and ensure cumulative spend excludes future transactions. Put the transformation in reproducible code.

### 15. Reusable preprocessing
```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore")),
])
preprocessor = ColumnTransformer([
    ("num", numeric_pipe, numeric_features),
    ("cat", categorical_pipe, categorical_features),
])
```
Attach a model to this preprocessor in a single `Pipeline`.

## Phase 4 — Training and evaluation

### 16. Baseline
Use `DummyClassifier` for classification or `DummyRegressor` for regression. A business rule can also be a useful baseline if it is defined before model comparison.

### 17. Candidate model
```python
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline

candidate = Pipeline([
    ("preprocess", preprocessor),
    ("model", LogisticRegression(max_iter=1000)),
])
candidate.fit(X_train, y_train)
```
Ensure the column lists match the actual input data. Handle class imbalance and threshold selection based on validation evidence.

### 18. Evaluation report
For classification, report class prevalence, confusion matrix, precision, recall, F1, and ROC-AUC; consider precision-recall analysis for rare positives. For regression, report MAE/RMSE and inspect residuals. State why the metrics align with the decision.

### 19. Cross-validation
Use stratified folds for independent classification rows, group-aware folds for repeated entities, and time-aware validation for temporal prediction. Pipelines keep learned transformations within each fold.

### 20. Threshold policy
Choose a threshold on validation data. For a limited review team, evaluate precision and the number of cases flagged at each threshold. If false negatives are costly, evaluate recall and the intervention trade-off. Lock the policy before final test evaluation.

### 21. Error analysis
Compare metrics across meaningful slices, show each slice's sample size, and avoid conclusions based on tiny samples. Differences can reflect data quality, distribution shift, or model limitations; investigate before assigning causes.

### 22. Test-set discipline
Reserve the test set for one final evaluation after model and threshold selection. If the test set has been repeatedly used for tuning, treat it as a validation set and obtain a new untouched holdout or future period where feasible.

## Phase 5 — Packaging and serving

### 23. Save a pipeline
```python
import joblib
joblib.dump(candidate, "model_pipeline.joblib")
```
Record model version, source commit, training data version, dependency versions, metrics, and feature schema. Only load trusted artifacts because pickle-based formats can execute code during deserialization.

### 24. Reusable prediction function
```python
def predict_frame(model, frame, required_features):
    missing = sorted(set(required_features) - set(frame.columns))
    if missing:
        raise ValueError(f"Missing required features: {missing}")
    return model.predict(frame[required_features])
```
For probabilities, check that the chosen model supports `predict_proba`; return documented identifiers and scores without unnecessary personal data.

### 25. API contract
A request should declare each required field and type. Return prediction, model version, and a request ID where appropriate. Return a clear client error for invalid inputs, avoid exposing stack traces, set timeouts, and provide a health endpoint that reveals no secrets.

### 26. Batch vs API
Batch scoring suits large periodic volumes where minute-level latency is unnecessary. Online APIs suit low-latency individual requests but need concurrency, timeouts, availability, and scaling plans. Choose based on freshness, volume, latency, and integration requirements.

### 27. Container and configuration
Use a reviewed base image, pinned dependencies, a non-root user, and a `.dockerignore`. Environment variables can carry non-secret settings such as port, environment, and model path. Inject secrets through a secret manager or workload identity—not `ENV`, source files, or image layers.

### 28. Automated tests
Test required-column validation, prediction output shape, finite numeric probabilities, unknown categories, missing values, and a malformed request. Add an API smoke test if a serving layer exists.

## Phase 6 — Operations and governance

### 29. Monitoring plan
Example signals:
- Data quality: schema failure and missing-value rates.
- Service: request success rate, latency, CPU/memory, restarts.
- Model: prediction distribution and input drift.
- Outcomes: delayed precision/recall or business KPI when labels arrive.
For each alert, define an owner, severity, investigation step, and escalation path.

### 30. Drift response
Compare incoming feature distributions with a documented baseline and inspect data-quality changes. Drift is a signal to investigate, not an automatic reason to retrain. Verify source changes, label availability, and real performance before choosing retraining or rollback.

### 31. Rollback
Store the previous known-good application image and compatible model artifact. Trigger rollback when pre-agreed health, error, latency, or quality guardrails fail. After rollback, verify health, representative predictions, error rate, and latency, then preserve sanitized evidence for incident review.

### 32. Privacy and security
Use approved data sources, minimize personal fields, apply least-privilege access, store secrets securely, redact logs, encrypt data in transit/at rest where required, define retention, audit access, and document incident response. Follow applicable organizational and legal requirements.

### 33. Cost model
Estimate compute, storage, network egress, logs/metrics, registry, and managed services. Use the provider's current pricing calculator or label assumptions as hypothetical. Include test/staging environments and cleanup. Optimize after load testing without violating reliability or latency targets.

### 34. Model card
Document intended and prohibited use, owner, target and data, evaluation design, metrics, subgroup behavior, limitations, privacy risks, monitoring, and version/update policy.

## Phase 7 — Communication and capstone

### 35. Technical README
Include prerequisites, environment setup, data acquisition/generation, commands to train/evaluate/test/serve, expected outputs, artifact locations, configuration, troubleshooting, and limitations.

### 36. Executive summary
State the business question, data and method, holdout result, recommended action, key uncertainty, operational requirements, and next step. Avoid technical jargon and causal claims unsupported by the design.

### 37. Reproducibility audit
Run the instructions in a clean environment, without relying on notebook state or files outside the repository. Fix missing dependencies, hidden paths, manual steps, and unclear data assumptions.

### 38. Capstone review
A complete capstone has documented data, automated validation, EDA, baseline and candidate model, leakage-safe evaluation, tests, reusable prediction, a model card, monitoring and rollback plans, and reproducible instructions.

### 39. Pilot plan
Define a limited rollout, human review, success metric, safety/quality guardrails, monitoring cadence, owner, and decision date. Where a model triggers an intervention, compare outcomes with an appropriate control or experiment before claiming incremental benefit.

### 40. Final presentation
A concise 5–8 slide story: problem, data, method, evaluation, examples, limitations and risks, recommendation, and next steps. Include only charts that support a decision.

## Final acceptance checklist

- [ ] Clear problem, target, prediction time, and success criteria.
- [ ] Data lineage, schema, and quality report.
- [ ] Leakage review and privacy controls.
- [ ] EDA with documented caveats.
- [ ] Reproducible baseline and candidate pipeline.
- [ ] Appropriate evaluation and error analysis.
- [ ] Validated prediction interface and tests.
- [ ] Artifact/version/dependency record.
- [ ] Monitoring, rollback, and cost plan.
- [ ] README, model card, and clean-environment reproduction.
