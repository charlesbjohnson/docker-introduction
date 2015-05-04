# Docker Introduction

**@WalmartLabs**
<br />
May 4th, 2015
<br />
Charles B Johnson

---

## Agenda

- What is Docker?

---

## What is Docker?

![docker from a marketing perspective](public/images/docker_marketing.png)

---
### What is Docker? (cont.)

#### Container virtualization tooling

- Docker Engine (github.com/docker/docker)
  - The core of the Docker projects
  - Originally a high level wrapper around lxc-containers (github.com/lxc/lxc)
  - Now using its own implementation (github.com/docker/libcontainer)

---

### What is Docker? (cont.)

#### Containers?

- Similar to `chroot`
- Virtual environment with its own resources (CPU, memory, storage, networking, etc.)
- "An environment as close as possible to a standard Linux installation but without the need for a separate kernel" (linuxcontainers.org/lxc/introduction/)
- Single kernel and host OS shared between containers

---

![vm vs container](public/images/vm_vs_container_diagram.png)

---

### What is Docker (cont.)

- Containers
  - `cgroups`
      - ie. so that a container cannot use all available CPU resources and starve other containers
  - Kernel namespaces
      - ie. so that processes in a container have no visibility into the processes in another container
- Docker
  - Images
      - Can be declaratively built with Dockerfiles
      - Layered through a Union File System (ie. AUFS)
      - Distributable through the Docker Registry
      - Versioned and tagged with metadata
  - Containers
      - Created from images
      - Run your actual application

---

### What is Docker (cont.)

![image vs container](public/images/image_vs_container_diagram.png)

---

## Agenda

- ~~What is Docker?~~
