# Deep Learning Interview Preparation

An interview-focused module covering neural network fundamentals, backpropagation, optimization, regularization, CNNs, sequence models, attention, transformers, training diagnostics, and production considerations.

## Learning outcomes

By completing this module, you should be able to:

- Explain neurons, layers, activations, logits, loss functions, and gradient-based learning.
- Describe backpropagation and how gradients update model parameters.
- Compare SGD, momentum, RMSProp, and Adam at a high level.
- Diagnose vanishing/exploding gradients, overfitting, and unstable training.
- Explain initialization, normalization, dropout, weight decay, and learning-rate schedules.
- Describe CNNs, sequence models, attention, and transformer blocks.
- Choose suitable metrics and validation strategies for deep learning tasks.
- Discuss transfer learning, GPU training, reproducibility, inference latency, and deployment monitoring.
- Implement small PyTorch examples and explain their behavior.

## Files

| File | Purpose |
|---|---|
| `README.md` | Learning roadmap, core concepts, and interview frameworks |
| `examples.ipynb` | Hands-on PyTorch examples for tensors, autograd, training, CNNs, and attention |
| `practice.md` | Theory questions, troubleshooting scenarios, and coding exercises |
| `solution.md` | Worked answers, PyTorch patterns, and debugging guidance |

## Suggested 7-day study plan

### Day 1 — Neural network foundations
Perceptrons, dense layers, activations, logits, loss functions, and forward propagation.

### Day 2 — Backpropagation and optimization
Chain rule, gradients, SGD, momentum, Adam, learning rates, initialization, and gradient clipping.

### Day 3 — Generalization and training diagnostics
Train/validation curves, overfitting, dropout, weight decay, normalization, early stopping, and data leakage.

### Day 4 — Convolutional neural networks
Convolution, filters, stride, padding, pooling, receptive fields, transfer learning, and image augmentation.

### Day 5 — Sequence models and attention
RNNs, LSTMs, GRUs, attention, queries/keys/values, and positional information.

### Day 6 — Transformers and deployment
Transformer blocks, self-attention, fine-tuning, batching, quantization concepts, inference latency, and monitoring.

### Day 7 — Mock interview
Explain a training loop, diagnose learning curves, compare architectures, and walk through an end-to-end deep learning project.

## Core concepts checklist

- **Forward pass:** compute predictions from inputs and current parameters.
- **Loss:** quantify how predictions differ from targets according to the task objective.
- **Backpropagation:** use the chain rule to compute gradients of the loss with respect to parameters.
- **Optimizer:** use gradients and optimizer state to update parameters.
- **Learning rate:** controls update size; too large may destabilize training, too small may make learning slow.
- **Generalization:** evaluate on data not used for fitting or repeated model selection.
- **Normalization:** can improve optimization stability; exact behavior depends on the layer and training/inference mode.
- **Transfer learning:** reuse pretrained representations, then freeze or fine-tune layers based on data size and task similarity.
- **Attention:** forms context-dependent weighted combinations of value vectors using query-key compatibility.
- **Reproducibility:** seed random generators where possible, record versions/configuration, and remember GPU kernels may still introduce nondeterminism.

## Common interview comparisons

| Topic | Key distinction |
|---|---|
| Sigmoid vs softmax | Sigmoid gives independent probabilities per output; softmax produces a normalized distribution over mutually exclusive classes |
| BCE vs cross-entropy | Binary cross-entropy suits binary/multi-label outputs; multiclass cross-entropy suits mutually exclusive classes |
| SGD vs Adam | SGD has simpler state and can generalize well; Adam adapts updates per parameter and often converges quickly |
| Dropout vs weight decay | Dropout randomly masks activations during training; weight decay penalizes parameter magnitude through the optimizer/objective |
| Batch norm vs layer norm | Batch norm uses batch statistics during training; layer norm normalizes within each example across chosen dimensions |
| CNN vs transformer | CNNs use local convolutional structure; transformers use attention to mix information across positions |
| Freeze vs fine-tune | Freezing preserves pretrained parameters; fine-tuning updates some or all of them |

## Interview answer framework

For an architecture question:
1. Explain the data flow and core operation.
2. Describe the assumptions or inductive bias.
3. State the important dimensions, activations, and loss.
4. Discuss compute, memory, strengths, and limitations.
5. Explain how you would train and evaluate it.

For a training failure:
1. Verify labels, shapes, preprocessing, and data splits.
2. Check a simple baseline and whether the model can overfit a tiny batch.
3. Inspect loss curves, gradient norms, and prediction distributions.
4. Check learning rate, initialization, normalization, and data pipeline.
5. Change one factor at a time and compare with a reproducible baseline.

## Further reading

- [PyTorch tutorials](https://pytorch.org/tutorials/)
- [PyTorch autograd mechanics](https://pytorch.org/docs/stable/notes/autograd.html)
- [Dive into Deep Learning](https://d2l.ai/)
- [The Annotated Transformer](https://nlp.seas.harvard.edu/annotated-transformer/)
