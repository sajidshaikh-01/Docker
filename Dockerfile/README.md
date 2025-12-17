# Dockerfile – 
---

## 1. What is a Dockerfile?

A **Dockerfile** is a plain text file that contains a set of instructions used by Docker to **build a Docker image**.

In simple words:

> A Dockerfile tells Docker **which OS/runtime to use, where the application code is, how to install dependencies, and how to start the application**.

---

## 2. Why Dockerfile is Needed

Without a Dockerfile:

* Docker does not know how to run your application
* Docker does not know which runtime or dependencies are required

With a Dockerfile:

* Application becomes portable
* Same image runs in Dev, QA, and Production
* CI/CD automation becomes possible

---

## 3. Dockerfile Build Flow (Important)

```
Application Code
      ↓
Dockerfile (instructions)
      ↓
Docker Image
      ↓
Docker Container (running application)
```

---

## 4. Dockerfile Instructions (All Components)

Below are **all commonly used Dockerfile instructions**, explained one by one.

---

## 4.1 FROM (Mandatory)

### Purpose

* Defines the **base image** for your application

### Example

```dockerfile
FROM python:3.11-slim
```

### Explanation

* This image already contains:

  * Linux OS
  * Python runtime

### Key Points

* Usually the **first instruction**
* Can be used multiple times (multi-stage builds)

---

## 4.2 WORKDIR (Recommended)

### Purpose

* Sets the working directory inside the container

### Example

```dockerfile
WORKDIR /app
```

### Explanation

* All subsequent commands run inside `/app`
* Avoids long and messy paths

---

## 4.3 COPY

### Purpose

* Copies files from local system to the Docker image

### Example

```dockerfile
COPY . .
```

### Explanation

* Copies application code into the image
* Happens at **build time**, not runtime

---

## 4.4 ADD (Less Preferred)

### Purpose

* Similar to COPY but with extra features

### Example

```dockerfile
ADD app.tar.gz /app/
```

### Extra Capabilities

* Automatically extracts `.tar` files
* Can download files from URLs

### Best Practice

* Prefer **COPY** unless ADD features are required

---

## 4.5 RUN

### Purpose

* Executes commands during image build

### Example

```dockerfile
RUN pip install -r requirements.txt
```

### Explanation

* Used to install dependencies
* Creates a new image layer
* Runs only during **docker build**

---

## 4.6 CMD (Very Important)

### Purpose

* Defines the default command to start the container

### Example

```dockerfile
CMD ["python", "app.py"]
```

### Explanation

* Executed when container starts
* Can be overridden at runtime

### Important Rule

* Only one CMD is effective in a Dockerfile

---

## 4.7 ENTRYPOINT

### Purpose

* Defines a fixed command that always runs

### Example

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

### CMD vs ENTRYPOINT

| CMD              | ENTRYPOINT       |
| ---------------- | ---------------- |
| Default command  | Fixed command    |
| Easy to override | Hard to override |

### Production Usage

* ENTRYPOINT → executable
* CMD → arguments

---

## 4.8 EXPOSE

### Purpose

* Documents which port the application listens on

### Example

```dockerfile
EXPOSE 8080
```

### Important Note

* EXPOSE does **not** open the port
* Port mapping happens at runtime using `-p`

---

## 4.9 ENV

### Purpose

* Sets environment variables inside the container

### Example

```dockerfile
ENV APP_ENV=production
ENV PORT=8080
```

### Use Cases

* Application configuration
* Runtime behavior control

---

## 4.10 ARG (Build-Time Variable)

### Purpose

* Defines variables available during image build

### Example

```dockerfile
ARG VERSION=1.0
```

### Explanation

* Used only at build time
* Not available inside running container

---

## 4.11 LABEL

### Purpose

* Adds metadata to the image

### Example

```dockerfile
LABEL maintainer="devops@example.com"
```

### Use Cases

* Ownership
* Versioning
* Documentation

---

## 4.12 USER

### Purpose

* Defines which user runs the application

### Example

```dockerfile
USER appuser
```

### Why Important

* Improves security
* Avoids running apps as root

---

## 4.13 VOLUME

### Purpose

* Creates a mount point for persistent data

### Example

```dockerfile
VOLUME ["/data"]
```

### Use Cases

* Databases
* Logs
* Persistent storage

---

## 4.14 HEALTHCHECK

### Purpose

* Checks container health

### Example

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:8080 || exit 1
```

### Explanation

* Helps orchestration tools detect unhealthy containers

---

## 5. Simple Example Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 8080
CMD ["python", "app.py"]
```

---

## 6. Interview One-Line Summary

**A Dockerfile is a set of instructions that defines how an application and its dependencies are packaged into a Docker image and how the container is started.**




