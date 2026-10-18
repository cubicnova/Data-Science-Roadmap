# Scikit-Learn Cheatsheet

A practical reference for machine learning with Python, covering data preprocessing, supervised learning, unsupervised learning, model evaluation, pipelines, hyperparameter tuning, and model persistence.

Scikit-learn is an open-source Python library for classical machine learning. It provides algorithms and tools for classification, regression, clustering, dimensionality reduction, preprocessing, model selection, and evaluation.

## Table of Contents

- [1. Installation and Setup](#1-installation-and-setup)
- [2. Basic Machine Learning Workflow](#2-basic-machine-learning-workflow)
- [3. Datasets and Data Splitting](#3-datasets-and-data-splitting)
- [4. Classification Algorithms](#4-classification-algorithms)
- [5. Regression Algorithms](#5-regression-algorithms)
- [6. Clustering Algorithms](#6-clustering-algorithms)
- [7. Data Preprocessing](#7-data-preprocessing)
- [8. Encoding Categorical Features](#8-encoding-categorical-features)
- [9. Handling Missing Values](#9-handling-missing-values)
- [10. Feature Engineering and Selection](#10-feature-engineering-and-selection)
- [11. Model Evaluation: Classification](#11-model-evaluation-classification)
- [12. Model Evaluation: Regression](#12-model-evaluation-regression)
- [13. Cross-Validation](#13-cross-validation)
- [14. Hyperparameter Tuning](#14-hyperparameter-tuning)
- [15. Pipelines and ColumnTransformer](#15-pipelines-and-columntransformer)
- [16. Unsupervised Learning and Dimensionality Reduction](#16-unsupervised-learning-and-dimensionality-reduction)
- [17. Imbalanced Datasets](#17-imbalanced-datasets)
- [18. Model Interpretation](#18-model-interpretation)
- [19. Saving and Loading Models](#19-saving-and-loading-models)
- [20. Common Datasets](#20-common-datasets)
- [21. Complete Classification Example](#21-complete-classification-example)
- [22. Complete Regression Example](#22-complete-regression-example)
- [23. Common Mistakes](#23-common-mistakes)
- [24. Quick Reference](#24-quick-reference)
- [25. Practice Exercises](#25-practice-exercises)
- [26. Official Resources](#26-official-resources)

## 1. Installation and Setup

Install Scikit-learn and common data science libraries:

```bash
python -m pip install scikit-learn numpy pandas matplotlib seaborn
```

Import Scikit-learn:

```python
import sklearn

print(sklearn.__version__)
```

Check the installed package:

```bash
python -m pip show scikit-learn
```

Scikit-learn uses the `sklearn` import namespace.

## 2. Basic Machine Learning Workflow

A typical supervised machine learning workflow consists of:

1. Load and inspect the dataset.
2. Separate features (`X`) and target (`y`).
3. Split data into training and testing sets.
4. Preprocess features.
5. Select and train a model.
6. Evaluate predictions on unseen data.
7. Tune hyperparameters using training data and cross-validation.
8. Save the fitted model and its preprocessing steps.
9. Monitor performance after deployment.

### Estimator API

Most Scikit-learn estimators follow a consistent API.

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(X_train, y_train)      # Train
predictions = model.predict(X_test)
```

Important methods:

| Method | Purpose |
|---|---|
| `fit(X, y)` | Learn from training data |
| `predict(X)` | Predict target values |
| `predict_proba(X)` | Predict class probabilities, when supported |
| `decision_function(X)` | Return decision scores, when supported |
| `transform(X)` | Transform features |
| `fit_transform(X)` | Fit a transformer and transform data |
| `score(X, y)` | Return the estimator's default evaluation score |
| `get_params()` | Inspect estimator parameters |
| `set_params()` | Update estimator parameters |

For unsupervised estimators, `fit()` may not require a target. Not every estimator implements every method.

## 3. Datasets and Data Splitting

### Load a built-in dataset

```python
from sklearn.datasets import load_iris

iris = load_iris()

X = iris.data
y = iris.target

print(X.shape)
print(y.shape)
print(iris.feature_names)
print(iris.target_names)
```

Here, `X` contains the features and `y` contains the target labels.

### Split training and testing data

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

Important parameters:

- `test_size=0.2`: reserves 20% of samples for testing.
- `random_state=42`: makes randomized splitting reproducible.
- `stratify=y`: preserves approximate class proportions in classification tasks.

Use stratification when appropriate for classification. For time series, use chronological splits instead of randomly shuffling future observations into the training set.

### Generate synthetic data

```python
from sklearn.datasets import make_classification, make_regression

X_cls, y_cls = make_classification(
    n_samples=500,
    n_features=10,
    n_informative=5,
    random_state=42
)

X_reg, y_reg = make_regression(
    n_samples=500,
    n_features=5,
    noise=10,
    random_state=42
)
```

Synthetic data is useful for learning algorithms and testing workflows.

## 4. Classification Algorithms

Classification predicts discrete categories such as spam/not spam, fraudulent/legitimate, or customer churn/no churn.

### Logistic Regression

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)

predictions = model.predict(X_test)
```

Useful for classification problems, including binary and multiclass classification.

### Decision Tree

```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier(
    max_depth=5,
    random_state=42
)

model.fit(X_train, y_train)
```

Decision trees can capture nonlinear relationships and are relatively easy to interpret.

### Random Forest

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(
    n_estimators=200,
    max_depth=None,
    random_state=42
)

model.fit(X_train, y_train)
```

Random forests combine multiple decision trees to improve generalization.

### Support Vector Machine

```python
from sklearn.svm import SVC

model = SVC(
    kernel="rbf",
    C=1.0,
    probability=True,
    random_state=42
)

model.fit(X_train, y_train)
```

SVMs can perform well on high-dimensional datasets. Scaling numerical features is generally important.

### K-Nearest Neighbors

```python
from sklearn.neighbors import KNeighborsClassifier

model = KNeighborsClassifier(n_neighbors=5)
model.fit(X_train, y_train)
```

KNN predicts using nearby training samples. Feature scaling is usually important.

### Naive Bayes

```python
from sklearn.naive_bayes import GaussianNB

model = GaussianNB()
model.fit(X_train, y_train)
```

Naive Bayes is useful for fast baseline models. Multinomial and Bernoulli variants are often used for suitable discrete features, including text classification.

### Gradient Boosting

```python
from sklearn.ensemble import GradientBoostingClassifier

model = GradientBoostingClassifier(random_state=42)
model.fit(X_train, y_train)
```

Boosting builds an ensemble sequentially, improving the model by focusing on errors from earlier stages.

### Classification algorithm reference

| Algorithm | Common use |
|---|---|
| Logistic Regression | Interpretable classification baseline |
| Decision Tree | Nonlinear rules and decision boundaries |
| Random Forest | Robust nonlinear classification |
| SVM | High-dimensional classification |
| KNN | Similarity-based classification |
| Naive Bayes | Text and probabilistic classification |
| Gradient Boosting | Structured tabular data |
| MLPClassifier | Neural network classification |

Model selection should be based on validation performance, computational cost, interpretability, and the requirements of the problem.

## 5. Regression Algorithms

Regression predicts continuous numerical values, such as prices, revenue, demand, or temperature.

### Linear Regression

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)

predictions = model.predict(X_test)
```

### Ridge Regression

```python
from sklearn.linear_model import Ridge

model = Ridge(alpha=1.0)
model.fit(X_train, y_train)
```

Ridge regression applies L2 regularization to reduce excessively large coefficients.

### Lasso Regression

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=0.1)
model.fit(X_train, y_train)
```

Lasso applies L1 regularization, which can set some coefficients to zero.

### Decision Tree Regressor

```python
from sklearn.tree import DecisionTreeRegressor

model = DecisionTreeRegressor(
    max_depth=5,
    random_state=42
)

model.fit(X_train, y_train)
```

### Random Forest Regressor

```python
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor(
    n_estimators=200,
    random_state=42
)

model.fit(X_train, y_train)
```

### Support Vector Regression

```python
from sklearn.svm import SVR

model = SVR(kernel="rbf", C=1.0, epsilon=0.1)
model.fit(X_train, y_train)
```

SVR is sensitive to feature scales, so scaling is generally recommended.

### Regression algorithm reference

| Algorithm | Common use |
|---|---|
| Linear Regression | Linear relationships |
| Ridge | Regularized linear regression |
| Lasso | Regression with coefficient selection |
| Decision Tree Regressor | Nonlinear relationships |
| Random Forest Regressor | Nonlinear tabular data |
| Gradient Boosting Regressor | Structured prediction tasks |
| SVR | Small or medium-sized datasets |
| MLPRegressor | Neural network regression |

## 6. Clustering Algorithms

Clustering groups similar samples without requiring target labels.

### K-Means

```python
from sklearn.cluster import KMeans

model = KMeans(
    n_clusters=3,
    random_state=42,
    n_init=10
)

labels = model.fit_predict(X)
```

Inspect cluster centers:

```python
print(model.cluster_centers_)
```

K-Means works best when the data is suitable for distance-based clustering. Scaling can be important.

### DBSCAN

```python
from sklearn.cluster import DBSCAN

model = DBSCAN(eps=0.5, min_samples=5)
labels = model.fit_predict(X)
```

DBSCAN can discover density-based clusters and label noise points as `-1`.

### Agglomerative Clustering

```python
from sklearn.cluster import AgglomerativeClustering

model = AgglomerativeClustering(n_clusters=3)
labels = model.fit_predict(X)
```

### Clustering comparison

| Algorithm | Characteristics |
|---|---|
| K-Means | Requires a chosen number of clusters |
| DBSCAN | Density-based; can identify noise |
| Agglomerative Clustering | Hierarchical clustering |
| GaussianMixture | Probabilistic mixture modeling |

Clustering results require interpretation and validation. A cluster label is not automatically a meaningful business category.

## 7. Data Preprocessing

Preprocessing transforms raw features into a representation suitable for a model.

### StandardScaler

Transforms features to approximately zero mean and unit variance based on training data.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Fit the scaler only on training data.

### MinMaxScaler

Scales features to a specified range, commonly 0 to 1.

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### RobustScaler

Uses robust statistics such as the median and interquartile range.

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()
X_train_scaled = scaler.fit_transform(X_train)
```

Useful when outliers could distort conventional scaling.

### Normalizer

Normalizes individual samples rather than standardizing each feature.

```python
from sklearn.preprocessing import Normalizer

normalizer = Normalizer(norm="l2")
X_normalized = normalizer.fit_transform(X_train)
```

### Polynomial features

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(X_train)
```

Polynomial expansion introduces nonlinear combinations of features. It can increase the number of features substantially.

## 8. Encoding Categorical Features

Most conventional Scikit-learn estimators require categorical variables to be encoded numerically.

### OneHotEncoder

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(handle_unknown="ignore")

X_encoded = encoder.fit_transform(
    [["India"], ["USA"], ["India"]]
)
```

One-hot encoding creates indicator features for categories. The output may be sparse.

### OrdinalEncoder

```python
from sklearn.preprocessing import OrdinalEncoder

encoder = OrdinalEncoder(
    handle_unknown="use_encoded_value",
    unknown_value=-1
)

X_encoded = encoder.fit_transform(
    [["Low"], ["Medium"], ["High"]]
)
```

Use ordinal encoding when the categories have a meaningful order and the encoding reflects that order. For nominal categories, arbitrary numeric codes may introduce misleading relationships.

### LabelEncoder

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

y_encoded = encoder.fit_transform(
    ["Cat", "Dog", "Cat", "Bird"]
)
```

`LabelEncoder` is intended primarily for target labels (`y`), not general categorical input features.

## 9. Handling Missing Values

### SimpleImputer

```python
import numpy as np
from sklearn.impute import SimpleImputer

X = np.array([
    [1.0, 10.0],
    [2.0, np.nan],
    [np.nan, 30.0]
])

imputer = SimpleImputer(strategy="mean")
X_imputed = imputer.fit_transform(X)
```

Supported strategies include `mean`, `median`, `most_frequent`, and `constant`.

For categorical features, `most_frequent` or `constant` is often appropriate.

### KNNImputer

```python
from sklearn.impute import KNNImputer

imputer = KNNImputer(n_neighbors=3)
X_imputed = imputer.fit_transform(X)
```

KNN imputation estimates missing values from similar samples. Scaling and feature choice can affect its behavior.

### Missing-value indicator

```python
from sklearn.impute import MissingIndicator

indicator = MissingIndicator(features="all")
missing_flags = indicator.fit_transform(X)
```

Missingness can itself contain useful information. When appropriate, include missingness indicators in the preprocessing pipeline.

## 10. Feature Engineering and Selection

### SelectKBest

```python
from sklearn.feature_selection import SelectKBest, f_classif

selector = SelectKBest(score_func=f_classif, k=5)

X_selected = selector.fit_transform(X_train, y_train)
```

Use `f_classif` for suitable classification tasks with numerical features. For regression, a different scoring function such as `f_regression` may be appropriate.

### VarianceThreshold

```python
from sklearn.feature_selection import VarianceThreshold

selector = VarianceThreshold(threshold=0.0)
X_reduced = selector.fit_transform(X_train)
```

Removes features whose variance does not exceed the specified threshold.

### Mutual information

```python
from sklearn.feature_selection import mutual_info_classif

scores = mutual_info_classif(X_train, y_train, random_state=42)
```

Mutual information can identify nonlinear statistical relationships between features and a target.

### Permutation importance

```python
from sklearn.inspection import permutation_importance

result = permutation_importance(
    model,
    X_test,
    y_test,
    n_repeats=10,
    random_state=42
)

print(result.importances_mean)
```

Permutation importance measures how model performance changes when a feature is shuffled. Compute it on suitable held-out data and interpret correlated features carefully.

Feature selection and engineering must be performed without using held-out test information to fit transformations or select features.

## 11. Model Evaluation: Classification

### Accuracy

```python
from sklearn.metrics import accuracy_score

accuracy = accuracy_score(y_test, predictions)
```

Accuracy is the proportion of correct predictions. It can be misleading for imbalanced datasets.

### Confusion matrix

```python
from sklearn.metrics import confusion_matrix

cm = confusion_matrix(y_test, predictions)
print(cm)
```

For binary classification, with labels ordered as negative and positive:

- True Negative (TN): negative cases predicted negative.
- False Positive (FP): negative cases predicted positive.
- False Negative (FN): positive cases predicted negative.
- True Positive (TP): positive cases predicted positive.

### Precision, recall, and F1-score

```python
from sklearn.metrics import (
    precision_score,
    recall_score,
    f1_score
)

precision = precision_score(
    y_test, predictions, zero_division=0
)

recall = recall_score(
    y_test, predictions, zero_division=0
)

f1 = f1_score(
    y_test, predictions, zero_division=0
)
```

- **Precision:** Of the predicted positive cases, how many were truly positive?
- **Recall:** Of the actual positive cases, how many were detected?
- **F1-score:** Harmonic mean of precision and recall.

For multiclass tasks, specify an appropriate `average`, such as `"macro"` or `"weighted"`.

### Classification report

```python
from sklearn.metrics import classification_report

print(classification_report(y_test, predictions))
```

Reports precision, recall, F1-score, and support for each class.

### ROC-AUC

```python
from sklearn.metrics import roc_auc_score

probabilities = model.predict_proba(X_test)[:, 1]
auc = roc_auc_score(y_test, probabilities)
```

This example assumes binary classification and an estimator that supports `predict_proba()`. ROC-AUC measures ranking discrimination across thresholds, not the quality of probability calibration.

### Log loss

```python
from sklearn.metrics import log_loss

loss = log_loss(y_test, model.predict_proba(X_test))
```

Log loss penalizes incorrect probability estimates, especially confident incorrect predictions.

### Choose metrics for the problem

| Metric | Useful when |
|---|---|
| Accuracy | Classes are reasonably balanced and errors have similar costs |
| Precision | False positives are costly |
| Recall | False negatives are costly |
| F1-score | A balance of precision and recall is needed |
| ROC-AUC | Ranking quality across classification thresholds matters |
| Average precision | Positive-class ranking is important, particularly with imbalance |
| Log loss | Probability quality matters |

## 12. Model Evaluation: Regression

### Mean Absolute Error

```python
from sklearn.metrics import mean_absolute_error

mae = mean_absolute_error(y_test, predictions)
```

MAE measures average absolute prediction error in the target's units.

### Mean Squared Error and RMSE

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, predictions)
rmse = np.sqrt(mse)
```

RMSE penalizes large errors more strongly than MAE.

### R-squared

```python
from sklearn.metrics import r2_score

r2 = r2_score(y_test, predictions)
```

R-squared measures performance relative to predicting the mean of the observed target on the evaluated dataset. It can be negative when a model performs worse than that baseline.

### Mean Absolute Percentage Error

```python
from sklearn.metrics import mean_absolute_percentage_error

mape = mean_absolute_percentage_error(y_test, predictions)
```

MAPE is sensitive to zero and near-zero target values and may be unsuitable for some datasets.

### Regression metric reference

| Metric | Interpretation |
|---|---|
| MAE | Average absolute error |
| MSE | Average squared error |
| RMSE | Square root of average squared error |
| R² | Relative fit compared with a mean baseline |
| MAPE | Relative error measure with limitations around zero |

## 13. Cross-Validation

Cross-validation estimates model performance across multiple data splits.

### K-fold cross-validation

```python
from sklearn.model_selection import KFold, cross_val_score
from sklearn.linear_model import LinearRegression

cv = KFold(n_splits=5, shuffle=True, random_state=42)

scores = cross_val_score(
    LinearRegression(),
    X,
    y,
    cv=cv,
    scoring="r2"
)

print(scores)
print(scores.mean())
```

### Stratified K-fold

```python
from sklearn.model_selection import StratifiedKFold

cv = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

Useful for classification when preserving class proportions across folds matters.

### Cross-validate with multiple metrics

```python
from sklearn.model_selection import cross_validate
from sklearn.linear_model import LogisticRegression

results = cross_validate(
    LogisticRegression(max_iter=1000),
    X,
    y,
    cv=5,
    scoring=["accuracy", "f1_macro"],
    return_train_score=True
)

print(results["test_accuracy"].mean())
print(results["test_f1_macro"].mean())
```

Use a suitable cross-validation strategy for the data. Random K-fold is not appropriate for every time-dependent or grouped dataset.

## 14. Hyperparameter Tuning

Hyperparameters control model behavior before or during fitting. They differ from learned parameters such as regression coefficients.

### GridSearchCV

```python
from sklearn.model_selection import GridSearchCV
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(random_state=42)

param_grid = {
    "n_estimators": [100, 200],
    "max_depth": [None, 5, 10],
    "min_samples_split": [2, 5]
}

search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=5,
    scoring="f1_macro",
    n_jobs=-1
)

search.fit(X_train, y_train)

print(search.best_params_)
print(search.best_score_)

best_model = search.best_estimator_
```

Grid search evaluates every specified parameter combination.

### RandomizedSearchCV

```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint

param_distributions = {
    "n_estimators": randint(100, 501),
    "max_depth": [None, 5, 10, 20],
    "min_samples_split": randint(2, 11)
}

search = RandomizedSearchCV(
    estimator=RandomForestClassifier(random_state=42),
    param_distributions=param_distributions,
    n_iter=20,
    cv=5,
    scoring="f1_macro",
    random_state=42,
    n_jobs=-1
)

search.fit(X_train, y_train)
```

Randomized search samples parameter combinations and can explore large search spaces more efficiently than exhaustive grid search.

### Important attributes

```python
search.best_estimator_
search.best_params_
search.best_score_
search.cv_results_
```

Select models and tune hyperparameters using training data and cross-validation. Keep the test set separate for final evaluation.

## 15. Pipelines and ColumnTransformer

Pipelines combine preprocessing and model training into a single estimator. They also help prevent data leakage by fitting transformations within the relevant training folds.

### Basic pipeline

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000)
)

pipeline.fit(X_train, y_train)
predictions = pipeline.predict(X_test)
```

### Mixed numerical and categorical data

```python
import pandas as pd

from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "income"]
categorical_features = ["city", "occupation"]

numeric_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler())
])

categorical_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore"))
])

preprocessor = ColumnTransformer([
    ("numeric", numeric_pipeline, numeric_features),
    ("categorical", categorical_pipeline, categorical_features)
])

model = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", LogisticRegression(max_iter=1000))
])
```

Train and predict:

```python
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

This example assumes the specified columns exist in the DataFrame and the target has been separated from the features.

### Tune pipeline parameters

```python
param_grid = {
    "classifier__C": [0.1, 1, 10],
    "classifier__solver": ["lbfgs"]
}

search = GridSearchCV(
    model,
    param_grid=param_grid,
    cv=5,
    scoring="accuracy"
)

search.fit(X_train, y_train)
```

Pipeline parameters use the syntax `step_name__parameter_name`.

## 16. Unsupervised Learning and Dimensionality Reduction

### Principal Component Analysis (PCA)

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

pca_pipeline = make_pipeline(
    StandardScaler(),
    PCA(n_components=2)
)

X_reduced = pca_pipeline.fit_transform(X_train)
```

PCA creates new orthogonal features that capture variance. Scaling is often appropriate when original features have different units.

### Inspect explained variance

```python
pca = PCA(n_components=0.95)
X_reduced = pca.fit_transform(X_train_scaled)

print(pca.n_components_)
print(pca.explained_variance_ratio_)
```

A floating-point `n_components` between 0 and 1 selects enough components to retain the specified proportion of variance for supported solvers.

### Silhouette score

```python
from sklearn.metrics import silhouette_score
from sklearn.cluster import KMeans

labels = KMeans(
    n_clusters=3,
    random_state=42,
    n_init=10
).fit_predict(X)

score = silhouette_score(X, labels)
```

The silhouette score measures how well-separated and cohesive clusters are. It requires at least two clusters and fewer clusters than samples.

## 17. Imbalanced Datasets

When one class is much more common than another, accuracy may not reflect performance on the minority class.

### Class weighting

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000
)
```

`class_weight="balanced"` adjusts class weights inversely according to class frequencies for supported estimators.

### Use suitable metrics

```python
from sklearn.metrics import (
    balanced_accuracy_score,
    average_precision_score,
    classification_report
)

print(balanced_accuracy_score(y_test, predictions))
print(classification_report(y_test, predictions))
```

For binary classification:

```python
probabilities = model.predict_proba(X_test)[:, 1]
print(average_precision_score(y_test, probabilities))
```

### Resampling

Techniques such as oversampling, undersampling, and SMOTE can help address class imbalance. Resampling must be performed only within training folds, never on the entire dataset before splitting.

For imbalanced learning pipelines, the `imbalanced-learn` package provides compatible tools such as `SMOTE` and `imblearn.pipeline.Pipeline`.

## 18. Model Interpretation

### Linear model coefficients

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

print(model.coef_)
print(model.intercept_)
```

Coefficient interpretation depends on feature scaling, encoding, and the model's formulation.

### Tree feature importance

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(random_state=42)
model.fit(X_train, y_train)

importance = model.feature_importances_
print(importance)
```

Impurity-based feature importance can be biased toward certain features, including high-cardinality features. Consider permutation importance and domain knowledge as complementary tools.

### Partial dependence

```python
from sklearn.inspection import PartialDependenceDisplay
import matplotlib.pyplot as plt

PartialDependenceDisplay.from_estimator(
    model,
    X_test,
    features=[0, 1]
)

plt.show()
```

Partial dependence plots illustrate the average model response as selected features vary. Interpret them cautiously when features are strongly correlated.

## 19. Saving and Loading Models

### Save with joblib

```python
import joblib

joblib.dump(model, "model.joblib")
```

### Load the model

```python
model = joblib.load("model.joblib")
predictions = model.predict(X_test)
```

### Save the full pipeline

Save the fitted preprocessing and model pipeline together rather than saving only the final estimator.

```python
joblib.dump(model, "complete_pipeline.joblib")
```

The saved object should include all preprocessing steps required for inference.

**Security note:** Never load pickle- or joblib-based model files from untrusted sources. Loading can execute arbitrary code. Keep track of library versions, training data provenance, and model artifacts.

## 20. Common Datasets

Scikit-learn provides sample datasets for experimentation.

### Classification

```python
from sklearn.datasets import load_iris, load_wine, load_breast_cancer

iris = load_iris()
wine = load_wine()
cancer = load_breast_cancer()
```

### Regression

```python
from sklearn.datasets import load_diabetes, make_regression

diabetes = load_diabetes()

X, y = make_regression(
    n_samples=200,
    n_features=5,
    noise=10,
    random_state=42
)
```

### Clustering

```python
from sklearn.datasets import make_blobs

X, labels = make_blobs(
    n_samples=300,
    centers=4,
    random_state=42
)
```

Some datasets are synthetic or simplified educational examples. Check dataset documentation before using a dataset for real-world conclusions.

## 21. Complete Classification Example

This example trains a Logistic Regression model on the Iris dataset using a preprocessing pipeline.

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import (
    accuracy_score,
    classification_report,
    confusion_matrix
)

# Load data
X, y = load_iris(return_X_y=True)

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)

# Build pipeline
model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000)
)

# Train
model.fit(X_train, y_train)

# Predict
predictions = model.predict(X_test)

# Evaluate
print("Accuracy:", accuracy_score(y_test, predictions))
print("Confusion matrix:\n", confusion_matrix(y_test, predictions))
print(classification_report(y_test, predictions))
```

The test set is used only for evaluation after fitting. The scaler learns its statistics from the training set through the pipeline.

## 22. Complete Regression Example

This example predicts a continuous target using synthetic regression data.

```python
import numpy as np

from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_absolute_error, r2_score

# Generate data
X, y = make_regression(
    n_samples=500,
    n_features=5,
    noise=15,
    random_state=42
)

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

# Build pipeline
model = make_pipeline(
    StandardScaler(),
    Ridge(alpha=1.0)
)

# Train
model.fit(X_train, y_train)

# Predict
predictions = model.predict(X_test)

# Evaluate
print("MAE:", mean_absolute_error(y_test, predictions))
print("R2:", r2_score(y_test, predictions))
```

For a real regression problem, select metrics based on the target scale and the relative cost of different prediction errors.

## 23. Common Mistakes

| Mistake | Better practice |
|---|---|
| Training and testing on the same data | Keep a held-out test set |
| Scaling before splitting data | Fit preprocessing on training data only |
| Selecting features using the full dataset | Perform feature selection inside training folds |
| Ignoring class imbalance | Examine per-class metrics and suitable baselines |
| Using accuracy alone | Evaluate precision, recall, F1, or other relevant metrics |
| Using random splits for time series | Use chronological validation |
| Forgetting feature scaling | Scale when required by the algorithm |
| Tuning hyperparameters on the test set | Tune using cross-validation on training data |
| Ignoring missing values | Add suitable imputation steps |
| Saving only the estimator | Persist the entire preprocessing pipeline |
| Assuming feature importance proves causality | Treat importance as model-specific association |
| Loading untrusted model artifacts | Load only trusted serialized files |

### Data leakage

Data leakage occurs when information unavailable at prediction time influences model training or evaluation.

Common causes include:

- Fitting a scaler on the entire dataset before splitting.
- Selecting features using test labels.
- Oversampling before train/test splitting.
- Using future information to predict the past.
- Using information created after the target event.

Use pipelines and a correctly designed validation strategy to minimize leakage.

## 24. Quick Reference

| Task | Scikit-learn API |
|---|---|
| Load Iris dataset | `load_iris()` |
| Split data | `train_test_split()` |
| Standardize features | `StandardScaler()` |
| Scale to a range | `MinMaxScaler()` |
| Encode categories | `OneHotEncoder()` |
| Impute missing values | `SimpleImputer()` |
| Train classifier | `model.fit(X_train, y_train)` |
| Predict labels | `model.predict(X_test)` |
| Predict probabilities | `model.predict_proba(X_test)` |
| Evaluate accuracy | `accuracy_score()` |
| Generate classification report | `classification_report()` |
| Evaluate regression | `mean_absolute_error()` |
| Cross-validation | `cross_val_score()` |
| Tune parameters | `GridSearchCV()` |
| Randomized tuning | `RandomizedSearchCV()` |
| Combine steps | `Pipeline()` |
| Transform selected columns | `ColumnTransformer()` |
| Reduce dimensions | `PCA()` |
| Cluster data | `KMeans()` |
| Inspect feature importance | `permutation_importance()` |
| Save model | `joblib.dump()` |
| Load model | `joblib.load()` |

## 25. Practice Exercises

1. Train Logistic Regression on the Iris dataset and calculate accuracy.
2. Compare Decision Tree and Random Forest classification performance.
3. Train a Linear Regression model and calculate MAE and R².
4. Compare StandardScaler and MinMaxScaler.
5. Impute missing values in a dataset using SimpleImputer.
6. Encode categorical features with OneHotEncoder.
7. Build a Pipeline containing preprocessing and classification.
8. Evaluate a model using five-fold cross-validation.
9. Tune Random Forest hyperparameters with GridSearchCV.
10. Compare precision, recall, and F1-score on an imbalanced classification dataset.
11. Use PCA to reduce the number of features.
12. Apply K-Means clustering and evaluate the resulting clusters.
13. Calculate permutation feature importance.
14. Save a fitted pipeline and load it for prediction.
15. Build an end-to-end machine learning project with a clean training and evaluation workflow.

### Mini Project: Customer Churn Prediction

Build a classification pipeline that predicts whether a customer is likely to leave a service.

Requirements:

- Load a tabular dataset containing numerical and categorical features.
- Identify the target and potential data leakage.
- Split the data into training and testing sets.
- Impute missing values.
- Encode categorical variables.
- Scale numerical variables where appropriate.
- Train Logistic Regression and Random Forest models.
- Compare precision, recall, F1-score, and ROC-AUC where suitable.
- Tune hyperparameters using cross-validation.
- Save the best fitted pipeline.
- Document model limitations and potential monitoring metrics.

Use a real dataset with an appropriate license and avoid exposing personal or sensitive customer information.

## 26. Official Resources

- [Scikit-learn Official Website](https://scikit-learn.org/)
- [Getting Started](https://scikit-learn.org/stable/getting_started.html)
- [User Guide](https://scikit-learn.org/stable/user_guide.html)
- [API Reference](https://scikit-learn.org/stable/api/index.html)
- [Choosing the Right Estimator](https://scikit-learn.org/stable/machine_learning_map.html)
- [Common Pitfalls and Recommended Practices](https://scikit-learn.org/stable/common_pitfalls.html)
- [Model Evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Pipelines and Composite Estimators](https://scikit-learn.org/stable/modules/compose.html)
- [Scikit-learn Examples](https://scikit-learn.org/stable/auto_examples/index.html)

---

**Recommended learning sequence:** Python → NumPy → Pandas → statistics → data preprocessing → supervised learning → model evaluation → cross-validation → hyperparameter tuning → pipelines → unsupervised learning → model interpretation → deployment.


