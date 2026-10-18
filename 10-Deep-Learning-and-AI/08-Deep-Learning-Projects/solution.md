# Solutions — Deep Learning Projects

These are reference guidelines. Numerical results depend on TensorFlow version, initialization, and hardware; use the values produced by your own notebook run.

## Exercise 1 — Frame a project

A strong project statement identifies the user, intended decision, input data, target, and measurable outcome. Include foreseeable failure modes and explain how the model output will be reviewed. Do not use sensitive data without a lawful basis and appropriate safeguards.

## Exercise 2 — Establish a baseline

Run `examples.ipynb` and record the actual test metrics. A baseline provides a reference point for deciding whether added complexity is useful. For tabular classification, logistic regression, a decision tree, or gradient-boosted trees are reasonable comparisons. The notebook's synthetic data is generated from a designed rule and is not representative of a real deployment setting.

## Exercise 3 — Experiment with architecture

Change one factor at a time and record it. Compare training and validation curves, not training loss alone. If training loss improves while validation loss worsens, the larger model may be overfitting. Larger models can also require more memory, compute, and tuning.

## Exercise 4 — Early stopping

```python
early_stop = tf.keras.callbacks.EarlyStopping(
    monitor="val_loss",
    patience=5,
    restore_best_weights=True,
)
```

`patience` controls how many epochs without improvement are tolerated. Restoring best weights returns the model to the epoch with the best monitored validation loss. Keep the test set untouched until final evaluation.

## Exercise 5 — Evaluation beyond accuracy

When false negatives are costly, inspect recall/sensitivity alongside precision, the confusion matrix, and potentially PR-AUC. Increasing recall may increase false positives and reduce precision. High-stakes decisions require validation, context, human oversight, and an explicit assessment of the consequences of errors.

## Exercise 6 — Save and reload

```python
model.save("project_model.keras")
loaded = tf.keras.models.load_model("project_model.keras")

before = model.predict(X_test[:10], verbose=0)
after = loaded.predict(X_test[:10], verbose=0)
print("Maximum prediction difference:", abs(before - after).max())
```

Record dataset version, split seed, preprocessing, model architecture, package versions, hyperparameters, metrics, and model artifact location.

## Exercise 7 — Choose a project extension

- **Image classification:** split by source or subject when related images could otherwise leak across splits. Compare per-class metrics and review incorrect images.
- **Sentiment analysis:** split before fitting vocabulary or normalization statistics. Check class-wise metrics and examples with negation or ambiguous language.
- **Time-series forecasting:** use chronological splits and compare with a naive persistence or seasonal baseline. Report errors at the intended forecast horizon.
- **Anomaly detection:** tune thresholds using validation data, then evaluate on held-out data with representative anomaly prevalence. Reconstruction error alone does not prove a sample is anomalous.

## Exercise 8 — Error analysis

Create a table of incorrect predictions with the input, true label, predicted label, and likely cause. Group errors to find systematic patterns. Improvements might include better data quality, class weights, augmentation, features, or model capacity. Each change can introduce trade-offs and should be measured on validation data.

## Exercise 9 — Mini project report

A complete report documents the task, dataset provenance, split design, preprocessing, baseline, architecture, training configuration, metrics, errors, artifact loading, limitations, and next steps. Avoid claims beyond what the evaluation supports.
