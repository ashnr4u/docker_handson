------------------Docker build----------------------------------------------------------------
`docker build -t <image_name> . `→ image created → `docker ps` → 
`docker run -d -p <host_port>:<container_port> <image_name> `→ container starts → 
    application runs inside container → exposed on localhost:<host_port> → END

* `docker build -t <image_name> .` → Creates a Docker image from the Dockerfile in the current directory
* Image → A packaged snapshot of application + dependencies
* `docker ps` → Shows all running containers
* `docker run -d -p <host_port>:<container_port> <image_name>` → Starts a container and maps ports
* Container → A running instance of a Docker image
* Container starts → Docker initializes and runs the app inside container
* Application runs inside container → Your code executes in isolated environment
* `<container_port>` → Internal port where app listens inside container
* `<host_port>` → External port on your machine for access
* `localhost:<host_port>` → URL to access the running app in browser
* END → App is successfully running and accessible

-----------volume------------------------------
*docker volume create <volume_name> → creates a persistent storage area managed by Docker
*docker run -v <volume_name>:/app <image_name> → attaches volume to container for data storage
  Volume → a persistent storage layer that exists outside the container lifecycle
  Bind Mount → maps a host folder directly into the container filesystem
  -v <volume_name>:<container_path> → connects Docker volume to a folder inside container
  Data persistence → data is not lost even if container stops or is deleted
  Shared storage → multiple containers can use the same volume
*docker volume ls → lists all Docker volumes
*docker volume inspect <volume_name> → shows details of a specific volume
*END → data is safely stored outside container and survives restarts/deletion just list not list like blocks
______________________________________________________________________________________-

*FROM – base image (starting environment) → FROM python:3-slim
*WORKDIR – sets working directory inside container → WORKDIR /app
  -WORKDIR name can be anything — it only defines the container’s working folder, not your system folder.
*COPY – copies files from host to container → COPY . /app
*RUN – executes commands during image build → RUN pip install -r requirements.txt
*EXPOSE – declares which port the app uses → EXPOSE 3000
*CMD – default command when container starts → CMD ["python", "index.py"]
*ENTRYPOINT – main command that always runs → ENTRYPOINT ["python"]
*ENV – sets environment variables inside container → ENV PYTHONUNBUFFERED=1
*USER – runs container as a non-root user → USER appuser


## 🔁 After changing `index.py`
### 1. Stop container
```bash id="a1"
docker stop <container_id>
```
### 2. Remove container
``bash id="a2"
docker rm <container_id>
```
### 3. Rebuild image
```bash id="a3"
docker build -t <image_name> .
```
### 4. Run container
```bash id="a4"
docker run -d -p 3000:3000 --name <container_name> <image_name>
```
### 5. Check running containers
```bash id="a5"
docker ps
```

---

