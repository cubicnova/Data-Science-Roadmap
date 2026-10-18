# 09 - Model Evaluation

Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).

This module covers reliable evaluation of classification and regression models, choosing suitable metrics, cross-validation, baselines, learning curves, and avoiding data leakage.

## Learning Objectives
- Separate training, validation, and test data correctly.
- Choose classification and regression metrics for a task.
- Interpret confusion matrices, precision, recall, F1, ROC-AUC, MAE, RMSE, and R².
- Compare models with baselines and cross-validation.
- Recognize overfitting, underfitting, leakage, and poor split design.
- Communicate errors, limitations, and uncertainty.

## Setup
```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```
Run `examples.ipynb` from top to bottom. It uses built-in or generated datasets.

## Training, Validation, and Test Sets
- **Training:** fit model parameters and preprocessing.
- **Validation:** compare models, tune hyperparameters and choose thresholds.
- **Test:** final held-out estimate, used after decisions are made.

For small datasets, cross-validation can help. Keep the test set untouched during model selection. Use stratified splits for classification when appropriate, group-aware splits for related entities, and time-based splits when predicting the future from the past.

## Classification Metrics
A confusion matrix has true labels in rows and predicted labels in columns.

- **Accuracy:** proportion of predictions correct; can mislead on imbalanced datasets.
- **Precision:** among predicted positives, proportion that are truly positive.
- **Recall/sensitivity:** among actual positives, proportion detected.
- **F1:** harmonic mean of precision and recall.
- **Specificity:** among actual negatives, proportion correctly identified.
- **ROC-AUC:** ranking quality across thresholds; not calibration or performance at one chosen threshold.
- **Average precision / PR curve:** useful when positive cases are rare.
- **Log loss:** evaluates predicted probability quality and penalizes confident errors.

Choose metrics based on the cost of false positives and false negatives. For rare positive classes, report more than accuracy.

## Regression Metrics
| Metric | Meaning | Caution |
|---|---|---|
| MAE | Average absolute error, in target units | Does not penalize large errors as strongly as squared error |
| MSE | Average squared error | Units are squared |
| RMSE | Square root of MSE, in target units | Sensitive to large errors |
| R² | Relative to predicting the evaluation-set target mean | Can be negative |
| MAPE | Average absolute percentage error | Problematic at or near zero actual values |

Lower MAE/RMSE is generally better on the same evaluation data; higher R² is generally better.

## Cross-Validation and Baselines
Cross-validation repeats fitting/evaluation over folds and helps compare candidates. It does not fix leakage or guarantee future performance. Put preprocessing in a `Pipeline` so every fold learns transformations only from its training portion.

Compare against simple baselines such as `DummyClassifier` and `DummyRegressor`. Inspect class-wise errors, residuals, subgroup performance, and score variability rather than relying on one aggregate number.

## Overfitting and Learning Curves
- **Overfitting:** training score is much better than validation score.
- **Underfitting:** training and validation performance are both poor.
- **Learning curve:** scores vs. training-set size.
- **Validation curve:** scores vs. a hyperparameter.

## Recommended Workflow
1. Define the task and error costs.
2. Select a split strategy matching the data generation process.
3. Establish a baseline.
4. Fit preprocessing and models only on training data.
5. Select models using cross-validation on training data.
6. Select thresholds on validation data or within CV.
7. Evaluate once on the untouched test set.
8. Inspect errors and relevant subgroups.
9. Document limitations and monitor deployed performance.

## Repository Contents
```text
09-Model-Evaluation/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Further Reading
- [Scikit-learn model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Scikit-learn cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html)
- [Scikit-learn learning curves](https://scikit-learn.org/stable/modules/learning_curve.html)
