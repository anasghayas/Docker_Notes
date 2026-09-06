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

**Volume Mounting Examples:**
* **Host Volume:** `docker run -v [host_path]:[container_path] [image]`
  *(Example: `docker run -v /home/user/app:/var/lib/mysql mysql`)*
* **Anonymous Volume:** `docker run -v [container_path] [image]`
  *(Example: `docker run -v /var/lib/mysql mysql`)*
* **Named Volume:** `docker run -v [volume_name]:[container_path] [image]`
  *(Example: `docker run -v my_db_data:/var/lib/mysql mysql`)*

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

### `docker rm`
Used to completely delete a stopped container from your system. You cannot remove a running container unless you force it (with the `-f` flag) or stop it first.

**Syntax:**
```bash
docker rm [CONTAINER_ID or NAME]
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

### `docker build`
Used to create a new Docker image from a `Dockerfile`.

**Syntax:**
```bash
docker build -t [IMAGE_NAME]:[VERSION] [PATH_TO_DOCKERFILE]
```
**Common Uses:**
* `docker build -t my_app:1.0 .`
  * **`-t my_app:1.0`**: "Tags" the image with the name `my_app` and the version `1.0`.
  * **`.` (The dot)**: This is crucial. It tells Docker to look for the `Dockerfile` and build context in the **current directory**.

### `docker rmi`
Used to completely delete an image from your local system. Note that you cannot remove an image if a container (even a stopped one) is currently using it.

**Syntax:**
```bash
docker rmi [IMAGE_ID or NAME]
```

### `docker tag`
Used to rename or duplicate an image to a new name or tag. This is especially important when you need to push a local image to a specific remote registry (like Amazon ECR) because the image name must contain the full registry domain.

**Syntax:**
```bash
docker tag [LOCAL_IMAGE_NAME]:[VERSION] [REGISTRY_DOMAIN]/[NEW_IMAGE_NAME]:[VERSION]
```
*Example: `docker tag my-app:1.0 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0`*

### `docker push`
Used to upload a local Docker image to a remote Docker registry (like Docker Hub or Amazon ECR) so it can be pulled by other servers or users.

**Syntax:**
```bash
docker push [IMAGE_NAME]:[TAG]
```
*Example: `docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0`*

*(Note: When pushing to AWS ECR, you must first authenticate your Docker engine using the AWS CLI. The command generally looks like this: `aws ecr get-login-password | docker login --username AWS --password-stdin [REGISTRY_DOMAIN]`)*

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

## Docker Compose Management

### `docker-compose up`
Used to build, (re)create, start, and attach to containers for a service defined in a Compose file. 

**Syntax:**
```bash
docker-compose -f [FILE_NAME] up [OPTIONS]
```
*Note: If your file is named `docker-compose.yaml`, you don't need the `-f` flag. You can just type `docker-compose up`.*

**Common Examples:**
* `docker-compose -f mongo.yaml up` - Starts the containers defined in the `mongo.yaml` file.
* `docker-compose up -d` - Starts the services in detached mode (in the background).

### `docker-compose down`
Used to stop and remove containers and networks created by `docker-compose up`.

**Syntax:**
```bash
docker-compose -f [FILE_NAME] down
```
