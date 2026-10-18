# Practice — Backpropagation

Try each exercise before opening `solution.md`. Use paper for the derivative questions and Python/NumPy for the implementation tasks.

## Exercise 1 — Chain rule

Let \(a = wx+b\), \(y=\sigma(a)\), and \(L=\frac12(y-t)^2\), where \(\sigma(a)=1/(1+e^{-a})\).

1. Write \(\partial L/\partial y\).
2. Write \(\partial y/\partial a\).
3. Use the chain rule to derive \(\partial L/\partial w\) and \(\partial L/\partial b\).

Use \(x=2\), \(w=0.5\), \(b=-0.1\), and \(t=1\) to calculate approximate numerical values.

## Exercise 2 — Gradient descent

A parameter has current value \(w=0.8\), gradient \(\partial L/\partial w=-0.25\), and learning rate \(\eta=0.1\).

1. Calculate the updated weight.
2. Explain why a negative gradient causes the weight to increase.

## Exercise 3 — Matrix shapes

A batch contains 12 examples and 4 input features. A dense layer has 5 neurons.

1. What are the shapes of \(X\), \(W\), \(b\), and \(Z=XW+b\)?
2. What is the shape of the gradient \(dW\)?
3. Why is broadcasting useful for the bias?

## Exercise 4 — Gradient check

In `examples.ipynb`, locate the finite-difference gradient-check section.

1. Explain why a small perturbation \(\epsilon\) is used.
2. Compare the analytical gradient and numerical gradient.
3. What might a large difference indicate?

## Exercise 5 — XOR network

Run the XOR example.

1. Record the loss near the beginning and end of training.
2. Check whether all four predictions are close to their targets.
3. Change the hidden layer from 4 units to 2 units. Does learning still work with the current seed and hyperparameters?
4. Try learning rates `0.1`, `0.5`, and `1.0`. Describe how the loss changes.

## Exercise 6 — Debugging

Identify the likely issue in each situation:

1. The network's loss stays almost constant.
2. The loss becomes `nan`.
3. The gradient-check error is large.
4. The gradient arrays have incompatible shapes.
5. The weights are updated inside the loop before gradients for all layers are computed.

## Exercise 7 — Implement a derivative

Implement `sigmoid(z)` and `sigmoid_derivative_from_activation(a)` using NumPy. Verify that the derivative returns values close to zero when activation is close to 0 or 1 and reaches its maximum near 0.5.

## Exercise 8 — Mini project

Implement a two-layer neural network for a synthetic binary classification problem of your choice.

Requirements:

- Generate at least 100 samples locally.
- Split the data into training and test sets.
- Implement forward and backward passes with NumPy.
- Plot training loss.
- Report test predictions and accuracy.
- Include a gradient-check test for at least one parameter.

## Reflection

- Which intermediate values must be cached during the forward pass?
- How does backpropagation differ from gradient descent?
- Why can very small or very large gradients make training difficult?
