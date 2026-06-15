# Most Used Docker Commands
## Image Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker pull <image>` | Download an image from registry | `docker pull nginx` |
| `docker build -t <name> .` | Build image from Dockerfile | `docker build -t myapp .` |
| `docker images` | List all local images | `docker images` |
| `docker rmi <image>` | Remove an image | `docker rmi myapp:latest` |
| `docker tag <image> <new_tag>` | Tag an image | `docker tag myapp:latest myapp:v1` |
| `docker push <image>` | Upload image to registry | `docker push username/myapp` |

## Container Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker run <image>` | Create and start container | `docker run nginx` |
| `docker run -d <image>` | Run in background (detached) | `docker run -d nginx` |
| `docker run -p 8080:80 <image>` | Map host port to container port | `docker run -p 8080:80 nginx` |
| `docker run --name <name> <image>` | Assign container name | `docker run --name web nginx` |
| `docker run -v <volume>:/path <image>` | Mount volume | `docker run -v data:/app nginx` |
| `docker run -it <image>` | Interactive mode with terminal | `docker run -it ubuntu bash` |
| `docker ps` | List running containers | `docker ps` |
| `docker ps -a` | List all containers (including stopped) | `docker ps -a` |
| `docker stop <container>` | Stop running container | `docker stop web` |
| `docker start <container>` | Start stopped container | `docker start web` |
| `docker restart <container>` | Restart container | `docker restart web` |
| `docker rm <container>` | Remove stopped container | `docker rm web` |
| `docker rm -f <container>` | Force remove running container | `docker rm -f web` |
| `docker exec -it <container> <cmd>` | Run command in running container | `docker exec -it web bash` |
| `docker logs <container>` | View container logs | `docker logs web` |
| `docker logs -f <container>` | Follow logs in real-time | `docker logs -f web` |
| `docker inspect <container>` | Show container details | `docker inspect web` |

## Volume Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker volume create <name>` | Create a volume | `docker volume create mydata` |
| `docker volume ls` | List all volumes | `docker volume ls` |
| `docker volume inspect <name>` | Show volume details | `docker volume inspect mydata` |
| `docker volume rm <name>` | Remove a volume | `docker volume rm mydata` |
| `docker volume prune` | Remove unused volumes | `docker volume prune` |

## Network Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker network ls` | List networks | `docker network ls` |
| `docker network create <name>` | Create network | `docker network create mynet` |
| `docker network connect <net> <container>` | Connect container to network | `docker network connect mynet web` |
| `docker network disconnect <net> <container>` | Disconnect container | `docker network disconnect mynet web` |
| `docker network inspect <name>` | Show network details | `docker network inspect mynet` |
| `docker network rm <name>` | Remove network | `docker network rm mynet` |

## Cleanup Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker system prune` | Remove unused data | `docker system prune` |
| `docker system prune -a` | Remove all unused images and containers | `docker system prune -a` |
| `docker container prune` | Remove stopped containers | `docker container prune` |
| `docker image prune` | Remove unused images | `docker image prune` |

## Info Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker version` | Show Docker version info | `docker version` |
| `docker info` | Show system-wide info | `docker info` |
| `docker stats` | Live container resource usage | `docker stats` |
| `docker top <container>` | Show processes in container | `docker top web` |

## Quick Reference - Most Common Flags

| Flag | Description | Example |
|------|-------------|---------|
| `-d` | Run in background | `docker run -d nginx` |
| `-it` | Interactive + TTY | `docker run -it ubuntu` |
| `-p` | Port mapping (host:container) | `docker run -p 8080:80 nginx` |
| `-v` | Volume mount | `docker run -v /host:/container nginx` |
| `--name` | Name the container | `docker run --name web nginx` |
| `--rm` | Auto-remove on exit | `docker run --rm ubuntu` |
| `-e` | Set environment variable | `docker run -e ENV=prod nginx` |
| `--network` | Connect to network | `docker run --network mynet nginx` |
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
