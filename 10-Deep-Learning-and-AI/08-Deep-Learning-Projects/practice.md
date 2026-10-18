# Practice — Deep Learning Projects

Complete these exercises using `examples.ipynb` as a starting point. Use `solution.md` for reference guidance after attempting each task.

## Exercise 1 — Frame a project

Choose one problem: tabular classification, image classification, sentiment analysis, or time-series forecasting.

Write down:

1. The intended user and purpose.
2. The input features and target.
3. A metric that reflects success.
4. Two ways the model could fail.
5. One privacy, safety, or fairness concern.

## Exercise 2 — Establish a baseline

Run the included synthetic tabular project.

1. Record test loss and accuracy.
2. Explain why a baseline should be measured before adding layers.
3. Suggest a non-neural baseline for tabular data.
4. Explain why the synthetic score is not evidence of real-world performance.

## Exercise 3 — Experiment with architecture

Change the hidden-layer widths or number of layers.

1. Record the architecture and random seed.
2. Compare training and validation loss.
3. Did the larger model generalize better?
4. Explain why a larger model can increase overfitting and compute cost.

## Exercise 4 — Early stopping

Change `patience` in the `EarlyStopping` callback.

1. Compare the number of epochs run.
2. Explain the purpose of `restore_best_weights=True`.
3. Why should the test set not be used as the early-stopping validation set?

## Exercise 5 — Evaluation beyond accuracy

Imagine a medical screening model where false negatives are especially costly.

1. Explain why accuracy alone may be insufficient.
2. Which metrics would you inspect?
3. What trade-off can occur when increasing recall?
4. Why does a model score not replace expert review?

## Exercise 6 — Save and reload

1. Save the trained model.
2. Reload it from disk.
3. Compare predictions before and after reloading.
4. List the information you would record to reproduce the project.

## Exercise 7 — Choose a project extension

Choose one extension and outline its data and evaluation requirements:

- **Image classification:** class labels, image resize strategy, augmentation, per-class metrics.
- **Sentiment analysis:** text labels, text preprocessing, train-only vocabulary fitting, class-wise F1.
- **Time-series forecasting:** chronological split, naive forecast baseline, horizon-specific error.
- **Anomaly detection:** mostly-normal training data, threshold selection on validation data, precision/recall review.

## Exercise 8 — Error analysis

Review misclassified examples from your selected project. If the included toy dataset has few errors, describe how you would inspect errors on a larger dataset.

Categorize possible failure causes such as noisy labels, missing features, distribution shift, class imbalance, or insufficient model capacity. Propose one improvement and one possible downside.

## Exercise 9 — Mini project report

Submit a concise report containing:

- Problem and intended use.
- Dataset provenance and license.
- Split strategy and preprocessing.
- Baseline and chosen model.
- Training curve and evaluation metrics.
- Error analysis.
- Saved-model inference example.
- Limitations, risks, and future work.

## Reflection

- Why must preprocessing be fit on training data only?
- What is the difference between validation and test data?
- When would transfer learning be preferable to training from scratch?
- How would you detect data drift after deployment?
