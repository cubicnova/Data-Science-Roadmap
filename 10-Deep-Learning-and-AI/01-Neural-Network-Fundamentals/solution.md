# Solutions — Neural Network Fundamentals

These are reference answers. Coding solutions may vary; focus on correct reasoning and reproducible results.

## A. Concepts and calculations

1. A neuron combines inputs using learned weights and a bias, then transforms the result with an activation function.
2. Weights scale the input contributions; bias shifts the weighted sum and allows a threshold/intercept to move.
3. \(z = 2(0.5) + (-1)(3) + 1 = -1\).
4. Parameters are learned from data (weights and biases). Hyperparameters are chosen by the practitioner (learning rate, hidden-layer width, batch size, epoch count).
5. Composing linear functions is still a linear function. Nonlinear activations allow networks to represent nonlinear decision boundaries and functions.
6. Forward propagation computes predictions; backpropagation uses the chain rule to calculate how the loss changes with each parameter.
7. Loss quantifies mismatch between predictions and targets according to a chosen objective.
8. An epoch is one pass through the dataset; a batch is the subset used for a training step; an iteration/update is one optimizer step.
9. Held-out data estimates performance on examples the model did not learn from directly.
10. Possible causes include a learning rate that is too high or too low, incorrect labels, a bug in the forward/backward pass, unsuitable model capacity, unscaled inputs, or inappropriate loss/target shapes.

## B. Activation functions

11. Example:
```python
import numpy as np

def sigmoid(x):
    x = np.asarray(x, dtype=float)
    return 1 / (1 + np.exp(-x))

print(sigmoid(np.array([-4, -1, 0, 1, 4])))
```
For very large-magnitude inputs, a numerically stable implementation is preferable.
12. `np.maximum(0, np.array([-3, -0.5, 0, 2, 5]))` gives `[0, 0, 0, 2, 5]`.
13. Sigmoid maps values to (0, 1), which is useful for a binary probability output. It saturates at extreme values, where gradients become small.
14. Softmax is commonly used to convert multiclass logits into a probability distribution. Outputs are nonnegative and sum to approximately 1.
15. A ReLU unit can become stuck outputting zero for its inputs, receiving zero gradient on that side and failing to recover under some training conditions.

## C. Perceptrons and forward propagation

16. One implementation:
```python
import numpy as np

def perceptron(x, weights, bias):
    score = np.dot(np.asarray(x), np.asarray(weights)) + bias
    return int(score >= 0)
```
17. For AND with a threshold at 1.5, use weights `[1, 1]` and bias `-1.5` with a step condition `score >= 0`. Only `[1, 1]` produces 1.
```python
for x in ([0, 0], [0, 1], [1, 0], [1, 1]):
    print(x, perceptron(x, [1, 1], -1.5))
```
18. No. XOR is not linearly separable, so a single linear decision boundary cannot classify its four points correctly. A network with a hidden layer and nonlinear activations can represent it.
19. Weights: \(4 \times 3 = 12\); biases: 3; total trainable parameters: 15.
20. For a batch \(X\) with shape `(batch_size, input_dim)`, a common convention is `Z = X @ W + b`, where `W` has shape `(input_dim, output_dim)` and `b` has shape `(output_dim,)`.

## D. Training and evaluation

21. Gradient descent updates parameters in the direction that reduces loss, using the negative gradient scaled by a learning rate.
22. Too high: updates can overshoot or diverge. Too low: training can be very slow or stall before a useful solution.
23. Scaling keeps feature magnitudes comparable, can improve optimization conditioning, and often makes gradient-based training more stable.
24. Binary cross-entropy is common. Targets are usually 0/1; for logits, use a logits-aware loss such as PyTorch's `BCEWithLogitsLoss` rather than applying sigmoid twice.
25. A model can achieve high accuracy by predicting only the majority class while failing on the minority class. Inspect precision, recall, F1, and the confusion matrix.
26. Examples: more representative data, weight decay, dropout, early stopping, smaller model, data augmentation where appropriate, or reducing leakage.
27. Seeds make pseudo-random operations more repeatable. Hardware kernels, parallel operations, and library versions may still introduce nondeterminism.
28. PyTorch supplies automatic differentiation, tensor operations, neural-network layers, loss functions, optimizers, and device support; the NumPy version makes the mechanics more explicit.

## E. Coding challenges — guidance

29. Record the same initialization and data split for fair comparison. XOR is tiny and deterministic; report loss and predicted outputs for each run.
30. Narrow networks may have insufficient capacity; wider networks may fit the task more easily but can add unnecessary parameters. Compare results, not just width.
31. A rate of `0.001` may learn slowly, `0.01` may be a useful starting point, and `0.1` may be unstable for this setup. These are hypotheses, not guaranteed outcomes.
32. Split training data into training and validation sets. Compute validation loss with `model.eval()` and `torch.no_grad()`; do not use validation data for gradient updates.
33. Example outline:
```python
from sklearn.datasets import make_moons
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.neural_network import MLPClassifier
from sklearn.metrics import classification_report

X, y = make_moons(n_samples=1000, noise=0.2, random_state=42)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

models = {
    "Logistic regression": make_pipeline(
        StandardScaler(), LogisticRegression(max_iter=1000)
    ),
    "MLP": make_pipeline(
        StandardScaler(),
        MLPClassifier(hidden_layer_sizes=(16, 8), max_iter=1000,
                      random_state=42, early_stopping=True)
    ),
}

for name, model in models.items():
    model.fit(X_train, y_train)
    print(name)
    print(classification_report(y_test, model.predict(X_test)))
```
34. Example:
```python
from sklearn.metrics import ConfusionMatrixDisplay, confusion_matrix
import matplotlib.pyplot as plt

pred = models["MLP"].predict(X_test)
print(confusion_matrix(y_test, pred))
ConfusionMatrixDisplay.from_predictions(y_test, pred)
plt.show()
```
35. A strong report states a testable hypothesis, controls variables, reports metrics on held-out data, acknowledges sample-size/model-selection limitations, and proposes one specific next experiment.

## Mini-project notes

- Keep target definition and feature engineering documented.
- Fit preprocessing only on training data.
- Compare against a simple baseline.
- Do not claim causal relationships from predictive performance.
- Report failure cases and limitations alongside scores.
