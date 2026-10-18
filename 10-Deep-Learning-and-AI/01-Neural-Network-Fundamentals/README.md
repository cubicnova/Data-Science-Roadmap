# 01 — Neural Network Fundamentals

Learn how neural networks represent patterns, make predictions, calculate error, and improve through training. This module starts with a single perceptron and builds toward a small multilayer neural network using NumPy, then introduces a practical PyTorch workflow.

## Learning objectives

By the end of this module, you should be able to:

- Explain neurons, weights, biases, layers, and parameters.
- Compute a perceptron output and understand its limitations.
- Compare common activation functions: sigmoid, tanh, ReLU, and softmax.
- Describe forward propagation, loss functions, backpropagation, and gradient descent.
- Implement a small neural network with NumPy and train it on a toy classification problem.
- Build and evaluate a basic neural network in PyTorch.
- Diagnose common training issues such as poor scaling, unsuitable learning rates, and overfitting.

## Prerequisites

- Python fundamentals, functions, lists, and basic classes.
- NumPy arrays and matrix multiplication.
- Basic algebra; derivatives and the chain rule are helpful.
- Familiarity with train/test splits and classification metrics is useful.

## Setup

Use Python 3.10+ in a virtual environment or Anaconda environment.

```bash
python -m pip install numpy matplotlib scikit-learn torch
```

If you are using Jupyter:

```bash
python -m pip install jupyter
jupyter notebook
```

The NumPy examples are designed to run on CPU. PyTorch installation commands can differ by operating system and hardware; if the standard install fails, use the selector at https://pytorch.org/get-started/locally/.

## Concept map

1. **Neuron:** calculates a weighted sum, then applies an activation.
   \[
   z = \mathbf{w}^{T}\mathbf{x} + b,\qquad a = f(z)
   \]
2. **Network:** layers compose these operations to learn more complex functions.
3. **Forward pass:** produces predictions from inputs and current parameters.
4. **Loss:** measures prediction error, such as mean squared error or cross-entropy.
5. **Backpropagation:** applies the chain rule to compute gradients of the loss.
6. **Optimizer:** updates parameters using gradients; gradient descent is the basic example.
7. **Evaluation:** assess generalization on data not used for parameter updates.

## Notebook guide

Open `examples.ipynb` and run cells from top to bottom.

| Section | What you will learn |
|---|---|
| 1. A neuron | Weighted sums and bias |
| 2. Activation functions | Sigmoid, tanh, ReLU, softmax |
| 3. Perceptron | A simple linear classifier |
| 4. Forward pass | Matrix-based multilayer computation |
| 5. Gradient descent | Minimize a simple loss |
| 6. NumPy neural network | Train a small network on XOR |
| 7. PyTorch classifier | Train and evaluate a small MLP |
| 8. Experiments | Learning-rate and architecture comparisons |

## Practice workflow

1. Read the relevant section in the notebook.
2. Run the example and explain the output in your own words.
3. Complete the questions in `practice.md` without opening `solution.md`.
4. Compare your work with `solution.md`; explain any differences.
5. Change one hyperparameter at a time and record the effect.

## Important distinctions

- **Activation vs. loss:** an activation transforms a layer's output; a loss evaluates predictions against targets.
- **Backpropagation vs. optimization:** backpropagation calculates gradients; the optimizer uses them to update parameters.
- **Epoch vs. batch:** an epoch is one pass through the training data; a batch is a subset processed in one update.
- **Training accuracy vs. generalization:** high training performance does not guarantee good performance on unseen data.
- **Softmax:** converts a vector of logits into class probabilities; use a numerically stable framework loss when possible.

## Mini-project: student outcome classifier

Create a synthetic or ethically sourced dataset with numeric features such as study hours, attendance, and practice quiz score. Train a neural network to predict a clearly defined outcome.

Deliverables:
- A short description of the target and features.
- A reproducible train/test split.
- Baseline model and neural-network results.
- Accuracy plus precision, recall, and F1 for classification.
- A confusion matrix and a brief error analysis.
- A paragraph describing limitations, possible leakage, and next steps.

Do not use sensitive student data or interpret predictions as definitive judgments about individuals.

## Completion checklist

- [ ] I can explain each term in the concept map.
- [ ] I can implement a perceptron and activation functions.
- [ ] I understand what the loss and gradients represent.
- [ ] I can train a small network and plot its loss.
- [ ] I can evaluate a model on held-out data.
- [ ] I can explain at least two limitations of my experiment.

## Further learning

- [PyTorch Learn the Basics](https://pytorch.org/tutorials/beginner/basics/intro.html)
- [NumPy documentation](https://numpy.org/doc/)
- [scikit-learn model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Neural Networks and Deep Learning — Michael Nielsen](http://neuralnetworksanddeeplearning.com/)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
