# Reference Solutions — Machine Learning Workflow

These are suggested solutions, not the only valid answers. Adapt metrics and design choices to the business context and data.

## Beginner: workflow and framing

### 1. Workflow stages
Problem framing → data acquisition and understanding → quality checks and exploration → split strategy → preprocessing → baseline → model training and validation → final test evaluation → communication/deployment → monitoring and iteration.

### 2. Delivery duration
Regression, because the target is numeric. MAE is easy to interpret in minutes; RMSE is useful when large errors should be penalized more strongly.

### 3. Fraud review
A possible target is `is_fraud` (0/1). If fraud is rare, a model can achieve high accuracy by predicting “not fraud” for nearly every transaction. Precision, recall, F1, PR-AUC, and review capacity may be more informative.

### 4. Feature vs. target
For predicting a student's exam score, prior attendance and practice hours may be features (`X`), while the final exam score is the target (`y`).

### 5. Stakeholder questions
Examples: What decision will the churn score support? How early must the score be available? What is the cost of contacting a customer unnecessarily versus missing a likely churner? How will success be measured?

### 6. Data provenance
Permission and provenance help ensure lawful, ethical use and traceability. Unrepresentative data can lead to poor generalization or systematically worse outcomes for some groups.

### 7. Baseline
A simple benchmark such as majority-class prediction or predicting the mean. It shows whether a more complex model adds meaningful value.

### 8. Split purposes
- Training: fit model parameters and preprocessing.
- Validation/cross-validation: compare models and tune settings.
- Test: final estimate after decisions are complete.

### 9. Overfitting
The model learns patterns specific to the training sample, including noise, and performs worse on unseen data. A large train/validation performance gap is one warning sign.

### 10. Initial checks
Inspect shape and column names, data types, missing values, duplicates, value ranges, target distribution, and possible target leakage.

## Intermediate: data preparation and evaluation

### 11. Features and target

```python
X = df[["age", "income"]]
y = df["churn"]
```

Do not include the target in `X`.

### 12. Stratified split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)
```

For time-series or grouped observations, use a split method that respects time or groups instead.

### 13. Fit the scaler on training data
The scaler learns statistics such as mean and standard deviation. If test data influences those statistics, information from the held-out set leaks into training. Fit a pipeline on training data and use it to transform held-out data.

### 14–15. Numeric preprocessing and classifier

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])

model = Pipeline([
    ("preprocess", numeric_pipe),
    ("classifier", LogisticRegression(max_iter=1000)),
])
model.fit(X_train, y_train)
```

For mixed data, use `ColumnTransformer` to apply separate transformations to numeric and categorical columns.

### 16. Imbalanced classes
A classifier that always predicts the 95% negative class can score 95% accuracy while detecting no positive cases. Inspect recall and precision for the positive class, the confusion matrix, and possibly PR-AUC. Choose a threshold based on error costs and operational capacity.

### 17. Precision and recall
- Precision = true fraud alerts / all fraud alerts. Of the cases flagged, how many were actually fraud?
- Recall = detected fraud / all actual fraud. Of all fraud cases, how many were found?

### 18. MAE = 4.2 minutes
The model's predictions are off by an average absolute amount of about 4.2 minutes on the evaluated observations. This does not mean every prediction is within 4.2 minutes.

### 19. Chronological split
Use it when future predictions must be made from past data, such as demand forecasting. A random split could train on future patterns and evaluate on earlier observations, producing an unrealistic estimate.

### 20. Repeated test-set use
Repeatedly choosing settings based on test performance indirectly tunes to the test set. Keep it untouched during model selection; use validation data or cross-validation, then evaluate once at the end.

### 21. Missing values
- Median imputation: simple and robust to outliers, but may distort relationships and understate uncertainty.
- Row removal: straightforward, but can discard valuable data and introduce selection bias.
- A missingness indicator can sometimes preserve useful information about the missingness process.

### 22. Post-outcome feature
This is target leakage because the feature would not exist at prediction time. Remove it or reconstruct the feature using only information available at the intended prediction timestamp.

### 23. Precision, recall, F1

```python
TP, FP, FN = 40, 10, 20
precision = TP / (TP + FP)  # 0.80
recall = TP / (TP + FN)     # 0.6667
f1 = 2 * precision * recall / (precision + recall)  # about 0.7273
```

Precision is 0.80, recall is approximately 0.667, and F1 is approximately 0.727. True negatives are not needed for these three calculations.

### 24. Cross-validation
It evaluates a model across multiple folds, reducing dependence on one particular split. It costs more computation and still requires a suitable split strategy.

## Advanced: model selection and reliability

### 25. Baseline vs. logistic regression

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.dummy import DummyClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, roc_auc_score

X, y = make_classification(
    n_samples=1200, n_features=12, n_informative=6,
    n_redundant=2, weights=[0.75, 0.25],
    random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)

baseline = DummyClassifier(strategy="most_frequent")
candidate = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000, random_state=42)
)

for name, estimator in [("Baseline", baseline), ("Logistic regression", candidate)]:
    estimator.fit(X_train, y_train)
    pred = estimator.predict(X_test)
    print(name)
    print(classification_report(y_test, pred, zero_division=0))
    if hasattr(estimator, "predict_proba"):
        print("ROC-AUC:", roc_auc_score(y_test, estimator.predict_proba(X_test)[:, 1]))
```

Interpret all metrics together. The dummy model is a reference point, not a model expected to be useful.

### 26. Mixed numeric/categorical pipeline

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "income"]
categorical_features = ["region", "plan"]

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("onehot", OneHotEncoder(handle_unknown="ignore")),
])

preprocess = ColumnTransformer([
    ("num", numeric_pipe, numeric_features),
    ("cat", categorical_pipe, categorical_features),
])

pipeline = Pipeline([
    ("preprocess", preprocess),
    ("model", LogisticRegression(max_iter=1000)),
])
pipeline.fit(X_train, y_train)
```

The column names must exist in the input DataFrame. Fit only on the training split; the pipeline applies learned transformations consistently later.

### 27. Metric selection
- Accuracy: overall correctness when classes and error costs are reasonably balanced.
- Precision: when false positives are costly.
- Recall: when missing positives is costly.
- F1: a balance of precision and recall when both matter.
- ROC-AUC: ranking quality across thresholds; for rare positives, also inspect PR-AUC and precision/recall at an operational threshold.

### 28. Leakage vs. overfitting
Leakage means information unavailable at prediction time enters model development, such as a “refund issued” field used to predict whether a purchase will be refunded. Overfitting means the model adapts too closely to training examples, such as a deep tree memorizing noise. Both can produce poor generalization, but the remedies differ.

### 29. Reproducibility checklist
Record dataset source/version and access date; preprocessing and feature definitions; split method and seed; code commit; package versions; model class and hyperparameters; metrics and evaluation data; runtime assumptions; known limitations; and instructions to rerun the experiment.

### 30. Offline success, production failure
Possible causes include data drift, concept drift, training/serving feature mismatch, changed upstream schema, missing values or new categories, leakage in offline evaluation, a different production population, delayed labels, threshold mismatch, and infrastructure failures. Compare production inputs with training data, validate feature computation, monitor delayed outcomes, and review logs.

### 31. Threshold selection
The model estimates scores or probabilities; a threshold converts scores into actions. The best threshold depends on false-positive/false-negative costs, capacity, and policy constraints. Select it using validation data and an explicit decision criterion.

### 32. Fair comparison
Use the same training/test split or same cross-validation folds, identical target and eligible features, leakage-safe pipelines, predeclared metrics, and comparable tuning budgets. Tune on training/validation folds, freeze choices, and use the held-out test set only for final evaluation.

### 33. Feature importance and causation
Importance indicates a feature helps prediction under a particular model and dataset. It does not establish that changing the feature causes the outcome; confounding, proxies, and correlated predictors can explain importance.

### 34. Monitoring plan
- Inputs: schema, missingness, category changes, ranges, and feature drift.
- Predictions: score distribution, class/action rates, and threshold alerts.
- Outcomes: delayed precision/recall, calibration, and error by relevant segment when labels arrive.
- Business: intervention rate, cost, conversion/churn outcomes, and operational workload.
- Process: data pipeline failures, latency, model version, and retraining approval.

## Mini-project reference outline

```python
# 1. Define target and features.
X = df.drop(columns=["target"])
y = df["target"]

# 2. Split before fitting learned transformations.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# 3. Build a baseline and a leakage-safe pipeline.
# Choose numeric/categorical columns and use ColumnTransformer.
# Fit on X_train/y_train; evaluate once on X_test/y_test.

# 4. Record metrics, split method, limitations, and monitoring signals.
```

Use only features available at the time the prediction would be made. For a fair comparison, evaluate the baseline and candidate on the same held-out observations.

## Example model-card template

- **Model name/version:**
- **Intended use:**
- **Out-of-scope use:**
- **Target and prediction time:**
- **Dataset/source and date range:**
- **Split strategy:**
- **Baseline and candidate metrics:**
- **Known limitations:**
- **Fairness and privacy considerations:**
- **Monitoring and retraining triggers:**
