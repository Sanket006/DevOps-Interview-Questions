# 🚀 Worldpay Interview Prep — AWS DevOps Engineer (Python & Terraform)
> **Role:** Software Engineer I — CI/CD, AWS, Docker | Platform Design & Engineering (Sales & Boarding)
> **Candidate:** Sanket Ajay Chopade | Pune, India
> **Team Context:** Sales & Boarding (S&B) — targeting "10-min onboarding" via Boarding Gateway & Boarding Choreography Service (BCS)

---

## 📋 Table of Contents
- [HR / Behavioral Questions](#hr--behavioral-questions)
- [Technical Concept Questions](#technical-concept-questions)
- [Troubleshooting Questions](#troubleshooting-questions)
- [Project Walkthrough Questions](#project-walkthrough-questions)
- [Scenario-Based Questions](#scenario-based-questions)
- [Why / Decision Questions](#why--decision-questions)
- [Failure / Mistake Questions](#failure--mistake-questions)
- [Strengths & Weaknesses](#strengths--weaknesses)
- [Leadership & Conflict Questions](#leadership--conflict-questions)
- [Time Pressure / Estimation Questions](#time-pressure--estimation-questions)

---

## HR / Behavioral Questions

### ❓ Tell me about yourself.

> **Method:** Value Pitch — Strength → Example → Impact

I'm a DevOps Engineer with hands-on production experience in CI/CD automation, AWS infrastructure provisioning, and Kubernetes-based orchestration. At Hisan Labs, I maintained and optimized Jenkins pipelines that cut deployment time from 45 minutes to 10 minutes — a 77% reduction — while increasing release frequency 3x. I work across the full DevOps lifecycle: from writing modular Terraform for EKS and RDS provisioning, to building observability stacks using Prometheus, Grafana, and Datadog that reduced MTTR by 30%. I'm joining Worldpay because the S&B team's "10-min onboarding" ambition maps directly to the kind of automation problems I've been solving — and at a scale that genuinely challenges me.

---

### ❓ Why Worldpay?

> **Method:** Compare & Justify — Problem → Options → Chosen Solution → Trade-offs

Worldpay processes $2.2 trillion annually across 146 countries and is actively rebuilding its merchant onboarding workflow. That's not a greenfield startup problem — it's a high-stakes, high-complexity distributed systems challenge with real regulatory and reliability constraints. The Sales & Boarding team is building Boarding Gateway as a centralized entry point and BCS as an end-to-end workflow orchestrator. That kind of architecture — event-driven, multi-service, tolerance to partial failures — is exactly where my Terraform + EKS + CI/CD experience is directly applicable. I'm not choosing Worldpay because it's a big name; I'm choosing it because the engineering problem is specific, hard, and consequential.

---

### ❓ Where do you see yourself in 3 years?

> **Method:** Learning Framework — What → Why → How → Outcome

In 3 years, I want to own end-to-end infrastructure design for a production payment system — not just pipeline maintenance, but architecture decisions: multi-region failover strategy, DR planning, cost-optimized scaling. I'm pursuing AWS Solutions Architect certification to formalize system-design thinking beyond execution. At Worldpay, the scale of 130M+ daily transactions and 225 markets provides exposure to production problems that most engineers never see. By year 3, I want to be the person who's called when a deployment breaks in a market we haven't tested before.

---

## Technical Concept Questions

### ❓ Explain CI/CD and how you've implemented it.

> **Method:** Concept Explanation — Definition → How it Works → Example

**Definition:** CI/CD (Continuous Integration / Continuous Deployment) is an automated pipeline that moves code from a developer's commit to a production environment through standardized build, test, and deploy stages — eliminating manual handoffs and reducing deployment risk.

**How it works:**
```
Developer Push → SCM Trigger → Build → Test → Package → Deploy → Monitor
    (GitHub)       (Webhook)   (Maven)  (SQ)  (Docker)   (K8s)   (Prometheus)
```

**My Implementation at Hisan Labs:**

```groovy
// Jenkinsfile — simplified pipeline structure
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Test & Static Analysis') {
            steps {
                sh 'mvn test'
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Docker Build & Scan') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
                sh 'trivy image --exit-code 1 --severity CRITICAL myapp:${BUILD_NUMBER}'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh '''
                    aws eks update-kubeconfig --region ap-south-1 --name my-cluster
                    kubectl set image deployment/myapp app=myapp:${BUILD_NUMBER} --record
                    kubectl rollout status deployment/myapp
                '''
            }
        }
    }
}
```

**Result:** Deployment time dropped from 45 min → 10 min; release frequency increased 3x.

---

### ❓ What is Terraform and how does Infrastructure as Code work?

> **Method:** Concept Explanation — Definition → How it Works → Example

**Definition:** Terraform is a declarative IaC tool by HashiCorp that provisions cloud infrastructure through code. You describe the *desired state*; Terraform determines the diff and applies only the changes needed.

**How it works:**
```
.tf files → terraform init → terraform plan → terraform apply → State file
```

**Example — Modular EKS provisioning I used at Hisan Labs:**

```hcl
# modules/eks/main.tf
resource "aws_eks_cluster" "main" {
  name     = var.cluster_name
  role_arn = aws_iam_role.eks_role.arn
  version  = "1.28"

  vpc_config {
    subnet_ids         = var.subnet_ids
    security_group_ids = [aws_security_group.eks_sg.id]
  }

  tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

resource "aws_eks_node_group" "workers" {
  cluster_name    = aws_eks_cluster.main.name
  node_group_name = "${var.cluster_name}-workers"
  node_role_arn   = aws_iam_role.node_role.arn
  subnet_ids      = var.subnet_ids

  scaling_config {
    desired_size = var.desired_nodes
    max_size     = var.max_nodes
    min_size     = var.min_nodes
  }
}
```

```bash
# Deploy
terraform init
terraform plan -var-file="prod.tfvars"
terraform apply -auto-approve -var-file="prod.tfvars"
```

**Key benefit:** Every infra change is version-controlled in Git, peer-reviewed via PR, and fully repeatable. Manual provisioning time reduced by 40%.

---

### ❓ Explain Kubernetes and the core objects you work with.

> **Method:** Concept Explanation — Definition → How it Works → Example

**Definition:** Kubernetes is a container orchestration platform that manages the deployment, scaling, and health of containerized workloads across a cluster of nodes.

**Core objects I actively use:**

| Object | Purpose | My Use |
|---|---|---|
| `Deployment` | Manages replicas, rolling updates | All microservices in ApnaKart |
| `HPA` | Auto-scales pods on CPU/memory | Absorbs 5x peak load automatically |
| `Ingress` | Routes HTTP(S) traffic to services | Nginx Ingress for API routing |
| `ConfigMap` | Injects non-secret config | DB endpoints, feature flags |
| `Secret` | Injects sensitive config | DB credentials, API keys |
| `NetworkPolicy` | Restricts pod-to-pod traffic | Isolated payment service in Flight Reservation |
| `RBAC` | Controls API access | Enforced least-privilege for service accounts |

**Zero-downtime rolling update example:**

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: boarding-service
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0     # Never take pods down before new ones are ready
      maxSurge: 1           # One extra pod during rollout
  template:
    spec:
      containers:
      - name: boarding-service
        image: worldpay/boarding-service:v2.1.0
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
```

```bash
# Trigger rolling update
kubectl set image deployment/boarding-service \
  boarding-service=worldpay/boarding-service:v2.1.1

# Monitor rollout
kubectl rollout status deployment/boarding-service

# Rollback if needed
kubectl rollout undo deployment/boarding-service
```

---

### ❓ What is Docker and how does containerization work?

> **Method:** Concept Explanation — Definition → How it Works → Example

**Definition:** Docker packages an application and all its dependencies into a portable, isolated container image. The container runs identically across any environment — dev, staging, production.

**Multi-stage Dockerfile I used (reduced image size by 60%):**

```dockerfile
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime — slim image only
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar

# Non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```bash
# Build and push
docker build -t boarding-service:1.0.0 .
docker tag boarding-service:1.0.0 registry.worldpay.com/boarding-service:1.0.0
docker push registry.worldpay.com/boarding-service:1.0.0

# Scan before push
trivy image boarding-service:1.0.0
```

---

## Troubleshooting Questions

### ❓ A pod is in CrashLoopBackOff. How do you debug it?

> **Method:** Root Cause — Identify → Diagnose → Fix → Prevent

**Identify:**
```bash
# Step 1: Confirm state
kubectl get pods -n <namespace>

# Step 2: Get crash reason
kubectl describe pod <pod-name> -n <namespace>
# Look at: Events, Exit Code, Last State
```

**Diagnose:**
```bash
# Step 3: Inspect logs (current + previous crash)
kubectl logs <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --previous

# Step 4: Check resource constraints
kubectl top pod <pod-name> -n <namespace>
```

**Common causes and fixes:**

| Exit Code | Root Cause | Fix |
|---|---|---|
| `1` | App error / config issue | Check env vars, ConfigMaps |
| `137` | OOMKilled | Increase memory limits |
| `126` / `127` | Missing binary / wrong entrypoint | Fix Dockerfile ENTRYPOINT |
| `139` | Segfault | Check app logs, image integrity |

```bash
# Fix: Update resource limits in deployment
kubectl set resources deployment/<name> \
  --limits=memory=512Mi,cpu=500m \
  --requests=memory=256Mi,cpu=250m

# Or exec into a running pod to debug live
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh
```

**Prevent:**
- Add liveness/readiness probes to catch failures before CrashLoop escalates
- Set resource `requests` and `limits` on every container
- Gate images through Trivy in CI before they reach EKS

---

### ❓ A Jenkins pipeline build fails mid-way. How do you troubleshoot?

> **Method:** Root Cause — Identify → Diagnose → Fix → Prevent

```bash
# Step 1: Identify failing stage from console output
# Go to: Jenkins → Job → Build #N → Console Output

# Step 2: Reproduce locally if it's a script failure
bash -x deploy.sh   # -x flag traces every command

# Step 3: Check environment variables
printenv | grep -i aws
printenv | grep -i kube

# Step 4: Validate credentials and AWS connectivity
aws sts get-caller-identity
aws eks list-clusters --region ap-south-1

# Step 5: Check Kubernetes connectivity
kubectl config current-context
kubectl get nodes
```

**If SonarQube gate fails:**
```bash
# Check quality gate status directly
curl -u admin:password \
  "http://sonarqube:9000/api/qualitygates/project_status?projectKey=myapp"
```

**Prevent:** Add `post { failure { ... } }` block in Jenkinsfile to auto-notify Slack and archive logs on failure.

---

## Project Walkthrough Questions

### ❓ Walk me through your most complex project.

> **Method:** Project Walkthrough — Problem → Stack → Role → Challenges → Solution → Result

**Project: ApnaKart — Production-Grade Microservices Platform**

**Problem:** Build a realistic e-commerce platform on AWS with 10 Spring Boot microservices, production-grade CI/CD, and infrastructure that mirrors real-world patterns — database isolation, zero-downtime deploys, security hardening.

**Tech Stack:**
```
Frontend:     React
Backend:      10 Spring Boot microservices (product, order, payment, auth, etc.)
Infra:        AWS EKS, RDS (per-service), VPC, EC2, IAM, Security Groups
IaC:          Modular Terraform
CI/CD:        Jenkins (multi-stage pipeline)
Containers:   Docker (multi-stage builds)
Security:     SonarQube, Trivy, Kubernetes RBAC
```

**My Role:** Sole DevOps engineer — designed and built all infrastructure, pipelines, and Kubernetes configurations.

**Biggest Challenge:** Database-per-service isolation. With 10 services each needing their own RDS instance, naive Terraform would require 10 separate resource blocks. I solved this with a parameterized Terraform module:

```hcl
# modules/rds/main.tf
resource "aws_db_instance" "service_db" {
  identifier        = "${var.service_name}-db"
  engine            = "postgres"
  instance_class    = var.db_instance_class
  allocated_storage = var.storage_gb
  db_name           = var.db_name
  username          = var.db_user
  password          = var.db_password
  vpc_security_group_ids = [var.security_group_id]
  db_subnet_group_name   = var.subnet_group_name
  skip_final_snapshot    = false
}

# Root module calls
module "product_db" {
  source         = "./modules/rds"
  service_name   = "product"
  db_instance_class = "db.t3.micro"
  # ...
}
```

**Result:**
- Full build-to-production cycle in a single Jenkins trigger
- Zero-downtime rolling updates via Kubernetes HPA + rolling strategy
- 98% deployment success rate, 35% fewer deployment failures
- Zero critical vulnerabilities across all production deployments (Trivy gating)

---

## Scenario-Based Questions

### ❓ Production is down at 2 AM. A merchant onboarding workflow is failing. What do you do?

> **Method:** Action Plan — Clarify → Immediate Action → Investigation → Communication → Prevention

**Clarify (2 min):**
- Is it a full outage or partial? Which region? Which onboarding stage?
- Check PagerDuty/Grafana alert for initial context

**Immediate Action (5 min):**
```bash
# Check pod health in the S&B namespace
kubectl get pods -n sales-boarding
kubectl get events -n sales-boarding --sort-by='.lastTimestamp'

# Check recent deployments
kubectl rollout history deployment/boarding-gateway -n sales-boarding

# If recent bad deploy — rollback immediately
kubectl rollout undo deployment/boarding-gateway -n sales-boarding
kubectl rollout status deployment/boarding-gateway -n sales-boarding
```

**Investigation (ongoing):**
```bash
# Pull logs from failed pods
kubectl logs -l app=boarding-gateway -n sales-boarding --since=30m

# Check downstream dependencies
kubectl logs -l app=bcs -n sales-boarding --since=30m

# Check CloudWatch for Lambda / RDS errors
aws logs tail /aws/lambda/onboarding-trigger --since 30m --follow

# Check ALB target group health
aws elbv2 describe-target-health \
  --target-group-arn arn:aws:elasticloadbalancing:...
```

**Communication:**
- Post in #incidents Slack: severity, affected service, ETA for resolution, current action
- Page on-call SRE if infrastructure-level issue

**Prevention:**
- Add synthetic monitoring that simulates a full onboarding flow every 5 min
- Implement circuit breakers at BCS to prevent cascade failures if a third-party channel is down
- Enforce canary deployments before full rollout to production

---

## Why / Decision Questions

### ❓ Why Terraform over CloudFormation?

> **Method:** Compare & Justify — Problem → Options → Chosen Solution → Trade-offs

| Criteria | Terraform | CloudFormation |
|---|---|---|
| Multi-cloud | ✅ AWS + Azure + GCP | ❌ AWS only |
| State management | Explicit (`.tfstate`) | Managed by AWS |
| Module reuse | Mature registry | Limited |
| Syntax | HCL — readable | JSON/YAML — verbose |
| Drift detection | `terraform plan` | CloudFormation Drift |
| AWS-native features | Slight lag (days-weeks) | Immediate |

**My choice:** Terraform — primarily because S&B infrastructure may span AWS regions and potentially multi-cloud in future. The modular pattern I built at Hisan Labs reduced provisioning time 40% and scales cleanly. The trade-off: I'm responsible for state file management and locking (I use S3 + DynamoDB for remote state).

```hcl
# Remote state backend
terraform {
  backend "s3" {
    bucket         = "worldpay-terraform-state"
    key            = "sales-boarding/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```

---

### ❓ Why Kubernetes over plain EC2 for microservices?

> **Method:** Compare & Justify

| Need | EC2 | Kubernetes (EKS) |
|---|---|---|
| Auto-scaling | Manual ASG config | HPA — automatic, CPU/memory-based |
| Service discovery | Route53 / hard-coded | Built-in DNS via CoreDNS |
| Rolling deploys | Custom scripts | Native — `maxUnavailable: 0` |
| Self-healing | CloudWatch + Lambda | Kubelet restarts failed containers |
| Resource packing | One service per EC2 | Bin-packing multiple containers per node |

**Concrete reason from my work:** When I deployed 10 microservices for ApnaKart on EKS, HPA handled a 5x simulated load spike without manual intervention. On EC2, that would require pre-warmed ASGs per service — operationally expensive and slow to respond.

---

## Failure / Mistake Questions

### ❓ Tell me about a time something you deployed broke production.

> **Method:** Reflection — Situation → Mistake → Learning → Improvement

**Situation:** At Hisan Labs, I pushed a Terraform change to update RDS security group rules in production. The plan looked clean locally. On apply, it replaced the security group instead of modifying it — a known Terraform behavior with certain AWS resources that I had not accounted for.

**Mistake:** I ran `terraform apply` directly in production without first testing in a staging environment with identical resource state. The replacement caused ~8 minutes of database downtime for the affected service.

**Learning:**
1. Terraform's `plan` output shows `~` (update) vs `-/+` (destroy + recreate) — I now read every `-/+` line as a hard stop requiring review
2. Never apply infra changes to production without running the identical plan against staging first

**Improvement I implemented:**
```bash
# Added a pre-apply check in the pipeline
terraform plan -out=tfplan
# Script checks for destroy/replace operations
if terraform show -json tfplan | jq '.resource_changes[].change.actions | contains(["delete"])' | grep -q true; then
  echo "DESTRUCTIVE CHANGE DETECTED — manual approval required"
  exit 1
fi
```

Post this incident, we added a manual approval gate in Jenkins before any `terraform apply` in production.

---

## Strengths & Weaknesses

### ❓ What's your biggest strength as a DevOps engineer?

> **Method:** Value Pitch — Strength → Example → Impact

**Strength:** I connect infrastructure decisions to observable outcomes — I don't just provision; I instrument.

**Example:** At Hisan Labs, I didn't just deploy Prometheus and Grafana — I defined alert thresholds tied to SLOs, built dashboards that showed deployment frequency alongside error rate, and integrated Trivy into the CI pipeline so security failures blocked deploys before reaching production.

**Impact:** Zero critical vulnerabilities in production, MTTR reduced by 30%, and the team had enough observability data to retrospect on every incident with concrete metrics — not guesswork.

---

### ❓ What's an area you're actively improving?

> **Method:** Improvement Method — Weakness → Action Taken → Progress

**Weakness:** System design at scale — I'm strong at execution (pipelines, Kubernetes configs, Terraform modules) but I have limited experience making architecture decisions for systems handling hundreds of millions of transactions across multiple regions.

**Action Taken:**
- Studying AWS Solutions Architect Associate material — specifically multi-region active-active vs active-passive tradeoffs, and data consistency under partition
- Analyzing Worldpay's architecture hints from the JD: Boarding Gateway as single entry point and BCS as orchestrator maps to a saga pattern — I've been studying saga orchestration vs choreography tradeoffs specifically

**Progress:** I can now articulate why BCS (centralized orchestrator) has different failure modes than a pure event-driven choreography approach, and when each is preferable. I'm not yet at the point where I'd design this from scratch — but I'm building toward it deliberately.

---

## Leadership & Conflict Questions

### ❓ Describe a time you took ownership of something outside your defined scope.

> **Method:** Ownership Method — Situation → Responsibility → Action → Team Impact

**Situation:** At Hisan Labs, a frontend developer was blocked because the staging environment kept crashing due to misconfigured resource limits on the React service pod. It wasn't my ticket — it was logged as a "backend issue."

**Responsibility I took:** Instead of re-routing it, I investigated directly. Found that the Node.js container had no memory limit set, was OOMKilled repeatedly, and the CrashLoopBackOff was masking the root cause.

**Action:**
```bash
kubectl describe pod frontend-service-xxx
# Output showed: OOMKilled, Exit Code 137

# Fix: Added resource limits
kubectl patch deployment frontend-service -p '{
  "spec": {
    "template": {
      "spec": {
        "containers": [{
          "name": "frontend",
          "resources": {
            "limits": {"memory": "256Mi", "cpu": "200m"},
            "requests": {"memory": "128Mi", "cpu": "100m"}
          }
        }]
      }
    }
  }
}'
```

Then documented the fix and added it to our Kubernetes deployment template as a required field going forward.

**Team Impact:** The frontend developer unblocked within an hour. We added a policy requiring resource limits on all containers — preventing the same class of issue across all services.

---

## Time Pressure / Estimation Questions

### ❓ You have a critical hotfix and a scheduled deployment due today. How do you handle it?

> **Method:** Prioritization Method — Situation → Constraints → Prioritization → Execution → Result

**Constraints:**
- Hotfix: Customer-impacting bug in production — time-sensitive, smaller scope
- Scheduled deployment: Planned feature release — larger scope, non-emergency

**Prioritization:**
1. Hotfix goes first — production stability always takes priority over new features
2. Hotfix gets its own fast-track pipeline — no full regression suite, just smoke tests on the affected service

```bash
# Fast-track hotfix deployment
git checkout -b hotfix/boarding-null-pointer main
# ... fix committed ...
git push origin hotfix/boarding-null-pointer

# Trigger hotfix pipeline (skips non-critical stages)
# Jenkins: parameterized build with SKIP_REGRESSION=true

kubectl set image deployment/boarding-gateway \
  boarding-gateway=registry/boarding-gateway:hotfix-v1.2.1
kubectl rollout status deployment/boarding-gateway
```

3. After hotfix is confirmed stable (15 min soak), assess whether scheduled deployment still fits the window
4. If not: communicate the delay to stakeholders with a revised ETA — don't rush a scheduled deployment after a production hotfix

**Result:** Hotfix deployed in under 30 min, scheduled deployment rescheduled to next day with documented reason. Both stakeholders informed proactively.

---

### ❓ Estimate: How many Jenkins builds does a team of 10 engineers trigger per day?

> **Method:** Structured Estimation — Clarify → Break into Parts → Assumptions → Calculate → Validate

**Clarify:** Are we talking feature builds, PR builds, hotfixes, or all combined? Assuming a standard sprint-based team.

**Break into Parts:**

| Trigger Type | Per Engineer/Day | Total (10 engineers) |
|---|---|---|
| Feature branch pushes | ~3–4 commits | 30–40 builds |
| PR opened / updated | ~1–2 PRs | 10–20 builds |
| Merge to main (CI trigger) | ~2–3 merges across team | 2–3 builds |
| Scheduled nightly builds | Fixed | 2–5 builds |
| Hotfixes / reruns | ~10% overhead | 5–8 builds |

**Total estimate:** ~50–75 builds/day for a 10-person team in active development.

**Validate:** At Hisan Labs with a smaller team (~4–5 engineers), we averaged ~20–30 builds/day — scales roughly linearly.

**Implication for Worldpay's S&B team:** With parallel boarding workflow development across multiple services, Jenkins agent pool sizing and pipeline parallelism become important. I'd recommend at least 3–5 concurrent Jenkins agents to avoid queue buildup.

---

## 🔧 Quick Reference — Commands I'll Be Asked About

```bash
# Kubernetes
kubectl get pods -n <ns>
kubectl describe pod <pod> -n <ns>
kubectl logs <pod> --previous
kubectl rollout undo deployment/<name>
kubectl exec -it <pod> -- /bin/sh

# Terraform
terraform init && terraform plan -out=tfplan
terraform apply tfplan
terraform state list
terraform destroy -target=module.rds

# AWS CLI
aws eks update-kubeconfig --region ap-south-1 --name <cluster>
aws sts get-caller-identity
aws s3 cp s3://bucket/file .
aws logs tail /aws/lambda/<fn> --follow

# Docker
docker build -t app:tag .
docker run --rm -it app:tag /bin/sh
trivy image app:tag

# Git
git log --oneline -10
git diff HEAD~1
git stash && git pull && git stash pop
```

---

*Prepared for Worldpay AWS DevOps Engineer Interview | Sanket Ajay Chopade | April 2026*