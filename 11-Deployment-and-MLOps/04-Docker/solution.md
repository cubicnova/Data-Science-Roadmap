# Solutions: Docker for ML Deployment

These are reference solutions. Commands assume Docker is installed and running. Image tags and base-image versions should be reviewed and updated according to your environment's security policy.

## Exercise 1 — Inspect the environment

```bash
docker --version
docker info
docker images
docker ps -a
```

- `docker --version`: prints the Docker CLI version.
- `docker info`: reports client/server and daemon information; a connection error usually means the daemon is not running or is not reachable.
- `docker images`: lists local images.
- `docker ps -a`: lists running and stopped containers.

## Exercise 2 — Containerize a Python application

Example `app.py`:

```python
from http.server import BaseHTTPRequestHandler, HTTPServer
import json
import os

PORT = int(os.environ.get("PORT", "8000"))

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/health":
            status, body = 200, {"status": "ok"}
        else:
            status, body = 404, {"error": "not found"}

        payload = json.dumps(body).encode("utf-8")
        self.send_response(status)
        self.send_header("Content-Type", "application/json")
        self.send_header("Content-Length", str(len(payload)))
        self.end_headers()
        self.wfile.write(payload)

HTTPServer(("0.0.0.0", PORT), Handler).serve_forever()
```

Example `Dockerfile`:

```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
COPY app.py .
EXPOSE 8000
USER 10001
CMD ["python", "app.py"]
```

Build and run:

```bash
docker build -t health-api:1.0 .
docker run --rm --name health-api -p 8000:8000 health-api:1.0
```

Test from another terminal:

```bash
curl http://localhost:8000/health
```

Expected JSON:

```json
{"status": "ok"}
```

## Exercise 3 — `.dockerignore`

```text
.git/
.venv/
__pycache__/
*.pyc
.env
artifacts/
.ipynb_checkpoints/
```

Secrets should not be embedded in images, because image layers may persist and be shared. Model artifacts can be large or sensitive; include them only through an intentional, versioned deployment design.

## Exercise 4 — Inspect and troubleshoot

```bash
docker ps
docker logs health-api
docker stop health-api
docker rm health-api
docker run --rm --name health-api -p 8080:8000 health-api:1.0
```

With `-p 8080:8000`, requests go to port `8080` on the host and are forwarded to port `8000` in the container. The app must listen on the container port.

## Exercise 5 — Environment variables

The sample app uses:

```python
PORT = int(os.environ.get("PORT", "8000"))
HTTPServer(("0.0.0.0", PORT), Handler).serve_forever()
```

Run it with:

```bash
docker run --rm -e PORT=8000 -p 8000:8000 health-api:1.0
```

`0.0.0.0` binds to all container network interfaces. Binding only to `127.0.0.1` makes the service reachable only from the container's own loopback interface in typical setups.

## Exercise 6 — Example ML deployment design

- Store the model in a trusted artifact registry or versioned deployment package.
- Configure an explicit model version or immutable artifact identifier.
- Validate required feature names, order, types, and missing-value handling at the API boundary.
- Pin tested dependencies and record Python/scikit-learn versions.
- Use runtime secret injection or a secrets manager; never bake credentials into the image.
- Provide `/health` and an inference endpoint, then test both in CI and after deployment.
- Deserialize only trusted model artifacts; pickle/joblib are not safe for untrusted input.

## Exercise 7 — Smaller, safer image

A suitable baseline is:

```dockerfile
FROM python:3.12-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app
COPY requirements.txt .
RUN python -m pip install --no-cache-dir -r requirements.txt
COPY app.py .
USER 10001
EXPOSE 8000
CMD ["python", "app.py"]
```

Use a `.dockerignore` file to exclude `.git`, `.venv`, `.env`, caches, tests not needed at runtime, and local artifacts. Pin dependencies in `requirements.txt` after testing them.

Trade-offs: slim images reduce size and attack surface but may omit system libraries needed to build or run some packages. Minimal images can also be harder to debug because they include fewer diagnostic tools.

## Reflection questions — sample answers

1. An image is a template; a container is an instance created from that image.
2. `-p 8080:8000` maps host port 8080 to container port 8000.
3. The writable layer is tied to the container lifecycle and is not a reliable persistence strategy. Use volumes or external storage for persistent data.
4. Least privilege limits the impact if the application is compromised.
5. A model depends on compatible libraries and preprocessing behavior; mismatches can cause errors or different predictions.
