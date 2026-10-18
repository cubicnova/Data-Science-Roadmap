# Solutions — PyTorch

These are reference answers. Equivalent implementations are acceptable if tensor shapes, dtypes, gradients, and evaluation logic are correct.

## A. Tensor fundamentals

1. A tensor is a multidimensional array with operations and optional automatic differentiation/device support. It resembles a NumPy array, but can participate in autograd and run on accelerators.
2.
```python
import torch
zeros = torch.zeros((3, 4))
ones = torch.ones((2, 2))
values = torch.arange(1, 7)
```
3. Use `tensor.shape`, `tensor.dtype`, and `tensor.device`.
4. `torch.tensor(data)` constructs a tensor from supplied values; `torch.zeros(shape)` creates a tensor of zeros with the requested dimensions.
5. `x.reshape(3, 4)` or `x.view(3, 4)` when the tensor's memory layout permits it.
6. `x.sum(dim=1)` computes row sums; `x.mean(dim=0)` computes column means.
7. Broadcasting aligns compatible dimensions, allowing operations such as `(3, 4) + (4,)`. Shapes such as `(3, 4) + (5,)` are incompatible.
8. `*` is element-wise multiplication; `@` is matrix multiplication.
9.
```python
import numpy as np
a = np.array([1, 2, 3], dtype=np.float32)
t = torch.from_numpy(a)
back = t.numpy()  # CPU tensor without gradient tracking
```
10. A CPU tensor and CUDA tensor generally cannot be combined directly. Move them to the same device, for example with `.to(device)`.

## B. Autograd

11.
```python
x = torch.tensor(3.0, requires_grad=True)
y = x**2 + 2*x + 1
y.backward()
print(x.grad)  # 2*x + 2 = 8
```
12. It tells autograd to track operations so gradients can be computed for the tensor.
13. By default, PyTorch accumulates gradients in `.grad`; clearing prevents gradients from previous batches being added unintentionally.
14. It computes gradients of the scalar loss with respect to leaf tensors and parameters participating in the computation graph.
15. Use it for inference/evaluation when gradients are not required, reducing memory use and computation.
16. `model.eval()` changes the behavior of modules such as Dropout and BatchNorm. `torch.no_grad()` disables gradient recording. They solve different problems and are often used together during evaluation.

## C. Datasets and models

17.
```python
from torch.utils.data import Dataset

class ToyDataset(Dataset):
    def __init__(self, features, targets):
        self.features = torch.as_tensor(features, dtype=torch.float32)
        self.targets = torch.as_tensor(targets)

    def __len__(self):
        return len(self.features)

    def __getitem__(self, index):
        return self.features[index], self.targets[index]
```
18.
```python
from torch.utils.data import DataLoader
loader = DataLoader(dataset, batch_size=16, shuffle=True)
```
19.
```python
model = torch.nn.Sequential(
    torch.nn.Linear(5, 16),
    torch.nn.ReLU(),
    torch.nn.Linear(16, 3)
)
```
20. `nn.Module` registers layers and parameters, supports device movement and state dictionaries, and integrates with PyTorch tooling. `forward()` defines how inputs flow through the model.
21. Model logits have shape `(batch_size, 3)`; target class indices have shape `(batch_size,)` and dtype `torch.long`.
22. Raw logits. `BCEWithLogitsLoss` incorporates sigmoid in a numerically stable calculation.
23. SGD and Adam are common optimizers. The learning rate scales parameter updates and strongly affects training speed and stability.

## D. Training and evaluation

24. `optimizer.zero_grad()` → forward pass → compute loss → `loss.backward()` → `optimizer.step()`.
25. Gradients accumulate across batches, potentially producing unintended updates.
26. `model.train()` enables training behavior for layers such as Dropout; `model.eval()` switches such layers to inference behavior.
27. Evaluation estimates generalization. Updating weights during evaluation contaminates the measurement and makes results harder to compare.
28. Overfitting occurs when a model fits training data but generalizes poorly. Options include early stopping, weight decay, dropout, more representative data, or a smaller model.
29. Repeated tuning against test performance leaks information from the test set into model selection. Use validation data for choices and reserve the test set for final evaluation.
30.
```python
torch.save(model.state_dict(), "model_state.pt")
model2 = MyModel()
model2.load_state_dict(torch.load("model_state.pt", map_location="cpu", weights_only=True))
model2.eval()
```
For older PyTorch versions without `weights_only`, consult the version's documentation and only load trusted files.
31.
```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = model.to(device)
batch_x = batch_x.to(device)
```
Move both model and data to the same device.
32. Common causes include wrong batch dimensions, incompatible feature counts, incorrect target shape, and using a one-hot target where class indices are expected.

## E. Coding challenges — reference patterns

33. Example:
```python
import torch
from torch import nn

torch.manual_seed(42)
X = torch.linspace(-2, 2, 200).reshape(-1, 1)
y = 3 * X - 0.7 + 0.15 * torch.randn_like(X)

model = nn.Linear(1, 1)
loss_fn = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.05)
losses = []

for epoch in range(100):
    optimizer.zero_grad()
    pred = model(X)
    loss = loss_fn(pred, y)
    loss.backward()
    optimizer.step()
    losses.append(loss.item())

print("Learned weight and bias:", model.weight.item(), model.bias.item())
```
34. Follow the `make_moons` PyTorch example in the notebook, then use `sklearn.metrics.classification_report` and `confusion_matrix` on the held-out test labels.
35. Keep the same seed, split, preprocessing, and architecture. Compare loss curves and test metrics; do not infer that one optimizer is universally best from one run.
36. A small learning rate can converge slowly; a moderate rate may work well; a large rate may overshoot or diverge. Inspect actual curves rather than assuming a specific outcome.
37. Create train/validation/test splits. Evaluate validation loss each epoch with `model.eval()` and `torch.no_grad()`. Save the best validation checkpoint and stop after a chosen patience period. Evaluate test data once after model selection.
38.
```python
torch.save(model.state_dict(), "model_state.pt")
reloaded = MyModel().to(device)
state = torch.load("model_state.pt", map_location=device, weights_only=True)
reloaded.load_state_dict(state)
reloaded.eval()
with torch.inference_mode():
    p1 = model(sample.to(device))
    p2 = reloaded(sample.to(device))
print(torch.allclose(p1, p2))
```
39. Select `device` using `torch.cuda.is_available()` and move both model and every batch to it. Test the CPU fallback too.
40. Include hypothesis, split strategy, preprocessing, architecture, optimizer, learning rate, epoch count, metrics, limitations, and a concrete next experiment.

## Mini-project checklist

- Use a baseline, such as logistic regression, where appropriate.
- Report more than accuracy if classes are imbalanced.
- Keep test data separate from model selection.
- Save model parameters and document preprocessing.
- State limitations; do not claim predictive performance proves causation.
