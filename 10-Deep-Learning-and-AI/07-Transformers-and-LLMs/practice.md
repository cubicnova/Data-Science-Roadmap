# Practice — Transformers and LLMs

Complete the questions before opening `solution.md`. Use `examples.ipynb` to experiment with the attention mechanics.

## A. Language-model fundamentals

1. What is a token? How can a word be split into multiple tokens?
2. Explain the difference between a token, token ID, and embedding vector.
3. Why do transformer models need information about token position?
4. What is a context window?
5. What is the difference between an encoder-only and decoder-only transformer?
6. What does next-token prediction mean?
7. Distinguish pretraining, fine-tuning, and instruction tuning.
8. Why can two models tokenize the same sentence differently?
9. What are unknown tokens or subword tokens, and why do tokenizers use subwords?
10. Why is fluent text not necessarily factually correct?

## B. Self-attention

11. Explain queries, keys, and values using an analogy.
12. Write the scaled dot-product attention formula and explain each term.
13. Why is the dot product scaled by the square root of the key dimension?
14. What does softmax do to attention scores?
15. What is a causal attention mask, and why is it needed for autoregressive generation?
16. What is padding, and how does a padding mask differ from a causal mask?
17. What is multi-head attention?
18. Why are residual connections used in transformer blocks?
19. What is the role of layer normalization?
20. What does the feed-forward network do inside a transformer block?

## C. Architecture and training

21. Match encoder-only, decoder-only, and encoder-decoder models to typical tasks.
22. What are logits?
23. What is the difference between greedy decoding and sampling?
24. Explain temperature, top-k, and top-p in text generation.
25. Why can increasing max new tokens increase latency and cost?
26. What is the difference between a model's training loss and a downstream task metric?
27. Why should a test set be kept separate from prompt/model selection?
28. Name two reasons a model may fail on text that looks different from its training data.

## D. Using pretrained models responsibly

29. Why should the tokenizer and model come from a compatible model family/checkpoint?
30. What information should you record to make an inference experiment reproducible?
31. What is hallucination in a language model?
32. Explain prompt injection and give one defense-in-depth measure.
33. Why should sensitive personal or company data not be pasted into an unapproved model?
34. What is a model card or dataset card, and how can it help?
35. How would you evaluate a summarization model beyond judging whether the output sounds good?
36. Give two examples of bias or unfair performance in language applications.
37. What should a developer do when a model's answer affects a high-impact decision?
38. Explain why retrieval-augmented generation (RAG) does not guarantee factual answers.

## E. Coding challenges

39. Implement a small token-to-ID dictionary for a fixed vocabulary and encode a sentence.
40. Create a learned embedding layer with vocabulary size 100 and embedding dimension 16.
41. Use PyTorch to calculate scaled dot-product attention for small query, key, and value tensors.
42. Add a causal mask to an attention-score matrix and verify that future positions receive zero attention after softmax.
43. Change the attention temperature/scaling and explain how attention weights change.
44. Use a compatible Hugging Face tokenizer to tokenize a short sentence and inspect token IDs.
45. Run a small pretrained text-generation model if your environment permits. Compare greedy decoding with sampling.
46. Create an evaluation table with criteria such as relevance, factuality, completeness, harmful content, and latency.
47. Test a model on paraphrases and ambiguous prompts; document failure modes.
48. Write a short report: objective, dataset, model/checkpoint, prompt, settings, evaluation, limitations, and next steps.

## Mini-project rubric (20 points)

| Criterion | Points |
|---|---:|
| Task and dataset clearly defined | 3 |
| Reproducible model/prompt setup | 3 |
| Appropriate evaluation design | 5 |
| Results and failure cases documented | 4 |
| Privacy, bias, and safety analysis | 3 |
| Clear conclusions and next steps | 2 |

**Reminder:** generated text can be incorrect even when confident and well written. Verify important claims and do not use sensitive data without authorization.
