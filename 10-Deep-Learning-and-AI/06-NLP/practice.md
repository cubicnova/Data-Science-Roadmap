# Practice — Natural Language Processing (NLP)

Try the exercises before checking `solution.md`. Use `examples.ipynb` to run the baseline and explore the outputs.

## Exercise 1 — NLP task identification

Identify the most suitable task for each scenario:

1. Assign a support message to a billing, technical, or account category.
2. Identify people, organizations, and locations in a paragraph.
3. Convert a paragraph from Hindi to English.
4. Estimate whether a review expresses positive or negative sentiment.
5. Produce a short summary of a long article.

## Exercise 2 — Text normalization

Write a Python function that:

- Converts text to lowercase.
- Removes URLs.
- Replaces repeated whitespace with a single space.
- Strips leading and trailing spaces.

Test it on at least three strings. Explain why punctuation and negation words should not automatically be removed for every task.

## Exercise 3 — Tokenization

Tokenize the sentence:

`"NLP is useful, but NLP is not magic!"`

1. Show a simple whitespace-tokenized version.
2. Explain how punctuation changes the tokens.
3. Describe one advantage of subword tokenization.

## Exercise 4 — Bag of words and TF-IDF

Use these documents:

- `["cats like milk", "dogs like walks", "cats and dogs"]`

1. Create a `CountVectorizer` and inspect the vocabulary.
2. Transform the documents into count vectors.
3. Create a `TfidfVectorizer` and compare its output.
4. Explain why a term that occurs in every document may receive less TF-IDF weight.

## Exercise 5 — Train a baseline classifier

Run the notebook's text classification pipeline.

1. Record accuracy, precision, recall, and F1-score.
2. Inspect the confusion matrix.
3. Explain what a false positive means for the positive class.
4. Why is a pipeline safer than fitting TF-IDF on all documents before splitting?

## Exercise 6 — N-grams

Change `ngram_range=(1, 1)` to `ngram_range=(1, 2)`.

1. Compare the number of features.
2. Describe a phrase whose meaning might be better represented by a bigram.
3. What are two disadvantages of adding many n-grams?

## Exercise 7 — Class imbalance

Suppose 95% of messages are ordinary and 5% are abusive.

1. Why can a model with 95% accuracy still be poor?
2. Which metrics would you inspect?
3. Name two ways to address imbalance without contaminating the test evaluation.

## Exercise 8 — Error analysis

Choose at least five incorrect predictions, if available, and categorize their likely causes: ambiguity, negation, spelling variation, domain-specific vocabulary, or insufficient context. Suggest one improvement and explain its trade-off.

## Exercise 9 — Mini project

Build a text classifier for a permitted dataset or a synthetic dataset.

Requirements:

- State the task and label definitions.
- Split data before fitting vectorizers.
- Create a TF-IDF + classifier pipeline.
- Report a confusion matrix and per-class precision, recall, and F1.
- Inspect at least five errors where available.
- Discuss limitations, possible bias, and privacy considerations.

## Reflection

- What information does TF-IDF discard?
- Why can stop-word removal damage sentiment analysis?
- When might pretrained embeddings or a transformer be worth the extra complexity?
