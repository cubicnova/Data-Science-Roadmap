# 06 — Natural Language Processing (NLP)

Natural Language Processing (NLP) is a field of AI that enables computers to process, analyze, and generate human language. This module introduces a practical workflow for preparing text, representing it numerically, and building a basic text classifier.

## Learning objectives

By the end of this module, you should be able to:

- Explain common NLP tasks and preprocessing steps.
- Clean and normalize text while understanding the trade-offs of each operation.
- Tokenize text and represent documents using bag-of-words and TF-IDF.
- Build a text-classification pipeline with scikit-learn.
- Evaluate a classifier using precision, recall, F1-score, and a confusion matrix.
- Explain why data leakage can occur during text preprocessing.
- Identify when simple baselines are appropriate and when embeddings or transformer models may help.

## NLP workflow

1. Define the task and label meaning.
2. Collect text and check data quality, privacy, and licensing.
3. Split data into training, validation, and test sets.
4. Apply suitable normalization and tokenization.
5. Convert text into numerical features.
6. Train a baseline model.
7. Evaluate on held-out data and inspect errors.
8. Improve the model with careful experiments and document limitations.

## Core concepts

- **Tokenization:** splitting text into units such as words, subwords, or characters.
- **Normalization:** making text more consistent, such as lowercasing or normalizing whitespace.
- **Stop words:** frequent words that some workflows remove; removal is task-dependent and can harm meaning (for example, negation).
- **Stemming and lemmatization:** reducing word forms, using different linguistic approaches.
- **Bag of words:** representing a document by token counts, without preserving word order.
- **TF-IDF:** weighting terms by their frequency in a document relative to the corpus.
- **N-grams:** contiguous sequences of tokens, such as unigrams and bigrams.
- **Embeddings:** dense vectors that encode useful relationships between tokens or texts.
- **Transformer:** a neural architecture that uses attention and underlies many modern language models.

## Baseline example

A strong first baseline is often a TF-IDF vectorizer paired with a linear classifier:

```python
from sklearn.pipeline import make_pipeline
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression

model = make_pipeline(
    TfidfVectorizer(ngram_range=(1, 2), min_df=1),
    LogisticRegression(max_iter=1000),
)
model.fit(train_texts, train_labels)
predictions = model.predict(test_texts)
```

Use a pipeline so the vectorizer is fitted only on the training data inside the training workflow. Never fit vocabulary or IDF weights on the full dataset before splitting.

## Files

| File | Purpose |
|---|---|
| `README.md` | Concepts, learning objectives, and workflow |
| `examples.ipynb` | Text cleaning, tokenization, TF-IDF, classification, and evaluation |
| `practice.md` | Exercises and a mini project |
| `solution.md` | Reference answers and code examples |

## Setup

Recommended: Python 3.9+.

```bash
python -m pip install numpy pandas scikit-learn matplotlib jupyter
jupyter notebook
```

The notebook uses a small synthetic dataset created locally, so it does not download text or rely on external APIs.

## Evaluation checklist

- Choose a split strategy suitable for the data (for example, time-based for time-ordered text).
- Fit text vectorizers only on training data.
- Check class balance and use stratification when appropriate.
- Compare against a simple baseline.
- Review per-class precision, recall, and F1—not accuracy alone.
- Inspect false positives and false negatives.
- Consider language, dialect, domain shift, privacy, and bias.
- Do not treat a text model's predictions as objective truth without validation.

## Further reading

- [scikit-learn text feature extraction](https://scikit-learn.org/stable/modules/feature_extraction.html#text-feature-extraction)
- [scikit-learn working with text data](https://scikit-learn.org/stable/tutorial/text_analytics/working_with_text_data.html)
- [Hugging Face NLP course](https://huggingface.co/learn/nlp-course/)
