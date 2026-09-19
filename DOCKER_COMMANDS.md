# 🐋 Docker & Docker Compose Cheatsheet

This cheatsheet covers the most essential commands you'll use day-to-day when working with Docker and Docker Compose.

---

## 🏗️ Images
Images are the read-only templates used to create containers (like a blueprint).

| Command | Description | Example |
| :--- | :--- | :--- |
| `docker build` | Build an image from a Dockerfile. | `docker build -t my-app .` |
| `docker images` | List all local images on your machine. | `docker images` |
| `docker pull` | Download an image from Docker Hub. | `docker pull ubuntu:latest` |
| `docker rmi` | Delete a specific image. | `docker rmi my-app:latest` |
| `docker image prune` | Remove all unused, dangling images. | `docker image prune` |

---

## 📦 Containers
Containers are the running instances of images.

| Command | Description | Example |
| :--- | :--- | :--- |
| `docker run` | Create and start a new container from an image. | `docker run -d -p 8080:80 nginx` |
| `docker ps` | List all **running** containers. | `docker ps` |
| `docker ps -a` | List **all** containers (running and stopped). | `docker ps -a` |
| `docker stop` | Gracefully stop a running container. | `docker stop <container_id>` |
| `docker start` | Start a stopped container. | `docker start <container_id>` |
| `docker rm` | Delete a stopped container. | `docker rm <container_id>` |
| `docker rm -f` | Force delete a container (even if running). | `docker rm -f <container_id>` |
| `docker exec` | Run a command inside a running container. | `docker exec -it <container_id> sh` |
| `docker logs` | View the console output/logs of a container. | `docker logs -f <container_id>` |

**Common `docker run` Flags:**
- `-d`: Detached mode (runs in the background).
- `-p host_port:container_port`: Port mapping.
- `-v host_path:container_path`: Volume mounting.
- `-e VAR=value`: Set environment variable.
- `--name my_name`: Assign a custom name to the container.
- `--rm`: Automatically remove the container when it stops.

---

## 🐙 Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications.

| Command | Description | Example |
| :--- | :--- | :--- |
| `docker-compose up` | Build, (re)create, start, and attach to containers. | `docker-compose up` |
| `docker-compose up -d` | Start containers in the background (detached). | `docker-compose up -d` |
| `docker-compose down` | Stop and remove containers, networks, and images. | `docker-compose down` |
| `docker-compose down -v`| Stop everything AND delete named volumes (wipes data).| `docker-compose down -v` |
| `docker-compose logs` | View output from all services. | `docker-compose logs -f` |
| `docker-compose ps` | List containers managed by Compose. | `docker-compose ps` |
| `docker-compose build`| Rebuild services (useful if you changed a Dockerfile).| `docker-compose build` |

---

## 🧹 System & Cleanup
Use these commands carefully to free up disk space!

| Command | Description |
| :--- | :--- |
| `docker system df` | Show docker disk usage. |
| `docker system prune` | Remove all stopped containers, dangling images, and unused networks. |
| `docker system prune -a`| Extremely destructive: removes everything not currently in use (including downloaded images). |

---

## 🌐 Networks & 💾 Volumes

| Command | Description |
| :--- | :--- |
| `docker network ls` | List all Docker networks. |
| `docker volume ls` | List all Docker volumes. |
| `docker volume rm` | Delete a specific volume. |
