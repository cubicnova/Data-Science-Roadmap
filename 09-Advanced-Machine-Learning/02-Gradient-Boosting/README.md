# Gradient Boosting

Learn how **Gradient Boosting** combines many small decision trees into a strong predictive model. This module covers the intuition, training workflow, key hyperparameters, evaluation, and practical exercises using scikit-learn.

## Learning Objectives

By the end of this module, you should be able to:

- Explain boosting and how gradient boosting differs from bagging.
- Describe the role of the loss function and negative gradient (residual-like signal).
- Train gradient-boosting models for classification and regression.
- Tune `n_estimators`, `learning_rate`, tree depth, and `subsample`.
- Evaluate models with appropriate metrics and diagnose overfitting.
- Interpret feature importance while recognizing its limitations.

## Contents

- [`examples.ipynb`](examples.ipynb) — guided, runnable examples.
- [`practice.md`](practice.md) — exercises to complete independently.
- [`solution.md`](solution.md) — worked solutions and explanations.

## 1. What Is Gradient Boosting?

Gradient Boosting is an ensemble technique that builds models **sequentially**. Each new weak learner (usually a shallow decision tree) is trained to improve the current ensemble by reducing a chosen loss function.

A simplified additive model is:

\[
F_m(x) = F_{m-1}(x) + \eta h_m(x)
\]

where:

- \(F_m(x)\) is the ensemble after iteration \(m\).
- \(h_m(x)\) is the next tree's contribution.
- \(\eta\) is the learning rate (shrinkage).

For squared-error regression, the next tree is related to the residuals. For other losses, the algorithm uses the negative gradient of the loss with respect to the current predictions.

## 2. How It Works

1. Start with a simple initial prediction.
2. Calculate how the current predictions differ from the targets according to the selected loss.
3. Fit a small tree to the direction that most reduces that loss.
4. Add the tree's contribution, scaled by the learning rate.
5. Repeat for the configured number of boosting stages.
6. Evaluate on held-out data and tune the model without using the test set for repeated decisions.

## 3. Gradient Boosting vs. Random Forest

| Aspect | Gradient Boosting | Random Forest |
|---|---|---|
| Training strategy | Sequential; later trees correct the current ensemble | Trees are trained independently |
| Main idea | Reduce loss stage by stage | Average/vote across diverse trees |
| Common controls | Learning rate, number of stages, tree depth, subsampling | Number of trees, feature sampling, tree depth |
| Typical risk | Can overfit if too complex or poorly tuned | Can overfit, but averaging often reduces variance |
| Parallelism | Boosting stages depend on earlier stages | Trees can be trained in parallel |

Neither method is universally best. Compare them using the same split and suitable metrics.

## 4. Important Hyperparameters

| Parameter | Meaning | Practical guidance |
|---|---|---|
| `n_estimators` | Number of boosting stages/trees | More stages can improve fit but increase training time and overfitting risk |
| `learning_rate` | Contribution of each stage | Smaller values often need more trees; tune jointly with `n_estimators` |
| `max_depth` | Maximum depth of each tree | Shallow trees limit interaction complexity and can regularize |
| `min_samples_leaf` | Minimum samples allowed in a leaf | Larger values create smoother, less complex trees |
| `subsample` | Fraction of training rows sampled per stage | Values below 1.0 add stochasticity and can reduce variance |
| `max_features` | Features considered for splits | Can add regularization; assess empirically |
| `loss` | Objective the model optimizes | Choose according to the task and target distribution |

## 5. Classification and Regression

- **Classification:** `sklearn.ensemble.GradientBoostingClassifier`
- **Regression:** `sklearn.ensemble.GradientBoostingRegressor`

For larger datasets, scikit-learn also provides histogram-based variants: `HistGradientBoostingClassifier` and `HistGradientBoostingRegressor`. They can be much faster on datasets with roughly 10,000 or more rows and support missing values natively.

## 6. Evaluation

For classification, consider accuracy, precision, recall, F1-score, ROC-AUC, and a confusion matrix. For imbalanced classes, do not rely on accuracy alone.

For regression, consider MAE, RMSE, and \(R^2\). Compare training and validation performance to identify overfitting.

## 7. Best Practices

- Split data before fitting models; keep the test set for final evaluation.
- Fit preprocessing only on training data (use a pipeline when needed).
- Tune learning rate and number of estimators together.
- Use cross-validation for robust model selection.
- Set `random_state` for reproducible experiments.
- Inspect feature importance cautiously; impurity-based importance can favor certain feature types and does not establish causation.
- Record the baseline, evaluation metric, parameters, and random seed.

## 8. Setup

Python 3.9+ is recommended. Install dependencies:

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
```

Launch the notebook:

```bash
jupyter notebook examples.ipynb
```

## References

- [scikit-learn: Ensemble methods and gradient-boosted trees](https://scikit-learn.org/stable/modules/ensemble.html)
- [GradientBoostingClassifier API](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html)
- [HistGradientBoostingClassifier API](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html)
