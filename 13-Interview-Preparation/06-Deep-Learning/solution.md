# Deep Learning Interview Practice — Solutions

These are reference answers. Framework details can vary by version, device, and architecture; explain the underlying idea as well as the implementation.

## Part A — Conceptual answer guide

1. A neural network composes parameterized operations. A basic neuron computes a weighted sum plus bias and then applies an activation.
2. The forward pass computes predictions and loss. Backpropagation applies the chain rule to calculate gradients; an optimizer uses those gradients to update parameters.
3. The chain rule expresses derivatives through nested functions, allowing gradients to propagate from the loss back through each layer.
4. Parameters are learned from data (weights/biases); hyperparameters configure training (learning rate, batch size, architecture, weight decay).
5. Sigmoid maps to (0,1); tanh to (-1,1); ReLU clips negative values to zero; Leaky ReLU keeps a small negative slope; softmax maps a vector to a normalized distribution.
6. Saturated sigmoid/tanh activations have derivatives near zero, so gradients can shrink across many layers. ReLU-family activations and suitable initialization can help, but do not eliminate every gradient issue.
7. Logits are unnormalized model scores. PyTorch's `CrossEntropyLoss` expects logits and internally applies a numerically stable log-softmax calculation; for binary classification, `BCEWithLogitsLoss` combines sigmoid and binary cross-entropy stably.
8. MSE penalizes squared numeric errors; binary cross-entropy fits binary or multi-label probability outputs; multiclass cross-entropy fits mutually exclusive classes. Match target encoding and output layer to the loss.
9. SGD updates using the current gradient; momentum accumulates a moving direction; RMSProp adapts step sizes using recent squared gradients; Adam combines momentum-like first moments and adaptive second moments.
10. Too-high learning rates can overshoot or diverge; too-low rates can make progress very slow or stall near plateaus.
11. Vanishing gradients become very small through depth/time; exploding gradients become excessively large. Inspect gradient norms and loss behavior; consider initialization, normalization, residual connections, gated sequence models, learning-rate changes, and gradient clipping.
12. Initialization sets starting weight scales. Xavier/Glorot is commonly used with tanh-like activations; He/Kaiming is designed for ReLU-family activations. Exact choice depends on architecture.
13. Dropout randomly masks activations during training; weight decay penalizes large weights; early stopping selects a checkpoint based on validation behavior; augmentation creates label-preserving training variations when designed correctly.
14. Batch normalization uses batch-derived statistics during training and running statistics at inference; layer normalization normalizes features within each example along configured dimensions. Layer norm is common in transformer blocks.
15. Training fits parameters; validation supports tuning and checkpoint/model selection; test gives a final estimate after choices are complete. Keep test data isolated from repeated decisions.
16. Training loss falling while validation loss rises suggests overfitting; both high can indicate underfitting, poor optimization, or data issues. Interpret curves alongside metrics, label noise, and dataset size.
17. Convolution applies a learned kernel over local regions; stride controls movement; padding controls boundary handling/output size; pooling downsamples or aggregates local responses.
18. CNNs exploit local connectivity and shared filters, learning spatial patterns with fewer parameters than fully connected layers on many image tasks.
19. RNNs process sequences recurrently; LSTMs and GRUs use gates to improve information and gradient flow across longer dependencies. Transformers use attention and can parallelize token processing during training.
20. Queries express what a position seeks, keys represent what positions offer for matching, and values carry information combined by attention weights.
21. Self-attention relates positions within the same sequence. Since attention alone does not inherently encode sequence order, transformers add positional encodings/embeddings or positional biases.
22. Transfer learning starts from pretrained features. Freeze when data is limited or compute constrained; fine-tune some/all layers when task data supports adaptation. Compare with a validation strategy and use an appropriately small learning rate for pretrained layers.
23. Larger batches often improve hardware utilization but use more memory and change gradient noise/optimization behavior. Gradient accumulation sums gradients over micro-batches to approximate a larger effective batch, with caveats for batch-dependent layers.
24. Mixed precision uses lower-precision arithmetic for speed/memory where supported, often with stability safeguards. Gradient clipping limits gradient norms or values to reduce extreme updates.
25. Consider smaller architectures, batching, optimized runtimes, quantization, pruning, compilation, caching, and hardware choice. Measure real workload latency percentiles and quality impact.
26. Seed random generators, record code/data/model versions and configuration, control data splits, and use deterministic operations where feasible. Some accelerator operations may remain nondeterministic or slower when forced deterministic.
27. Parameters count learned values; FLOPs estimate arithmetic work; latency is time per request/batch; throughput is work per unit time. They are related but not interchangeable.
28. Monitor input schema/quality, feature and prediction distributions, latency, throughput, errors, resource use, subgroup behavior, and labeled performance/calibration when labels become available.

## Part B — Troubleshooting guide

1. Check data/labels and target alignment, output/target shapes, loss semantics, learning rate, gradients, initialization, normalization, class balance, and whether a tiny batch can be overfit. Confirm parameters are actually updated.
2. Likely overfitting or distribution mismatch. Try more representative data, appropriate augmentation, weight decay/dropout, smaller model, early stopping, and leakage-safe validation. Investigate label noise and split differences.
3. Potential causes include too-high learning rate, invalid inputs/labels, unstable exponentials, mixed-precision overflow, division by zero, or exploding gradients. Check the first non-finite tensor and lower the learning rate if appropriate.
4. Check label mapping, class balance, preprocessing, output activation/loss compatibility, threshold, gradient flow, and whether the model is learning at all.
5. Check domain shift, label quality, duplicates/leakage, inconsistent capture conditions, unrealistic validation, and overfitting. Add representative data and per-condition error analysis.
6. Check learning rate (often too high), frozen/unfrozen layers, normalization mode, preprocessing normalization, class head dimensions, label mapping, and optimizer parameter groups.
7. Data loading, CPU preprocessing, storage I/O, small batches, synchronization, frequent logging, or inefficient transfers may be limiting utilization. Profile before changing architecture.
8. Standard full attention commonly has memory/compute that grows quadratically with sequence length in the attention matrix. Options include shorter/context-windowed inputs, efficient attention kernels, gradient checkpointing, smaller batches, mixed precision, or architectures designed for long sequences.
9. Small validation set, stochastic augmentation, data leakage/split problems, high learning rate, unstable optimization, or nondeterministic operations may cause fluctuations. Inspect confidence intervals and repeatability.
10. Monitor data and label shifts, preprocessing parity, model versions, prediction rates, latency, failure rates, and delayed-label metrics. Compare production examples with training assumptions and maintain rollback capability.

## Part C — PyTorch reference patterns

### 1. Tensor and device
```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
x = torch.tensor([[1.0, 2.0], [3.0, 4.0]], dtype=torch.float32, device=device)
print(x.shape, x.dtype, x.device)
```

### 2. Feed-forward classifier
```python
import torch.nn as nn

class SmallClassifier(nn.Module):
    def __init__(self, input_features, hidden_features, classes):
        super().__init__()
        self.network = nn.Sequential(
            nn.Linear(input_features, hidden_features),
            nn.ReLU(),
            nn.Dropout(p=0.2),
            nn.Linear(hidden_features, classes),
        )

    def forward(self, x):
        return self.network(x)  # raw logits for CrossEntropyLoss
```

### 3. Minimal training loop
```python
model.train()
for features, labels in train_loader:
    features = features.to(device)
    labels = labels.to(device)

    optimizer.zero_grad(set_to_none=True)
    logits = model(features)
    loss = criterion(logits, labels)
    loss.backward()
    optimizer.step()
```
For multiclass classification, `criterion` may be `nn.CrossEntropyLoss()` and labels are usually class indices of integer dtype. For binary classification with one logit per example, `nn.BCEWithLogitsLoss()` expects floating targets with compatible shape.

### 4. Evaluation
```python
model.eval()
total_loss = 0.0
with torch.inference_mode():
    for features, labels in valid_loader:
        features = features.to(device)
        labels = labels.to(device)
        logits = model(features)
        loss = criterion(logits, labels)
        total_loss += loss.item()
```
`model.eval()` changes behavior of modules such as dropout and batch normalization; inference mode disables autograd bookkeeping. They solve different problems and are commonly used together.

### 5. Count trainable parameters
```python
def count_trainable_parameters(model):
    return sum(parameter.numel() for parameter in model.parameters()
               if parameter.requires_grad)
```

### 6. Scaled dot-product attention
```python
import math
import torch

def scaled_dot_product_attention(query, key, value, mask=None):
    scores = query @ key.transpose(-2, -1) / math.sqrt(query.size(-1))
    if mask is not None:
        scores = scores.masked_fill(mask == 0, float("-inf"))
    weights = torch.softmax(scores, dim=-1)
    return weights @ value, weights
```
The mask must broadcast to the score tensor and must not mask every key for a query, or softmax can produce invalid values.

### 7. Gradient clipping
```python
optimizer.zero_grad(set_to_none=True)
loss = criterion(model(inputs), targets)
loss.backward()
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
optimizer.step()
```

### 8. Save and load weights
```python
torch.save(model.state_dict(), "model_state.pt")

model = SmallClassifier(input_features=20, hidden_features=64, classes=3)
state = torch.load("model_state.pt", map_location="cpu", weights_only=True)
model.load_state_dict(state)
model.eval()
```
The architecture must match the saved state. `weights_only` availability depends on the installed PyTorch version; follow the installed version's official loading guidance and only load trusted files.

## Part D — Debugging solutions

1. For mutually exclusive multiclass classification, pass raw logits to `CrossEntropyLoss` and use class-index targets in the usual setup. Applying softmax first is unnecessary and can worsen numerical behavior. For multi-label classification, use one logit per label with `BCEWithLogitsLoss`.
2. Possible causes include `torch.no_grad()` around training, detached tensors, calling `.item()` on a value used for further computation, not using the loss to call `backward()`, frozen parameters, or an unused branch of the graph.
3. Check dropout/evaluation mode, batch normalization, randomness in data transforms, data-loader shuffling, nondeterministic kernels, and seed control.
4. Random evaluation augmentation makes the metric depend on random transformations and may not match the intended inference distribution. Use deterministic validation preprocessing unless augmentation-at-test-time is an intentional evaluation method.
5. Gradients accumulate by default, so updates use accumulated gradients across batches. Clear gradients each iteration unless accumulation is deliberate.
6. Dropout may remain active and batch normalization may use/update batch statistics, making evaluation noisy or inconsistent. Call `model.eval()` before evaluation.

## Suggested answer-quality rubric

Score each category from 1–5:
- Conceptual accuracy
- Tensor shapes and loss compatibility
- Training diagnostics and debugging
- Validation and generalization
- Compute, latency, and production awareness

A strong answer connects mathematical intuition to correct implementation and realistic deployment constraints.
