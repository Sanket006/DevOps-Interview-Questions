# DevOps Interview Questions – Detailed Structured Answers

---

## 1️⃣ Tell Me About Yourself

### Introduction

I am Sanket Ajay Chopade, a DevOps Engineer with hands-on experience in Linux, AWS, Docker, Kubernetes, Terraform, Jenkins, Git, and GitHub.

### Technical Expertise

* Cloud: AWS (EC2, S3, IAM, VPC)
* CI/CD: Jenkins, GitHub Actions
* Containers: Docker, Kubernetes
* IaC: Terraform
* Version Control: Git & GitHub
* Monitoring: Prometheus & Grafana

### Practical Experience

* Built CI/CD pipelines using Jenkins
* Automated deployments on AWS EC2
* Created Docker images and deployed containers
* Managed Kubernetes deployments and scaling

### Career Objective

I aim to work in a challenging DevOps role where I can automate infrastructure, improve deployment efficiency, and ensure system reliability.

---

## 2️⃣ What is Git and GitHub?

### Git

Git is a distributed version control system used to track changes in source code.

#### Key Features:

* Branching & merging
* Distributed architecture
* Version history tracking

### GitHub

GitHub is a cloud-based platform that hosts Git repositories and provides collaboration features.

#### Key Features:

* Pull requests
* Code reviews
* Access control
* GitHub Actions (CI/CD)

---

## 3️⃣ What is Git Rebase?

### Definition

Git Rebase is used to integrate changes from one branch into another by rewriting commit history.

### Why Use Rebase?

* Keeps commit history clean
* Avoids unnecessary merge commits

### Example:

```
git checkout feature-branch
git rebase main
```

---

## 4️⃣ How to Give Access to a Private Repository?

### Steps:

1. Go to repository settings
2. Click "Collaborators and Teams"
3. Add the user by username or email
4. Assign appropriate role (Read/Write/Admin)

---

## 5️⃣ Explain CI/CD

### Continuous Integration (CI)

Developers frequently merge code into a shared repository, triggering automated builds and tests.

### Continuous Delivery (CD)

Code is automatically prepared for release but requires manual approval.

### Continuous Deployment

Code is automatically deployed to production without manual intervention.

---

## 6️⃣ What is Jenkins?

### Definition

Jenkins is an open-source automation server used to build, test, and deploy applications.

### Why Used?

* Automates CI/CD pipelines
* Supports plugins (Docker, SonarQube, etc.)
* Distributed builds via agents

---

## 7️⃣ SonarQube, Trivy, Nexus, Artifacts

### SonarQube

Code quality and static analysis tool.

### Trivy

Container image vulnerability scanner.

### Nexus

Artifact repository manager.

### Artifacts

Build outputs like JAR, WAR, Docker images.

---

## 8️⃣ Jenkinsfile & Pipeline Stages

### Jenkinsfile

A Groovy-based file defining pipeline as code.

### Common Stages:

1. Checkout
2. Build
3. Test
4. Code Analysis
5. Security Scan
6. Build Docker Image
7. Push to Registry
8. Deploy

---

## 9️⃣ VM vs Container

### Virtual Machine

* Uses Hypervisor
* Full OS
* Heavyweight

### Container

* Uses container runtime
* Shares host OS kernel
* Lightweight

---

## 🔟 Dockerfile

### Definition

A Dockerfile is a text file with instructions to build a Docker image.

### Example:

```
FROM nginx:latest
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 1️⃣1️⃣ Build Image & Run Container

### Commands:

```
docker build -t myapp .
docker run -d -p 8080:80 myapp
```

---

## 1️⃣2️⃣ Access Container & Logs

```
docker exec -it container_id /bin/bash
docker logs container_id
```

---

## 1️⃣3️⃣ Jenkins Master & Agent

### Master

* Manages pipelines
* Schedules builds

### Agent

* Executes jobs

---

## 1️⃣4️⃣ Deploy manifest.yaml in Kubernetes

```
kubectl apply -f manifest.yaml
```

---

## 1️⃣5️⃣ Increase Replicas

```
kubectl scale deployment myapp --replicas=3
```

---

## 1️⃣6️⃣ Setup Prometheus & Grafana

### Steps:

* Install via Helm
* Configure service monitoring
* Expose via LoadBalancer/NodePort

---

## 1️⃣7️⃣ Bash Script (1–3)

```
#!/bin/bash
for i in 1 2 3
do
  echo $i
done
```

---

## 1️⃣8️⃣ Troubleshoot Memory Spikes

### Steps:

1. Check memory usage (`top`, `htop`)
2. Identify high memory processes
3. Check logs
4. Restart service if needed
5. Scale vertically or horizontally
6. Set alerts in monitoring tools

---

# End of Document
