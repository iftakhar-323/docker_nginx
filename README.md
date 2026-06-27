# Lab Manual: Running NGINX in Docker

A concise, image-driven lab. Pull the NGINX image, serve custom HTML, inspect a running container, and manage its lifecycle.

---

## 1. Introduction

Deploy NGINX in Docker: pull an image, mount a custom HTML directory, map a host port, verify the service, and manage the container lifecycle.

**Why it matters:** Docker packages the app, its runtime, and its config into one immutable artifact. The same image runs identically on any host.

**Outcome:** NGINX container serving custom HTML at `http://localhost:8080`.

### The Big Picture

Before touching any command, understand the four moving parts: a Docker Hub registry, a local image, a running container, and your host machine. The next four chapters touch each of them in order.

<p align="center">
  <img src="image/docker-pull-nginx.png" alt="Dataflow: Docker Hub pulls into a local image, image runs as a container, container port 80 is exposed to host port 8080, host directory is mounted into the container's web root">
</p>

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

An **image** is a read-only template. A **container** is a running instance created from an image. Only containers run.

Pull the official image from Docker Hub:

```bash
docker pull nginx
```

<p align="center"><img src="image/02-docker-pull-complete.png" alt="Docker pulls layer by layer, then prints Digest and Status. The last line confirms docker.io/library/nginx:latest is downloaded."></p>

Confirm the image is local:

```bash
docker images
```

<p align="center"><img src="image/03-docker-images-output.png" alt="The REPOSITORY column shows nginx, TAG shows latest, IMAGE ID shows a 12-char hash, SIZE shows about 241MB. This proves the image exists on your machine and is ready to run."></p>

---

## 4. Chapter 2 — Run with Volume Mount and Port Mapping

The default NGINX image serves only its built-in welcome page. To serve your own HTML and make it reachable from the host, you need two contracts: a **port mapping** and a **volume mount**.

<p align="center">
  <img src="image/port-mapping.png" alt="Port mapping: the host exposes 8080, the container listens on 80. Docker bridges them so curl localhost:8080 reaches NGINX inside the container.">
</p>

<p align="center">
  <img src="image/volume-mount.png" alt="Volume mount: the host folder ./nginx-lab/html is mounted at /usr/share/nginx/html inside the container. NGINX reads your files from there. The :ro flag makes the mount read-only.">
</p>

Create one HTML file:

```bash
echo '<h1>Hello from NGINX running in Docker!</h1>' > nginx-lab/html/index.html
```

<p align="center"><img src="image/05-create-html-file.png" alt="Terminal shows echo creating index.html, then cat printing its single h1 line. This file is the content the container will serve."></p>

Run the container:

```bash
docker run --name my-nginx \
  -v $(pwd)/nginx-lab/html:/usr/share/nginx/html:ro \
  -p 8080:80 \
  -d nginx
```

<p align="center"><img src="image/06-docker-run-command.png" alt="Docker prints a long 64-char container ID and returns to the prompt. That ID means the container is now running detached in the background."></p>

| Flag | What it does |
|---|---|
| `--name my-nginx` | Gives the container a stable name. Use it for stop, start, logs, rm. |
| `-v .../html:/usr/share/nginx/html:ro` | Mounts host folder into the container's web root, read-only. |
| `-p 8080:80` | Host port 8080 forwards to container port 80. Without this, the host cannot reach NGINX. |
| `-d` | Detached. Container runs in the background; the terminal stays free. |

Test from the terminal:

```bash
curl http://localhost:8080
```

<p align="center"><img src="image/07-curl-test-result.png" alt="curl returns the literal h1 line you wrote. This proves the host reached the container, NGINX read the mounted file, and served it back."></p>

Same result in the browser:

<p align="center"><img src="image/08-browser-test-result.png" alt="Browser at localhost:8080 displays the heading on a white page. Same content as curl, just rendered."></p>

### Common mistake: forgetting `-p`

Without port mapping, the host cannot reach the container. Watch what happens:

<p align="center"><img src="image/09-curl-connection-refused.png" alt="curl prints Connection refused on port 8080. The container is still running, but there is no bridge from the host. This is the symptom of a missing -p flag."></p>

---

## 5. Chapter 3 — Inspect the Container

A detached container prints nothing to your terminal. To see what it is doing, use `docker ps` for state and `docker logs` for activity.

**Status** is whether the process is alive. **Health** is whether the service is responding correctly. They are independent: a container can be `Up` but `unhealthy`.

<p align="center"><img src="image/status-vs-health.png" alt="Two container cards: one shows Status Up with a green check, the other shows Status Up but Health Unhealthy with a red X. Caption: Status and Health are independent."></p>

List running containers:

```bash
docker ps
```

<p align="center"><img src="image/10-docker-ps-output.png" alt="Table with NAMES my-nginx, STATUS Up, PORTS 0.0.0.0:8080->80/tcp. This confirms the process is alive, the name is correct, and the port mapping is active."></p>

Read what NGINX is doing:

```bash
docker logs my-nginx
```

<p align="center"><img src="image/12-docker-logs-output.png" alt="Log lines: Configuration complete; ready for start up, then worker process notices, then access log entries showing GET requests from 172.17.0.1 with status 200. This is NGINX's stdout."></p>

Run `docker logs my-nginx` again after a few `curl` requests — new access entries appear at the bottom, one per request.

---

## 6. Chapter 4 — Container Lifecycle

Containers are temporary. Stop them, restart them, remove them as your app changes. The image is untouched by all of this.

<p align="center"><img src="image/container-lifecycle.png" alt="State machine: Created to Running via docker run, Running to Stopped via docker stop, Stopped back to Running via docker start, Stopped or Running to Removed via docker rm. The image layer at the bottom persists throughout."></p>

Stop the container:

```bash
docker stop my-nginx
```

<p align="center"><img src="image/17-docker-stop.png" alt="Docker prints my-nginx. The process is sent SIGTERM, then SIGKILL after a grace period. It is no longer running, but the record still exists."></p>

Restart it:

```bash
docker start my-nginx
```

<p align="center"><img src="image/19-docker-start.png" alt="Docker prints my-nginx again. The same container ID is reused. Anything the container had on disk is preserved across stop and start."></p>

Remove it:

```bash
docker stop my-nginx
docker rm my-nginx
```

<p align="center"><img src="image/21-docker-stop-rm.png" alt="Two lines, my-nginx each. rm deletes the container record. The image nginx:latest in docker images is unchanged."></p>

<p align="center"><img src="image/22-docker-images-after-rm.png" alt="docker images still lists nginx latest. This proves rm operates on the container, never on the image. You can run my-nginx again from this same image any time."></p>

### What happens if you try to start a removed container

```bash
docker start my-nginx
```

<p align="center"><img src="image/23-docker-start-removed-error.png" alt="Docker prints Error response from daemon: No such container: my-nginx. The record is gone, so start cannot find it. You must use docker run to create a new container from the image."></p>

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

- **One image, many containers.** Stop and remove freely. The image persists.
- **Read-only mounts** protect your source files from the web server.
- **Explicit port mapping** is required for the host to reach the container.
- **Status** reports the process. **Health** reports the service. Both matter.
- **Content lives on the host.** Edits apply immediately, no rebuild needed.

---

## 9. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `port is already allocated` | Another process holds the host port. | Pick a different port or free it: `lsof -ti:8080 \| xargs kill -9` |
| `Conflict. The container name "/my-nginx" is already in use` | A container with that name exists, possibly stopped. | `docker rm my-nginx` |
| `curl: (7) Connection refused` | Container not running, or `-p` missing. | Check `docker ps` and your `-p 8080:80`. |
| Permission denied writing to mounted volume | Mount has `:ro`. | Drop `:ro` if the app needs to write. |

---

## License

Released under the MIT License. See [`LICENSE`](LICENSE) for details.
