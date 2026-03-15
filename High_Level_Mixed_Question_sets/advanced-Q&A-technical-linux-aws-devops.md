# 🚀 DevOps Engineer Interview Questions (Fundamentals → Advanced)

This repository contains **100 technical DevOps Engineer interview questions**, structured from **fundamentals to advanced concepts**. It covers **Linux, Git, CI/CD, Docker, Kubernetes, Cloud (AWS), Infrastructure as Code, Monitoring, Security, and Networking**.

This set is **ideal for fresher to mid-level DevOps roles** and is **highly aligned with real DevOps interview preparation**.

---

## 🐧 Linux (1–15)

1. What is the difference between soft link and hard link?
2. Explain the Linux boot process.
3. What is systemd and how is it different from init?
4. How do you check disk usage and inode usage in Linux?
5. What is the difference between top, htop, and ps?
6. Explain file permissions and chmod with examples.
7. What is umask?
8. How do you find a process using a specific port?
9. What are runlevels / targets in Linux?
10. Explain crontab and its syntax.
11. Difference between /etc/passwd and /etc/shadow?
12. What is a zombie process?
13. How do you troubleshoot high CPU or memory usage?
14. What is swap memory?
15. Explain grep, awk, and sed.

---

## 🌱 Git & GitHub (16–25)

16. Difference between git fetch and git pull?
17. What is a merge conflict and how do you resolve it?
18. Explain Git branching strategy.
19. What is git rebase?
20. Difference between merge and rebase?
21. What is .gitignore?
22. How do you revert a commit?
23. What is a pull request?
24. What is Git tagging?
25. How do you secure a GitHub repository?

---

## 🔁 CI/CD (26–40)

26. What is CI/CD?
27. Difference between Continuous Integration and Continuous Deployment?
28. Explain a typical CI/CD pipeline.
29. What is Jenkins?
30. What are Jenkins agents?
31. Difference between Freestyle and Pipeline jobs?
32. What is a Jenkinsfile?
33. Declarative vs Scripted pipeline?
34. What are Jenkins stages?
35. How do you secure Jenkins?
36. How do you handle secrets in CI/CD?
37. What is Blue-Green deployment?
38. What is Canary deployment?
39. Rollback strategy in CI/CD?
40. How do you integrate Jenkins with Docker?

---

## 🐳 Docker (41–55)

41. What is Docker?
42. Difference between Docker image and container?
43. What is a Dockerfile?
44. Explain Dockerfile instructions.
45. What is Docker Compose?
46. Difference between CMD and ENTRYPOINT?
47. What is Docker volume?
48. Bind mount vs volume?
49. How do you reduce Docker image size?
50. What is multi-stage build?
51. How do you expose ports in Docker?
52. Docker networking types?
53. How do you troubleshoot a failing container?
54. What is Docker registry?
55. Difference between Docker and VM?

---

## ☸ Kubernetes (56–75)

56. What is Kubernetes?
57. What problem does Kubernetes solve?
58. Explain Kubernetes architecture.
59. What is a Pod?
60. Difference between Pod and Deployment?
61. What is a ReplicaSet?
62. What is a Service?
63. Types of Kubernetes services?
64. What is Ingress?
65. ConfigMap vs Secret?
66. What is Namespace?
67. What is StatefulSet?
68. What is DaemonSet?
69. How does Kubernetes handle scaling?
70. What is HPA?
71. What is etcd?
72. How do you troubleshoot a pod crash?
73. What is CrashLoopBackOff?
74. Rolling update vs Recreate strategy?
75. How do you secure Kubernetes?

---

## ☁ AWS / Cloud (76–90)

76. What is cloud computing?
77. IaaS vs PaaS vs SaaS?
78. Explain EC2 instance types.
79. What is AMI?
80. What is VPC?
81. Public vs private subnet?
82. What is Internet Gateway vs NAT Gateway?
83. What is Security Group?
84. NACL vs Security Group?
85. What is Auto Scaling Group?
86. What is ELB?
87. ALB vs NLB?
88. What is S3 and its storage classes?
89. What is IAM?
90. IAM Role vs IAM User?

---

## 🏗 Infrastructure as Code (91–95)

91. What is Infrastructure as Code?
92. What is Terraform?
93. Terraform state file?
94. Difference between terraform plan and apply?
95. How do you manage multiple environments in Terraform?

---

## 📊 Monitoring, Security & Networking (96–100)

96. What is monitoring in DevOps?
97. Difference between monitoring and logging?
98. What tools are used for monitoring?
99. What is SSL/TLS?
100. What is DNS and how does it work?

---

## 🔥 How to use this effectively (for you)

* Answer **10 questions daily** out loud
* Revise weak areas after every mock interview
* Use this as a **checklist before DevOps interviews**

---

✅ **Tip:** Mastering these questions gives you strong coverage for **Linux, DevOps, Cloud, and SRE interview rounds**.

---

# Interview-Ready Linux, Git & CI/CD Notes

## Questions 1–10 (Linux)

### 1. Difference between Soft Link and Hard Link

| Hard Link                                | Soft Link (Symbolic Link)          |
| ---------------------------------------- | ---------------------------------- |
| Points to the inode                      | Points to the file path            |
| Works only within same filesystem        | Can work across filesystems        |
| File remains even if original is deleted | Breaks if original file is deleted |
| Cannot link directories (usually)        | Can link directories               |

📌 Example

```bash
ln file1 hardlink
ln -s file1 softlink
```

---

### 2. Linux Boot Process

Linux boot process steps:

1. **BIOS/UEFI** – Performs hardware checks
2. **Bootloader (GRUB)** – Loads the Linux kernel
3. **Kernel** – Initializes hardware and mounts root filesystem
4. **init/systemd** – Starts system services
5. **User Space** – Login prompt appears

📌 Modern Linux uses **systemd** instead of traditional **init**.

---

### 3. What is systemd? How is it different from init?

**systemd** is a system and service manager for Linux.

* Starts services in parallel, making boot faster

| init               | systemd          |
| ------------------ | ---------------- |
| Sequential startup | Parallel startup |
| Uses runlevels     | Uses targets     |
| Slower boot        | Faster boot      |
| Shell scripts      | Unit files       |

---

### 4. How do you check disk usage and inode usage?

📌 Disk usage

```bash
df -h
```

📌 Directory size

```bash
du -sh /path
```

📌 Inode usage

```bash
df -i
```

⚠ If inodes are full, you can’t create new files even if space exists.

---

### 5. Difference between top, htop, and ps

* **ps** – Snapshot of running processes
* **top** – Real-time process monitoring
* **htop** – Enhanced interactive version of top

📌 htop allows scrolling, searching, and killing processes easily.

---

### 6. Explain file permissions and chmod

Linux permissions:

* **r (4)** – read
* **w (2)** – write
* **x (1)** – execute

Permission format:

```bash
-rwxr-xr--
```

📌 chmod example

```bash
chmod 755 file.sh
```

* Owner: rwx
* Group: r-x
* Others: r-x

---

### 7. What is umask?

**umask** defines default permission for newly created files/directories.

It subtracts permissions from the system default.

📌 Example:

```bash
umask 022
```

* Files: 644
* Directories: 755

---

### 8. How do you find a process using a specific port?

```bash
sudo netstat -tulnp | grep 8080
```

or

```bash
sudo lsof -i :8080
```

📌 Useful when a service fails to start due to port conflict.

---

### 9. What are runlevels / targets in Linux?

* **Runlevels** are system states in SysV init
* **Targets** are the systemd replacement

Common targets:

* `multi-user.target` → CLI mode
* `graphical.target` → GUI mode
* `rescue.target` → Single-user mode

📌 Check current target:

```bash
systemctl get-default
```

---

### 10. Explain crontab and its syntax

**crontab** is used to schedule recurring tasks.

📌 Syntax:

```sql
* * * * * command
│ │ │ │ │
│ │ │ │ └ Day of week
│ │ │ └ Month
│ │ └ Day of month
│ └ Hour
└ Minute
```

📌 Example:

```bash
0 2 * * * /backup.sh
```

➡ Runs daily at 2 AM.

✅ **Interview Tip (Important)**

If you explain command + real use case, interviewers rate you higher.

---

## Questions 11–20 (Linux + Git)

### 11. Difference between /etc/passwd and /etc/shadow

| /etc/passwd                        | /etc/shadow                          |
| ---------------------------------- | ------------------------------------ |
| Stores user account info           | Stores encrypted passwords           |
| Readable by all users              | Accessible only by root              |
| Contains username, UID, GID, shell | Contains password hash & expiry info |

📌 Passwords were moved to `/etc/shadow` for security.

---

### 12. What is a zombie process?

A zombie process is a process that has finished execution but still has an entry in the process table.

Happens when the parent process doesn’t read the child’s exit status.

📌 Identified by **Z** state in `ps`.

```bash
ps aux | grep Z
```

⚠ Zombies don’t consume CPU but waste process table entries.

---

### 13. How do you troubleshoot high CPU or memory usage?

Steps:

1. Identify the process

```bash
top
```

2. Check detailed process info

```bash
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu
```

3. Kill or restart if required

```bash
kill -9 PID
```

4. Check logs:

```bash
/var/log/
```

📌 Always investigate root cause before killing.

---

### 14. What is swap memory?

Swap is disk space used as virtual memory when RAM is full.

Slower than RAM but prevents system crashes.

📌 Check swap:

```bash
free -h
swapon --show
```

📌 Common in low-memory servers.

---

### 15. Explain grep, awk, and sed

| Command | Purpose                       |
| ------- | ----------------------------- |
| grep    | Search text                   |
| awk     | Pattern scanning & processing |
| sed     | Stream editor (modify text)   |

📌 Examples:

```bash
grep "error" logfile
awk '{print $1}' file
sed 's/old/new/g' file
```

---

### 16. Difference between git fetch and git pull

| git fetch              | git pull            |
| ---------------------- | ------------------- |
| Downloads changes only | Downloads + merges  |
| Safe, no code change   | May cause conflicts |
| Manual merge required  | Auto merge          |

📌 Best practice: **fetch → review → merge**

---

### 17. What is a merge conflict and how do you resolve it?

Occurs when Git can’t auto-merge changes.

Happens when same lines are modified.

📌 Steps:

1. Open conflicted file
2. Resolve manually
3. Add and commit

```bash
git add file
git commit
```

---

### 18. Explain Git branching strategy

A branching strategy defines how branches are used.

Common types:

* **main/master** – Production
* **develop** – Development
* **feature/*** – New features
* **hotfix/*** – Urgent fixes

📌 Helps in collaboration and stable releases.

---

### 19. What is git rebase?

Rebase reapplies commits on top of another branch.

Creates linear commit history.

```bash
git rebase main
```

📌 Avoid rebasing shared branches.

---

### 20. Difference between merge and rebase

| Merge                | Rebase           |
| -------------------- | ---------------- |
| Preserves history    | Rewrites history |
| Creates merge commit | No extra commit  |
| Safer for teams      | Cleaner history  |

📌 Use **merge** for shared branches
📌 Use **rebase** for local cleanup

🔥 **Interview Advice (Important)**

If asked “Which one do you prefer?”, say:

> “Rebase for local branches, merge for shared branches.”

---

## Questions 21–30 (Git + CI/CD)

### 21. What is .gitignore?

`.gitignore` is a file that tells Git which files or directories to ignore.

Commonly used for logs, build files, secrets, and dependencies.

📌 Example:

```bash
node_modules/
.env
*.log
```

📌 Prevents accidental commits of sensitive or unnecessary files.

---

### 22. How do you revert a commit?

Two common ways:

1. Revert last commit (safe way)

```bash
git revert HEAD
```

Creates a new commit that undoes changes.

2. Reset commit (dangerous on shared branches)

```bash
git reset --hard HEAD~1
```

📌 Use revert in production environments.

---

### 23. What is a pull request (PR)?

A pull request is a request to merge changes from one branch to another.

Used for code review and collaboration.

📌 Typical PR flow:

1. Create feature branch
2. Push changes
3. Open PR
4. Review → Merge

---

### 24. What is Git tagging?

Tags are used to mark specific points in history, usually releases.

📌 Example:

```bash
git tag v1.0
git push origin v1.0
```

📌 Tags are immutable and used for versioning.

---

### 25. How do you secure a GitHub repository?

Common practices:

* Use private repositories
* Enable branch protection rules
* Require PR reviews
* Use SSH keys
* Scan secrets using GitHub security tools

📌 Prevents unauthorized changes.

---

### 26. What is CI/CD?

CI/CD automates build, test, and deployment.

Improves code quality and delivery speed.

📌 **CI** → Build & Test
📌 **CD** → Deploy automatically

---

### 27. Difference between Continuous Integration and Continuous Deployment

| CI                   | CD                   |
| -------------------- | -------------------- |
| Frequent code merges | Automated deployment |
| Runs tests           | Pushes to production |
| Detects issues early | Faster releases      |

📌 CI is mandatory, CD is optional.

---

### 28. Explain a typical CI/CD pipeline

Steps:

1. Code commit
2. Build
3. Test
4. Security scan
5. Deploy
6. Monitor

📌 Triggered automatically on code push.

---

### 29. What is Jenkins?

Jenkins is an open-source automation server.

Used for building CI/CD pipelines.

📌 Supports:

* Plugins
* Distributed builds
* Pipeline as Code

---

### 30. What are Jenkins agents?

Agents execute pipeline jobs.

Master controls scheduling, agents do the work.

📌 Benefits:

* Load distribution
* Multiple OS support
* Scalability

🎯 **Interview Tip**

If asked “Have you used Jenkins?”, say:

> “Yes, I’ve created CI/CD pipelines using Jenkinsfile, integrated GitHub, built Docker images, and deployed applications.”

---

# Jenkins & Docker Interview-Ready Notes

## Questions 31–40 (Jenkins + Deployment Strategies)

### 31. Difference between Freestyle and Pipeline jobs

| Freestyle Job                   | Pipeline Job             |
| ------------------------------- | ------------------------ |
| GUI-based                       | Code-based               |
| Harder to version control       | Stored as Jenkinsfile    |
| Limited flexibility             | Highly flexible          |
| Not ideal for complex workflows | Best for CI/CD pipelines |

📌 Pipeline jobs are preferred in modern DevOps.

---

### 32. What is a Jenkinsfile?

A **Jenkinsfile** defines the CI/CD pipeline as code.

Written in **Groovy**.

📌 Benefits:

* Version controlled
* Reusable
* Auditable

📌 Example:

```jenkinsfile
pipeline {
  stages {
    stage('Build') { steps { echo 'Building' } }
  }
}
```

---

### 33. Declarative vs Scripted pipeline

| Declarative          | Scripted           |
| -------------------- | ------------------ |
| Simple & structured  | More flexible      |
| Less code            | Complex logic      |
| Easier for beginners | Advanced use cases |
| Uses pipeline {}     | Uses node {}       |

📌 Declarative is recommended unless advanced logic is required.

---

### 34. What are Jenkins stages?

Stages represent logical steps in a pipeline.

Helps visualize pipeline execution.

📌 Common stages:

* Build
* Test
* Scan
* Deploy

---

### 35. How do you secure Jenkins?

Best practices:

* Enable role-based access control
* Use HTTPS
* Restrict admin access
* Secure credentials using Jenkins Credentials Manager
* Keep plugins updated

📌 Never hardcode secrets.

---

### 36. How do you handle secrets in CI/CD?

Secure methods:

* Jenkins credentials store
* Environment variables
* Vault (HashiCorp Vault, AWS Secrets Manager)
* Encrypted files

📌 Secrets should never be stored in Git.

---

### 37. What is Blue-Green deployment?

Two environments: **Blue (live)** and **Green (new)**.

Traffic is switched after successful deployment.

📌 Benefits:

* Zero downtime
* Easy rollback

📌 Used with load balancers.

---

### 38. What is Canary deployment?

Deploys new version to small percentage of users.

Gradually increases traffic if stable.

📌 Reduces risk of failure in production.

---

### 39. Rollback strategy in CI/CD

Common rollback methods:

* Re-deploy previous version
* Switch traffic back (Blue-Green)
* Roll back Kubernetes deployment

```bash
kubectl rollout undo deployment app
```

📌 Rollback must be fast and automated.

---

### 40. How do you integrate Jenkins with Docker?

Steps:

1. Jenkins pulls code
2. Builds Docker image
3. Pushes to registry
4. Deploys container

📌 Example:

```bash
docker build -t app .
docker push app
```

📌 Enables consistent environments.

🔥 **Interview Power Answer (Memorize)**

> “I use Jenkins pipelines with Docker for CI/CD, manage secrets securely, and implement Blue-Green or Canary deployments to ensure zero downtime.”

---

## Questions 41–55 (Docker)

### 41. What is Docker?

Docker is a containerization platform that allows applications to run in isolated environments called containers.

📌 Benefits:

* Lightweight
* Fast startup
* Consistent across environments

---

### 42. Difference between Docker image and container

| Docker Image              | Docker Container     |
| ------------------------- | -------------------- |
| Blueprint/template        | Running instance     |
| Read-only                 | Read-write           |
| Used to create containers | Executes application |

📌 Image → Container

---

### 43. What is a Dockerfile?

A Dockerfile is a text file containing instructions to build a Docker image.

📌 Common instructions:

* FROM
* RUN
* COPY
* CMD
* ENTRYPOINT

---

### 44. Explain Dockerfile instructions

| Instruction | Purpose          |
| ----------- | ---------------- |
| FROM        | Base image       |
| RUN         | Execute commands |
| COPY        | Copy files       |
| ADD         | Copy + extract   |
| EXPOSE      | Expose ports     |
| CMD         | Default command  |
| ENTRYPOINT  | Main command     |

📌 Dockerfile builds image layer by layer.

---

### 45. What is Docker Compose?

Docker Compose is used to run multi-container applications using a `docker-compose.yml` file.

📌 Example use case:

* App + Database

```bash
docker-compose up
```

---

### 46. Difference between CMD and ENTRYPOINT

| CMD                 | ENTRYPOINT            |
| ------------------- | --------------------- |
| Default command     | Fixed main command    |
| Can be overridden   | Not easily overridden |
| Used for parameters | Used for execution    |

📌 Best practice: use **ENTRYPOINT + CMD**

---

### 47. What is a Docker volume?

Volumes store persistent data outside containers.

Data survives container deletion.

📌 Example:

```bash
docker volume create myvol
```

---

### 48. Bind mount vs Volume

| Bind Mount           | Volume              |
| -------------------- | ------------------- |
| Host-specific path   | Managed by Docker   |
| Less portable        | Highly portable     |
| Good for development | Best for production |

---

### 49. How do you reduce Docker image size?

Best practices:

* Use alpine images
* Multi-stage builds
* Remove unnecessary packages
* Minimize layers

📌 Smaller images = faster deployment.

---

### 50. What is a multi-stage build?

Allows multiple `FROM` statements.

Keeps only required artifacts in final image.

📌 Reduces image size significantly.

---

### 51. How do you expose ports in Docker?

📌 In Dockerfile:

```bash
EXPOSE 8080
```

📌 While running:

```bash
docker run -p 8080:80 app
```

---

### 52. Docker networking types

| Network | Use Case         |
| ------- | ---------------- |
| bridge  | Default          |
| host    | High performance |
| none    | No network       |
| overlay | Multi-host       |

📌 Bridge is most common.

---

### 53. How do you troubleshoot a failing container?

Steps:

```bash
docker ps -a
docker logs container_id
docker exec -it container_id sh
```

📌 Check logs first.

---

### 54. What is Docker registry?

A Docker registry stores Docker images.

Examples:

* Docker Hub
* AWS ECR
* GitHub Container Registry

📌 Used for image distribution.

---

### 55. Difference between Docker and Virtual Machine

| Docker              | VM             |
| ------------------- | -------------- |
| Lightweight         | Heavy          |
| Shares host OS      | Full OS        |
| Fast startup        | Slow startup   |
| Less resource usage | More resources |

📌 Containers are preferred for microservices.

🔥 **Interview Power Statement**

> “I use Docker with multi-stage builds, volumes for persistence, Docker Compose for multi-container apps, and integrate Docker with Jenkins for CI/CD.”

---

# Kubernetes Interview-Ready Notes

## Questions 56–75 (Kubernetes)

### 56. What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to deploy, scale, and manage containerized applications automatically.

📌 It handles:

* Scheduling
* Scaling
* Self-healing
* Load balancing

---

### 57. What problem does Kubernetes solve?

Kubernetes solves:

* Manual container management
* Scaling issues
* High availability
* Self-healing of failed containers

📌 Ideal for microservices architecture.

---

### 58. Explain Kubernetes architecture

Main components:

#### Control Plane

* **API Server** – Entry point
* **Scheduler** – Assigns pods
* **Controller Manager** – Maintains desired state
* **etcd** – Stores cluster data

#### Worker Node

* kubelet
* kube-proxy
* Container runtime

---

### 59. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

It can contain one or more containers sharing:

* Network
* Storage

📌 Containers in a pod communicate via localhost.

---

### 60. Difference between Pod and Deployment

| Pod             | Deployment            |
| --------------- | --------------------- |
| Single instance | Manages multiple pods |
| No self-healing | Self-healing          |
| Manual scaling  | Auto scaling          |

📌 Always use Deployments, not standalone Pods.

---

### 61. What is a ReplicaSet?

Ensures a specified number of pod replicas are running.

Used internally by Deployments.

📌 You rarely create ReplicaSets directly.

---

### 62. What is a Service?

A Service exposes pods using a stable IP and DNS name.

Solves the problem of dynamic pod IPs.

---

### 63. Types of Kubernetes Services

| Type         | Use                      |
| ------------ | ------------------------ |
| ClusterIP    | Internal access          |
| NodePort     | External via node IP     |
| LoadBalancer | Cloud load balancer      |
| ExternalName | External service mapping |

📌 ClusterIP is default.

---

### 64. What is Ingress?

Ingress manages external HTTP/HTTPS access.

Works with Ingress Controller (e.g., NGINX).

📌 Provides:

* Path-based routing
* TLS termination

---

### 65. ConfigMap vs Secret

| ConfigMap          | Secret          |
| ------------------ | --------------- |
| Non-sensitive data | Sensitive data  |
| Plain text         | Base64 encoded  |
| Config values      | Passwords, keys |

📌 Secrets should be encrypted at rest.

---

### 66. What is a Namespace?

Namespace provides logical isolation within a cluster.

Useful for multi-team or multi-environment setups.

📌 Examples:

* dev
* test
* prod

---

### 67. What is StatefulSet?

Used for stateful applications.

Provides:

* Stable network identity
* Persistent storage

📌 Example: Databases

---

### 68. What is DaemonSet?

Ensures one pod runs on every node.

Used for:

* Logging
* Monitoring
* Security agents

📌 Example: Fluentd

---

### 69. How does Kubernetes handle scaling?

* Horizontal scaling – Increase pods
* Vertical scaling – Increase resources

📌 Managed automatically using HPA.

---

### 70. What is HPA?

**Horizontal Pod Autoscaler**

Scales pods based on:

* CPU
* Memory
* Custom metrics

📌 Example:

```bash
kubectl autoscale deployment app --cpu-percent=50
```

---

### 71. What is etcd?

etcd is a distributed key-value store.

Stores cluster state and configuration.

📌 Backup etcd regularly.

---

### 72. How do you troubleshoot a pod crash?

Steps:

```bash
kubectl get pods
kubectl describe pod pod-name
kubectl logs pod-name
```

📌 Check events and logs.

---

### 73. What is CrashLoopBackOff?

Indicates a container keeps crashing repeatedly.

Causes:

* App error
* Wrong config
* Missing dependency

📌 Fix root cause and redeploy.

---

### 74. Rolling update vs Recreate strategy

| Rolling Update      | Recreate          |
| ------------------- | ----------------- |
| Zero downtime       | Downtime          |
| Gradual replacement | All pods replaced |
| Default             | Rarely used       |

📌 Rolling update is preferred.

---

### 75. How do you secure Kubernetes?

Best practices:

* RBAC
* Network Policies
* Secrets encryption
* Image scanning
* TLS communication

📌 Security is a shared responsibility.

🔥 **Interview Power Answer**

> “I use Kubernetes Deployments with Services and Ingress, manage configs via ConfigMaps and Secrets, enable HPA for scaling, and follow RBAC and network policies for security.”

---

# AWS, Terraform, Monitoring & Security Interview-Ready Notes

## Questions 76–90 (AWS / Cloud)

### 76. What is cloud computing?

Cloud computing is the on-demand delivery of computing resources (servers, storage, networking, databases) over the internet with pay-as-you-go pricing.

📌 Benefits:

* Scalability
* Cost efficiency
* High availability

---

### 77. IaaS vs PaaS vs SaaS

| Model | Description    | Example           |
| ----- | -------------- | ----------------- |
| IaaS  | Infrastructure | EC2               |
| PaaS  | Platform       | Elastic Beanstalk |
| SaaS  | Software       | Gmail             |

📌 More control → IaaS
📌 Less control → SaaS

---

### 78. Explain EC2 instance types

EC2 instances are categorized by workload:

* General purpose – t3, t2
* Compute optimized – c5
* Memory optimized – r5
* Storage optimized – i3

📌 Choose based on application needs.

---

### 79. What is AMI?

AMI (Amazon Machine Image) is a template used to launch EC2 instances.

Contains:

* OS
* Software
* Configurations

📌 Enables quick and consistent deployments.

---

### 80. What is VPC?

VPC (Virtual Private Cloud) is a logically isolated network in AWS.

You control:

* IP range
* Subnets
* Routing
* Security

📌 Foundation of AWS networking.

---

### 81. Public vs Private subnet

| Public Subnet         | Private Subnet     |
| --------------------- | ------------------ |
| Has internet access   | No direct internet |
| Uses Internet Gateway | Uses NAT Gateway   |
| Hosts ALB / Bastion   | Hosts databases    |

📌 Security best practice: databases in private subnet.

---

### 82. Internet Gateway vs NAT Gateway

| Internet Gateway            | NAT Gateway    |
| --------------------------- | -------------- |
| Inbound & outbound internet | Outbound only  |
| Public subnet               | Private subnet |
| Free                        | Paid service   |

📌 NAT protects private instances.

---

### 83. What is a Security Group?

Security Group acts as a virtual firewall.

Controls inbound and outbound traffic.

📌 Features:

* Stateful
* Allow rules only

---

### 84. NACL vs Security Group

| NACL         | Security Group |
| ------------ | -------------- |
| Stateless    | Stateful       |
| Subnet level | Instance level |
| Allow & deny | Allow only     |

📌 Use Security Groups most of the time.

---

### 85. What is Auto Scaling Group?

Automatically adjusts EC2 instance count based on demand.

Ensures:

* High availability
* Fault tolerance

📌 Works with ELB.

---

### 86. What is ELB?

ELB (Elastic Load Balancer) distributes traffic across instances.

Types:

* ALB
* NLB
* Classic (legacy)

📌 Prevents single point of failure.

---

### 87. ALB vs NLB

| ALB                | NLB              |
| ------------------ | ---------------- |
| Layer 7            | Layer 4          |
| HTTP/HTTPS         | TCP/UDP          |
| Path-based routing | High performance |

📌 Use ALB for web apps.

---

### 88. What is S3 and its storage classes?

S3 is object storage used for storing files.

Storage classes:

* Standard
* Intelligent-Tiering
* Standard-IA
* Glacier

📌 Used for backups, static sites, logs.

---

### 89. What is IAM?

IAM (Identity and Access Management) controls who can access AWS resources.

Uses:

* Users
* Groups
* Roles
* Policies

📌 Follow least privilege principle.

---

### 90. IAM Role vs IAM User

| IAM User              | IAM Role              |
| --------------------- | --------------------- |
| Permanent credentials | Temporary credentials |
| Human access          | Service access        |
| Needs access keys     | No access keys        |

📌 Use roles for AWS services.

🔥 **Interview Power Answer**

> “I design AWS infrastructure using VPC, public and private subnets, ALB with Auto Scaling, IAM roles for security, and S3 for storage while following least-privilege access.”

---

## Questions 91–100 (Terraform + Monitoring + Security)

### 91. What is Infrastructure as Code (IaC)?

Infrastructure as Code is the practice of managing infrastructure using code instead of manual processes.

📌 Benefits:

* Automation
* Consistency
* Version control
* Easy rollback

📌 Tools: Terraform, CloudFormation, Ansible

---

### 92. What is Terraform?

Terraform is an open-source IaC tool by HashiCorp used to provision and manage infrastructure across multiple cloud providers using declarative configuration.

📌 Uses HCL (HashiCorp Configuration Language).

---

### 93. What is Terraform state file?

The Terraform state file (`terraform.tfstate`) tracks real infrastructure.

Maps Terraform configuration to actual resources.

📌 Best practice:

* Store state remotely (S3 + DynamoDB locking).

---

### 94. Difference between terraform plan and terraform apply

| terraform plan       | terraform apply           |
| -------------------- | ------------------------- |
| Shows changes        | Applies changes           |
| Safe preview         | Modifies infra            |
| No resources created | Creates/updates resources |

📌 Always run plan first.

---

### 95. How do you manage multiple environments in Terraform?

Common approaches:

* Workspaces
* Separate state files
* Environment-specific variables
* Folder structure per environment

📌 Best practice: workspaces + remote state.

---

### 96. What is monitoring in DevOps?

Monitoring tracks system health, performance, and availability in real time.

📌 Metrics include:

* CPU
* Memory
* Disk
* Network

📌 Helps detect issues early.

---

### 97. Difference between monitoring and logging

| Monitoring       | Logging       |
| ---------------- | ------------- |
| Metrics & alerts | Event records |
| Real-time        | Historical    |
| Detect issues    | Debug issues  |

📌 Both are essential.

---

### 98. What tools are used for monitoring?

Common tools:

* Prometheus
* Grafana
* CloudWatch
* Nagios
* Datadog

📌 Grafana is used for visualization.

---

### 99. What is SSL/TLS?

SSL/TLS encrypts communication between client and server.

Prevents data theft and tampering.

📌 TLS is the modern, secure version of SSL.

---

### 100. What is DNS and how does it work?

DNS converts domain names into IP addresses.

📌 Flow:

1. User enters domain
2. DNS resolver queries server
3. IP address returned
4. Browser connects to server

📌 Example: google.com → IP address

🎯 **Final Interview Power Statement (Memorize This)**

> “I use Terraform for Infrastructure as Code with remote state management, implement monitoring using Prometheus and CloudWatch, and ensure security with IAM, TLS, and least-privilege access.”
