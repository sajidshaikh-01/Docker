# Docker Commands – 
---
## 1. Docker Version & Info Commands

### Check Docker version

```bash
docker --version
```

Shows installed Docker version.

### Detailed Docker info

```bash
docker info
```

Displays Docker engine status, storage driver, cgroups, containers, images, etc.

---

## 2. Docker Image Commands

### List Docker images

```bash
docker images
```

Shows all images available locally.

---

### Pull an image from registry

```bash
docker pull nginx
```

Downloads image from Docker Hub (or configured registry).

---

### Build Docker image

```bash
docker build -t myapp:1.0 .
```

Builds an image using Dockerfile in current directory.

---

### Remove an image

```bash
docker rmi nginx
```

Deletes image from local system.

---

### Remove unused images

```bash
docker image prune
```

Cleans dangling (unused) images.

---

## 3. Docker Container Commands

### Run a container

```bash
docker run nginx
```

Creates and starts a container.

---

### Run container in detached mode

```bash
docker run -d nginx
```

Runs container in background.

---

### Run container with port mapping

```bash
docker run -p 80:80 nginx
```

Maps host port 80 to container port 80.

---

### Run container with name

```bash
docker run --name web nginx
```

Assigns a custom name to container.

---

### List running containers

```bash
docker ps
```

Shows running containers.

---

### List all containers (running + stopped)

```bash
docker ps -a
```

Shows all containers.

---

### Stop a container

```bash
docker stop web
```

Gracefully stops a running container.

---

### Start a stopped container

```bash
docker start web
```

Starts an existing stopped container.

---

### Restart a container

```bash
docker restart web
```

Restarts container.

---

### Remove a container

```bash
docker rm web
```

Deletes stopped container.

---

### Force remove running container

```bash
docker rm -f web
```

Stops and removes container forcefully.

---

## 4. Container Inspection & Debugging

### View container logs

```bash
docker logs web
```

Shows application logs.

---

### Follow logs (real-time)

```bash
docker logs -f web
```

Streams container logs.

---

### Execute command inside container

```bash
docker exec -it web /bin/bash
```

Access container shell.

---

### Inspect container details

```bash
docker inspect web
```

Displays detailed JSON configuration.

---

### Show container resource usage

```bash
docker stats
```

Displays CPU, memory, and network usage.

---

## 5. Docker Volume Commands

### List volumes

```bash
docker volume ls
```

Shows all volumes.

---

### Create volume

```bash
docker volume create mydata
```

Creates a named volume.

---

### Use volume in container

```bash
docker run -v mydata:/data nginx
```

Mounts volume to container.

---

### Remove volume

```bash
docker volume rm mydata
```

Deletes volume.

---

### Remove unused volumes

```bash
docker volume prune
```

Cleans unused volumes.

---

## 6. Docker Network Commands

### List networks

```bash
docker network ls
```

Shows available networks.

---

### Create a network

```bash
docker network create mynet
```

Creates custom bridge network.

---

### Run container in a network

```bash
docker run --network mynet nginx
```

Connects container to network.

---

### Inspect network

```bash
docker network inspect mynet
```

Shows network details.

---

### Remove network

```bash
docker network rm mynet
```

Deletes network.

---

## 7. Docker System Cleanup Commands

### Disk usage

```bash
docker system df
```

Shows Docker disk usage.

---

### Remove unused data (safe cleanup)

```bash
docker system prune
```

Removes stopped containers, unused networks, dangling images.

---

### Aggressive cleanup (including volumes)

```bash
docker system prune -a --volumes
```

Removes all unused Docker data.

---

## 8. Docker Registry Commands

### Login to registry

```bash
docker login
```

Authenticates to Docker Hub or private registry.

---

### Tag an image

```bash
docker tag myapp:1.0 myrepo/myapp:1.0
```

Prepares image for pushing.

---

### Push image to registry

```bash
docker push myrepo/myapp:1.0
```

Uploads image to registry.

---

### Pull image from registry

```bash
docker pull myrepo/myapp:1.0
```

Downloads image.

---

## 9. Docker Compose Commands (Basic)

### Start services

```bash
docker-compose up
```

Starts multi-container application.

---

### Start in detached mode

```bash
docker-compose up -d
```

Runs services in background.

---

### Stop services

```bash
docker-compose down
```

Stops and removes containers, networks.

---

### View compose logs

```bash
docker-compose logs
```

Displays logs for all services.

---

## 10. Interview Cheat Sheet (Must Remember)

| Task            | Command             |
| --------------- | ------------------- |
| Build image     | docker build        |
| Run container   | docker run          |
| List containers | docker ps           |
| Logs            | docker logs         |
| Enter container | docker exec         |
| Cleanup         | docker system prune |

---







