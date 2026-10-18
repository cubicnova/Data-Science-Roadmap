# 06 — Model Versioning

Learn how to identify, compare, approve, release, and roll back machine-learning models throughout their lifecycle.

## Learning objectives

By completing this module, you will be able to:

- Explain why model versioning is more than naming files `model_final_v2.pkl`.
- Version model artifacts, code, data references, features, configuration, and evaluation results.
- Design a model metadata manifest and release identifier.
- Use Git for source code and understand when Git LFS or external artifact storage is more appropriate for large files.
- Understand experiment tracking and model registries with MLflow.
- Compare candidate models using consistent datasets and evaluation protocols.
- Promote models through candidate, staging, production, and archived states.
- Design reproducible releases, rollback procedures, and audit trails.
- Identify security, privacy, compatibility, and governance risks in model artifacts.

## Why model versioning matters

A deployed model is not just a binary file. Its behavior depends on the training data, feature definitions, preprocessing, code, dependencies, parameters, threshold, and runtime environment. If an incident occurs, the team should be able to answer:

- Which model version served this prediction?
- Which data snapshot and code revision produced it?
- What evaluation results approved it?
- Which feature schema and threshold were used?
- Can the previous release be restored safely?

## What should be versioned?

| Item | Example identifier |
|---|---|
| Model artifact | `risk-model/1.4.0` |
| Source code | Git commit SHA |
| Training data | Immutable snapshot or dataset version |
| Feature schema | `customer-features/v3` |
| Parameters/configuration | Versioned YAML/JSON or tracked run |
| Dependencies | Lock file or container image digest |
| Evaluation | Metrics report tied to model and dataset |
| Serving release | Container digest + model version + config version |

Do not store secrets in metadata. For large/private datasets, version the immutable reference and access policy rather than committing raw data to Git.

## Recommended workflow

1. Create a reproducible training run and record its inputs.
2. Evaluate the candidate on a predefined validation protocol.
3. Save the model artifact and a metadata manifest.
4. Run compatibility, security, and integration tests.
5. Register the candidate with evaluation evidence.
6. Promote it to staging, then production after approval.
7. Record deployment time, serving image, configuration, and model version.
8. Monitor quality and operational metrics.
9. Roll back to a known-good release when release criteria are violated.

## Semantic versioning for models

A model version such as `2.1.0` can be interpreted as:

- **Major:** incompatible input/output contract or substantial behavior change.
- **Minor:** backward-compatible capability or model update under the team's policy.
- **Patch:** metadata, packaging, or compatible fix with no intended contract break.

This is a convention, not a universal standard. Define what each component means in your team. Never rely on the version string alone; use an immutable artifact digest or registry ID as well.

## Model manifest example

```json
{
  "model_name": "customer-risk",
  "model_version": "1.4.0",
  "artifact_uri": "registry://customer-risk/1.4.0",
  "artifact_sha256": "REPLACE_WITH_REAL_SHA256",
  "code_commit": "REPLACE_WITH_GIT_COMMIT_SHA",
  "data_version": "customer-snapshot-2026-10-01",
  "feature_schema_version": "customer-features-v3",
  "training_run_id": "REPLACE_WITH_RUN_ID",
  "created_at_utc": "2026-10-18T00:00:00Z",
  "metrics": {
    "precision": 0.91,
    "recall": 0.88
  },
  "approval_status": "candidate"
}
```

The values above are illustrative placeholders, not measured results. Store the manifest alongside the artifact and validate it during release.

## Tools and their roles

- **Git:** source code, configuration, tests, manifests, and small text assets.
- **Git LFS:** large binary files where repository hosting and quota policies support it.
- **DVC or dataset registry:** versioned data references and reproducible data pipelines.
- **MLflow Tracking:** parameters, metrics, run IDs, and artifacts.
- **MLflow Model Registry or a platform registry:** model versions, lineage, tags, aliases/stages, and promotion workflow.
- **Object storage/artifact registry:** immutable, access-controlled model artifacts.
- **Container registry:** versioned serving images, ideally referenced by digest.

Tools overlap, but they solve different parts of the lineage problem. Choose a simple system that the team can operate reliably.

## Notebook example

`examples.ipynb` demonstrates a local versioning workflow:
- Train a pipeline on a synthetic dataset.
- Record model metadata and evaluation metrics.
- Compute a SHA-256 digest for the saved artifact.
- Compare candidate metadata and enforce a simple promotion gate.
- Show a lightweight local registry layout.

The notebook does not require a cloud service or MLflow server.

## Setup

Python 3.10+ is recommended:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install numpy pandas scikit-learn joblib
```

Optional tooling:

```powershell
pip install mlflow dvc
```

Use the tool's official installation and configuration instructions for team or cloud environments.

## Production checklist

- [ ] Model version and artifact digest are immutable and recorded.
- [ ] Code, data, feature schema, configuration, and dependencies are traceable.
- [ ] Metrics are tied to a known evaluation dataset and protocol.
- [ ] Promotion gates are automated where practical.
- [ ] Artifacts come from trusted, access-controlled storage.
- [ ] Registry permissions and audit logs are configured.
- [ ] Serving API and feature schema compatibility are tested.
- [ ] Rollback target and database/feature compatibility are documented.
- [ ] Secrets and sensitive training data are not embedded in artifacts or manifests.
- [ ] Retention, deprecation, and archival policies are defined.

## Further reading

- [MLflow Tracking](https://mlflow.org/docs/latest/ml/tracking/)
- [MLflow Model Registry](https://mlflow.org/docs/latest/ml/model-registry/)
- [DVC documentation](https://dvc.org/doc)
- [Git LFS](https://git-lfs.com/)
- [scikit-learn model persistence](https://scikit-learn.org/stable/model_persistence.html)
