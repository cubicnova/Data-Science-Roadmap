# Solutions - Classification

These are reference solutions. Results may vary slightly across Scikit-learn versions, but the workflow should remain reproducible.

## Part A - Concepts (1–10)

1. Classification predicts discrete labels; regression predicts numeric values.
2. Binary: spam/not spam. Multiclass: one of several flower species. Multilabel: a news article may have both `sports` and `business` tags.
3. Features (`X`) are input variables; the target (`y`) is the label the model learns to predict.
4. Training data fits the model; held-out test data estimates performance on unseen examples.
5. Stratification preserves approximate class proportions in each split, when feasible.
6. TP: positive correctly predicted positive. TN: negative correctly predicted negative. FP: negative incorrectly predicted positive. FN: positive incorrectly predicted negative.
7. Accuracy is overall correct predictions; precision is the proportion of predicted positives that are correct; recall is the proportion of actual positives detected; F1 is the harmonic mean of precision and recall.
8. If 99% of cases are negative, a model that always predicts negative can achieve 99% accuracy while detecting no positives.
9. A lower threshold typically labels more cases positive, often increasing recall and false positives. A higher threshold typically predicts fewer positives, often improving precision while reducing recall.
10. Repeatedly using the test set to make decisions indirectly tunes to it, weakening its role as an independent estimate.

## Part B - Choose an Algorithm or Metric (11–16)

11. Logistic Regression is a common simple baseline; a `DummyClassifier` is also useful as a no-skill baseline.
12. k-Nearest Neighbors; consider scaling numeric features with `StandardScaler` or another suitable transformer.
13. Report precision, recall, F1, confusion matrix, and possibly average precision / PR-AUC and ROC-AUC. Consider the operational cost of false alarms and missed fraud.
14. Recall/sensitivity is important for minimizing missed positive cases. Monitor false positives, precision, and the burden of follow-up tests.
15. Precision for the spam class is important to limit legitimate emails incorrectly marked as spam.
16. Overfitting, leakage, a non-representative split, distribution shift, or low-quality labels can all cause this.

## Part C - Iris Multiclass Classification (17–23)

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.dummy import DummyClassifier
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

iris = load_iris()
X, y = iris.data, iris.target

print("Shape:", X.shape)
print("Features:", iris.feature_names)
print("Classes:", iris.target_names)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)

models = {
    "Logistic Regression": LogisticRegression(max_iter=1000, random_state=42),
    "Decision Tree": DecisionTreeClassifier(random_state=42),
    "Most-frequent baseline": DummyClassifier(strategy="most_frequent"),
}

for name, model in models.items():
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    print("\\n", name)
    print("Accuracy:", accuracy_score(y_test, pred))
    print("Confusion matrix:\\n", confusion_matrix(y_test, pred))
    print(classification_report(
        y_test, pred, target_names=iris.target_names, zero_division=0
    ))
```

The Iris dataset has 150 observations, four features, and three classes. The split contains 120 training observations and 30 test observations. Exact model scores can depend on estimator settings and library version. A single split is only one sample of the evaluation process; cross-validation gives a more robust view of variability.

## Part D - Binary Classification and Metrics (24–30)

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, roc_auc_score, precision_recall_curve
)

X, y = make_classification(
    n_samples=1000,
    n_features=20,
    n_informative=8,
    n_redundant=4,
    weights=[0.85, 0.15],
    random_state=42,
)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)

model = RandomForestClassifier(n_estimators=200, random_state=42)
model.fit(X_train, y_train)

pred = model.predict(X_test)
proba = model.predict_proba(X_test)[:, 1]

print("Accuracy:", accuracy_score(y_test, pred))
print("Precision:", precision_score(y_test, pred, zero_division=0))
print("Recall:", recall_score(y_test, pred, zero_division=0))
print("F1:", f1_score(y_test, pred, zero_division=0))
print("Confusion matrix:\\n", confusion_matrix(y_test, pred))
print("ROC-AUC:", roc_auc_score(y_test, proba))

precision, recall, thresholds = precision_recall_curve(y_test, proba)
print("Number of threshold points:", len(thresholds))
```

A sound threshold-selection workflow is to create a validation split or use cross-validation within training data, choose a threshold according to error costs or a chosen metric, freeze that choice, then evaluate once on the held-out test set. The test set should not be used repeatedly to select thresholds.

## Part E - Model Comparison and Imbalance (31–35)

```python
import pandas as pd
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

X, y = make_classification(
    n_samples=1000, n_features=20, n_informative=8, n_redundant=4,
    weights=[0.85, 0.15], random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

models = {
    "Logistic Regression": LogisticRegression(max_iter=1000, class_weight="balanced"),
    "Decision Tree": DecisionTreeClassifier(random_state=42, class_weight="balanced"),
    "Random Forest": RandomForestClassifier(
        n_estimators=200, random_state=42, class_weight="balanced"
    ),
}

rows = []
for name, model in models.items():
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    rows.append({
        "model": name,
        "accuracy": accuracy_score(y_test, pred),
        "precision": precision_score(y_test, pred, zero_division=0),
        "recall": recall_score(y_test, pred, zero_division=0),
        "f1": f1_score(y_test, pred, zero_division=0),
    })

display(pd.DataFrame(rows).set_index("model").round(3))
```

Class weighting can improve minority-class recall but may reduce precision. The effect is dataset-dependent. Resampling must be performed on training data only; if cross-validating, resampling should occur inside each training fold, commonly via a compatible pipeline. ROC-AUC summarizes ranking over thresholds; precision-recall analysis focuses on positive-class retrieval and can be especially informative when positives are rare.

## Part F - Mini-Projects (36–40)

36. A strong Iris report includes a reproducible split, baseline, at least two models, accuracy and per-class metrics, confusion matrix, and a discussion of small-dataset limitations.
37. A spam pipeline could tokenize text, convert text to features (for example TF-IDF), and train a linear classifier. Evaluate precision and recall for the spam class, and inspect false positives because legitimate messages wrongly filtered may be costly.
38. For fraud detection, use time- or group-aware splitting if it better reflects deployment, compare against a baseline, report precision, recall, F1, PR-AUC, and confusion matrix, and consider the investigation capacity and financial impact of errors.
39. Select thresholds on validation predictions:

```python
import pandas as pd
from sklearn.metrics import precision_score, recall_score, f1_score

threshold_rows = []
for threshold in [0.30, 0.50, 0.70]:
    threshold_pred = (validation_probabilities >= threshold).astype(int)
    threshold_rows.append({
        "threshold": threshold,
        "precision": precision_score(y_validation, threshold_pred, zero_division=0),
        "recall": recall_score(y_validation, threshold_pred, zero_division=0),
        "f1": f1_score(y_validation, threshold_pred, zero_division=0),
    })
display(pd.DataFrame(threshold_rows))
```

Here `validation_probabilities` and `y_validation` must come from a validation split that was not used to fit the final model. The snippet illustrates the process; define those variables in your own experiment.
40. The report should cover the problem definition, dataset provenance, label quality, split design, baseline, model, suitable metrics, error analysis, limitations, and deployment considerations.

## Good Practices

- Never select a model based on test performance alone after trying many alternatives.
- Use stratification when suitable, but use group- or time-aware splitting when the data structure requires it.
- Consider probability calibration when probabilities inform important decisions.
- Document class definitions, metric choices, and threshold rationale.
