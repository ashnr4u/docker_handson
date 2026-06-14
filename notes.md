Here's everything in a single markdown code block:

```markdown
# Docker Build Process

```bash
docker build -t <image_name> .
```
- Creates a Docker image from the Dockerfile in the current directory
- **Image**: A packaged snapshot of application + dependencies

```bash
docker run -d -p <host_port>:<container_port> <image_name>
```
- Starts a container and maps ports
- **Container**: A running instance of a Docker image
- `<container_port>`: Internal port where app listens inside container  
- `<host_port>`: External port on your machine for access

```bash
docker ps
```
- Shows all running containers

**Result**: Application runs inside container → exposed on `localhost:<host_port>`

---

# Volumes

```bash
docker volume create <volume_name>
```
- Creates a persistent storage area managed by Docker

```bash
docker run -v <volume_name>:/app <image_name>
```
- Attaches volume to container for data storage

**Key Concepts:**
- **Volume**: Persistent storage layer outside container lifecycle
- **Bind Mount**: Maps a host folder directly into container filesystem
- `-v <volume_name>:<container_path>`: Connects Docker volume to container folder

**Benefits:**
- Data persistence (data survives container stop/delete)
- Shared storage (multiple containers can use same volume)

**Commands:**
```bash
docker volume ls           # Lists all Docker volumes
docker volume inspect <volume_name>   # Shows volume details
```

---

# Dockerfile Instructions

| Instruction | Purpose | Example |
|-------------|---------|---------|
| `FROM` | Base image (starting environment) | `FROM python:3-slim` |
| `WORKDIR` | Sets working directory inside container | `WORKDIR /app` |
| `COPY` | Copies files from host to container | `COPY . /app` |
| `RUN` | Executes commands during image build | `RUN pip install -r requirements.txt` |
| `EXPOSE` | Declares which port the app uses | `EXPOSE 3000` |
| `CMD` | Default command when container starts | `CMD ["python", "index.py"]` |
| `ENTRYPOINT` | Main command that always runs | `ENTRYPOINT ["python"]` |
| `ENV` | Sets environment variables | `ENV PYTHONUNBUFFERED=1` |
| `USER` | Runs container as non-root user | `USER appuser` |

> **Note**: `WORKDIR` can be anything — it only defines the container's working folder, not your system folder.

---

# After Changing `index.py`

1. **Stop container**
   ```bash
   docker stop <container_id>
   ```

2. **Remove container**
   ```bash
   docker rm <container_id>
   ```

3. **Rebuild image**
   ```bash
   docker build -t <image_name> .
   ```

4. **Run container**
   ```bash
   docker run -d -p 3000:3000 --name <container_name> <image_name>
   ```

5. **Check running containers**
   ```bash
   docker ps
   ```
```
