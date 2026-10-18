# Model Serialization

Learn how to save trained machine-learning models to disk and load them later for inference.

## Learning objectives

By the end of this module, you should be able to:

- Explain model serialization and deserialization.
- Save and load Python objects with `pickle`.
- Persist scikit-learn estimators and pipelines with `joblib`.
- Compare serialization formats and choose an appropriate option.
- Validate a loaded model and preserve the metadata needed to use it safely.
- Recognize security, compatibility, and reproducibility risks.

## Key concepts

### Serialization vs. deserialization

- **Serialization** converts an in-memory object into a storable representation.
- **Deserialization** reconstructs the object from that representation.
- **Inference** uses the restored model to generate predictions.

### Common Python options

| Method | Typical use | Notes |
|---|---|---|
| `pickle` | General Python objects and estimators | Built into Python; only load trusted files |
| `joblib` | NumPy-heavy objects and scikit-learn models | Convenient for many ML workloads; still unsafe for untrusted files |
| ONNX | Interoperable model deployment | Can support runtimes outside Python; conversion support varies |
| JSON | Configuration and simple metadata | Not a general serializer for arbitrary trained estimators |

For this module, examples use `pickle` and `joblib`. They are educational examples, not a recommendation to accept serialized files from unknown sources.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas scikit-learn joblib jupyter
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The notebook creates a small synthetic dataset, so no external dataset download is required.

## Suggested workflow

1. Prepare data and train a model or pipeline.
2. Evaluate the model before saving it.
3. Save the estimator and relevant metadata.
4. Load only artifacts from trusted sources.
5. Run a smoke test and compare predictions with the original model.
6. Track dependency versions and the feature schema alongside the artifact.

## Best practices

- **Never load untrusted pickle/joblib files.** Deserialization can execute arbitrary code.
- Keep model artifacts and metadata versioned together.
- Record Python and library versions; scikit-learn does not guarantee that persisted models work across arbitrary version changes.
- Prefer serializing a complete preprocessing-plus-model `Pipeline` so inference uses the same transformations as training.
- Validate input feature names, order, types, and missing-value behavior.
- Store metrics and a model version or creation timestamp in a separate JSON metadata file.
- Avoid committing large or sensitive artifacts to Git. Use an approved artifact store for production.
- Treat serialization as persistence, not as a security boundary.

## Repository structure

```text
02-Model-Serialization/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Practice

Complete the exercises in [`practice.md`](practice.md), then compare your work with [`solution.md`](solution.md).

## References

- Python `pickle`: https://docs.python.org/3/library/pickle.html
- joblib persistence: https://joblib.readthedocs.io/en/latest/persistence.html
- scikit-learn model persistence: https://scikit-learn.org/stable/model_persistence.html
