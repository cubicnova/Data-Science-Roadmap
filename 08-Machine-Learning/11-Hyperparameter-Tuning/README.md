# 11 - Hyperparameter Tuning

Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap), this module teaches how to improve machine learning models by selecting suitable hyperparameters using reliable validation strategies.

## Learning Objectives

By the end of this module, you should be able to:
- Distinguish model parameters from hyperparameters.
- Explain why hyperparameter tuning must use training/validation data, not the final test set.
- Use `GridSearchCV` and `RandomizedSearchCV`.
- Build leakage-safe tuning workflows with `Pipeline`.
- Choose suitable cross-validation strategies.
- Interpret `best_params_`, `best_score_`, and `cv_results_`.
- Understand search-space design, scoring metrics, compute cost, and reproducibility.
- Recognize overfitting to validation folds and avoid tuning leakage.
- Compare a tuned model with a simple baseline.

## Prerequisites

- Python, pandas, NumPy, and Scikit-learn.
- Familiarity with classification, regression, model evaluation, and preprocessing.
- Recommended: modules `03-Data-Preprocessing`, `06-Classification`, and `09-Model-Evaluation`.

## Setup

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Run `examples.ipynb` from top to bottom. The notebook uses datasets bundled with Scikit-learn or generated locally.

## Parameters vs. Hyperparameters

- **Model parameters** are learned from data during fitting, such as coefficients in Linear Regression or split thresholds in a decision tree.
- **Hyperparameters** are configuration choices set before or around fitting, such as tree depth, number of trees, regularization strength, or the number of neighbors.

Hyperparameter tuning searches for configurations that perform well under a chosen validation procedure and scoring metric.

## Common Hyperparameter Search Methods

### Grid Search

`GridSearchCV` evaluates every combination in a specified parameter grid using cross-validation.

**Advantages:** systematic and easy to interpret for a small search space.

**Limitations:** can become expensive as the number of parameters and values grows; it can waste trials on unimportant combinations.

```python
from sklearn.model_selection import GridSearchCV
from sklearn.tree import DecisionTreeClassifier

param_grid = {
    "max_depth": [2, 3, 5, None],
    "min_samples_split": [2, 5, 10],
}
search = GridSearchCV(
    DecisionTreeClassifier(random_state=42),
    param_grid=param_grid,
    cv=5,
    scoring="accuracy",
    n_jobs=-1,
)
search.fit(X_train, y_train)
print(search.best_params_)
```

### Randomized Search

`RandomizedSearchCV` samples a fixed number of configurations from specified distributions or lists. It can explore broad spaces more efficiently than a full grid.

```python
from scipy.stats import randint
from sklearn.model_selection import RandomizedSearchCV
from sklearn.ensemble import RandomForestClassifier

param_distributions = {
    "n_estimators": randint(100, 501),
    "max_depth": [None, 5, 10, 20],
    "min_samples_split": randint(2, 16),
}
search = RandomizedSearchCV(
    RandomForestClassifier(random_state=42),
    param_distributions=param_distributions,
    n_iter=20,
    cv=5,
    scoring="accuracy",
    random_state=42,
    n_jobs=-1,
)
search.fit(X_train, y_train)
```

### Other Approaches

- **Manual search:** useful for a small number of interpretable choices.
- **Halving search:** allocates resources progressively; requires appropriate Scikit-learn APIs and careful resource choices.
- **Bayesian optimization:** uses previous trial results to propose promising configurations; often supplied by additional libraries.
- **Nested cross-validation:** estimates the generalization performance of the entire tuning procedure by separating inner model selection from outer evaluation.

## A Safe Tuning Workflow

1. Define the objective and select a scoring metric based on the real task.
2. Reserve a final test set before tuning.
3. Build preprocessing and estimator steps into a `Pipeline`.
4. Choose a CV strategy matching the data: stratified, group-aware, or time-aware as appropriate.
5. Define a reasonable search space and computational budget.
6. Run `GridSearchCV` or `RandomizedSearchCV` on training data only.
7. Inspect `best_params_`, `best_score_`, and `cv_results_`.
8. Evaluate the selected estimator on the untouched test set once.
9. Compare with a baseline and document variability and limitations.

## Pipeline Parameter Names

For a pipeline with steps named `preprocessor` and `classifier`, parameters are referenced with double underscores:

```python
param_grid = {
    "classifier__C": [0.1, 1.0, 10.0],
    "classifier__class_weight": [None, "balanced"],
}
```

Pipeline tuning is valuable because each CV fold fits preprocessing only on that fold's training partition.

## Scoring Metrics

Choose a metric before searching:

- Classification: `accuracy`, `balanced_accuracy`, `f1`, `precision`, `recall`, `roc_auc`, `average_precision`.
- Regression: `neg_mean_absolute_error`, `neg_root_mean_squared_error`, `r2`.

Scikit-learn uses a “higher is better” convention for search scores. Loss metrics are often exposed with a `neg_` prefix, so a more negative value corresponds to a larger loss; convert scores to positive errors when reporting them.

## Search-Space Design

- Search a small, meaningful range first.
- Use log-spaced values for parameters that operate across orders of magnitude, such as regularization strength `C`.
- Avoid combinations that are invalid or incompatible.
- Use `random_state` where supported for reproducibility.
- Account for the number of candidates × CV folds × model fit cost.
- Do not assume the best CV score will be the best result on every future dataset.

## Common Mistakes

- Tuning on the final test set.
- Preprocessing the entire dataset before cross-validation.
- Choosing a metric that does not match the application.
- Searching huge grids without a compute budget.
- Comparing models using different data splits or metrics.
- Reporting only `best_score_` without test performance or context.
- Treating the highest CV score as a guarantee of future performance.

## Repository Contents

```text
11-Hyperparameter-Tuning/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Further Reading

- [Scikit-learn: Tuning the hyper-parameters of an estimator](https://scikit-learn.org/stable/modules/grid_search.html)
- [Scikit-learn: GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)
- [Scikit-learn: RandomizedSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)
- [Scikit-learn: Nested versus non-nested cross-validation](https://scikit-learn.org/stable/auto_examples/model_selection/plot_nested_cross_validation_iris.html)
