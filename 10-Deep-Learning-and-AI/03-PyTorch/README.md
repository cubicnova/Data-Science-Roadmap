# 03 — PyTorch

Learn how to build, train, evaluate, and save neural networks with PyTorch. This module focuses on tensors, automatic differentiation, neural-network layers, loss functions, optimizers, training loops, evaluation, and model persistence.

## Learning objectives

By the end of this module, you should be able to:

- Create tensors and understand shapes, data types, and devices.
- Perform tensor operations and use broadcasting safely.
- Explain autograd and inspect gradients.
- Build models with `torch.nn.Module` and `nn.Sequential`.
- Select appropriate loss functions and optimizers.
- Write a correct training and evaluation loop.
- Use `model.train()` and `model.eval()` appropriately.
- Save and load model parameters with `state_dict`.
- Use a GPU when available and understand common training pitfalls.

## Prerequisites

- Python functions, classes, and basic object-oriented programming.
- NumPy arrays, matrix multiplication, and basic linear algebra.
- Basic understanding of neural networks, classification, and regression.

## Setup

Install PyTorch and the supporting libraries. The exact PyTorch installation command can depend on your operating system, Python version, and accelerator. Use the official selector if needed: https://pytorch.org/get-started/locally/.

```bash
python -m pip install torch numpy matplotlib scikit-learn jupyter
```

Launch Jupyter:

```bash
jupyter notebook
```

## Module roadmap

1. **Tensors:** create, inspect, index, reshape, and combine tensors.
2. **Tensor operations:** arithmetic, matrix multiplication, broadcasting, and reductions.
3. **Autograd:** compute derivatives automatically with `requires_grad=True`.
4. **Datasets and DataLoaders:** batch, shuffle, and iterate through examples.
5. **Models:** define layers using `nn.Module` and `nn.Sequential`.
6. **Losses and optimizers:** connect predictions and targets, then update parameters.
7. **Training loop:** zero gradients, forward pass, loss, backward pass, optimizer step.
8. **Evaluation:** disable gradient tracking and compute metrics on held-out data.
9. **Persistence and devices:** save/load `state_dict`, choose CPU/GPU, and reproduce runs.

## Notebook guide

Open `examples.ipynb` and run the cells from top to bottom.

| Section | Practical outcome |
|---|---|
| Tensor basics | Create tensors and inspect shapes/dtypes |
| Tensor operations | Practice reductions and matrix multiplication |
| Autograd | Calculate gradients |
| Dataset/DataLoader | Batch a synthetic dataset |
| Regression | Train a small model on synthetic data |
| Classification | Train an MLP on `make_moons` |
| Evaluation | Report held-out metrics |
| Save/load | Persist model parameters and verify predictions |
| GPU and troubleshooting | Select a device and inspect common mistakes |

## The standard training loop

```python
model.train()
for features, targets in train_loader:
    optimizer.zero_grad()
    predictions = model(features)
    loss = loss_fn(predictions, targets)
    loss.backward()
    optimizer.step()
```

- `zero_grad()` clears gradients from the previous update.
- The model's forward pass produces predictions.
- The loss compares predictions with targets.
- `backward()` computes gradients.
- `step()` updates trainable parameters.

## Common loss-function patterns

| Task | Model output | Typical loss |
|---|---|---|
| Regression | Continuous values | `nn.MSELoss()` or `nn.L1Loss()` |
| Binary classification | One logit per example | `nn.BCEWithLogitsLoss()` |
| Multiclass classification | One logit per class | `nn.CrossEntropyLoss()` |

For `BCEWithLogitsLoss`, pass raw logits, not sigmoid probabilities. For `CrossEntropyLoss`, pass raw class logits and integer class labels (`torch.long`), not softmax probabilities.

## Best practices

- Split data before fitting preprocessing; fit scalers on training data only.
- Keep tensor shapes and target dtypes explicit.
- Do not call `loss.backward()` without a loss connected to trainable parameters.
- Use `model.eval()` and `torch.no_grad()` or `torch.inference_mode()` during evaluation.
- Track validation performance and avoid repeatedly tuning against the test set.
- Save `state_dict` and the preprocessing configuration needed to reproduce predictions.
- A fixed seed improves reproducibility but does not guarantee identical results across every platform or kernel.

## Practice and assessment

1. Complete `practice.md` before checking `solution.md`.
2. Run every notebook cell and explain what each operation does.
3. Change one hyperparameter at a time and record the result.
4. Complete the mini-project below.

## Mini-project: neural-network classifier

Build a PyTorch model for a small tabular classification dataset such as `make_moons` or another permitted dataset.

Deliverables:
- Dataset description and target definition.
- Reproducible train/validation/test split.
- Training loop with loss history.
- At least one baseline for comparison.
- Accuracy, precision, recall, F1, and confusion matrix.
- Saved model parameters and a short inference example.
- Brief error analysis, limitations, and next steps.

## Completion checklist

- [ ] I can create and manipulate tensors.
- [ ] I understand autograd and gradients.
- [ ] I can implement a custom `nn.Module`.
- [ ] I can write a training loop without copying a template blindly.
- [ ] I can evaluate a model without updating its weights.
- [ ] I can save and reload a model using `state_dict`.
- [ ] I can identify common shape, dtype, and device errors.

## Further learning

- [PyTorch Learn the Basics](https://pytorch.org/tutorials/beginner/basics/intro.html)
- [PyTorch Tensor documentation](https://pytorch.org/docs/stable/tensors.html)
- [Autograd mechanics](https://pytorch.org/docs/stable/notes/autograd.html)
- [Saving and Loading Models](https://pytorch.org/tutorials/beginner/saving_loading_models.html)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
