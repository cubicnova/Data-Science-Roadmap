# 04 — TensorFlow and Keras

TensorFlow is an open-source machine-learning framework, and Keras provides a high-level API for building and training neural networks. Together, they make it possible to define models, train them with automatic differentiation, evaluate them, and save them for later use.

## Learning objectives

By the end of this module, you should be able to:

- Explain the roles of TensorFlow, `tf.Tensor`, and Keras.
- Create tensors and perform basic TensorFlow operations.
- Use `tf.GradientTape` to calculate gradients.
- Build a neural network with `keras.Sequential`.
- Compile a model with an optimizer, loss function, and metrics.
- Train, validate, and evaluate a model.
- Make predictions and save/reload a trained model.
- Recognize common training problems such as overfitting, shape mismatches, and data leakage.

## Concepts

### TensorFlow and Keras

- **TensorFlow:** numerical computation, tensor operations, automatic differentiation, and model execution.
- **Keras:** a high-level API for defining layers, models, training, evaluation, and inference.
- **Tensor:** a multidimensional array with a shape and data type.
- **Layer:** a reusable transformation, such as a dense layer.
- **Loss:** a scalar measure of how far predictions are from targets.
- **Optimizer:** updates trainable parameters using gradients.

### Typical Keras workflow

1. Prepare and split the dataset.
2. Define a model and its layers.
3. Compile with an optimizer, loss, and metrics.
4. Train using `model.fit()`.
5. Evaluate on held-out data using `model.evaluate()`.
6. Predict with `model.predict()`.
7. Save with `model.save()` and reload with `keras.models.load_model()`.

Example:

```python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers

model = keras.Sequential([
    layers.Input(shape=(10,)),
    layers.Dense(16, activation="relu"),
    layers.Dense(1, activation="sigmoid"),
])
model.compile(
    optimizer="adam",
    loss="binary_crossentropy",
    metrics=["accuracy"],
)
```

The output layer and loss must match the task. For binary classification, one sigmoid output with binary cross-entropy is a common choice. For multi-class classification with integer labels, a softmax output with sparse categorical cross-entropy is often suitable. For regression, a linear output and an appropriate regression loss are common.

## Files

| File | Purpose |
|---|---|
| `README.md` | Concepts, learning objectives, setup, and workflow |
| `examples.ipynb` | Tensor operations, automatic differentiation, classification, evaluation, and model saving |
| `practice.md` | Guided exercises and a mini project |
| `solution.md` | Reference answers and code patterns |

## Setup

Use Python 3.10–3.12 and a compatible TensorFlow release for your operating system. TensorFlow installation support changes across versions and platforms; check the [official installation guide](https://www.tensorflow.org/install) before installing.

A common pip setup in a virtual environment is:

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
# source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install tensorflow matplotlib jupyter
```

Then launch Jupyter:

```bash
jupyter notebook
```

If TensorFlow does not install for your Python version or platform, use a supported Python version or the official environment instructions. The notebook includes an import/version check at the beginning.

## Training checklist

- Keep test data separate until final evaluation.
- Fit preprocessing steps on training data only.
- Match the output layer and loss to the target format.
- Inspect input and target shapes before training.
- Use validation metrics to monitor generalization.
- Save the best model when appropriate.
- Set random seeds when you need more reproducible experiments, while remembering that exact reproducibility can still vary by hardware and software.

## Further reading

- [TensorFlow tutorials](https://www.tensorflow.org/tutorials)
- [Keras guides](https://keras.io/guides/)
- [TensorFlow API documentation](https://www.tensorflow.org/api_docs/python/tf)
