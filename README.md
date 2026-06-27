# Lab Manual: Running NGINX in Docker

A concise, image-driven lab. Pull the NGINX image, serve custom HTML, inspect a running container, and manage its lifecycle.

<p align="center">
  <img src="image/docker-pull-nginx.png" alt="Dataflow: Docker Hub to local image to running container" width="800">
</p>

---

## 1. Introduction

Deploy NGINX in Docker: pull an image, mount a custom HTML directory, map a host port, verify the service, and manage the container lifecycle.

**Why it matters:** Docker packages the app, its runtime, and its config into one immutable artifact. The same image runs identically on any host.

**Outcome:** NGINX container serving custom HTML at `http://localhost:8080`.

---

## 2. Prerequisites

| | |
|---|---|
| Docker Engine | 24.x or newer |
| curl | 7.x or newer |
| OS | Linux, macOS, or Windows with WSL2 |
| Editor | Any — VS Code, Puku CLI, or a plain terminal |

Create the project structure:

```bash
mkdir -p nginx-lab/html
```

---

## 3. Chapter 1 — Pull the Image

Images are immutable templates. Containers are running instances created from images.

```bash
docker pull nginx
docker images
```

<p align="center"><img src="image/02-docker-pull-complete.png" alt="docker pull nginx output" width="900"></p>

<p align="center"><img src="image/03-docker-images-output.png" alt="docker images output" width="900"></p>

The image is now local. `nginx:latest` appears in `docker images`.

---

## 4. Chapter 2 — Run with Volume Mount and Port Mapping

Mount a host directory into the container's web root, map host port 8080 to container port 80, run detached.

<p align="center">
  <img src="image/port-mapping.png" alt="Port mapping: host 8080 forwards to container 80" width="700">
  &nbsp;&nbsp;&nbsp;
  <img src="image/volume-mount.png" alt="Volume mount: host directory mapped into container web root" width="700">
</p>

Create the HTML page:

```bash
echo '<h1>Hello from NGINX running in Docker!</h1>' > nginx-lab/html/index.html
```

<p align="center"><img src="image/05-create-html-file.png" alt="Creating index.html" width="900"></p>

Run the container:

```bash
docker run --name my-nginx \
  -v $(pwd)/nginx-lab/html:/usr/share/nginx/html:ro \
  -p 8080:80 \
  -d nginx
```

<p align="center"><img src="image/06-docker-run-command.png" alt="docker run command" width="900"></p>

| Flag | Purpose |
|---|---|
| `--name my-nginx` | Stable name for the container. |
| `-v .../html:/usr/share/nginx/html:ro` | Mount host directory read-only into the web root. |
| `-p 8080:80` | Map host port 8080 to container port 80. |
| `-d` | Detached (background). |

Test:

```bash
curl http://localhost:8080
```

<p align="center"><img src="image/07-curl-test-result.png" alt="curl output" width="900"></p>

<p align="center"><img src="image/08-browser-test-result.png" alt="Browser view" width="900"></p>

**Without `-p 8080:80` the host cannot reach the container.** This is the most common mistake:

<p align="center"><img src="image/09-curl-connection-refused.png" alt="Connection refused without port mapping" width="900"></p>

---

## 5. Chapter 3 — Inspect the Container

Detached containers produce no terminal output. Use these commands to see what is happening.

<p align="center"><img src="image/status-vs-health.png" alt="Container status versus health" width="800"></p>

```bash
docker ps
```

<p align="center"><img src="image/10-docker-ps-output.png" alt="docker ps output" width="900"></p>

```bash
docker logs my-nginx
```

<p align="center"><img src="image/12-docker-logs-output.png" alt="docker logs output" width="900"></p>

Each new `curl` adds an access-log entry. Run `docker logs my-nginx` again after a few requests to see them accumulate.

---

## 6. Chapter 4 — Container Lifecycle

Containers are ephemeral. Stop, restart, and remove as versions change. The image is never affected.

<p align="center"><img src="image/container-lifecycle.png" alt="Container lifecycle" width="800"></p>

```bash
docker stop my-nginx
docker start my-nginx
docker rm my-nginx
```

<p align="center"><img src="image/17-docker-stop.png" alt="docker stop" width="900"></p>
<p align="center"><img src="image/18-docker-ps-a.png" alt="docker ps -a showing stopped container" width="900"></p>
<p align="center"><img src="image/19-docker-start.png" alt="docker start" width="900"></p>
<p align="center"><img src="image/20-curl-after-start.png" alt="curl after restart" width="900"></p>
<p align="center"><img src="image/21-docker-stop-rm.png" alt="docker stop and rm" width="900"></p>
<p align="center"><img src="image/22-docker-images-after-rm.png" alt="image persists after rm" width="900"></p>

After `docker rm`, the container record is gone. `docker start` no longer works:

<p align="center"><img src="image/23-docker-start-removed-error.png" alt="docker start error after rm" width="900"></p>

To run NGINX again, create a new container from the image with `docker run`.

---

## 7. Workflow Summary

```bash
docker pull nginx
mkdir -p nginx-lab/html
echo '<h1>Hello from NGINX running in Docker!</h1>' > nginx-lab/html/index.html

docker run --name my-nginx \
  -v $(pwd)/nginx-lab/html:/usr/share/nginx/html:ro \
  -p 8080:80 \
  -d nginx

curl http://localhost:8080
docker ps
docker logs my-nginx

docker stop my-nginx
docker start my-nginx
docker rm my-nginx
```

---

## 8. Key Principles

- One image produces many containers. Stop and remove freely; the image persists.
- Read-only mounts protect your source files from the web server.
- Explicit port mapping is required for the host to reach the container.
- Status (`Up`) reports the process. Health (`healthy`) reports the service. Both matter in production.
- Content lives on the host. Updates apply immediately, without rebuilding.

---

## 9. Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `port is already allocated` | Another process holds the host port. | Pick a different port or free it. |
| `Conflict. The container name "/my-nginx" is already in use` | A container with that name exists. | `docker rm my-nginx` |
| `curl: (7) Connection refused` | Container not running, or port mapping missing. | Check `docker ps` and `-p`. |
| Permission denied writing to mounted volume | Mount is `:ro`. | Remove `:ro` if writes are needed. |

---

## License

Released under the MIT License. See [`LICENSE`](LICENSE) for details.
