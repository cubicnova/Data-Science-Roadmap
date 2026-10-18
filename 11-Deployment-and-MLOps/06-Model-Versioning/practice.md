# Practice — Model Versioning

Complete these exercises before consulting `solution.md`. Use synthetic or approved data only. Never load model artifacts from untrusted sources.

## 01. Why version models?

Give four operational questions that a model versioning system should answer during an incident.

## 02. Model versus code version

Explain why a Git commit alone does not fully identify a trained model.

## 03. Artifact inventory

List the artifacts and metadata you would preserve for a fraud model release.

## 04. Semantic versioning

Define major, minor, and patch rules for a team that serves a stable prediction API.

## 05. Immutable identifiers

Explain why a human-readable version string should be paired with a digest or immutable registry ID.

## 06. Metadata manifest

Create a JSON manifest containing model name/version, artifact URI, code commit, data version, feature schema, metrics, and approval status.

## 07. Metadata validation

Write checks that reject a manifest with a missing version, invalid metric, or empty data reference.

## 08. Artifact hashing

Calculate a SHA-256 digest for a model file and explain how the digest supports integrity checking.

## 09. Trusted artifacts

Explain the risk of loading a pickle/joblib artifact from an untrusted location.

## 10. Data lineage

Design a scheme for referencing training and evaluation snapshots without committing private data to Git.

## 11. Feature schema versioning

Describe what should happen when a new model requires a feature that the current API does not provide.

## 12. Dependency lineage

Explain why the same model artifact can behave differently under incompatible library versions.

## 13. Experiment tracking

Identify parameters, metrics, artifacts, and tags to log for each training run.

## 14. MLflow run

Write pseudocode that logs a model parameter, validation metric, and artifact under an MLflow run.

## 15. Registry purpose

Compare experiment tracking with a model registry.

## 16. Promotion states

Define candidate, staging, production, and archived states, including entry criteria for each.

## 17. Evaluation comparability

Explain why candidate and production metrics must use comparable datasets and evaluation procedures.

## 18. Model comparison

Compare two model versions on recall, precision, latency, memory, and operational cost.

## 19. Quality gates

Define an example promotion gate that includes minimum recall, maximum latency, and no critical test failures.

## 20. Approval workflow

Design a human approval process for a model used in a high-impact decision.

## 21. Release manifest

Create a release manifest linking model version, API version, container digest, feature schema, and configuration.

## 22. Canary deployment

Explain how to route a small percentage of traffic to a candidate model and what to monitor.

## 23. Shadow deployment

Explain shadow testing and why shadow predictions should not affect user-visible decisions.

## 24. Rollback planning

Write steps to roll back a model after error rates and business metrics degrade.

## 25. Compatibility checks

Design tests for model input schema, output schema, and preprocessing compatibility.

## 26. Backward compatibility

A new model changes the score range and adds a required feature. Explain the migration risks.

## 27. Artifact storage

Compare Git, Git LFS, object storage, and a model registry for code, datasets, binaries, and lifecycle metadata.

## 28. Access control

Specify who should be able to upload, approve, promote, and delete model versions.

## 29. Audit trail

List events that should be audited during training, registration, promotion, and rollback.

## 30. Retention policy

Design a retention and archival policy that balances audit needs, cost, and privacy.

## 31. Reproducible retraining

List the information needed to reproduce a model training run as closely as practical.

## 32. Drift and versioning

Explain how monitoring signals should be associated with the exact model version that served traffic.

## 33. Model deprecation

Plan how to deprecate a model version while avoiding breaking existing clients.

## 34. Security review

Identify risks involving artifact tampering, dependency compromise, leaked data, and unauthorized promotion.

## 35. Privacy review

Explain why training examples or personally identifiable information should not be embedded in model metadata.

## 36. Model card

Draft the key sections of a model card that accompanies a release.

## 37. Registry migration

Plan a migration from local model files to a central model registry.

## 38. Incident analysis

A prediction is disputed six weeks after release. Explain how lineage helps investigate it.

## 39. Capstone

Create a local model-versioning workflow with metadata, artifact digest, evaluation gate, and rollback plan.

## 40. Review and reflection

Write a short checklist your team would require before promoting a model to production.

## Mini-project deliverables

1. A reproducible training script or notebook.
2. A model artifact saved from a trusted training run.
3. A metadata manifest containing code/data/schema lineage and evaluation metrics.
4. An artifact SHA-256 digest and a verification step.
5. A promotion gate with explicit quality criteria.
6. A version comparison report.
7. A rollback and retention policy.
8. Optional: MLflow Tracking/Registry integration.

## Self-review

- [ ] Model identity is immutable and traceable.
- [ ] Metrics are linked to a named evaluation dataset and protocol.
- [ ] Training and serving preprocessing are consistent.
- [ ] Candidate promotion has explicit quality and compatibility checks.
- [ ] Rollback has been planned and tested.
- [ ] Artifact access is controlled and provenance is verified.
- [ ] Sensitive data and secrets are excluded from metadata and logs.
