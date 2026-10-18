# Practice Exercises — Cloud Deployment

Complete the exercises in order. Use a local environment first; a cloud account is optional.

## Exercise 1 — Choose a deployment target

A team has a small FastAPI model endpoint with unpredictable traffic, a moderate model size, and limited platform engineering support.

1. Compare a VM, managed container service, Kubernetes, and serverless container platform.
2. Recommend one option and justify it using operations, scaling, latency, and cost.
3. Name two conditions that would change your recommendation.

**Deliverable:** a short decision note (150–250 words).

## Exercise 2 — Environment configuration

Write Python code that reads the following configuration from environment variables:

- `APP_ENV`, defaulting to `development`
- `MODEL_PATH`, defaulting to `artifacts/model.joblib`
- `LOG_LEVEL`, defaulting to `INFO`

Requirements:
1. Use safe defaults for non-secret settings.
2. Never print secret values.
3. Explain how production secrets should be supplied.

**Deliverable:** a small `config.py` file or notebook cell.

## Exercise 3 — Health endpoint design

Design a `/health` endpoint for a model API.

1. Return a JSON response with a status field.
2. Use an appropriate HTTP status code.
3. Explain why a health endpoint should not return environment variables, credentials, or stack traces.
4. Explain when a separate readiness check is useful.

**Deliverable:** code or pseudocode plus a short explanation.

## Exercise 4 — Container review

Review the following risky Dockerfile:

```dockerfile
FROM python:latest
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
ENV API_KEY="replace-me"
CMD ["python", "main.py"]
```

Identify at least five risks and propose a safer alternative for each.

**Deliverable:** a risk/remediation table.

## Exercise 5 — Deployment pipeline

Design a CI/CD pipeline for an ML API with `development`, `staging`, and `production` environments.

Include:
- code formatting/linting and unit tests;
- API/integration tests;
- dependency and image scanning;
- image tagging and registry publishing;
- staging deployment and smoke tests;
- production approval or policy gate;
- rollback to a previously known-good version.

**Deliverable:** a numbered workflow or diagram.

## Exercise 6 — Observability and alerting

Choose at least four metrics for a cloud-hosted prediction API. For each metric, specify:
- what it measures;
- why it matters;
- a possible alert condition;
- the first investigation step after an alert.

Avoid choosing arbitrary universal thresholds without considering the service's baseline and SLO.

**Deliverable:** an observability table.

## Exercise 7 — Cost estimation

Create a cost model for a service running in the cloud. Include at least:
- compute;
- storage;
- network egress;
- logging/monitoring;
- optional managed service or GPU cost.

Use a provider pricing calculator or clearly labeled hypothetical prices. Explain the assumptions and identify two ways to reduce cost without violating latency or reliability requirements.

**Deliverable:** a monthly cost estimate with assumptions. Do not create paid resources just to complete this task.

## Exercise 8 — Rollback scenario

After a new model release, the 5xx rate rises and p95 latency doubles.

1. List the first five actions you would take.
2. Describe the criteria for rollback.
3. Explain how to verify that rollback worked.
4. State what evidence you would preserve for a post-incident review.

**Deliverable:** a concise incident runbook.

## Exercise 9 — Cloud security review

Review this deployment plan:

- public API with no TLS;
- admin endpoint exposed to the internet;
- broad administrator permissions for the application identity;
- API key stored in the image;
- all request bodies logged;
- no dependency scanning.

Identify each issue, its impact, and a mitigation.

**Deliverable:** a security review table.

## Exercise 10 — Capstone

Create a deployment plan for a small classification model served through FastAPI.

Your plan must include:
- model artifact and preprocessing versioning;
- Docker image build;
- configuration and secret handling;
- cloud runtime choice and justification;
- networking and identity;
- health checks and observability;
- staged release and rollback;
- cost controls and cleanup.

**Deliverable:** a README or design note with a simple architecture diagram.

## Self-assessment

Score each item from 0 (not yet) to 2 (can explain and implement):

- [ ] I can compare common cloud deployment options.
- [ ] I can configure a service without hardcoding secrets.
- [ ] I can explain container image hygiene.
- [ ] I can define useful health checks and service metrics.
- [ ] I can design a basic CI/CD release and rollback workflow.
- [ ] I can identify security and cost risks.

**Interpretation:** 0–4: review the concepts; 5–8: repeat the practical tasks; 9–12: apply the workflow to a small project.
