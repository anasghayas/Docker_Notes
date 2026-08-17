# Docker Commands Reference

This file contains a list of common Docker commands and their uses.

## Image & Container Execution

### `docker run`
Used to create and start a new container from a specified Docker image. If the image is not available locally, Docker will automatically try to pull (download) it from Docker Hub before running it.

**Syntax:**
```bash
docker run [OPTIONS] IMAGE[:TAG] [COMMAND] [ARG...]
```

**Common Examples & Uses:**
* `docker run ubuntu:22.04` - Runs a container using the Ubuntu 22.04 image.
* `docker run -it ubuntu bash` - Runs an Ubuntu container interactively (`-i`) and attaches a terminal (`-t`), opening a `bash` shell.

**Port Binding & Naming Examples:**
* **Bind a Port:** `docker run -p [host_port]:[container_port] [image]:[tag]`
  *(Example: `docker run -p 8080:80 nginx:latest` maps port 80 inside the container to port 8080 on your host machine).*
* **Name a Container & Run in Background:** `docker run -d -p 8080:80 --name my_web_server nginx:latest`
  *(Runs detached `-d`, maps the port, and explicitly assigns the name `my_web_server` to the container).*

## Container Management

### `docker ps`
Used to list Docker containers. By default, it only shows containers that are currently running.

**Syntax:**
```bash
docker ps [OPTIONS]
```

**Common Examples & Uses:**
* `docker ps` - Lists all currently running containers, showing their container ID, image, command, created time, status, mapped ports, and names.
* `docker ps -a` (or `--all`) - Lists **all** containers, including those that have stopped or exited. This is useful for finding the ID or name of a container that crashed or finished its job.
* `docker ps -q` - Only displays the numeric container IDs (quiet mode). This is commonly used in scripts (e.g., to pass all running container IDs to a `docker stop` command).

### `docker start`
Used to start one or more stopped containers. It does not create a new container; it just resumes a previously created one.

**Syntax:**
```bash
docker start [CONTAINER_ID or NAME]
```

### `docker stop`
Used to gracefully stop one or more running containers.

**Syntax:**
```bash
docker stop [CONTAINER_ID or NAME]
```

### `docker exec`
Used to run a new command inside an *already running* container. This is extremely useful for troubleshooting or inspecting the inside of a container.

**Syntax:**
```bash
docker exec [OPTIONS] [CONTAINER_ID or NAME] [COMMAND]
```
**Common Uses (Going inside a container):**
* `docker exec -it my_container /bin/bash`
  * **`-it`**: Stands for **interactive** (`-i`) and allocates a **tty/terminal** (`-t`), allowing you to type commands.
  * **`/bin/bash`**: Opens the bash shell. 
  * *Note:* If `bash` doesn't work (which is common in minimal images like Alpine), try using `sh` instead: `docker exec -it my_container sh`.
  * Once inside, you can use standard Linux commands. For example, `cd /` will take you to the root directory.

### `docker logs`
Used to fetch the logs of a container. This is essential for debugging applications that are running in the background (detached mode).

**Syntax:**
```bash
docker logs [OPTIONS] [CONTAINER_ID or NAME]
```
**Common Uses:**
* `docker logs my_container` - Shows the static logs for that container.
* `docker logs -f my_container` - Follows the log output live (like `tail -f`), streaming new logs to your terminal as they happen.

## Image Management

### `docker images`
Used to list all the Docker images currently downloaded and stored locally on your machine. It shows the repository, tag, image ID, creation date, and size.

**Syntax:**
```bash
docker images
```

## Network Management

### `docker network ls`
Used to list all the virtual networks currently managed by Docker on your machine.

**Syntax:**
```bash
docker network ls
```

### `docker network create`
Used to create a brand new, custom Docker network. Containers connected to the same custom network can talk to each other securely using their container names.

**Syntax:**
```bash
docker network create [NETWORK_NAME]
```
*Example: `docker network create my-custom-network`*
