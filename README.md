# Running an NGINX Web Server in a Docker Container

> **Hands-on Docker Lab using Puku CLI**

---

# 1. Introduction

This lab demonstrates how to deploy a simple **NGINX Web Server** inside a Docker container using **Puku CLI**. You will interact with Puku CLI using natural language prompts while it performs the required Docker operations.

During this lab, you will:

- Pull the official NGINX Docker image.
- Create and run an NGINX container.
- Serve a custom HTML page.
- Verify the running container.
- View container logs.
- Manage the Docker container lifecycle.

<p align="center">
    <img src="images/how_it_work.png" width="850">
</p>

---

# 2. Learning Objectives

After completing this lab, you will be able to:

- Pull Docker images.
- Create Docker containers.
- Configure Docker port mapping.
- Mount local HTML files.
- Verify running containers.
- Inspect container logs.
- Manage the container lifecycle.

---

# 3. Prerequisites

Before starting this lab, ensure the following tools are available.

| Software | Purpose |
|-----------|----------|
| Docker Engine | Run Docker containers |
| Puku CLI | Execute Docker tasks |
| Web Browser | Test the deployed web page |

Basic knowledge of Linux commands and HTML is recommended.

---

# 4. Real-World Scenario

A development team needs a lightweight web server to test a static website. Instead of installing NGINX directly on every machine, the team decides to deploy it inside a Docker container.

Your task is to use **Puku CLI** to deploy the NGINX web server, verify that it is running correctly, and manage the container.

---

# 5. Environment Setup

## Step 1 — Verify Docker Installation

### Ask Puku CLI

```text
Check whether Docker is installed and display the installed version.
```

Expected Result

- Docker version information is displayed.

<p align="center">
    <img src="images/version_dockerinfo.png" width="850">
</p>

---

## Step 2 — Verify Docker Engine

### Ask Puku CLI

```text
Verify that Docker Engine is running properly.
```

Expected Result

- Docker Engine is active and ready to create containers.

---

## Step 3 — Prepare the Workspace

Create the following project structure.

```text
nginx-lab/
│
├── html/
│   └── index.html
│
├── images/
│
└── README.md
```

Create the following HTML file.

```html
<!DOCTYPE html>
<html>
<head>
    <title>NGINX Docker Lab</title>
</head>

<body>

<h1>Hello from NGINX running in Docker!</h1>

</body>
</html>
```

---

# Think First

1. Why must an NGINX image be downloaded before creating a container?

2. Why is the HTML file stored outside the container?

---

# Fill in the Blanks

Complete the following.

```
A Docker container is created from a Docker __________.
```

```
NGINX serves web content from the __________ folder.
```

---

# Solution

<details>
<summary>Show Answers</summary>

```
Image
```

```
HTML
```

</details>

---

# Test & Verify

Before continuing, confirm the following.

- Docker is installed.
- Docker Engine is running.
- The project folder has been created.
- The HTML file is ready.

---

# Checkpoint

- [ ] Docker installation verified.
- [ ] Docker Engine running.
- [ ] Workspace prepared.
- [ ] HTML page created.

---

# Experiment

Modify the text inside **index.html** before deploying the container.

Predict whether the page will be visible before an NGINX container is started.

Record your observation.
````
````md
---

# Chapter 1 — Pull the NGINX Docker Image

## Overview

Before creating a container, the required Docker image must be downloaded from Docker Hub. In this chapter, you will pull the official NGINX image and verify that it has been downloaded successfully.

---

## What You Will Build

You will:

- Download the official NGINX Docker image.
- Verify the downloaded image.

---

## Think First

Before continuing, answer the following questions.

1. What is a Docker Image?

2. Can a container be created without an image?

---

## Implementation

### Step 1 — Pull the NGINX Image

### Ask Puku CLI

```text
Pull the latest official NGINX Docker image from Docker Hub.
```

Expected Result

- The image is downloaded successfully.

<p align="center">
    <img src="images/docker_pull.png" width="850">
</p>

---

### Step 2 — Verify the Image

### Ask Puku CLI

```text
Display all Docker images available on the local machine.
```

Expected Result

- The NGINX image appears in the image list.

<p align="center">
    <img src="images/docker_image.png" width="850">
</p>

---

## Fill in the Blanks

Complete the following.

```
Docker images are downloaded from __________.
```

```
The downloaded image is stored on the __________ machine.
```

---

## Solution

<details>
<summary>Show Answers</summary>

```
Docker Hub
```

```
Local
```

</details>

---

## Understanding

The official NGINX image is downloaded from Docker Hub and stored locally. Once downloaded, it can be reused multiple times to create Docker containers without downloading it again.

---

## Test & Verify

Verify the following.

- The image download completed successfully.
- The NGINX image appears in the local image list.

---

## Checkpoint

- [ ] NGINX image downloaded.
- [ ] Image verified successfully.

---

## Experiment

Ask Puku CLI to display Docker images again.

Observe whether the image is downloaded again or simply displayed from local storage.

---

# Chapter 2 — Create and Run the NGINX Container

## Overview

After downloading the image, you can create a running Docker container. The container will serve the HTML page stored in your local workspace.

---

## What You Will Build

You will:

- Create an NGINX container.
- Mount the local HTML folder.
- Configure Docker port mapping.
- Access the web page from a browser.

---

## Think First

1. Why is port mapping required?

2. Why is the HTML folder mounted into the container?

---

## Implementation

### Step 1 — Run the Container

### Ask Puku CLI

```text
Create a Docker container named my-nginx using the downloaded NGINX image. Mount the local html directory to the NGINX web root in read-only mode and expose the web server on port 8080.
```

Expected Result

- The container starts successfully.

<p align="center">
    <img src="images/docker-run.png" width="850">
</p>

---

### Understanding the Port Mapping

The browser accesses the application through **localhost:8080**, while NGINX listens on **port 80** inside the container.

<p align="center">
    <img src="images/port-mapping.png" width="850">
</p>

---

### Step 2 — Verify the Running Container

### Ask Puku CLI

```text
Display all currently running Docker containers.
```

Expected Result

- The **my-nginx** container appears with the **Up** status.

<p align="center">
    <img src="images/docker-ps.png" width="850">
</p>

---

## Fill in the Blanks

Complete the following.

```
The container is accessible through port ________.
```

```
NGINX listens on port ________ inside the container.
```

---

## Solution

<details>
<summary>Show Answers</summary>

```
8080
```

```
80
```

</details>

---

## Understanding

Docker maps **Host Port 8080** to **Container Port 80**. When a browser accesses `http://localhost:8080`, Docker forwards the request to the NGINX web server running inside the container.

---

## Test & Verify

Open the following URL in your browser.

```
http://localhost:8080
```

Verify that your custom HTML page is displayed successfully.

---

## Checkpoint

- [ ] Container created successfully.
- [ ] Container is running.
- [ ] Port mapping works correctly.
- [ ] Web page is accessible.

---

## Experiment

Edit the contents of **index.html**.

Refresh the browser and observe whether the changes appear immediately.

Explain why the container does not need to be recreated after modifying the HTML file.
````
````md
---

# Chapter 3 — View Container Logs

## Overview

After deploying the container, it is important to verify that the NGINX server is running correctly. Docker logs provide useful information about the container startup process and incoming client requests.

---

## What You Will Build

You will:

- View the NGINX container logs.
- Verify that the web server is running.
- Understand the information displayed in the logs.

---

## Think First

Before continuing, answer the following questions.

1. Why are Docker logs important?

2. What information can be obtained from container logs?

---

## Implementation

### Step 1 — Display Container Logs

### Ask Puku CLI

```text
Display the logs of the running Docker container named my-nginx.
```

Expected Result

- Startup information is displayed.
- NGINX version is shown.
- Request logs are visible.
- The container is running normally.

<p align="center">
    <img src="images/docker-logs.png" width="850">
</p>

---

## Key Observations

From the output, observe the following.

- NGINX starts successfully.
- The installed NGINX version is displayed.
- Incoming browser requests are recorded.
- HTTP Status **200 OK** confirms successful responses.

---

## Fill in the Blanks

Complete the following.

```
Container activity can be monitored using Docker ________.
```

```
A successful browser request returns HTTP status ________.
```

---

## Solution

<details>
<summary>Show Answers</summary>

```
Logs
```

```
200 OK
``>

</details>

---

## Understanding

Container logs help monitor application behavior while the container is running. They provide useful information for debugging, verifying deployment, and identifying runtime issues.

---

## Test & Verify

Refresh the browser several times.

Ask Puku CLI again:

```text
Display the logs of the running Docker container named my-nginx.
```

Verify that additional request entries appear in the output.

---

## Checkpoint

- [ ] Logs displayed successfully.
- [ ] NGINX startup verified.
- [ ] Browser requests recorded.
- [ ] Container operating normally.

---

## Experiment

Refresh the browser multiple times and compare the updated log entries.

What changes do you observe in the request log?

---

# Chapter 4 — Manage the Container Lifecycle

## Overview

Docker allows containers to be started, stopped, restarted, and removed whenever required. In this chapter, you will perform the basic lifecycle operations of the NGINX container.

---

## What You Will Build

You will learn how to:

- Stop a container.
- Restart a container.
- Remove a container.
- Understand the Docker container lifecycle.

<p align="center">
    <img src="images/container-lifecycle.png" width="850">
</p>

---

## Think First

Before continuing, answer the following questions.

1. What happens after a container is stopped?

2. Can a removed container be started again?

---

## Implementation

### Step 1 — Stop the Container

### Ask Puku CLI

```text
Stop the running Docker container named my-nginx.
```

Expected Result

- The container stops successfully.

---

### Step 2 — Start the Container

### Ask Puku CLI

```text
Start the Docker container named my-nginx.
```

Expected Result

- The container starts successfully.

---

### Step 3 — Remove the Container

### Ask Puku CLI

```text
Stop the container if necessary and remove the Docker container named my-nginx.
```

Expected Result

- The container is removed successfully.

---

## Fill in the Blanks

Complete the following.

```
A stopped container can be ________ again.
```

```
A removed container must be ________ again before use.
```

---

## Solution

<details>
<summary>Show Answers</summary>

```
Started
```

```
Created
``>

</details>

---

## Understanding

Stopping a container pauses its execution without deleting it. The container can be started again whenever needed.

Removing a container permanently deletes that container instance. However, the Docker image remains available and can be used to create a new container.

---

## Test & Verify

Verify the following.

- The container stops successfully.
- The container starts successfully.
- The container is removed successfully.

---

## Checkpoint

- [ ] Container stopped.
- [ ] Container restarted.
- [ ] Container removed.
- [ ] Docker image still available.

---

## Experiment

After removing the container, ask Puku CLI to display the available Docker images.

Observe whether the NGINX image still exists and explain why it remains available after the container has been removed.
````
````md
---

# Mini Challenge

Complete the following tasks independently using **Puku CLI**.

### Task List

- Pull the official NGINX Docker image.
- Create a container named **web-server**.
- Mount the local **html** directory.
- Expose the container on **port 8081**.
- Verify that the website is accessible.
- Display the container logs.

Record the prompts used and compare the results with the previous exercises.

---

# Final Challenge

## Scenario

A development team wants to deploy a simple company landing page using Docker. Your task is to create and deploy the website using Puku CLI.

### Requirements

- Create a new HTML page.
- Deploy it using an NGINX container.
- Use a different container name.
- Use **port 9090**.
- Verify the website from a browser.
- Display the container logs.
- Remove the container after testing.

---

# Epilogue

In this lab, you successfully deployed an NGINX web server using Docker through **Puku CLI**.

You learned how to:

- Download an official Docker image.
- Create and run a Docker container.
- Mount a local HTML directory.
- Configure Docker port mapping.
- Verify the running container.
- View Docker logs.
- Manage the Docker container lifecycle.

The skills learned in this lab provide a practical foundation for working with Docker-based applications.

---

# Workflow Summary

```text
Verify Docker
      │
      ▼
Pull NGINX Image
      │
      ▼
Run Docker Container
      │
      ▼
Mount HTML Directory
      │
      ▼
Configure Port Mapping
      │
      ▼
Verify Running Container
      │
      ▼
View Container Logs
      │
      ▼
Stop / Start / Remove Container
```

---

# Key Principles

- A Docker Image is used to create containers.
- A Docker Container is a running instance of an image.
- Docker Hub stores official container images.
- Port mapping connects the host to the container.
- Volume mounting allows local files to be served inside a container.
- Docker logs help monitor container activity.
- Containers can be stopped, restarted, and removed without deleting the image.

---

# Troubleshooting

| Problem | Cause | Solution |
|----------|-------|----------|
| Docker is not running | Docker Engine is stopped | Start Docker Engine and try again |
| NGINX image not found | Image has not been downloaded | Pull the official NGINX image again |
| Website is not accessible | Container is not running | Verify the container status and restart it |
| Port already in use | Another application is using the selected port | Use a different host port |

---

# Best Practices

- Use official Docker images whenever possible.
- Assign meaningful names to Docker containers.
- Store project files outside the container.
- Verify Docker resources after each operation.
- Remove unused containers to keep the environment clean.

---

# Next Steps

After completing this lab, continue exploring:

- Dockerfile
- Docker Compose
- Docker Volumes
- Docker Networks
- Multi-container Applications
- Kubernetes Fundamentals

---

# Additional Resources

- Docker Documentation  
  https://docs.docker.com/

- Docker Hub  
  https://hub.docker.com/

- Official NGINX Image  
  https://hub.docker.com/_/nginx

- NGINX Documentation  
  https://nginx.org/en/docs/

---

# Conclusion

This lab demonstrated how to deploy, verify, and manage an NGINX web server using Docker through **Puku CLI**.

By completing the exercises, you gained practical experience with Docker images, containers, networking, volume mounting, logging, and lifecycle management. These skills provide a solid starting point for more advanced Docker and containerization workflows.
````
