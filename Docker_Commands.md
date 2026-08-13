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
