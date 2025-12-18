# Docker Networking – 
---
# 1. What is Docker Networking?

Docker networking allows containers to communicate:

* with each other,
* with the host machine,
* and with the outside world.

Every container gets:

* its own internal IP,
* its own network namespace,
* virtual Ethernet interfaces.

Docker uses **Linux networking features** like:

* network namespaces,
* veth pairs,
* bridges,
* iptables,
* routing tables.

---

# 2. How Docker Networking Works (Simple Explanation)

When you start a container:

1. Docker creates a **veth pair** (virtual cables)
2. One end stays inside the container
3. The other end attaches to a Docker network (like a virtual switch)
4. Docker assigns an internal IP (e.g., 172.17.0.2)
5. Docker configures NAT so containers can access the internet

Result:

* Containers can talk to each other
* Containers can talk to the internet
* Internet can reach containers only when ports are published

---

# 3. Docker Network Types

Docker supports **five types** of networks:

```
bridge
host
none
macvlan
overlay
```

Each is used for different purposes.

---

# 3.1 Bridge Network (Default & Most Common)

### Description

This is the **default** Docker network.

Each container receives an internal IP like:

```
172.17.0.X
```

### Use Cases

* Local development
* Small production setups
* Apps requiring container-to-container communication

### Example

```
docker run --network bridge nginx
```

### Key Features

* Containers communicate using container names
* Port mapping (`-p 80:80`) required for external access

---

# 3.2 Host Network

### Description

Container **shares the host's network namespace**.

No internal IP is created.
The container uses the host IP directly.

### Example

```
docker run --network host nginx
```

### Use Cases

* High‑performance applications
* Network monitoring tools
* DNS or DHCP services

### Downside

* No port isolation
* Not secure for multi-tenant environments

---

# 3.3 None Network

### Description

Container receives **no network**.
No IP, no gateway.

### Example

```
docker run --network none nginx
```

### Use Cases

* Security-restricted workloads
* Batch jobs with no network access

---

# 3.4 Macvlan Network

### Description

Containers are assigned **real MAC addresses** and appear as **physical devices** on the LAN.

### Example Use Case

* Running legacy applications that expect direct Layer‑2 network access
* When container needs its own IP on corporate network

### Example

```
docker network create -d macvlan my-macvlan
```

---

# 3.5 Overlay Network (Used in Kubernetes & Swarm)

### Description

Used for **multi-host networking**.

Containers running on different hosts
communicate securely as if on the same LAN.

### How it works

* Uses VXLAN tunneling
* Encrypted traffic between nodes

### Use Cases

* Docker Swarm
* Kubernetes (CNI plugins)
* ECS (bridge + overlay combinations)

---

# 4. Container-to-Container Communication

### Inside the same Docker network

Containers communicate by **name**:

```
http://backend:5000
```

Docker DNS resolves names automatically.

---

# 5. Port Publishing (Host ↔ Container)

```
docker run -p 80:80 nginx
```

Meaning:

* Host port 80 → Container port 80
* Users can access via host public IP

---

# 6. Production Networking Best Practices

### ✅ 1. Use User-Defined Bridge Networks

Don’t run apps on the default `bridge` network.

Create your own network:

```
docker network create app-net
```

### Advantages

* Automatic DNS
* Cleaner isolation
* More control

---

### ✅ 2. Always Use Container Names for Communication

Example:

```
frontend → http://backend:5000
```

Not by IP.

IP changes every restart.

---

### ✅ 3. Expose Only Necessary Ports

Never expose your entire container.

Use:

```
-p 80:80
```

Avoid:

```
--network host
```

Unless needed for performance.

---

### ✅ 4. Use Firewalls + Security Groups

For EC2:

* Open ports only for required services
* Block everything else

Example security group:

* 80 open → frontend
* 5000 only for internal or admin use

---

### ✅ 5. Use Reverse Proxy in Production

Never expose backend directly.
Use:

* Nginx
* HAProxy
* Traefik

Example:

```
client → nginx (port 80) → backend (port 5000)
```

---

### ✅ 6. Use Overlay Networks for Multi-Node Clusters

In Kubernetes or Swarm:

* Use CNI (Calico, Flannel, Weave)
* Use service discovery

Containers across servers communicate securely.

---

### ✅ 7. Limit Container-to-Container Communication

Use:

* Docker network policies
* Kubernetes network policies

To prevent lateral movement in case of attacks.

---

### ✅ 8. Use Dedicated Networks for Databases

Example:

```
docker network create db-net
```

Attach only backend + database.

Avoid exposing DB to internet.

---

### ✅ 9. Avoid Host Networking in Production

Host networking gives:

* No port isolation
* No NAT
* Security risks

Use only when **absolutely required**.

---

### ✅ 10. Use Load Balancers for Scale

In AWS:

* ALB → route to containers
* NLB → fast TCP routing

---

# 7. Helpful Commands

### List networks

```
docker network ls
```

### Inspect a network

```
docker network inspect app-net
```

### Create user-defined network

```
docker network create app-net
```

### Run container inside network

```
docker run --network app-net backend
```

---

# 8. Interview One-Liners

### ✔ "Docker networking isolates containers using network namespaces and virtual Ethernet pairs."

### ✔ "User-defined bridge networks provide automatic DNS for container name resolution."

### ✔ "Overlay networks allow multi-host container communication in Kubernetes and Swarm."

### ✔ "Host networking removes network isolation and should be avoided unless performance is critical."

---



