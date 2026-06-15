# Docker Hands-on 2: Ubuntu Container

Pulled and started an Ubuntu 22.04 container:

```bash
docker run --interactive --tty --rm ubuntu:22.04
```
Shorthand equivalent:
```bash
docker run -it --rm ubuntu:22.04 bash
```
* `-i` (`--interactive`) keeps standard input open.
* `-t` (`--tty`) allocates a terminal session.
* `--rm` automatically removes the container when it exits.
Updated package lists:
```bash
apt update
```
Installed the ping utility:
```bash
apt install iputils-ping --yes
```
Verified internet connectivity:
```bash
ping google.com -c 1
```
**Note:** Since the container was started with `--rm`, all changes made inside it are deleted when the container exits.

_____________________________________________________________________________________________

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
_____________________________________________________________________________________________
# Creating docker file
* The Why?
This allows you to continue using the same container with all previously installed packages intact.
A container stores changes made during its lifetime, such as installed packages. By creating a container without the `--rm` flag, those changes persist when the container is stopped and started again. However, the changes exist only in that specific container. To make the setup reusable, a Dockerfile can be used to build a custom image with the required dependencies pre-installed. Any container created from that image will automatically have the same environment.
