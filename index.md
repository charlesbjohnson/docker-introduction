# Docker Introduction

**@WalmartLabs**
<br />
May 4th, 2015
<br />
Charles B Johnson

---

## Agenda

- *What is Docker?*
- How Do You Use Docker?

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
- *How Do You Use Docker?*

---

## How Do You Use Docker?

#### Installation

- On OS X
  - Containers rely on capabilities that are only in the Linux kernel
  - Need to use a Linux VM to host Docker, Boot2Docker (github.com/boot2docker/boot2docker)
  is typically used
- On Linux
  - A kernel version 3.10+
  - Or 2.6.32-431 with backported kernel fixes
  - TL;DR Centos 6.5 can work with Docker, but 7+ is recommended

---

### How Do You Use Docker? (cont.)

#### In a nutshell

- Select a base image
- Add layers on top of that image to meet your application dependencies
- Run a container based on that image
  - Start your application

This can be done declaratively with a Dockerfile, or manually

---

### How Do You Use Docker? (cont.)

#### An io.js container in a nutshell, the manual way

- Select a base image
  - ubuntu:15.04

![ubuntu docker hub](public/images/ubuntu_docker_hub.png)

---

### How Do You Use Docker? (cont.)

- Add layers on top of that image to meet your application dependencies
  - Create a layer that installs io.js and its dependencies

```bash
base_image="ubuntu:15.04"
iojs="https://iojs.org/dist/v1.8.1/iojs-v1.8.1-linux-x64.tar.gz"

script="
  apt-get update --assume-yes && apt-get install --assume-yes curl

  cd /usr/local
  curl --location \"$iojs\" \
    | tar --extract --gunzip --strip-components 1
"

container_id=$(docker run --detach $base_image bash -c "$script")
docker wait $container_id
image_id=$(docker commit $container_id)
```

---

### How Do You Use Docker? (cont.)

- Run a container based on that image
  - Start your application

```bash
app="
  let http = require('http');

  let server = http.createServer((request, response) => {
    response.end('Hello World\n');
  });

  server.listen(9000);
  console.log('Server running on http://127.0.0.1:9000');
"

docker run --publish 9001:9000 $image_id \
  iojs --use_strict --harmony_arrow_functions --eval "$app"
```

![it works](public/images/iojs_example.png)

---

## Agenda

- ~~What is Docker?~~
- ~~How Do You Use Docker?~~
