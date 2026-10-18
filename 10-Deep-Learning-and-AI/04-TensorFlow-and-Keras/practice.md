# Practice — TensorFlow and Keras

Complete the exercises in order. Use `examples.ipynb` as a reference, but try writing each solution yourself before opening `solution.md`.

## Exercise 1 — Tensor basics

1. Create a rank-1 tensor containing `[2, 4, 6, 8]`.
2. Create a 2 × 2 tensor and print its shape and data type.
3. Multiply each value in the rank-1 tensor by 3.
4. Explain the difference between a tensor's shape and rank.

## Exercise 2 — Automatic differentiation

Use `tf.GradientTape` to calculate the derivative of:

\[
f(x)=3x^2+2x+1
\]

at \(x=4\). Check your result manually using calculus.

## Exercise 3 — Build a Keras model

Create a model for binary classification with:

- 8 input features.
- One hidden `Dense` layer with 16 units and ReLU activation.
- One output unit with sigmoid activation.
- Adam optimizer, binary cross-entropy loss, and accuracy metric.

Print the model summary. Explain why the output layer has one unit.

## Exercise 4 — Choose output and loss

Select a suitable output layer and loss for each task:

1. Predict a house's sale price.
2. Classify an email as spam or not spam.
3. Classify an image into one of 10 categories when labels are integer class IDs.
4. Predict one of three mutually exclusive classes when targets are one-hot encoded.

## Exercise 5 — Train and evaluate

Run the synthetic binary classification example in `examples.ipynb`.

1. Record test loss and test accuracy.
2. Explain why evaluating on training data alone is insufficient.
3. Compare training and validation accuracy.
4. If training accuracy is high but validation accuracy is low, name two possible remedies.

## Exercise 6 — Callbacks

Add `EarlyStopping` with `monitor="val_loss"`, `patience=5`, and `restore_best_weights=True`. Explain what problem early stopping helps address.

## Exercise 7 — Save and reload

1. Save a trained model as `binary_classifier.keras`.
2. Reload it with `keras.models.load_model`.
3. Compare predictions from the original and reloaded models.
4. Why is the `.keras` format useful?

## Exercise 8 — Debugging

Identify a likely cause and a fix for each issue:

1. `ModuleNotFoundError: No module named 'tensorflow'`.
2. A model expects 8 features but receives 7.
3. Binary cross-entropy reports a target/output shape mismatch.
4. Validation loss rises while training loss continues to fall.
5. Test accuracy looks suspiciously high after preprocessing the full dataset before splitting.

## Exercise 9 — Mini project

Build a small Keras classifier using a synthetic dataset or a dataset you are permitted to use.

Requirements:

- Use a fixed random seed where possible.
- Split into training, validation, and test sets.
- Define and compile a Keras model.
- Train with at least one callback.
- Plot training and validation loss.
- Report test metrics.
- Save and reload the model, then compare predictions.

## Reflection

- What does `model.compile()` configure?
- What happens during `model.fit()`?
- How does automatic differentiation help with neural-network training?
- Why should the test set not guide repeated model tuning?
