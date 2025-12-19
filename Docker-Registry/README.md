# Docker Registry – 
---
# 1. What Is a Docker Registry?

A **Docker Registry** is a storage and distribution system for Docker **images**.

Think of it like **GitHub but for Docker images**.

A registry allows you to:

* Store Docker images
* Version Docker images
* Push (upload) images
* Pull (download) images
* Share images between teams and environments

You build images locally → push to registry → pull in production.

---

# 2. Why Docker Registry Is Needed

Without a registry:

* You cannot move images between environments (dev → prod)
* No central storage for images
* Deployment becomes manual
* CI/CD pipelines cannot work

With a registry:

* CI can push images automatically
* Production servers can pull latest image
* Teams can share containers easily
* Versioning becomes simple

---

# 3. Docker Registry Components

A registry consists of two parts:

### 1. **Registry** (Stores images)

Stores all image layers.

### 2. **Repository** (Collection of image versions)

Example:

```
myapp:1.0
myapp:1.1
myapp:latest
```

---

# 4. Types of Docker Registries

There are two main types:

## 4.1 Public Registry (Free / Open)

### Examples:

* **Docker Hub** (default)
* GitHub Container Registry (GHCR)
* GitLab Container Registry

### Use Cases:

* Open-source images
* Public sharing
* Learning and testing

---

## 4.2 Private Registry (Secure / Enterprise)

Used inside organizations.

### Examples:

* **AWS ECR** (Amazon Elastic Container Registry)
* Google GCR
* Azure ACR
* Harbor
* JFrog Artifactory

### Use Cases:

* Production images
* Sensitive applications
* Internal microservices
* Secure access control

---

# 5. How Docker Registry Works (Simple Flow)

### Step 1: Build an image

```
docker build -t myapp:1.0 .
```

### Step 2: Login to registry

```
docker login
```

### Step 3: Tag the image

```
docker tag myapp:1.0 myrepo/myapp:1.0
```

### Step 4: Push image to registry

```
docker push myrepo/myapp:1.0
```

### Step 5: Pull image from registry

```
docker pull myrepo/myapp:1.0
```

---

# 6. How Registries Are Used in Production

In a typical pipeline:

## 6.1 CI/CD Pipeline Pushes Images

1. Developer pushes code to GitHub
2. CI pipeline runs (GitHub Actions/Jenkins/GitLab CI)
3. CI builds a new Docker image
4. CI tags the image with version
5. CI pushes the image to private registry (ECR/GCR/ACR)

---

## 6.2 Production Servers Pull Images

Production environment (EC2, ECS, EKS, Kubernetes) pulls from registry:

Example in Kubernetes:

```
image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:1.0
```

### Benefits:

* Fast deployments
* Rollbacks available (use previous tag)
* Immutable builds

---

# 7. Registry vs Repository (Important Difference)

### Registry

* Full storage system
* Example: Docker Hub, ECR

### Repository

* Specific project inside registry
* Example:

```
myapp
myapp:1.0
myapp:2.0
```

---

# 8. Tags in Docker Registry

Tags help version images.

Examples:

```
myapp:1.0
myapp:2.0
myapp:latest
myapp:prod
myapp:test
```

---

# 9. Production Use Cases for Docker Registries

### 🟩 1. Central Image Storage

All microservices store images in one place.

### 🟩 2. CI/CD Deployments

CI builds → pushes to registry → production pulls.

### 🟩 3. Version Control for Applications

Easily roll back to:

```
myapp:1.0
```

### 🟩 4. Security & Access Control

Private registries restrict access with:

* IAM roles (ECR)
* Permissions
* Scanning for vulnerabilities

### 🟩 5. Multi-environment Deployment

Use different tags for:

* dev
* staging
* production

Example:

```
myapp:dev
myapp:stg
myapp:prod
```

### 🟩 6. Auto-scaling in Kubernetes

Kubernetes nodes pull latest images when scaling up pods.

---

# 10. Docker Registry Commands You Must Know

### Login

```
docker login
```

### Tag image

```
docker tag myapp:1.0 myrepo/myapp:1.0
```

### Push image

```
docker push myrepo/myapp:1.0
```

### Pull image

```
docker pull myrepo/myapp:1.0
```

### List images

```
docker images
```

---

# 11. Interview One-Liners

✔ “A Docker registry stores and distributes container images.”
✔ “Docker Hub is the default public registry; ECR/GCR/ACR are private registries.”
✔ “Registries enable CI/CD by storing versioned images for deployment.”
✔ “A repository is a collection of image versions inside a registry.”
✔ “Production always uses a private registry for security.”

---



