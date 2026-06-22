# Flask Docker Basics

## What this is

A simple Flask application containerized using Docker. This project demonstrates the fundamentals of creating a Docker image, running a container, and exposing an application through a port.

## What I built

* A Flask API that returns **"Hello World"**
* A Docker image using a custom Dockerfile
* A containerized application that can run consistently across different environments

---

## Dockerfile

```dockerfile
FROM python:3-alpine3.15
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
EXPOSE 3000
CMD python ./index.py
```

---

## Dockerfile Breakdown

| Instruction                           | Purpose                                                   |
| ------------------------------------- | --------------------------------------------------------- |
| `FROM python:3-alpine3.15`            | Uses the official Python image built on Alpine Linux 3.15 |
| `WORKDIR /app`                        | Sets `/app` as the working directory inside the container |
| `COPY . /app`                         | Copies all project files into the container               |
| `RUN pip install -r requirements.txt` | Installs Python dependencies during image build           |
| `EXPOSE 3000`                         | Documents that the application listens on port 3000       |
| `CMD python ./index.py`               | Starts the application when the container runs            |

---

## What does `python:3-alpine3.15` mean?

`python:3-alpine3.15` is an official Python Docker image where:

* `python:3` refers to the Python 3 version family.
* `alpine3.15` refers to Alpine Linux version 3.15, which is the underlying operating system used in the image.

This image combines:

```text
Python Runtime
+
Alpine Linux 3.15
```

and provides a lightweight environment for running Python applications.

---

## Why use Alpine Linux?

Alpine Linux is designed to be small and lightweight.

| Base Image  | Approximate Size |
| ----------- | ---------------- |
| Ubuntu      | ~70–80 MB        |
| Debian Slim | ~20–30 MB        |
| Alpine      | ~5–7 MB          |

Benefits:

* Smaller image size
* Faster image downloads
* Reduced storage usage
* Faster deployments

> Note: Exact image sizes may vary between releases.

---

## What is Docker Image Size?

Docker image size refers to the amount of disk space occupied by all image layers.

Smaller images generally provide:

* Faster downloads
* Faster deployments
* Lower storage requirements
* Improved CI/CD performance

Example image sizes:

| Image                 | Approximate Size |
| --------------------- | ---------------- |
| `python:3`            | ~1 GB+           |
| `python:3-slim`       | ~100–200 MB      |
| `python:3-alpine3.15` | ~50–70 MB        |

> Note: These values can change over time as images are updated.

---


## Build the Docker Image

```bash
docker build -t flask-app .
```

Explanation:

* `docker build` creates an image from the Dockerfile
* `-t flask-app` assigns a name to the image
* `.` specifies the current directory as the build context

---

## Run the Container

```bash
docker run -p 3000:3000 flask-app
```
Explanation:
* First `3000` → Host machine port
* Second `3000` → Container port

Requests made to:

```text
http://localhost:3000
```

are forwarded to the Flask application running inside the container.

---

## Key Docker Concepts Learned

* Creating a Docker image using a Dockerfile
* Understanding Docker image tags
* Using lightweight base images (Alpine)
* Installing dependencies during image build
* Exposing application ports
* Running containers from images
* Mapping host ports to container ports
* Using Docker for consistent application deployment
