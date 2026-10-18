# Solutions — Natural Language Processing (NLP)

## Exercise 1 — NLP task identification

1. Text classification.
2. Named-entity recognition (NER).
3. Machine translation.
4. Sentiment analysis (a text-classification task).
5. Text summarization.

## Exercise 2 — Text normalization

```python
import re

def normalize_text(text):
    text = text.lower()
    text = re.sub(r"https?://\S+|www\.\S+", " ", text)
    text = re.sub(r"\s+", " ", text)
    return text.strip()

samples = [
    "  GREAT result!  ",
    "Read https://example.com/page now",
    "Line one\n\nLine two",
]
for sample in samples:
    print(normalize_text(sample))
```

Keep punctuation when it carries useful information. Removing words such as “not” can reverse sentiment, so preprocessing should match the task and be tested rather than applied automatically.

## Exercise 3 — Tokenization

A basic whitespace split gives:

```python
sentence = "NLP is useful, but NLP is not magic!"
print(sentence.split())
# ['NLP', 'is', 'useful,', 'but', 'NLP', 'is', 'not', 'magic!']
```

Punctuation remains attached to words. A tokenizer may separate punctuation or normalize it. Subword tokenization can handle many unseen or rare word forms by breaking them into smaller known units.

## Exercise 4 — Bag of words and TF-IDF

```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

docs = ["cats like milk", "dogs like walks", "cats and dogs"]
count = CountVectorizer()
count_matrix = count.fit_transform(docs)
print(count.get_feature_names_out())
print(count_matrix.toarray())

tfidf = TfidfVectorizer()
tfidf_matrix = tfidf.fit_transform(docs)
print(tfidf_matrix.toarray())
```

A term appearing in every document is less useful for distinguishing documents, so IDF downweights it relative to rarer terms. Exact values depend on the vectorizer's formula and settings.

## Exercise 5 — Train a baseline classifier

Use the actual metrics from your notebook run. A false positive means the model predicts the positive class when the true label is negative. A scikit-learn pipeline fits the vocabulary and IDF values as part of model fitting, helping prevent test-set information from leaking into feature construction.

## Exercise 6 — N-grams

Bigrams can capture phrases such as “not good” that unigrams may represent poorly. Adding n-grams increases feature dimensionality, can make the model more memory-intensive, and may introduce sparse features that occur too rarely to generalize.

## Exercise 7 — Class imbalance

A classifier that always predicts the majority class can reach 95% accuracy while detecting none of the minority examples. Inspect per-class precision and recall, F1-score, the confusion matrix, and potentially precision-recall curves. Options include class weights and resampling within training data only. Keep the test set untouched and representative.

## Exercise 8 — Error analysis

Review actual misclassified examples. Label each error category, then make one targeted change, such as adding bigrams for negation or improving normalization for spelling variation. Compare the change on validation data and retain it only if it improves the intended objective without unacceptable trade-offs.

## Exercise 9 — Mini project checklist

A complete submission should document labels, split strategy, a training-only TF-IDF pipeline, classifier choice, per-class metrics, confusion matrix, error analysis, limitations, and privacy considerations. Do not fit the vectorizer on the entire dataset before splitting.
