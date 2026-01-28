# 🐳 Docker Interview Questions & Answers (DevOps – Fresher to Mid-Level)

This README contains **60 Docker interview questions with clear, medium-length, interview-ready answers**. It is suitable for **DevOps Engineer roles (Fresher to Mid-level)** and focuses on **core concepts, real-world usage, troubleshooting, security, and best practices**.

---

## 📘 Part 1: Core Docker Concepts (1–30)

### 1. What is Docker?

Docker is an open-source containerization platform that allows developers to package applications with their dependencies into containers. Containers run consistently across environments, making deployment faster, reliable, and portable compared to traditional virtual machines.

### 2. What is a Docker container?

A Docker container is a lightweight, standalone executable package that includes an application, its dependencies, libraries, and configuration files. Containers share the host OS kernel but remain isolated from each other.

### 3. What is the difference between Docker and Virtual Machines?

Docker containers share the host OS kernel, making them lightweight and faster to start. Virtual Machines run a full OS with a hypervisor, consuming more resources and taking longer to boot.

### 4. What is a Docker image?

A Docker image is a read-only template used to create containers. It contains application code, runtime, libraries, and dependencies. Images are built in layers and stored in registries like Docker Hub.

### 5. What is Docker Hub?

Docker Hub is a cloud-based registry service where Docker images can be stored, shared, and managed. It provides official images, community images, and private repositories.

### 6. What is a Dockerfile?

A Dockerfile is a text file containing a set of instructions used to build a Docker image. It defines the base image, dependencies, commands, environment variables, and exposed ports.

### 7. Explain the FROM instruction in Dockerfile

FROM specifies the base image for building a Docker image. Every Dockerfile must start with a FROM instruction unless using ARG before it.

### 8. What is the difference between CMD and ENTRYPOINT?

CMD provides default arguments that can be overridden at runtime, while ENTRYPOINT defines a fixed command that always runs when the container starts. They are often used together for flexibility.

### 9. What are Docker layers?

Docker images are built in layers, where each instruction in the Dockerfile creates a new layer. Layers help in caching, faster builds, and reusability.

### 10. What is Docker Engine?

Docker Engine is the core component that runs Docker containers. It includes the Docker daemon, REST API, and CLI for building and managing containers.

### 11. What is a Docker volume?

A Docker volume is a persistent storage mechanism used to store data outside the container’s lifecycle. Volumes are managed by Docker and are preferred over bind mounts for data persistence.

### 12. What is the difference between volume and bind mount?

Volumes are managed by Docker and stored in Docker’s directory, while bind mounts map a host directory directly into a container. Volumes are more portable and secure.

### 13. What is Docker networking?

Docker networking allows containers to communicate with each other and external systems. Docker provides bridge, host, none, and overlay network drivers.

### 14. What is the default Docker network?

The default Docker network is the bridge network, which allows containers on the same host to communicate using internal IP addresses.

### 15. What is port mapping in Docker?

Port mapping connects a container’s internal port to a port on the host machine using the `-p` flag. It enables external access to containerized applications.

### 16. What is Docker Compose?

Docker Compose is a tool used to define and run multi-container applications using a YAML file. It simplifies managing services, networks, and volumes.

### 17. What is docker-compose.yml?

It is a configuration file that defines services, images, ports, networks, and volumes for a multi-container application. It enables one-command deployment using `docker-compose up`.

### 18. What is Docker Swarm?

Docker Swarm is Docker’s native container orchestration tool. It manages a cluster of Docker nodes and handles service deployment, scaling, and load balancing.

### 19. What is a multi-stage Docker build?

Multi-stage builds allow using multiple FROM statements in a Dockerfile to reduce image size by copying only required artifacts into the final image.

### 20. What is container isolation?

Container isolation ensures that containers run independently without interfering with each other. Docker uses Linux namespaces and cgroups to achieve isolation.

### 21. How do you check running containers?

You can use the command `docker ps` to list running containers and `docker ps -a` to list all containers, including stopped ones.

### 22. How do you stop and remove a container?

To stop a container, use:

```
docker stop <container_id>
```

To remove it, use:

```
docker rm <container_id>
```

### 23. How do you view container logs?

Container logs can be viewed using the `docker logs <container_id>` command. It helps in debugging application issues.

### 24. What is Docker caching?

Docker caching reuses unchanged image layers during image build to speed up the build process. Changing a Dockerfile instruction invalidates cache for subsequent layers.

### 25. What is a dangling image?

Dangling images are unused images without tags, usually created during image rebuilds. They can be cleaned using `docker image prune`.

### 26. How do you reduce Docker image size?

Use lightweight base images, multi-stage builds, minimize layers, and remove unnecessary packages to reduce image size.

### 27. What is Docker restart policy?

Restart policies define how containers restart after failure or reboot. Common policies include `always`, `on-failure`, and `unless-stopped`.

### 28. How do you exec into a running container?

You can access a running container using:

```
docker exec -it <container_id> /bin/bash
```

### 29. What are Docker secrets?

Docker secrets securely store sensitive data like passwords and API keys, mainly used in Docker Swarm environments.

### 30. Why is Docker important for DevOps?

Docker enables consistency, faster deployments, easier scaling, and smooth CI/CD integration, making it a core tool in modern DevOps practices.

---

## 📙 Part 2: Real-World, Troubleshooting & Best Practices (31–60)

### 31. What happens when you run docker run?

`docker run` pulls the image if not available locally, creates a container, allocates a filesystem, sets up networking, and starts the container with the specified command.

### 32. What is the difference between docker run and docker start?

`docker run` creates and starts a new container, while `docker start` starts an existing stopped container without creating a new one.

### 33. What is an orphan container?

An orphan container is a container that is no longer managed by Docker Compose but still exists after services are removed or renamed.

### 34. What is Docker inspect?

`docker inspect` provides detailed JSON output about containers, images, networks, and volumes, useful for debugging configuration issues.

### 35. What is the purpose of .dockerignore?

`.dockerignore` excludes unnecessary files from the Docker build context, reducing image size and improving build performance.

### 36. What is a container lifecycle?

The container lifecycle includes created, running, paused, stopped, and deleted states. Containers exist only while the main process is running.

### 37. What is the difference between COPY and ADD?

COPY simply copies files, while ADD has extra features like extracting archives and fetching URLs. COPY is preferred for clarity and security.

### 38. What are Docker namespaces?

Namespaces provide isolation by separating process IDs, network, filesystem, users, and hostnames between containers.

### 39. What are cgroups in Docker?

Control groups (cgroups) limit and monitor resource usage such as CPU, memory, and I/O for containers, preventing resource exhaustion.

### 40. How do you limit container resources?

You can limit resources using flags like `--memory`, `--cpus`, and `--cpu-shares` while running a container.

### 41. What is Docker overlay network?

An overlay network allows containers running on different hosts to communicate securely, mainly used in Docker Swarm.

### 42. What is Docker bridge network?

A bridge network is a private internal network on a single Docker host that enables container-to-container communication.

### 43. How does Docker handle logging?

Docker captures container stdout and stderr. Logs can be viewed using `docker logs` or forwarded to logging drivers like JSON-file, syslog, or ELK.

### 44. What is a sidecar container?

A sidecar container runs alongside a main container to provide supporting features like logging, monitoring, or proxy services.

### 45. What is Docker health check?

Docker health checks monitor container health using commands defined in the Dockerfile or Compose file to detect unhealthy containers.

### 46. What is the difference between docker attach and docker exec?

`docker attach` connects to the main process, while `docker exec` runs a new command inside a running container.

### 47. What is Docker image tagging?

Tagging assigns version names to images, helping manage releases and rollbacks, such as `app:v1`, `app:latest`.

### 48. What is image immutability?

Docker images are immutable, meaning once built they cannot be changed. Any modification requires creating a new image.

### 49. How do you share data between containers?

Data can be shared using Docker volumes or by mounting the same volume into multiple containers.

### 50. What is a Docker registry?

A Docker registry stores and distributes Docker images. It can be public like Docker Hub or private like AWS ECR.

### 51. How do you push an image to Docker Hub?

Login using `docker login`, tag the image, and push using:

```
docker push <username/image:tag>
```

### 52. What is Docker build context?

The build context is the set of files sent to the Docker daemon during image build, defined by the directory passed to `docker build`.

### 53. What is Docker rootless mode?

Rootless mode allows running Docker without root privileges, improving security by reducing attack surface.

### 54. What is container sprawl?

Container sprawl occurs when too many unused containers or images accumulate, causing resource wastage and management issues.

### 55. How do you clean up Docker resources?

Use commands like `docker system prune`, `docker image prune`, and `docker volume prune`.

### 56. What is Docker checkpoint and restore?

It allows pausing and saving container state and restoring it later, useful for live migration (experimental feature).

### 57. What is a private Docker registry?

A private registry hosts Docker images internally, improving security, control, and compliance.

### 58. How do you secure Docker containers?

Use minimal base images, non-root users, image scanning, resource limits, secrets management, and network isolation.

### 59. What is Docker-in-Docker (DinD)?

Docker-in-Docker runs Docker inside a container, commonly used in CI/CD pipelines, but must be used carefully due to security risks.

### 60. How does Docker fit into CI/CD pipelines?

Docker ensures consistent environments, enables faster builds, supports automated testing, and simplifies deployment across environments.

---

## ✅ Usage

* Use this README for **interview preparation**
* Revise before **DevOps / Cloud interviews**

🚀 *Happy Learning & Best of Luck for Your DevOps Interviews!*