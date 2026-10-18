# Practice — CI/CD for ML

Complete the exercises before consulting `solution.md`. Use a test repository and synthetic data. Do not expose production credentials in CI experiments.

## 01. CI/CD definitions

Explain CI, continuous delivery, and continuous deployment using an ML project example.

## 02. ML-specific CI/CD

List four ways ML pipelines differ from ordinary software pipelines.

## 03. Pipeline stages

Design a sequence of checks from pull request to production release.

## 04. Test pyramid

Separate unit, integration, contract, and end-to-end tests for a prediction API.

## 05. Repository setup

Create a minimal Python project layout with `src/`, `tests/`, `requirements.txt`, and a workflow directory.

## 06. Dependency management

Explain why CI should install declared dependencies from a controlled file or lock file.

## 07. Linting

Add Ruff to a project and configure CI to fail when lint checks fail.

## 08. Unit testing

Write a unit test for a preprocessing or feature transformation function.

## 09. Integration testing

Test an inference API endpoint using a test client without deploying a public server.

## 10. Pull request triggers

Configure a workflow to run on pushes to `main` and pull requests targeting `main`.

## 11. GitHub Actions anatomy

Explain workflow, event, job, runner, step, action, and artifact.

## 12. Permissions

Set the workflow's repository permissions to the minimum needed for tests.

## 13. Secrets

Explain why deployment secrets must not be exposed to untrusted pull-request code.

## 14. Action pinning

Explain the risk of using mutable third-party action tags and how reviewed SHA pinning helps.

## 15. Data schema checks

Write checks for required columns, data types, and null-rate thresholds.

## 16. Data leakage

Describe a CI check or review rule that can help detect leakage between train and test.

## 17. Model evaluation

Design a model quality gate with recall and F1 thresholds and explain how thresholds should be chosen.

## 18. Test-set discipline

Explain why the final test set should not be used repeatedly to tune candidate models.

## 19. Artifact metadata

Create a manifest linking model version, source commit, data snapshot, metric report, and artifact digest.

## 20. Artifact integrity

Explain how a digest can detect changes and why it does not by itself prove trusted provenance.

## 21. Model load test

Design a CI test that verifies the model artifact loads and accepts a representative input.

## 22. API contract check

Test that prediction responses include the expected fields and types.

## 23. Build artifacts

Describe what should be packaged and what should be excluded from a model-serving artifact.

## 24. Container build

Write a Dockerfile for an API with a non-root user and no embedded secrets.

## 25. Container scanning

Propose a policy for critical vulnerabilities in base images and dependencies.

## 26. Environment separation

Explain differences among development, CI, staging, and production credentials and data.

## 27. Continuous delivery vs deployment

Choose which approach fits a high-impact model and justify the approval policy.

## 28. Deployment approvals

Design a production approval gate with owner, evidence, and audit record.

## 29. Canary release

Plan a gradual rollout of a new model version and define abort criteria.

## 30. Blue/green deployment

Explain how blue/green deployment helps rollback and what state compatibility must be checked.

## 31. Rollback

Write a runbook for reverting a model/API release after latency and error rates rise.

## 32. Monitoring after release

Choose service and model metrics to watch after deployment.

## 33. Scheduled retraining

Design a workflow for periodic retraining that still requires model quality gates.

## 34. Change-aware workflows

Explain which changes should trigger code tests, model training, image builds, or deployment.

## 35. Caching

Explain how dependency caching improves CI speed and why cache keys should include dependency files.

## 36. Reproducibility

List environment, code, data, seed, and configuration details needed to reproduce a release.

## 37. Supply-chain security

Identify risks from compromised dependencies, untrusted artifacts, and excessive workflow permissions.

## 38. Failure diagnosis

A CI pipeline fails only on the hosted runner. Create a debugging checklist.

## 39. Capstone

Build a CI pipeline for a FastAPI ML service with tests, model checks, and a release plan.

## 40. Review checklist

Write a pre-production checklist covering quality, security, traceability, rollout, and rollback.

## Mini-project deliverables

1. A small Python or FastAPI ML project.
2. Automated linting and unit/integration tests.
3. Data/schema and model quality checks.
4. A GitHub Actions workflow for pull requests and main-branch pushes.
5. An artifact manifest and version/digest strategy.
6. A staging-to-production promotion policy.
7. A monitoring and rollback runbook.
8. A security review of workflow permissions, dependencies, and secrets.

## Self-review checklist

- [ ] CI is reproducible and fails on test regressions.
- [ ] Model metrics are measured on a documented evaluation split.
- [ ] Secrets are isolated from untrusted pull-request workflows.
- [ ] Artifacts are versioned and their provenance is checked.
- [ ] Production deployment is gated and auditable.
- [ ] Post-deployment monitoring and rollback are defined.
