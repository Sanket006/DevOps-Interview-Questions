# Docker Troubleshooting Interview Questions & Model Answers

This README contains **real-world, scenario-based Docker troubleshooting questions with detailed answers**, exactly aligned with what **DevOps interviewers expect** and what you will face **on the job**.

---

## Part 1: 20 Scenario-Based Docker Troubleshooting Questions (Step-by-Step)

### 1. Container exits immediately after starting

**Scenario**

You run `docker run myapp`, but the container exits instantly.

**Troubleshooting & Answer**

Check container logs:

```bash
docker logs <container_id>
```

Common causes:

* Main process finished execution
* Application crashed
* Wrong CMD or ENTRYPOINT

**Fix:**

Ensure a foreground process runs (not background)

Example:

```dockerfile
CMD ["node", "app.js"]
```

Use:

```bash
docker run -it myapp /bin/bash
```

to debug interactively.

---

### 2. Port is not accessible from the browser

**Scenario**

App runs inside container but `localhost:8080` is unreachable.

**Troubleshooting & Answer**

Verify port mapping:

```bash
docker ps
```

Correct syntax:

```bash
docker run -p 8080:80 myapp
```

Ensure app listens on `0.0.0.0`, not `localhost`

Check container firewall rules

---

### 3. “Cannot connect to Docker daemon” error

**Scenario**

Docker commands fail with daemon connection error.

**Troubleshooting & Answer**

Check daemon status:

```bash
systemctl status docker
```

Start Docker:

```bash
sudo systemctl start docker
```

Add user to Docker group:

```bash
sudo usermod -aG docker $USER
```

Logout and login again

---

### 4. Image builds successfully but container fails at runtime

**Scenario**

`docker build` succeeds, but `docker run` fails.

**Troubleshooting & Answer**

Inspect logs:

```bash
docker logs <container_id>
```

Common causes:

* Missing runtime dependency
* Wrong environment variables

Fix by validating:

```dockerfile
ENV NODE_ENV=production
```

Test locally inside container shell

---

### 5. Container cannot access the internet

**Scenario**

Inside container, `ping google.com` fails.

**Troubleshooting & Answer**

Check Docker network:

```bash
docker network ls
```

Inspect network:

```bash
docker network inspect bridge
```

Restart Docker:

```bash
systemctl restart docker
```

Verify DNS:

```bash
cat /etc/resolv.conf
```

---

### 6. High memory usage causing container to crash

**Scenario**

Container exits with `OOMKilled`.

**Troubleshooting & Answer**

Check container status:

```bash
docker inspect <container_id>
```

Set memory limits:

```bash
docker run -m 512m myapp
```

Optimize application memory usage

Monitor using:

```bash
docker stats
```

---

### 7. Docker build is very slow

**Scenario**

Image build takes too long.

**Troubleshooting & Answer**

* Use `.dockerignore`
* Optimize layer caching:

```dockerfile
COPY package.json .
RUN npm install
COPY . .
```

* Reduce image size using alpine base images

---

### 8. Volume data disappears after container restart

**Scenario**

Data is lost after container stops.

**Troubleshooting & Answer**

Use named volumes:

```bash
docker volume create mydata
docker run -v mydata:/app/data myapp
```

Avoid anonymous volumes

Verify volume:

```bash
docker volume inspect mydata
```

---

### 9. Permission denied inside container

**Scenario**

Application cannot write to mounted volume.

**Troubleshooting & Answer**

* Check host directory permissions
* Match UID/GID:

```dockerfile
RUN useradd -u 1000 appuser
USER appuser
```

Use:

```bash
chown -R 1000:1000 /data
```

---

### 10. “Executable file not found in $PATH”

**Scenario**

Container fails to start with exec error.

**Troubleshooting & Answer**

Verify ENTRYPOINT:

```dockerfile
ENTRYPOINT ["./start.sh"]
```

Ensure executable permission:

```bash
chmod +x start.sh
```

Use absolute paths

---

### 11. Container cannot communicate with another container

**Scenario**

Microservices cannot talk to each other.

**Troubleshooting & Answer**

Create custom network:

```bash
docker network create mynet
```

Run containers on same network

Use container name instead of IP

---

### 12. Dockerfile COPY fails

**Scenario**

Build fails at COPY step.

**Troubleshooting & Answer**

Verify build context:

```bash
docker build .
```

Check `.dockerignore`

Use correct relative paths

---

### 13. Image size is too large

**Scenario**

Docker image is several GBs.

**Troubleshooting & Answer**

* Use multi-stage builds
* Use slim base images
* Remove unnecessary files:

```dockerfile
RUN rm -rf /var/lib/apt/lists/*
```

---

### 14. Environment variables not available inside container

**Scenario**

App cannot read env variables.

**Troubleshooting & Answer**

Pass env correctly:

```bash
docker run -e DB_HOST=localhost myapp
```

Or use env file:

```bash
docker run --env-file .env myapp
```

Validate inside container

---

### 15. Container restarts continuously

**Scenario**

Container in restart loop.

**Troubleshooting & Answer**

Check restart policy:

```bash
docker inspect <container_id>
```

Inspect logs

Fix crash cause before enabling restart

---

### 16. Disk space fills quickly

**Scenario**

Docker consumes all disk space.

**Troubleshooting & Answer**

Check usage:

```bash
docker system df
```

Clean unused data:

```bash
docker system prune -a
```

Monitor log sizes

---

### 17. “Bind for 0.0.0.0 failed: port already allocated”

**Scenario**

Port conflict error.

**Troubleshooting & Answer**

Identify process:

```bash
lsof -i :8080
```

Use different port:

```bash
docker run -p 9090:80 myapp
```

---

### 18. Docker container time is incorrect

**Scenario**

Container time differs from host.

**Troubleshooting & Answer**

Mount timezone:

```bash
-v /etc/localtime:/etc/localtime:ro
```

Set TZ env variable

---

### 19. Application works locally but not in Docker

**Scenario**

App runs fine outside container.

**Troubleshooting & Answer**

* Verify dependencies
* Match OS environment
* Check network and file paths
* Debug inside container shell

---

### 20. Docker login fails to private registry

**Scenario**

Cannot pull private images.

**Troubleshooting & Answer**

Login properly:

```bash
docker login registry.example.com
```

Check credentials and permissions

Verify network and TLS certificates

---

## Part 2: Top 20 Most-Asked Docker Scenario Questions (Perfect Interview Answers)

### Interview Answer Format

**LOGS → ROOT CAUSE → FIX → PREVENTION**

---

### 1. Container exits immediately after start

**Scenario**

Container starts and stops instantly.

**Perfect Answer**

* Logs: `docker logs <id>`
* Root cause: Main process finished or crashed; CMD runs in background
* Fix:

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

* Prevention: Always ensure one long-running PID 1 process

---

### 2. Application not accessible on browser

**Scenario**

App works inside container but not on localhost.

**Perfect Answer**

* Logs: Check `docker ps`
* Root cause: Port not exposed or app bound to localhost
* Fix:

```bash
docker run -p 8080:80 myapp
```

App must listen on `0.0.0.0`

* Prevention: Standardize port exposure in Dockerfile

---

### 3. “Cannot connect to Docker daemon”

**Scenario**

Docker commands fail.

**Perfect Answer**

* Root cause: Docker service not running or permission issue
* Fix:

```bash
systemctl start docker
usermod -aG docker $USER
```

* Prevention: Enable Docker on boot

---

### 4. Image builds but fails at runtime

**Scenario**

Build succeeds; container crashes.

**Perfect Answer**

* Logs: `docker logs`
* Root cause: Missing runtime dependency or env variable
* Fix: Validate runtime dependencies and ENV
* Prevention: Use health checks and runtime tests

---

### 5. Container cannot reach the internet

**Scenario**

No outbound connectivity.

**Perfect Answer**

* Root cause: Broken bridge network or DNS issue
* Fix: Restart Docker, verify `resolv.conf`
* Prevention: Avoid custom DNS unless required

---

### 6. Container killed due to high memory (OOM)

**Scenario**

Container exits with OOMKilled.

**Perfect Answer**

* Root cause: Memory limit exceeded
* Fix:

```bash
docker run -m 512m myapp
```

* Prevention: Memory profiling + limits

---

### 7. Docker build is extremely slow

**Scenario**

Build takes several minutes.

**Perfect Answer**

* Root cause: Poor layer caching, large context
* Fix: Use `.dockerignore` and optimized COPY
* Prevention: Order Dockerfile layers properly

---

### 8. Data lost after container restart

**Scenario**

Container data disappears.

**Perfect Answer**

* Root cause: No persistent volume
* Fix:

```bash
docker run -v myvol:/data myapp
```

* Prevention: Always use named volumes

---

### 9. Permission denied on mounted volume

**Scenario**

App cannot write to volume.

**Perfect Answer**

* Root cause: UID/GID mismatch
* Fix: Align container user with host UID
* Prevention: Run non-root containers properly

---

### 10. “Executable file not found in $PATH”

**Scenario**

Container fails at startup.

**Perfect Answer**

* Root cause: Script not executable or wrong path
* Fix:

```bash
chmod +x start.sh
```

* Prevention: Use absolute paths in ENTRYPOINT

---

### 11. Containers cannot communicate

**Scenario**

Microservices fail to connect.

**Perfect Answer**

* Root cause: Different networks
* Fix: Use custom bridge network
* Prevention: Docker Compose or named networks

---

### 12. COPY fails during build

**Scenario**

Docker build fails at COPY.

**Perfect Answer**

* Root cause: Wrong build context or `.dockerignore`
* Fix: Run `docker build .` from correct directory
* Prevention: Keep clean repo structure

---

### 13. Docker image size too large

**Scenario**

Image is multiple GBs.

**Perfect Answer**

* Root cause: Heavy base image, unnecessary files
* Fix: Multi-stage builds, alpine images
* Prevention: Minimal runtime images

---

### 14. Environment variables not available

**Scenario**

App cannot read env vars.

**Perfect Answer**

* Root cause: Variables not passed
* Fix:

```bash
docker run -e KEY=value myapp
```

* Prevention: Use env files or secrets

---

### 15. Container stuck in restart loop

**Scenario**

Container restarts continuously.

**Perfect Answer**

* Root cause: App crash with restart policy enabled
* Fix: Debug crash before restart policy
* Prevention: Health checks

---

### 16. Docker consuming too much disk space

**Scenario**

Disk fills quickly.

**Perfect Answer**

* Root cause: Unused images, volumes, logs
* Fix:

```bash
docker system prune -a
```

* Prevention: Scheduled cleanup

---

### 17. Port already in use error

**Scenario**

Bind failed error.

**Perfect Answer**

* Root cause: Port conflict
* Fix: Use different host port
* Prevention: Document port usage

---

### 18. Container time mismatch

**Scenario**

Time differs from host.

**Perfect Answer**

* Root cause: Timezone mismatch
* Fix: Mount `/etc/localtime`
* Prevention: Standard TZ configuration

---

### 19. App works locally but not in Docker

**Scenario**

Works on host, fails in container.

**Perfect Answer**

* Root cause: OS or dependency mismatch
* Fix: Debug inside container
* Prevention: Parity between local & container env

---

### 20. Docker pull fails from private registry

**Scenario**

Cannot pull private image.

**Perfect Answer**

* Root cause: Authentication or TLS issue
* Fix: Proper docker login
* Prevention: Use credential helpers

---

## 🔥 How to Answer in Interviews (One-Line Formula)

> **“I first check container logs, inspect the container, identify the root cause, fix it, and then add preventive controls like limits, health checks, or automation.”**

---

### ✅ Final Tip for You, Sanket

If you consistently answer Docker troubleshooting questions using:

**Logs → Inspect → Root Cause → Fix → Prevention**

You will sound like a **real production-ready DevOps engineer**, not just someone who knows commands.
