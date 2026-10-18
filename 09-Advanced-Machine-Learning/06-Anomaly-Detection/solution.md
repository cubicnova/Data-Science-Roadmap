# Solutions — Anomaly Detection

These are reference patterns. Results vary with library versions, generated data, and model settings. Anomaly scores are algorithm-specific; do not compare raw scores across different estimators without calibration.

## 1–6. Foundations

1. Anomaly detection finds observations that differ from expected patterns. Examples include unusual payment activity, abnormal machine vibration, and suspicious login behavior.
2. Outlier detection works with a dataset that may already contain unusual observations. Novelty detection learns a representation of normal data and identifies unusual new observations.
3. A point anomaly is one unusual record; a contextual anomaly is unusual only in context (e.g. a high temperature in winter); a collective anomaly is an unusual sequence or group.
4. Labels are often unavailable, expensive, delayed, or incomplete. Some methods learn structure from data, or from a relatively clean set of normal examples.
5. An anomaly can be a legitimate rare event, a new customer behavior, a sensor change, or a data-quality issue. Human or downstream validation is often necessary.
6. Without reliable labels, use expert review, stability, alert-volume constraints, known incident examples, and monitoring. These are useful but do not replace rigorous labeled evaluation when labels become available.

## 7–8. Generate and inspect data

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs

rng = np.random.default_rng(42)
X_normal, _ = make_blobs(n_samples=1000, centers=[(0, 0), (3, 3)],
                         cluster_std=[0.7, 0.6], random_state=42)
X_anom = rng.uniform(low=-5, high=8, size=(40, 2))
X = np.vstack([X_normal, X_anom])
y_true = np.r_[np.zeros(len(X_normal), dtype=int), np.ones(len(X_anom), dtype=int)]

plt.scatter(X[y_true == 0, 0], X[y_true == 0, 1], s=12, label="Normal")
plt.scatter(X[y_true == 1, 0], X[y_true == 1, 1], s=24, label="Synthetic anomaly")
plt.legend()
plt.title("Synthetic data with known labels")
plt.show()

df = pd.DataFrame(X, columns=["feature_1", "feature_2"])
print(df.describe())
print(df.isna().sum())
```

The synthetic labels are available for evaluating this exercise, but a detector would not receive them during unsupervised fitting.

## 9–11. Isolation Forest

```python
from sklearn.ensemble import IsolationForest
from sklearn.metrics import precision_score, recall_score, f1_score, average_precision_score

iso = IsolationForest(n_estimators=200, contamination=0.04, random_state=42)
pred_label = iso.fit_predict(X)
pred_anomaly = (pred_label == -1).astype(int)
print("Flagged:", pred_anomaly.sum())
print("Precision:", precision_score(y_true, pred_anomaly, zero_division=0))
print("Recall:", recall_score(y_true, pred_anomaly, zero_division=0))
print("F1:", f1_score(y_true, pred_anomaly, zero_division=0))

# IsolationForest's decision_function: lower values are more abnormal.
anomaly_score = -iso.decision_function(X)
print("AP:", average_precision_score(y_true, anomaly_score))
```

In scikit-learn, `predict` returns `1` for inliers and `-1` for outliers. `contamination` sets a threshold based on the expected fraction of outliers; it does not discover or guarantee the real prevalence. More trees can improve stability at additional compute cost.

## 12–14. Local Outlier Factor

```python
from sklearn.neighbors import LocalOutlierFactor

lof = LocalOutlierFactor(n_neighbors=20, contamination=0.04)
lof_labels = lof.fit_predict(X)
lof_anomaly = (lof_labels == -1).astype(int)
lof_score = -lof.negative_outlier_factor_
print("Flagged:", lof_anomaly.sum())
print("Precision:", precision_score(y_true, lof_anomaly, zero_division=0))
print("Recall:", recall_score(y_true, lof_anomaly, zero_division=0))
print("AP:", average_precision_score(y_true, lof_score))
```

For novelty detection, use a clean training set and only predict on unseen data:

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

X_train, X_test, y_train, y_test = train_test_split(
    X, y_true, test_size=0.30, random_state=42, stratify=y_true
)
# For a true novelty scenario, train on normal examples only.
X_normal_train = X_train[y_train == 0]
lof_novelty = make_pipeline(
    StandardScaler(),
    LocalOutlierFactor(n_neighbors=20, novelty=True, contamination=0.04)
)
lof_novelty.fit(X_normal_train)
# The novelty=True API is intended for unseen observations:
test_pred = lof_novelty.predict(X_test)
test_anomaly = (test_pred == -1).astype(int)
```

LOF compares local density to neighboring density. Smaller neighborhoods detect very local deviations but may be noisy; larger neighborhoods smooth local structure. In novelty mode, do not use `predict`, `decision_function`, or `score_samples` on the training observations.

## 15–16. One-Class SVM

```python
from sklearn.svm import OneClassSVM
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

ocsvm = make_pipeline(
    StandardScaler(),
    OneClassSVM(kernel="rbf", nu=0.04, gamma="scale")
)
# Demonstration fit uses normal examples only for novelty detection.
ocsvm.fit(X_normal)
test_pred = ocsvm.predict(X)
test_anomaly = (test_pred == -1).astype(int)
print("Precision:", precision_score(y_true, test_anomaly, zero_division=0))
print("Recall:", recall_score(y_true, test_anomaly, zero_division=0))
```

`nu` controls an upper bound on the fraction of training errors and a lower bound on the fraction of support vectors in the standard formulation. `gamma` controls the RBF kernel's locality. Scaling matters because feature magnitudes affect distances and kernel values.

## 17. Elliptic Envelope

```python
from sklearn.covariance import EllipticEnvelope

rng = np.random.default_rng(42)
X_gaussian = rng.normal(size=(1000, 2))
X_gaussian_outliers = rng.uniform(-6, 6, size=(40, 2))
X_cov = np.vstack([X_gaussian, X_gaussian_outliers])
y_cov = np.r_[np.zeros(len(X_gaussian), dtype=int),
              np.ones(len(X_gaussian_outliers), dtype=int)]

detector = EllipticEnvelope(contamination=0.04, random_state=42)
labels = detector.fit_predict(X_cov)
anomaly = (labels == -1).astype(int)
print("Precision:", precision_score(y_cov, anomaly, zero_division=0))
print("Recall:", recall_score(y_cov, anomaly, zero_division=0))
```

Elliptic Envelope estimates robust location and covariance. It works best when normal data are approximately Gaussian/elliptical and may perform poorly with multiple clusters or complex shapes.

## 18–20. Compare algorithms and evaluate

```python
from sklearn.ensemble import IsolationForest
from sklearn.neighbors import LocalOutlierFactor
from sklearn.svm import OneClassSVM
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline
from sklearn.metrics import roc_auc_score, average_precision_score

detectors = {
    "Isolation Forest": IsolationForest(contamination=0.04, random_state=42),
    "LOF": LocalOutlierFactor(n_neighbors=20, contamination=0.04),
    "One-Class SVM": make_pipeline(
        StandardScaler(), OneClassSVM(nu=0.04, gamma="scale")
    ),
}
rows = []
for name, detector in detectors.items():
    if name == "LOF":
        labels = detector.fit_predict(X)
        score = -detector.negative_outlier_factor_
    else:
        labels = detector.fit_predict(X)
        if name == "Isolation Forest":
            score = -detector.decision_function(X)
        else:
            # Pipeline decision_function is available; more negative is more abnormal.
            score = -detector.decision_function(X)
    flagged = (labels == -1).astype(int)
    rows.append({
        "model": name,
        "precision": precision_score(y_true, flagged, zero_division=0),
        "recall": recall_score(y_true, flagged, zero_division=0),
        "f1": f1_score(y_true, flagged, zero_division=0),
        "average_precision": average_precision_score(y_true, score),
        "roc_auc": roc_auc_score(y_true, score),
    })
display(pd.DataFrame(rows).set_index("model").round(3))
```

Use known labels only for evaluation, not fitting an unsupervised detector. With rare anomalies, inspect average precision and the number of alerts, not accuracy alone. Scores from different models have different scales and meanings.

## 21–22. Thresholds and alert budget

For models that provide a continuous score, choose the threshold on a validation set. For a target recall, find the threshold achieving the target while maximizing precision among eligible thresholds. For a limited alert budget, rank validation examples by anomaly score and inspect the top `k` alerts. Estimate precision at `k` from validation labels, then freeze the policy before testing.

Example for Isolation Forest scores (larger means more anomalous after negation):

```python
from sklearn.metrics import precision_recall_curve

# Fit only on training data; compute validation scores with the same fitted model.
iso = IsolationForest(contamination="auto", random_state=42)
iso.fit(X_train)
val_score = -iso.decision_function(X_val)  # X_val must be a held-out validation set
precision, recall, thresholds = precision_recall_curve(y_val, val_score)
```

Do not pick a threshold from the final test labels.

## 23. Score interpretation

For Isolation Forest, `decision_function` is negative for predicted outliers and non-negative for inliers. Negating it creates a convenient score where larger values are more anomalous. Check each estimator's documentation before interpreting score direction.

## 24–26. Preprocessing and time series

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import OneClassSVM

model = make_pipeline(StandardScaler(), OneClassSVM(nu=0.03, gamma="scale"))
model.fit(X_normal_train)
```

A pipeline fits scaling on training data and applies the same transformation to later data. For time series, split chronologically, create rolling features using only past values, and evaluate on a future window. Random splits can leak future behavior into training.

## 27–31. Review, stability, dimensions, contamination, drift

- Review alerts with domain experts and record outcomes; do not automatically remove unusual observations.
- For stability, compare flagged sets across seeds using Jaccard similarity: intersection size divided by union size.
- In high dimensions, distance concentration and irrelevant features can weaken neighborhood methods. Use domain-informed features, dimensionality reduction fit on training data, or simpler models.
- Contaminated training data can cause anomalies to be learned as normal. Use trusted normal subsets, robust methods, and iterative review where justified.
- Monitor feature distributions, score distributions, alert volume, reviewer outcomes, and data pipeline health. Recalibrate thresholds when the operational environment changes.

## 32–35. Cybersecurity, fraud, deployment, and explanations

For login anomalies, useful features may include time-of-day deviation, new device, failed-login bursts, impossible travel indicators, ASN changes, and per-account frequency. Avoid using sensitive attributes without a legitimate and lawful basis. Combine risk scores with rules and human review; record why the alert was raised and what happened after investigation.

For fraud, an anomaly score prioritizes investigation; it does not establish intent or confirm fraud. Deployment should define latency, throughput, alert priority, audit logs, access control, rollback, and fallback behavior. Explanations should describe observed deviations, not claim causal certainty.

## 36–37. Mini-project templates

1. Generate clustered normal points and injected anomalies.
2. Fit Isolation Forest, LOF, and One-Class SVM.
3. Compare precision, recall, F1, average precision, and alert counts using known synthetic labels.
4. Visualize flagged points and inspect false positives/negatives.
5. For novelty detection, fit only on normal training examples and evaluate on unseen mixed data.
6. Document threshold selection, assumptions, compute cost, and limitations.

## 38–40. Review and challenge

Common mistakes include treating every outlier as bad data, using the wrong LOF API, ignoring feature scaling, selecting thresholds on test labels, and trusting a contamination setting as ground truth. An alert triage report should include record ID, score, threshold, model version, key feature deviations, analyst outcome, and follow-up action.
