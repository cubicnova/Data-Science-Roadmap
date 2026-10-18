# Worked Solutions — Cloud Deployment

These are reference solutions, not the only valid answers. In real systems, decisions depend on traffic, model size, latency targets, compliance, team skills, and the cloud provider.

## Exercise 1 — Choose a deployment target

A managed container service is a reasonable starting point. It can run a containerized FastAPI service while reducing the need to maintain operating systems and cluster control planes. A VM may offer more control but requires more operational work. Kubernetes provides extensive orchestration but adds complexity that may not be justified for one small service. A serverless container platform may be attractive for intermittent traffic if cold starts, runtime limits, and scale-to-zero behavior meet the latency target.

The decision could change if the model needs a specialized GPU, if traffic is steady and high enough that another option is cheaper, or if the organization already operates a Kubernetes platform.

## Exercise 2 — Environment configuration

```python
import os
from dataclasses import dataclass

@dataclass(frozen=True)
class Settings:
    app_env: str
    model_path: str
    log_level: str

def load_settings() -> Settings:
    return Settings(
        app_env=os.getenv("APP_ENV", "development"),
        model_path=os.getenv("MODEL_PATH", "artifacts/model.joblib"),
        log_level=os.getenv("LOG_LEVEL", "INFO").upper(),
    )

settings = load_settings()
print(settings.app_env, settings.model_path, settings.log_level)
```

This example contains no secret. Production credentials should be injected through a managed secret store or workload identity mechanism, with narrowly scoped permissions. Do not print secrets or place them in source control, image layers, or logs. Validate configuration values at startup in a real application.

## Exercise 3 — Health endpoint design

Example with FastAPI:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/health")
def health():
    return {"status": "ok"}
```

A successful response returns HTTP 200. If the process cannot serve requests, the runtime should observe a failing status or failed probe. A readiness check can indicate whether the model is loaded and the service is ready to receive traffic; a liveness check should generally detect a stuck or unhealthy process. Do not disclose secrets or stack traces from health endpoints.

## Exercise 4 — Container review

| Risk | Safer approach |
|---|---|
| `python:latest` changes unexpectedly | Pin a reviewed base-image version or digest |
| `COPY . .` may include `.env`, datasets, `.git`, or virtual environments | Add `.dockerignore` and copy only required files |
| Dependencies are not pinned/reproducible | Use reviewed, pinned dependencies and a lock file where appropriate |
| `API_KEY` is embedded in the image | Inject secret at runtime from a secret manager |
| Application may run as root | Create and use a non-root user |
| No explicit runtime configuration | Set a working directory, unbuffered logs, and a clear command |
| No image/dependency scanning | Scan and patch the image and dependencies |
| No health check strategy | Add runtime-appropriate health/readiness probes |

One safer illustrative Dockerfile:

```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
RUN useradd --create-home appuser
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ ./app/
USER appuser
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Pin dependencies and review the base image before production use.

## Exercise 5 — Deployment pipeline

1. Trigger on a pull request or merge.
2. Run formatting, linting, type checks, and unit tests.
3. Run API integration tests against a test environment.
4. Scan dependencies and the built container image.
5. Build an immutable image tagged with the commit SHA; publish it to a registry after policy checks.
6. Deploy that image to staging with environment-specific configuration and secret references.
7. Run health, smoke, and representative inference tests.
8. Require an approval or policy gate for production.
9. Deploy gradually if supported, watching error rate, latency, and model-quality signals.
10. If release indicators breach defined limits, stop promotion and restore the last known-good image/model version.
11. Record the deployed image digest, model version, test results, and release outcome.

## Exercise 6 — Observability and alerting

| Metric | Why it matters | Example alert design | First investigation step |
|---|---|---|---|
| 5xx error rate | Detects service failures | Above the agreed SLO/error budget threshold for a sustained window | Compare errors with recent deployments and inspect sanitized logs |
| p95 latency | Captures slow requests | Sustained breach of the service latency objective | Check traffic, resource saturation, and model inference duration |
| CPU/memory utilization | Detects resource pressure | Near configured limits with sustained duration or restarts | Inspect container limits, concurrency, and workload changes |
| Request volume/concurrency | Helps explain load and scaling | Demand exceeds tested capacity or scaling fails to respond | Review autoscaling events and upstream traffic |
| Prediction distribution/drift | Can reveal input or model behavior changes | Material shift from a validated baseline | Check data pipeline, schema, and recent model/data changes |

Thresholds should be set using baselines, load tests, and service-level objectives. Avoid logging sensitive payloads unnecessarily.

## Exercise 7 — Cost estimation

A useful model is:

`Monthly cost = compute + storage + egress + logs/monitoring + managed services`

For example, use a spreadsheet with columns for resource, quantity, unit, unit price, monthly usage, and estimated monthly cost. Label any assumed prices as hypothetical; actual rates vary by provider, region, discounts, and usage. Use the chosen provider's current pricing calculator for a realistic estimate.

Potential savings include scaling replicas to demand, using a smaller CPU/memory profile after load testing, reducing noisy logs while preserving required audit events, and deleting unused development resources. Do not reduce capacity below what the latency and availability targets require.

## Exercise 8 — Rollback scenario

1. Confirm the alert and determine whether impact is ongoing.
2. Compare error/latency metrics with the release timeline.
3. Pause further rollout and notify the service owner/on-call team.
4. Check deployment health, resource saturation, logs, and model load/inference timings.
5. If release-related impact breaches the defined rollback criteria, restore the last known-good application image and compatible model artifact.

After rollback, verify health checks, representative predictions, error rate, and latency. Preserve release identifiers, timestamps, dashboards, sanitized logs, relevant traces, model/data versions, and actions taken. Avoid collecting unnecessary sensitive request data.

## Exercise 9 — Cloud security review

| Issue | Impact | Mitigation |
|---|---|---|
| No TLS | Traffic may be exposed or altered in transit | Use HTTPS with managed certificates and secure transport settings |
| Public admin endpoint | Unauthorized access and service compromise | Keep it private or strongly restrict access; require authentication and authorization |
| Broad administrator permissions | Excessive blast radius | Apply least privilege to the service identity |
| API key in image | Credential leakage through registry or image layers | Rotate the key and load secrets at runtime from a secret manager |
| Full request bodies logged | Sensitive data exposure and retention risk | Log only necessary metadata; redact sensitive fields and define retention |
| No dependency scanning | Known vulnerable packages may reach production | Add dependency/image scanning and a remediation policy |

Also review network ingress/egress, audit logs, patching, image provenance, and incident response procedures.

## Exercise 10 — Capstone reference architecture

```text
Developer commit
      |
      v
CI: lint -> tests -> dependency/image scan
      |
      v
Build immutable container image
      |
      v
Container registry
      |
      v
Staging deployment -> smoke tests
      |
  approval / policy gate
      |
      v
Production runtime <---- Secret manager / workload identity
      |
      +---- TLS / ingress / access controls
      |
      +---- Logs, metrics, alerts
      |
      +---- Versioned model artifact
```

The runtime might be a managed container platform for a small service, provided it meets the model's resource and latency requirements. Version preprocessing and model artifacts together. Keep credentials out of the repository and image. Use a private or restricted network where possible, enable TLS, and grant the service identity only the access it needs. Define health/readiness checks, error-rate and latency alerts, staged rollout, and a tested rollback to a known-good image and model version. Estimate cost with the provider calculator and remove temporary resources after experiments.

## General review checklist

- Is the model artifact reproducible and versioned?
- Are inputs validated and errors handled safely?
- Are secrets external to code and image layers?
- Is access restricted by identity and network policy?
- Are logs useful without leaking sensitive information?
- Can the team detect a failed release and roll back quickly?
- Are costs and resource cleanup part of the deployment process?
