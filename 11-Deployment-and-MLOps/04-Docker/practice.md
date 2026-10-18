# Practice: Docker for ML Deployment

Use a local Docker installation. If Docker is unavailable, complete the Dockerfile and command-writing exercises and run the Python application directly.

## Exercise 1 — Inspect the environment

Run:

```bash
docker --version
docker info
docker images
docker ps -a
```

Write down what each command tells you. If the daemon is unavailable, identify the service you need to start.

## Exercise 2 — Containerize a Python application

Create `app.py` that responds to `GET /health` with JSON:

```json
{"status": "ok"}
```

Then create a Dockerfile that:

- Uses a maintained slim Python base image.
- Sets `/app` as the working directory.
- Copies only required application files.
- Exposes port `8000`.
- Runs the app as a non-root user where practical.
- Starts the application with an exec-form `CMD`.

Build the image as `health-api:1.0` and run it with host port `8000` mapped to container port `8000`.

## Exercise 3 — Add a `.dockerignore`

Exclude at least:

- `.git/`
- `.venv/`
- `__pycache__/`
- `*.pyc`
- `.env`
- local model artifacts and notebook checkpoints

Explain why `.env` and local artifacts should not be copied into every image by default.

## Exercise 4 — Inspect and troubleshoot

1. View running containers.
2. View the application's logs.
3. Stop and remove the container.
4. Deliberately try mapping a different host port, such as `8080:8000`.
5. Explain the difference between the host port and the container port.

Useful commands include `docker ps`, `docker logs`, `docker stop`, and `docker rm`.

## Exercise 5 — Configure with environment variables

Update the application to read the listening port from an environment variable named `PORT`, defaulting to `8000`. Run the container with `-e PORT=8000`.

Explain why the application should bind to `0.0.0.0` inside the container instead of only `127.0.0.1`.

## Exercise 6 — ML deployment design

Suppose your API loads a scikit-learn model. Write a short design note describing:

- Where the model artifact comes from.
- How the artifact version is selected.
- How the app checks the expected input feature schema.
- How dependencies are pinned.
- How you prevent secrets from entering the image.
- How you test health and inference endpoints.

Do not load model artifacts from untrusted sources.

## Exercise 7 — Optional challenge: smaller, safer image

Improve the Dockerfile by using a non-root user, a slim base image, `.dockerignore`, and pinned application dependencies. Explain at least two trade-offs of using a smaller base image.

## Reflection questions

1. How is an image different from a container?
2. What does `-p 8080:8000` mean?
3. Why should containers avoid storing important state in their writable layer?
4. Why is running as root inside a container discouraged?
5. Why should a production ML image include the preprocessing and model dependencies that match the saved model?
