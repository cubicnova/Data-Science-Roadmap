# 06 - Classification

Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap), this module introduces classification, a supervised machine learning task that predicts categories.

## Learning Objectives

By the end of this module, you should be able to:

- Explain binary and multiclass classification.
- Identify features and target labels.
- Train baseline and common classification algorithms.
- Split data into training and test sets using stratification where appropriate.
- Evaluate models using accuracy, precision, recall, F1-score, and a confusion matrix.
- Explain the trade-offs between false positives and false negatives.
- Interpret predicted class probabilities and decision thresholds.
- Compare models fairly and avoid data leakage.
- Recognize class imbalance and choose metrics that fit the problem.

## Prerequisites

- Python fundamentals, NumPy, and pandas.
- Understanding of train/test splits and preprocessing.
- Familiarity with the Introduction to Machine Learning and Data Preprocessing modules.

## Setup

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` and run cells in order. Examples use datasets included with Scikit-learn.

## What Is Classification?

Classification is supervised learning where the target is a discrete class or category. A model learns from labelled examples and predicts a class for new observations.

Examples:
- Spam vs. legitimate email (**binary classification**).
- Identify a flower species (**multiclass classification**).
- Assign multiple independent tags to a document (**multilabel classification**).

Classification is different from regression, which predicts a continuous numeric value.

## Common Classification Algorithms

| Algorithm | High-level idea | Notes |
|---|---|---|
| Logistic Regression | Models class probabilities using a linear decision function | Strong baseline; scaling can help |
| k-Nearest Neighbors (KNN) | Predicts from nearby training examples | Sensitive to feature scales and distance choice |
| Decision Tree | Learns a sequence of feature-based splits | Interpretable in small trees; can overfit |
| Random Forest | Combines many decision trees | Often a useful general-purpose baseline |
| Support Vector Machine (SVM) | Finds a separating boundary with a margin | Scaling is often important |
| Naive Bayes | Uses Bayes' rule with simplifying feature assumptions | Fast; often useful for text |
| Gradient Boosting | Builds an ensemble of sequential trees | Powerful, but requires careful validation |

No algorithm is best for every dataset. Compare candidates using the same split or cross-validation strategy and a metric appropriate to the task.

## A Typical Classification Workflow

1. Define the label and the real-world decision the model supports.
2. Inspect label quality, feature types, class balance, and data quality.
3. Separate features (`X`) from the target (`y`).
4. Split into training and test sets; use stratification for classification when appropriate.
5. Build a simple baseline.
6. Fit preprocessing and the classifier on training data only.
7. Predict labels and, when useful, class probabilities.
8. Evaluate with more than one metric when error types matter.
9. Inspect false positives and false negatives.
10. Tune using cross-validation on training data, then evaluate once on the held-out test set.

## Classification Metrics

### Confusion Matrix

In Scikit-learn, rows represent true labels and columns represent predicted labels. For binary classification with labels ordered `[0, 1]`:

| | Predicted 0 | Predicted 1 |
|---|---:|---:|
| Actual 0 | True Negative (TN) | False Positive (FP) |
| Actual 1 | False Negative (FN) | True Positive (TP) |

### Accuracy

Fraction of predictions that are correct:

\[
Accuracy = \frac{TP + TN}{TP + TN + FP + FN}
\]

Accuracy is easy to understand but may be misleading when classes are imbalanced.

### Precision

Of the examples predicted positive, the proportion that are truly positive:

\[
Precision = \frac{TP}{TP + FP}
\]

Precision matters when false alarms are costly.

### Recall

Of all actual positive examples, the proportion detected:

\[
Recall = \frac{TP}{TP + FN}
\]

Recall matters when missing a positive case is costly.

### F1-score

Harmonic mean of precision and recall:

\[
F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}
\]

F1 is useful when balancing precision and recall, but it does not include true negatives and may not capture every business cost.

Other metrics include balanced accuracy, ROC-AUC, average precision / PR-AUC, log loss, and Matthews correlation coefficient. Metric selection depends on the problem, class balance, and the cost of different errors.

## Class Imbalance

If one class is much more common than another, a model can achieve high accuracy by mostly predicting the majority class. Consider:

- Inspecting class counts.
- Using stratified splits.
- Reporting precision, recall, F1, and a confusion matrix.
- Considering class weights or resampling within the training workflow.
- Selecting thresholds based on validation data and error costs.

Never resample the entire dataset before splitting; doing so can leak information and produce unrealistic evaluation.

## Thresholds and Probabilities

Some classifiers provide class probabilities. A binary classifier often predicts the positive class when its estimated probability meets a threshold, commonly 0.5. This threshold is not universally optimal.

- Lowering the threshold often increases recall and may increase false positives.
- Raising the threshold often increases precision and may miss more positive cases.

Choose a threshold using validation data and the practical costs of errors, not by repeatedly tuning on the final test set. A probability estimate is not automatically well calibrated.

## Avoiding Leakage

- Split data before fitting transformations.
- Use a `Pipeline` to combine preprocessing and classification.
- Do not use target-derived features or information that would only be available after the prediction time.
- Keep duplicate or related entities from leaking across splits when the deployment scenario requires group-aware splitting.
- Use the test set only for final evaluation.

## Repository Contents

```text
06-Classification/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Study Plan

1. Read the concepts and metric definitions.
2. Run `examples.ipynb`.
3. Attempt `practice.md` before opening the solutions.
4. Compare models and explain why the metric matters.
5. Complete a mini-project and write a short model evaluation report.

## Further Reading

- [Scikit-learn: Classification metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#classification-metrics)
- [Scikit-learn: Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)
- [Scikit-learn: Classification algorithms](https://scikit-learn.org/stable/supervised_learning.html)
