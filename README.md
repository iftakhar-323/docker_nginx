# Running an NGINX Web Server in a Docker Container

> **Professional Lab Manual – Docker & NGINX using Docker CLI**

---

# 1. Introduction

## What is this Lab?

This lab introduces the fundamental concepts of Docker by deploying an **NGINX Web Server** inside a Docker container. Instead of installing software directly on the operating system, Docker packages applications and their dependencies into lightweight containers that can run consistently across different environments.

Throughout this lab, you will learn how to download Docker images, create containers, mount local files, configure networking, inspect running containers, and manage the complete lifecycle of a Docker container.

<p align="center">
<img src="images/how_it_work.png" width="700">
</p>

---

## Why is this Important?

Docker has become one of the most important technologies in modern software development because it provides:

- Consistent execution environments
- Fast application deployment
- Lightweight virtualization
- Easy scalability
- Simplified dependency management
- Efficient resource utilization

Docker is widely used in:

- DevOps
- Cloud Computing
- CI/CD Pipelines
- Backend Development
- Software Engineering
- Microservice Architecture

---

## What You Will Build

By completing this lab, you will build a complete Dockerized NGINX web server capable of:

- Pulling Docker images
- Running Docker containers
- Serving custom HTML pages
- Mounting local directories
- Configuring port mapping
- Viewing container logs
- Managing the complete container lifecycle

---

# 2. Learning Objectives

After successfully completing this lab, you will be able to:

- Create Docker containers.
- Configure Docker networking.
- Build an NGINX web server.
- Deploy a containerized application.
- Analyze Docker container status.
- Test container deployment.
- Debug common Docker issues.
- Integrate local files with Docker containers.

---

# 3. Prerequisites

Before starting this lab, ensure that the following software is installed.

| Software | Purpose |
|-----------|----------|
| Docker Engine | Container Runtime |
| Docker CLI | Container Management |
| Puku CLI | Development Environment |
| Git | Version Control |
| VS Code (Optional) | Markdown Editing |

### Required Knowledge

- Basic Linux terminal commands
- Basic HTML
- Basic understanding of web servers

---

# 4. Prologue (Real-World Scenario)

A software company wants to deploy an internal documentation website that every developer can access locally.

Installing NGINX separately on each developer's machine results in inconsistent environments, dependency conflicts, and difficult maintenance.

As a DevOps Engineer, your responsibility is to deploy the web server inside a Docker container so that every developer can start the same environment using Docker.

Your expected outcome is to create a reusable Docker environment capable of serving a custom web page.

---

# 5. Environment Setup

## System Requirements

- Linux / Windows / macOS
- Docker Engine Installed
- Docker CLI
- Internet Connection
- Web Browser

---

## Step 1 — Verify Docker Installation

Run:

```bash
docker --version
```

Expected Output

```text
Docker version 24.x.x
```

<p align="center">
<img src="images/version_dockerinfo.png" width="700">
</p>

---

## Step 2 — Verify Docker Engine

Run:

```bash
docker info
```

This command verifies that Docker Engine is running correctly.

---

## Step 3 — Create the Project Directory

```bash
mkdir -p nginx-lab/html

cd nginx-lab
```

---

## Step 4 — Project Structure

Create the following directory structure.

```text
nginx-lab/
│
├── html/
│   └── index.html
│
├── images/
│   ├── how_it_work.png
│   ├── version_dockerinfo.png
│   ├── docker_pull.png
│   ├── docker_image.png
│   ├── docker-run.png
│   ├── docker-ps.png
│   ├── docker-logs.png
│   ├── port-mapping.png
│   └── container-lifecycle.png
│
└── README.md
```

---

## Step 5 — Create the HTML File

Create an `index.html` file inside the `html` directory.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Docker NGINX Lab</title>
</head>
<body>

<h1>Hello from Docker!</h1>

<p>This page is served from an NGINX Docker Container.</p>

</body>
</html>
```

---

# Think First

Before executing any Docker command, answer the following questions.

### Question 1

What is the primary difference between a Docker Image and a Docker Container?

---

### Question 2

Why do organizations prefer Docker over installing software directly on operating systems?

---

# Fill in the Blanks

Complete the following command.

```bash
docker ______ nginx
```

---

Complete the following command.

```bash
mkdir -p ______/html
```

---

# Solution

<details>

<summary>Show Solution</summary>

```bash
docker pull nginx
```

```bash
mkdir -p nginx-lab/html
```

</details>

---

# Understanding

In this section you prepared the Docker environment.

You verified the Docker installation, confirmed that Docker Engine was running, created the project directory, and prepared the HTML workspace that will later be mounted inside the Docker container.

These steps ensure that the development environment is ready before creating the first Docker container.

---

# Test & Verify

Before moving to the implementation chapters, predict the answers to the following questions.

1. Will Docker work if Docker Engine is not running?

2. Can a Docker container exist without a Docker image?

3. Why is an HTML folder created before running the container?

Discuss your answers before continuing.

---

# Checkpoint

Verify the following tasks.

- [ ] Docker is installed.
- [ ] Docker Engine is running.
- [ ] Project directory has been created.
- [ ] HTML file has been created.
- [ ] Folder structure matches the lab manual.

---

# Experiment

Stop the Docker Engine (if your environment allows it) and run:

```bash
docker info
```

Observe the error message.

**Question**

Why does Docker fail to execute the command when Docker Engine is stopped?

Write your observation before continuing to the next chapter.

---

## Part 1 Summary

You have successfully prepared the Docker development environment.

In the next section, you will download the official NGINX image, verify the downloaded image, and create your first Docker container.

---

# Chapter 1 — Pulling the NGINX Image

## Overview

Docker containers are created from Docker images. Before running a container, the required image must first be downloaded from Docker Hub. In this chapter, you will download the official NGINX image and verify that it is available on your local machine.

---

## What You Will Build

By the end of this chapter, you will:

- Download the official NGINX image.
- Store the image locally.
- Verify the downloaded image.
- Understand the relationship between Docker Images and Containers.

---

## Think First

Answer the following questions before executing any command.

### Question 1

Can a Docker container be created without first downloading an image?

### Question 2

Where are Docker images stored after downloading?

Write your answers before continuing.

---

## Implementation

Download the official NGINX image.

```bash
docker pull nginx
```

<p align="center">
<img src="images/docker_pull.png" width="700">
</p>

---

### Verify the Download

Run:

```bash
docker images
```

<p align="center">
<img src="images/docker_image.png" width="700">
</p>

Expected Output

```text
REPOSITORY   TAG      IMAGE ID      CREATED      SIZE
nginx        latest   xxxxxxxxx     xx days ago  xxxMB
```

---

## Fill in the Blanks

Complete the command below.

```bash
docker ______ nginx
```

Complete the command below.

```bash
docker ______
```

---

## Solution

<details>

<summary>Show Solution</summary>

```bash
docker pull nginx
```

```bash
docker images
```

</details>

---

## Understanding

The `docker pull` command downloads an image from Docker Hub and stores it locally.

The `docker images` command displays all locally available Docker images. These images can later be used to create one or more Docker containers.

One Docker image can create multiple independent containers.

---

## Test & Verify

Before executing the following command, predict its output.

```bash
docker images
```

Questions:

- Will the NGINX image appear?
- Will Docker display the image tag?
- Will Docker display the image size?

Now execute the command and compare your prediction with the actual output.

---

## Checkpoint

Verify the following.

- [ ] NGINX image downloaded successfully.
- [ ] Image appears in Docker Images.
- [ ] No download errors occurred.

---

## Experiment

Try downloading another image.

```bash
docker pull alpine
```

Question:

How is the Alpine image size different from the NGINX image?

Record your observation.

---

# Chapter 2 — Running an NGINX Container

## Overview

After downloading the Docker image, the next step is to create a running container.

A Docker container is an isolated runtime environment created from an image. In this chapter, you will launch an NGINX container, mount a local HTML directory, and expose the web server through port mapping.

---

## What You Will Build

You will build a running NGINX web server that:

- Uses the official Docker image.
- Mounts a local HTML folder.
- Uses port mapping.
- Serves a custom web page.

---

## Think First

Before running the container, answer the following.

### Question 1

Why do we mount a local folder instead of copying HTML files into the container?

### Question 2

What is the purpose of port mapping?

---

## Implementation

Run the following command.

```bash
docker run \
--name my-nginx \
-v $(pwd)/html:/usr/share/nginx/html:ro \
-p 8080:80 \
-d nginx
```

<p align="center">
<img src="images/docker-run.png" width="700">
</p>

---

## Command Breakdown

| Option | Description |
|---------|-------------|
| `--name` | Assigns a name to the container |
| `-v` | Mounts a local directory |
| `:ro` | Read-only permission |
| `-p` | Maps host port to container port |
| `-d` | Runs the container in detached mode |

---

## Port Mapping

The NGINX server listens on **port 80** inside the container.

The host machine accesses the web server through **port 8080**.

<p align="center">
<img src="images/port-mapping.png" width="700">
</p>

```
Browser
      │
localhost:8080
      │
Docker Host
      │
Container :80
```

---

## Fill in the Blanks

Complete the following command.

```bash
docker run --name ______ \
-p ______:80 \
-d nginx
```

---

## Solution

<details>

<summary>Show Solution</summary>

```bash
docker run --name my-nginx \
-p 8080:80 \
-d nginx
```

</details>

---

## Understanding

When the container starts, Docker creates an isolated environment using the downloaded image.

The mounted volume connects the local HTML directory with the container's web root, allowing any modification made to the HTML files to be reflected immediately without rebuilding the container.

Port mapping connects port **8080** on the host machine to port **80** inside the container, allowing the web server to be accessed from a browser.

---

## Test & Verify

Open your browser.

```
http://localhost:8080
```

Questions:

- Is the web page displayed?
- Is the HTML page loaded from your local directory?
- What happens if you edit `index.html` and refresh the browser?

---

## Checkpoint

- [ ] Container created successfully.
- [ ] Browser displays the HTML page.
- [ ] Port mapping works correctly.
- [ ] Local HTML files are mounted correctly.

---

## Experiment

Stop the running container.

```bash
docker stop my-nginx
```

Refresh the browser.

Question:

What happens after stopping the container?

Explain why the webpage becomes inaccessible.

---

## Chapter Summary

In this chapter, you learned how to:

- Download Docker images.
- Verify Docker images.
- Create Docker containers.
- Configure port mapping.
- Mount local directories.
- Deploy an NGINX web server.
- Access the web server through a browser.

The next chapter focuses on inspecting running containers, viewing logs, and managing the complete container lifecycle.

---

# Chapter 3 — Inspecting the Running Container

## Overview

After creating a Docker container, it is important to monitor its status and inspect its activities. Docker provides several commands that help administrators verify whether a container is running correctly and diagnose potential issues.

---

## What You Will Build

In this chapter, you will learn how to:

- View running containers.
- Inspect container information.
- Monitor container logs.
- Verify web server activity.

---

## Think First

Before executing the commands, answer the following questions.

### Question 1

How can you verify whether a Docker container is currently running?

### Question 2

Why are container logs useful for troubleshooting?

Write your answers before continuing.

---

## Implementation

### Step 1 — Display Running Containers

Run:

```bash
docker ps
```

<p align="center">
<img src="images/docker-ps.png" width="700">
</p>

---

Expected Output

```text
CONTAINER ID   IMAGE   STATUS   PORTS   NAMES
```

---

### Step 2 — Display Container Logs

Run:

```bash
docker logs my-nginx
```

<p align="center">
<img src="images/docker-logs.png" width="700">
</p>

---

## Fill in the Blanks

Complete the following command.

```bash
docker ______
```

Complete the following command.

```bash
docker ______ my-nginx
```

---

## Solution

<details>

<summary>Show Solution</summary>

```bash
docker ps
```

```bash
docker logs my-nginx
```

</details>

---

## Understanding

The `docker ps` command displays all currently running containers together with their status, ports, and names.

The `docker logs` command displays the output generated by the application running inside the container. These logs are extremely useful for monitoring server activity and diagnosing problems.

---

## Test & Verify

Refresh your browser several times while the container is running.

Execute:

```bash
docker logs my-nginx
```

Questions:

- Can you observe new HTTP requests?
- Do the timestamps change?
- Does every page refresh generate a log entry?

Compare your observations with your predictions.

---

## Checkpoint

- [ ] Running container is displayed.
- [ ] Container logs are visible.
- [ ] Browser requests appear in the logs.
- [ ] No runtime errors are displayed.

---

## Experiment

Modify the HTML page while the container is still running.

Refresh the browser.

Question:

Did the changes appear immediately?

Explain why Docker can display the updated page without creating another container.

---

# Chapter 4 — Managing the Container Lifecycle

## Overview

Containers are temporary runtime environments. Docker provides lifecycle commands that allow developers to pause, resume, stop, restart, and remove containers whenever necessary.

---

## What You Will Build

You will practice the complete lifecycle management of a Docker container.

---

## Think First

Before executing the following commands, answer these questions.

### Question 1

Can a stopped container be started again?

### Question 2

What happens after a container is removed?

---

## Implementation

### Stop the Container

```bash
docker stop my-nginx
```

---

### Start the Container

```bash
docker start my-nginx
```

---

### Pause the Container

```bash
docker pause my-nginx
```

---

### Resume the Container

```bash
docker unpause my-nginx
```

---

### Remove the Container

```bash
docker stop my-nginx

docker rm my-nginx
```

<p align="center">
<img src="images/container-lifecycle.png" width="700">
</p>

---

## Fill in the Blanks

Complete the commands below.

```bash
docker ______ my-nginx
```

```bash
docker ______ my-nginx
```

```bash
docker ______ my-nginx
```

---

## Solution

<details>

<summary>Show Solution</summary>

```bash
docker stop my-nginx

docker start my-nginx

docker rm my-nginx
```

</details>

---

## Understanding

Stopping a container terminates the running application while preserving its data and configuration.

Starting the container resumes execution from its previous state.

Removing a container permanently deletes the container instance, but the Docker image remains available for creating new containers.

---

## Test & Verify

Execute each lifecycle command one by one.

After every command, verify the container status using:

```bash
docker ps -a
```

Questions:

- Does the container status change after each command?
- What status is shown after stopping the container?
- Is the container visible after removing it?

---

## Checkpoint

- [ ] Container stopped successfully.
- [ ] Container restarted successfully.
- [ ] Container paused and resumed.
- [ ] Container removed successfully.

---

## Experiment

Remove the container.

Now execute:

```bash
docker start my-nginx
```

Question:

Why does Docker display an error?

Explain your answer.

---

# Mini Challenge

Without referring to the previous sections, complete the following tasks.

- Download the official NGINX image.
- Create a container named **web-server**.
- Expose it on port **8081**.
- Mount the local HTML folder.
- Verify the web page from a browser.
- Display the container logs.

Record every command you used.

---

# Final Challenge

## Scenario

Your organization wants to host a company landing page using Docker.

Requirements:

- Create a new HTML page.
- Deploy it using an NGINX container.
- Use a different container name.
- Use port **9090**.
- Verify the website in your browser.
- Stop and remove the container after testing.

Do not refer to the previous chapters while completing this challenge.

---

# Epilogue

Congratulations!

You have successfully deployed and managed an NGINX web server using Docker.

Throughout this lab you learned how Docker images become containers, how local files are mounted into containers, how networking enables browser access, and how Docker simplifies application deployment.

---

## Architecture Summary

```text
Docker Hub
      │
      ▼
Docker Image
      │
      ▼
Docker Container
      │
      ▼
NGINX Web Server
      │
      ▼
Custom HTML Page
      │
      ▼
Browser
```

---

## Workflow Summary

```text
Verify Docker
      │
      ▼
Pull Image
      │
      ▼
Create Container
      │
      ▼
Mount HTML Folder
      │
      ▼
Configure Port Mapping
      │
      ▼
Access Website
      │
      ▼
Inspect Container
      │
      ▼
View Logs
      │
      ▼
Manage Lifecycle
```

---

# Key Principles

- Docker Images are templates.
- Containers are running instances of images.
- One image can create multiple containers.
- Volume mounting shares files between host and container.
- Port mapping connects host applications with container services.
- Docker logs simplify troubleshooting.
- Containers are temporary and can be recreated at any time.

---

# Troubleshooting

| Problem | Cause | Solution |
|----------|-------|----------|
| Port already in use | Another application is using the port | Use another host port |
| Container name already exists | Duplicate container name | Remove or rename the container |
| Website not accessible | Container stopped | Start the container |
| Image not found | Image not downloaded | Run `docker pull nginx` |

---

# Best Practices

- Use official Docker images.
- Assign meaningful container names.
- Keep images updated.
- Remove unused containers regularly.
- Mount project files instead of copying them.
- Avoid using the `latest` tag in production.
- Use Git to maintain project versions.

---

# Next Steps

Continue learning:

1. Dockerfile
2. Docker Compose
3. Docker Networks
4. Docker Volumes
5. Multi-container Applications
6. Docker Hub
7. Kubernetes

---

# Additional Resources

- https://docs.docker.com/
- https://docs.docker.com/reference/cli/
- https://hub.docker.com/
- https://hub.docker.com/_/nginx
- https://nginx.org/en/docs/

---

# Conclusion

This lab introduced the fundamental concepts of Docker by deploying an NGINX web server inside a container. You learned how to download Docker images, create containers, configure networking, mount local directories, inspect running containers, monitor logs, and manage the complete container lifecycle.

These practical skills provide a solid foundation for learning advanced Docker, DevOps, and cloud-native technologies.