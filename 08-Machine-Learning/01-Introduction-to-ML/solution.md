# Solutions - Introduction to Machine Learning

These are reference solutions. Your wording, code structure, and justified choices may differ.

## Part A: Concepts

1. **Machine learning** uses data to learn patterns that can be applied to predictions or discovering structure in new observations.
2. **Classification:** spam detection; predicting whether a transaction is fraudulent. **Regression:** predicting house price; estimating delivery duration.
3. **Supervised learning** trains from examples with known targets. **Unsupervised learning** looks for structure in data without supplied target labels.
4. **Reinforcement learning** trains an agent through actions and rewards. A game-playing agent is one example.
5. Features: `area_sqft`, `bedrooms`. Target: `price`.
6. An **algorithm** is the learning procedure; a **model** is the fitted result of applying that procedure to training data.
7. The training set fits the model. The test set estimates performance on unseen data. Repeatedly tuning against the test set makes it less independent.
8. **Overfitting** means fitting training-specific noise or details; **underfitting** means the model is too simple or poorly trained to capture useful patterns.
9. **Data leakage** occurs when information that should not be available to the model influences training or evaluation. For example, scaling the entire dataset before splitting lets test-set statistics influence training preprocessing.
10. Accuracy can hide poor performance on a minority class. Precision, recall, F1, a confusion matrix, and suitable threshold analysis may be more informative.

## Part B: Identify the Task

1. Classification
2. Regression
3. Clustering
4. Reinforcement learning
5. Classification
6. Regression

## Part C: Python and Scikit-learn

### Exercise 1 - Explore a Dataset

```python
from sklearn.datasets import load_iris
import numpy as np

iris = load_iris()
X = iris.data
y = iris.target

print("Samples:", X.shape[0])
print("Features:", X.shape[1])
print("Feature names:", iris.feature_names)
print("Target names:", iris.target_names)
print("First five rows:\n", X[:5])
print("Class counts:", np.bincount(y))
```

Expected shape: 150 samples and 4 features. Each of the three classes has 50 samples.

### Exercise 2 - Train/Test Split

```python
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris

iris = load_iris()
X, y = iris.data, iris.target

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42,
    stratify=y,
)

print("X_train:", X_train.shape)
print("X_test:", X_test.shape)
print("y_train:", y_train.shape)
print("y_test:", y_test.shape)
```

With 150 samples, the split contains 120 training observations and 30 test observations. Stratification aims to preserve the class proportions in both sets.

### Exercise 3 - Train a Classifier

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

iris = load_iris()
X, y = iris.data, iris.target

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)

model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, y_pred))
print("Confusion matrix:\n", confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred, target_names=iris.target_names))
```

The exact metrics depend on the installed library version and split, but the code prints all required evaluation outputs.

### Exercise 4 - Compare with a Baseline

```python
from sklearn.dummy import DummyClassifier
from sklearn.metrics import accuracy_score
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_iris

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
)

baseline = DummyClassifier(strategy="most_frequent")
baseline.fit(X_train, y_train)
baseline_pred = baseline.predict(X_test)

model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)
model_pred = model.predict(X_test)

print("Baseline accuracy:", accuracy_score(y_test, baseline_pred))
print("Logistic Regression accuracy:", accuracy_score(y_test, model_pred))
```

A baseline shows whether a more complex model provides meaningful improvement over a simple strategy. Production suitability also depends on error costs, calibration, fairness, latency, data quality, robustness, and monitoring.

### Exercise 5 - First Regression Model

```python
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, r2_score

diabetes = load_diabetes()
X, y = diabetes.data, diabetes.target

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print("MAE:", mean_absolute_error(y_test, y_pred))
print("R²:", r2_score(y_test, y_pred))
```

- **MAE** is the average absolute difference between actual and predicted target values; lower is generally better when comparing on the same target and test set.
- **R²** compares the model with predicting the test-target mean. A negative score means the model performed worse than that mean-prediction reference on the evaluated data.

## Part D: Mini-Project Guidance

### Mini-Project 1 - Iris Classification

A strong submission should:
1. State that the task is to predict one of three Iris species from four measurements.
2. Use `train_test_split(..., stratify=y, random_state=42)`.
3. Compare a simple baseline with a trained classifier.
4. Report accuracy and class-level metrics.
5. Discuss that this small, clean dataset is educational and may not represent real-world deployment conditions.

### Mini-Project 2 - Regression Evaluation

```python
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.dummy import DummyRegressor
from sklearn.metrics import mean_absolute_error, r2_score

X, y = load_diabetes(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42
)

models = {
    "Mean baseline": DummyRegressor(strategy="mean"),
    "Linear Regression": LinearRegression(),
}

for name, model in models.items():
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    print(name)
    print("  MAE:", round(mean_absolute_error(y_test, pred), 3))
    print("  R²:", round(r2_score(y_test, pred), 3))
```

A useful model should improve on a reasonable baseline on the same held-out data. Interpret the metrics in context rather than relying on a single score.

### Mini-Project 3 - Model Thinking

Answers will vary. For a cybersecurity example, classifying network events as benign or suspicious is a classification task if reliable labels exist. Possible features include protocol, connection duration, byte counts, and selected aggregate traffic statistics. A metric such as recall may be important when missing suspicious events is costly, but precision must also be considered to control false alerts. Risks include label quality, privacy, class imbalance, changing attacker behavior, and leakage from fields that would not be available at inference time.

## Additional Guidance

- Keep the test set untouched until final evaluation.
- For real workflows, consider cross-validation and preprocessing pipelines.
- Do not interpret a high score on a small teaching dataset as proof of production readiness.
- Consult the [Scikit-learn Getting Started guide](https://scikit-learn.org/stable/getting_started.html) for additional examples.
