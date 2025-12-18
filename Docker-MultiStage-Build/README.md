# Docker Multistage Builds & Distroless Images – 

---

# 1. What is a Multistage Docker Build?

A **multistage build** is a feature in Docker that allows you to use **multiple FROM statements** in one Dockerfile.

Each stage can use a different base image.

The idea is:

> Use heavy images for building the app, and lightweight images for running the app.

This helps reduce image size and improve security.

---

# 2. Why Multistage Builds Are Needed

In normal builds:

* You use a large base image (example: golang, node, python)
* The final image contains:

  * compilers
  * build tools
  * package managers
  * source code
  * caches
  * temp files

❌ This makes the image huge
❌ Slower deployments
❌ Bigger attack surface

**Solution → Multistage builds**

---

# 3. How Multistage Builds Work (Simple Explanation)

A multistage Dockerfile has multiple stages:

```
FROM golang:1.21 AS builder
# compile the app

FROM ubuntu:22.04 AS runner
# copy only final binary
```

Stage 1: Build your application
Stage 2: Run your application

You copy only the **final output** (binary/artifact) from stage 1 into stage 2.

The heavy build tools **do NOT** go into the final image.

---

# 4. How Multistage Works in Production

### Production Workflow:

1. CI/CD builds the multistage Dockerfile
2. Stage 1 compiles your app using full SDK/runtime
3. Stage 2 contains only the minimal required files
4. CI pushes the final lightweight image to registry (ECR, Docker Hub)
5. Production servers pull the **small final image**

### Why production prefers multistage:

* Smaller image → Faster deployments
* Smaller image → Less network bandwidth
* Smaller image → Faster scaling in Kubernetes
* Fewer vulnerabilities → Better security audits
* Only production dependencies exist in the final image

---

# 5. Advantages of Multistage Builds

### ✅ Smaller Images

Because unnecessary build tools are excluded.

### ✅ More Secure

Attack surface is reduced dramatically.

### ✅ Cleaner Dockerfiles

Build logic + run logic separated.

### ✅ Faster Deployments

Small images → faster pulls → faster autoscaling.

### ✅ Ideal for Go, Java, Node, Python, React, Angular

---

# 6. What Are Distroless Images?

A **distroless image** is a Docker base image that contains:

* ONLY your application
* ONLY required runtime libraries

It does **NOT** contain:

* No shell (`/bin/sh`)
* No package manager
* No OS tools
* No bash
* No debugging commands

Example distroless base image:

```
FROM gcr.io/distroless/python3
```

Distroless images are produced by Google.

---

# 7. How Distroless Images Work (Internally)

A normal Linux image contains:

* OS packages
* Shell
* Tools like curl, wget, ping
* Compilers
* Debuggers

A distroless image contains ONLY:

* Application
* Minimal required runtime

Nothing else.

This makes them:

* Extremely small
* Extremely secure
* Perfect for production

---

# 8. Advantages of Distroless Images

### 🔐 1. **Very Secure**

Attackers cannot:

* Run bash
* Install tools
* Explore filesystem
* Execute shell commands

### 📦 2. **Very Small Size**

Faster deployments.

### 🚀 3. **Faster Startup**

Great for Kubernetes autoscaling.

### 🛡 4. **Reduced CVEs**

Less OS → fewer vulnerabilities.

---

# 9. Multistage Build Example (Go Application)

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

# Stage 2: Run final minimal image
FROM ubuntu:22.04
WORKDIR /app
COPY --from=builder /app/app .
CMD ["./app"]
```

Final image has:

* NO Go compiler
* Only the final binary

---

# 10. Multistage + Distroless Example (Go Production)

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

# Stage 2: Distroless
FROM gcr.io/distroless/base
COPY --from=builder /app/app /
CMD ["/app"]
```

Final image size: **~8MB**

---

# 11. Multistage Build Example (Python Application)

```dockerfile
# Stage 1: Builder
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
COPY . .

# Stage 2: Runner
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /install /usr/local
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

# 12. Multistage + Distroless Example (Python)

```dockerfile
# Stage 1: Build
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
COPY . .

# Stage 2: Distroless
FROM gcr.io/distroless/python3
COPY --from=builder /install /usr/local
COPY app.py /app/app.py
WORKDIR /app
CMD ["app.py"]
```

---

# 13. When to Use Multistage vs Distroless

| Feature                  | Multistage  | Distroless          |
| ------------------------ | ----------- | ------------------- |
| Reduce image size        | ✅           | ✅ (more)            |
| Improve security         | ⚠️ moderate | 🔐 extremely secure |
| Debuggable               | Yes         | No shell available  |
| Production best practice | Yes         | Yes (advanced)      |
| Easy for beginners       | Yes         | No                  |

---

# 14. Key Takeaways

* **Multistage builds** help reduce image size by separating build and run stages.
* **Distroless images** provide ultra-minimal, secure runtime environments.
* Combined, they give:

  * very small images
  * high security
  * fast deployment
  * low CVEs

---

---

