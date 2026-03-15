# DevOps Interview Answers (Level 1 & Level 2)

---

# 🔹 LEVEL 1 – Core DevOps & Cloud Fundamentals

## 1️⃣ Can you introduce yourself?

### Structure:

* Background
* Technical Skills
* Hands-on Experience
* Current Goal

**Answer:**
I am Sanket Ajay Chopade, a DevOps enthusiast with hands-on experience in AWS, Linux, Docker, Kubernetes, Terraform, Jenkins, Git, and GitHub. I have built CI/CD pipelines, containerized applications using Docker, and deployed them to Kubernetes clusters.

I have worked on automating infrastructure using Terraform and implemented CI/CD pipelines to reduce manual deployment efforts. I am passionate about cloud automation, infrastructure as code, and building scalable systems.

Currently, I am focusing on strengthening my DevOps fundamentals and gaining real-world production-level experience.

---

## 2️⃣ What are the best practices in Ansible?

### Best Practices:

* Use Roles for modularity
* Follow proper directory structure
* Keep playbooks idempotent
* Use Ansible Vault for secrets
* Avoid hardcoding values (use variables)
* Use inventory grouping
* Enable linting (ansible-lint)
* Version control playbooks

---

## 3️⃣ What is the purpose of storing Terraform state file in Amazon S3?

### Purpose:

* Remote state storage
* Team collaboration
* State locking using DynamoDB
* Backup & versioning
* Prevent state corruption

---

## 4️⃣ Write a basic Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 5️⃣ Build Docker Image & Deploy to Kubernetes using CI/CD

### Step 1: Dockerfile

```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

### Step 2: Kubernetes Deployment Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: <dockerhub-username>/nginx-app:latest
        ports:
        - containerPort: 80
```

### Step 3: Jenkins Pipeline

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t username/nginx-app:latest .'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push username/nginx-app:latest'
      }
    }
    stage('Deploy') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
      }
    }
  }
}
```

---

## 6️⃣ Terraform configuration to create S3 bucket

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-devops-bucket-123"
}
```

---

## 7️⃣ What is Blue/Green Deployment?

Blue = current production
Green = new version
Switch traffic after testing
Zero downtime deployment

---

## 8️⃣ How does Rolling Update work?

Pods are updated gradually
Old pods terminate only after new pods become ready
Ensures availability

---

## 9️⃣ How does Canary Deployment work?

Small percentage of traffic to new version
Monitor metrics
Gradually increase traffic

---

## 🔟 What is an Ingress Controller?

Manages external access to services in Kubernetes
Provides routing, SSL termination, load balancing

---

## 1️⃣1️⃣ terraform taint & terraform import

### terraform taint

Marks resource for recreation

### terraform import

Imports existing resource into Terraform state

---

# 🔹 LEVEL 2 – Troubleshooting & Architecture

## 1️⃣ Pod in CrashLoopBackOff

### Steps:

1. kubectl describe pod
2. kubectl logs pod-name
3. Check resource limits
4. Check environment variables
5. Check liveness/readiness probes

---

## 2️⃣ Migrate Jenkins to GitHub Actions

* Analyze pipelines
* Convert stages to YAML workflows
* Store secrets in GitHub secrets
* Use GitHub runners
* Test workflow

---

## 3️⃣ Ran terraform destroy accidentally

* Stop execution immediately
* Check remote state
* Recover from S3 versioning
* Restore infrastructure using apply

---

## 4️⃣ Kubernetes Hands-on Experience

* Deployed apps
* Created deployments, services
* Configured HPA
* Used Ingress
* Managed namespaces

---

## 5️⃣ Kubernetes Security Best Practices

* RBAC
* Network policies
* Pod security policies
* Secrets management
* Image scanning

---

## 6️⃣ Where do you store secrets?

* AWS Secrets Manager
* Kubernetes Secrets
* HashiCorp Vault

---

## 7️⃣ Why moving from current organization?

Looking for growth, challenging projects, cloud-native exposure, and scalable production systems.

---

# ✅ End of Document
