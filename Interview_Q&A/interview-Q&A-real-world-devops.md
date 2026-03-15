# 🚀 DevOps Engineer Interview Preparation Guide (0–2 Years)

# Round 1 DevOps Technical Screening questions.

This README contains structured, production-ready answers for common DevOps technical screening questions. The answers are written in a clear, real-world format suitable for interviews.

---

# 1️⃣ Explain your end-to-end CI/CD workflow. Which type of Jenkins pipeline do you use and why?

## 🔹 Overview

In my projects, I implemented a complete CI/CD pipeline using:

GitHub → Jenkins → Docker → Kubernetes (EKS) → Monitoring (Datadog/Prometheus)

---

## 🔹 Step-by-Step Workflow

### 1. Code Commit Phase

* Developers push code to GitHub (main/develop branch)
* GitHub Webhook triggers Jenkins automatically

---

### 2. Continuous Integration (CI)

**Jenkins Stages:**

* Checkout code from Git
* Build application (Maven/npm)
* Run unit tests
* Run static code analysis (SonarQube)
* Build Docker image
* Tag image with build number
* Push image to DockerHub / AWS ECR

---

### 3. Continuous Deployment (CD)

* Update Helm values.yaml with new image tag
* Deploy to Kubernetes using:

```bash
helm upgrade --install app-release ./chart
```

---

### 4. Post Deployment Verification
* Perform health checks
* Monitoring using Prometheus/Grafana or Datadog
* Send Slack/Email notifications

---

## 🔹 Jenkins Pipeline Type Used

I primarily use **Declarative Pipeline** because:

* Clean and structured syntax
* Easier maintenance
* Built-in error handling
* Better readability for teams

I use **Scripted Pipeline** only for complex dynamic logic.

---

# 2️⃣ Difference between Declarative and Scripted Jenkins pipelines

| Feature        | Declarative         | Scripted         |
| -------------- | ------------------- | ---------------- |
| Syntax         | Structured          | Groovy-based     |
| Readability    | High                | Moderate         |
| Flexibility    | Limited             | Very High        |
| Error Handling | Built-in post block | Manual try/catch |
| Recommended    | Standard CI/CD      | Complex logic    |

In production, I prefer Declarative unless advanced logic is required.

---

# 3️⃣ How do you define and trigger Jenkins pipelines?

## 🔹 Definition

Pipeline is defined inside a `Jenkinsfile` stored in Git.

## 🔹 Trigger Methods

* GitHub Webhook (Preferred)
* Poll SCM
* Manual Trigger
* CRON Scheduling
* Upstream Job Trigger

Webhook is preferred because it provides real-time automation.

---

# 4️⃣ What are Jenkins Shared Libraries? How are they structured and used?

## 🔹 Purpose

Shared Libraries allow reusable pipeline code across multiple projects.

## 🔹 Structure

```
(root)
 ├── vars/
 ├── src/
 └── resources/
```

## 🔹 Usage in Jenkinsfile

```groovy
@Library('my-shared-lib') _
```

## 🔹 Benefits

* Reusability
* Standardization
* Clean Jenkinsfile
* Enterprise best practice

---

# 5️⃣ What types of applications have you deployed using Jenkins pipelines?

## 🔹 Application I have deployed :

* Node.js applications
* Spring Boot applications
* React frontend
* Microservices architecture
* Containerized apps on Kubernetes (EKS)

## 🔹 Deployment targets:

1. EC2

2. EKS

3. ECS

4. On-prem Kubernetes

---

# 6️⃣ Which deployment tools have you used?

* Docker (Containerization)
* Kubernetes (Orchestration)
* Helm (Package management)
* Terraform (Infrastructure provisioning)
* AWS EKS
* EC2

---

# 7️⃣ If a Jenkins pipeline runs but the build does not trigger, what could be the reasons?

* Webhook misconfiguration
* Incorrect Git credentials
* Poll SCM not configured
* Branch filter mismatch
* Missing Jenkinsfile
* Firewall blocking webhook
* Token mismatch
* Multibranch indexing issue

## 🔹 Troubleshooting Steps

* Check GitHub webhook delivery logs
* Verify Jenkins console output
* Validate credentials
* Test webhook manually

---

# 8️⃣ What is a webhook, and how is it used in CI/CD?

A webhook is an HTTP callback that automatically triggers an event when code is pushed.

GitHub → Sends POST request → Jenkins → Pipeline Triggered

Benefits:

* Real-time CI
* Automation
* Reduced manual effort

---

# 9️⃣ How do you create and manage Kubernetes clusters using Terraform?

## 🔹 Steps (EKS Example)

1. Write Terraform code for:

   * VPC
   * Subnets
   * IAM Roles
   * EKS Cluster
   * Node Groups

2. Execute:

```bash
terraform init
terraform plan
terraform apply
```

3. Configure kubectl:

```bash
aws eks update-kubeconfig --region ap-south-1 --name cluster-name
```

## 🔹 Best Practices

* Use remote backend (S3 + DynamoDB)
* Version control Terraform code
* Use reusable modules

---

# 🔟 What is the role of the master (control plane) and worker nodes?

## 🔹 Control Plane Components

* API Server
* Scheduler
* Controller Manager
* etcd

Responsible for cluster management.

## 🔹 Worker Node Components

* kubelet
* kube-proxy
* Container runtime

Responsible for running Pods.

---

# 1️⃣1️⃣ What common kubernetes errors have you faced (CrashLoopBackOff, ImagePullBackOff), and how did you resolve them?

## 🔹 CrashLoopBackOff

**Causes:**

* Application crash
* Wrong environment variables
* Database connectivity issue

**Debug:**

```bash
kubectl logs pod-name
kubectl describe pod pod-name
```

---

## 🔹 ImagePullBackOff

**Causes:**

* Incorrect image tag
* Private registry authentication issue

**Fix:**

* Verify image name/tag
* Create imagePullSecret

---

# 1️⃣2️⃣ How do you access a running pod and define Kubernetes objects?

```bash
kubectl exec -it pod-name -- /bin/sh
```

View logs:

```bash
kubectl logs pod-name
```

---

## 🔹 Defining Kubernetes Objects

Example structure:

```yaml
apiVersion:
kind:
metadata:
spec:
```

Apply configuration:

```bash
kubectl apply -f deployment.yaml
```

---

# 1️⃣3️⃣ What are the stages involved in building a Docker image?

1. Write Dockerfile
2. Choose base image
3. Install dependencies
4. Copy application code
5. Expose port
6. Define CMD/ENTRYPOINT
7. Build image

```bash
docker build -t app:v1 .
```

---

# 1️⃣4️⃣ Difference between ENTRYPOINT and CMD in Docker

| CMD                | ENTRYPOINT               |
| ------------------ | ------------------------ |
| Default command    | Main executable          |
| Can be overridden  | Hard to override         |
| Used for arguments | Used for primary process |

Example:

```dockerfile
ENTRYPOINT ["java"]
CMD ["-jar", "app.jar"]
```

---

# 1️⃣5️⃣ How do you connect & manage services like DB, EC2, EKS, ECS?

## 🔹 EC2

* SSH access
* Security Groups
* IAM roles

## 🔹 EKS

* kubectl
* IAM + RBAC

## 🔹 ECS

* Task definitions
* Load balancer integration

## 🔹 Databases

* Security Groups
* AWS Secrets Manager
* Environment variables

---

# Round 2 DevOps Technical Screening questions.

---

# 1️⃣ Which branching strategy do you follow (GitFlow / Trunk-based), and how do you avoid breaking the release branch?

## ✅ Answer

In production environments, I prefer **Trunk-Based Development for fast-moving microservices**, and **GitFlow for enterprise release-driven systems**.

### 🔹 Trunk-Based Strategy (Preferred for CI/CD maturity)

* Developers create short-lived feature branches.
* Merge into `main` within 1–2 days.
* Use feature flags to avoid incomplete feature exposure.
* Every merge triggers full CI pipeline.

### 🔹 GitFlow (Used in structured release environments)

* `main` → production-ready
* `develop` → integration branch
* `feature/*` → feature development
* `release/*` → pre-production testing
* `hotfix/*` → urgent production fixes

### 🛡 How I Avoid Breaking Release Branch

* Mandatory Pull Request reviews (minimum 2 approvals)
* CI pipeline gating (build + unit tests + SAST must pass)
* Branch protection rules
* Semantic versioning
* Automated regression tests before merging into release branch
* Blue-Green deployment validation before final promotion

---

# 2️⃣ If a critical bug is found in production, what is your approach to fixing it?

## ✅ Answer (Production Incident Flow)

### 🚨 Step 1: Incident Response

* Create P1 incident in monitoring tool (Datadog/CloudWatch)
* Alert DevOps + Engineering team
* Initiate incident bridge call

### 🔍 Step 2: Immediate Mitigation

* Rollback to last stable version (via Helm rollback / Jenkins pipeline)
* Or enable feature flag disablement
* Scale up replicas if performance-related

### 🧪 Step 3: Root Cause Analysis (RCA)

* Check logs (ELK/CloudWatch)
* Review recent commits
* Check metrics (CPU, memory, latency)

### 🔧 Step 4: Hotfix Deployment

* Create `hotfix/*` branch
* Patch issue
* Run pipeline (build → scan → test)
* Deploy to staging
* Deploy to production via controlled rollout

### 📄 Step 5: Postmortem

* Document RCA
* Define preventive action
* Improve monitoring/alert thresholds

---

# 3️⃣ Explain your complete deployment workflow from code commit to production.

## ✅ Answer (Enterprise CI/CD Flow)

1. Developer pushes code to GitHub
2. Webhook triggers Jenkins pipeline
3. Pipeline stages:

   * Checkout Code
   * Install Dependencies
   * Run Unit Tests
   * SonarQube Code Scan
   * Build Docker Image
   * Trivy Image Scan
   * Push to ECR
   * Update Helm Chart
   * Deploy to Dev (EKS)
4. Automated Integration Tests
5. Manual approval (if required)
6. Deploy to Staging
7. Canary or Blue-Green deployment to Production
8. Post-deployment smoke tests
9. Monitoring validation (Datadog dashboards)

---

# 4️⃣ What stages do you define in your Jenkins pipeline to ensure code quality?

## ✅ 1.Answer

Typical Jenkins pipeline stages:

1. Code Checkout
2. Linting (ESLint / flake8)
3. Unit Testing
4. Code Coverage Check
5. SonarQube SAST Scan
6. Dependency Vulnerability Scan
7. Docker Build
8. Docker Image Scan (Trivy)
9. Artifact Push (ECR/Nexus)
10. Deployment
11. Post-deploy health check

Fail-fast strategy is implemented.

## ✅ 2.Answer

To ensure production-grade code quality, I design a multi-stage Jenkins pipeline that integrates CI, security, and compliance checks.

### 1️⃣ Code Checkout Stage

* Pull code from Git (GitHub/GitLab/Bitbucket)
* Validate branch strategy (feature, develop, main)

### 2️⃣ Build Stage

* Compile application (Maven/Gradle/npm)
* Fail fast if build breaks

### 3️⃣ Static Code Analysis (SAST)

* SonarQube integration
* Enforce quality gates (coverage %, code smells, bugs)

### 4️⃣ Unit Testing Stage

* Run automated unit tests
* Generate coverage reports (Jacoco)
* Enforce minimum coverage threshold (e.g., 80%)

### 5️⃣ Dependency Scanning

* OWASP Dependency-Check / Snyk
* Block pipeline on high/critical vulnerabilities

### 6️⃣ Docker Build Stage

* Build Docker image with version tagging
* Use minimal base images (Alpine, Distroless)

### 7️⃣ Docker Image Scanning

* Trivy / Anchore scanning
* Fail pipeline if CVSS score exceeds threshold

### 8️⃣ Artifact Publishing

* Push artifact to Nexus / Artifactory / ECR
* Immutable tagging (version + commit SHA)

### 9️⃣ Integration Testing

* Deploy to staging namespace
* Run API/Integration tests

### 🔟 Manual Approval (For Production)

* Required approval before prod deploy
* Role-based access control

### 1️⃣1️⃣ Deployment Stage

* Deploy using Helm / Kubernetes manifests
* Blue-Green or Canary strategy

### 1️⃣2️⃣ Post-Deployment Validation

* Health checks
* Smoke tests
* Monitoring alerts verification

---

# 5️⃣ How do you design and reuse Jenkins Shared Libraries?

## ✅ Answer

Jenkins Shared Libraries help standardize CI/CD across multiple repositories.

### 📌 Structure:

* vars/ → Global pipeline functions
* src/ → Reusable Groovy classes
* resources/ → Static files/templates

### 📌 Design Approach:

* Create reusable functions (buildApp(), scanImage(), deployHelm())
* Parameterize environment (dev/stage/prod)
* Follow DRY principle
* Version control the library

### 📌 Usage in Jenkinsfile:

```
@Library('shared-lib@v1.0') _
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                buildApp()
            }
        }
    }
}
```

### Benefits:

* Centralized logic
* Easier maintenance
* Reduced duplication
* Standardizes CI process
* Version-controlled library

---

# 6️⃣ How do you perform Docker image vulnerability scanning during build time and at the registry level?

## ✅ Answer

### 🔍 Build-Time Scanning:

* Integrate Trivy in Jenkins pipeline
* Scan before pushing image
* Fail pipeline on HIGH/CRITICAL CVEs

Example:

```
trivy image --severity HIGH,CRITICAL myapp:1.0
```

### 🔍 Registry-Level Scanning:

* Enable ECR image scanning
* Use Harbor with built-in scanning
* Continuous monitoring for new CVEs
* Use AWS Inspector integration
* Periodic scanning jobs

### Best Practices:

* Use minimal base images
* Regular base image updates
* Multi-stage builds

---

# 7️⃣ Which security tools or plugins have you used for image scanning?

## ✅ Answer

I have used:

* Trivy
* Snyk
* Clair
* OWASP Dependency Check
* Anchore
* AWS ECR native scanning
* SonarQube (SAST)
* Aqua Security

In Jenkins:

* Trivy CLI integration
* Snyk plugin
* SonarQube Scanner plugin

---

# 8️⃣ How do you pass environment variables during Docker build and runtime?

## ✅ Answer

### 🔹 During Build Time:

Use ARG

```
ARG APP_VERSION
ENV VERSION=$APP_VERSION
```

Build command:

```
docker build --build-arg APP_VERSION=1.0 .
```

### 🔹 During Runtime:

Using -e flag:

```
docker run -e DB_HOST=prod-db myapp
```

Using .env file:

```
docker run --env-file .env myapp
```

In Kubernetes:

* ConfigMaps
* Secrets
* Environment variables in Deployment YAML

---

# 9️⃣ How do you establish secure connections with databases?

## ✅ Answer

### Security Measures:

* Use SSL/TLS encryption
* Use IAM-based authentication (AWS RDS)
* Store credentials in AWS Secrets Manager / Kubernetes Secrets
* Rotate passwords automatically
* Restrict DB access via Security Groups
* Enable encryption at rest

### In Kubernetes:

* Mount secrets as environment variables or volumes
* Network policies to restrict pod communication


---

# 🔟 How do you authenticate to EKS clusters and securely manage secrets?

## ✅ Answer

### Authentication:

* Use IAM roles with aws-auth ConfigMap
* IAM Roles for Service Accounts (IRSA)
* kubectl via AWS CLI with MFA

### Secret Management:

* AWS Secrets Manager
* Kubernetes Secrets (Base64 encoded)
* Encrypt secrets at rest (KMS)
* HashiCorp Vault

Best Practice:

* Avoid hardcoding secrets
* Use RBAC
* Enable audit logging

---

# 1️⃣1️⃣ How do you create and deploy AWS Lambda functions?

## ✅ Answer

### Creation Methods:

* AWS Console
* AWS CLI
* Terraform
* Serverless Framework

### Deployment Steps:

1. Package code (zip or container image)
2. Upload to Lambda
3. Attach IAM role
4. Configure trigger (API Gateway, S3, EventBridge)
5. Monitor via CloudWatch

---

# 1️⃣2️⃣ What are the different ways to push artifacts to AWS Lambda?

## ✅ Answer

1. Zip upload via Console
2. AWS CLI (update-function-code)
3. CI/CD pipeline (Jenkins/GitHub Actions)
4. S3 bucket upload
5. Container image via ECR
6. Infrastructure as Code (Terraform)

---

# 1️⃣3️⃣ What is email signing and Helm chart signing? Which tools are used?

## ✅ Answer

### 📧 Email Signing:

* Uses DKIM, SPF, DMARC
* Ensures authenticity and integrity
* Tools: DKIM keys, Postfix, AWS SES

### ⛵ Helm Chart Signing:

* Ensures integrity of Helm packages
* Uses GPG keys

Command:

```
helm package --sign --key <key-name>
```

Verification:

```
helm verify mychart-1.0.0.tgz
```

Tools:

* GnuPG (GPG)
* Cosign (modern signing)
* Notary

---

# 💡 Key Takeaway

Interviewers expect:

* Production-level troubleshooting mindset
* Secure CI/CD implementation
* Kubernetes & AWS expertise
* DevSecOps awareness
* Monitoring & Observability integration

# 🎯 Final Interview Tips

* Explain answers in workflow format
* Mention real tools (AWS, Docker, EKS, Terraform)
* Demonstrate troubleshooting mindset
* Use production terminology
* Quantify impact if possible

---

⭐ This document is optimized for DevOps Engineer (0–2 years) technical screening rounds. If this helped you, consider using it for structured interview preparation and portfolio showcase.
