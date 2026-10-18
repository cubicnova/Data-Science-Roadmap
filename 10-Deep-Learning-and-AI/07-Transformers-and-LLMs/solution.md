# Solutions — Transformers and LLMs

These are reference answers. Equivalent explanations and implementations are acceptable when they preserve the correct concepts and tensor shapes.

## A. Language-model fundamentals

1. A token is a unit processed by a language model. It may be a word, part of a word, punctuation, or another symbol.
2. A token is the text unit; a token ID is its integer vocabulary index; an embedding is a learned dense vector representing that ID.
3. Attention by itself does not encode sequence order. Positional information helps the model distinguish sequences with different order.
4. A context window is the maximum amount of token context the model can process in one pass, subject to the model and configuration.
5. Encoder-only models generally use bidirectional context and are often used for understanding/classification. Decoder-only models use causal attention and generate text autoregressively.
6. Predict the probability distribution for the next token given previous tokens.
7. Pretraining learns general patterns from large corpora; fine-tuning updates a pretrained model for a task/domain; instruction tuning trains behavior on instruction-response examples.
8. Tokenizers can use different vocabularies, normalization rules, and subword algorithms.
9. Subwords let a tokenizer represent rare or unseen words from smaller units without needing every full word in the vocabulary.
10. Language models optimize statistical patterns and can produce plausible completions without reliable grounding or verification.

## B. Self-attention

11. A query represents what a token is seeking, keys represent what tokens can match against, and values are the information mixed based on those matches.
12.
\[
\operatorname{Attention}(Q,K,V)=\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}+M\right)V
\]
`QKᵀ` scores compatibility; `sqrt(d_k)` scales scores; `M` masks forbidden positions; softmax converts scores to weights; multiplication by `V` combines information.
13. Dot-product magnitude tends to grow with dimension. Scaling helps keep scores in a range where softmax gradients are more useful.
14. Softmax maps scores to nonnegative weights that sum to one along the selected dimension.
15. A causal mask prevents a position from using future tokens during next-token prediction, preserving the autoregressive training objective.
16. Padding fills batches to a common length. A padding mask excludes those artificial positions; a causal mask excludes future positions.
17. Multi-head attention runs several attention projections in parallel, allowing different heads to represent different relationships.
18. Residual connections provide a direct path for information and gradients and make deeper networks easier to optimize.
19. Layer normalization stabilizes intermediate activations by normalizing features within each example/token representation.
20. The feed-forward sublayer applies learned nonlinear transformations to each token representation, expanding and then projecting feature dimensions.

## C. Architecture and training

21. Encoder-only: classification/embeddings; decoder-only: autoregressive generation; encoder-decoder: translation or input-to-output generation such as summarization.
22. Logits are raw unnormalized scores before a probability transformation such as softmax.
23. Greedy decoding always chooses the highest-scoring next token. Sampling draws from a probability distribution and can produce varied outputs.
24. Temperature adjusts distribution sharpness; top-k limits candidates to the k most likely tokens; top-p retains a high-probability nucleus whose cumulative probability reaches p.
25. More generated tokens require more model steps and computation; autoregressive generation is sequential, so latency and resource use can increase.
26. Training loss is an optimization objective; downstream metrics measure task behavior, such as accuracy, F1, factuality, or human-rated usefulness.
27. Repeated selection based on test results leaks information from the test set into model selection and gives an overly optimistic estimate.
28. Distribution shift, domain-specific terminology, changed writing style, ambiguous inputs, or language coverage gaps can all cause failures.

## D. Using pretrained models responsibly

29. Token IDs must correspond to the vocabulary and conventions expected by the model's embedding layer. Mismatches can make input meaningless.
30. Record model/checkpoint ID and revision, tokenizer, library versions, prompt, input data version, decoding parameters, random seed where applicable, hardware, and evaluation method.
31. Hallucination is generated content that is unsupported, fabricated, or incorrect while being presented as a plausible answer.
32. Prompt injection attempts to manipulate the model through untrusted input. Defenses include separating instructions from data, least-privilege tool access, validating tool arguments, and testing adversarial inputs; no single defense is sufficient.
33. Sensitive data may be retained, exposed, or processed contrary to organizational/legal requirements. Use only approved tools and data-handling procedures.
34. Model cards and dataset cards describe intended use, provenance, evaluation, limitations, and risks, supporting more informed decisions.
35. Use task-specific checks: factual consistency, coverage of important points, omission/hallucination analysis, human review, and representative test cases.
36. Examples include lower quality for dialects or underrepresented languages, and stereotypes in generated descriptions or recommendations.
37. Use appropriate human oversight, validated domain-specific evaluation, documented limitations, privacy protections, and an appeal/correction path. Do not treat a general model as the sole authority.
38. RAG can retrieve irrelevant, stale, incomplete, or adversarial documents, and a model can still misinterpret sources or make unsupported claims.

## E. Coding challenges — reference patterns

39.
```python
vocab = {"<unk>": 0, "data": 1, "science": 2, "is": 3, "useful": 4}
sentence = "data science is useful"
ids = [vocab.get(token, vocab["<unk>"]) for token in sentence.lower().split()]
print(ids)  # [1, 2, 3, 4]
```
This is a toy whitespace tokenizer; production tokenizers handle punctuation, Unicode, subwords, and normalization.
40.
```python
import torch
from torch import nn

embedding = nn.Embedding(num_embeddings=100, embedding_dim=16)
token_ids = torch.tensor([1, 5, 7, 12])
vectors = embedding(token_ids)
print(vectors.shape)  # (4, 16)
```
41.
```python
import math
import torch

Q = torch.randn(2, 3, 4)  # batch, query length, head dimension
K = torch.randn(2, 5, 4)  # batch, key length, head dimension
V = torch.randn(2, 5, 6)  # batch, key length, value dimension

scores = Q @ K.transpose(-2, -1) / math.sqrt(Q.shape[-1])
weights = torch.softmax(scores, dim=-1)
output = weights @ V
print(output.shape)  # (2, 3, 6)
```
42.
```python
scores = torch.tensor([[1.0, 2.0, 3.0],
                       [1.0, 2.0, 3.0],
                       [1.0, 2.0, 3.0]])
length = scores.shape[-1]
future = torch.triu(torch.ones(length, length, dtype=torch.bool), diagonal=1)
masked_scores = scores.masked_fill(future, float("-inf"))
weights = torch.softmax(masked_scores, dim=-1)
print(weights)
```
The upper-triangular future positions receive zero probability. A real implementation must align mask shape with batch/head/query dimensions.
43. Sharper score distributions generally concentrate weight on fewer tokens; flatter distributions spread weight more evenly. Interpret the effect in context.
44.
```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert/distilbert-base-uncased")
text = "Transformers learn contextual representations."
encoded = tokenizer(text)
print(encoded.tokens() if hasattr(encoded, "tokens") else tokenizer.convert_ids_to_tokens(encoded["input_ids"]))
print(encoded["input_ids"])
```
A first run may download tokenizer files.
45. Use a small generation-capable checkpoint with its matching tokenizer and compare `do_sample=False` against `do_sample=True` with explicit `temperature` and `top_p`. Record the checkpoint and settings; results depend on the model and prompt.
46. Score each output against defined criteria and use multiple examples. For high-impact uses, add domain-expert review and clear acceptance thresholds.
47. Keep a fixed evaluation set, vary one prompt characteristic at a time, and categorize errors instead of reporting only a few successful examples.
48. A strong report includes the objective, data provenance, model ID/revision, prompt and generation settings, metrics, representative successes/failures, risks, limitations, and next steps.

## Mini-project checklist

- Keep the task narrow and measurable.
- Record model/tokenizer versions and generation parameters.
- Use representative examples, not only cherry-picked successes.
- Review factuality, bias, privacy, and misuse risks.
- Verify consequential claims with trusted sources.
