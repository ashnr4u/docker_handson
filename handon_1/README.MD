# Flask Docker Basics

## What this is
A simple Flask app containerized using Docker.

## What I built
- A Flask API that returns "Hello World"
- Dockerized using a Dockerfile

## How to run

```bash
docker build -t flask-app .
docker run -p 3000:3000 flask-app
```
