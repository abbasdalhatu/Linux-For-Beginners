
# MODULE 17 – ADVANCED LINUX
## LESSON 12: CONTAINERS AND VIRTUALIZATION

---

## 1. Introduction

Containers and virtualization are technologies that allow multiple isolated computing environments to operate on the same physical machine.

They are widely used in:

- Cloud computing.
- Software development.
- Cybersecurity laboratories.
- Application deployment.
- Server administration.
- DevOps engineering.
- Artificial intelligence infrastructure.

### Learning objectives

By the end of this lesson, you should be able to:

1. Explain virtualization.
2. Understand virtual machines.
3. Explain containers.
4. Differentiate containers from virtual machines.
5. Understand Docker architecture.
6. Inspect container images.
7. Run basic Docker containers.
8. Understand container networking.
9. Explain container security.
10. Understand Kubernetes fundamentals.

---

## 2. Understanding Virtualization

Virtualization is the process of creating virtual versions of computing resources.

A virtual machine behaves like an independent computer but shares the physical hardware with other virtual machines.

For example, one physical laptop can run:

- Windows 11 as the host operating system.
- Kali Linux as a virtual machine.
- Ubuntu as another virtual machine.
- Metasploitable as an isolated vulnerable virtual machine.

### Important virtualization terms

| Term | Meaning |
|---|---|
| Host | Physical computer running virtualization software |
| Guest | Operating system running inside a virtual machine |
| Hypervisor | Software that creates and manages virtual machines |
| Virtual CPU | CPU resources assigned to a VM |
| Virtual RAM | Memory allocated to a VM |
| Virtual disk | Virtual storage assigned to a VM |
| Snapshot | Saved state of a virtual machine |

### Types of hypervisors

**Type 1: Bare-metal hypervisor**

Runs directly on physical hardware.

Examples:

- VMware ESXi.
- Microsoft Hyper-V.
- Xen.

**Type 2: Hosted hypervisor**

Runs as an application on an existing operating system.

Examples:

- VMware Workstation.
- Oracle VirtualBox.
- Parallels Desktop.

### Practical example

Your Windows computer can run Kali Linux through VMware Workstation without replacing Windows.

---

## 3. Understanding Virtual Machines

A virtual machine provides an isolated computing environment with its own guest operating system.

### Virtual machine architecture

```text
PHYSICAL HARDWARE
       |
       v
HOST OPERATING SYSTEM
       |
       v
HYPERVISOR
       |
       +----------------+
       |                |
       v                v
   KALI LINUX       UBUNTU
   Virtual Machine  Virtual Machine
```

### Advantages

- Operating-system isolation.
- Multiple operating systems on one computer.
- Snapshots and rollback.
- Safe testing environments when properly configured.
- Reproducible laboratory environments.

### Disadvantages

- Higher resource consumption.
- More storage requirements.
- Guest operating systems require maintenance.
- Performance overhead compared with many container workloads.

### Inspect virtualization information

```bash
lscpu | grep -i virtualization
```

Another command:

```bash
systemd-detect-virt
```

This may identify whether Linux is running inside a virtual machine or container.

---

## 4. Understanding Containers

A container is an isolated environment that packages an application and its required dependencies.

Unlike a traditional virtual machine, a container normally shares the host operating system's kernel.

Containers use Linux kernel features such as:

- Namespaces.
- Control groups (cgroups).
- Capabilities.
- Seccomp.
- Linux security modules.

### Container architecture

```text
PHYSICAL HARDWARE
       |
       v
HOST OPERATING SYSTEM
       |
       v
LINUX KERNEL
       |
       +----------------------+
       |          |           |
       v          v           v
   CONTAINER  CONTAINER   CONTAINER
   App A      App B       App C
```

### Advantages

- Lightweight compared with full virtual machines.
- Fast application startup.
- Consistent application environments.
- Easier deployment.
- Efficient resource utilization.

### Disadvantages

- Shared kernel increases the importance of isolation.
- Incorrect configuration can expose host resources.
- Images may contain vulnerable software.
- Containers require security monitoring.

---

## 5. Virtual Machines vs Containers

| Feature | Virtual Machine | Container |
|---|---|---|
| Operating system | Has its own guest OS | Shares host kernel |
| Startup | Generally slower | Generally faster |
| Resource usage | Generally higher | Generally lower |
| Isolation | Virtual hardware boundary | Kernel-based isolation |
| Portability | Virtual machine images | Container images |
| Typical use | Multiple OS environments | Application deployment |

### Real-world example

A cybersecurity laboratory may use:

- VMware to run Kali Linux.
- VMware to run Metasploitable.
- Docker to run a web application or database service.

The VM provides an operating-system environment, while Docker packages individual applications.

---

## 6. Introduction to Docker

Docker is a platform for building, distributing, and running containers.

Important Docker components include:

### A. Docker Engine

The platform responsible for creating and managing containers.

### B. Docker Image

A read-only template used to create containers.

### C. Docker Container

A running or stopped instance created from an image.

### D. Dockerfile

A text file containing instructions for building an image.

### E. Docker Registry

A location where container images are stored.

Docker Hub is a popular public container registry.

### Docker architecture

```text
USER
 |
 v
DOCKER CLIENT
 |
 v
DOCKER API
 |
 v
DOCKER ENGINE
 |
 +-------------------+
 |                   |
 v                   v
IMAGES            CONTAINERS
 |
 v
REGISTRY
```

---

## 7. Understanding Docker Images

A Docker image contains the filesystem and configuration required to create a container.

Images are usually built in layers.

### Important image commands

Display Docker version:

```bash
docker --version
```

Display locally available images:

```bash
docker images
```

Search Docker Hub:

```bash
docker search nginx
```

Pull an image:

```bash
docker pull nginx:alpine
```

Inspect an image:

```bash
docker image inspect nginx:alpine
```

Display image history:

```bash
docker history nginx:alpine
```

### Important image concepts

| Concept | Meaning |
|---|---|
| Repository | Image collection or name |
| Tag | Identifies a version or variant |
| Layer | Filesystem change within an image |
| Registry | Image storage service |
| Digest | Content-based image identifier |

### Security recommendation

Prefer trusted image publishers, review image metadata, and use specific image versions or digests when reproducibility matters.

---

## 8. Understanding Docker Containers

A container is created from an image.

### Display running containers

```bash
docker ps
```

### Display all containers

```bash
docker ps -a
```

### Create and run a container

```bash
docker run -d --name web-lab nginx:alpine
```

Explanation:

- `docker run` creates and starts a container.
- `-d` runs it in the background.
- `--name` assigns a name.
- `nginx:alpine` identifies the image.

### Inspect the container

```bash
docker inspect web-lab
```

### Display container logs

```bash
docker logs web-lab
```

### Stop the container

```bash
docker stop web-lab
```

### Start the container again

```bash
docker start web-lab
```

### Remove the container

```bash
docker rm web-lab
```

Only remove containers created for your own practice.

---

## 9. Docker Networking

Containers can communicate through virtual networks.

Common Docker network types include:

| Network | Purpose |
|---|---|
| Bridge | Connects containers through a virtual bridge |
| Host | Uses the host network namespace |
| None | Disables normal container networking |
| Overlay | Connects container networks across multiple hosts |

### Display Docker networks

```bash
docker network ls
```

### Inspect the default bridge network

```bash
docker network inspect bridge
```

### Create a practice network

```bash
docker network create lesson12-net
```

### Inspect it

```bash
docker network inspect lesson12-net
```

### Remove it

```bash
docker network rm lesson12-net
```

### Security considerations

- Avoid unnecessary exposed ports.
- Restrict network communication.
- Do not expose administrative interfaces publicly.
- Use isolated networks for sensitive workloads.
- Monitor unexpected container connections.

---

## 10. Docker Volumes and Storage

Containers can use persistent storage.

Without appropriate persistent storage, data written inside a container's writable layer may be lost when the container is removed.

### Display volumes

```bash
docker volume ls
```

### Create a volume

```bash
docker volume create lesson12-data
```

### Inspect the volume

```bash
docker volume inspect lesson12-data
```

### Remove the volume

```bash
docker volume rm lesson12-data
```

Do not mount sensitive host directories into containers unless necessary and understood.

---

## 11. Understanding Dockerfiles

A Dockerfile defines how an image is built.

Example:

```dockerfile
FROM nginx:alpine

LABEL purpose="Linux container learning"

EXPOSE 80
```

Explanation:

| Instruction | Purpose |
|---|---|
| FROM | Selects a base image |
| LABEL | Adds image metadata |
| EXPOSE | Documents the intended container port |

`EXPOSE` does not automatically publish a port to the host.

### Build an image

Create a project directory:

```bash
mkdir -p ~/module17-lab/lesson12/docker-project
cd ~/module17-lab/lesson12/docker-project
```

Create the Dockerfile:

```bash
nano Dockerfile
```

Enter the example above.

Build the image:

```bash
docker build -t lesson12-web .
```

Inspect it:

```bash
docker images
```

The image is created locally using the selected base image.

---

## 12. Container Resource Management

Containers can be configured with resource limits.

This helps prevent one application from consuming excessive resources.

### Inspect container resource usage

```bash
docker stats
```

Press `Ctrl+C` to exit.

### Display container processes

```bash
docker top web-lab
```

### Display container details

```bash
docker inspect web-lab
```

### Resource limits

Docker supports limits for resources such as:

- CPU.
- Memory.
- Process count.

Example:

```bash
docker run -d \
  --name limited-web \
  --memory=256m \
  --cpus=0.5 \
  nginx:alpine
```

This limits the container to approximately 256 MiB of memory and half a CPU's processing capacity.

Remove the demonstration container when finished:

```bash
docker stop limited-web
docker rm limited-web
```

---

## 13. Container Security

Container security is essential because containers can access host resources when improperly configured.

### Common container security risks

| Risk | Possible consequence |
|---|---|
| Running as root | Increased impact of compromise |
| Privileged containers | Excessive host-level access |
| Vulnerable images | Exposure to known vulnerabilities |
| Exposed ports | Unauthorized network access |
| Sensitive mounted volumes | Host data exposure |
| Weak secrets management | Credential compromise |
| Unrestricted capabilities | Excessive privileges |

### Important security practices

1. Use trusted images.
2. Keep images updated.
3. Avoid privileged containers.
4. Run applications as non-root where possible.
5. Limit Linux capabilities.
6. Use read-only filesystems where practical.
7. Apply CPU and memory limits.
8. Restrict container networking.
9. Protect secrets.
10. Monitor container logs.

### Inspect a container's security configuration

```bash
docker inspect web-lab
```

Review fields such as:

- `User`
- `Privileged`
- `ReadonlyRootfs`
- `CapAdd`
- `CapDrop`
- `Mounts`
- `NetworkSettings`

A secure container configuration depends on the application and its requirements.

---

## 14. Introduction to Kubernetes

Kubernetes is an open-source platform for managing containerized applications.

It helps organizations deploy and manage containers across multiple machines.

### Important Kubernetes components

| Component | Purpose |
|---|---|
| Cluster | Collection of machines managed by Kubernetes |
| Node | Machine participating in a cluster |
| Pod | Smallest deployable Kubernetes workload unit |
| Deployment | Manages application replicas and updates |
| Service | Provides stable network access to workloads |
| ConfigMap | Stores non-secret configuration |
| Secret | Stores sensitive configuration data |
| Namespace | Organizes cluster resources |

### Simplified Kubernetes architecture

```text
KUBERNETES CLUSTER
        |
        v
   CONTROL PLANE
        |
        v
      NODES
        |
        v
       PODS
        |
        v
    CONTAINERS
```

### Common Kubernetes commands

Display cluster information:

```bash
kubectl cluster-info
```

Display nodes:

```bash
kubectl get nodes
```

Display pods:

```bash
kubectl get pods
```

Display deployments:

```bash
kubectl get deployments
```

These commands require access to a configured Kubernetes cluster.

You do not need to install Kubernetes for this lesson.

---

## 15. Practical Lab: Inspect Your Virtualization Environment

This section focuses on your existing Kali VM.

### Step 1: Create your lab directory

```bash
mkdir -p ~/module17-lab/lesson12
cd ~/module17-lab/lesson12
```

### Step 2: Identify the virtualization environment

```bash
systemd-detect-virt
```

### Step 3: Inspect CPU virtualization information

```bash
lscpu | grep -i virtualization
```

### Step 4: Inspect memory

```bash
free -h
```

### Step 5: Inspect block devices

```bash
lsblk
```

### Step 6: Inspect mounted filesystems

```bash
findmnt
```

### Step 7: Inspect kernel information

```bash
uname -a
```

### Step 8: Record your findings

Write down:

- Virtualization platform.
- Guest operating system.
- Available CPU resources.
- Available memory.
- Virtual disk size.
- Mounted filesystems.

---

## 16. Practical Lab: Docker Container Deployment

This practical exercise is optional if Docker is not installed.

### Step 1: Check Docker availability

```bash
command -v docker
```

### Step 2: Inspect Docker version

```bash
docker --version
```

### Step 3: Check Docker service

```bash
systemctl status docker
```

If Docker is not installed, do not install it just to complete the virtualization inspection. You can complete this project later in an approved Docker environment.

### Step 4: Pull the practice image

```bash
docker pull nginx:alpine
```

### Step 5: Run the container

```bash
docker run -d --name lesson12-nginx -p 127.0.0.1:8080:80 nginx:alpine
```

The port binding exposes the web service only on the local loopback interface.

### Step 6: Verify the container

```bash
docker ps
```

### Step 7: Test the web service

```bash
curl http://127.0.0.1:8080
```

You should receive the default Nginx response.

### Step 8: Inspect the logs

```bash
docker logs lesson12-nginx
```

### Step 9: Inspect resource usage

```bash
docker stats --no-stream lesson12-nginx
```

### Step 10: Stop and remove the container

```bash
docker stop lesson12-nginx
docker rm lesson12-nginx
```

### Step 11: Remove the unused practice image if desired

```bash
docker rmi nginx:alpine
```

Only remove it if no other container depends on it.

---

## 17. Practical Project: Container Environment Report

Create a script that records virtualization and container information.

### Step 1: Create the script

```bash
nano container-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson12/container-report.txt"

{
    echo "======================================"
    echo "     VIRTUALIZATION REPORT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "OPERATING SYSTEM:"
    cat /etc/os-release

    echo
    echo "KERNEL:"
    uname -a

    echo
    echo "VIRTUALIZATION TYPE:"
    systemd-detect-virt

    echo
    echo "CPU INFORMATION:"
    lscpu

    echo
    echo "MEMORY:"
    free -h

    echo
    echo "BLOCK DEVICES:"
    lsblk

    echo
    echo "FILESYSTEMS:"
    findmnt

    echo
    echo "DOCKER AVAILABILITY:"
    if command -v docker >/dev/null 2>&1; then
        docker --version
        docker ps -a
    else
        echo "Docker is not installed."
    fi

    echo
    echo "KUBERNETES AVAILABILITY:"
    if command -v kubectl >/dev/null 2>&1; then
        kubectl version --client
    else
        echo "kubectl is not installed."
    fi

    echo
    echo "======================================"
    echo "       REPORT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Report saved to: $REPORT"
```

### Step 2: Make executable

```bash
chmod +x container-report.sh
```

### Step 3: Run the script

```bash
./container-report.sh
```

### Step 4: Read the report

```bash
cat ~/module17-lab/lesson12/container-report.txt
```

### Step 5: Verify the file

```bash
ls -lh ~/module17-lab/lesson12/
```

---

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is virtualization?
2. What is a hypervisor?
3. Differentiate Type 1 and Type 2 hypervisors.
4. What is a virtual machine?
5. What is a container?
6. Why do containers generally consume fewer resources than virtual machines?
7. What is Docker?
8. Differentiate a Docker image and container.
9. What is a Dockerfile?
10. What is a Docker registry?
11. Explain Docker networking.
12. What are Docker volumes?
13. Why should containers not run as root unnecessarily?
14. What is Kubernetes?
15. Explain the purpose of a Kubernetes Pod.
16. Identify five container security risks.

---

## 19. Important Commands Summary

| Command | Purpose |
|---|---|
| `systemd-detect-virt` | Identify virtualization environment |
| `lscpu` | Inspect CPU information |
| `free -h` | Inspect memory |
| `lsblk` | Inspect block devices |
| `findmnt` | Inspect mounted filesystems |
| `docker --version` | Display Docker version |
| `docker images` | List Docker images |
| `docker pull` | Download an image |
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker inspect` | Inspect container configuration |
| `docker logs` | View container logs |
| `docker stats` | Monitor container resources |
| `docker network ls` | List Docker networks |
| `docker volume ls` | List Docker volumes |
| `docker build` | Build an image |
| `kubectl get pods` | List Kubernetes pods |

---

## 20. Lesson Summary

In this lesson, you studied:

- Virtualization fundamentals.
- Hypervisors.
- Virtual machines.
- Container architecture.
- Docker fundamentals.
- Docker images.
- Docker containers.
- Container networking.
- Container storage.
- Dockerfiles.
- Container resource management.
- Container security.
- Kubernetes fundamentals.

You also learned how to inspect your virtualization environment and deploy a local Docker container.

### Completion checklist

- [ ] Understand virtualization.
- [ ] Understand hypervisors.
- [ ] Differentiate VMs and containers.
- [ ] Understand Docker architecture.
- [ ] Understand images and containers.
- [ ] Understand Docker networking.
- [ ] Understand Docker volumes.
- [ ] Understand Dockerfiles.
- [ ] Understand container security.
- [ ] Understand Kubernetes fundamentals.
- [ ] Complete the virtualization inspection.
- [ ] Build the container environment report.

**END OF LESSON 12**
