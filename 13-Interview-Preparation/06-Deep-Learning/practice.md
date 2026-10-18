# Deep Learning Interview Practice

Answer conceptual questions aloud. For coding problems, explain tensor shapes, training/evaluation modes, loss selection, and edge cases.

## Part A — Conceptual questions

1. What is a neural network, and what does a neuron compute?
2. Explain forward propagation and backpropagation.
3. How does the chain rule enable gradient computation in deep networks?
4. What is the difference between a parameter and a hyperparameter?
5. Compare sigmoid, tanh, ReLU, Leaky ReLU, and softmax.
6. Why can sigmoid or tanh contribute to vanishing gradients?
7. What are logits, and why should a loss function often consume logits directly?
8. Compare mean squared error, binary cross-entropy, and multiclass cross-entropy.
9. Compare SGD, momentum, RMSProp, and Adam.
10. What happens when the learning rate is too high or too low?
11. What are vanishing and exploding gradients? How can you diagnose and mitigate them?
12. Why does weight initialization matter? Compare Xavier/Glorot and He initialization at a high level.
13. Compare dropout, weight decay, early stopping, and data augmentation.
14. What are batch normalization and layer normalization? Where are they commonly useful?
15. Explain train, validation, and test sets for deep learning experiments.
16. What do training and validation loss curves tell you?
17. Explain convolution, kernel/filter, stride, padding, and pooling.
18. Why are CNNs useful for image data?
19. Compare RNNs, LSTMs, and GRUs.
20. Explain attention using queries, keys, and values.
21. What is self-attention? Why do transformers need positional information?
22. What is transfer learning? When would you freeze a backbone versus fine-tune it?
23. How do batch size and gradient accumulation affect training?
24. What are mixed precision and gradient clipping?
25. How would you reduce inference latency and memory usage?
26. How would you make deep learning experiments reproducible?
27. What is the difference between model parameters, FLOPs, latency, and throughput?
28. What would you monitor after deploying a neural network?

## Part B — Troubleshooting scenarios

1. Training loss is not decreasing. List a systematic diagnostic sequence.
2. Training loss is low but validation loss is high. What does this suggest and what would you try?
3. Loss becomes NaN after a few updates. What could cause it?
4. A network predicts almost the same class for every input. What would you inspect?
5. A CNN achieves high training accuracy but poor real-world image performance. What are plausible causes?
6. Fine-tuning a pretrained model reduces accuracy immediately. What settings would you check?
7. GPU utilization is low while training is slow. What might be bottlenecking the workflow?
8. A transformer model runs out of GPU memory as sequence length increases. Explain why and suggest options.
9. Validation metrics fluctuate sharply between epochs. What data or training issues might explain this?
10. A model performs well offline but degrades after deployment. What would you monitor and investigate?

## Part C — Coding exercises

1. Create a PyTorch tensor, inspect its shape/dtype, and move it to a device when available.
2. Implement a small feed-forward classifier using `nn.Module`.
3. Write a training loop using `zero_grad()`, forward pass, loss, `backward()`, and `step()`.
4. Explain the difference between `model.train()` and `model.eval()`.
5. Use `torch.no_grad()` or inference mode during evaluation.
6. Calculate the number of trainable parameters in a simple linear layer.
7. Build a CNN for a small image classification dataset.
8. Add dropout and compare training/validation curves.
9. Freeze a pretrained backbone and train a new classification head.
10. Implement scaled dot-product attention using tensor operations.
11. Use gradient clipping in a training loop.
12. Save and load a model's `state_dict` safely.

## Part D — Debugging prompts

1. A model outputs two-class probabilities but uses `CrossEntropyLoss` with one-hot labels and a final softmax. Explain the common recommended PyTorch pattern and why.
2. Gradients are always `None`. What might have broken the computation graph?
3. The evaluation score changes between repeated runs even though weights are unchanged. What settings or data behavior could cause this?
4. Validation data is augmented with random crops and color jitter. Why might this be undesirable for final evaluation?
5. A training loop forgets `optimizer.zero_grad()`. What happens?
6. The model is evaluated without calling `model.eval()`. Why can dropout or batch normalization behave unexpectedly?

## Part E — Mock interview case

**Case: image classifier for product defects**

Prepare a 5–10 minute walkthrough:
- Define classes, labels, prediction unit, and error costs
- Explore class balance, image quality, duplicates, and leakage
- Establish a simple baseline
- Choose a pretrained architecture and fine-tuning strategy
- Select augmentation and validation split
- Choose metrics and inspect per-class errors
- Discuss latency, memory, versioning, monitoring, and retraining

## Self-review checklist

- [ ] I can explain backpropagation and the role of the optimizer.
- [ ] I can choose a loss function that matches output/target semantics.
- [ ] I can diagnose overfitting and unstable gradients.
- [ ] I can explain CNNs, attention, and transformers at a high level.
- [ ] I can write a minimal correct PyTorch training loop.
- [ ] I can distinguish training mode from evaluation mode.
- [ ] I can discuss model performance and deployment constraints together.
