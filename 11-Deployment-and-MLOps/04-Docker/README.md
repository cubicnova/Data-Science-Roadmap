# Docker for Machine Learning Deployment

Learn how to package a Python machine-learning application into a Docker image and run it consistently across development and deployment environments.

## Learning objectives

By the end of this module, you should be able to:

- Explain images, containers, Dockerfiles, registries, ports, and volumes.
- Write a Dockerfile for a small Python ML inference service.
- Build and run an image locally.
- Expose an HTTP endpoint and test it.
- Use `.dockerignore`, environment variables, and reproducible dependency pins.
- Troubleshoot common container build and runtime issues.

## What is Docker?

Docker packages an application and its runtime dependencies into an image. A running instance of an image is a container. Containers help reduce differences between local development and deployment environments.

### Core terms

| Term | Meaning |
|---|---|
| Image | Immutable template used to create containers |
| Container | A running or stopped instance of an image |
| Dockerfile | Instructions used to build an image |
| Registry | Service that stores and distributes images |
| Port mapping | Connects a host port to a container port |
| Volume | Persists or shares data outside the container's writable layer |
| `.dockerignore` | Excludes files from the build context |

## Prerequisites

- Basic Python and command-line knowledge.
- Docker Desktop on Windows/macOS, or Docker Engine on Linux.
- A terminal (PowerShell, Command Prompt, Bash, or similar).

Verify your installation:

```bash
docker --version
docker info
```

If `docker info` cannot connect to the daemon, start Docker Desktop or the Docker service before continuing.

## Example application

The notebook builds a tiny HTTP prediction service using Python's standard library. It returns a demonstration score for two numeric inputs; it is intentionally not a trained production model. The example keeps the container build small and avoids requiring a model download.

## Build and run

Create the following files in a directory named `docker-ml-demo`.

### `app.py`

```python
from http.server import BaseHTTPRequestHandler, HTTPServer
import json
import os

HOST = "0.0.0.0"
PORT = int(os.environ.get("PORT", "8000"))

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/health":
            body = {"status": "ok"}
            self.send_response(200)
        elif self.path == "/predict?x1=1&x2=2":
            # Demonstration only: replace with a real, validated model.
            body = {"prediction": 0.5}
            self.send_response(200)
        else:
            body = {"message": "Try GET /health or /predict?x1=1&x2=2"}
            self.send_response(200)

        payload = json.dumps(body).encode("utf-8")
        self.send_header("Content-Type", "application/json")
        self.send_header("Content-Length", str(len(payload)))
        self.end_headers()
        self.wfile.write(payload)

HTTPServer((HOST, PORT), Handler).serve_forever()
```

### `Dockerfile`

```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
COPY app.py .
EXPOSE 8000
USER 10001
CMD ["python", "app.py"]
```

### `.dockerignore`

```text
.git
.venv
__pycache__/
*.py[cod]
.ipynb_checkpoints/
artifacts/
.env
```

Build and run:

```bash
docker build -t ml-api-demo:1.0 .
docker run --rm -p 8000:8000 ml-api-demo:1.0
```

In another terminal, test the health endpoint:

```bash
curl http://localhost:8000/health
```

Expected response:

```json
{"status": "ok"}
```

Stop the container with `Ctrl+C`. For a detached container, use `docker run -d --name ml-api-demo -p 8000:8000 ml-api-demo:1.0`, then stop it with `docker stop ml-api-demo`.

## Useful commands

```bash
docker images
docker ps
docker ps -a
docker logs <container-name-or-id>
docker inspect <container-name-or-id>
docker stop <container-name-or-id>
docker rm <container-name-or-id>
docker rmi <image-name:tag>
```

## Security and operational practices

- Use a small, maintained base image and update it regularly.
- Run as a non-root user where possible.
- Do not bake secrets into images or commit `.env` files.
- Use `.dockerignore` to avoid sending unnecessary files into the build context.
- Pin and review dependencies for real applications.
- Keep containers stateless; use volumes or external storage for persistent data.
- Add health checks, structured logs, resource limits, and monitoring for production services.
- Scan images and follow your organization's image-signing and registry policies.
- A container is not a complete security boundary; apply least privilege and keep the host secure.

## Repository structure

```text
04-Docker/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Practice

Complete the exercises in [`practice.md`](practice.md), then compare your work with [`solution.md`](solution.md).

## References

- Docker Get Started: https://docs.docker.com/get-started/
- Dockerfile reference: https://docs.docker.com/reference/dockerfile/
- Docker security: https://docs.docker.com/engine/security/
