# 08 — Deep Learning Projects

This module brings together practical deep-learning workflows in small, reproducible projects. The goal is to practice problem framing, data preparation, model selection, training, evaluation, error analysis, and saving model artifacts—not just to build a neural network.

## Learning objectives

By completing this module, you should be able to:

- Translate a problem statement into a supervised learning task.
- Establish a simple baseline before adding model complexity.
- Build and train a Keras model for tabular classification.
- Track training and validation metrics.
- Evaluate a model on held-out data and inspect its errors.
- Save and reload a trained model.
- Document limitations, fairness considerations, reproducibility, and next steps.
- Plan extensions to computer vision, NLP, time series, and autoencoder projects.

## Project workflow

1. **Problem framing:** define intended use, target, success metric, and constraints.
2. **Data review:** check provenance, permissions, privacy, missing values, labels, and class balance.
3. **Data splitting:** create train/validation/test sets before fitting data-dependent preprocessing.
4. **Baseline:** build a simple benchmark.
5. **Modeling:** choose an architecture that fits the data and available compute.
6. **Training:** record settings and monitor validation performance.
7. **Evaluation:** report suitable metrics and inspect representative errors.
8. **Artifact:** save the model and document how to load it.
9. **Communication:** explain results, limitations, risks, and possible improvements.

## Project menu

| Project | Task | Suggested starting point |
|---|---|---|
| Tabular classification | Predict one of two labels from numeric features | Dense Keras network; compare with logistic regression |
| Image classification | Assign a label to an image | CNN; consider transfer learning for larger datasets |
| Sentiment analysis | Classify text sentiment | TextVectorization + Embedding or a TF-IDF baseline |
| Time-series forecasting | Predict future values | Naive baseline, then a dense or recurrent model |
| Autoencoder | Reconstruct inputs or detect unusual patterns | Encoder-decoder; evaluate reconstruction error carefully |

Choose datasets that you have permission to use. For image and NLP projects, use an appropriate split and avoid near-duplicate leakage. For time series, split chronologically rather than randomly.

## Included practical notebook

`examples.ipynb` implements an end-to-end **synthetic tabular classification project** with TensorFlow/Keras:

- Generates a local dataset (no download required).
- Splits the data into training, validation, and test sets.
- Builds and trains a neural network with early stopping.
- Plots training and validation loss.
- Evaluates held-out test data.
- Saves and reloads the model, then checks prediction consistency.

The synthetic dataset is for learning workflow mechanics only. Its results should not be presented as evidence of real-world model performance.

## Setup

Use a Python version and operating system supported by your TensorFlow release. Check the [official TensorFlow installation guide](https://www.tensorflow.org/install).

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
# source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install tensorflow numpy matplotlib scikit-learn jupyter
jupyter notebook
```

## Deliverable checklist

- [ ] Problem statement and intended use
- [ ] Dataset source, license, and limitations
- [ ] Reproducible split and preprocessing
- [ ] Baseline and model architecture
- [ ] Training configuration and validation plot
- [ ] Test metrics appropriate to the task
- [ ] Error analysis
- [ ] Saved model and inference example
- [ ] Limitations, privacy, fairness, and failure modes
- [ ] Next experiments and how success will be measured

## Further reading

- [TensorFlow tutorials](https://www.tensorflow.org/tutorials)
- [Keras code examples](https://keras.io/examples/)
- [TensorFlow computer vision tutorials](https://www.tensorflow.org/tutorials/images)
- [TensorFlow text and NLP tutorials](https://www.tensorflow.org/tutorials/text)
