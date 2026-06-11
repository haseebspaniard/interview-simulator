# 🎯 DevOps Interview Simulator

> **AI-Powered Mock Interview Platform** — Containerized with Docker, served via Nginx on port 80

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-haseebspaniard%2Finterview--simulator-blue?logo=docker)](https://hub.docker.com/r/haseebspaniard/interview-simulator)
[![Docker Image Version](https://img.shields.io/badge/version-v1-green)](https://hub.docker.com/r/haseebspaniard/interview-simulator/tags)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Table of Contents

- [About the Project](#-about-the-project)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Architecture Overview](#-architecture-overview)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Run from Docker Hub (Recommended)](#run-from-docker-hub-recommended)
  - [Run from Source (Local Build)](#run-from-source-local-build)
- [Docker Workflow](#-docker-workflow)
  - [Build the Image](#1-build-the-image)
  - [Run the Container](#2-run-the-container)
  - [Push to Docker Hub](#3-push-to-docker-hub)
  - [Pull and Run Anywhere](#4-pull-and-run-anywhere)
- [Dockerfile Explained](#-dockerfile-explained)
- [CI/CD Pipeline (GitHub Actions)](#-cicd-pipeline-github-actions)
- [Useful Docker Commands](#-useful-docker-commands)
- [Contributing](#-contributing)
- [Author](#-author)

---

## 📖 About the Project

The **DevOps Interview Simulator** is an AI-powered mock interview platform designed to help engineers prepare for DevOps, cloud, and infrastructure interviews. It is a fully static web application served using **Nginx** inside a **Docker container**.

This project was created as a demonstration of end-to-end Docker containerization — from writing a `Dockerfile`, building an image, pushing it to Docker Hub, and deploying it on any server with a single command.

### Key Highlights

- ✅ Zero runtime dependencies (no Node.js, no Python needed on the server)
- ✅ Lightweight Nginx-based container using the `nginx:alpine` base image
- ✅ Single command deployment anywhere Docker is installed
- ✅ Pushed and publicly available on Docker Hub
- ✅ Full GitHub repository with version control

---

## 📁 Project Structure

```
interview-simulator/
├── index.html           # Main application entry point
├── support.js           # JavaScript logic for the simulator
├── Dockerfile           # Docker build instructions
├── .dockerignore        # Files excluded from the Docker image
├── assets/
│   ├── logo.png         # Application logo
│   ├── logo-dark.png    # Dark mode logo variant
│   └── mark.png         # Brand mark / favicon
└── uploads/
    └── akumen logo-06.png   # Uploaded client assets
```

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Application** | HTML5, JavaScript (Vanilla) |
| **Web Server** | Nginx (Alpine) |
| **Containerization** | Docker |
| **Container Registry** | Docker Hub |
| **Version Control** | Git + GitHub |
| **OS (Dev)** | Ubuntu (VirtualBox VM) |

---

## 🏗 Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                        LOCAL MACHINE                             │
│                                                                  │
│  ┌────────────────┐   docker build   ┌──────────────────────┐   │
│  │  Source Files  │ ──────────────►  │  Docker Image        │   │
│  │                │                  │  (nginx:alpine base)  │   │
│  │  index.html    │                  │  + app files          │   │
│  │  support.js    │                  │  interview-sim:v1     │   │
│  │  assets/       │                  └──────────────────────┘   │
│  │  Dockerfile    │                            │                 │
│  └────────────────┘               docker push  │                 │
└───────────────────────────────────────────────┼─────────────────┘
                                                 ▼
┌──────────────────────────────────────────────────────────────────┐
│                        DOCKER HUB                                │
│                                                                  │
│       haseebspaniard/interview-simulator:v1                      │
│       (Publicly accessible image registry)                       │
└──────────────────────────────────────────────┬───────────────────┘
                                               │
                                  docker pull  │
                                               ▼
┌──────────────────────────────────────────────────────────────────┐
│                      PRODUCTION SERVER                           │
│                                                                  │
│   docker run -d -p 80:80 haseebspaniard/interview-simulator:v1  │
│                                                                  │
│   ┌─────────────────────────────────────────────────┐           │
│   │  Docker Container                               │           │
│   │  ┌─────────────┐    serves    ┌──────────────┐  │           │
│   │  │    Nginx    │ ──────────►  │  index.html  │  │           │
│   │  │  Port 80    │              │  + assets    │  │           │
│   │  └─────────────┘              └──────────────┘  │           │
│   └─────────────────────────────────────────────────┘           │
└──────────────────────────────────────────────┬───────────────────┘
                                               │
                                    HTTP :80   │
                                               ▼
                                    👤  End User
                                  http://<SERVER-IP>
```

---

## 🚀 Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your machine or server
- That's it — no other runtime required

---

### Run from Docker Hub (Recommended)

The fastest way to run the simulator. No source code needed.

```bash
docker pull haseebspaniard/interview-simulator:v1

docker run -d \
  --name devops-simulator \
  -p 80:80 \
  haseebspaniard/interview-simulator:v1
```

Open your browser and navigate to:

```
http://localhost          # On your local machine
http://<YOUR-SERVER-IP>   # On a remote/cloud VM
```

---

### Run from Source (Local Build)

**Step 1 — Clone the repository**

```bash
git clone https://github.com/haseebspaniard/interview-simulator.git
cd interview-simulator
```

**Step 2 — Build the Docker image**

```bash
docker build -t interview-simulator:v1 .
```

**Step 3 — Run the container**

```bash
docker run -d \
  --name devops-simulator \
  -p 80:80 \
  interview-simulator:v1
```

**Step 4 — Open in browser**

```
http://localhost
```

---

## 🐳 Docker Workflow

### 1. Build the Image

```bash
docker build -t devops-interview-simulator:v1 .
```

Verify the image was created:

```bash
docker images
```

Expected output:

```
REPOSITORY                    TAG    IMAGE ID       SIZE
devops-interview-simulator    v1     147a2fbe2087   93.6MB
```

---

### 2. Run the Container

```bash
docker run -d \
  --name devops-simulator \
  -p 80:80 \
  devops-interview-simulator:v1
```

Check the container is running:

```bash
docker ps
```

Expected output:

```
CONTAINER ID   IMAGE                          STATUS         PORTS                  NAMES
5edad1d28b9f   devops-interview-simulator     Up 1 hour      0.0.0.0:80->80/tcp     devops-simulator
```

---

### 3. Push to Docker Hub

Tag the image with your Docker Hub username:

```bash
docker tag devops-interview-simulator:v1 haseebspaniard/interview-simulator:v1
```

Login to Docker Hub:

```bash
docker login
```

Push the image:

```bash
docker push haseebspaniard/interview-simulator:v1
```

---

### 4. Pull and Run Anywhere

On any machine with Docker installed — no source files needed:

```bash
docker pull haseebspaniard/interview-simulator:v1
docker run -d -p 80:80 haseebspaniard/interview-simulator:v1
```

---

### Save and Load the Image (Offline Transfer)

Export the image to a `.tar` file:

```bash
docker save devops-interview-simulator:v1 > devops-interview-simulator.tar
```

Import the image on another machine:

```bash
docker load < devops-interview-simulator.tar
```

---

## 📄 Dockerfile Explained

```dockerfile
# Use the official lightweight Nginx image based on Alpine Linux
# Alpine keeps the image size minimal (~5MB base)
FROM nginx:alpine

# Copy all project files into Nginx's default web root directory
# Nginx will serve files from /usr/share/nginx/html/ automatically
COPY . /usr/share/nginx/html/

# Expose port 80 for HTTP traffic
EXPOSE 80

# Start Nginx in the foreground (required for Docker to keep the container alive)
CMD ["nginx", "-g", "daemon off;"]
```

**Why `nginx:alpine`?**

| Feature | Benefit |
|---|---|
| Alpine Linux base | Tiny image size (~5MB) |
| No runtime needed | No Node.js or Python required |
| Built-in static file serving | Zero configuration needed |
| Production-grade web server | Fast and secure by default |

---

## ⚙️ CI/CD Pipeline (GitHub Actions)

A GitHub Actions workflow can automate the entire build and push process on every `git push`.

Create `.github/workflows/docker-publish.yml`:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/interview-simulator:v1 .

      - name: Push Docker image to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/interview-simulator:v1
```

**Setup:**

1. Go to your GitHub repo → **Settings → Secrets and variables → Actions**
2. Add two secrets:
   - `DOCKER_USERNAME` — your Docker Hub username
   - `DOCKER_PASSWORD` — your Docker Hub access token

Now every push to `main` automatically builds and publishes the image.

---

## 🧰 Useful Docker Commands

```bash
# List all running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# List all local images
docker images

# Stop a running container
docker stop devops-simulator

# Remove a container
docker rm devops-simulator

# Remove an image
docker rmi devops-interview-simulator:v1

# View container logs
docker logs devops-simulator

# Open a shell inside the running container
docker exec -it devops-simulator sh

# Inspect container details
docker inspect devops-simulator
```

---

## 🤝 Contributing

Contributions are welcome. To contribute:

1. Fork this repository
2. Create a new branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "add: your feature description"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 👤 Author

**Haseeb** — DevOps Trainer & Engineer

- Docker Hub: [haseebspaniard](https://hub.docker.com/u/haseebspaniard)
- GitHub: [haseebspaniard](https://github.com/haseebspaniard)

---

> **Build once. Run anywhere. That's the power of Docker.**
