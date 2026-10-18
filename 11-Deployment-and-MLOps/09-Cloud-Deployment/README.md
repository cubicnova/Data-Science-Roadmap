# 09 — Cloud Deployment

Learn how to package, deploy, configure, monitor, and maintain a machine-learning application in a cloud environment.

> **Scope:** This module teaches cloud deployment concepts and a provider-neutral workflow. Examples run locally and use a small FastAPI prediction service; cloud-provider commands are illustrative and should be adapted to your account, region, registry, and security requirements.

## Learning objectives

By the end of this module, you should be able to:

- Explain common cloud components used by ML applications.
- Distinguish virtual machines, containers, managed container platforms, and serverless services.
- Package a model-serving API with Docker.
- Configure applications through environment variables and secrets.
- Identify health checks, logging, monitoring, scaling, and rollback requirements.
- Plan a safe deployment to a cloud provider.
- Estimate the main cost drivers and avoid leaving unnecessary resources running.

## Suggested folder contents

- `README.md` — concepts, setup, workflow, and checklist.
- `examples.ipynb` — runnable examples for configuration, health checks, and deployment planning.
- `practice.md` — hands-on exercises.
- `solution.md` — worked solutions and discussion.

## Prerequisites

- Basic Python and machine-learning knowledge.
- Familiarity with REST APIs and JSON.
- Basic command-line and Git knowledge.
- Optional: Docker installed for the container exercise.
- Optional: a cloud account for provider-specific practice. **Do not create billable resources unless you understand the pricing and cleanup steps.**

## 1. What does cloud deployment mean?

Cloud deployment makes an application available on infrastructure managed by a cloud provider. A typical ML inference service contains:

1. **Model artifact** — trained weights, preprocessing pipeline, and metadata.
2. **Application/API** — validates requests and runs inference.
3. **Runtime** — Python environment or container image.
4. **Compute** — VM, container service, Kubernetes, or serverless runtime.
5. **Networking** — DNS, TLS, ingress/load balancer, and firewall rules.
6. **Configuration and secrets** — environment-specific settings and credentials.
7. **Observability** — logs, metrics, traces, alerts, and health checks.
8. **Delivery workflow** — tests, build, registry, deployment, verification, and rollback.

## 2. Common deployment options

| Option | Best fit | Trade-offs |
|---|---|---|
| Virtual machine (VM) | Maximum OS/runtime control; small services | You manage patching, process supervision, scaling, and much of security |
| Managed container service | Containerized APIs with less infrastructure management | Provider-specific configuration and pricing |
| Kubernetes | Many services, advanced scheduling, platform standardization | Higher operational complexity; often excessive for a first deployment |
| Serverless functions/containers | Event-driven or intermittent workloads | Runtime, request-duration, memory, cold-start, and platform limits |
| Managed ML platform | Managed model endpoints and ML lifecycle integrations | Platform coupling and potentially higher managed-service cost |
| Static hosting | Front-end assets and documentation | Not a general-purpose model inference runtime |

Choose based on latency, traffic pattern, model size, operational skills, security, compliance, portability, and cost—not popularity alone.

## 3. Provider mapping

Equivalent services have different names and capabilities. Verify current documentation before deploying.

| Capability | AWS examples | Azure examples | Google Cloud examples |
|---|---|---|---|
| Object storage | Amazon S3 | Azure Blob Storage | Cloud Storage |
| Container registry | Amazon ECR | Azure Container Registry | Artifact Registry |
| Managed containers | ECS / App Runner | Container Apps | Cloud Run |
| Kubernetes | EKS | AKS | GKE |
| Virtual machines | EC2 | Azure Virtual Machines | Compute Engine |
| Monitoring/logging | CloudWatch | Azure Monitor | Cloud Monitoring / Logging |
| Secrets | Secrets Manager / Parameter Store | Key Vault | Secret Manager |

These are examples, not a claim that the services are interchangeable in every detail.

## 4. A practical deployment workflow

1. **Prepare the artifact:** save the model and preprocessing steps together; record versions and expected input schema.
2. **Build the service:** create an API with request validation, useful error messages, and health endpoints.
3. **Test locally:** run unit tests, API tests, and a smoke test against representative inputs.
4. **Containerize:** use a small base image, pin dependencies, avoid root where practical, and exclude secrets from the image.
5. **Build and publish:** tag images immutably (for example, with a commit SHA) and scan dependencies/images.
6. **Configure infrastructure:** set environment variables, secret references, resource limits, networking, TLS, and identity permissions.
7. **Deploy to staging:** use a non-production environment with representative configuration.
8. **Verify:** check health, logs, latency, prediction behavior, and error rates.
9. **Release gradually:** use rolling, canary, or blue/green deployment when supported.
10. **Monitor and roll back:** define service-level indicators and a tested rollback path.
11. **Clean up:** remove temporary resources and review cost reports.

## 5. Example container layout

A simple service repository could look like:

```text
cloud-ml-api/
├── app/
│   ├── main.py
│   └── model.py
├── tests/
│   └── test_health.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
└── README.md
```

Example `requirements.txt`:

```text
fastapi
uvicorn[standard]
scikit-learn
joblib
```

Example `Dockerfile` (adapt versions and file paths to your app):

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

For production, pin and review dependency versions, use a lock file where appropriate, scan the image, configure a non-root user, and avoid embedding credentials. Add `.dockerignore` entries such as `.git`, `.venv`, `__pycache__`, `.env`, and local datasets.

## 6. Configuration, security, and reliability

- **Configuration:** separate code from environment-specific values. Read configuration from environment variables or a managed configuration service.
- **Secrets:** use a cloud secret manager or workload identity; never commit API keys or bake them into an image.
- **Identity:** grant least privilege to the service identity; avoid long-lived access keys when workload identity is available.
- **Network:** expose only required ports; use TLS; restrict admin endpoints and internal services.
- **Data:** minimize sensitive data in logs and protect data at rest and in transit.
- **Reliability:** define startup/readiness/liveness checks appropriate to the runtime; use timeouts and sensible concurrency.
- **Model integrity:** version the model artifact, verify its provenance, and test it before promotion.
- **Supply chain:** scan dependencies and images; keep a record of image digest and source commit.

## 7. Health checks and observability

Useful signals include:

- **Availability:** successful health checks and request success rate.
- **Latency:** p50, p95, and p99 response time.
- **Traffic:** requests per second and concurrency.
- **Errors:** HTTP 4xx/5xx rates, inference failures, and timeouts.
- **Resource usage:** CPU, memory, GPU (if applicable), and restarts.
- **ML quality:** prediction distributions, drift indicators, and delayed ground-truth metrics where available.

A health endpoint should confirm that the process can serve requests. It should not expose secrets, internal stack traces, or detailed infrastructure information.

## 8. Scaling and cost basics

- **Vertical scaling:** increase resources assigned to one instance.
- **Horizontal scaling:** add instances or replicas.
- **Autoscaling:** adjust capacity based on demand or metrics.
- **Scale to zero:** reduce idle cost when the platform and latency requirements permit it.
- **Cost drivers:** compute duration/size, GPU use, storage, network egress, logs, managed-service fees, and minimum instance counts.

Estimate cost using the provider's current pricing calculator and realistic traffic assumptions. Set budgets/alerts where available, and delete test resources after the exercise.

## 9. Local practice workflow

1. Open `examples.ipynb` in Jupyter Notebook or JupyterLab.
2. Run the cells from top to bottom.
3. Complete the tasks in `practice.md`.
4. Compare your work with `solution.md`.
5. If Docker is installed, build and run the example service using the commands below after creating the sample files described in the notebook or practice tasks.

Useful commands:

```bash
docker build -t cloud-ml-api:dev .
docker run --rm -p 8000:8000 --env APP_ENV=local cloud-ml-api:dev
```

Then test a health endpoint such as `http://localhost:8000/health`. These commands are examples; they will work only after you have created a compatible Dockerfile and application.

## 10. Deployment readiness checklist

- [ ] Model and preprocessing artifacts are versioned.
- [ ] API input validation and error handling are tested.
- [ ] Unit, integration, and smoke tests pass.
- [ ] Dependencies and container image are reviewed/scanned.
- [ ] Secrets are stored outside source code and images.
- [ ] Service identity follows least privilege.
- [ ] TLS, network rules, and access controls are configured.
- [ ] Health checks, logs, metrics, and alerts are enabled.
- [ ] Resource limits, autoscaling, and cost estimates are reviewed.
- [ ] Staging verification and rollback procedures are documented.
- [ ] Temporary resources are cleaned up after testing.

## Key takeaways

Cloud deployment is not just uploading code. A dependable ML service needs a reproducible artifact, secure configuration, reliable infrastructure, observable behavior, controlled releases, and a tested recovery plan.
