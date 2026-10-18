# Solutions — TensorFlow and Keras

## Exercise 1 — Tensor basics

```python
import tensorflow as tf

values = tf.constant([2, 4, 6, 8])
matrix = tf.constant([[1, 2], [3, 4]])
print(values * 3)       # [6, 12, 18, 24]
print(matrix.shape)     # (2, 2)
print(matrix.dtype)     # commonly int32 for these integer literals
print(tf.rank(matrix))  # 2
```

Shape gives the size along each dimension; rank is the number of dimensions.

## Exercise 2 — Automatic differentiation

```python
import tensorflow as tf

x = tf.Variable(4.0)
with tf.GradientTape() as tape:
    f = 3 * x**2 + 2 * x + 1
gradient = tape.gradient(f, x)
print(gradient.numpy())  # 26.0
```

Since \(f'(x)=6x+2\), at \(x=4\) the derivative is \(24+2=26\).

## Exercise 3 — Build a Keras model

```python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers

model = keras.Sequential([
    layers.Input(shape=(8,)),
    layers.Dense(16, activation="relu"),
    layers.Dense(1, activation="sigmoid"),
])
model.compile(
    optimizer="adam",
    loss="binary_crossentropy",
    metrics=["accuracy"],
)
model.summary()
```

A single sigmoid output represents the estimated probability of the positive class.

## Exercise 4 — Choose output and loss

1. House price: one linear output; mean squared error or mean absolute error.
2. Spam/not spam: one sigmoid output; binary cross-entropy.
3. Ten classes with integer IDs: ten softmax outputs; sparse categorical cross-entropy.
4. Three classes with one-hot targets: three softmax outputs; categorical cross-entropy.

These are common defaults, not the only possible modeling choices.

## Exercise 5 — Train and evaluate

Use the test metrics printed by your notebook run. Values can vary with software, initialization, and training details. Training metrics alone do not show how well the model generalizes to unseen examples. If training performance is much better than validation performance, consider a smaller model, regularization, more representative data, or early stopping.

## Exercise 6 — Callbacks

```python
early_stop = tf.keras.callbacks.EarlyStopping(
    monitor="val_loss",
    patience=5,
    restore_best_weights=True,
)
history = model.fit(
    X_train, y_train,
    validation_data=(X_val, y_val),
    epochs=100,
    callbacks=[early_stop],
    verbose=0,
)
```

Early stopping can stop training when validation loss stops improving, reducing unnecessary epochs and helping limit overfitting.

## Exercise 7 — Save and reload

```python
model.save("binary_classifier.keras")
reloaded = tf.keras.models.load_model("binary_classifier.keras")

before = model.predict(X_test, verbose=0)
after = reloaded.predict(X_test, verbose=0)
print("Maximum absolute difference:", abs(before - after).max())
```

Predictions should be equal or extremely close for the same model and inputs. The `.keras` format stores the model configuration, weights, and relevant training configuration.

## Exercise 8 — Debugging

1. Install TensorFlow into the same environment used by the notebook, then restart the kernel.
2. Make sure the feature engineering pipeline produces exactly the expected number and order of features.
3. Check that the output layer, label shape, and loss function agree.
4. The model may be overfitting; use early stopping, regularization, a smaller model, or more data.
5. Split first, then fit preprocessing only on the training set. Apply the fitted transformation to validation and test sets without fitting on them.

## Exercise 9 — Mini project checklist

A complete submission should include data splitting, model definition, compile settings, training history, callback usage, plots, final test metrics, a saved `.keras` model, and a prediction comparison after reloading. Keep the test set out of repeated tuning decisions.
