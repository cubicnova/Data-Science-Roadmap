# Solutions — Model Versioning

Reference answers and illustrative code. Replace placeholder metrics and identifiers with values from actual runs.

## 01. Why version models?
Examples: which model produced a prediction; what data and code trained it; which metrics approved it; which artifact and feature schema were deployed; and how to restore the previous release.

## 02. Model versus code version
Training is data-dependent and may be stochastic. The same source code can produce different artifacts from different data, dependencies, parameters, seeds, or feature logic. Track the code commit alongside data references, configuration, environment, run ID, and artifact digest.

## 03. Artifact inventory
Preserve the model/pipeline artifact, feature schema, training configuration, data snapshot references, code commit, dependency lock file, training run ID, evaluation report, model card, artifact digest, and approval/deployment records.

## 04. Semantic versioning
One possible policy: major for incompatible API/feature contracts; minor for a new approved model with compatible contract; patch for packaging or metadata fixes that do not change intended behavior. Document the policy and pair the version with an immutable artifact identifier.

## 05. Immutable identifiers
Human-readable versions are easy to communicate but may be reused or mislabeled. A registry ID or cryptographic digest identifies the exact artifact bytes and supports integrity checks.

## 06. Metadata manifest
```json
{
  "model_name": "fraud-detector",
  "model_version": "1.2.0",
  "artifact_uri": "registry://fraud-detector/1.2.0",
  "artifact_sha256": "replace-with-real-digest",
  "code_commit": "replace-with-git-sha",
  "data_version": "fraud-data-snapshot-v4",
  "feature_schema_version": "features-v2",
  "metrics": {"precision": 0.90, "recall": 0.87},
  "approval_status": "candidate"
}
```
Numbers are illustrative only.

## 07. Metadata validation
Check required keys, nonempty strings, semantic version format, valid URI, digest format, metric ranges, and an allowed status enum. Fail release validation if required lineage fields are absent.

## 08. Artifact hashing
```python
import hashlib
from pathlib import Path

def sha256_file(path):
    digest = hashlib.sha256()
    with open(path, "rb") as file:
        for chunk in iter(lambda: file.read(1024 * 1024), b""):
            digest.update(chunk)
    return digest.hexdigest()

print(sha256_file("model.joblib"))
```
Compare against a digest obtained from a trusted manifest or registry. A digest cannot prove provenance if an attacker can replace both the artifact and the expected digest.

## 09. Trusted artifacts
Pickle-based formats can execute code while loading. Use trusted, access-controlled artifact storage, verify provenance and digests, restrict who can publish artifacts, and consider safer formats such as ONNX or `skops` where they fit the model and threat model.

## 10. Data lineage
Use immutable dataset IDs, object-store version IDs, DVC hashes, or governed data catalog references. Record snapshot date, schema, query/code version, and access policy. Do not put private raw data in a public Git repository.

## 11. Feature schema versioning
Treat schema changes as compatibility changes. Validate the incoming schema, maintain backward compatibility where possible, deploy producer/consumer changes in a safe order, and reject unsupported schema versions clearly.

## 12. Dependency lineage
Serialization formats and library internals can change across versions. Pin and test dependencies, record runtime versions, and validate load/prediction behavior in the target container before promotion.

## 13. Experiment tracking
Log parameters, data reference, split strategy, random seed, code commit, library versions, metrics, plots/confusion matrix, model artifact, feature schema, and run notes.

## 14. MLflow run
```python
import mlflow

mlflow.set_experiment("fraud-detector")
with mlflow.start_run():
    mlflow.log_param("model_type", "logistic_regression")
    mlflow.log_param("random_state", 42)
    mlflow.log_metric("validation_recall", float(recall))
    mlflow.log_artifact("model_manifest.json")
```
The variables and file must exist in the actual training workflow.

## 15. Tracking vs registry
Tracking records individual experiments and their parameters, metrics, and artifacts. A registry manages named model versions and their lifecycle metadata, lineage, approvals, and promotion.

## 16. Promotion states
Candidate: training completed, awaiting checks. Staging: integration, security, and performance checks pass. Production: approved and serving traffic. Archived: no longer promoted by default but retained for audit/recovery.

## 17. Evaluation comparability
Differences may reflect changed test data or measurement rather than a better model. Use a documented, representative evaluation protocol and keep a final test set separate from tuning.

## 18. Model comparison
Compare predictive metrics, calibration, latency percentiles, memory/CPU, throughput, failure modes, interpretability, cost, and fairness/safety requirements. Select according to agreed business and risk criteria, not one metric alone.

## 19. Quality gates
Example: recall must exceed a pre-agreed threshold, p95 latency must stay below the service objective, all schema/security/integration tests must pass, and no critical vulnerability may remain. Thresholds should be chosen with stakeholders.

## 20. Approval workflow
Require documented evaluation, data lineage, risk assessment, test evidence, and reviewer approval. High-impact models may need independent validation, fairness review, and formal sign-off before promotion.

## 21. Release manifest
```json
{
  "release_id": "risk-api-2026.10.18.1",
  "model_version": "1.4.0",
  "artifact_digest": "sha256:replace-with-real-digest",
  "api_version": "v1",
  "container_digest": "sha256:replace-with-image-digest",
  "feature_schema_version": "v3",
  "config_version": "risk-config-12"
}
```

## 22. Canary deployment
Route a small, controlled share of traffic to the candidate. Compare errors, latency, resource use, input distribution, and appropriate outcome metrics against the current model. Define abort thresholds and enough observation time before increasing traffic.

## 23. Shadow deployment
Run the candidate on copied or mirrored requests while only the production model's response affects users. Compare outputs and resource impact. Protect sensitive data and ensure shadow requests cannot trigger duplicate side effects.

## 24. Rollback planning
Stop further rollout; confirm the incident and affected release; route traffic to the last known-good immutable model/image/config; verify schema compatibility and health; monitor recovery; preserve logs and artifacts for investigation; communicate and document root cause.

## 25. Compatibility checks
Test required feature names/types/ranges, preprocessing behavior, prediction output schema, artifact loading, dependency compatibility, representative golden cases, and API contract compatibility.

## 26. Backward compatibility
A required feature can cause older clients to fail. A changed score range can invalidate thresholds and downstream assumptions. Introduce schema/API versions or a compatibility layer, recalibrate consumers, document the migration, and test rollback compatibility.

## 27. Artifact storage
- Git: code, tests, manifests, small text files.
- Git LFS: large files when supported and governed.
- Object storage: scalable artifact/data blobs with access control and retention.
- Model registry: model versions, lineage, aliases/stages, and promotion metadata.
They can be combined.

## 28. Access control
Training jobs may create candidates; reviewers approve; release automation promotes; runtime identities read production artifacts; only a small administrative group can delete or alter retention. Log every privileged action.

## 29. Audit trail
Record training run IDs, input/data versions, artifact digest, evaluations, approval identity/time, promotion and deployment events, configuration changes, access to artifacts, rollbacks, and archival/deletion events.

## 30. Retention policy
Retain production releases, manifests, evaluation evidence, and audit records according to legal/business policy. Archive superseded artifacts; delete only under approved retention rules. Apply data minimization and access controls.

## 31. Reproducible retraining
Capture source commit, data snapshot, feature code, preprocessing, parameters, random seeds, dependency lock file, runtime/hardware where relevant, training command, and evaluation protocol. Exact bit-for-bit reproducibility may not be possible on all hardware/libraries.

## 32. Drift and versioning
Attach model version, release ID, feature schema, and deployment timestamp to metrics/log events. Compare drift and performance by version and segment so incidents can be attributed to a release rather than aggregated across models.

## 33. Model deprecation
Announce the end-of-support date, publish a migration guide, provide a compatibility period if needed, monitor old-version usage, notify owners, and remove the old route/artifact only after policy and retention requirements are met.

## 34. Security review
Protect artifacts against tampering, scan dependencies, verify build provenance, restrict registry permissions, use signed/digested images, prevent secret leakage, minimize data, and require approval for production promotion.

## 35. Privacy review
Metadata should describe lineage and evaluation without storing raw personal records, credentials, or unnecessary sensitive fields. Link to governed data systems with access control instead of embedding raw samples.

## 36. Model card
Include intended use, out-of-scope uses, training/evaluation data summary, metrics and limitations, subgroup analysis where appropriate, known risks, threshold, version/lineage, owner, approval date, and monitoring/retraining policy.

## 37. Registry migration
Inventory existing files and metadata; calculate digests; create manifests; upload artifacts to controlled storage; register versions and lineage; validate load/prediction parity; switch consumers gradually; retain rollback copies until migration is verified.

## 38. Incident analysis
The release ID connects the disputed prediction to the exact model artifact, features/schema, code/data lineage, configuration, and evaluation evidence. Request IDs and privacy-safe logs help reconstruct the serving path without logging sensitive raw data unnecessarily.

## 39. Capstone
Train a model, save it, create a manifest, hash the artifact, validate the manifest, evaluate a candidate against a threshold, register the release, and document how to switch to a prior digest. Keep all identifiers and metrics tied to actual run outputs.

## 40. Review and reflection
Before production, require immutable identity, trusted provenance, evaluation evidence, schema compatibility, security checks, explicit approval, monitoring, rollback, and retention ownership.
