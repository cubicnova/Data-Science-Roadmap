# Solutions — Backpropagation

These are reference solutions. Small numerical differences are expected due to rounding.

## Exercise 1 — Chain rule

Given \(L=\frac12(y-t)^2\), \(y=\sigma(a)\), and \(a=wx+b\):

\[
\frac{\partial L}{\partial y}=y-t,\qquad
\frac{\partial y}{\partial a}=y(1-y)
\]

Therefore:

\[
\frac{\partial L}{\partial w}=(y-t)y(1-y)x
\]
\[
\frac{\partial L}{\partial b}=(y-t)y(1-y)
\]

With \(x=2,w=0.5,b=-0.1\), \(a=0.9\), so \(y=\sigma(0.9)\approx0.71095\). For \(t=1\):

- \(\partial L/\partial y\approx-0.28905\)
- \(\partial y/\partial a\approx0.20550\)
- \(\partial L/\partial w\approx-0.11882\)
- \(\partial L/\partial b\approx-0.05941\)

## Exercise 2 — Gradient descent

\[
w_{\text{new}}=w-\eta\frac{\partial L}{\partial w}
=0.8-0.1(-0.25)=0.825
\]

The gradient is negative, so subtracting the learning-rate-scaled gradient increases the parameter.

## Exercise 3 — Matrix shapes

- \(X:(12,4)\)
- \(W:(4,5)\)
- \(b:(1,5)\) (or a one-dimensional vector of length 5)
- \(Z:(12,5)\)
- \(dW:(4,5)\)

The same bias vector is added to each example's pre-activation row, so broadcasting avoids explicitly copying the bias 12 times.

## Exercise 4 — Gradient check

A central finite-difference approximation is:

\[
g_{\text{num}}\approx\frac{L(\theta+\epsilon)-L(\theta-\epsilon)}{2\epsilon}
\]

A small \(\epsilon\) approximates the local derivative. If it is too large, truncation error increases; if too small, floating-point rounding can dominate. Analytical and numerical gradients should be close within a reasonable tolerance. A large difference can signal a missing chain-rule factor, wrong matrix transpose, inconsistent loss scaling, or an implementation bug.

## Exercise 5 — XOR network

The loss should generally fall substantially and predictions should approach `[0, 1, 1, 0]`. Exact values depend on initialization, hidden-unit count, and learning rate. A learning rate that is too high can cause oscillation or divergence; one that is too low can make progress slow. Use the actual notebook output for your report rather than assuming a particular final number.

## Exercise 6 — Debugging

1. Check the learning rate, activation derivatives, label shapes, and whether gradients are applied.
2. Check for excessively large learning rates, overflow in exponentials, or unstable values.
3. Check the chain rule, transpose operations, loss reduction, and numerical epsilon.
4. Print each array's `.shape` and verify matrix multiplication dimensions.
5. Compute all gradients from the same pre-update parameters, then update the parameters together.

## Exercise 7 — Derivative

```python
import numpy as np

def sigmoid(z):
    # Clip for numerical stability in this simple implementation.
    z = np.clip(z, -500, 500)
    return 1.0 / (1.0 + np.exp(-z))

def sigmoid_derivative_from_activation(a):
    return a * (1.0 - a)

a = sigmoid(np.array([-8.0, 0.0, 8.0]))
print(a)
print(sigmoid_derivative_from_activation(a))
```

The derivative is \(a(1-a)\), approaches zero near 0 and 1, and equals 0.25 at \(a=0.5\).

## Exercise 8 — Mini project checklist

A complete solution should include a reproducible seed, a held-out test set, a forward pass, a backward pass, a training loop, a loss plot, test metrics, and a numerical gradient check. Ensure that the test set is not used for gradient updates.
