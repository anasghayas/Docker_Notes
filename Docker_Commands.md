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
* `docker run -d nginx` - Runs an Nginx container in "detached" mode (in the background so it doesn't block your terminal).
* `docker run -p 8080:80 nginx` - Maps port `80` of the Nginx container to port `8080` on your local machine (so you can view it at `localhost:8080` in your browser).
* `docker run -it ubuntu bash` - Runs an Ubuntu container interactively (`-i`) and attaches a terminal (`-t`), opening a `bash` shell so you can type commands directly inside the container.

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
