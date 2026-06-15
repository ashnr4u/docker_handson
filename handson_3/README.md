# Docker Volumes & Bind Mounts – Hands-on Notes

## What are Volumes and Bind Mounts?

### Docker Volume
A Docker volume is storage managed by Docker to persist data outside a container. It is recommended for production use.

- Volumes are **managed by Docker**
- They exist outside the container lifecycle
- They persist even after container deletion
- They are more **portable and safer**

---

### Bind Mount
A bind mount links a folder from your **host machine directly into the container**.

- Direct mapping between host path and container path
- Very flexible for development
- Changes are reflected immediately on both sides

---

### Summary
Both volumes and bind mounts are used to persist data even when containers stop or are removed.

---

## Creating a Named Volume

```bash
docker volume create my-volume
```

This creates a Docker-managed storage area.

- Not inside any container
- Stored in Docker’s internal storage on the host system

---

## Running a Container with a Volume

```bash
docker run -it --rm -v my-volume:/my-data ubuntu:22.04
```

### Breakdown:
- `-v my-volume:/my-data` → mounts the volume inside the container at `/my-data`
- `--rm` → removes container after exit
- `-it` → interactive terminal

Inside the container:
- `/my-data` is backed by the Docker volume `my-volume`

---

## Writing Data to Volume

```bash
echo "Hello from the container!" > /my-data/hello.txt
```

- The file is stored in the **Docker volume (my-volume)**
- NOT inside the container filesystem

---

## Reading Data

```bash
cat /my-data/hello.txt
```

- Reads from the mounted volume
- Not from ephemeral container storage

---

## Important Note

If the container is deleted, the file still remains because it is stored in the volume.

---

## Bind Mount (Host Directory Mapping)

We can mount a directory from the host system using a bind mount:

```bash
docker run -it --rm -v ${PWD}/my-data:/my-data ubuntu:22.04
```

### Breakdown:
- `-v ${PWD}/my-data:/my-data` → bind mounts a local folder into container
- `${PWD}` → current directory in PowerShell

Inside container:
```bash
echo "Hello from the container!" > /my-data/hello.txt
```

- `/my-data` is now directly linked to your host folder
- Any file created here appears on your machine instantly

### Reading file:
```bash
cat /my-data/hello.txt
```

---

## Mental Model

### Volume
```
Container → /my-data → Docker Volume → hidden storage
```

### Bind Mount
```
Host Machine → local folder → mounted into container → /my-data
```

---

## Key Concepts

### Docker Components

- **Image** → blueprint (read-only template)
- **Container** → running instance with temporary filesystem
- **Volume** → external persistent storage (managed by Docker)
- **Bind Mount** → direct mapping to host filesystem

---

## Final Insight

When you “run an image”, you are actually:

> creating and starting a container from that image

---

## Quick Summary

- Volumes = Docker-managed persistent storage (best for production)
- Bind mounts = direct host folder mapping (best for development)
- Data persists beyond container lifecycle in both cases
```