# Practice — PyTorch

Complete these exercises before consulting `solution.md`. Use `examples.ipynb` to test your understanding.

## A. Tensor fundamentals

1. What is a PyTorch tensor, and how is it similar to or different from a NumPy array?
2. Create a `3 × 4` tensor of zeros, a `2 × 2` tensor of ones, and a tensor containing the values 1 through 6.
3. Print the shape, data type, and device of a tensor.
4. Explain the difference between `torch.tensor(data)` and `torch.zeros(shape)`.
5. Reshape a tensor with 12 elements to shape `(3, 4)` without changing its values.
6. Calculate the sum of each row and the mean of each column of a 2D tensor.
7. What is broadcasting? Give a valid example and an example that causes a shape mismatch.
8. Explain the difference between `*` and `@` for tensors.
9. Convert a NumPy array to a tensor and convert a CPU tensor back to NumPy.
10. Why can a tensor's device matter when combining it with another tensor?

## B. Autograd

11. Create a scalar tensor `x=3` with gradients enabled and calculate \(y=x^2+2x+1\). Compute and print \(dy/dx\).
12. Explain the purpose of `requires_grad=True`.
13. Why should gradients usually be cleared before each training update?
14. What does `loss.backward()` do?
15. When is `torch.no_grad()` useful?
16. Explain why `model.eval()` is not the same as `torch.no_grad()`.

## C. Datasets and models

17. Define a small custom `Dataset` class that returns a feature and its target.
18. Create a `DataLoader` with batch size 16 and shuffling enabled.
19. Build a model with input size 5, one hidden layer of 16 units, ReLU, and 3 output units.
20. What does `nn.Module` provide? Why do custom models normally implement `forward()`?
21. For multiclass classification with 3 classes, what should the target dtype and shape be when using `CrossEntropyLoss`?
22. For binary classification using `BCEWithLogitsLoss`, should the model output logits or sigmoid probabilities?
23. Name two optimizers and explain the role of the learning rate.

## D. Training and evaluation

24. Write the five core operations in a typical minibatch training step in the correct order.
25. What can happen if you forget `optimizer.zero_grad()`?
26. What is the difference between training mode and evaluation mode?
27. Why should evaluation not update model weights?
28. Describe overfitting and name two ways to reduce it.
29. Why should the test set not be used repeatedly to choose model hyperparameters?
30. Describe a simple way to save and reload model parameters.
31. How would you choose between CPU and GPU in code?
32. What are common causes of a tensor shape mismatch?

## E. Coding challenges

33. Train a linear model on synthetic regression data and plot training loss.
34. Train an MLP on `make_moons`; report accuracy, precision, recall, F1, and a confusion matrix.
35. Compare SGD and Adam using the same dataset, split, and architecture. Record training time and test metrics.
36. Compare learning rates `0.001`, `0.01`, and `0.1`. Plot training loss and explain any instability.
37. Add a validation split and early stopping. Keep the test set untouched until the final evaluation.
38. Save the model `state_dict`, create a fresh model instance, load the parameters, and verify that predictions match.
39. If a CUDA device is available, run the model and batches on that device. If not, ensure the CPU path still works.
40. Write an experiment report with hypothesis, data split, settings, metrics, limitations, and one follow-up experiment.

## Mini-project rubric (20 points)

| Criterion | Points |
|---|---:|
| Dataset and target clearly described | 3 |
| Correct split and preprocessing | 4 |
| PyTorch model and training loop | 4 |
| Appropriate evaluation metrics | 4 |
| Reproducibility and readable code | 2 |
| Limitations and next steps | 3 |

**Important:** model outputs, target shapes, and target dtypes must match the selected loss function. Fit preprocessing only on training data.
