# Solutions — Imbalanced Datasets

These are reference answers and example patterns. Exact scores vary by package version and random seed. Run comparisons using the same folds and data partitions.

## 1–6. Foundations

1. Class imbalance means one target class occurs much less often than another. Examples include payment fraud and rare equipment failures.
2. A majority-only classifier can be accurate when most observations belong to one class, yet have zero recall for the minority class.
3. For fraud as positive: TP = fraudulent transactions flagged; TN = legitimate transactions not flagged; FP = legitimate transactions flagged; FN = fraud not detected.
4. Precision is TP/(TP+FP); recall is TP/(TP+FN). Recall may dominate in serious disease screening or threat detection; precision may dominate when each false alert is expensive to investigate.
5. F1 is the harmonic mean of precision and recall. It omits true negatives and does not directly encode different business costs.
6. ROC-AUC evaluates ranking over true-positive vs false-positive rates; average precision summarizes the precision-recall curve. With rare positives, precision directly reflects how many alerts are correct and should be compared with the positive prevalence.

## 7–10. Create data, split, and baseline

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split, StratifiedKFold, cross_validate
from sklearn.dummy import DummyClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import (
    accuracy_score, balanced_accuracy_score, precision_score, recall_score,
    f1_score, average_precision_score, roc_auc_score, confusion_matrix,
    classification_report, precision_recall_curve, ConfusionMatrixDisplay
)

X, y = make_classification(
    n_samples=5000, n_features=20, n_informative=6, n_redundant=3,
    weights=[0.98, 0.02], class_sep=1.0, random_state=42
)
print(pd.Series(y).value_counts().sort_index())
print("Positive prevalence:", y.mean())

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, stratify=y, random_state=42
)
print("Train prevalence:", y_train.mean(), "Test prevalence:", y_test.mean())

baseline = DummyClassifier(strategy="most_frequent")
baseline.fit(X_train, y_train)
pred = baseline.predict(X_test)
prob = baseline.predict_proba(X_test)[:, 1]
print("Accuracy:", accuracy_score(y_test, pred))
print("Balanced accuracy:", balanced_accuracy_score(y_test, pred))
print("Precision:", precision_score(y_test, pred, zero_division=0))
print("Recall:", recall_score(y_test, pred, zero_division=0))
print("F1:", f1_score(y_test, pred, zero_division=0))
print("Average precision:", average_precision_score(y_test, prob))
```

A majority-only baseline has accuracy close to the majority prevalence and no minority-class recall. Its AP may equal the positive prevalence because all examples receive the same score.

## 11–13. Logistic regression and metrics

```python
model = LogisticRegression(max_iter=2000, random_state=42)
model.fit(X_train, y_train)
pred = model.predict(X_test)
prob = model.predict_proba(X_test)[:, 1]
print(classification_report(y_test, pred, zero_division=0))
print(confusion_matrix(y_test, pred))
print("AP:", average_precision_score(y_test, prob))
print("ROC-AUC:", roc_auc_score(y_test, prob))

precision, recall, thresholds = precision_recall_curve(y_test, prob)
plt.plot(recall, precision)
plt.xlabel("Recall")
plt.ylabel("Precision")
plt.title("Precision–Recall curve")
plt.show()
```

For confusion matrix entries `tn, fp, fn, tp`, specificity = `tn / (tn + fp)`, precision = `tp / (tp + fp)`, recall = `tp / (tp + fn)`, and balanced accuracy = `(recall + specificity) / 2`. Handle zero denominators explicitly.

## 14–15. Class weights

```python
weighted = LogisticRegression(class_weight="balanced", max_iter=2000, random_state=42)
weighted.fit(X_train, y_train)
weighted_prob = weighted.predict_proba(X_test)[:, 1]
weighted_pred = weighted.predict(X_test)
print(classification_report(y_test, weighted_pred, zero_division=0))
print("AP:", average_precision_score(y_test, weighted_prob))
```

Balanced weights are calculated from class frequencies. Custom weights change the error trade-off. Select them with cross-validation and a metric or cost function tied to the task, not by looking at test scores.

## 16–19. Resampling and leakage-safe pipelines

Install imbalanced-learn if needed:

```bash
python -m pip install imbalanced-learn
```

```python
from imblearn.over_sampling import RandomOverSampler, SMOTE
from imblearn.under_sampling import RandomUnderSampler
from imblearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

# Direct resampling is for demonstration on training data only:
ros = RandomOverSampler(random_state=42)
X_ros, y_ros = ros.fit_resample(X_train, y_train)
print("Before:", np.bincount(y_train))
print("After: ", np.bincount(y_ros))

rus = RandomUnderSampler(random_state=42)
X_rus, y_rus = rus.fit_resample(X_train, y_train)
print("Undersampled:", np.bincount(y_rus))

smote_pipeline = Pipeline([
    ("scale", StandardScaler()),
    ("smote", SMOTE(random_state=42)),
    ("model", LogisticRegression(max_iter=2000, random_state=42))
])
```

Never run a sampler on the full dataset before splitting: synthetic or duplicated examples can leak information across partitions. An imbalanced-learn pipeline lets cross-validation fit scaling and sampling only on each fold's training portion.

## 20. Compare strategies using the same folds

```python
from sklearn.model_selection import StratifiedKFold, cross_validate
from sklearn.ensemble import RandomForestClassifier

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
strategies = {
    "Logistic baseline": Pipeline([
        ("scale", StandardScaler()),
        ("model", LogisticRegression(max_iter=2000, random_state=42))
    ]),
    "Class weighted": Pipeline([
        ("scale", StandardScaler()),
        ("model", LogisticRegression(class_weight="balanced", max_iter=2000, random_state=42))
    ]),
    "Random oversampling": Pipeline([
        ("scale", StandardScaler()),
        ("sampler", RandomOverSampler(random_state=42)),
        ("model", LogisticRegression(max_iter=2000, random_state=42))
    ]),
    "SMOTE": Pipeline([
        ("scale", StandardScaler()),
        ("sampler", SMOTE(random_state=42)),
        ("model", LogisticRegression(max_iter=2000, random_state=42))
    ]),
}
scoring = {"ap": "average_precision", "recall": "recall", "precision": "precision",
           "balanced_accuracy": "balanced_accuracy"}
rows = []
for name, estimator in strategies.items():
    result = cross_validate(estimator, X_train, y_train, cv=cv, scoring=scoring, n_jobs=-1)
    rows.append({"strategy": name, **{
        metric: result[f"test_{metric}"].mean()
        for metric in scoring
    }})
display(pd.DataFrame(rows).set_index("strategy").round(3))
```

Compare mean and standard deviation, not just a single fold. A higher recall may come with more false positives. AP is often a useful primary metric for ranking rare positives, but business costs may call for a custom objective.

## 21. Compare model families

```python
from sklearn.tree import DecisionTreeClassifier
models = {
    "Logistic": Pipeline([
        ("scale", StandardScaler()),
        ("model", LogisticRegression(class_weight="balanced", max_iter=2000, random_state=42))
    ]),
    "Decision tree": DecisionTreeClassifier(class_weight="balanced", random_state=42),
    "Random forest": RandomForestClassifier(
        class_weight="balanced_subsample", n_estimators=200, random_state=42, n_jobs=-1
    ),
}
for name, estimator in models.items():
    result = cross_validate(estimator, X_train, y_train, cv=cv,
                            scoring={"ap": "average_precision", "bal_acc": "balanced_accuracy"},
                            n_jobs=-1)
    print(name, "AP", result["test_ap"].mean(), "balanced accuracy", result["test_bal_acc"].mean())
```

For samplers, use the imbalanced-learn pipeline. For model comparisons, hold the folds fixed.

## 22–23. Threshold selection and costs

Choose thresholds on a validation partition, not the test set. One simple approach targets a minimum recall:

```python
X_fit, X_val, y_fit, y_val = train_test_split(
    X_train, y_train, test_size=0.25, stratify=y_train, random_state=7
)
model = LogisticRegression(class_weight="balanced", max_iter=2000, random_state=42)
model.fit(X_fit, y_fit)
val_prob = model.predict_proba(X_val)[:, 1]

precision, recall, thresholds = precision_recall_curve(y_val, val_prob)
target_recall = 0.80
valid = np.where(recall[:-1] >= target_recall)[0]
if len(valid):
    chosen_idx = valid[np.argmax(precision[:-1][valid])]
    threshold = thresholds[chosen_idx]
    print("Threshold:", threshold, "Precision:", precision[chosen_idx], "Recall:", recall[chosen_idx])
else:
    print("Target recall not achieved on validation data.")
```

For costs, define `cost = FP * cost_fp + FN * cost_fn`, calculate it for each candidate threshold on validation predictions, and choose the threshold with minimum cost. State assumptions and ensure the costs reflect actual operational impact.

## 24–25. Calibration and ROC-AUC

Resampling changes the training class distribution; class weighting changes the loss. Therefore raw probabilities may not reflect deployment prevalence. Inspect calibration curves or Brier score on representative validation data, and consider calibration with a separate calibration procedure if needed. ROC-AUC measures ranking and is not itself a measure of alert precision. Always examine AP, prevalence, and the confusion matrix at the selected threshold.

## 26–29. Confusion matrix, pipeline, tuning, and scaling

```python
from sklearn.model_selection import GridSearchCV

pipeline = Pipeline([
    ("scale", StandardScaler()),
    ("sampler", SMOTE(random_state=42)),
    ("model", LogisticRegression(max_iter=2000, random_state=42))
])
param_grid = {
    "sampler__k_neighbors": [3, 5],
    "model__C": [0.1, 1.0, 10.0],
}
search = GridSearchCV(pipeline, param_grid, scoring="average_precision", cv=cv, n_jobs=-1)
search.fit(X_train, y_train)
print(search.best_params_, search.best_score_)
best_model = search.best_estimator_

# Final test evaluation only after all choices are complete:
test_prob = best_model.predict_proba(X_test)[:, 1]
test_pred = (test_prob >= 0.5).astype(int)  # replace with a pre-selected validation threshold if used
print("AP:", average_precision_score(y_test, test_prob))
print("Balanced accuracy:", balanced_accuracy_score(y_test, test_pred))
print(classification_report(y_test, test_pred, zero_division=0))
ConfusionMatrixDisplay.from_predictions(y_test, test_pred)
plt.show()
```

Scaling matters for distance-based methods such as SMOTE and for many linear models. Trees generally do not require feature scaling. Scaling belongs inside the pipeline to avoid fold leakage.

## 30–34. Robustness, drift, fairness, and error analysis

- Report mean and standard deviation over stratified folds; consider confidence intervals where appropriate.
- Precision depends on prevalence: if prevalence falls while sensitivity and specificity stay fixed, precision generally falls.
- Monitor prevalence, AP, precision/recall on delayed labels, alert volume, calibration, and subgroup error rates.
- Inspect false positives and false negatives with domain experts; improve labels, features, thresholds, and downstream review rather than relying only on synthetic data.
- Aggregate scores can hide subgroup disparities. Compare metrics across relevant, lawfully defined groups and investigate sample size and uncertainty.

## 35. Experiment log

Record: dataset and prevalence, target definition, split strategy, sampler, model and parameters, CV folds, primary metric, secondary metrics, threshold, confusion matrix, runtime, package versions, and limitations.

## 36–37. Mini-project patterns

1. Define the positive class and cost of each error.
2. Split into train/test with stratification before any sampling.
3. Establish a dummy baseline.
4. Compare class weighting, random oversampling, and SMOTE within pipelines and CV.
5. Tune only on training folds; choose threshold on validation data.
6. Evaluate once on the test set.
7. Discuss alert workload, calibration, drift, fairness, and limitations.

## 38–40. Review and challenge

Common mistakes include resampling before splitting, optimizing accuracy alone, tuning the threshold on the test set, reporting only one fold, and assuming synthetic samples represent new real-world cases. For extremely rare positives, compare average precision to positive prevalence and assess whether the resulting precision and alert volume are operationally useful. A model is useful only if it improves decisions at an acceptable cost.
