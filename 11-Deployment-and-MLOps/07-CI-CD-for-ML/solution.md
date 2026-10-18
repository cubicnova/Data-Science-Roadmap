# Solutions — CI/CD for ML

Reference answers and example implementations. Adapt the pipeline to the risk, data sensitivity, and scale of your application.

## 01. CI/CD definitions
CI automatically validates changes when they are integrated. Continuous delivery keeps tested changes ready for release, often with manual production approval. Continuous deployment automatically releases changes that pass all required checks.

## 02. ML-specific CI/CD
ML pipelines must validate data and feature schemas, evaluate model quality, track data/model lineage, check reproducibility and artifact compatibility, and monitor drift or delayed-label performance after release.

## 03. Pipeline stages
A practical sequence is checkout → dependency install → lint/type checks → unit/integration tests → schema/data checks → model evaluation/quality gate → security scans → artifact/image build → staging deploy → smoke tests → approval → production rollout → monitor/rollback.

## 04. Test pyramid
Unit tests cover pure functions and preprocessing. Integration tests exercise model loading, database/storage access, and route behavior. Contract tests validate request/response schemas. End-to-end tests validate the deployed workflow in a controlled environment.

## 05. Repository setup
```text
ml-service/
├── src/
├── tests/
├── requirements.txt
├── pyproject.toml
├── Dockerfile
└── .github/workflows/ci.yml
```

## 06. Dependency management
A declared dependency file makes CI setup explicit. A tested lock file or exact pins improve reproducibility and reduce unexpected breakage. Upgrade dependencies deliberately and run tests/scans before releasing.

## 07. Linting
```bash
pip install ruff
ruff check .
```
In GitHub Actions, run `ruff check .` as a step; a nonzero exit code fails the job.

## 08. Unit testing
```python
def normalize(values):
    total = sum(values)
    if total == 0:
        raise ValueError("sum must not be zero")
    return [value / total for value in values]

def test_normalize():
    assert normalize([1, 1]) == [0.5, 0.5]
```

## 09. Integration testing
For FastAPI, use `fastapi.testclient.TestClient` to call endpoints in-process. Load a known trusted test artifact or dependency override and assert status codes, schema, and expected behavior.

## 10. Pull request triggers
```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
permissions:
  contents: read
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: python -m pip install -r requirements.txt
      - run: python -m pip install pytest
      - run: pytest -q
```
For higher assurance, pin actions to reviewed full commit SHAs and keep workflow permissions minimal.

## 11. GitHub Actions anatomy
A workflow is the YAML automation definition; an event triggers it; a job runs on a runner; steps execute commands or actions; an action is a reusable unit; artifacts are files preserved or passed between jobs/runs.

## 12. Permissions
Start with `permissions: contents: read` for test-only workflows. Add only required permissions for publishing packages or deploying. Separate privileged deployment workflows from untrusted PR validation where practical.

## 13. Secrets
Fork PR code can be attacker-controlled. Do not make production secrets available to it. Use protected environments, approvals, least-privilege credentials, short-lived identity federation where supported, and masked secret storage.

## 14. Action pinning
A mutable tag can point to changed code later. Pinning a reviewed full commit SHA fixes the action revision used by the workflow. Maintain an update process to review and refresh pinned versions.

## 15. Data schema checks
```python
required = {"age", "income", "label"}
missing = required - set(df.columns)
assert not missing, f"Missing columns: {missing}"
assert df["age"].notna().mean() >= 0.99
assert df["income"].notna().mean() >= 0.99
```
Add type, range, freshness, duplicate, category, and privacy checks appropriate to the dataset.

## 16. Data leakage
Keep preprocessing inside a pipeline fit only on training data. Add tests that split data before fitting transformations, review time/target-derived features, and ensure duplicated entities do not cross splits where that would invalidate evaluation.

## 17. Model evaluation
```python
def quality_gate(metrics):
    return metrics["recall"] >= 0.80 and metrics["f1"] >= 0.75
```
These are example thresholds, not universal targets. Choose thresholds before evaluating the release candidate and tie results to a documented dataset and metric definition.

## 18. Test-set discipline
Repeatedly selecting models based on the final test set leaks information from that set into the development process. Use training/validation data for selection and reserve the test set for final confirmation.

## 19. Artifact metadata
```json
{
  "model_version": "1.2.0",
  "source_commit": "replace-with-git-sha",
  "data_version": "snapshot-v4",
  "feature_schema_version": "features-v2",
  "metrics_report": "evaluation.json",
  "artifact_sha256": "replace-with-real-digest"
}
```

## 20. Artifact integrity
A digest detects byte changes when compared with an expected trusted digest. If an attacker can replace both artifact and digest, the digest alone does not establish origin; use access control, signed provenance, and trusted build pipelines.

## 21. Model load test
Load the artifact in a clean CI environment, check expected input schema, run a representative fixture through `predict` or the inference API, and assert output shape, types, finite scores, and acceptable latency if relevant. Only load trusted artifacts.

## 22. API contract check
Assert status code, content type, required response fields, types/ranges, model version, and documented error response for invalid input. Use a schema-aware test when possible.

## 23. Build artifacts
Include application code, pinned dependencies, trusted model artifact, feature schema, and release metadata. Exclude `.env`, credentials, raw private datasets, caches, training-only files, and unnecessary notebook outputs.

## 24. Container build
```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt && useradd --create-home appuser
COPY main.py .
USER appuser
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
Use tested dependency versions, a `.dockerignore`, image scanning, and trusted model artifacts. Never bake secrets into image layers.

## 25. Container scanning
Fail or block promotion for critical/high vulnerabilities according to an agreed policy, with documented exceptions, owners, and deadlines. Rebuild from patched base images and rescan before release.

## 26. Environment separation
Development uses safe local credentials/data; CI uses isolated short-lived access and synthetic fixtures; staging uses production-like but controlled configuration; production uses protected identities, least privilege, audit logging, and approved data access.

## 27. Continuous delivery vs deployment
For a high-impact model, continuous delivery with explicit approval and independent validation is often appropriate. Lower-risk services may allow automated deployment after strong quality, security, and operational gates.

## 28. Deployment approvals
Require a release ID, evaluation report, test results, security scan status, change summary, rollback target, approver identity, and timestamp. Use protected deployment environments and audit the approval.

## 29. Canary release
Route a small percentage of traffic to the new immutable release. Compare latency, errors, resource use, prediction distribution, and delayed-label outcomes where available. Abort if agreed safety or service thresholds are breached.

## 30. Blue/green deployment
Run two environments and switch traffic after checks pass. Rollback can switch traffic back, but stateful dependencies, schema changes, queues, and external side effects must remain compatible.

## 31. Rollback
Freeze rollout; confirm the affected release; route traffic to the last known-good image/model/config; verify readiness and smoke tests; monitor recovery; preserve evidence; communicate status; and document root cause and follow-up actions.

## 32. Monitoring after release
Track request rate, p95/p99 latency, 4xx/5xx rates, timeouts, saturation, input schema/drift, prediction distributions, model version, and delayed-label quality. Compare metrics by release and segment.

## 33. Scheduled retraining
Trigger on a schedule or justified data/quality signal, create a versioned dataset snapshot, train in an isolated workflow, evaluate against baselines, enforce quality/security gates, register the candidate, and require approval before production promotion.

## 34. Change-aware workflows
Documentation-only changes may need lint and link checks; application changes need unit/integration tests; data/schema changes need validation and possibly retraining; dependency or Docker changes need image rebuild/scans; production release changes need deployment approval and smoke tests.

## 35. Caching
Cache dependencies to reduce install time. Include the dependency file or lock-file hash in the cache key so dependency changes invalidate stale caches. Do not cache secrets or untrusted build outputs across security boundaries.

## 36. Reproducibility
Record source commit, data snapshot, feature code/schema, parameters, random seed, library/runtime versions, lock file, training command, environment, and evaluation protocol.

## 37. Supply-chain security
Threats include compromised packages/actions, malicious PR changes, exposed secrets, overprivileged tokens, mutable artifacts, and untrusted model files. Mitigate with pinned/reviewed dependencies, minimal permissions, protected environments, provenance checks, scanning, and secret isolation.

## 38. Failure diagnosis
Inspect the failing step and full logs; compare local and runner Python/OS versions; verify dependency pins and environment variables; check paths/case sensitivity; reproduce in a clean environment or container; inspect flaky tests and network dependencies; avoid printing secrets.

## 39. Capstone
Build a FastAPI model service with schema tests, unit/integration tests, linting, an artifact manifest, a quality gate, and a GitHub Actions workflow. Document how the tested image is promoted to staging/production and how to restore the prior immutable release.

## 40. Review checklist
Require passing tests, documented data/model metrics, trusted artifacts, minimal workflow permissions, secret-safe logs, dependency/image scans, explicit approval, health checks, release monitoring, and a tested rollback target.
