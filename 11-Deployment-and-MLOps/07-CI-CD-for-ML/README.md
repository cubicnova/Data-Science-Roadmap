# 07 — CI/CD for Machine Learning

Learn to automate testing, validation, packaging, and controlled release of machine-learning code and models using Continuous Integration (CI) and Continuous Delivery/Deployment (CD).

## Learning objectives

- Explain CI, continuous delivery, and continuous deployment.
- Design a pipeline for code, data, model, and API quality checks.
- Run unit tests, linting, type checks, schema validation, and model evaluation gates.
- Use GitHub Actions to automate checks on pushes and pull requests.
- Build immutable artifacts and container images with release metadata.
- Separate development, staging, and production environments.
- Use approval gates, canary releases, rollback procedures, and deployment monitoring.
- Identify risks such as secrets in logs, untrusted pull-request code, mutable tags, and unvalidated model artifacts.

## CI/CD in ML

Traditional software CI/CD focuses heavily on source code and application tests. ML pipelines must also consider data and model behavior.

A typical pipeline:

```text
Pull request
   ↓
Lint + type checks + unit tests
   ↓
Data/schema checks + pipeline tests
   ↓
Train/evaluate candidate (when appropriate)
   ↓
Quality/security gates
   ↓
Build immutable package/image + provenance
   ↓
Deploy to staging → smoke/load tests
   ↓
Approval or automated promotion
   ↓
Production rollout → monitor → rollback if needed
```

Do not retrain an expensive model on every tiny code change by default. Use change-aware workflows, cached dependencies, scheduled training, or dedicated training pipelines when appropriate.

## CI vs CD

- **Continuous Integration:** frequently merge changes and automatically verify them.
- **Continuous Delivery:** keep changes deployable; production release may require a human approval.
- **Continuous Deployment:** automatically release every change that passes required checks.

Teams may use “CD” for either delivery or deployment. Document the exact policy.

## What should be tested?

### Code and application
- Formatting, linting, type checks, and unit tests.
- API contract and integration tests.
- Dependency and container vulnerability scans.
- Secret scanning and license/policy checks.

### Data and features
- Required columns, types, ranges, null rates, freshness, and schema compatibility.
- Duplicate/invalid records and unexpected categories.
- Train/serve feature parity and leakage checks.

### Model
- Reproducible evaluation against a known dataset.
- Task-appropriate metrics, calibration, subgroup checks where relevant.
- Model artifact loadability and input/output contract.
- Latency, memory, and quality gates.

A metric threshold is only meaningful when its dataset, split, metric definition, and evaluation protocol are documented.

## Example GitHub Actions workflow

Create `.github/workflows/ci.yml`:

```yaml
name: Python CI

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
          cache: pip
      - run: python -m pip install --upgrade pip
      - run: pip install -r requirements.txt
      - run: pip install pytest ruff
      - run: ruff check .
      - run: pytest -q
```

Pin third-party actions to reviewed commit SHAs in higher-assurance environments. Keep permissions minimal, especially for workflows that run code from pull requests. Do not expose deployment secrets to untrusted fork pull requests.

## Model quality gate example

```python
def quality_gate(metrics: dict) -> bool:
    return (
        metrics["recall"] >= 0.80
        and metrics["f1"] >= 0.75
    )
```

The values above are examples only. Choose thresholds before evaluating a release candidate and keep the final test set separate from model selection.

## Artifact and release traceability

Every release should identify:
- source commit,
- data snapshot/reference,
- feature schema and preprocessing version,
- training run ID and configuration,
- model artifact digest,
- dependency lock file,
- container image digest,
- evaluation report and approval,
- deployment timestamp and rollback target.

Prefer immutable artifact/image digests over relying only on mutable tags such as `latest`.

## Environment strategy

- **Development:** rapid iteration, synthetic or approved sample data.
- **CI:** isolated, reproducible checks with no production credentials.
- **Staging:** production-like configuration, safe test data, smoke/load/security checks.
- **Production:** controlled identity, minimal permissions, monitoring, audit logs, and rollback.

Promotion should move the same tested artifact between environments where practical, rather than rebuilding different artifacts without traceability.

## Setup

Python 3.10+ is recommended:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install numpy pandas scikit-learn pytest ruff
```

Optional tooling:

```powershell
pip install fastapi httpx mlflow
```

GitHub Actions runs in GitHub-hosted runners when configured for a GitHub repository. Local commands and notebook examples do not require a cloud deployment account.

## Security and reliability checklist

- Use least-privilege workflow permissions.
- Store secrets in repository/environment secrets or an approved secret manager.
- Never print tokens or include secrets in build arguments or container layers.
- Treat pull-request code and artifacts as untrusted until validated.
- Pin dependencies and reviewed CI actions; scan dependencies and images.
- Validate artifact provenance and digest before deployment.
- Require approval for production environments where appropriate.
- Use health checks, staged rollout, monitoring, and a tested rollback path.
- Keep audit logs for release approvals and deployments.

## Mini-project

Build a CI pipeline for a small scikit-learn model or FastAPI inference service. The pipeline should lint code, run tests, validate the model contract, evaluate candidate metrics, build a package or image, and document how staging/production promotion and rollback work.

## Definition of done

- [ ] CI runs on pushes and pull requests.
- [ ] Tests fail the pipeline when a regression is introduced.
- [ ] Model/data checks use documented inputs and thresholds.
- [ ] Artifact metadata identifies source and model versions.
- [ ] Secrets are not available to untrusted workflows.
- [ ] Deployment requires appropriate quality gates and approvals.
- [ ] Monitoring and rollback are documented.

## Further reading

- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [GitHub Actions security hardening](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [pytest documentation](https://docs.pytest.org/)
- [Ruff documentation](https://docs.astral.sh/ruff/)
- [MLflow documentation](https://mlflow.org/docs/latest/)
