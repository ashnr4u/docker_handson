
# Docker Hands-on 2: Ubuntu Container

## Pull and Run an Ubuntu 22.04 Container

Start an interactive Ubuntu 22.04 container that is automatically removed after exit:

```bash
docker run --interactive --tty --rm ubuntu:22.04
```

Shorthand equivalent:

```bash
docker run -it --rm ubuntu:22.04 bash
```

- `-i` (or `--interactive`) keeps standard input open  
- `-t` (or `--tty`) allocates a terminal session  
- `--rm` automatically removes the container when it exits

## Inside the Container

Update package lists:

```bash
apt update
```

Install the `ping` utility:

```bash
apt install iputils-ping --yes
```

Test internet connectivity:

```bash
ping google.com -c 1
```

> **Note:** Since the container was started with `--rm`, all changes made inside it are deleted when the container exits.

---

## Persist Changes Without `--rm`

To keep installed packages and other changes, start a container **without** the `--rm` flag:

```bash
docker run -it --name my-ubuntu-container ubuntu:22.04 bash
```

After exiting, the container can be started again:

```bash
docker start my-ubuntu-container
```

To open a shell inside the running container:

```bash
docker exec -it my-ubuntu-container bash
```

---

## Creating a Dockerfile

### Why Create an Image?

Creating an image allows you to reuse the same environment setup across multiple containers. While containers started without `--rm` preserve changes, those changes are tied to a specific container. A Dockerfile automates the setup so any container created from the resulting image has all dependencies pre-installed.

### Step 1: Create a Dockerfile

Create a file named `Dockerfile` (no extension) with the following content:

```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install -y iputils-ping
```

### Step 2: Build the Image

Run the following command in the same directory as the `Dockerfile`:

```bash
docker build -t my-ubuntu-image .
```

- `-t` assigns a name/tag to the image  
- `.` refers to the current directory (where the Dockerfile is located)

### Step 3: Run a Container from the Image

```bash
docker run -it --rm my-ubuntu-image
```

## Key Idea

A Dockerfile allows you to **automate environment setup** so every container created from the image already has the required dependencies installed — no manual installation needed.
```

