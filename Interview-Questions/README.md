# Docker Interview Questions 
Suitable for DevOps roles (1–3 years experience).
---

# 1. Basic Docker Questions

### **1. What is Docker?**

Docker is a containerization platform that lets you package applications with their dependencies into containers.

### **2. What is a Docker container?**

A lightweight, isolated runtime environment with its own filesystem, network, and process space.

### **3. What is a Docker image?**

A read-only template used to create containers.

### **4. How is a container different from a VM?**

Containers share the host OS kernel; VMs have full OS → containers are faster and lightweight.

### **5. What is Dockerfile?**

A text file with instructions to build a Docker image.

### **6. What is the use of `EXPOSE` in Dockerfile?**

Documents which port the container listens on.

### **7. What is a Docker registry?**

A repository where Docker images are stored (Example: Docker Hub, ECR).

### **8. What is the difference between `CMD` and `ENTRYPOINT`?**

* CMD → default command (can be overridden)
* ENTRYPOINT → main command (harder to override)

### **9. What is a Docker volume?**

Persistent storage mechanism for containers.

### **10. What is the default Docker network?**

`bridge` network.

---

# 2. Intermediate Questions

### **11. What is Docker Compose?**

A tool to run multi-container applications using a YAML file.

### **12. What is the use of `docker exec`?**

Used to run commands inside a running container.

### **13. Explain Docker image layers.**

Each Dockerfile instruction creates a read‑only layer; layers are cached and shared.

### **14. What is the Docker daemon?**

A background service (`dockerd`) that manages containers/images.

### **15. What is BuildKit?**

A modern image-building engine that improves build performance and caching.

### **16. Explain multi-stage builds.**

Multiple `FROM` stages used to build and copy only the final output → reduces image size.

### **17. What is `.dockerignore` used for?**

To exclude unnecessary files from the build context.

### **18. Difference between `docker stop` and `docker kill`?**

* `stop` → graceful shutdown
* `kill` → immediate termination

### **19. What happens when a container exits?**

It transitions to *stopped* state; data in writable layer is lost.

### **20. What is a dangling image?**

Images without tags, usually leftover from builds.

---

# 3. Advanced Questions

### **21. What are distroless images?**

Images without shell or OS packages → more secure & lightweight.

### **22. What is namespace isolation?**

Ensures containers have isolated processes, networks, mounts, etc.

### **23. What are cgroups?**

Linux kernel feature that limits CPU/RAM resource usage.

### **24. What is overlay2 storage driver?**

Default driver that merges layers efficiently.

### **25. What is container orchestration?**

Automated management of containers using tools like Kubernetes, ECS.

### **26. Explain `docker inspect`.**

Shows low-level container details like environment variables, mounts, IP, etc.

### **27. What is a Docker secret?**

Secure storage for passwords/keys (mainly in Swarm/K8s).

### **28. Explain Docker healthchecks.**

Used to monitor container health and restart unhealthy containers.

### **29. What is image tagging?**

Marking images with version identifiers.

### **30. Can we run multiple containers using the same image?**

Yes, containers are instances of images.

---

# 4. Scenario-Based Questions

### **31. Container restarts again and again — what will you check?**

* Application logs
* Healthcheck failures
* Wrong entrypoint or missing dependencies
* Port already in use

---

### **32. Your container is running but app not accessible — how do you debug?**

* Check port mapping: `docker ps`
* Check firewall/security groups
* Test inside container: `curl localhost:<port>`
* Check logs

---

### **33. You updated code but container still runs old version — why?**

* Image caching
* Didn’t rebuild image
* Mounted volumes overriding app files

---

### **34. App works locally but not in EC2 — what to check?**

* SG inbound rules
* Public IP/port mapping
* Environment variables
* Binding to `0.0.0.0` instead of `localhost`

---

### **35. Your image size is huge — how do you reduce it?**

* Use multistage builds
* Use alpine/distroless
* Clean package managers
* Use `.dockerignore`

---

### **36. Your DB container loses data after restart — why?**

* No volume configured
* Data stored in container writable layer

---

### **37. You have two containers: backend and DB — how will backend connect?**

Use Docker DNS:

```
database:5432
```

Within same user-defined network.

---

### **38. App running in container cannot write files — why?**

* File permissions
* Running as non‑root user
* Readonly filesystem

---

### **39. Production container crashes due to OOM — how to prevent?**

Set memory limits:

```
docker run -m 512m app
```

---

### **40. You need to roll back to previous version — how?**

Pull previous tag:

```
docker run myapp:1.0
```

Or update deployment to older tag.

---

# 5. More Real Production Scenarios

### **41. You need zero-downtime deployment — what to use?**

* Run two containers
* Use load balancer
* Update containers one by one

---

### **42. Logs not visible with `docker logs` — why?**

App writes logs to file instead of STDOUT.

---

### **43. You want to access container files — how?**

```
docker exec -it container bash
```

---

### **44. Your image build is slow — how to speed it up?**

* Reorder Dockerfile instructions
* Use BuildKit
* Use layer caching

---

### **45. How do you secure Docker in production?**

* Run as non-root
* Use distroless images
* Scan images
* Restrict network access
* Use secrets

---

### **46. How to share data between two containers?**

Use a **volume**:

```
docker run -v shared:/data container1
```

```
docker run -v shared:/data container2
```

---

### **47. How to connect container to a specific network?**

```
docker run --network app-net myapp
```

---

### **48. Which command shows container resource usage?**

```
docker stats
```

---

### **49. Docker container not binding to port — fix?**

Ensure app runs on:

```
0.0.0.0
```

Not `localhost`.

---

### **50. What if two containers need same port?**

Use different host ports:

```
-p 8080:80
-p 8081:80
```

---

### **51. Your team uses Docker Hub — how do you avoid public access?**

Make repository **private** or use **ECR/ACR/GCR**.

---

### **52. Need to run a container automatically at reboot — how?**

Use restart policy:

```
restart: always
```




