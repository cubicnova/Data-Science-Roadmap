# 02 — Backpropagation

Backpropagation is the algorithm used to calculate how each parameter in a neural network contributes to prediction error. It applies the **chain rule** from calculus to propagate gradients from the output layer back toward earlier layers.

## Learning objectives

By the end of this module, you should be able to:

- Explain the forward pass, loss calculation, backward pass, and parameter update.
- Apply the chain rule to a simple computational graph.
- Calculate gradients for weights and biases in a small neural network.
- Implement backpropagation using NumPy without an automatic-differentiation framework.
- Validate analytical gradients with finite differences.
- Recognize common implementation errors such as incorrect activation derivatives or gradient shapes.

## Core concepts

### Training loop

1. **Forward pass:** calculate layer activations and the prediction.
2. **Loss:** measure the difference between prediction and target.
3. **Backward pass:** calculate derivatives of the loss with respect to parameters.
4. **Update:** move parameters in the direction that reduces the loss, typically with gradient descent.

For a parameter \(\theta\), gradient descent updates:

\[
\theta \leftarrow \theta - \eta \frac{\partial L}{\partial \theta}
\]

where \(L\) is the loss and \(\eta\) is the learning rate.

### Chain rule

If \(L\) depends on \(z\), and \(z\) depends on \(w\), then:

\[
\frac{\partial L}{\partial w}
=
\frac{\partial L}{\partial z}
\frac{\partial z}{\partial w}
\]

Backpropagation repeatedly applies this rule through the network.

### A dense layer

For input \(X\), weights \(W\), bias \(b\), and activation function \(f\):

\[
Z = XW + b,\qquad A=f(Z)
\]

For a batch of \(m\) examples, gradients are averaged over the batch. For a sigmoid unit, \(f(z)=1/(1+e^{-z})\), and \(f'(z)=f(z)(1-f(z))\).

## Files

| File | Purpose |
|---|---|
| `README.md` | Concepts, formulas, and learning path |
| `examples.ipynb` | Runnable NumPy examples, gradient checking, and a toy network |
| `practice.md` | Exercises without worked answers |
| `solution.md` | Reference solutions and explanations |

## Setup

Python 3.9+ is recommended. Install the only required third-party packages:

```bash
python -m pip install numpy matplotlib jupyter
```

Open `examples.ipynb` in Jupyter Notebook, JupyterLab, or VS Code. The notebook uses a small synthetic XOR dataset and does not download any data.

## Suggested learning path

1. Read the chain-rule explanation.
2. Run the scalar derivative and finite-difference examples.
3. Study the matrix shapes in the NumPy network.
4. Run training and inspect the loss curve and predictions.
5. Complete `practice.md`, then compare with `solution.md`.

## Common mistakes

- Mixing up the derivative of the activation with the derivative of the loss.
- Forgetting that the derivative must be propagated through every layer.
- Using a sum in one gradient and a mean in another, causing inconsistent scaling.
- Updating weights before all gradients for the current step have been calculated.
- Accidentally changing matrix dimensions or transposing the wrong matrix.
- Expecting a neural network to learn well from every random initialization or learning rate.

## Further reading

- [Michael Nielsen — Neural Networks and Deep Learning, Chapter 2](http://neuralnetworksanddeeplearning.com/chap2.html)
- [3Blue1Brown — Neural networks](https://www.3blue1brown.com/topics/neural-networks)
- [NumPy documentation](https://numpy.org/doc/)
