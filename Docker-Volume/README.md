# Docker Volumes – 
---
# 1. What Are Docker Volumes?

Docker **volumes** are used to store data **outside the container filesystem** so that the data persists even after the container stops or is deleted.

A container's internal filesystem is ephemeral:

* If a container crashes → data is lost
* If a container is recreated → data is lost

**Solution: Docker Volumes.**

Volumes allow:

* Persistent storage
* Sharing data between containers
* Storing database files
* Backup & migration

---

# 2. Why Volumes Are Needed

Containers are temporary. Data is not.

Without volumes:

* Logs disappear
* Database files disappear
* Uploads disappear
* Config files disappear

Production cannot work without persistent storage.

---

# 3. Docker Volume Types

Docker provides **three** main types of storage:

```
1. Volume (Named Volume) – Recommended for most cases
2. Bind Mount
3. tmpfs Mount
```

Each serves different use cases.

---

# 3.1 Named Volumes (Managed by Docker)

### Description

Volumes stored under:

```
/var/lib/docker/volumes/<volume-name>/_data
```

Managed entirely by Docker.

### How to create:

```
docker volume create mydata
```

### How to use:

```
docker run -v mydata:/data nginx
```

### Production Use Cases

* Databases (MySQL, Postgres, MongoDB)
* Container logs
* Application uploads
* Shared storage between containers

### Advantages

* Portable
* Safe
* Backups easy
* Docker handles everything

---

# 3.2 Bind Mounts (Host ↔ Container Mapping)

### Description

Maps a **host machine directory** into the container.

Example:

```
/host/data ↔ /container/data
```

### How to use:

```
docker run -v /home/ubuntu/app-data:/app/data python-app
```

### Production Use Cases

* Development environments (edit on host → reflect inside container)
* Accessing host files
* Mounting configuration directories
* Mounting SSL certificates

### Advantages

* Full control of file location
* Good for dev environments

### Disadvantages

* Not portable across different hosts
* Higher coupling between host and container

---

# 3.3 tmpfs Mounts (In-Memory Storage)

### Description

Data is stored **in RAM** only.
Not written to disk.

### How to use:

```
docker run --tmpfs /app/cache nginx
```

### Production Use Cases

* High-speed caching
* Sensitive data (removed instantly on restart)
* Session storage

### Advantages

* Fastest storage type
* Very secure (data vanishes on stop)

---

# 4. How Docker Volumes Work Internally

When a volume is mounted to a container:

* Docker isolates the volume into its own directory
* Container reads/writes to that path
* The data stays even if container is deleted
* Multiple containers can share the same volume

Example:

```
docker run -v mydata:/var/lib/mysql mysql
```

The MySQL container writes DB files into `mydata` volume.

If container dies → data stays.
If new container starts → it reuses the same data.

---

# 5. Real Production Use Cases

## 5.1 Databases

Volumes are essential for DB containers:

* MySQL → `/var/lib/mysql`
* Postgres → `/var/lib/postgresql/data`
* MongoDB → `/data/db`

Example:

```
docker run -v dbdata:/var/lib/mysql mysql:8
```

---

## 5.2 Application Uploads

User uploads should not disappear when the app restarts.

Example:

```
docker run -v uploads:/app/uploads myapp
```

---

## 5.3 Shared Storage Between Containers

Example:
Nginx + PHP-FPM sharing code files

```
docker run -v app:/var/www/html nginx
```

```
docker run -v app:/var/www/html php-fpm
```

---

## 5.4 Backups and Restore

Volumes make it easy:

```
docker run --rm -v dbdata:/data -v $(pwd):/backup ubuntu tar cvf /backup/db.tar /data
```

---

## 5.5 Logs and Monitoring

Store logs persistently:

```
docker run -v logdata:/var/log/nginx nginx
```

---

## 5.6 Configurations

Mount host configs:

```
docker run -v /etc/nginx/nginx.conf:/etc/nginx/nginx.conf nginx
```

---

# 6. Production Best Practices

### ✅ 1. Always Use Named Volumes for Databases

Never store DB data inside containers.

---

### ✅ 2. Use Bind Mounts for Local Development Only

It tightly couples host & container.

---

### ✅ 3. Keep Sensitive Data in tmpfs

Example:

* API tokens
* Private keys
* Secrets

---

### ✅ 4. Use Volume Drivers for Cloud Storage

In AWS ECS/EKS:

* EBS volumes
* EFS for shared storage

---

### ✅ 5. Backup Volumes Regularly

Important for DB disaster recovery.

---

### ✅ 6. Use Least Privilege Access

Limit which containers can mount which volumes.

---

### ✅ 7. Monitor Volume Size

Containers can fill the disk → production outage.

---

# 7. Commands You MUST Know

### Create a volume

```
docker volume create mydata
```

### List volumes

```
docker volume ls
```

### Inspect volume

```
docker volume inspect mydata
```

### Remove volume

```
docker volume rm mydata
```

### Remove unused volumes

```
docker volume prune
```

---

# 8. Interview One-Liners

✔ "Volumes persist data even if containers are deleted."
✔ "Named volumes are Docker-managed and recommended for production."
✔ "Bind mounts map host directories to containers — best for development."
✔ "tmpfs stores data in RAM — useful for sensitive or fast temporary data."
✔ "Databases must always use volumes for durability."

---
