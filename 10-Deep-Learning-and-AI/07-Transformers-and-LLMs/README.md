# 07 — Transformers and LLMs

Transformers are the foundation of many modern language models and multimodal AI systems. This module introduces tokenization, embeddings, positional information, self-attention, transformer blocks, language-model objectives, inference, prompting, and responsible evaluation.

## Learning objectives

By the end of this module, you should be able to:

- Explain tokens, token IDs, embeddings, and positional encodings.
- Describe query, key, and value vectors in self-attention.
- Explain attention masks and why causal language models cannot attend to future tokens.
- Identify the roles of multi-head attention, residual connections, layer normalization, and feed-forward layers.
- Distinguish encoder-only, decoder-only, and encoder-decoder transformer architectures.
- Explain next-token prediction and the high-level pretraining/fine-tuning workflow.
- Use a pretrained model with Hugging Face Transformers for inference.
- Evaluate generated text for usefulness, factuality, safety, and limitations.
- Recognize risks such as hallucination, prompt injection, data leakage, bias, and excessive confidence.

## Prerequisites

- Python basics and virtual environments.
- Basic neural networks, tensors, and gradient descent.
- Familiarity with classification and train/test evaluation.
- Basic linear algebra is useful for understanding attention scores.

## Setup

Install the core packages:

```bash
python -m pip install torch transformers tokenizers datasets numpy matplotlib jupyter
```

Model downloads may require an internet connection and can use substantial disk space and memory. Start with small models or toy examples. If installation versions conflict, follow the official [Hugging Face Transformers installation guide](https://huggingface.co/docs/transformers/installation).

Launch Jupyter:

```bash
jupyter notebook
```

## Learning roadmap

1. **Language-model basics:** tokens, vocabulary, token IDs, context windows.
2. **Embeddings:** map token IDs to dense vectors.
3. **Positional information:** represent token order.
4. **Self-attention:** compute relationships among tokens using queries, keys, and values.
5. **Scaled dot-product attention:** scores, scaling, masks, softmax, weighted values.
6. **Multi-head attention:** learn several attention patterns in parallel.
7. **Transformer block:** attention, residual connections, normalization, feed-forward network.
8. **Architecture families:** encoder-only, decoder-only, encoder-decoder.
9. **Language modeling:** next-token prediction, pretraining, instruction tuning, fine-tuning.
10. **Inference and generation:** logits, greedy decoding, sampling, temperature, top-k/top-p.
11. **Using pretrained models:** tokenizer/model pairing, inference pipelines, resource limits.
12. **Evaluation and responsible use:** factuality, bias, privacy, safety, and prompt injection.

## Notebook guide

Open `examples.ipynb` and run the cells from top to bottom. The first sections use small NumPy/PyTorch examples to make attention mechanics visible. The Hugging Face section is optional and may download a model.

| Section | What you will learn |
|---|---|
| Tokenization concept | Tokens and token IDs |
| Embeddings and positions | Represent tokens and order |
| Self-attention | Compute query/key/value attention |
| Causal mask | Prevent looking at future tokens |
| Transformer block | Combine attention and feed-forward layers |
| Text generation concepts | Greedy decoding and temperature |
| Hugging Face inference | Load a small pretrained model when available |
| Evaluation checklist | Assess output quality and limitations |

## Core concepts

### Scaled dot-product attention

\[
\operatorname{Attention}(Q,K,V)=\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}+M\right)V
\]

- **Query (Q):** what a token is looking for.
- **Key (K):** what each token offers for matching.
- **Value (V):** the information combined using attention weights.
- **Mask (M):** excludes disallowed positions, such as future tokens in causal language modeling.
- **Scaling:** helps keep dot-product magnitudes manageable as key dimension grows.

### Transformer architecture families

| Family | Typical attention pattern | Common uses |
|---|---|---|
| Encoder-only | Bidirectional context | Classification, embeddings |
| Decoder-only | Causal/self-attention | Autoregressive text generation |
| Encoder-decoder | Encoder reads input; decoder generates output | Translation, summarization |

### Common generation controls

- **Greedy decoding:** choose the highest-probability next token at each step.
- **Temperature:** changes the sharpness of the sampling distribution; lower values are more deterministic.
- **Top-k:** sample from the k highest-scoring candidate tokens.
- **Top-p:** sample from the smallest candidate set whose cumulative probability reaches p.
- **Max new tokens:** caps the number of generated tokens; it does not guarantee factuality or quality.

## Practical notes

- Use the tokenizer that belongs with the selected model.
- Keep prompts and outputs within the model's context limit.
- Generated output can be fluent and still incorrect; verify important claims independently.
- Do not submit secrets, credentials, private records, or sensitive data to a model without authorization and an approved data-handling process.
- Treat retrieved documents, web pages, and user-provided text as untrusted input. Instructions inside that content should not automatically override system or application rules.
- Evaluate model quality on representative examples and document failure cases.
- Check model and dataset licenses before redistribution or commercial use.

## Practice workflow

1. Run the conceptual examples first.
2. Complete `practice.md` without checking `solution.md`.
3. Compare answers and explain the attention calculation in your own words.
4. If you run a pretrained model, record the model ID, package versions, prompt, decoding settings, and limitations.

## Mini-project: small text-generation or classification study

Choose a permitted public text dataset or a small set of non-sensitive examples. Use a pretrained model for a narrowly defined task such as sentiment classification, short summarization, or controlled text generation.

Deliverables:
- Task, dataset source, and license.
- Baseline or comparison method.
- Model ID and inference settings.
- Evaluation criteria and representative outputs.
- At least five failure cases or a clear explanation if fewer occur.
- Notes on hallucination, bias, privacy, prompt injection, and suitability for deployment.

## Completion checklist

- [ ] I can explain tokenization and embeddings.
- [ ] I can describe queries, keys, values, and attention masks.
- [ ] I understand the roles of residual connections and feed-forward layers.
- [ ] I can distinguish transformer architecture families.
- [ ] I understand next-token prediction and generation controls.
- [ ] I can run a pretrained model and document its settings.
- [ ] I can identify limitations and evaluate generated outputs critically.

## Further learning

- [Hugging Face Transformers documentation](https://huggingface.co/docs/transformers/index)
- [Hugging Face NLP course](https://huggingface.co/learn/nlp-course/)
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [PyTorch documentation](https://pytorch.org/docs/stable/)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
