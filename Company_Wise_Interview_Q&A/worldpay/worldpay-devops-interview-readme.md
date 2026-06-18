# Tell Me About Yourself

I am a DevOps Engineer with hands-on experience in building and optimizing CI/CD pipelines and cloud infrastructure on AWS. During my internship at Hisan Labs, I worked extensively with Jenkins, GitHub Actions, Docker, Kubernetes, and Terraform to automate deployments and manage scalable infrastructure.

One of my key achievements was reducing deployment time by 77% by optimizing CI/CD pipelines and implementing automated Kubernetes rollouts. I also worked on provisioning AWS infrastructure using Terraform, ensuring consistent and repeatable deployments.

In my projects like ApnaKart, I built a production-grade microservices platform using EKS, Jenkins, and Terraform, implementing zero-downtime deployment strategies like rolling and blue-green deployments.

I am particularly interested in this role at Worldpay because of its large-scale payment systems, where reliability, automation, and scalability are critical, and I’m excited to contribute and learn in such an environment.


# 🔍 CI/CD & Automation — Deep Follow-Up Interview Q&A

> Based on real experience from DevOps internship at Hisan Labs Private Limited and the ApnaKart microservices platform project.

---

## Q1. You mentioned reducing deployment time by 77% — what exact bottlenecks did you identify and eliminated?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Identify
The original pipeline ran end-to-end in ~45 minutes. After auditing the pipeline logs stage by stage, three primary bottlenecks surfaced:

| Bottleneck | Estimated Delay |
|---|---|
| Sequential stage execution (no parallelism) | ~12 min |
| Docker image rebuilt from scratch on every run | ~10 min |
| SonarQube analysis blocking deployment even on unchanged code | ~8 min |
| Kubernetes rollout waiting without readiness gates | ~6 min |

### 🩺 Diagnose
- Jenkins stages were **fully sequential** — unit tests, build, analysis, and Docker push all ran one after another even when they were independent.
- Docker builds had **no layer caching** — every run re-downloaded base images and re-installed dependencies.
- SonarQube was configured to **always scan**, with no incremental or branch-scoped analysis.
- Kubernetes rollout used `kubectl apply` with no `--timeout` or readiness probe validation, causing the pipeline to either hang or assume success prematurely.

### 🔧 Fix

**1. Introduced parallel stage execution in Jenkinsfile:**
```groovy
stage('Parallel: Test + Lint') {
  parallel {
    stage('Unit Tests') {
      steps { sh 'mvn test' }
    }
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar'
        }
      }
    }
  }
}
```

**2. Enabled Docker layer caching using BuildKit:**
```bash
export DOCKER_BUILDKIT=1
docker build \
  --cache-from=<ecr_url>/app:latest \
  -t <ecr_url>/app:${BUILD_NUMBER} .
```

**3. Added SonarQube quality gate with timeout instead of blocking indefinitely:**
```groovy
stage('Quality Gate') {
  steps {
    timeout(time: 5, unit: 'MINUTES') {
      waitForQualityGate abortPipeline: true
    }
  }
}
```

**4. Added Kubernetes rollout status check with timeout:**
```bash
kubectl rollout status deployment/app-deployment \
  --namespace=production \
  --timeout=120s
```

### 🛡️ Prevent
- Set **pipeline timeout limits** per stage to prevent silent hangs.
- Added **build status notifications** via Slack webhook so failures surface immediately.
- Documented expected stage durations in the runbook — any stage exceeding threshold triggers an alert.

### 📊 Result
Pipeline dropped from **45 minutes → 10 minutes (77% reduction)**. Release frequency increased **3x** as teams stopped avoiding deployments due to long wait times.

---

## Q2. How did you design your Jenkins pipeline stages? Why that structure?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
10 Spring Boot microservices in ApnaKart needed a CI/CD pipeline that was fast, safe, and auditable — without requiring separate pipelines per service.

### 🔄 Options Considered

| Approach | Pro | Con |
|---|---|---|
| Single flat pipeline (all sequential) | Simple to write | Slow, no isolation of failures |
| Fully parallel pipeline | Fast | Race conditions, harder to debug |
| Staged pipeline with selective parallelism | Balanced speed + safety | Slightly more complex to design |
| Per-service independent pipelines | Maximum isolation | High maintenance overhead for 10 services |

### ✅ Chosen Structure

```
Source Checkout
      ↓
Maven Compile
      ↓
 ┌────────────┬──────────────┐
Unit Tests    SonarQube Scan
 └────────────┴──────────────┘
      ↓
Quality Gate (SonarQube)
      ↓
Docker Build + Push to ECR
      ↓
Deploy to EKS (Rolling / Blue-Green)
      ↓
Rollout Status Verification
      ↓
Slack Notification
```

**Why this structure:**
- **Compile first** — no point running tests or analysis against code that doesn't even compile.
- **Test + Scan in parallel** — they are independent of each other; running them together saves ~8–10 minutes.
- **Quality Gate as a hard gate** — deployment only proceeds if SonarQube passes. This enforces DevSecOps policy.
- **Rollout verification step** — confirms Kubernetes has actually scheduled and started pods before marking the build green.

### ⚖️ Trade-offs
- Parallel stages add complexity to Jenkinsfile — worth it for pipelines running multiple times per day.
- Quality Gate adds latency if SonarQube server is slow — mitigated with a 5-minute timeout.

---

## Q3. What challenges did you face while integrating Jenkins with GitHub Actions?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Identify
The goal was a **hybrid CI/CD model**: GitHub Actions handles lightweight checks (linting, PR validation, secret scanning) while Jenkins handles heavyweight stages (Docker builds, EKS deployments). The challenge was making them work as one cohesive pipeline without duplication.

### 🩺 Diagnose
Three friction points emerged:

**1. Triggering Jenkins from GitHub Actions without exposing Jenkins externally**

GitHub Actions couldn't call a Jenkins webhook directly if Jenkins was inside a private VPC.

**2. Duplicate environment variable management**

Both systems needed the same secrets (ECR credentials, kubeconfig, SonarQube token) — maintaining them in two places created drift.

**3. Build status visibility**

GitHub PRs showed GitHub Actions status but had no visibility into whether the downstream Jenkins pipeline passed or failed.

### 🔧 Fix

**1. Used GitHub Actions to trigger Jenkins via API call (no public exposure needed):**
```yaml
# .github/workflows/trigger-jenkins.yml
- name: Trigger Jenkins Pipeline
  run: |
    curl -X POST \
      --user "${{ secrets.JENKINS_USER }}:${{ secrets.JENKINS_TOKEN }}" \
      "https://<jenkins-url>/job/apnakart-deploy/buildWithParameters" \
      --data "BRANCH=${GITHUB_REF_NAME}&COMMIT=${GITHUB_SHA}"
```

**2. Centralized secrets in AWS Secrets Manager — both systems pull from the same source:**
```bash
# Jenkins pipeline retrieves secret at runtime
aws secretsmanager get-secret-value \
  --secret-id prod/apnakart/ecr-credentials \
  --query SecretString \
  --output text
```

**3. Jenkins posts build status back to GitHub commit using GitHub Status API:**
```groovy
post {
  success {
    githubNotify status: 'SUCCESS',
      description: 'Jenkins deployment passed',
      context: 'jenkins/deploy'
  }
  failure {
    githubNotify status: 'FAILURE',
      description: 'Jenkins deployment failed',
      context: 'jenkins/deploy'
  }
}
```

### 🛡️ Prevent
- Documented the integration architecture so the two systems' responsibilities are clearly separated.
- Set GitHub branch protection rules to require the Jenkins status check before merging to `main`.

---

## Q4. How do you handle pipeline failures in production environments?

> **Method: 🚨 Action Plan Method** → Clarify → Immediate Action → Investigation → Communication → Prevention

### 🔍 Clarify
Pipeline failures in production fall into two categories:
- **Pre-deployment failures** (build/test/scan stage fails) — production is unaffected; block the release.
- **Post-deployment failures** (rollout fails after deployment starts) — production is partially affected; immediate rollback required.

### ⚡ Immediate Action

**For pre-deployment failure:**
```groovy
// Jenkins marks build as FAILED — deployment stage is never reached
// No action needed on infrastructure side
post {
  failure {
    slackSend channel: '#alerts',
      message: "❌ BUILD FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER} — ${env.BUILD_URL}"
  }
}
```

**For post-deployment rollout failure:**
```bash
# Check rollout status
kubectl rollout status deployment/app-deployment \
  --namespace=production \
  --timeout=120s

# If timeout or error — immediate rollback
kubectl rollout undo deployment/app-deployment \
  --namespace=production

# Verify rollback completed
kubectl rollout status deployment/app-deployment \
  --namespace=production
```

### 🩺 Investigation

```bash
# Check which pods failed and why
kubectl get pods -n production --field-selector=status.phase=Failed

# Get detailed failure reason
kubectl describe pod <failed-pod-name> -n production

# Check application logs
kubectl logs <failed-pod-name> -n production --previous

# Check events in the namespace
kubectl get events -n production --sort-by='.lastTimestamp'
```

### 📢 Communication
- Slack alert fires automatically via Jenkins `post { failure {} }` block.
- Incident is logged with: **what failed, when, which build/commit, rollback status, and ETA for fix**.
- If rollback itself fails — escalate immediately; do not attempt a second deployment without root cause identified.

### 🛡️ Prevention
- **Readiness probes** on all Kubernetes deployments — pods must pass health check before traffic is shifted.
- **Canary or blue-green strategy** for high-risk changes — limits blast radius before full rollout.
- **Automated smoke tests** after deployment — if smoke test fails, rollback triggers without human intervention.

```yaml
# Kubernetes readiness probe example
readinessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10
  failureThreshold: 3
```

---

## Q5. What strategies did you use to ensure rollback safety in your CI/CD pipelines?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
In a 10-microservice system, a failed deployment can cascade. Rolling back one service while others have already moved forward can cause version incompatibilities — rollback must be deliberate, fast, and safe.

### 🔄 Options Considered

| Strategy | Mechanism | Use Case |
|---|---|---|
| `kubectl rollout undo` | Reverts to previous ReplicaSet | Quick rollback, no pre-planning needed |
| Blue-Green Deployment | Switch load balancer between two environments | Zero-downtime, safest rollback |
| Canary Deployment | Gradual traffic shift with automatic abort | Best for risk-sensitive features |
| Docker image tagging with Git SHA | Always deployable previous image exists | Foundation for all strategies |

### ✅ Chosen Approach: Layered Rollback Safety

**Layer 1 — Docker images tagged with Git SHA (not `latest`):**
```bash
IMAGE_TAG="${GIT_COMMIT:0:8}"
docker build -t <ecr_url>/service:${IMAGE_TAG} .
docker push <ecr_url>/service:${IMAGE_TAG}

# Kubernetes deployment uses exact image tag
kubectl set image deployment/app-deployment \
  app=<ecr_url>/service:${IMAGE_TAG} \
  --namespace=production
```
This ensures every build is independently deployable — `latest` is never used in production.

**Layer 2 — Kubernetes rolling update with `maxUnavailable: 0`:**
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```
At no point during deployment does available capacity drop below 100%. Old pods stay alive until new ones are healthy.

**Layer 3 — Rollout verification gate in Jenkins:**
```groovy
stage('Verify Rollout') {
  steps {
    script {
      def status = sh(
        script: "kubectl rollout status deployment/app-deployment -n production --timeout=120s",
        returnStatus: true
      )
      if (status != 0) {
        sh "kubectl rollout undo deployment/app-deployment -n production"
        error("Rollout failed — automatic rollback triggered")
      }
    }
  }
}
```

**Layer 4 — Terraform state versioning for infrastructure rollback:**
```bash
# List available state versions in S3
aws s3api list-object-versions \
  --bucket apnakart-terraform-state \
  --prefix terraform.tfstate

# Restore a previous state version if infra change caused the issue
aws s3api get-object \
  --bucket apnakart-terraform-state \
  --key terraform.tfstate \
  --version-id <VERSION_ID> \
  terraform.tfstate
```

### ⚖️ Trade-offs
- `maxSurge: 1` temporarily runs one extra pod — adds minor cost during deployments.
- Automatic rollback on timeout can mask slow-starting services (e.g., JVM warm-up) — tuned `initialDelaySeconds` in readiness probes to prevent false rollbacks.
- Git SHA tagging requires ECR lifecycle policies to prevent image accumulation:

```bash
aws ecr put-lifecycle-policy \
  --repository-name apnakart/service \
  --lifecycle-policy-text '{"rules":[{"rulePriority":1,"selection":{"tagStatus":"untagged","countType":"imageCountMoreThan","countNumber":10},"action":{"type":"expire"}}]}'
```

### 📊 Result
Achieved **98% deployment success rate** and **35% reduction in deployment failures** through this layered approach. Rollback time from failure detection to healthy state averaged under **3 minutes**.

---

*Answers follow structured interview methods mapped to question type. All commands reflect real configurations used in production deployments.*

---

# ☁️ AWS & Infrastructure — Deep Follow-Up Interview Q&A

> Based on real experience from DevOps internship at Hisan Labs Private Limited and the ApnaKart production-grade microservices platform.

---

## Q1. Walk me through your Terraform module design for AWS infrastructure.

> **Method: 🧾 Project Walkthrough** → Problem → Tech Stack → Role → Challenges → Solution → Result

### 🧩 Problem
ApnaKart required provisioning a complete AWS footprint for 10 Spring Boot microservices — EKS cluster, RDS instances (one per service), VPC with public/private subnets, IAM roles, Security Groups, ALB, CloudFront, and S3. Doing this manually was error-prone and non-repeatable across dev, test, UAT, and prod environments.

### 🏗️ Module Structure

```
terraform/
├── main.tf                  # Root — calls all modules
├── variables.tf             # Input variable declarations
├── outputs.tf               # Exported values (cluster endpoint, RDS URLs, etc.)
├── backend.tf               # S3 remote state + DynamoDB lock config
├── terraform.tfvars         # Environment-specific values
│
└── modules/
    ├── vpc/                 # VPC, subnets, route tables, IGW, NAT Gateway
    ├── eks/                 # EKS cluster, node groups, OIDC provider
    ├── rds/                 # RDS per-service instances, subnet groups, param groups
    ├── iam/                 # IAM roles, policies, IRSA role bindings
    ├── alb/                 # Application Load Balancer, target groups, listeners
    ├── s3/                  # S3 buckets, bucket policies, versioning
    └── security_groups/     # Inbound/outbound rules per service tier
```

### 🔧 How Each Module Is Called

Each module accepts inputs and returns outputs so modules compose without hardcoded values:

```hcl
# main.tf (root)

module "vpc" {
  source             = "./modules/vpc"
  vpc_cidr           = var.vpc_cidr
  public_subnets     = var.public_subnets
  private_subnets    = var.private_subnets
  availability_zones = var.availability_zones
  environment        = var.environment
}

module "eks" {
  source          = "./modules/eks"
  vpc_id          = module.vpc.vpc_id
  private_subnets = module.vpc.private_subnet_ids
  cluster_name    = var.cluster_name
  node_instance_type = var.node_instance_type
  desired_capacity   = var.desired_capacity
  environment     = var.environment
}

module "rds" {
  source            = "./modules/rds"
  for_each          = var.microservices          # One RDS per service
  vpc_id            = module.vpc.vpc_id
  private_subnets   = module.vpc.private_subnet_ids
  db_name           = each.key
  db_instance_class = var.db_instance_class
  environment       = var.environment
}
```

### ⚙️ Why `for_each` for RDS
10 microservices each get their own isolated database. Using `for_each` avoids copy-pasting the RDS block 10 times and allows per-service parameterization:

```hcl
# variables.tf
variable "microservices" {
  type    = set(string)
  default = ["order", "product", "user", "payment", "cart",
             "notification", "inventory", "review", "search", "gateway"]
}
```

### 🏷️ Consistent Tagging Across All Resources
```hcl
# modules/vpc/main.tf
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr

  tags = {
    Name        = "${var.environment}-vpc"
    Environment = var.environment
    Project     = "apnakart"
    ManagedBy   = "terraform"
  }
}
```

### 📊 Result
- **40% reduction** in manual provisioning time.
- Identical module calls across dev/test/UAT/prod — environment differences handled entirely through `terraform.tfvars`.
- Full infrastructure teardown and recreation in a new environment takes under 20 minutes.

---

## Q2. How did you manage state files in Terraform (remote backend, locking)?

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

### 📖 Definition
Terraform state is a JSON file that maps your configuration to real infrastructure. By default it's stored locally — which breaks in team environments because two engineers running `terraform apply` simultaneously can corrupt state or create duplicate resources.

**Remote backend** moves state to a shared, centralized location. **State locking** prevents concurrent writes.

### ⚙️ How It Works — S3 Backend + DynamoDB Lock

```hcl
# backend.tf
terraform {
  backend "s3" {
    bucket         = "apnakart-terraform-state"
    key            = "environments/prod/terraform.tfstate"
    region         = "ap-south-1"
    encrypt        = true
    dynamodb_table = "apnakart-terraform-lock"
  }
}
```

| Component | Purpose |
|---|---|
| S3 bucket | Stores the `.tfstate` file remotely |
| `encrypt = true` | Encrypts state at rest using S3 SSE (AES-256) |
| `key` path | Separate state file per environment (prod/dev/uat) |
| DynamoDB table | Provides atomic lock — only one `apply` runs at a time |

### 🔐 How DynamoDB Locking Works

When `terraform apply` starts:
1. Terraform writes a lock entry to the DynamoDB table with a lock ID.
2. Any other `terraform apply` on the same key reads the lock and **blocks** until it clears.
3. Once apply finishes (or errors), the lock is released automatically.

```bash
# Create the DynamoDB lock table (one-time setup)
aws dynamodb create-table \
  --table-name apnakart-terraform-lock \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region ap-south-1
```

### 📁 Per-Environment State Isolation

Each environment has its own state file under a separate S3 key:

```
s3://apnakart-terraform-state/
├── environments/
│   ├── dev/terraform.tfstate
│   ├── test/terraform.tfstate
│   ├── uat/terraform.tfstate
│   └── prod/terraform.tfstate
```

This means a `terraform destroy` in dev never touches prod state.

### 🔄 State Versioning for Recovery
S3 bucket versioning was enabled so any accidental state corruption can be rolled back:

```bash
# Enable versioning on state bucket
aws s3api put-bucket-versioning \
  --bucket apnakart-terraform-state \
  --versioning-configuration Status=Enabled

# List previous state versions
aws s3api list-object-versions \
  --bucket apnakart-terraform-state \
  --prefix environments/prod/terraform.tfstate
```

---

## Q3. What security best practices did you implement in IAM policies?

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

### 📖 Core Principle: Least Privilege
Every IAM identity (user, role, service account) gets only the permissions it needs — nothing more. Broad policies like `*:*` were never used in production.

### ✅ Practice 1 — IRSA (IAM Roles for Service Accounts)

Instead of embedding AWS credentials in pods or using node-level IAM roles (which give all pods on a node the same permissions), IRSA binds an IAM role to a specific Kubernetes service account.

```hcl
# IAM role for the order-service pod only
resource "aws_iam_role" "order_service_role" {
  name = "apnakart-order-service-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Federated = aws_iam_openid_connect_provider.eks.arn
      }
      Action = "sts:AssumeRoleWithWebIdentity"
      Condition = {
        StringEquals = {
          "${aws_iam_openid_connect_provider.eks.url}:sub" =
            "system:serviceaccount:production:order-service-sa"
        }
      }
    }]
  })
}

# Attach only S3 read permission — nothing else
resource "aws_iam_role_policy" "order_s3_read" {
  name = "order-s3-read"
  role = aws_iam_role.order_service_role.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect   = "Allow"
      Action   = ["s3:GetObject"]
      Resource = "arn:aws:s3:::apnakart-assets/orders/*"
    }]
  })
}
```

### ✅ Practice 2 — Condition Keys to Restrict Access by Resource Tag

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["ec2:StartInstances", "ec2:StopInstances"],
    "Resource": "*",
    "Condition": {
      "StringEquals": {
        "ec2:ResourceTag/Environment": "dev"
      }
    }
  }]
}
```
This prevents a dev IAM role from accidentally starting/stopping production EC2 instances.

### ✅ Practice 3 — No Inline Policies on IAM Users; Use Roles
- IAM users were never given inline policies directly.
- All permissions went through **managed policies attached to roles**.
- Humans assumed roles via `sts:AssumeRole` — never used long-lived access keys for production.

### ✅ Practice 4 — S3 Bucket Policies Block Public Access

```hcl
resource "aws_s3_bucket_public_access_block" "state" {
  bucket                  = aws_s3_bucket.terraform_state.id
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

### ✅ Practice 5 — CloudTrail for IAM Audit
All API calls were logged via CloudTrail to S3 — this provided a full audit trail of who assumed which role, when, and what actions were performed.

---

## Q4. How did you design your VPC architecture for microservices?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 Requirements
- 10 microservices must be reachable internally but not directly exposed to the internet.
- Databases must be completely isolated from public traffic.
- External traffic must only enter through a controlled ingress (ALB).
- EKS node-to-node and pod-to-pod communication must be secure and segmented.

### 🏛️ Architecture Overview

```
Internet
    |
[CloudFront + WAF]
    |
[Application Load Balancer]  ← Public Subnet (ap-south-1a, 1b)
    |
[EKS Worker Nodes]           ← Private Subnet (ap-south-1a, 1b)
    |
[RDS Instances]              ← Isolated Subnet (ap-south-1a, 1b) — no route to internet
```

### 🧱 Subnet Design

| Subnet Type | CIDR Example | What Lives Here | Internet Access |
|---|---|---|---|
| Public | `10.0.1.0/24`, `10.0.2.0/24` | ALB, NAT Gateway | Yes (IGW) |
| Private | `10.0.3.0/24`, `10.0.4.0/24` | EKS nodes, pods | Outbound only (NAT GW) |
| Isolated | `10.0.5.0/24`, `10.0.6.0/24` | RDS instances | None |

```hcl
# Private subnet — EKS nodes
resource "aws_subnet" "private" {
  count             = length(var.availability_zones)
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.private_subnets[count.index]
  availability_zone = var.availability_zones[count.index]

  tags = {
    "kubernetes.io/cluster/${var.cluster_name}" = "shared"
    "kubernetes.io/role/internal-elb"           = "1"
  }
}
```

The Kubernetes tag `kubernetes.io/role/internal-elb` tells the AWS Load Balancer Controller which subnets to use for internal ALBs.

### 🔒 Security Groups Per Tier

```hcl
# ALB Security Group — accepts 443 from internet only
resource "aws_security_group" "alb" {
  name   = "${var.environment}-alb-sg"
  vpc_id = var.vpc_id

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port       = 8080
    to_port         = 8080
    protocol        = "tcp"
    security_groups = [aws_security_group.eks_nodes.id]
  }
}

# RDS Security Group — accepts traffic only from EKS nodes
resource "aws_security_group" "rds" {
  name   = "${var.environment}-rds-sg"
  vpc_id = var.vpc_id

  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.eks_nodes.id]
  }
}
```

### 🔁 NAT Gateway for Outbound Traffic from Private Subnets

EKS nodes in private subnets need outbound internet access (to pull Docker images from ECR, for example) — but must not be directly reachable from the internet.

```hcl
resource "aws_nat_gateway" "main" {
  allocation_id = aws_eip.nat.id
  subnet_id     = aws_subnet.public[0].id   # NAT GW lives in public subnet

  tags = {
    Name = "${var.environment}-nat-gw"
  }
}

resource "aws_route_table" "private" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main.id  # Outbound only
  }
}
```

### ⚖️ Trade-offs
- NAT Gateway costs ~$32/month per AZ — for high availability, one per AZ is recommended but adds cost. In non-prod environments a single NAT Gateway was used to reduce spend.
- Isolated subnets for RDS add complexity in subnet group configuration but are non-negotiable for production database security.

---

## Q5. How do you optimize AWS cost for services like EKS, EC2, and RDS?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
Running EKS, multiple EC2 nodes, and 10 RDS instances continuously at full capacity in non-production environments burns money without adding value. Cost optimization is about right-sizing and right-scheduling — not cutting corners on production reliability.

---

### 🔧 EKS Cost Optimization

**1. Use Spot Instances for non-production node groups:**

```hcl
resource "aws_eks_node_group" "dev_nodes" {
  cluster_name    = aws_eks_cluster.main.name
  node_group_name = "dev-spot-nodes"
  node_role_arn   = aws_iam_role.node_role.arn
  subnet_ids      = var.private_subnet_ids

  capacity_type  = "SPOT"            # Up to 70% cheaper than On-Demand
  instance_types = ["t3.medium", "t3.large", "t3a.medium"]  # Multiple types = fewer interruptions

  scaling_config {
    desired_size = 2
    min_size     = 1
    max_size     = 5
  }
}
```

**2. Cluster Autoscaler — scale down idle nodes automatically:**

```yaml
# cluster-autoscaler deployment annotation
annotations:
  cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
```

```bash
# Cluster Autoscaler flags to enable aggressive scale-down
--scale-down-enabled=true
--scale-down-delay-after-add=5m
--scale-down-unneeded-time=5m
--scale-down-utilization-threshold=0.5
```

**3. HPA to prevent over-provisioning replicas:**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: order-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: order-service
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

---

### 🔧 EC2 Cost Optimization

**1. Reserved Instances for predictable production workloads:**
- Production node groups use **1-year Reserved Instances** (~40% saving vs On-Demand).
- Non-production uses Spot or On-Demand with auto-shutdown schedules.

**2. Auto Scaling schedules for non-production environments:**

```bash
# Scale down dev EKS node group to 0 after business hours
aws autoscaling put-scheduled-action \
  --auto-scaling-group-name dev-node-group \
  --scheduled-action-name scale-down-night \
  --recurrence "0 20 * * 1-5" \
  --desired-capacity 0 \
  --min-size 0

# Scale back up in the morning
aws autoscaling put-scheduled-action \
  --auto-scaling-group-name dev-node-group \
  --scheduled-action-name scale-up-morning \
  --recurrence "0 8 * * 1-5" \
  --desired-capacity 2 \
  --min-size 1
```

---

### 🔧 RDS Cost Optimization

**1. Use `db.t3.micro` / `db.t3.small` for non-production:**

```hcl
variable "db_instance_class" {
  default = {
    dev  = "db.t3.micro"
    test = "db.t3.small"
    uat  = "db.t3.medium"
    prod = "db.r6g.large"
  }
}
```

**2. Stop RDS instances in dev/test during off-hours:**

```bash
# Stop all dev RDS instances
aws rds stop-db-instance --db-instance-identifier apnakart-order-dev

# Can also be automated via Lambda + EventBridge cron
```

> Note: AWS automatically restarts stopped RDS instances after 7 days — automate the stop schedule to handle this.

**3. Enable RDS storage autoscaling instead of over-provisioning upfront:**

```hcl
resource "aws_db_instance" "service" {
  allocated_storage     = 20
  max_allocated_storage = 100   # Autoscales up to 100GB only when needed
  # ...
}
```

---

### 📊 Cost Optimization Summary

| Service | Strategy | Estimated Saving |
|---|---|---|
| EKS (non-prod) | Spot Instances | ~60–70% |
| EKS (prod) | Reserved Instances (1yr) | ~40% |
| EC2 / Node Groups | Scheduled scale-to-zero (off-hours) | ~35% |
| RDS (non-prod) | t3.micro + stop after hours | ~50% |
| RDS (prod) | Storage autoscaling (no over-provisioning) | ~20% |

### ⚖️ Trade-offs
- Spot Instances can be interrupted — never use them for production stateful workloads or single-replica deployments.
- Scheduled scale-to-zero assumes no one needs the dev environment at night — coordinate with the team before enabling.
- Stopping RDS instances adds a ~2–3 minute cold start in the morning — acceptable for dev, not for production.

---

*Answers follow structured interview methods mapped to question type. All commands and configurations reflect real implementations used in production and project environments.*

---

# ⎈ Kubernetes (EKS) — Deep Follow-Up Interview Q&A

> Based on real experience from DevOps internship at Hisan Labs Private Limited and the ApnaKart production-grade microservices platform on AWS EKS.

---

## Q1. Explain how you implemented zero-downtime deployments using Kubernetes.

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

### 📖 Definition
Zero-downtime deployment means traffic is never interrupted during a release — old pods continue serving requests until new pods are fully healthy and ready to accept traffic.

This is not a single feature. It is a combination of four Kubernetes mechanisms working together.

---

### ⚙️ Mechanism 1 — RollingUpdate Strategy with `maxUnavailable: 0`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
  namespace: production
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0   # Never take a pod down before a new one is ready
      maxSurge: 1         # Allow one extra pod above desired count during rollout
```

With `maxUnavailable: 0`, Kubernetes will not terminate a single old pod until the replacement is Running **and** passing its readiness probe. This is the single most important setting for zero-downtime.

---

### ⚙️ Mechanism 2 — Readiness Probes

Without a readiness probe, Kubernetes marks a pod Ready as soon as the container starts — before the application inside has actually initialised. Traffic gets routed to a pod that isn't ready to serve it.

```yaml
readinessProbe:
  httpGet:
    path: /actuator/health/readiness
  port: 8080
  initialDelaySeconds: 20   # Spring Boot JVM needs time to start
  periodSeconds: 10
  failureThreshold: 3       # 3 consecutive failures before pod is marked unready
  successThreshold: 1
```

Kubernetes only routes traffic to a pod once readiness passes. If the new pod never becomes ready, the rollout halts and old pods keep serving.

---

### ⚙️ Mechanism 3 — Liveness Probes

Liveness probes detect pods that are running but stuck (deadlock, OOM loop, thread exhaustion). Kubernetes restarts them automatically.

```yaml
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 15
  failureThreshold: 3
```

> **Key distinction:** readiness controls traffic routing. Liveness controls pod restart.
> Never point both at the same endpoint without understanding the difference.

---

### ⚙️ Mechanism 4 — Graceful Shutdown with `terminationGracePeriodSeconds`

When Kubernetes terminates an old pod, it sends SIGTERM first. The application needs time to finish in-flight requests before the process exits. Without this, active requests get dropped.

```yaml
spec:
  terminationGracePeriodSeconds: 30  # Give the app 30s to drain in-flight requests
  containers:
  - name: order-service
    lifecycle:
      preStop:
        exec:
          command: ["/bin/sh", "-c", "sleep 5"]  # Small delay before SIGTERM
                                                  # allows load balancer to deregister pod
```

The `preStop` sleep is necessary because there is a race condition between Kubernetes removing the pod from the Service endpoints and the load balancer actually stopping new connections. The 5-second sleep bridges that gap.

---

### ✅ Complete Zero-Downtime Deployment Flow

```
1. kubectl apply / Jenkins deploys new image tag
2. Kubernetes creates 1 new pod (maxSurge: 1)
3. New pod starts — readiness probe begins checking
4. Readiness probe PASSES → pod added to Service endpoints
5. Kubernetes removes 1 old pod (sends SIGTERM)
6. Old pod drains in-flight requests (terminationGracePeriodSeconds)
7. Old pod exits cleanly
8. Repeat until all old pods replaced
9. Rollout marked complete
```

### 📊 Result
Achieved **98% deployment success rate** and **zero-downtime rolling updates** across all 10 microservices in ApnaKart under production workloads.

---

## Q2. What issues did you face with HPA and how did you tune it?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Issue 1 — HPA Not Scaling Up During Traffic Spikes

#### Identify
During load testing, CPU on pods spiked above the HPA threshold but new pods were not being created fast enough. Requests were timing out.

#### Diagnose
```bash
# Check HPA status
kubectl get hpa order-service-hpa -n production

# Output showed:
# TARGETS: 85%/70%   MINPODS: 2   MAXPODS: 8   REPLICAS: 2
# — HPA saw the breach but replicas hadn't changed yet
```

Root cause: HPA has a default **scale-up stabilisation window of 0 seconds** but the metrics server scrape interval is **15 seconds**. By the time the decision was made and a new pod scheduled, the spike had already caused timeouts.

#### Fix — Pre-warm with higher `minReplicas` for known traffic windows

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: order-service-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: order-service
  minReplicas: 3        # Was 1 — increased baseline to absorb initial spikes
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60    # Was 80 — lowered threshold to trigger earlier
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 0     # Scale up immediately on breach
      policies:
      - type: Pods
        value: 2                        # Add 2 pods at a time, not 1
        periodSeconds: 30
    scaleDown:
      stabilizationWindowSeconds: 300   # Wait 5 minutes before scaling down
```

---

### 🔎 Issue 2 — HPA Showed `<unknown>` for CPU Metrics

#### Identify
```bash
kubectl get hpa -n production
# NAME                  REFERENCE          TARGETS         MINPODS   MAXPODS   REPLICAS
# order-service-hpa     Deployment/order   <unknown>/60%   3         10        3
```

#### Diagnose
HPA pulls metrics from the **Kubernetes Metrics Server**. `<unknown>` means Metrics Server is either not installed or the pod's resource `requests` are not defined. HPA calculates utilisation as `(actual CPU) / (requested CPU)` — without a `requests` value, the percentage is undefined.

```bash
# Confirm metrics server is running
kubectl get deployment metrics-server -n kube-system

# Check if resources are defined on the pod
kubectl describe pod order-service-xxx -n production | grep -A5 "Requests"
```

#### Fix — Add explicit `resources.requests` to every container

```yaml
containers:
- name: order-service
  image: <ecr_url>/order-service:abc123
  resources:
    requests:
      cpu: "250m"       # HPA uses this as the denominator
      memory: "256Mi"
    limits:
      cpu: "500m"
      memory: "512Mi"
```

#### Prevent
Made `resources.requests` a **required field** in Helm chart values schema validation so no deployment can be applied without them.

---

### 🔎 Issue 3 — HPA Scaling Down Too Aggressively, Causing Latency Spikes

#### Identify
After traffic decreased, HPA scaled replicas down quickly. The remaining pods then got hit by a new wave and CPU spiked before scale-up could respond.

#### Fix — Increase `scaleDown` stabilisation window (already shown above)

```yaml
behavior:
  scaleDown:
    stabilizationWindowSeconds: 300  # Only scale down if low CPU persists for 5 min
```

This prevents thrashing — rapidly scaling down then immediately back up.

---

## Q3. How do you manage secrets securely in Kubernetes?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
Kubernetes Secrets are base64-encoded by default — **not encrypted**. Anyone with access to the cluster or the etcd database can decode them trivially:

```bash
echo "c3VwZXJzZWNyZXQ=" | base64 --decode
# supersecret
```

This means storing database passwords, API keys, or TLS certificates as plain Kubernetes Secrets is a security risk — especially in a 10-microservice production system.

---

### 🔄 Options Compared

| Approach | Encryption at Rest | Audit Trail | Works Offline | Complexity |
|---|---|---|---|---|
| Plain Kubernetes Secrets (base64) | ❌ No | ❌ No | ✅ Yes | Low |
| Sealed Secrets (Bitnami) | ✅ Yes (asymmetric) | ❌ Limited | ✅ Yes | Medium |
| AWS Secrets Manager + External Secrets Operator | ✅ Yes (AWS KMS) | ✅ Yes (CloudTrail) | ❌ No | Medium |
| HashiCorp Vault | ✅ Yes | ✅ Full | ✅ Yes | High |

---

### ✅ What Was Implemented in ApnaKart

**Current state:** Kubernetes Secrets with base64 encoding — functional but not production-hardened for sensitive credentials.

**Honest limitation:** Base64 is encoding, not encryption. This was identified as a critical gap during a project review.

**Improvement path implemented / planned:**

#### Step 1 — Enable Envelope Encryption on EKS etcd using AWS KMS

```hcl
# In Terraform EKS module
resource "aws_eks_cluster" "main" {
  name = var.cluster_name

  encryption_config {
    provider {
      key_arn = aws_kms_key.eks_secrets.arn
    }
    resources = ["secrets"]
  }
}
```

This encrypts Kubernetes Secrets at rest in etcd using a KMS key — even if someone gets raw etcd access, secrets are unreadable.

#### Step 2 — External Secrets Operator pulling from AWS Secrets Manager

```yaml
# ExternalSecret manifest — pulls from AWS Secrets Manager into a K8s Secret
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: order-db-credentials
  namespace: production
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: aws-secrets-manager
    kind: ClusterSecretStore
  target:
    name: order-db-secret       # Creates a standard K8s Secret with this name
    creationPolicy: Owner
  data:
  - secretKey: DB_PASSWORD
    remoteRef:
      key: prod/apnakart/order-service
      property: db_password
```

```bash
# The actual secret value lives in AWS Secrets Manager — never in Git
aws secretsmanager create-secret \
  --name prod/apnakart/order-service \
  --secret-string '{"db_password":"<value>","db_user":"order_svc"}'
```

#### Step 3 — Never commit secrets to Git

```bash
# .gitignore
*.env
*secret*.yaml
*credentials*.yaml
terraform.tfvars      # Contains sensitive variable values
```

All secret values are injected at deploy time from AWS Secrets Manager or environment-specific parameter stores — never stored in the repository.

### ⚖️ Trade-offs
- External Secrets Operator adds a dependency on AWS connectivity — if the pod can't reach Secrets Manager, secret sync fails. Mitigated by caching the last-known value with `refreshInterval`.
- KMS encryption adds ~1–5ms latency per secret read at pod startup — negligible for most workloads.

---

## Q4. How do you debug a failing pod in production?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### ⚡ Structured Debugging Sequence

Never guess. Work through layers: cluster → node → pod → container → application.

---

#### Layer 1 — Get the High-Level Picture

```bash
# What is the pod's current state?
kubectl get pods -n production

# Example output:
# order-service-7d4f9b-xkp2q   0/1   CrashLoopBackOff   5   8m
```

Common failure states and what they mean:

| Status | Likely Cause |
|---|---|
| `CrashLoopBackOff` | App crashes on start — check logs |
| `OOMKilled` | Container exceeded memory limit |
| `Pending` | No node has capacity, or PVC unbound |
| `ImagePullBackOff` | Wrong image tag or ECR auth failure |
| `Error` | Container exited with non-zero code |

---

#### Layer 2 — Read the Events First

```bash
kubectl describe pod order-service-7d4f9b-xkp2q -n production
```

The `Events` section at the bottom is the most useful output — it shows scheduling failures, image pull errors, probe failures, and OOMKill signals in plain language.

---

#### Layer 3 — Read Application Logs

```bash
# Current logs
kubectl logs order-service-7d4f9b-xkp2q -n production

# Logs from previous crash (for CrashLoopBackOff)
kubectl logs order-service-7d4f9b-xkp2q -n production --previous

# Follow logs in real time
kubectl logs -f order-service-7d4f9b-xkp2q -n production

# Multi-container pod — specify container
kubectl logs order-service-7d4f9b-xkp2q -n production -c order-service
```

---

#### Layer 4 — OOMKilled Diagnosis

```bash
kubectl describe pod order-service-7d4f9b-xkp2q -n production | grep -A5 "Last State"

# Output:
# Last State: Terminated
#   Reason: OOMKilled
#   Exit Code: 137
```

Fix: Increase memory limit or profile the application for memory leaks.

```yaml
resources:
  requests:
    memory: "256Mi"
  limits:
    memory: "512Mi"   # Increase if OOMKilled
```

---

#### Layer 5 — ImagePullBackOff Diagnosis

```bash
kubectl describe pod <pod-name> -n production | grep -A3 "Warning"
# Warning  Failed  pull access denied / not found / 401 Unauthorized
```

```bash
# Verify the image exists in ECR
aws ecr describe-images \
  --repository-name apnakart/order-service \
  --image-ids imageTag=abc1234

# Verify the node has ECR pull permissions (IRSA or node IAM role)
kubectl get serviceaccount order-service-sa -n production -o yaml
```

---

#### Layer 6 — Exec Into a Running Pod for Live Inspection

```bash
# Open a shell inside the container
kubectl exec -it order-service-7d4f9b-xkp2q -n production -- /bin/sh

# Check environment variables are injected correctly
env | grep DB_

# Test database connectivity from inside the pod
nc -zv <rds-endpoint> 5432

# Check disk space
df -h
```

---

#### Layer 7 — Check Node-Level Issues

```bash
# Is the node itself under pressure?
kubectl describe node <node-name> | grep -A10 "Conditions"

# Check node resource usage
kubectl top nodes

# Check pod resource usage
kubectl top pods -n production
```

---

### 🛡️ Prevention Checklist
- Readiness and liveness probes configured on every deployment.
- Resource limits set — prevents one pod from starving others.
- Alerts on `CrashLoopBackOff` and `OOMKilled` in Datadog / CloudWatch.
- Log aggregation in ELK / Datadog so logs persist after pod deletion.

---

## Q5. What is your strategy for resource limits and requests?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 📖 Core Distinction

| Field | What It Does | Scheduler Uses It? |
|---|---|---|
| `requests` | Minimum guaranteed resources for the pod | ✅ Yes — for pod placement |
| `limits` | Maximum resources the container can use | ❌ No — enforced at runtime |

A pod with no `requests` can be scheduled onto a full node. A pod with no `limits` can consume all node resources and starve neighbouring pods.

---

### 🔄 Options for Setting Values

| Approach | Pro | Con |
|---|---|---|
| Set requests = limits | Predictable, Guaranteed QoS class | Wasteful — pods can't burst |
| requests < limits | Allows bursting during spikes | Risk of node overcommit |
| No limits (limits omitted) | Maximum flexibility | One runaway pod kills the node |
| LimitRange (namespace default) | Protects against missing values | Can mask misconfiguration |

---

### ✅ Strategy Used in ApnaKart

**Rule: Requests set to average observed usage. Limits set to 2x requests.**

This allows pods to burst during traffic spikes without starving other pods on the same node.

```yaml
# order-service — Spring Boot JVM application
resources:
  requests:
    cpu: "250m"       # Average observed: ~200–250m under normal load
    memory: "256Mi"   # Spring Boot baseline heap + overhead
  limits:
    cpu: "500m"       # 2x request — allows burst without monopolising node
    memory: "512Mi"   # Hard ceiling — OOMKill before starving neighbours
```

```yaml
# react-frontend — lightweight Node server
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "200m"
    memory: "256Mi"
```

---

### 🔒 LimitRange — Namespace-Level Safety Net

If a developer deploys a pod without resource fields, the LimitRange injects defaults automatically:

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: apnakart-limits
  namespace: production
spec:
  limits:
  - type: Container
    default:
      cpu: "300m"
      memory: "256Mi"
    defaultRequest:
      cpu: "150m"
      memory: "128Mi"
    max:
      cpu: "1000m"       # No single container can request more than 1 CPU
      memory: "1Gi"
```

---

### 📊 ResourceQuota — Namespace-Level Hard Cap

Prevents the entire production namespace from exceeding allocated capacity:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: production-quota
  namespace: production
spec:
  hard:
    requests.cpu: "10"
    requests.memory: "20Gi"
    limits.cpu: "20"
    limits.memory: "40Gi"
    pods: "50"
```

---

### ⚙️ How to Measure Actual Usage Before Setting Values

Never set requests by guessing. Observe real usage first:

```bash
# Current pod resource usage
kubectl top pods -n production --sort-by=cpu

# Historical metrics in Datadog or CloudWatch
# Set requests at p75 of observed CPU — not p99 (that's what limits are for)
```

---

### ⚖️ Trade-offs

- **Requests too low** → pod gets scheduled on overloaded nodes → CPU throttling → latency spikes.
- **Requests too high** → node capacity wasted → fewer pods per node → higher AWS bill.
- **Limits too low for memory** → OOMKill under normal load spikes → CrashLoopBackOff.
- **Limits too low for CPU** → CPU throttling (not OOMKill) — harder to detect, shows up as latency.

> CPU limits cause throttling, not termination. Memory limits cause OOMKill. These fail differently — monitor both separately.

---

*Answers follow structured interview methods mapped to question type. All configurations reflect real implementations used in production and project environments.*

---

# 📊 Monitoring & Observability — Deep Follow-Up Interview Q&A

> Based on real experience from DevOps internship at Hisan Labs Private Limited and the ApnaKart production-grade microservices platform on AWS EKS.

---

## Q1. How did you reduce MTTR by 30%? What exact changes helped?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Identify — What Was Causing High MTTR Before

MTTR (Mean Time To Resolve) was high for three specific reasons:

| Problem | Impact |
|---|---|
| No centralised alerting — engineers discovered issues from user reports | Detection delay: 15–30 min |
| Alert notifications had no context — just "pod down" with no logs or cause | Diagnosis delay: 20–40 min |
| No runbooks — each incident required tribal knowledge to resolve | Resolution delay: variable |

Every minute spent finding *where* the problem was, *what* caused it, and *how* to fix it added directly to MTTR. The 30% reduction came from cutting each of those three phases.

---

### 🔧 Fix 1 — Automated Detection via Prometheus Alerting Rules

Shifted from reactive (user reports) to proactive (system-triggered) detection.

```yaml
# prometheus-rules.yaml
groups:
- name: apnakart-critical
  rules:

  - alert: PodCrashLooping
    expr: rate(kube_pod_container_status_restarts_total[5m]) > 0
    for: 2m
    labels:
      severity: critical
      team: platform
    annotations:
      summary: "Pod {{ $labels.pod }} is crash looping"
      description: "Container {{ $labels.container }} in namespace {{ $labels.namespace }} has restarted {{ $value }} times in 5 minutes."
      runbook_url: "https://wiki.internal/runbooks/pod-crashloop"

  - alert: ServiceHighLatency
    expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m])) > 2
    for: 3m
    labels:
      severity: warning
    annotations:
      summary: "P99 latency above 2s on {{ $labels.service }}"
      description: "{{ $labels.service }} p99 latency is {{ $value }}s — SLA threshold is 2s."
```

Before this, detection time averaged 15–30 minutes. After: under 2 minutes.

---

### 🔧 Fix 2 — Alert Notifications With Context, Not Just State

Slack alerts were rewritten to include actionable information instead of raw metric values.

```yaml
# alertmanager.yml — Slack receiver with context
receivers:
- name: slack-critical
  slack_configs:
  - api_url: "<slack_webhook_url>"
    channel: "#prod-alerts"
    title: "🔴 {{ .GroupLabels.alertname }}"
    text: |
      *Severity:* {{ .CommonLabels.severity }}
      *Namespace:* {{ .CommonLabels.namespace }}
      *Pod:* {{ .CommonLabels.pod }}
      *Summary:* {{ .CommonAnnotations.summary }}
      *Description:* {{ .CommonAnnotations.description }}
      *Runbook:* {{ .CommonAnnotations.runbook_url }}
      *Grafana Dashboard:* https://grafana.internal/d/apnakart-prod
```

An engineer receiving this alert knows immediately: what broke, where it broke, how severe it is, and where to go next. Diagnosis time dropped because engineers stopped spending the first 10 minutes finding the right dashboard.

---

### 🔧 Fix 3 — Runbooks Attached to Every Alert

Every alert annotation includes a `runbook_url` field pointing to a documented resolution procedure. Runbooks cover:

- Immediate triage steps
- Common root causes for that specific alert
- Exact commands to run
- Escalation path if the runbook steps don't resolve it

This removed the tribal knowledge dependency — a new engineer on call could follow the runbook without needing a senior to walk them through it.

---

### 🔧 Fix 4 — Datadog APM for Distributed Trace Correlation

Before APM: when a request failed, engineers manually checked logs across 10 services to find where it failed.

After APM: a single trace ID tied together the entire request path across all microservices. The failing service, failing method, and error message were visible in one view within seconds.

```yaml
# Datadog APM agent configuration in Kubernetes DaemonSet
env:
- name: DD_APM_ENABLED
  value: "true"
- name: DD_TRACE_SAMPLE_RATE
  value: "1.0"           # 100% sampling — reduced later once baseline established
- name: DD_LOGS_INJECTION
  value: "true"          # Injects trace ID into application logs automatically
```

With `DD_LOGS_INJECTION`, every log line emitted by a Spring Boot service automatically includes the trace ID. This means: alert fires → open trace → click to correlated logs — all in under 60 seconds.

---

### 📊 MTTR Reduction Breakdown

| Phase | Before | After | Saving |
|---|---|---|---|
| Detection | 15–30 min | < 2 min | ~20 min |
| Diagnosis | 20–40 min | 5–10 min | ~20 min |
| Resolution | Variable | Faster via runbooks | ~10 min |
| **Total MTTR** | ~60–90 min | ~35–45 min | **~30% reduction** |

---

## Q2. What alerts did you configure in Prometheus/Grafana?

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

### 📖 Alert Design Principle
Alerts are grouped into three tiers — **Critical** (page someone now), **Warning** (investigate within the hour), **Info** (track, no action needed). Every alert fires only on a symptom that requires human action.

---

### 🔴 Critical Alerts — Immediate Action Required

```yaml
# 1. Pod crash looping
- alert: PodCrashLooping
  expr: rate(kube_pod_container_status_restarts_total[5m]) > 0
  for: 2m
  labels:
    severity: critical

# 2. Deployment has zero available replicas — service is down
- alert: DeploymentUnavailable
  expr: kube_deployment_status_replicas_available == 0
  for: 1m
  labels:
    severity: critical

# 3. Node not ready
- alert: NodeNotReady
  expr: kube_node_status_condition{condition="Ready", status="true"} == 0
  for: 5m
  labels:
    severity: critical

# 4. High error rate — more than 5% of requests returning 5xx
- alert: HighErrorRate
  expr: rate(http_requests_total{status=~"5.."}[5m]) /
        rate(http_requests_total[5m]) > 0.05
  for: 3m
  labels:
    severity: critical
```

---

### 🟡 Warning Alerts — Investigate Before It Becomes Critical

```yaml
# 5. P99 latency breach
- alert: HighP99Latency
  expr: histogram_quantile(0.99,
          rate(http_request_duration_seconds_bucket[5m])) > 2
  for: 5m
  labels:
    severity: warning

# 6. CPU throttling — pods are being throttled by limits
- alert: CPUThrottlingHigh
  expr: rate(container_cpu_cfs_throttled_seconds_total[5m]) /
        rate(container_cpu_cfs_periods_total[5m]) > 0.25
  for: 10m
  labels:
    severity: warning

# 7. Memory close to limit — 90% of limit consumed
- alert: PodMemoryNearLimit
  expr: container_memory_working_set_bytes /
        on(pod, container) kube_pod_container_resource_limits{resource="memory"}
        > 0.9
  for: 5m
  labels:
    severity: warning

# 8. Persistent Volume near full
- alert: PVCAlmostFull
  expr: kubelet_volume_stats_used_bytes /
        kubelet_volume_stats_capacity_bytes > 0.85
  for: 10m
  labels:
    severity: warning

# 9. HPA at max replicas — autoscaler cannot scale further
- alert: HPAMaxedOut
  expr: kube_horizontalpodautoscaler_status_current_replicas ==
        kube_horizontalpodautoscaler_spec_max_replicas
  for: 5m
  labels:
    severity: warning
```

---

### 🔵 Infrastructure Alerts — AWS + EKS Layer

```yaml
# 10. RDS CPU high
- alert: RDSHighCPU
  expr: aws_rds_cpuutilization_average > 80
  for: 10m
  labels:
    severity: warning

# 11. ALB 5xx error rate
- alert: ALBHighErrorRate
  expr: aws_applicationelb_httpcode_elb_5_xx_count_sum > 10
  for: 5m
  labels:
    severity: critical
```

---

### 📊 Grafana Dashboard Structure

Each dashboard was structured with a top-to-bottom drill-down flow:

```
Row 1: System Health Overview    (RED metrics: Rate, Errors, Duration per service)
Row 2: Kubernetes Cluster        (Node CPU/Memory, Pod restarts, Pending pods)
Row 3: Per-Service Metrics       (Latency p50/p95/p99, request rate, error rate)
Row 4: Database Layer            (RDS connections, query latency, storage)
Row 5: CI/CD Pipeline            (Deployment frequency, success rate, rollback count)
```

The top row gives a 5-second health summary. Engineers can drill down without switching dashboards.

---

## Q3. How do you avoid alert fatigue?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
Alert fatigue happens when engineers receive so many notifications that they stop trusting or responding to them. It is not a monitoring problem — it is an alert design problem. Noisy alerts are worse than no alerts because they train people to ignore the system entirely.

---

### 🔄 What Causes Alert Fatigue (and the fix for each)

**Problem 1 — Alerts fire on cause, not symptom**

Alerting on CPU at 70% is alerting on a cause. CPU spikes happen constantly and most of the time they self-resolve. The symptom is high latency or error rate — alert on that instead.

```yaml
# ❌ Wrong — fires constantly, rarely needs action
- alert: HighCPU
  expr: container_cpu_usage_seconds_total > 0.7

# ✅ Right — fires only when users are actually affected
- alert: ServiceDegraded
  expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m])) > 2
  for: 5m
```

---

**Problem 2 — No `for` duration — alerts fire on momentary spikes**

Without a `for` clause, an alert fires the instant the condition is true — even for 1-second spikes that resolve themselves.

```yaml
# ❌ Wrong — fires on 1-second CPU spike
- alert: PodCrashLooping
  expr: rate(kube_pod_container_status_restarts_total[5m]) > 0

# ✅ Right — only fires if condition persists for 2 minutes
- alert: PodCrashLooping
  expr: rate(kube_pod_container_status_restarts_total[5m]) > 0
  for: 2m
```

---

**Problem 3 — Alert grouping not configured — 10 pods down = 10 separate Slack messages**

```yaml
# alertmanager.yml
route:
  group_by: ['alertname', 'namespace']     # Group related alerts together
  group_wait: 30s                           # Wait 30s to collect related alerts
  group_interval: 5m                        # Minimum time between grouped notifications
  repeat_interval: 4h                       # Don't re-notify unless condition persists 4h
```

10 pods in the same namespace crashing = 1 grouped Slack message, not 10.

---

**Problem 4 — Alerts fire during maintenance windows**

```yaml
# Silence alerts during planned deployments in Alertmanager
# Created via Alertmanager UI or API before each deployment

curl -X POST http://alertmanager:9093/api/v2/silences \
  -H "Content-Type: application/json" \
  -d '{
    "matchers": [{"name": "namespace", "value": "production"}],
    "startsAt": "2025-03-15T02:00:00Z",
    "endsAt": "2025-03-15T03:00:00Z",
    "comment": "Planned deployment window",
    "createdBy": "sanket"
  }'
```

---

**Problem 5 — Info-level alerts mixed with critical alerts in the same channel**

```yaml
# alertmanager.yml — route by severity to separate channels
routes:
- match:
    severity: critical
  receiver: slack-critical        # #prod-alerts — engineers watch this closely
- match:
    severity: warning
  receiver: slack-warning         # #prod-warnings — reviewed during business hours
- match:
    severity: info
  receiver: slack-info            # #monitoring-logs — nobody on call watches this
```

---

### ✅ Alert Quality Checklist

Before any alert goes to production, it passes this test:

| Question | Required Answer |
|---|---|
| Does this alert require human action? | Yes |
| Is this alerting on a symptom, not a cause? | Yes |
| Does it have a `for` duration? | Yes |
| Does it have a runbook URL? | Yes |
| Has it been tested to confirm it actually fires? | Yes |
| Can it be resolved without waking someone at 2am? | If no → must be critical severity |

---

## Q4. How do you correlate logs, metrics, and traces during incidents?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → How They Connect

### 📋 Why Correlation Matters
In a 10-microservice system, a single user request touches multiple services. When it fails, the error might be logged in service C, the slowdown visible in metrics from service B, and the root cause actually in service A. Without correlation, you are debugging three separate systems manually.

The goal: **one incident ID that links every signal together.**

---

### 🏛️ Three Pillars and Their Roles

| Pillar | Tool Used | What It Answers |
|---|---|---|
| Metrics | Prometheus + Grafana + CloudWatch | *Something is wrong* — latency/error rate spike |
| Logs | ELK Stack (Elasticsearch, Logstash, Kibana) | *What happened* — error messages, stack traces |
| Traces | Datadog APM | *Where it happened* — which service, which call, which line |

These are not independent. They are connected by a **trace ID**.

---

### ⚙️ How Trace IDs Connect All Three

**Step 1 — Datadog agent injects a trace ID into every incoming request:**

```yaml
# application.properties (Spring Boot)
dd.trace.enabled=true
dd.logs.injection=true     # Automatically adds trace_id and span_id to every log line
```

Every log line emitted by the application now looks like:

```
2025-03-15 14:23:01 ERROR OrderService - Payment timeout
  dd.trace_id=4532109876543210
  dd.span_id=1234567890123456
  service=order-service
  env=production
```

**Step 2 — ELK collects and indexes those logs with the trace ID as a searchable field:**

```json
# Logstash filter — parse trace ID from log output
filter {
  grok {
    match => {
      "message" => "dd.trace_id=%{NUMBER:trace_id} dd.span_id=%{NUMBER:span_id}"
    }
  }
}
```

**Step 3 — Incident correlation flow:**

```
1. Prometheus alert fires → "OrderService P99 latency > 2s"
2. Engineer opens Grafana → latency spike visible at 14:23:00
3. Engineer queries Kibana for logs between 14:22:00–14:24:00 for order-service
4. Finds error: "Payment timeout" with trace_id=4532109876543210
5. Opens Datadog APM → pastes trace ID → sees full distributed trace:
   → order-service called payment-service
   → payment-service called external-gateway
   → external-gateway timed out after 5s
6. Root cause identified: external payment gateway latency — not an internal issue
7. Fix: increase timeout on payment-service HTTP client / add circuit breaker
```

Without trace ID correlation, step 4→5 would require manually checking payment-service logs separately and guessing the timing correlation. With it, the entire chain is visible in one click.

---

### ⚙️ Practical Kibana Query for Incident Investigation

```
# Find all errors in production in a 5-minute window
kubernetes.namespace: production
AND level: ERROR
AND @timestamp: [2025-03-15T14:20:00 TO 2025-03-15T14:25:00]

# Narrow to a specific trace
trace_id: "4532109876543210"

# Find all services involved in this trace
trace_id: "4532109876543210" AND NOT service: "order-service"
```

---

### ⚙️ CloudWatch for AWS-Layer Correlation

When the root cause is at the infrastructure layer (not application), CloudWatch Container Insights bridges the gap:

```bash
# CloudWatch Logs Insights query — find all EKS pod errors in a time window
fields @timestamp, @message, kubernetes.pod_name, kubernetes.namespace
| filter @message like /ERROR/
| filter kubernetes.namespace = "production"
| sort @timestamp desc
| limit 50
```

This surfaces AWS-level issues (node evictions, EBS volume failures, ENI exhaustion) that do not appear in application logs.

---

### 🗺️ Incident Correlation Reference

```
Alert fires (Prometheus / Datadog)
          ↓
Check Grafana dashboard
  → Which service? Which time window?
          ↓
Query Kibana / CloudWatch Logs
  → Error messages, stack traces, trace IDs
          ↓
Open Datadog APM with trace ID
  → Full distributed trace across all services
  → Identify which service and which dependency failed
          ↓
Root cause confirmed
  → Apply fix or rollback
  → Document in incident report
```

---

### ⚖️ Trade-offs

- 100% trace sampling (used initially) adds overhead at high request volume — reduced to 20% sampling in steady state once baselines were established.
- ELK adds operational overhead — index management, retention policies, and storage costs. For smaller teams, Datadog Logs alone can replace ELK.
- Correlation only works if every service uses the same tracing library and log format. One service logging without the trace ID breaks the chain — standardising log format across all 10 microservices was a prerequisite.

---

*Answers follow structured interview methods mapped to question type. All configurations reflect real implementations used in production and project environments.*

---

# 🔐 DevSecOps — Deep Follow-Up Interview Q&A

> Based on real experience from DevOps internship at Hisan Labs Private Limited and the ApnaKart production-grade microservices platform on AWS EKS.

---

## Q1. How did you integrate Trivy and SonarQube into your pipeline?

> **Method: 🧾 Project Walkthrough** → Problem → Tech Stack → Role → Challenges → Solution → Result

### 🧩 Problem
A 10-microservice platform with Docker images built on every commit and deployed to production EKS needs two types of security checks:

| Check | Tool | What It Catches |
|---|---|---|
| Source code quality and security | SonarQube | SQL injection, hardcoded secrets, OWASP Top 10 code flaws, code coverage gaps |
| Container image vulnerabilities | Trivy | CVEs in OS packages, language dependencies, misconfigurations in Dockerfile |

Neither check is useful if it runs after deployment. Both must be **hard gates** in the pipeline — deployment stops if they fail.

---

### 🏗️ Full Pipeline Security Integration

```
Source Checkout
      ↓
Maven Compile
      ↓
 ┌──────────────────┬──────────────────┐
Unit Tests          SonarQube Analysis
 └──────────────────┴──────────────────┘
      ↓
SonarQube Quality Gate  ← HARD GATE (pipeline aborts on failure)
      ↓
Docker Image Build
      ↓
Trivy Image Scan        ← HARD GATE (pipeline aborts on CRITICAL CVEs)
      ↓
Push to ECR
      ↓
Deploy to EKS
```

---

### ⚙️ SonarQube Integration

**Step 1 — SonarQube analysis runs parallel to unit tests (saves time):**

```groovy
// Jenkinsfile
stage('Parallel: Test + SonarQube') {
  parallel {
    stage('Unit Tests') {
      steps {
        sh 'mvn test'
      }
    }
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh '''
            mvn sonar:sonar \
              -Dsonar.projectKey=apnakart-order-service \
              -Dsonar.projectName="ApnaKart Order Service" \
              -Dsonar.java.coveragePlugin=jacoco \
              -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
          '''
        }
      }
    }
  }
}
```

**Step 2 — Quality Gate blocks deployment if thresholds are not met:**

```groovy
stage('SonarQube Quality Gate') {
  steps {
    timeout(time: 5, unit: 'MINUTES') {
      waitForQualityGate abortPipeline: true   // Pipeline fails if gate fails
    }
  }
}
```

**Quality Gate thresholds configured in SonarQube:**

| Metric | Threshold |
|---|---|
| Coverage on new code | ≥ 80% |
| Duplicated lines on new code | < 3% |
| Maintainability rating | A |
| Reliability rating | A |
| Security rating | A |
| Security Hotspots reviewed | 100% |

If any threshold is breached, `waitForQualityGate` returns `FAILED` and `abortPipeline: true` stops the build before Docker even runs. The commit never reaches production.

---

### ⚙️ Trivy Integration

Trivy scans the built Docker image for CVEs **before it is pushed to ECR**.

```groovy
stage('Docker Build') {
  steps {
    sh '''
      docker build \
        --cache-from ${ECR_URL}/order-service:latest \
        -t ${ECR_URL}/order-service:${GIT_COMMIT:0:8} \
        -f Dockerfile .
    '''
  }
}

stage('Trivy Image Scan') {
  steps {
    sh '''
      trivy image \
        --exit-code 1 \
        --severity CRITICAL \
        --ignore-unfixed \
        --format table \
        --output trivy-report-${GIT_COMMIT:0:8}.txt \
        ${ECR_URL}/order-service:${GIT_COMMIT:0:8}
    '''
  }
  post {
    always {
      archiveArtifacts artifacts: 'trivy-report-*.txt', allowEmptyArchive: true
    }
  }
}
```

**Flag breakdown:**

| Flag | Purpose |
|---|---|
| `--exit-code 1` | Returns non-zero exit if vulnerabilities found — Jenkins marks stage as FAILED |
| `--severity CRITICAL` | Only block on CRITICAL CVEs — HIGH/MEDIUM generate warnings, not failures |
| `--ignore-unfixed` | Skip CVEs with no available fix — flagging unfixable CVEs creates noise without actionable outcome |
| `--format table` | Human-readable report archived as a build artifact |

```groovy
stage('Push to ECR') {
  // This stage only runs if Trivy scan passed
  steps {
    sh '''
      aws ecr get-login-password --region ap-south-1 | \
        docker login --username AWS --password-stdin ${ECR_URL}
      docker push ${ECR_URL}/order-service:${GIT_COMMIT:0:8}
    '''
  }
}
```

If Trivy finds a CRITICAL CVE, the image is never pushed to ECR. The registry stays clean.

---

### 📊 Result
- **Zero critical CVEs** across all production deployments.
- SonarQube caught 3 security hotspots (hardcoded test credentials, insecure HTTP client config, missing input validation) before they reached production.
- Trivy scan added ~2 minutes to pipeline time — acceptable given the risk it eliminates.

---

## Q2. What types of vulnerabilities did you commonly encounter?

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

Vulnerabilities fell into three categories: code-level (caught by SonarQube), image-level (caught by Trivy), and configuration-level (caught by manual review and IAM audits).

---

### 🔴 Category 1 — Code-Level (SonarQube)

**1. Hardcoded Credentials**

The most common finding. Developers hardcode database URLs or API keys during local development and accidentally commit them.

```java
// ❌ What SonarQube flagged — Rule: java:S2068
private static final String DB_PASSWORD = "admin123";
DataSource ds = DriverManager.getConnection(url, "root", "admin123");
```

```java
// ✅ Fix — inject from environment variable
String dbPassword = System.getenv("DB_PASSWORD");
DataSource ds = DriverManager.getConnection(url, dbUser, dbPassword);
```

Environment variables are injected at runtime via Kubernetes Secrets (mounted as env vars) — value never appears in source code.

---

**2. Insecure HTTP Client (Missing TLS Validation)**

```java
// ❌ SonarQube flagged — Rule: java:S4423
SSLContext ctx = SSLContext.getInstance("TLS");
ctx.init(null, new TrustManager[]{ new X509TrustManager() {
  public void checkClientTrusted(...) {}  // Accepts any certificate — MITM risk
  public void checkServerTrusted(...) {}
}}, null);
```

```java
// ✅ Fix — use default SSL context which validates certificates
HttpClient client = HttpClient.newBuilder()
  .sslContext(SSLContext.getDefault())
  .build();
```

---

**3. Missing Input Validation**

```java
// ❌ SonarQube flagged — Rule: java:S2076 (command injection risk)
public String getOrderStatus(String orderId) {
  return Runtime.getRuntime()
    .exec("order-cli status " + orderId)  // orderId could contain shell metacharacters
    .toString();
}
```

```java
// ✅ Fix — use ProcessBuilder with explicit argument list
ProcessBuilder pb = new ProcessBuilder("order-cli", "status", orderId);
```

---

### 🟡 Category 2 — Image-Level CVEs (Trivy)

**1. Outdated Base Image OS Packages**

The most frequent Trivy finding. Base images like `openjdk:11-jdk` pull in Ubuntu/Debian system packages that accumulate CVEs over time.

```dockerfile
# ❌ Bloated base image — carries hundreds of OS packages, many with CVEs
FROM openjdk:11-jdk

# ✅ Fix — use distroless or slim variant
FROM eclipse-temurin:17-jre-alpine

# ✅ Better — distroless has near-zero attack surface
FROM gcr.io/distroless/java17-debian12
```

Switching from `openjdk:11-jdk` to `eclipse-temurin:17-jre-alpine` reduced the number of Trivy findings from 40+ to under 5 on the same application.

---

**2. Outdated Application Dependencies**

Trivy also scans `pom.xml` and `package.json` inside the image for known CVEs in libraries:

```
# Example Trivy finding
Library: spring-web
Version: 5.3.18
CVE: CVE-2022-22965 (Spring4Shell)
Severity: CRITICAL
Fixed in: 5.3.20
```

Fix: update the dependency version in `pom.xml` and rebuild.

---

**3. Running as Root Inside Container**

```dockerfile
# ❌ Default — container runs as root
FROM eclipse-temurin:17-jre-alpine
COPY target/order-service.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]

# ✅ Fix — create and switch to non-root user
FROM eclipse-temurin:17-jre-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --chown=appuser:appgroup target/order-service.jar app.jar
USER appuser
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

Running as root means a container escape vulnerability gives the attacker root access on the node. Non-root limits the blast radius.

---

### 🔵 Category 3 — Configuration-Level (Manual Audit + IAM Review)

**1. Overly Permissive IAM Policies**

```json
// ❌ What was found during IAM review — wildcard on S3
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": "*"
}

// ✅ Fix — scoped to specific bucket and actions only
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:PutObject"],
  "Resource": "arn:aws:s3:::apnakart-assets/orders/*"
}
```

**2. Kubernetes Secrets as Plaintext Base64**

Base64 is encoding, not encryption. Any cluster user with `get secret` RBAC permission can decode them:

```bash
kubectl get secret db-credentials -n production -o jsonpath='{.data.password}' | base64 --decode
```

Mitigated by: KMS envelope encryption on EKS etcd + RBAC restrictions on `get/list secrets` in production namespace.

---

## Q3. How do you enforce security gates in CI/CD?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Problem
Security checks only have value if they can actually stop a bad deployment. An advisory scan that reports issues but does not block the pipeline is decorative — developers learn to ignore it.

Security gates are the mechanism that makes security checks binding.

---

### 🔄 Options for Enforcing Gates

| Approach | How It Blocks | Limitation |
|---|---|---|
| Exit code check (`--exit-code 1`) | Tool returns non-zero → Jenkins stage fails → pipeline stops | Only as strict as the threshold configured |
| SonarQube `waitForQualityGate` | Polling SonarQube API — aborts pipeline if gate status is ERROR | Requires SonarQube server to be reachable |
| OPA / Kyverno admission controller | Blocks `kubectl apply` if image not from approved registry or not scanned | Enforces at cluster level, not just pipeline |
| Branch protection rules (GitHub) | Requires CI status checks to pass before merge to main | Only enforces on PRs — not on direct pushes |

---

### ✅ Three-Layer Gate Approach

Security is enforced at three independent checkpoints so that bypassing one does not allow a vulnerable deployment through.

---

**Gate 1 — Pipeline Level (Jenkins)**

```groovy
// SonarQube gate — blocks on code quality failure
stage('Quality Gate') {
  steps {
    timeout(time: 5, unit: 'MINUTES') {
      waitForQualityGate abortPipeline: true
    }
  }
}

// Trivy gate — blocks on CRITICAL CVEs in Docker image
stage('Trivy Scan') {
  steps {
    sh '''
      trivy image \
        --exit-code 1 \
        --severity CRITICAL \
        --ignore-unfixed \
        ${ECR_URL}/order-service:${GIT_COMMIT:0:8}
    '''
  }
}
```

If either stage returns a non-zero exit code, Jenkins marks the build FAILED and all downstream stages (ECR push, EKS deploy) are skipped automatically.

---

**Gate 2 — Git Level (GitHub Branch Protection)**

```yaml
# GitHub branch protection rules on main branch:
# — Require status checks to pass before merging
#   ✓ jenkins/build
#   ✓ jenkins/sonarqube
#   ✓ jenkins/trivy-scan
# — Require branches to be up to date before merging
# — Restrict who can push directly to main
```

This means even if a developer bypasses the Jenkins pipeline (direct `git push` to main), GitHub rejects the push until all status checks pass. The pipeline cannot be skipped.

---

**Gate 3 — Cluster Level (Kyverno Admission Controller)**

The cluster itself rejects workloads that did not pass security scanning — even if someone runs `kubectl apply` manually outside of Jenkins.

```yaml
# Kyverno policy — only allow images from the approved ECR registry
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-image-registry
spec:
  validationFailureAction: Enforce    # Reject, not just warn
  rules:
  - name: allowed-registries
    match:
      resources:
        kinds: [Pod]
    validate:
      message: "Images must come from the approved ECR registry."
      pattern:
        spec:
          containers:
          - image: "<ecr_account_id>.dkr.ecr.ap-south-1.amazonaws.com/*"
```

```yaml
# Kyverno policy — block containers running as root
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-root-containers
spec:
  validationFailureAction: Enforce
  rules:
  - name: check-runAsNonRoot
    match:
      resources:
        kinds: [Pod]
    validate:
      message: "Containers must not run as root."
      pattern:
        spec:
          containers:
          - securityContext:
              runAsNonRoot: true
```

A deployment that somehow bypassed the pipeline is still rejected at the Kubernetes API server level before any pod is scheduled.

---

### 📊 Gate Coverage Summary

| Threat | Gate 1 (Jenkins) | Gate 2 (GitHub) | Gate 3 (Kyverno) |
|---|---|---|---|
| Code with security vulnerabilities | ✅ SonarQube | ✅ Status check | — |
| Image with CRITICAL CVEs | ✅ Trivy | ✅ Status check | — |
| Image from unscanned/unknown registry | — | — | ✅ Registry policy |
| Container running as root | — | — | ✅ securityContext policy |
| Direct `kubectl apply` bypassing pipeline | — | — | ✅ Admission controller |
| Direct `git push` bypassing pipeline | — | ✅ Branch protection | — |

---

### ⚖️ Trade-offs

- `--ignore-unfixed` on Trivy means CVEs with no available patch are skipped — this is the right pragmatic call since flagging them adds noise without an actionable fix. Revisit when new patches release.
- Kyverno admission controller adds ~50ms latency to every `kubectl apply` — negligible for deployment frequency but worth knowing if asked.
- Branch protection only applies to PRs — direct pushes to non-protected branches still bypass Gate 2. All feature branches were named in a pattern (`feature/*`, `hotfix/*`) and mapped to non-production environments to limit risk.

---

*Answers follow structured interview methods mapped to question type. All configurations reflect real implementations used in production and project environments.*

---

# 🔗 Cross-Questions — Connecting Different Parts of Experience

> These questions test whether you understand *how* different parts of your work connected to each other — not just what each tool does in isolation.

---

## Q1. You improved deployment speed — how did that impact system reliability?

> **Method: ⭐ STAR Method** → Situation → Task → Action → Result

### 📍 Situation
Before pipeline optimisation, deploying to production took 45 minutes end-to-end. Because deployments were slow and painful, the team avoided them — batching multiple changes into large, infrequent releases.

### 🎯 Task
Reduce deployment time while ensuring the speed improvement did not come at the cost of reliability. Faster deployments are only valuable if they do not increase failure rate or production incidents.

### ⚙️ Action
The critical insight was that **deployment speed and reliability are not in tension — they are correlated**. Large infrequent releases are riskier than small frequent ones because:

- More changes per deployment = larger blast radius if something breaks
- Longer gaps between releases = more merge conflicts, stale branches, harder rollbacks
- Slow pipelines = engineers skip tests or bypass gates under pressure

The optimisations made were:

| Change | Speed Impact | Reliability Impact |
|---|---|---|
| Parallel test + SonarQube stages | -12 min pipeline time | No change — same tests running |
| Docker layer caching | -10 min build time | No change — same image produced |
| Readiness probes + `maxUnavailable: 0` | +2 min rollout (by design) | Zero-downtime deployments |
| Automated rollback on rollout failure | No speed impact | 35% reduction in deployment failures |
| Trivy + SonarQube hard gates | +2 min pipeline time | Zero critical CVEs in production |

The rollout itself was deliberately slowed down (`maxUnavailable: 0`) to protect reliability — the time savings came from the earlier stages.

### 📊 Result
- Pipeline: **45 min → 10 min (77% reduction)**
- Release frequency: **3x increase** — smaller, safer changes deployed more often
- Deployment success rate: **98%**
- System availability: **99.9%**
- Deployment failures: **35% reduction**

Faster deployments reduced risk per deployment. More frequent deployments reduced the size of each change. Both factors improved reliability — not despite the speed improvement, but because of how it was implemented.

---

## Q2. You used Terraform + Ansible — when do you choose one over the other?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Core Distinction

| Dimension | Terraform | Ansible |
|---|---|---|
| Purpose | **Provisioning** — create and manage infrastructure resources | **Configuration** — install software, manage state on existing resources |
| State model | Stateful — tracks resource state in `.tfstate` | Stateless — applies tasks idempotently on every run |
| What it manages | Cloud resources: VPCs, EKS clusters, RDS, IAM roles, S3 | OS-level: packages, files, services, application config |
| Idempotency | Enforced via state file | Enforced via task design |
| When it runs | On infrastructure change | On application config change or new node bootstrap |

The clean boundary: **Terraform creates the server. Ansible configures what runs on it.**

---

### ✅ How They Were Used in ApnaKart

**Terraform handled everything at the cloud API level:**

```hcl
# Terraform — provisions the EKS node group
resource "aws_eks_node_group" "production" {
  cluster_name    = aws_eks_cluster.main.name
  node_group_name = "prod-nodes"
  node_role_arn   = aws_iam_role.node_role.arn
  subnet_ids      = module.vpc.private_subnet_ids
  instance_types  = ["t3.large"]

  scaling_config {
    desired_size = 3
    min_size     = 2
    max_size     = 8
  }
}
```

Terraform does not know or care what software runs on those nodes. It only knows the node group exists, has the right IAM role, and is in the right subnets.

**Ansible handled OS and agent-level configuration on those nodes:**

```yaml
# Ansible playbook — configures Datadog agent on EKS nodes after they join
- name: Install and configure Datadog agent
  hosts: eks_nodes
  become: true
  tasks:
    - name: Add Datadog apt repository
      apt_repository:
        repo: "deb https://apt.datadoghq.com/ stable 7"
        state: present

    - name: Install Datadog agent
      apt:
        name: datadog-agent
        state: present

    - name: Configure Datadog agent
      template:
        src: datadog.yaml.j2
        dest: /etc/datadog-agent/datadog.yaml
      notify: restart datadog-agent

    - name: Enable and start Datadog agent
      systemd:
        name: datadog-agent
        enabled: true
        state: started
```

Terraform cannot do this — it has no mechanism for SSH-ing into a node and managing files. Ansible cannot provision the EKS node group — it has no state model for cloud resources.

---

### 🔄 Decision Rule in Practice

```
Does the task involve creating or modifying a cloud resource?
  → Use Terraform

Does the task involve configuring software or state on an existing server?
  → Use Ansible

Does it involve both?
  → Terraform first, Ansible second
  → Terraform outputs the resource (node IP, endpoint)
  → Ansible uses that output as inventory
```

```hcl
# Terraform outputs the node IPs
output "node_ips" {
  value = aws_instance.eks_nodes[*].private_ip
}
```

```bash
# Ansible uses Terraform output as dynamic inventory
terraform output -json node_ips | \
  jq -r '.[]' > ansible/inventory/eks_nodes.txt

ansible-playbook -i ansible/inventory/eks_nodes.txt \
  ansible/playbooks/configure-nodes.yml
```

### ⚖️ Trade-offs
- Ansible can technically provision cloud resources via AWS modules — but without a state file, it cannot track what it created or safely destroy it. Do not use Ansible as a Terraform replacement.
- Terraform can run `user_data` scripts on EC2 at boot — but this runs once at creation, not on config changes. For ongoing configuration management, Ansible is the right tool.

---

## Q3. How does your CI/CD pipeline interact with Kubernetes deployments?

> **Method: 🧠 Concept Explanation** → Definition → How It Works → Example

### 📖 The Connection Point
The CI/CD pipeline and Kubernetes are separate systems with one explicit handoff point: **the pipeline builds and pushes a Docker image, then tells Kubernetes which image to run.**

```
Jenkins Pipeline                    Kubernetes (EKS)
─────────────────                   ────────────────
Source checkout
Maven compile
Unit tests + SonarQube
Quality gate
Docker build
Trivy scan
ECR push          ──── image tag ──→ kubectl set image / Helm upgrade
                                      Kubernetes pulls image from ECR
                                      Rolling update begins
                                      Readiness probes validate
Rollout verify    ←── rollout status ─ kubectl rollout status
Slack notify
```

---

### ⚙️ Handoff Method 1 — `kubectl set image` with Git SHA tag

```groovy
stage('Deploy to EKS') {
  steps {
    withAWS(credentials: 'aws-deploy-role', region: 'ap-south-1') {
      sh '''
        aws eks update-kubeconfig \
          --name apnakart-prod \
          --region ap-south-1

        kubectl set image deployment/order-service \
          order-service=${ECR_URL}/order-service:${GIT_COMMIT:0:8} \
          --namespace=production

        kubectl rollout status deployment/order-service \
          --namespace=production \
          --timeout=120s
      '''
    }
  }
}
```

The pipeline authenticates to EKS using an IAM role (not hardcoded credentials), updates the image tag on the running deployment, then waits for Kubernetes to confirm the rollout succeeded before marking the build green.

---

### ⚙️ Handoff Method 2 — Helm upgrade for multi-value changes

When a deployment requires more than an image update (environment variables, resource limits, replica count), Helm is cleaner:

```groovy
stage('Helm Deploy') {
  steps {
    sh '''
      helm upgrade order-service ./helm/order-service \
        --namespace production \
        --set image.tag=${GIT_COMMIT:0:8} \
        --set replicaCount=3 \
        --set resources.requests.cpu=250m \
        --values ./helm/order-service/values-prod.yaml \
        --wait \           # Equivalent to kubectl rollout status
        --timeout 2m
    '''
  }
}
```

---

### ⚙️ Branch-to-Environment Mapping

The pipeline uses the Git branch name to determine which Kubernetes namespace to deploy into:

```groovy
def getNamespace(branch) {
  switch(branch) {
    case 'develop':    return 'dev'
    case 'test':       return 'test'
    case 'uat':        return 'uat'
    case 'main':       return 'production'
    default:           return 'dev'
  }
}

stage('Deploy') {
  steps {
    script {
      def namespace = getNamespace(env.BRANCH_NAME)
      sh "kubectl set image deployment/order-service ... --namespace=${namespace}"
    }
  }
}
```

A commit to `develop` deploys to the `dev` namespace. A merge to `main` deploys to `production`. The same pipeline code handles all environments — only the namespace changes.

---

## Q4. How did observability improvements influence your deployment strategies?

> **Method: ⭐ STAR Method** → Situation → Task → Action → Result

### 📍 Situation
Early in the project, deployments were monitored manually — someone would watch pod status after `kubectl apply` and assume success if nothing visibly crashed. There was no systematic way to know whether a deployment had actually improved or degraded system performance.

### 🎯 Task
Use the observability stack (Prometheus, Grafana, Datadog) as an input to deployment decisions — not just as a post-incident tool.

### ⚙️ Action

**1. Post-deployment health window**

After every production deployment, a 10-minute observation window was added to the runbook. Engineers checked three signals before declaring the deployment stable:

```
Grafana check (5 min post-deploy):
  ✓ Error rate < 1% on deployed service
  ✓ P99 latency within 10% of pre-deploy baseline
  ✓ No new CrashLoopBackOff events in namespace

Datadog APM check:
  ✓ No new error traces introduced by the new commit
  ✓ Downstream services not showing latency increase
```

**2. Metrics informed HPA tuning**

Before observability was in place, HPA thresholds were set by guessing. After 2 weeks of Prometheus metrics showing actual CPU distribution under load, thresholds were adjusted based on evidence:

```
Observed p75 CPU under normal load: ~180m
Observed p99 CPU during traffic spikes: ~420m
HPA threshold set at: 60% of 250m request = 150m

Result: HPA triggers early enough to have new pods ready
before the spike hits — not after users are already affected
```

**3. Canary deployments triggered by error rate baselines**

Observability data established what "normal" looked like for each service. Any deployment that pushed error rate above the established baseline triggered an automatic rollback:

```groovy
stage('Post-Deploy Validation') {
  steps {
    sh '''
      # Query Prometheus for error rate 5 min after deploy
      ERROR_RATE=$(curl -s "http://prometheus:9090/api/v1/query" \
        --data-urlencode \
        'query=rate(http_requests_total{status=~"5..",service="order-service"}[5m])
               / rate(http_requests_total{service="order-service"}[5m])' \
        | jq '.data.result[0].value[1]' | tr -d '"')

      if (( $(echo "$ERROR_RATE > 0.05" | bc -l) )); then
        echo "Error rate ${ERROR_RATE} exceeds 5% threshold — triggering rollback"
        kubectl rollout undo deployment/order-service -n production
        exit 1
      fi
    '''
  }
}
```

### 📊 Result
- Deployment failures caught **within minutes** of release rather than from user reports.
- HPA tuning based on real metrics eliminated the false-scale-down problem that was causing latency spikes.
- Post-deploy validation made engineers more confident deploying frequently — contributing to the **3x increase in release frequency**.

---

## Q5. You mention zero critical vulnerabilities — how do you validate that claim?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

This is the right question to ask. "Zero critical vulnerabilities" is only meaningful if there is a verifiable, reproducible process behind it — not a one-time scan or a developer's assertion.

---

### ✅ What Makes the Claim Verifiable

**Evidence 1 — Trivy scan runs on every build, not just periodically**

The scan is not a scheduled weekly job that could be missed. It runs inside the Jenkins pipeline on every image built for production:

```bash
trivy image \
  --exit-code 1 \          # Build fails if CRITICAL CVEs found
  --severity CRITICAL \
  --ignore-unfixed \
  --format json \
  --output trivy-${GIT_COMMIT:0:8}.json \
  ${ECR_URL}/service:${GIT_COMMIT:0:8}
```

The `--exit-code 1` flag means an image with a CRITICAL CVE is **never pushed to ECR**. It physically cannot reach production.

**Evidence 2 — Scan reports are archived as build artifacts**

```groovy
post {
  always {
    archiveArtifacts artifacts: 'trivy-*.json', allowEmptyArchive: true
  }
}
```

Every build has a linked Trivy JSON report. The claim is auditable — not just asserted.

**Evidence 3 — ECR image scanning as a second independent check**

AWS ECR has native vulnerability scanning powered by Clair. It provides a second scan independent of Trivy, running on every image pushed to the registry:

```bash
# Enable ECR enhanced scanning
aws ecr put-registry-scanning-configuration \
  --scan-type ENHANCED \
  --rules '[{
    "repositoryFilters": [{"filter": "apnakart/*", "filterType": "WILDCARD"}],
    "scanFrequency": "CONTINUOUS_SCAN"
  }]'

# Check scan findings for a specific image
aws ecr describe-image-scan-findings \
  --repository-name apnakart/order-service \
  --image-id imageTag=abc1234 \
  --query 'imageScanFindings.findingSeverityCounts'
```

If Trivy missed something, ECR's independent scan provides a safety net.

---

### ⚠️ Honest Limitations of the Claim

**Limitation 1 — `--ignore-unfixed` means some CVEs are excluded**

CVEs with no available patch are not counted. This is the right operational decision but means "zero critical CVEs" technically means "zero critical CVEs with an available fix."

**Limitation 2 — The claim covers images at build time, not runtime**

A new CVE published after an image is deployed will not be caught until the next rebuild. Continuous ECR scanning mitigates this — it rescans existing images when new CVE data is published.

**Limitation 3 — Trivy scans the image layer, not the running process**

A vulnerability in application logic (business logic flaw, auth bypass) is not a CVE and will not appear in Trivy output. SonarQube covers this layer — but its coverage depends on test quality and rule configuration.

The correct framing: *"Zero critical CVEs with available fixes, in all images at build time, as of their last scan."* That is a strong and defensible claim.

---

## Q6. How does your microservices architecture affect monitoring complexity?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 The Core Problem
Monitoring a monolith means watching one process. Monitoring 10 microservices means watching 10 processes, their interactions with each other, their interactions with 10 databases, their interactions with shared infrastructure, and the emergent behaviour of the whole system under load.

Complexity does not scale linearly. It scales with the number of inter-service connections.

```
10 services with potential connections to each other:
Possible connections = 10 × (10-1) / 2 = 45 interaction paths to monitor
```

---

### 📊 Complexity Added Per Layer

| Layer | Monolith | 10 Microservices |
|---|---|---|
| Processes to monitor | 1 | 10 |
| Databases | 1 | 10 (database-per-service) |
| Network paths | Internal function calls | HTTP/gRPC across services — each can fail independently |
| Log streams | 1 | 10 separate streams to aggregate |
| Deployment units | 1 | 10 — each can be at a different version |
| Failure modes | App crashes | Partial failures, cascading timeouts, circuit breaks |

---

### ⚙️ How Monitoring Was Adapted

**1. RED Method per service, not per cluster**

Every service gets its own Grafana dashboard row with Rate, Errors, and Duration metrics:

```promql
# Request rate per service
rate(http_requests_total{service="order-service"}[5m])

# Error rate per service
rate(http_requests_total{service="order-service", status=~"5.."}[5m])
  / rate(http_requests_total{service="order-service"}[5m])

# P99 latency per service
histogram_quantile(0.99,
  rate(http_request_duration_seconds_bucket{service="order-service"}[5m]))
```

This makes it possible to see at a glance which of the 10 services is degraded — without digging into individual pod logs.

**2. Distributed tracing becomes non-negotiable**

In a monolith, a slow function call is visible in a single stack trace. In 10 microservices, a slow user-facing request might be caused by a timeout deep in service 7, triggered by a database connection pool exhaustion in service 3, caused by a misconfigured HPA in a different deployment window.

Without distributed tracing, this chain is invisible. Datadog APM with trace ID propagation across all services makes it a 30-second investigation.

**3. Centralised log aggregation with service-level filtering**

10 services writing logs to 10 separate pods means 100+ log files at any given time (10 services × multiple replicas). ELK centralises these:

```
# Kibana query — find errors affecting the checkout flow across all services
kubernetes.labels.app: ("order-service" OR "payment-service" OR "cart-service")
AND level: ERROR
AND @timestamp: [now-15m TO now]
```

**4. Namespace-scoped alerts to reduce noise**

Alerting at the cluster level fires for every pod across every namespace. Namespace-scoped alerts fire only for production workloads:

```yaml
# Only alert on production namespace
- alert: PodCrashLooping
  expr: rate(kube_pod_container_status_restarts_total{namespace="production"}[5m]) > 0
  for: 2m
```

---

### ⚖️ Trade-offs
- More services = more dashboards to maintain. Without a consistent naming convention and dashboard template, 10 one-off dashboards become unmanageable.
- Distributed tracing adds overhead (~1–3% CPU per service for the agent). At 10 services this is 10 agents running — not free.
- The monitoring stack itself (Prometheus, Grafana, ELK, Datadog) needs to be monitored. A silent monitoring failure is worse than no monitoring because it creates false confidence.

---

## Q7. How do Docker optimizations (60% image reduction) impact deployment speed?

> **Method: 🧾 Project Walkthrough** → Problem → Tech Stack → Role → Challenges → Solution → Result

### 🧩 Problem
5 microservices in the Flight Reservation system had Docker images built naively — single-stage builds on fat base images. Large images create problems at three points in the deployment lifecycle.

```
Large Image Impact Chain:

Build time     → More layers = longer build (no caching benefit)
Push time      → Larger image = longer ECR push (network bottleneck)
Pull time      → Each EKS node must pull image on first schedule (startup delay)
Attack surface → More OS packages = more CVEs for Trivy to find
```

---

### ⚙️ What Was Changed

**Before — single-stage build:**

```dockerfile
# ❌ Single stage — JDK included in final image, all build tools present
FROM openjdk:11-jdk
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn package -DskipTests
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "target/booking-service.jar"]
```

Problems:
- `openjdk:11-jdk` image: ~628MB
- Maven, source files, build cache all present in the final image
- Final image size: ~700MB+

---

**After — multi-stage build:**

```dockerfile
# ✅ Stage 1 — Build stage (this layer is discarded after build)
FROM maven:3.9-eclipse-temurin-17-alpine AS builder
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline   # Cache dependencies separately — Docker layer cache
COPY src ./src
RUN mvn package -DskipTests

# ✅ Stage 2 — Runtime stage (only this layer ships)
FROM eclipse-temurin:17-jre-alpine AS runtime
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=builder --chown=appuser:appgroup /build/target/booking-service.jar app.jar
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

The final image contains only the JRE and the compiled JAR. Maven, source code, intermediate build files, and the JDK are all left behind in the builder stage.

---

### 📊 Size Reduction Breakdown

| Service | Before | After | Reduction |
|---|---|---|---|
| Booking Service | 710MB | 195MB | 73% |
| Auth Service | 680MB | 190MB | 72% |
| Payment Service | 695MB | 198MB | 72% |
| Notification Service | 650MB | 185MB | 72% |
| API Gateway | 720MB | 210MB | 71% |
| **Average** | **691MB** | **196MB** | **~60% reduction** |

---

### ⚙️ How This Accelerated Deployment Speed

**1. Faster ECR push on every build:**

```
Before: ~700MB push at ~100Mbps ≈ 56 seconds
After:  ~200MB push at ~100Mbps ≈ 16 seconds
Saving: ~40 seconds per service per build
```

**2. Faster EKS node cold pull (new node joins cluster or pod rescheduled):**

When a pod is scheduled onto a node that does not have the image cached, it pulls from ECR. 200MB pulls 3.5x faster than 700MB — directly reducing pod startup time during scale-out events.

**3. Fewer Trivy findings = fewer pipeline blocks:**

```
Before multi-stage (openjdk:11-jdk base): 40+ Trivy findings (mostly OS packages)
After multi-stage (eclipse-temurin:17-jre-alpine): 4 findings (0 CRITICAL)
```

Fewer findings means fewer unplanned pipeline breaks to investigate and fix before a deployment can proceed.

**4. Docker layer caching for `dependency:go-offline`:**

```dockerfile
COPY pom.xml .
RUN mvn dependency:go-offline   # This layer is cached unless pom.xml changes
COPY src ./src
RUN mvn package -DskipTests     # Only this layer re-runs on source code changes
```

By separating dependency download from compilation, Maven dependencies are only re-downloaded when `pom.xml` changes — not on every source code commit. This alone removed 3–5 minutes from build time on dependency-heavy services.

---

### 📊 Cumulative Impact on Full Pipeline

```
Before optimisations:
  Build:  8 min (single stage, no cache)
  Scan:   3 min (40+ findings to process)
  Push:   ~1 min (700MB)
  Pull:   ~1 min (700MB, cold node)
  Total per service: ~13 min

After optimisations:
  Build:  2 min (multi-stage, dependency cache)
  Scan:   1 min (4 findings)
  Push:   20 sec (200MB)
  Pull:   15 sec (200MB, cold node)
  Total per service: ~3.5 min
```

Docker optimisation alone contributed roughly 3–4 minutes of the total pipeline time reduction.

---

*Answers follow structured interview methods mapped to question type. Cross-questions explicitly connect multiple parts of the experience to demonstrate systemic understanding — not just tool knowledge in isolation.*

---

# ⚙️ Real-Time Scenario-Based Questions

> These questions test decision-making under pressure — not just tool knowledge. Every answer follows a structured action plan with concrete commands.

---

# 🚀 Scenario: Deployment & Releases

## Q1. A production deployment fails halfway — what steps will you take?

> **Method: 🚨 Action Plan Method** → Clarify → Immediate Action → Investigation → Communication → Prevention

### 🔍 Clarify First
"Fails halfway" has two distinct meanings with different responses:

| Failure Point | What Happened | Response |
|---|---|---|
| Pipeline failure (pre-deploy) | Build/scan/push failed — nothing reached Kubernetes | Fix the pipeline. No rollback needed. |
| Rollout failure (mid-deploy) | New pods started but are not healthy — old pods may already be terminating | **Immediate rollback required** |

---

### ⚡ Immediate Action — Rollout Failure

```bash
# Step 1 — Confirm the rollout is actually stuck or failing
kubectl rollout status deployment/order-service -n production
# Output: Waiting for rollout to finish: 2 out of 3 new replicas updated...
# Or: error: deployment "order-service" exceeded its progress deadline

# Step 2 — Rollback immediately — do not wait to investigate
kubectl rollout undo deployment/order-service -n production

# Step 3 — Confirm rollback completed and old pods are healthy
kubectl rollout status deployment/order-service -n production
kubectl get pods -n production -l app=order-service
```

**Why rollback before investigating:** Every minute a broken deployment is partially live means some percentage of user requests are hitting the bad pods. Restore service first, investigate second.

---

### 🩺 Investigation — After Service Is Restored

```bash
# Check what the failing pods were doing
kubectl describe pod <failing-pod-name> -n production
# Look at Events section — scheduling failures, probe failures, OOMKill

# Read the logs from the failed pods
kubectl logs <failing-pod-name> -n production --previous

# Compare the failed image with the previous good image
kubectl rollout history deployment/order-service -n production
# REVISION  CHANGE-CAUSE
# 3         deploy: abc1234 (failed)
# 2         deploy: def5678 (current, rolled back to)

# Check what changed between revisions
git diff def5678 abc1234
```

Common root causes:

| Symptom | Likely Cause |
|---|---|
| `CrashLoopBackOff` | App crash on startup — check logs for exception |
| `OOMKilled` | Memory limit too low for new version, or memory leak introduced |
| Readiness probe timeout | App takes longer to start — increase `initialDelaySeconds` |
| `ImagePullBackOff` | ECR push succeeded but image tag mismatch or auth issue |
| Pods stuck in `Pending` | Insufficient node capacity for new replicas |

---

### 📢 Communication
While rollback is running:

```
Slack #prod-alerts:
"⚠️ Production rollback in progress — order-service deployment abc1234 failed.
Rolling back to def5678. ETA to stable: 3 minutes.
Users may see intermittent errors. Investigating root cause."
```

After rollback confirmed:

```
"✅ Rollback complete. order-service stable on def5678.
Root cause: [finding]. Fix in progress — redeployment ETA: [time]."
```

---

### 🛡️ Prevention
- `maxUnavailable: 0` ensures old pods are never terminated before new ones are healthy.
- Jenkins `rollout status --timeout=120s` catches stuck rollouts automatically and blocks the build from being marked green.
- Post-deploy Prometheus query validates error rate before declaring deployment complete.

---

## Q2. You need to deploy a hotfix urgently — how do you handle it safely?

> **Method: 🚨 Action Plan Method** → Clarify → Immediate Action → Investigation → Communication → Prevention

### 🔍 Clarify
"Urgently" does not mean "skip everything." It means compress the timeline without skipping the gates that matter.

The question to ask: **what is the blast radius if the hotfix itself is broken?**

---

### ⚡ Hotfix Process

**Step 1 — Branch from the production tag, not from develop**

```bash
# Find the current production commit
kubectl get deployment order-service -n production \
  -o jsonpath='{.spec.template.spec.containers[0].image}'
# Output: <ecr>/order-service:abc1234

# Create hotfix branch from that exact commit — not from develop
git checkout -b hotfix/payment-timeout abc1234
git cherry-pick <fix-commit>   # Or apply fix directly
git push origin hotfix/payment-timeout
```

This guarantees the hotfix contains only the production code plus the fix — not 2 weeks of unmerged develop changes.

---

**Step 2 — Run the full pipeline, but targeted**

```groovy
// Jenkinsfile — hotfix branches skip non-critical stages, keep security gates
when {
  branch 'hotfix/*'
}
stages {
  // ✅ Keep — non-negotiable
  stage('Compile')         { ... }
  stage('Unit Tests')      { ... }
  stage('SonarQube')       { ... }
  stage('Quality Gate')    { ... }
  stage('Docker Build')    { ... }
  stage('Trivy Scan')      { ... }
  stage('ECR Push')        { ... }

  // ⏭️ Skip — only for hotfix branches
  // Integration tests, performance tests, full regression suite
  // These run on post-hotfix validation, not blocking it

  stage('Deploy to Production') { ... }
}
```

Security gates (Trivy, SonarQube) are never skipped. A hotfix that introduces a new critical CVE is not a fix.

---

**Step 3 — Deploy to production directly, bypassing lower environments**

```bash
# Hotfix branch maps directly to production namespace
kubectl set image deployment/order-service \
  order-service=${ECR_URL}/order-service:${HOTFIX_TAG} \
  --namespace=production

# Watch the rollout in real time
kubectl rollout status deployment/order-service \
  --namespace=production \
  --timeout=120s
```

---

**Step 4 — Validate immediately after deploy**

```bash
# Manual smoke test — hit the affected endpoint directly
curl -X POST https://api.apnakart.com/orders \
  -H "Authorization: Bearer <test-token>" \
  -d '{"productId": "123", "quantity": 1}'

# Check Grafana — error rate should drop within 2 minutes of hotfix deploy
# Check Datadog APM — confirm the specific error trace no longer appears
```

---

**Step 5 — Back-merge into develop immediately after**

```bash
git checkout develop
git merge hotfix/payment-timeout
git push origin develop
```

If this is skipped, the hotfix gets overwritten by the next regular deployment from develop.

---

### ⚖️ What Gets Compressed vs What Never Gets Skipped

| Stage | Normal Deploy | Hotfix Deploy |
|---|---|---|
| Unit tests | ✅ Run | ✅ Run |
| SonarQube | ✅ Run | ✅ Run |
| Trivy scan | ✅ Run | ✅ Run |
| Integration tests | ✅ Run | ⏭️ Post-deploy |
| Performance tests | ✅ Run | ⏭️ Skip |
| Deploy to dev/test/uat | ✅ All stages | ⏭️ Skip — straight to prod |
| Post-deploy validation | ✅ Automated | ✅ Manual + automated |

---

## Q3. A new release increases latency — how do you investigate?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Identify
```bash
# Confirm it is the new release causing it — check timing in Grafana
# Latency spike correlates with deployment timestamp?

# Check deployment history
kubectl rollout history deployment/order-service -n production
# Look for revision timestamp matching the latency spike
```

---

### 🩺 Diagnose — Work Through Layers

**Layer 1 — Is the latency isolated to one service or the full request chain?**

```promql
# Grafana — P99 latency per service
histogram_quantile(0.99,
  rate(http_request_duration_seconds_bucket[5m]))
  by (service)
```

If only `order-service` shows increased P99 — the issue is in that service.
If `payment-service` and `cart-service` also show it — the new release is causing downstream cascade.

---

**Layer 2 — CPU throttling vs actual slowness**

```bash
kubectl top pods -n production -l app=order-service

# If CPU is near the limit — the pod is being throttled, not genuinely slow
# CPU throttling looks like latency but is caused by resource limits
```

```promql
# Check throttle ratio
rate(container_cpu_cfs_throttled_seconds_total{container="order-service"}[5m])
/ rate(container_cpu_cfs_periods_total{container="order-service"}[5m])
# > 0.25 means 25%+ of CPU time is being throttled
```

If throttling is the cause: temporarily increase CPU limit or request to confirm, then fix the code.

---

**Layer 3 — Database query regression**

New code often introduces N+1 queries or missing indexes. Check RDS slow query log:

```bash
# Enable and query RDS slow query log via CloudWatch
aws logs filter-log-events \
  --log-group-name /aws/rds/instance/apnakart-order-db/slowquery \
  --start-time $(date -d '30 minutes ago' +%s000) \
  --filter-pattern "Query_time"
```

---

**Layer 4 — Datadog APM trace comparison**

```
Compare traces from before and after the deployment:
→ Open a slow trace from after the release
→ Find which span is consuming the most time
→ Compare to a fast trace from the previous release

Common findings:
- New synchronous external API call added (payment service calling auth on every request)
- Missing database index on a new query
- N+1 query in a new ORM relationship
- JSON serialisation of a large response object added
```

---

### 🔧 Fix Options

| Root Cause | Fix |
|---|---|
| CPU throttling | Increase `resources.limits.cpu` or optimise code |
| New slow DB query | Add index, rewrite query, or add caching |
| Missing async processing | Move non-critical work to async queue |
| External API call in hot path | Cache response or make call async |

---

### 🛡️ Prevention
- Add P99 latency baseline check as a post-deploy validation gate.
- Performance regression test suite runs on the hotfix branch before production merge.

---

---

# 📈 Scenario: Scaling & Load

## Q4. Traffic suddenly increases 5x — how will your system respond?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### ⚙️ Automatic Response Chain (No Human Intervention Needed)

A 5x traffic spike triggers a cascade of automatic responses across three layers:

```
Traffic 5x spike hits ALB
         ↓
ALB distributes across healthy pods (existing capacity absorbs initial burst)
         ↓
Pod CPU/memory rises above HPA threshold
         ↓
HPA triggers — adds pods (up to maxReplicas)
         ↓
New pods scheduled on existing nodes
         ↓
If nodes are full — Cluster Autoscaler adds new EC2 nodes
         ↓
New nodes join EKS, new pods schedule on them
         ↓
Traffic absorbed — latency returns to baseline
```

---

### ⚙️ HPA Configuration for Spike Absorption

```yaml
spec:
  minReplicas: 3          # Baseline absorbs moderate spikes without scaling
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60    # Triggers early — at 60%, not 90%
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 0    # No delay on scale-up
      policies:
      - type: Pods
        value: 3                       # Add 3 pods at once during spike
        periodSeconds: 30
```

---

### ⚙️ Cluster Autoscaler Response

```bash
# Cluster Autoscaler detects pods in Pending state (no node capacity)
# Triggers ASG scale-out — new EC2 nodes provisioned in ~2-3 minutes

# Monitor Cluster Autoscaler activity
kubectl logs -n kube-system \
  -l app=cluster-autoscaler \
  --tail=50 | grep -E "scale up|adding node"
```

New nodes take 2–3 minutes to join the cluster. **HPA and minReplicas must absorb the first 2–3 minutes of the spike** — this is why baseline replicas and early HPA thresholds matter.

---

### ⚠️ What Can Still Break at 5x

| Component | Risk at 5x | Mitigation |
|---|---|---|
| RDS connection pool | Too many pods = too many DB connections | PgBouncer connection pooler in front of RDS |
| External API rate limits | 5x calls to payment gateway hits rate limit | Circuit breaker + retry with backoff |
| EKS node bootstrap time | New nodes take 2–3 min | Warm pool in ASG or higher minReplicas |
| ALB target registration | New pods take ~30s to register with ALB | `deregistration_delay.timeout_seconds` tuning |

---

## Q5. HPA is not scaling properly — what will you check first?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### Structured Diagnostic Sequence

```bash
# Step 1 — What does HPA think is happening?
kubectl describe hpa order-service-hpa -n production

# Key fields to check:
# Conditions:
#   AbleToScale: True/False — can HPA actually scale?
#   ScalingActive: True/False — is scaling logic running?
#   ScalingLimited: True/False — is it hitting min/max boundary?
# Current Metrics: <unknown> or actual value?
# Events: any error messages?
```

---

| Finding | Root Cause | Fix |
|---|---|---|
| `TARGETS: <unknown>/60%` | Metrics Server not running OR `resources.requests` not set on pod | Install Metrics Server; add resource requests |
| `ScalingLimited: True` | Already at `maxReplicas` | Increase `maxReplicas` or add more nodes |
| `AbleToScale: False` | HPA cannot modify the deployment | Check RBAC — HPA needs permission to patch Deployments |
| Correct metrics, no scale | Within stabilisation window | Wait, or reduce `stabilizationWindowSeconds` |
| Metrics correct, ScalingActive: False | Deployment has 0 replicas | HPA does not manage 0-replica deployments |

---

```bash
# Step 2 — Is Metrics Server running?
kubectl get deployment metrics-server -n kube-system
kubectl top pods -n production    # If this fails, Metrics Server is the problem

# Step 3 — Do pods have resource requests defined?
kubectl get pod <pod-name> -n production -o jsonpath=\
  '{.spec.containers[0].resources}'
# Must show: {"requests":{"cpu":"250m","memory":"256Mi"}}

# Step 4 — Is the HPA actually computing metrics?
kubectl get hpa order-service-hpa -n production
# TARGETS should show actual/threshold — e.g., 72%/60%
# Not <unknown>/60%
```

---

## Q6. Your application is CPU-bound — what optimization steps would you take?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🔍 Diagnose Before Optimising

```bash
# Confirm CPU-bound (not I/O-bound, not memory-bound)
kubectl top pods -n production --sort-by=cpu

# Check throttle ratio — are pods hitting their CPU limit?
# High throttle = limit too low OR genuine CPU inefficiency
```

---

### 🔄 Options and When to Use Each

**Option 1 — Increase CPU limit (immediate, not a real fix)**

```yaml
resources:
  limits:
    cpu: "1000m"    # Was 500m
```

This buys time. It does not fix the underlying cause. Only use this to stop a production incident while the real fix is being deployed.

---

**Option 2 — Profile the application to find the hot path**

For Spring Boot microservices, async Profiler identifies which method is consuming CPU:

```bash
# Run async-profiler inside the container
kubectl exec -it order-service-xxx -n production -- /bin/sh

# Inside container — attach profiler to JVM process
java -jar async-profiler.jar -d 30 -f /tmp/cpu-profile.html $(pgrep java)
```

Most common findings: JSON serialisation of large objects, regex in hot loops, synchronous blocking calls in async context.

---

**Option 3 — Move synchronous work to async processing**

```java
// ❌ CPU-bound: notification sent synchronously on every order
public Order createOrder(OrderRequest req) {
  Order order = orderRepository.save(req);
  notificationService.sendEmail(order);   // Blocks thread, consumes CPU
  return order;
}

// ✅ Async: publish to queue, notification service consumes separately
public Order createOrder(OrderRequest req) {
  Order order = orderRepository.save(req);
  eventPublisher.publish(new OrderCreatedEvent(order.getId()));  // Non-blocking
  return order;
}
```

---

**Option 4 — Add caching to eliminate repeated computation**

```java
// ❌ Product catalogue fetched and processed on every request
public Product getProduct(String id) {
  return productRepository.findById(id)
    .map(this::applyPricingRules);   // Pricing calculation is CPU-intensive
}

// ✅ Cache the result — pricing rules don't change per-request
@Cacheable(value = "products", key = "#id")
public Product getProduct(String id) {
  return productRepository.findById(id)
    .map(this::applyPricingRules);
}
```

---

**Option 5 — JVM tuning for container environments**

```dockerfile
ENTRYPOINT ["java",
  "-XX:+UseContainerSupport",          # JVM respects container CPU limits
  "-XX:MaxRAMPercentage=75.0",
  "-XX:+UseG1GC",                      # G1GC has lower pause times than default
  "-XX:ParallelGCThreads=2",           # Limit GC threads to avoid CPU contention
  "-jar", "app.jar"]
```

Without `UseContainerSupport`, the JVM reads the node's total CPU count (8 cores) and creates thread pools sized for 8 cores — inside a container limited to 500m. This causes massive thread contention.

---

---

# 🤝 Scenario: Collaboration

## Q7. Developers push unstable code frequently — how would you handle this?

> **Method: 🤝 Resolution Method** → Problem → Perspective → Action → Resolution

### 🧩 Problem
Developers pushing unstable code to shared branches disrupts CI/CD pipelines, breaks test environments, and slows down the whole team.

### 👁️ Perspective
Before implementing gates, understand the cause. Developers push unstable code for two reasons:
1. **No fast feedback loop** — they push and find out 40 minutes later the build broke.
2. **No enforcement** — gates exist in the pipeline but can be bypassed.

Enforcement without fast feedback creates friction without fixing the root problem.

---

### ⚙️ Action — Three-Layer Fix

**Layer 1 — Pre-commit hooks (catch issues before push, not after)**

```bash
# Install pre-commit hooks in the repo
# .pre-commit-config.yaml
repos:
- repo: https://github.com/pre-commit/pre-commit-hooks
  hooks:
  - id: check-yaml
  - id: check-json
  - id: detect-private-key        # Catch hardcoded secrets
  - id: end-of-file-fixer

- repo: local
  hooks:
  - id: mvn-compile
    name: Maven Compile Check
    entry: mvn compile -q
    language: system
    pass_filenames: false
```

A developer gets feedback in 10 seconds instead of 10 minutes. Most trivial breakages are caught before the push even leaves the laptop.

---

**Layer 2 — Branch protection + required status checks**

```yaml
# GitHub branch protection on develop and main:
required_status_checks:
  strict: true
  contexts:
    - "jenkins/compile"
    - "jenkins/unit-tests"
    - "jenkins/sonarqube"
required_pull_request_reviews:
  required_approving_review_count: 1
restrict_pushes:
  push_allowances: []   # Nobody can force-push, including admins
```

Code cannot merge unless CI passes. This is not punitive — it is structural. The pipeline enforces quality, not a manager.

---

**Layer 3 — Short-lived feature branches with small PRs**

```
❌ Pattern causing problems:
  Developer works on feature for 2 weeks
  Opens one giant PR with 40 files changed
  Merge conflicts everywhere, tests failing, unstable

✅ Better pattern (trunk-based development):
  Developer works in feature branch ≤ 2 days
  Opens small PR: 1 feature, 1 bug fix
  CI passes quickly on small diff
  Merged frequently — develop stays stable
```

---

### 🔗 Resolution
Present this as a process improvement, not a blame conversation. The framing to developers:

*"Pre-commit hooks give you feedback in 10 seconds instead of waiting for the pipeline. Branch protection means once CI passes, you know your code is stable — you don't have to worry about someone else's push breaking your environment."*

---

## Q8. QA reports intermittent bugs — how do you debug environment-related issues?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### 🔎 Identify
"Intermittent" is the critical word. It means the bug is not deterministic — it depends on state, timing, load, or environment differences. Reproduce it before debugging it.

```bash
# First question: does it happen in all environments or just one?
# prod: yes | uat: no → environment config difference
# prod: yes | uat: yes | dev: no → probably not config — likely a data or timing issue
```

---

### 🩺 Diagnose

**Check 1 — Environment variable and config differences**

```bash
# Compare ConfigMaps across environments
kubectl get configmap order-service-config -n production -o yaml > prod-config.yaml
kubectl get configmap order-service-config -n uat -o yaml > uat-config.yaml
diff prod-config.yaml uat-config.yaml

# Compare secrets (check key names, not values)
kubectl get secret order-service-secret -n production -o jsonpath='{.data}' | jq 'keys'
kubectl get secret order-service-secret -n uat -o jsonpath='{.data}' | jq 'keys'
```

---

**Check 2 — Resource differences (QA env may have lower limits causing throttling)**

```bash
kubectl describe deployment order-service -n uat | grep -A10 "Limits\|Requests"
kubectl describe deployment order-service -n production | grep -A10 "Limits\|Requests"
```

A pod running with 128Mi memory in UAT and 512Mi in production will behave differently under load.

---

**Check 3 — Replica count and load balancing**

```bash
# If UAT has 1 replica and prod has 3 — intermittent bugs may be sticky-session issues
kubectl get deployment order-service -n uat
kubectl get deployment order-service -n production
```

Intermittent bugs that affect 1 in 3 requests often indicate a bug in one specific pod — caused by different startup state, different cached data, or a race condition on init.

---

**Check 4 — Log correlation during the intermittent failure window**

```bash
# Kibana query — find errors in the 30-second window when QA reported the bug
kubernetes.namespace: uat
AND level: ERROR
AND @timestamp: [2025-03-15T14:30:00 TO 2025-03-15T14:31:00]
```

---

**Check 5 — Pod restart history**

```bash
# Has any pod restarted recently? Restart clears in-memory state
kubectl get pods -n uat -l app=order-service
# RESTARTS column — non-zero restarts explain intermittent state loss
```

---

### 🛡️ Prevention
- Helm values files for each environment stored in Git — config drift is visible in PR history.
- Environment parity policy: UAT resource limits must match production within 20% to catch resource-related issues before they reach prod.

---

---

# 💰 Scenario: Cost & Optimization

## Q9. AWS bill spikes unexpectedly — how do you investigate?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### ⚡ Immediate Action

```bash
# Step 1 — Open AWS Cost Explorer
# Filter by: Service, then sort by highest cost increase vs previous period
# Common culprits: EC2, NAT Gateway, data transfer, EKS, RDS

# Step 2 — Check for runaway EC2/EKS instances
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[].Instances[].[InstanceId,InstanceType,LaunchTime,Tags]' \
  --output table
# Look for instances launched recently that shouldn't be there
```

---

### 🩺 Common Spike Causes and Diagnostics

**Cause 1 — Cluster Autoscaler scaled up and never scaled down**

```bash
# Check current node count vs expected
kubectl get nodes
# Too many nodes? Autoscaler might be stuck

# Check Cluster Autoscaler logs for scale-down blocked reasons
kubectl logs -n kube-system \
  -l app=cluster-autoscaler | grep "scale down"
# Common blocker: pod with no PodDisruptionBudget blocking node drain
```

**Cause 2 — NAT Gateway data transfer spike**

```bash
# Enable VPC Flow Logs and check in CloudWatch Logs Insights
fields @timestamp, srcAddr, dstAddr, bytes
| filter dstAddr not like /^10\./        # Traffic leaving VPC
| stats sum(bytes) as total_bytes by srcAddr
| sort total_bytes desc
| limit 20
```

A pod making unexpected external API calls or an S3 bucket accessed over internet instead of VPC endpoint both show up here.

**Cause 3 — S3 request costs (GetObject at scale)**

```bash
# Check S3 request metrics in CloudWatch
aws cloudwatch get-metric-statistics \
  --namespace AWS/S3 \
  --metric-name NumberOfObjects \
  --dimensions Name=BucketName,Value=apnakart-assets \
  --start-time 2025-03-01T00:00:00Z \
  --end-time 2025-03-15T00:00:00Z \
  --period 86400 \
  --statistics Average
```

Fix: add CloudFront in front of S3 — CDN serves cached responses, S3 GetObject requests drop dramatically.

**Cause 4 — RDS Multi-AZ accidentally enabled on non-prod**

```bash
aws rds describe-db-instances \
  --query 'DBInstances[].[DBInstanceIdentifier,MultiAZ,DBInstanceClass]' \
  --output table
# Multi-AZ doubles the RDS cost — should only be on production
```

---

### 🛡️ Prevention

```bash
# Set AWS Budget alerts — notify before the bill, not after
aws budgets create-budget \
  --account-id <account-id> \
  --budget '{
    "BudgetName": "apnakart-monthly",
    "BudgetLimit": {"Amount": "500", "Unit": "USD"},
    "TimeUnit": "MONTHLY",
    "BudgetType": "COST"
  }' \
  --notifications-with-subscribers '[{
    "Notification": {
      "NotificationType": "ACTUAL",
      "ComparisonOperator": "GREATER_THAN",
      "Threshold": 80
    },
    "Subscribers": [{"SubscriptionType": "EMAIL",
                     "Address": "devops@apnakart.com"}]
  }]'
```

A budget alert at 80% of monthly spend gives time to investigate before the spike becomes a bill.

---

## Q10. You are asked to reduce infrastructure cost by 20% — what will you do?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 🧩 Approach
20% is a specific target. Work from highest-cost services first — cutting 20% on a $10/month service saves $2. Cutting 20% on a $400/month service saves $80.

**Step 1 — Cost breakdown by service (AWS Cost Explorer):**

```
Typical ApnaKart cost distribution:
EKS + EC2 nodes:   ~45% of bill
RDS (10 instances): ~30% of bill
NAT Gateway:        ~10% of bill
Data transfer:       ~8% of bill
Everything else:     ~7% of bill
```

Attack the top 75% of the bill first.

---

### ✅ Reduction Actions by Category

**EKS / EC2 — Target: 15–20% saving**

```hcl
# Action 1: Convert non-prod node groups to Spot
resource "aws_eks_node_group" "dev" {
  capacity_type  = "SPOT"
  instance_types = ["t3.medium", "t3.large", "t3a.medium"]  # Multiple types
  # Saving: ~65% vs On-Demand
}

# Action 2: Production — move from On-Demand to 1-year Reserved
# Done via AWS console: EC2 → Reserved Instances → Purchase
# Saving: ~37% vs On-Demand

# Action 3: Right-size node instance type using CloudWatch metrics
# If nodes are consistently at 20% CPU — drop from t3.large to t3.medium
```

**RDS — Target: 20–30% saving on non-prod**

```bash
# Action 1: Stop non-prod RDS instances outside business hours
# Lambda + EventBridge cron: stop at 20:00, start at 08:00 weekdays
# Saving: ~55% (8 hours running vs 24 hours)

# Action 2: Right-size — use t3.micro for dev, t3.small for test
# 10 instances at t3.micro vs t3.medium: ~60% cheaper

# Action 3: Disable Multi-AZ on non-production
aws rds modify-db-instance \
  --db-instance-identifier apnakart-order-test \
  --no-multi-az \
  --apply-immediately
```

**NAT Gateway — Target: 30–50% saving**

```bash
# Action 1: Add S3 VPC Gateway Endpoint — S3 traffic bypasses NAT Gateway
# Gateway endpoints are FREE — this is the highest ROI change

resource "aws_vpc_endpoint" "s3" {
  vpc_id       = aws_vpc.main.id
  service_name = "com.amazonaws.ap-south-1.s3"
  route_table_ids = [aws_route_table.private.id]
}

# Action 2: Add ECR VPC Interface Endpoint — Docker pulls bypass NAT
resource "aws_vpc_endpoint" "ecr_dkr" {
  vpc_id              = aws_vpc.main.id
  service_name        = "com.amazonaws.ap-south-1.ecr.dkr"
  vpc_endpoint_type   = "Interface"
  subnet_ids          = var.private_subnet_ids
  security_group_ids  = [aws_security_group.vpc_endpoints.id]
}
```

EKS nodes pulling Docker images from ECR generate significant NAT Gateway data transfer costs. A VPC endpoint routes this traffic internally — no NAT Gateway charge.

---

### 📊 Estimated Savings Summary

| Action | Effort | Estimated Monthly Saving |
|---|---|---|
| Spot instances for non-prod EKS | Low — Terraform change | ~$60–80 |
| Reserved instances for prod nodes | Low — AWS console | ~$50–70 |
| Stop non-prod RDS off-hours | Medium — Lambda automation | ~$40–60 |
| Right-size non-prod RDS | Low — Terraform change | ~$30–40 |
| S3 + ECR VPC endpoints | Low — Terraform change | ~$20–30 |
| **Total** | | **~$200–280/month** |

On a $500–800/month infrastructure bill, these actions achieve the 20% target without touching production reliability.

### ⚖️ What Not to Cut
- Production RDS Multi-AZ — this is a reliability control, not a cost inefficiency.
- Production node count minimums — scaling to zero in production to save money is not an option.
- Monitoring stack — cutting Datadog or Prometheus to save cost increases MTTR and creates hidden costs.

---

*Answers follow structured interview methods. All commands are production-applicable. All cost numbers are estimates based on ap-south-1 pricing — actual figures depend on current AWS rates and exact workload.*

---

# 🛠️ Real-Time Troubleshooting Scenarios

> The critical section. Every answer follows the Root Cause Method: Identify → Diagnose → Fix → Prevent.
> Commands are ordered from fastest to slowest — start at the top, stop when you find the cause.

---

# ⎈ Kubernetes Issues

## Q1. A pod is stuck in `CrashLoopBackOff` — how do you debug it step-by-step?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### What CrashLoopBackOff Actually Means
The container starts, crashes immediately, Kubernetes restarts it, it crashes again. The backoff is Kubernetes adding exponential delay (10s → 20s → 40s → 80s → 5min) between restart attempts to avoid hammering a broken process.

The crash is always caused by something inside the container — not Kubernetes itself.

---

### Step-by-Step Diagnostic Sequence

**Step 1 — Read the exit code**

```bash
kubectl describe pod <pod-name> -n production

# Look for:
# Last State: Terminated
#   Reason: Error / OOMKilled / Completed
#   Exit Code: 1 / 137 / 0
```

| Exit Code | Meaning |
|---|---|
| `0` | Process exited cleanly — but readiness probe still failing |
| `1` | Application error — unhandled exception, config missing |
| `137` | OOMKilled — container exceeded memory limit (128 + 9 = 137) |
| `139` | Segfault — rare in JVM apps, common in native binaries |
| `143` | SIGTERM not handled — graceful shutdown not implemented |

---

**Step 2 — Read the previous container logs (before current restart)**

```bash
# Current logs (may be empty if crash is immediate)
kubectl logs <pod-name> -n production

# Logs from the crashed container — this is usually where the answer is
kubectl logs <pod-name> -n production --previous
```

Most common log findings:

```
# Missing environment variable
java.lang.NullPointerException: DB_URL environment variable is null

# Wrong database credentials
com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Connection refused

# Port already in use (two containers sharing same port)
java.net.BindException: Address already in use: 8080

# OOM before JVM OOMKilled signal
java.lang.OutOfMemoryError: Java heap space
```

---

**Step 3 — Check Events section for infrastructure-level failures**

```bash
kubectl describe pod <pod-name> -n production | grep -A20 "Events:"

# Common events:
# Warning  BackOff    Restarting failed container
# Warning  OOMKilling  Memory limit reached
# Warning  Failed     Failed to pull image — ImagePullBackOff
# Warning  Unhealthy  Liveness probe failed
```

---

**Step 4 — Verify environment variables and secrets are injected**

```bash
# Check if the ConfigMap and Secret the pod depends on actually exist
kubectl get configmap order-service-config -n production
kubectl get secret order-service-secret -n production

# Check what env vars the pod sees (only works if pod is briefly alive)
kubectl exec <pod-name> -n production -- env | grep -E "DB_|APP_|SPRING_"
```

A secret that was deleted or renamed causes immediate CrashLoopBackOff with no obvious log output.

---

**Step 5 — If OOMKilled — check memory usage trend**

```bash
# Current memory usage
kubectl top pod <pod-name> -n production

# Historical — Grafana query
container_memory_working_set_bytes{pod="<pod-name>", namespace="production"}
```

Fix: increase memory limit or profile for memory leaks.

```yaml
resources:
  limits:
    memory: "512Mi"    # Increase from 256Mi
```

---

**Step 6 — Run the image locally to reproduce**

```bash
docker run --rm \
  -e DB_URL=jdbc:postgresql://localhost:5432/orders \
  -e DB_PASSWORD=test \
  -p 8080:8080 \
  <ecr_url>/order-service:abc1234
```

If it crashes locally with the same error, the issue is in the application — not the cluster configuration.

---

### Prevention
- Resource limits set correctly (prevent OOMKill).
- Liveness probe `initialDelaySeconds` long enough for JVM startup (prevent probe-induced crash loops).
- All required environment variables documented and validated in Helm `values.schema.json`.

---

## Q2. Service is not reachable via Ingress — what could be wrong?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### Ingress Traffic Path

```
User → DNS → ALB / Ingress Controller → Service (ClusterIP) → Pod
```

A break at any point in this chain produces "not reachable." Debug the chain from outside in.

---

**Step 1 — Is DNS resolving to the right address?**

```bash
nslookup api.apnakart.com
# Should return the ALB DNS name or IP

dig api.apnakart.com
# Check TTL — stale DNS cache can cause "not reachable" after a new deployment
```

---

**Step 2 — Is the Ingress resource configured correctly?**

```bash
kubectl get ingress -n production
# Check ADDRESS column — should show ALB DNS name
# Empty ADDRESS = Ingress Controller not processing this resource

kubectl describe ingress order-service-ingress -n production
# Check:
# Rules — correct host and path?
# Backend — correct service name and port?
# Events — any errors from the Ingress controller?
```

Common misconfigurations:

```yaml
# ❌ Wrong — service name typo or wrong port
spec:
  rules:
  - host: api.apnakart.com
    http:
      paths:
      - path: /orders
        backend:
          service:
            name: order-sevice     # Typo — 'service' not 'sevice'
            port:
              number: 8080

# ❌ Wrong — pathType missing (required in networking.k8s.io/v1)
        pathType: Prefix           # Must be Prefix, Exact, or ImplementationSpecific
```

---

**Step 3 — Does the Service exist and have endpoints?**

```bash
# Service exists?
kubectl get service order-service -n production

# Service has endpoints (pods selected)?
kubectl get endpoints order-service -n production
# If ENDPOINTS shows <none> — selector labels don't match pod labels

# Verify label match
kubectl get service order-service -n production -o jsonpath='{.spec.selector}'
# {"app":"order-service"}

kubectl get pods -n production -l app=order-service
# Should return pods — if empty, label mismatch
```

---

**Step 4 — Are the pods actually healthy and ready?**

```bash
kubectl get pods -n production -l app=order-service
# READY column must show 1/1 — not 0/1

# 0/1 means pod is running but readiness probe is failing
# Traffic will not be sent to unready pods
kubectl describe pod <pod-name> -n production | grep -A5 "Readiness"
```

---

**Step 5 — Is the Ingress Controller running?**

```bash
kubectl get pods -n ingress-nginx    # NGINX Ingress
# OR
kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-load-balancer-controller

# Check Ingress Controller logs for this specific ingress
kubectl logs -n ingress-nginx \
  -l app.kubernetes.io/name=ingress-nginx \
  --tail=50 | grep "order-service"
```

---

**Step 6 — TLS/HTTPS issues**

```bash
# Check certificate is valid and not expired
kubectl describe ingress order-service-ingress -n production | grep -i tls

# Test with curl — check for certificate errors
curl -v https://api.apnakart.com/orders
# Look for: SSL certificate verify result or handshake failure
```

---

## Q3. Pods are running but application is failing — what checks will you perform?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### Why This Is a Distinct Problem
Pod status `Running` only means the container process started. It says nothing about whether the application inside is functioning correctly. A pod can be `Running 1/1` while returning 500 errors on every request.

---

**Step 1 — Check readiness — is pod actually receiving traffic?**

```bash
kubectl get pods -n production
# READY: 0/1 = pod running but readiness probe failing = no traffic sent to it
# READY: 1/1 = pod passing readiness probe = receiving traffic

kubectl describe pod <pod-name> -n production | grep -A10 "Readiness"
# "Readiness probe failed: HTTP probe failed with statuscode: 503"
```

---

**Step 2 — Check application logs for runtime errors**

```bash
kubectl logs <pod-name> -n production --tail=100

# Filter for errors only
kubectl logs <pod-name> -n production | grep -E "ERROR|Exception|FATAL"

# Follow live — reproduce the failure while watching
kubectl logs -f <pod-name> -n production
```

---

**Step 3 — Test the application directly (bypass Ingress and Service)**

```bash
# Port-forward directly to the pod — removes network layer variables
kubectl port-forward pod/<pod-name> 8080:8080 -n production

# In another terminal — hit the endpoint directly
curl -v http://localhost:8080/actuator/health
curl -v http://localhost:8080/orders
```

If this works but the Service URL does not — the issue is in Service/Ingress routing, not the application.
If this also fails — the issue is inside the pod.

---

**Step 4 — Check database and downstream service connectivity**

```bash
kubectl exec -it <pod-name> -n production -- /bin/sh

# Test database connection from inside the pod
nc -zv <rds-endpoint> 5432
# Connection succeeded? → DB is reachable
# Connection refused / timeout? → Security group or NetworkPolicy blocking

# Test DNS resolution of other services
nslookup payment-service.production.svc.cluster.local
# If this fails — CoreDNS problem

# Test HTTP to downstream service
curl -v http://payment-service.production.svc.cluster.local:8080/health
```

---

**Step 5 — Check environment variables are correct at runtime**

```bash
kubectl exec <pod-name> -n production -- env | grep -E "DB_|SPRING_|APP_"

# Common issue: Secret value is correct but env var name is wrong
# Application expects DB_PASSWORD but Kubernetes Secret mounts as DATABASE_PASSWORD
```

---

**Step 6 — Check resource pressure**

```bash
kubectl top pod <pod-name> -n production
# CPU near limit → throttling → slow responses
# Memory near limit → GC pressure → intermittent timeouts before OOMKill
```

---

---

# 🔧 CI/CD Failures

## Q4. Jenkins pipeline suddenly fails at the build stage — how do you debug?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

**Step 1 — Read the full console output, not just the failure summary**

```
Jenkins UI → Job → Build #N → Console Output
Scroll to the first ERROR or BUILD FAILURE line
Everything before it is context — everything at that line is the cause
```

---

**Step 2 — Categorise the failure by error type**

```bash
# Maven compilation failure
[ERROR] COMPILATION ERROR
[ERROR] OrderService.java:[42,8] cannot find symbol

# Fix: missing dependency in pom.xml, wrong Java version, syntax error
```

```bash
# Dependency download failure
[ERROR] Failed to execute goal on project order-service:
Could not resolve dependencies: Could not find artifact

# Fix: Nexus/Artifactory unreachable, wrong repository URL, network issue
```

```bash
# Out of disk space on Jenkins agent
[ERROR] No space left on device

# Fix:
df -h    # Check disk usage on agent
docker system prune -f    # Remove dangling images and containers
```

```bash
# Docker daemon not running on agent
Cannot connect to the Docker daemon at unix:///var/run/docker.sock

# Fix:
sudo systemctl start docker
sudo usermod -aG docker jenkins    # Jenkins user needs docker group membership
sudo systemctl restart jenkins
```

---

**Step 3 — Check if it's a flaky failure (infrastructure) vs consistent failure (code)**

```bash
# Check build history — did this build pass yesterday?
# Jenkins UI → Job → Build History → Compare recent builds

# If: passed 10 times, failed once → infrastructure flakiness
# If: failing consistently since a specific commit → code change caused it

# Identify the commit that broke it
git log --oneline -10
git bisect start
git bisect bad HEAD
git bisect good <last-known-good-commit>
```

---

**Step 4 — Reproduce locally**

```bash
# Run the same Maven command Jenkins runs
mvn clean package -DskipTests

# If local passes but Jenkins fails — environment difference
# Check: Java version, Maven version, environment variables, network access
java -version
mvn -version
```

---

**Step 5 — Check Jenkins agent resources**

```bash
# On Jenkins agent node
free -h          # Memory available?
df -h            # Disk space?
top              # CPU usage?

# Clean workspace if disk is issue
# Jenkins UI → Job → Workspace → Wipe Out Current Workspace
```

---

## Q5. GitHub Actions workflow is not triggering — what could be the issue?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

**Step 1 — Check the workflow trigger configuration**

```yaml
# .github/workflows/deploy.yml

# ❌ Common mistake 1 — wrong branch name
on:
  push:
    branches: [master]    # Repository uses 'main' — will never trigger

# ❌ Common mistake 2 — path filter too restrictive
on:
  push:
    paths:
      - 'src/**'          # Pushing only to README.md won't trigger this

# ✅ Correct
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
```

---

**Step 2 — Check if the workflow file is on the correct branch**

```bash
# Workflow changes only take effect when the file exists on the target branch
# A workflow added only to a feature branch does NOT run on main

git log --oneline --all -- .github/workflows/deploy.yml
# Confirm the file exists on main/develop
```

---

**Step 3 — Check Actions permissions and status**

```
GitHub UI → Repository → Settings → Actions → General
→ Actions permissions: must be "Allow all actions" or explicitly allowed
→ Workflow permissions: needs read/write if it pushes or creates releases
```

---

**Step 4 — Check for syntax errors in the YAML**

```bash
# Validate workflow syntax locally
npm install -g @github/actionlint
actionlint .github/workflows/deploy.yml

# Common syntax issues:
# - Incorrect indentation (YAML is whitespace-sensitive)
# - Missing 'run' keyword
# - Invalid expression syntax in ${{ }}
```

---

**Step 5 — Check if Actions are disabled at org level or repo level**

```
GitHub UI → Repository → Actions tab
If Actions are disabled: "This repository's Actions workflows are disabled"
→ Settings → Actions → Enable
```

---

**Step 6 — Check for a required approval gate blocking the run**

```
GitHub UI → Actions → Workflow run (if it shows as "Waiting")
→ Environment protection rules may require manual approval before running
→ Settings → Environments → production → Required reviewers
```

---

---

# ☁️ AWS Issues

## Q6. EC2 instance is unreachable — what steps will you take?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

**Step 1 — Check instance state in AWS console**

```bash
aws ec2 describe-instances \
  --instance-ids i-0abc1234def56789 \
  --query 'Reservations[0].Instances[0].[State.Name,PublicIpAddress,PrivateIpAddress]'

# State: running = instance is on
# State: stopped = start it
# State: terminated = gone — cannot recover
```

---

**Step 2 — Check System Status and Instance Status checks**

```bash
aws ec2 describe-instance-status \
  --instance-ids i-0abc1234def56789 \
  --query 'InstanceStatuses[0].[SystemStatus.Status,InstanceStatus.Status]'

# Both should be "ok"
# "impaired" on System Status = underlying AWS host hardware issue → stop/start (moves to new host)
# "impaired" on Instance Status = OS-level issue → check system logs
```

---

**Step 3 — Check Security Group inbound rules**

```bash
aws ec2 describe-security-groups \
  --group-ids sg-0abc1234 \
  --query 'SecurityGroups[0].IpPermissions'

# For SSH: port 22 must be open from your IP or bastion CIDR
# For app traffic: port 8080 / 443 must be open from ALB security group
```

---

**Step 4 — Check instance system log for boot errors**

```bash
aws ec2 get-console-output \
  --instance-id i-0abc1234def56789 \
  --output text

# Look for: kernel panic, disk full on boot, OOM killer, systemd failures
```

---

**Step 5 — Use SSM Session Manager (no SSH or open port 22 needed)**

```bash
# If SSM agent is installed and IAM role allows it
aws ssm start-session --target i-0abc1234def56789

# Inside session — diagnose OS state
df -h              # Disk full?
free -h            # Memory?
systemctl status   # Failed services?
journalctl -xe     # System logs
```

---

**Step 6 — If disk full — resize or clean**

```bash
# Check disk usage
df -h
du -sh /* 2>/dev/null | sort -rh | head -20

# Common culprits: Docker images, Jenkins build artifacts, application logs
docker system prune -f
journalctl --vacuum-size=500M
```

---

## Q7. EKS cluster nodes are not joining — how do you troubleshoot?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

**Step 1 — Check node status from the cluster side**

```bash
kubectl get nodes
# Node not in list = it never joined
# Node in list with NotReady = joined but kubelet has issues
```

---

**Step 2 — Check the bootstrap script on the node**

```bash
# SSH or SSM into the new node
aws ssm start-session --target <new-node-instance-id>

# Check bootstrap log
cat /var/log/cloud-init-output.log | tail -50

# Check kubelet status
systemctl status kubelet
journalctl -u kubelet --no-pager | tail -50
```

Common bootstrap failures:

```
# Wrong cluster name in bootstrap script
error: failed to run Kubelet: unable to load bootstrap kubeconfig: no such file

# Fix: verify user_data in launch template has correct --cluster-name

# API server unreachable — security group not allowing node → control plane
error: Post "https://<cluster-endpoint>/api/v1/nodes": dial tcp: connection refused

# Fix: check security group allows port 443 from node subnet to control plane
```

---

**Step 3 — Check IAM role attached to node**

```bash
# Node must have the AmazonEKSWorkerNodePolicy, AmazonEKS_CNI_Policy,
# and AmazonEC2ContainerRegistryReadOnly policies

aws iam list-attached-role-policies \
  --role-name <node-instance-role-name>
```

Without `AmazonEKSWorkerNodePolicy`, the node cannot authenticate to the cluster API.

---

**Step 4 — Check aws-auth ConfigMap**

```bash
kubectl describe configmap aws-auth -n kube-system

# Should contain the node IAM role ARN
# mapRoles:
# - rolearn: arn:aws:iam::<account>:role/eks-node-role
#   username: system:node:{{EC2PrivateDNSName}}
#   groups:
#   - system:bootstrappers
#   - system:nodes
```

If the node role is missing from `aws-auth`, nodes cannot join regardless of IAM policies.

---

**Step 5 — Check VPC CNI plugin**

```bash
kubectl get pods -n kube-system -l k8s-app=aws-node
# Should be Running on every node
# If Pending — CNI plugin itself cannot start — check IRSA for VPC CNI role
```

---

## Q8. RDS database latency spikes — what do you check?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

**Step 1 — Check CloudWatch metrics for the RDS instance**

```bash
# Check key metrics in the last 1 hour
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name CPUUtilization \
  --dimensions Name=DBInstanceIdentifier,Value=apnakart-order-prod \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 60 \
  --statistics Average
```

| Metric | High Value Means |
|---|---|
| `CPUUtilization` > 80% | Query-heavy load or missing index |
| `DatabaseConnections` near max | Connection pool exhaustion |
| `ReadLatency` / `WriteLatency` high | I/O bottleneck — check IOPS |
| `FreeStorageSpace` near 0 | Disk full — RDS will stop accepting writes |
| `ReadIOPS` / `WriteIOPS` > provisioned | IOPS limit hit — upgrade storage type |

---

**Step 2 — Enable and check slow query log**

```bash
# Check Performance Insights for top SQL queries by load
# AWS Console → RDS → Performance Insights → Top SQL

# OR query slow log via CloudWatch
aws logs filter-log-events \
  --log-group-name /aws/rds/instance/apnakart-order-prod/slowquery \
  --filter-pattern "Query_time: [time>=1]" \
  --start-time $(date -d '30 minutes ago' +%s000)
```

---

**Step 3 — Check active connections and locks**

```sql
-- Connect to RDS and run
SELECT count(*) FROM pg_stat_activity;           -- Total connections
SELECT * FROM pg_stat_activity WHERE state = 'active';   -- Active queries

-- Check for lock waits
SELECT * FROM pg_locks l
JOIN pg_stat_activity a ON l.pid = a.pid
WHERE NOT l.granted;
```

---

**Step 4 — Check for missing indexes on new queries**

```sql
-- Find sequential scans (usually indicates missing index)
SELECT relname, seq_scan, idx_scan
FROM pg_stat_user_tables
ORDER BY seq_scan DESC
LIMIT 10;
```

---

**Step 5 — Check connection pool configuration**

```yaml
# Spring Boot application.properties
spring.datasource.hikari.maximum-pool-size=10       # Per pod
spring.datasource.hikari.minimum-idle=2
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.idle-timeout=600000
```

With 10 pods and pool size 10 = 100 connections to one RDS instance. For `db.t3.medium`, max connections is ~170. At 100+ active connections, latency increases.

---

---

# 🌐 Networking Issues

## Q9. Microservices cannot communicate — how do you debug?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### Kubernetes Service Discovery Path

```
order-service pod
  → DNS query: payment-service.production.svc.cluster.local
  → CoreDNS resolves to ClusterIP
  → kube-proxy routes to pod IP
  → payment-service pod
```

Break at any step = services cannot communicate.

---

**Step 1 — Test DNS resolution from inside the calling pod**

```bash
kubectl exec -it <order-service-pod> -n production -- /bin/sh

# Test DNS resolution
nslookup payment-service.production.svc.cluster.local
# Should return a ClusterIP (e.g., 10.100.x.x)

# If nslookup fails — CoreDNS problem
# If nslookup returns IP — problem is at network layer, not DNS
```

---

**Step 2 — Test TCP connectivity to the target service**

```bash
# From inside order-service pod
nc -zv payment-service.production.svc.cluster.local 8080
# Connection succeeded? → Service is reachable
# Connection refused? → Service port mismatch or pod not ready
# Connection timeout? → NetworkPolicy blocking or kube-proxy issue
```

---

**Step 3 — Verify Service selector matches pod labels**

```bash
kubectl describe service payment-service -n production
# Selector: app=payment-service

kubectl get pods -n production -l app=payment-service
# If empty → no pods match → Service has no endpoints

kubectl get endpoints payment-service -n production
# Addresses: <none> → confirmed — selector mismatch
```

---

**Step 4 — Check port configuration consistency**

```bash
# Service port must match containerPort in pod spec
kubectl get service payment-service -n production -o yaml | grep -A5 "ports:"
# port: 8080, targetPort: 8080

kubectl describe pod <payment-pod> -n production | grep -A5 "Ports:"
# 8080/TCP

# Mismatch between targetPort and containerPort = connection refused
```

---

**Step 5 — Check CoreDNS is healthy**

```bash
kubectl get pods -n kube-system -l k8s-app=kube-dns
# Should show 2 running CoreDNS pods

kubectl logs -n kube-system -l k8s-app=kube-dns | grep -i error
```

---

**Step 6 — Check NetworkPolicy is not blocking**

```bash
kubectl get networkpolicy -n production
# If any NetworkPolicy exists — it may be blocking this communication

# See Q10 for detailed NetworkPolicy debugging
```

---

## Q10. Kubernetes NetworkPolicy is blocking traffic — how do you identify it?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### How NetworkPolicies Block Traffic
By default, all pod-to-pod traffic in Kubernetes is allowed. The moment **any** NetworkPolicy selects a pod, that pod's traffic is restricted to what the policy explicitly allows. Everything else is blocked.

---

**Step 1 — Check if NetworkPolicies exist in the namespace**

```bash
kubectl get networkpolicy -n production

# If output is empty → NetworkPolicy is not the problem
# If policies exist → one of them may be blocking traffic
```

---

**Step 2 — Identify which pods each policy selects**

```bash
kubectl describe networkpolicy -n production

# Example output:
# Name: allow-order-to-payment
# PodSelector: app=payment-service     ← This policy governs INBOUND to payment-service
# PolicyTypes: Ingress
# Ingress:
#   From:
#     PodSelector: app=order-service   ← Only order-service can reach payment-service
#
# This means: ALL other services are blocked from reaching payment-service
```

---

**Step 3 — Test with a temporary debug pod**

```bash
# Create a pod that should be allowed
kubectl run test-allowed -n production \
  --image=busybox \
  --labels="app=order-service" \
  --rm -it \
  -- wget -qO- http://payment-service:8080/health
# Should succeed

# Create a pod that should be blocked
kubectl run test-blocked -n production \
  --image=busybox \
  --labels="app=random-service" \
  --rm -it \
  -- wget -qO- http://payment-service:8080/health
# Should fail — confirms NetworkPolicy is enforcing correctly
```

---

**Step 4 — Fix a misconfigured NetworkPolicy**

```yaml
# ❌ Policy accidentally blocks inter-namespace traffic
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-internal
  namespace: production
spec:
  podSelector: {}          # Applies to ALL pods in namespace
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector: {}      # Only allows traffic FROM pods in same namespace
                           # Blocks traffic from monitoring namespace (Prometheus scraping)

# ✅ Fix — also allow traffic from monitoring namespace
  ingress:
  - from:
    - podSelector: {}
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: monitoring    # Allow Prometheus scraping
```

---

**Step 5 — Temporarily disable a policy to confirm it's the cause**

```bash
# Save policy first
kubectl get networkpolicy allow-order-to-payment -n production -o yaml > /tmp/policy-backup.yaml

# Delete policy temporarily to test
kubectl delete networkpolicy allow-order-to-payment -n production

# Test connectivity — if it now works, the policy was the blocker
nc -zv payment-service.production.svc.cluster.local 8080

# Restore policy after confirmation
kubectl apply -f /tmp/policy-backup.yaml
```

---

---

# 📊 Observability Issues

## Q11. Alerts are not firing — what could be wrong?

> **Method: 🔍 Root Cause Method** → Identify → Diagnose → Fix → Prevent

### Alert Pipeline

```
Prometheus scrapes metrics
    ↓
Alerting rules evaluate (every 15s by default)
    ↓
Rule threshold breached for `for` duration
    ↓
Alert enters PENDING state
    ↓
Alert enters FIRING state
    ↓
Alertmanager receives alert
    ↓
Alertmanager routes to receiver (Slack/PagerDuty)
    ↓
Notification sent
```

Silence anywhere in this pipeline = alerts don't fire.

---

**Step 1 — Is Prometheus actually scraping the target?**

```bash
# Open Prometheus UI
kubectl port-forward svc/prometheus -n monitoring 9090:9090

# Browser → http://localhost:9090/targets
# Status should be UP for all targets
# DOWN = scrape failing — check network, pod selector, annotations
```

```yaml
# Pod must have these annotations for Prometheus to scrape it
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"
  prometheus.io/path: "/actuator/prometheus"
```

---

**Step 2 — Is the alerting rule evaluating correctly?**

```bash
# Prometheus UI → Alerts tab
# Check if the alert shows as Inactive / Pending / Firing

# Test the rule expression directly
# Prometheus UI → Graph → paste the expr from your alert rule
# If it returns no data → metric name wrong or labels wrong
```

```bash
# Check rule file is loaded
kubectl exec -it prometheus-pod -n monitoring -- \
  promtool check rules /etc/prometheus/rules/*.yaml
```

---

**Step 3 — Is Alertmanager receiving alerts?**

```bash
kubectl port-forward svc/alertmanager -n monitoring 9093:9093
# Browser → http://localhost:9093
# Alerts tab → should show firing alerts
# If Prometheus shows FIRING but Alertmanager shows nothing → check Alertmanager URL in prometheus.yml
```

```yaml
# prometheus.yml
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - alertmanager:9093    # Must be reachable from Prometheus pod
```

---

**Step 4 — Is the alert silenced?**

```bash
# Alertmanager UI → Silences tab
# Check for active silences that match the alert labels
# A maintenance window silence may have been created and forgotten

# List silences via API
curl http://alertmanager:9093/api/v2/silences | jq '.[] | select(.status.state=="active")'
```

---

**Step 5 — Is the Slack webhook still valid?**

```bash
# Test the webhook directly
curl -X POST <slack_webhook_url> \
  -H "Content-Type: application/json" \
  -d '{"text": "Alertmanager webhook test"}'

# If this fails → webhook URL expired or Slack app removed
# Regenerate webhook in Slack API console
```

---

**Step 6 — Check Alertmanager routing rules**

```yaml
# alertmanager.yml
route:
  receiver: slack-critical        # Default receiver
  routes:
  - match:
      severity: warning
    receiver: slack-warning
# If a new alert label doesn't match any route → falls through to default
# Verify the alert's labels match at least one route
```

---

## Q12. Metrics show normal but users report downtime — what will you do?

> **Method: 🚨 Action Plan Method** → Clarify → Immediate Action → Investigation → Communication → Prevention

### 🔍 Clarify
This is the most dangerous scenario: your monitoring is lying or incomplete. **Believe the users first, not the dashboards.**

---

### ⚡ Immediate Action

```bash
# Step 1 — Manually verify from outside the system (user perspective)
curl -v https://api.apnakart.com/health
curl -v https://api.apnakart.com/orders

# If curl fails from your machine → there is definitely a problem
# If curl succeeds → issue may be geographic, CDN-related, or specific user segment
```

```bash
# Step 2 — Check from multiple vantage points
# Use an external uptime checker (UptimeRobot, Pingdom) result
# Check CloudFront error rates — users may be hitting CDN, not origin

aws cloudwatch get-metric-statistics \
  --namespace AWS/CloudFront \
  --metric-name 5xxErrorRate \
  --dimensions Name=DistributionId,Value=<distribution-id> \
  --start-time $(date -u -d '30 minutes ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 60 \
  --statistics Average
```

---

### 🩺 Investigation — Why Metrics Showed Normal

**Reason 1 — Wrong metrics being monitored**

The system might be monitoring pod CPU and memory (infrastructure layer) but not error rate at the application layer (user experience layer).

```promql
# Check if this metric was actually being collected
rate(http_requests_total{status=~"5.."}[5m])

# If this returns no data → application is not exposing HTTP metrics
# Pods look healthy but no application-level errors are visible
```

**Reason 2 — Metrics aggregation masking errors**

```promql
# If monitoring avg error rate across all services:
avg(rate(http_requests_total{status=~"5.."}[5m]))

# One service at 0% and one at 100% averages to 50%
# The 50% threshold alert doesn't fire
# Users on that one service experience 100% failure

# Fix: alert per-service, not cluster-wide average
rate(http_requests_total{status=~"5.."}[5m]) by (service)
```

**Reason 3 — CDN caching the error response**

CloudFront may be caching a 500 response and serving it to users. Prometheus scrapes the origin — which may have recovered. But users still see the cached error.

```bash
# Invalidate CloudFront cache for affected paths
aws cloudfront create-invalidation \
  --distribution-id <distribution-id> \
  --paths "/api/orders/*"
```

**Reason 4 — External dependency down, health endpoint returning 200**

The `/health` endpoint returns 200 but the payment service it depends on is down. Every order attempt fails silently.

```yaml
# Fix: use Spring Boot health groups that include downstream dependencies
management:
  health:
    defaults:
      enabled: true
  endpoint:
    health:
      show-details: always
      group:
        readiness:
          include: db, paymentService, diskSpace
```

Now the readiness probe fails when payment service is down — triggering the alert correctly.

---

### 📢 Communication
While investigating:

```
Status page / Slack:
"We are investigating reports of service disruption.
Metrics are being reviewed. Update in 10 minutes."
```

Do not say "metrics show everything is normal" to users experiencing downtime.
It destroys trust and is likely wrong.

---

### 🛡️ Prevention — Synthetic Monitoring

The root fix for "metrics normal but users affected" is adding **synthetic monitoring** — probes that simulate real user journeys from outside the cluster:

```yaml
# Prometheus Blackbox Exporter — probe real endpoints on a schedule
modules:
  http_2xx:
    prober: http
    timeout: 5s
    http:
      valid_http_versions: ["HTTP/1.1", "HTTP/2.0"]
      valid_status_codes: [200]
      fail_if_body_matches_regexp:
        - "error"
        - "unavailable"
```

```yaml
# Scrape config — probe the actual user-facing endpoint every 30s
- job_name: 'blackbox'
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
  - targets:
    - https://api.apnakart.com/health
    - https://api.apnakart.com/orders
  relabel_configs:
  - source_labels: [__address__]
    target_label: __param_target
  - target_label: __address__
    replacement: blackbox-exporter:9115
```

```yaml
# Alert on synthetic probe failure
- alert: ExternalEndpointDown
  expr: probe_success{job="blackbox"} == 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "External endpoint {{ $labels.instance }} is unreachable"
```

This alert fires based on what **users actually experience** — not what the internal metrics report.

---

*All commands are ordered from fastest diagnostic to deepest investigation. In a real incident, stop at the first command that reveals the cause — do not run all of them.*

---

# 🧠 Advanced Thinking & Design Questions

> These questions test system thinking, not tool knowledge.
> The expected answer structure: Requirements → Architecture → Components → Scaling → Trade-offs.
> Interviewers are evaluating whether you think in systems, not in commands.

---

## Q1. Design a highly available CI/CD pipeline for a microservices architecture.

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 Requirements
Before designing, clarify what "highly available" means for a pipeline:

| Requirement | Target |
|---|---|
| Pipeline must not be a single point of failure | Multiple agents, no single Jenkins master |
| A failure in one service pipeline must not block others | Independent pipelines per service |
| A failed deployment must auto-rollback | Rollback gate built into pipeline |
| Security gates cannot be bypassed | Hard gates — not advisory |
| Pipeline must support multiple environments | dev → test → uat → prod with promotion gates |

---

### 🏛️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     Source Control (GitHub)                  │
│  feature/* → develop → test → uat → main (prod)             │
└──────────────────────┬──────────────────────────────────────┘
                       │ webhook trigger
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                Jenkins (HA Setup)                            │
│  Active Controller ──── Passive Controller (hot standby)    │
│  Shared NFS / EFS for JENKINS_HOME                          │
│                                                             │
│  Agent Pool:                                                │
│  ├── Agent Group A: Build agents (Maven, Node)              │
│  ├── Agent Group B: Docker/Trivy agents                     │
│  └── Agent Group C: Deploy agents (kubectl, helm, aws cli)  │
└──────────────────────┬──────────────────────────────────────┘
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
   [Compile]      [Test +        [Trivy +
   [Lint]          SonarQube]    Security]
         └─────────────┼─────────────┘
                       │ all pass
                       ▼
               [Docker Build]
                       │
               [Push to ECR]
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
      [dev]         [test]         [uat]    ← Parallel deploys to lower envs
         └─────────────┼─────────────┘
                       │ manual approval gate
                       ▼
                  [production]
                       │
               [Post-Deploy Validation]
                       │
              pass ────┴──── fail
               │                │
          [Complete]        [Auto-Rollback]
```

---

### ⚙️ Jenkins HA Configuration

**Problem with single Jenkins controller:** if it goes down, all pipelines stop. No deployments, no builds.

```groovy
// Solution: Jenkins controller on EKS with persistent storage
// JENKINS_HOME stored on EFS — survives pod restarts

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: jenkins
  namespace: cicd
spec:
  replicas: 1           // Active controller
  volumeClaimTemplates:
  - metadata:
      name: jenkins-home
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: efs-sc     // EFS — multi-AZ durable storage
      resources:
        requests:
          storage: 50Gi
```

For true HA, use **Jenkins Operations Center** (CloudBees) or migrate to **GitLab CI / GitHub Actions** where the control plane is managed — not self-hosted.

---

### ⚙️ Independent Pipelines Per Microservice

A monorepo pipeline that builds all 10 services on every commit fails 10 pipelines when 1 service has a flaky test. Use path-based triggers:

```yaml
# GitHub Actions — triggers only when relevant service changes
# .github/workflows/order-service.yml
on:
  push:
    paths:
      - 'services/order-service/**'
      - 'shared-libs/**'         # Also trigger if shared library changes
    branches: [develop, main]
```

Each service has its own pipeline. A failing order-service build does not affect payment-service deployment.

---

### ⚙️ Promotion Gate Between Environments

```groovy
// Jenkinsfile — environment promotion with manual approval for production
stage('Deploy to UAT') {
  steps {
    sh "helm upgrade order-service ./helm --namespace uat --set image.tag=${IMAGE_TAG}"
    sh "kubectl rollout status deployment/order-service -n uat --timeout=120s"
  }
}

stage('Approval Gate: Promote to Production') {
  steps {
    timeout(time: 24, unit: 'HOURS') {           // Auto-reject after 24h
      input message: 'Deploy to production?',
            submitter: 'tech-lead,release-manager'  // Only these people can approve
    }
  }
}

stage('Deploy to Production') {
  steps {
    sh "helm upgrade order-service ./helm --namespace production --set image.tag=${IMAGE_TAG}"
  }
}
```

---

### ⚙️ Automatic Rollback as a Pipeline Stage

```groovy
stage('Post-Deploy Validation') {
  steps {
    script {
      sleep(60)    // Wait 60s for metrics to stabilise
      def errorRate = sh(
        script: """
          curl -s 'http://prometheus:9090/api/v1/query' \
            --data-urlencode 'query=rate(http_requests_total{status=~"5..",
              service="order-service",namespace="production"}[2m])
              / rate(http_requests_total{service="order-service",
              namespace="production"}[2m])' \
            | jq -r '.data.result[0].value[1] // "0"'
        """,
        returnStdout: true
      ).trim().toFloat()

      if (errorRate > 0.05) {
        sh "kubectl rollout undo deployment/order-service -n production"
        error("Error rate ${errorRate} exceeded 5% — deployment rolled back")
      }
    }
  }
}
```

---

### ⚖️ Trade-offs

| Decision | Chosen | Alternative | Reason |
|---|---|---|---|
| Jenkins vs GitHub Actions | Jenkins for complex pipelines | GitHub Actions for simplicity | Jenkins gives more control over agents and secret management |
| Per-service pipelines vs mono-pipeline | Per-service | Mono-pipeline | Failure isolation — one flaky test does not block 9 other services |
| Manual approval for prod | Yes | Fully automated | High-risk environment needs a human checkpoint |
| Rollback strategy | Automatic on error rate | Manual only | Speed — MTTR matters more than avoiding false rollbacks |

---

## Q2. How would you design a multi-region deployment in AWS?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 Requirements

| Requirement | Design Implication |
|---|---|
| Survive full AWS region failure | Active-active or active-passive across 2+ regions |
| Users get low latency globally | Route traffic to nearest region |
| Data must be consistent | Database replication strategy required |
| RTO (Recovery Time Objective) | How fast must recovery be? |
| RPO (Recovery Point Objective) | How much data loss is acceptable? |

---

### 🏛️ Architecture — Active-Passive (Primary + Standby)

For most production systems, active-passive is the pragmatic starting point. Active-active adds significant complexity and cost.

```
                    ┌─────────────────────┐
                    │   Route53 (DNS)     │
                    │  Health-check based │
                    │  failover routing   │
                    └──────┬──────┬───────┘
                           │      │
              Primary       │      │    Secondary
              ap-south-1    │      │    ap-southeast-1
                           ▼      ▼
              ┌──────────────┐  ┌──────────────┐
              │  CloudFront  │  │  CloudFront  │
              └──────┬───────┘  └──────┬───────┘
                     │                 │
              ┌──────▼───────┐  ┌──────▼───────┐
              │  ALB         │  │  ALB         │
              └──────┬───────┘  └──────┬───────┘
                     │                 │
              ┌──────▼───────┐  ┌──────▼───────┐
              │  EKS Cluster │  │  EKS Cluster │
              │  (ACTIVE)    │  │  (STANDBY)   │
              └──────┬───────┘  └──────┬───────┘
                     │                 │
              ┌──────▼───────┐         │
              │  RDS Primary │────────►│ RDS Read Replica
              │  (ap-south-1)│ async   │ (ap-southeast-1)
              └──────────────┘ repl.   └──────────────────┘
```

---

### ⚙️ Route53 Health-Check Based Failover

```hcl
# Primary record — receives traffic when healthy
resource "aws_route53_record" "primary" {
  zone_id = var.hosted_zone_id
  name    = "api.apnakart.com"
  type    = "A"

  failover_routing_policy {
    type = "PRIMARY"
  }

  set_identifier = "primary-ap-south-1"

  health_check_id = aws_route53_health_check.primary.id

  alias {
    name                   = aws_lb.primary.dns_name
    zone_id                = aws_lb.primary.zone_id
    evaluate_target_health = true
  }
}

# Secondary record — receives traffic when primary is unhealthy
resource "aws_route53_record" "secondary" {
  zone_id = var.hosted_zone_id
  name    = "api.apnakart.com"
  type    = "A"

  failover_routing_policy {
    type = "SECONDARY"
  }

  set_identifier = "secondary-ap-southeast-1"
  # No health check on secondary — it always accepts traffic when primary fails

  alias {
    name                   = aws_lb.secondary.dns_name
    zone_id                = aws_lb.secondary.zone_id
    evaluate_target_health = true
  }
}

# Health check — checks the primary ALB every 10 seconds
resource "aws_route53_health_check" "primary" {
  fqdn              = aws_lb.primary.dns_name
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3           // 3 consecutive failures → failover
  request_interval  = 10
}
```

When the primary region fails, Route53 detects 3 consecutive health check failures (~30 seconds) and switches all DNS to the secondary region.

---

### ⚙️ Database Replication Strategy

**RDS Cross-Region Read Replica — for read-heavy workloads:**

```hcl
resource "aws_db_instance" "secondary_replica" {
  provider = aws.ap_southeast_1

  identifier          = "apnakart-order-replica"
  replicate_source_db = aws_db_instance.primary.arn   // Cross-region replication

  instance_class      = "db.r6g.large"
  publicly_accessible = false

  # On failover — promote replica to standalone primary
  # aws rds promote-read-replica --db-instance-identifier apnakart-order-replica
}
```

**RPO consideration:** RDS cross-region replication is asynchronous. Typical replication lag is 1–5 seconds. On failover, the last 1–5 seconds of writes may be lost.

---

### ⚙️ Terraform Workspace Per Region

```hcl
# Use Terraform workspaces to manage per-region infrastructure
terraform workspace new ap-south-1
terraform workspace new ap-southeast-1

# In main.tf — region-specific configuration
provider "aws" {
  region = var.region[terraform.workspace]
}

variable "region" {
  default = {
    ap-south-1    = "ap-south-1"
    ap-southeast-1 = "ap-southeast-1"
  }
}
```

---

### ⚖️ Trade-offs

| Aspect | Active-Passive | Active-Active |
|---|---|---|
| Cost | 1.3–1.5x (standby uses fewer resources) | 2x (full capacity in both regions) |
| Complexity | Medium | High — data consistency across regions is hard |
| RTO | 30–60 seconds (DNS failover) | Near-zero (traffic always in both) |
| RPO | 1–5 seconds (async replication lag) | Near-zero (synchronous replication) |
| When to use | Most production systems | Financial systems, global real-time apps |

---

## Q3. How do you ensure disaster recovery for Kubernetes workloads?

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 What Needs to Be Recovered

```
A Kubernetes cluster has four categories of state:

1. Cluster configuration   → EKS control plane config, node group definitions
2. Workload definitions    → Deployments, Services, ConfigMaps, Ingress (YAML/Helm)
3. Application state       → Data in RDS, S3, ElastiCache (not in Kubernetes)
4. Secrets                 → Kubernetes Secrets / AWS Secrets Manager values
```

Items 1, 2, and 4 are recoverable if stored in Git and AWS.
Item 3 (application data) is the real DR problem — handled outside Kubernetes.

---

### ⚙️ Pillar 1 — Everything in Git (GitOps)

All Kubernetes manifests, Helm charts, and Kustomize overlays live in Git. Recreating a cluster is a `helm install` — not a manual process.

```
git-repo/
├── helm/
│   ├── order-service/
│   │   ├── Chart.yaml
│   │   ├── values-dev.yaml
│   │   ├── values-prod.yaml
│   │   └── templates/
│   └── ...
├── kustomize/
│   ├── base/
│   └── overlays/
│       ├── dev/
│       ├── uat/
│       └── prod/
└── terraform/
    └── eks/
```

If the entire cluster is destroyed: `terraform apply` recreates infrastructure, `helm install` restores all workloads. Time to recovery depends on EKS provisioning time (~15 minutes).

---

### ⚙️ Pillar 2 — etcd Backup (Cluster State Backup)

EKS manages the etcd control plane — AWS backs this up automatically. For self-managed clusters:

```bash
# Take an etcd snapshot
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-snapshot-$(date +%Y%m%d).db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

# Upload to S3 immediately
aws s3 cp /backup/etcd-snapshot-$(date +%Y%m%d).db \
  s3://apnakart-backups/etcd/

# Restore from snapshot
ETCDCTL_API=3 etcdctl snapshot restore /backup/etcd-snapshot-20250315.db \
  --data-dir=/var/lib/etcd-restore
```

---

### ⚙️ Pillar 3 — Velero for Workload Backup and Restore

Velero backs up all Kubernetes objects (Deployments, Services, ConfigMaps, PVCs) to S3 and can restore them to a new cluster.

```yaml
# Velero backup schedule — daily at 2am
apiVersion: velero.io/v1
kind: Schedule
metadata:
  name: daily-backup
  namespace: velero
spec:
  schedule: "0 2 * * *"
  template:
    includedNamespaces:
    - production
    - monitoring
    storageLocation: default      # S3 bucket configured in Velero
    ttl: 720h                     # Keep backups for 30 days
    snapshotVolumes: true         # Also snapshot EBS volumes (for PVCs)
```

```bash
# Restore to a new cluster after disaster
velero restore create --from-backup daily-backup-20250315020000

# Monitor restore progress
velero restore describe daily-backup-20250315020000
```

---

### ⚙️ Pillar 4 — RDS Automated Backups + Point-in-Time Recovery

```hcl
resource "aws_db_instance" "production" {
  backup_retention_period   = 7           // Keep 7 days of automated backups
  backup_window             = "02:00-03:00"  // Run during low-traffic window
  deletion_protection       = true         // Cannot be deleted accidentally
  skip_final_snapshot       = false        // Always create a final snapshot on delete
  final_snapshot_identifier = "apnakart-order-final-${timestamp()}"
}
```

```bash
# Restore to any point in the last 7 days (point-in-time recovery)
aws rds restore-db-instance-to-point-in-time \
  --source-db-instance-identifier apnakart-order-prod \
  --target-db-instance-identifier apnakart-order-restored \
  --restore-time 2025-03-15T14:30:00Z
```

---

### 📊 DR Runbook Summary

| Scenario | Recovery Steps | Estimated RTO |
|---|---|---|
| Single pod failure | Kubernetes self-heals automatically | < 1 minute |
| Deployment corrupted | `kubectl rollout undo` | < 3 minutes |
| Namespace destroyed | `velero restore` | 10–20 minutes |
| Full EKS cluster lost | `terraform apply` + `helm install` | 20–40 minutes |
| RDS instance destroyed | Restore from automated backup | 15–30 minutes |
| Full region failure | Route53 failover + RDS replica promote | 5–10 minutes |

---

### ⚖️ Trade-offs
- Velero snapshots of PVCs require EBS snapshots — adds ~$0.05/GB/month storage cost.
- GitOps approach only works if developers never make manual `kubectl edit` changes in production — those changes are not in Git and will be lost on cluster rebuild.
- RTO of 20–40 minutes for full cluster rebuild may be unacceptable for some businesses — active-passive multi-region reduces this to minutes.

---

## Q4. How would you implement blue-green vs canary deployments?

> **Method: ⚖️ Compare & Justify** → Problem → Options → Chosen Solution → Trade-offs

### 📖 Core Distinction

| Strategy | How It Works | Traffic Switch | Rollback |
|---|---|---|---|
| Rolling Update | Replace pods one at a time | Gradual, automatic | `kubectl rollout undo` |
| Blue-Green | Two full environments — switch all traffic at once | Instant (0% → 100%) | Switch back instantly |
| Canary | Route small % of traffic to new version — increase gradually | Controlled (5% → 25% → 100%) | Reduce % to 0% |

---

### ⚙️ Blue-Green Implementation

Blue-green requires running two complete environments simultaneously. Traffic is on Blue. Green gets the new version. Switch is instantaneous via Service selector update.

```yaml
# Blue deployment (currently live)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-blue
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
      slot: blue
  template:
    metadata:
      labels:
        app: order-service
        slot: blue
    spec:
      containers:
      - name: order-service
        image: <ecr>/order-service:v1.0.0

---
# Green deployment (new version — not receiving traffic yet)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-green
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
      slot: green
  template:
    metadata:
      labels:
        app: order-service
        slot: green
    spec:
      containers:
      - name: order-service
        image: <ecr>/order-service:v2.0.0

---
# Service — points to BLUE currently
apiVersion: v1
kind: Service
metadata:
  name: order-service
  namespace: production
spec:
  selector:
    app: order-service
    slot: blue           # ← Change this to 'green' to switch traffic
```

**Traffic switch (takes effect in milliseconds):**

```bash
# Switch all traffic to Green
kubectl patch service order-service -n production \
  -p '{"spec":{"selector":{"app":"order-service","slot":"green"}}}'

# Verify — check which pods are now receiving traffic
kubectl get endpoints order-service -n production

# Rollback — switch back to Blue instantly
kubectl patch service order-service -n production \
  -p '{"spec":{"selector":{"app":"order-service","slot":"blue"}}}'
```

**When to use blue-green:**
- Database schema migrations — you need full control over when the new code hits the database.
- Major version changes where gradual exposure is not acceptable.
- When rollback must be instant and complete — not pod-by-pod.

**Cost:** Blue-green doubles compute cost during deployment (both environments running).

---

### ⚙️ Canary Implementation

Canary routes a controlled percentage of traffic to the new version. Start at 5%, validate metrics, increase to 25%, validate, promote to 100%.

**Method 1 — Replica ratio (simple, no extra tooling):**

```yaml
# Stable deployment — 19 replicas (95% of traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-stable
spec:
  replicas: 19
  template:
    metadata:
      labels:
        app: order-service
        track: stable
    spec:
      containers:
      - image: <ecr>/order-service:v1.0.0

---
# Canary deployment — 1 replica (5% of traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-canary
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: order-service
        track: canary
    spec:
      containers:
      - image: <ecr>/order-service:v2.0.0

---
# Single Service selects BOTH deployments
apiVersion: v1
kind: Service
spec:
  selector:
    app: order-service    # Matches both stable and canary pods
                          # kube-proxy distributes proportionally to replica count
```

Traffic distribution = replica count ratio. 19:1 = ~5% canary.

---

**Method 2 — Argo Rollouts (precise percentage control):**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: order-service
  namespace: production
spec:
  replicas: 5
  strategy:
    canary:
      steps:
      - setWeight: 5         # Step 1: 5% traffic to canary
      - pause: {duration: 5m}
      - setWeight: 25        # Step 2: 25% traffic
      - pause: {duration: 10m}
      - setWeight: 50        # Step 3: 50% traffic
      - pause: {}            # Step 4: Manual approval to proceed
      - setWeight: 100       # Step 5: Full rollout
      canaryMetadata:
        labels:
          track: canary
      stableMetadata:
        labels:
          track: stable
      # Auto-rollback if error rate exceeds threshold
      analysis:
        templates:
        - templateName: error-rate-check
        startingStep: 2
```

```yaml
# Analysis template — auto-promote or auto-rollback based on real metrics
apiVersion: argoproj.io/v1alpha1
kind: AnalysisTemplate
metadata:
  name: error-rate-check
spec:
  metrics:
  - name: error-rate
    interval: 1m
    successCondition: result[0] < 0.05      # Auto-promote if error rate < 5%
    failureCondition: result[0] >= 0.10     # Auto-rollback if error rate >= 10%
    provider:
      prometheus:
        address: http://prometheus:9090
        query: |
          rate(http_requests_total{status=~"5..",
            rollouts_pod_template_hash="{{args.canary-hash}}"}[2m])
          / rate(http_requests_total{
            rollouts_pod_template_hash="{{args.canary-hash}}"}[2m])
```

---

### 📊 Decision Guide

| Situation | Use |
|---|---|
| Low-risk, frequent releases | Rolling Update |
| High-risk release, need instant rollback | Blue-Green |
| New feature — want real user validation | Canary |
| Database migration alongside code change | Blue-Green (controlled cutover) |
| A/B testing between versions | Canary (with header-based routing) |

---

## Q5. Design an observability system for 100+ microservices.

> **Method: 🏗️ Top-Down Design** → Requirements → Architecture → Components → Scaling → Trade-offs

### 📋 Requirements at 100+ Service Scale

| Challenge | Implication |
|---|---|
| 100+ services × 10 replicas = 1000+ pods | Metrics collection must be automated — no manual scrape config per pod |
| 100+ log streams | Centralised aggregation is non-negotiable |
| A single request touches 8–10 services | Distributed tracing required |
| Alert for 100 services individually | Alert design must be templated, not hand-crafted per service |
| On-call engineer cannot know all 100 services | Runbooks and dashboards must be self-service |

---

### 🏛️ Three-Pillar Architecture

```
                        ┌─────────────────────────────────┐
                        │         OBSERVE LAYER            │
                        │                                  │
  ┌──────────┐          │  Metrics: Prometheus + Thanos    │
  │ 100+     │──scrape──►  Logs:    ELK / Loki            │
  │ services │──logs───►  Traces:  Jaeger / Datadog APM   │
  │ (pods)   │──traces──►                                  │
  └──────────┘          └────────────┬────────────────────┘
                                     │
                        ┌────────────▼────────────────────┐
                        │         STORE LAYER              │
                        │                                  │
                        │  Thanos Object Storage (S3)      │
                        │  Elasticsearch Cluster           │
                        │  Jaeger Storage Backend          │
                        └────────────┬────────────────────┘
                                     │
                        ┌────────────▼────────────────────┐
                        │         SURFACE LAYER            │
                        │                                  │
                        │  Grafana (unified dashboards)    │
                        │  Alertmanager (routed alerts)    │
                        │  Kibana (log search)             │
                        └─────────────────────────────────┘
```

---

### ⚙️ Metrics at Scale — Prometheus + Thanos

Single Prometheus cannot handle 100+ services with 1000+ pods long-term — cardinality explodes, memory grows, single point of failure.

**Solution: Prometheus per cluster + Thanos for global querying and long-term storage**

```yaml
# One Prometheus per namespace or per cluster — smaller scrape surface
# Thanos Sidecar runs alongside each Prometheus — ships data to S3

# Thanos architecture:
#
# [Prometheus A] ──Sidecar──► S3 (long-term storage)
# [Prometheus B] ──Sidecar──► S3
#         │                    │
#         └────────────────────┘
#                    │
#           [Thanos Query]      ← Single query endpoint for Grafana
#                    │
#           [Thanos Store]      ← Serves historical data from S3
```

```yaml
# Thanos Sidecar in Prometheus StatefulSet
containers:
- name: thanos-sidecar
  image: quay.io/thanos/thanos:v0.32.0
  args:
  - sidecar
  - --prometheus.url=http://localhost:9090
  - --objstore.config-file=/etc/thanos/objstore.yaml   # S3 config
  - --tsdb.path=/prometheus
```

---

### ⚙️ Automated Service Discovery — No Manual Scrape Config

At 100+ services, maintaining a static scrape list is unmanageable. Use Kubernetes service discovery:

```yaml
# prometheus.yml — automatically discovers all pods with the right annotation
scrape_configs:
- job_name: 'kubernetes-pods'
  kubernetes_sd_configs:
  - role: pod
  relabel_configs:
  # Only scrape pods with annotation prometheus.io/scrape: "true"
  - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
    action: keep
    regex: true
  # Use the port specified in annotation
  - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_port]
    action: replace
    target_label: __address__
    regex: (.+)
    replacement: ${1}
  # Add namespace and pod name as labels automatically
  - source_labels: [__meta_kubernetes_namespace]
    target_label: namespace
  - source_labels: [__meta_kubernetes_pod_name]
    target_label: pod
```

Every new service gets auto-discovered when it adds the annotation. Zero config changes needed in Prometheus.

---

### ⚙️ Logs at Scale — Loki over ELK

For 100+ services, ELK (Elasticsearch) becomes expensive and operationally heavy. **Loki** is a better fit for Kubernetes-native log aggregation at scale.

```
ELK:  Indexes every log field → expensive storage, powerful search
Loki: Indexes only labels (namespace, pod, service) → cheap storage,
      log content searched at query time
```

```yaml
# Promtail DaemonSet — ships logs from every node to Loki
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: promtail
  namespace: monitoring
spec:
  template:
    spec:
      containers:
      - name: promtail
        image: grafana/promtail:2.9.0
        args:
        - -config.file=/etc/promtail/config.yaml
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

```yaml
# Loki scrape config — auto-adds Kubernetes labels to every log line
scrape_configs:
- job_name: kubernetes-pods
  kubernetes_sd_configs:
  - role: pod
  pipeline_stages:
  - docker: {}           # Parse Docker log format
  - labels:
      namespace:
      pod:
      container:
      app:
```

Every log line in Grafana is automatically tagged with `namespace`, `pod`, `container`, and `app` — searchable across all 100+ services from one Grafana datasource.

---

### ⚙️ Alerts at Scale — Templated Rules, Not Per-Service Rules

Writing individual alert rules for 100 services = 100+ rule files to maintain. Use recording rules and label-based templating:

```yaml
# One alert rule covers ALL services via labels
groups:
- name: service-health
  rules:
  - alert: ServiceHighErrorRate
    expr: |
      (
        rate(http_requests_total{status=~"5..",namespace="production"}[5m])
        / rate(http_requests_total{namespace="production"}[5m])
      ) > 0.05
    for: 3m
    labels:
      severity: critical
    annotations:
      summary: "High error rate on {{ $labels.service }}"
      description: "{{ $labels.service }} in {{ $labels.namespace }} has error rate {{ $value | humanizePercentage }}"
      runbook_url: "https://wiki.internal/runbooks/{{ $labels.service }}"

  # Same pattern for latency — one rule, all services
  - alert: ServiceHighLatency
    expr: |
      histogram_quantile(0.99,
        rate(http_request_duration_seconds_bucket{namespace="production"}[5m]))
      > 2
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "P99 latency > 2s on {{ $labels.service }}"
```

One rule file covers all 100+ services. When a new service is deployed, it is automatically covered by these rules.

---

### ⚙️ Dashboards at Scale — Templated Variables

One dashboard template for all services using Grafana variables:

```
Grafana Dashboard → Variables:
  $namespace = production / uat / dev      (dropdown)
  $service   = order-service / payment-service / ...  (populated from Prometheus labels)

Every panel uses $namespace and $service variables:
  rate(http_requests_total{namespace="$namespace", service="$service"}[5m])

Result: One dashboard works for all 100+ services.
        Engineer selects service from dropdown — no new dashboard per service.
```

---

### ⚙️ Distributed Tracing — Sampling Strategy at Scale

100% trace sampling across 100 services at high traffic volume generates enormous data volume.

```yaml
# Head-based sampling — sample 10% of all requests
# Sample 100% of requests with errors (always capture failures)
# Sample 100% of requests with latency > 2s (always capture slow requests)

# Jaeger sampling config
sampling:
  default_strategy:
    type: probabilistic
    param: 0.1              # 10% baseline sampling

  per_service_strategies:
  - service: payment-service
    type: probabilistic
    param: 0.5              # 50% — higher risk service, sample more
  - service: health-check-service
    type: probabilistic
    param: 0.01             # 1% — high volume, low value traces
```

---

### 📊 Tooling Decision by Scale

| Scale | Metrics | Logs | Traces |
|---|---|---|---|
| 1–10 services | Prometheus + Grafana | ELK or CloudWatch | Datadog APM |
| 10–50 services | Prometheus + Grafana | Loki + Grafana | Jaeger or Datadog |
| 50–100+ services | Prometheus + Thanos + Grafana | Loki + S3 storage tier | Jaeger with sampling |
| 500+ services | Managed (Datadog / New Relic) | Managed | Managed |

At 100+ services the operational overhead of self-managed Prometheus, ELK, and Jaeger becomes a team in itself. Evaluate managed observability (Datadog, Grafana Cloud) against the engineering cost of self-hosting.

---

### ⚖️ Key Trade-offs

| Decision | Trade-off |
|---|---|
| Loki vs ELK | Loki is cheaper and simpler. ELK is more powerful for complex log analytics. Choose based on query patterns. |
| Thanos vs VictoriaMetrics | Both solve Prometheus scaling. VictoriaMetrics is simpler to operate. Thanos integrates more naturally with existing Prometheus. |
| Head sampling vs tail sampling | Head sampling is cheap but misses rare errors. Tail sampling captures all errors but requires buffering full traces before making sampling decisions — complex to implement. |
| Self-hosted vs managed | Self-hosted is cheaper at small scale. Managed wins above ~50 services when engineering time to operate the observability stack exceeds the cost difference. |

---

*These design answers are intentionally opinionated — real system design requires making choices and defending them, not listing all options neutrally. Adjust the specific tool choices to match your actual experience and the interviewer's stack.*

---

# 🎯 Behavioral + Ownership Questions

> These questions are not about tools. They are about judgment, ownership, and growth.
> Interviewers use behavioral questions to determine: do you take responsibility, do you learn, and do you make the people around you better?
> Every answer uses the STAR method but the *Result* must include what changed — not just what happened.

---

## Q1. Tell me about a time you handled a production incident.

> **Method: ⭐ STAR Method** → Situation → Task → Action → Result

### 📍 Situation
During my internship at Hisan Labs, we had a production deployment of a new version of the order processing service on the ApnaKart platform. Approximately 12 minutes after the deployment was marked complete, Slack alerts fired showing the P99 latency on the order-service had spiked from ~180ms to over 4 seconds. Simultaneously, the error rate crossed 8% — well above our 5% threshold. This was a Friday evening.

### 🎯 Task
I was the engineer who had triggered the deployment. That meant I owned the incident — not just the fix, but the communication, the investigation, and making sure we understood *why* so it could not happen again.

### ⚙️ Action

**Minute 0–3 — Stabilise first, investigate second**

```bash
# Confirmed the rollout was the cause by correlating deployment timestamp with spike
kubectl rollout history deployment/order-service -n production
# Revision 7 deployed at 18:42 — latency spike started at 18:54

# Immediate rollback — did not wait to find root cause
kubectl rollout undo deployment/order-service -n production

# Confirmed rollback was progressing
kubectl rollout status deployment/order-service -n production --timeout=120s
```

**Minute 3 — Communicated to the team**

Posted in Slack #prod-alerts:

> *"Production incident active. Order-service latency at P99 4s, error rate 8%. Root cause under investigation. Rollback to revision 6 in progress. ETA to stable: 2 minutes. I own this."*

"I own this" is not a phrase I use lightly. It tells the team that one person is driving and they should support, not duplicate effort.

**Minute 5 — Service restored, investigation begins**

```bash
# Logs from the failed pods (revision 7)
kubectl logs -l app=order-service -n production --previous | grep ERROR

# Finding:
# ERROR - HikariPool-1 - Connection is not available, request timed out after 30000ms
# ERROR - Unable to acquire JDBC Connection
```

The new version introduced a new database query that was not using the connection pool correctly — it was opening connections without releasing them. Within 12 minutes the pool was exhausted. Every new request waited 30 seconds for a connection that never came.

```sql
-- Confirmed on RDS: connections at maximum
SELECT count(*) FROM pg_stat_activity;
-- 170 (max_connections for db.t3.medium)

-- All connections in 'idle in transaction' state — never closed by application
SELECT state, count(*) FROM pg_stat_activity GROUP BY state;
-- idle in transaction: 168
```

**Root cause:** A new service method introduced in revision 7 opened a JPA transaction but did not annotate the method with `@Transactional`. The connection was opened at the start of the request and held open until the request thread ended — which in some cases was never due to a downstream timeout. Classic connection leak.

```java
// ❌ Revision 7 — missing @Transactional, connection never released cleanly
public OrderSummary getOrderHistory(String userId) {
  List<Order> orders = orderRepository.findByUserId(userId);
  // ... processing
  return summary;
}

// ✅ Fix — explicit transaction boundary ensures connection returned to pool
@Transactional(readOnly = true)
public OrderSummary getOrderHistory(String userId) {
  List<Order> orders = orderRepository.findByUserId(userId);
  // ... processing
  return summary;
}
```

**Minute 45 — Post-incident write-up published**

Shared with the team before end of day — not the next week.

### 📊 Result
- Service restored in **under 5 minutes** from alert to stable.
- Root cause identified within **40 minutes**.
- Fix deployed and validated the following morning.
- **Process change introduced:** added a post-deploy validation gate that queries active RDS connection count — if connections climb above 80% of max within 5 minutes of deployment, the pipeline triggers an automatic rollback. This class of failure cannot reach production silently again.
- **Personal learning:** I had reviewed the PR for revision 7 and missed the missing `@Transactional`. I added JPA transaction management to my personal code review checklist.

---

## Q2. Describe a situation where automation saved significant effort.

> **Method: ⭐ STAR Method** → Situation → Task → Action → Result

### 📍 Situation
At Hisan Labs, every new feature deployment required a series of manual steps that every engineer had to perform in sequence:

1. SSH into the Jenkins server to check the current pipeline status
2. Manually trigger the build with the correct parameters
3. Watch the console output and wait
4. If it passed — manually run `kubectl set image` with the correct image tag
5. Watch pod status in a separate terminal
6. Post a Slack message with the deployment details
7. Update a shared spreadsheet tracking which version was deployed to which environment

This process took **35–45 minutes of active engineer time per deployment** and was error-prone — wrong image tags, forgotten Slack posts, stale spreadsheet entries.

### 🎯 Task
Automate the entire deployment workflow so a deployment required a single trigger — a Git push or a button click — and produced a complete audit trail automatically.

### ⚙️ Action

**Step 1 — Replaced manual `kubectl` commands with a Jenkins pipeline stage**

```groovy
stage('Deploy to Production') {
  steps {
    withAWS(credentials: 'aws-deploy-role', region: 'ap-south-1') {
      sh '''
        aws eks update-kubeconfig --name apnakart-prod --region ap-south-1
        kubectl set image deployment/order-service \
          order-service=${ECR_URL}/order-service:${GIT_COMMIT:0:8} \
          --namespace=production
        kubectl rollout status deployment/order-service \
          --namespace=production --timeout=120s
      '''
    }
  }
}
```

No SSH. No manual image tag lookup. No waiting at a terminal.

**Step 2 — Automated Slack notification with deployment context**

```groovy
post {
  success {
    slackSend(
      channel: '#deployments',
      color: 'good',
      message: """
✅ *Deployment Successful*
*Service:* order-service
*Environment:* production
*Image Tag:* ${GIT_COMMIT.take(8)}
*Deployed by:* ${currentBuild.rawBuild.getCause(hudson.model.Cause.UserIdCause)?.userId ?: 'automated'}
*Build URL:* ${env.BUILD_URL}
*Duration:* ${currentBuild.durationString}
      """
    )
  }
  failure {
    slackSend(
      channel: '#prod-alerts',
      color: 'danger',
      message: "❌ Deployment FAILED — order-service — ${env.BUILD_URL}"
    )
  }
}
```

**Step 3 — Replaced the spreadsheet with Jenkins build history + ECR image tags**

Every deployment is now permanently recorded in:
- Jenkins build history (who triggered, when, which branch)
- ECR image tags (Git SHA — traceable to exact commit)
- Kubernetes rollout history (`kubectl rollout history`)

The spreadsheet was retired.

**Step 4 — Extended to all 10 microservices using a shared pipeline library**

```groovy
// vars/deployService.groovy — shared library function
def call(String serviceName, String namespace) {
  sh """
    kubectl set image deployment/${serviceName} \
      ${serviceName}=${ECR_URL}/${serviceName}:${GIT_COMMIT.take(8)} \
      --namespace=${namespace}
    kubectl rollout status deployment/${serviceName} \
      --namespace=${namespace} --timeout=120s
  """
}

// Jenkinsfile for any service — 3 lines instead of manual process
@Library('apnakart-shared') _
deployService('payment-service', 'production')
```

### 📊 Result
- Engineer time per deployment: **35–45 minutes → under 2 minutes** (trigger + confirm Slack message).
- Deployment errors from wrong image tags or missed steps: **dropped to zero**.
- Release frequency: **3x increase** — because deployments were no longer painful enough to batch.
- The automation paid back the time invested to build it **within the first week** of deployment.

---

## Q3. What was your biggest failure in DevOps and what did you learn?

> **Method: 🔁 Reflection Method** → Situation → Mistake → Learning → Improvement

### 📍 Situation
Early in my internship, I was asked to update the Terraform configuration for the production EKS node group — specifically to change the instance type from `t3.medium` to `t3.large` to give more headroom for the increased pod density we were planning.

### ❌ Mistake
I made the change in `main.tf`, reviewed it locally, and ran `terraform apply` directly against the production workspace.

I did not realise that changing the instance type on an EKS managed node group forces a **rolling replacement of all nodes** — every node is drained, terminated, and replaced with a new instance type. I had not tested this in a lower environment first. I had not checked the Kubernetes cluster's pod disruption budget configuration. I had not checked if the cluster had enough capacity during the replacement to keep all pods running.

What happened: Kubernetes began draining nodes and rescheduling pods. Several pods in the `production` namespace were in a `Pending` state for 4–6 minutes because the replacement nodes had not joined the cluster yet. During that window, two microservices — the notification service and the inventory service — had zero running replicas. Users received errors on any request that touched those services.

The incident lasted approximately **8 minutes** from first pod eviction to full capacity restored.

### 🧠 Learning

Three things were wrong — not one:

**1. I skipped the lower environment test**

Any infrastructure change that causes node replacement should be tested in dev first — not because the Terraform code is complex, but because the *behaviour* needs to be observed and understood before it runs in production.

```bash
# What I should have run first
terraform workspace select dev
terraform plan    # Review: will this cause node replacement?
terraform apply   # Observe: how long does replacement take? What happens to pods?
```

**2. I did not check PodDisruptionBudgets before draining**

```bash
# Before any node drain or replacement
kubectl get pdb -n production

# If PDB maxUnavailable is 0 — no pod can be evicted
# Terraform/EKS will timeout waiting to drain the node
# Understanding this prevents surprise hangs
```

**3. I did not have a change window or communication plan**

Production infrastructure changes that can cause downtime need:
- A time window (low traffic period)
- A communication to the team ("I'm applying X at 2am — here is the rollback plan")
- A rollback plan written before applying

I had none of these.

### ✅ Improvement
After this incident, I introduced a personal checklist for any Terraform change touching production:

```
Before applying any Terraform change to production:
  □ Has this been applied and observed in dev first?
  □ Does terraform plan show resource replacement? If yes — what is the impact?
  □ Is there a PDB check needed?
  □ Is there a maintenance window for this?
  □ Has the team been informed?
  □ Is there a rollback plan and has it been written down?
```

I also raised this in a team retrospective — not to report myself, but because I suspected others had not thought through these edge cases either. We added the checklist to the team's runbook for infrastructure changes.

The failure cost 8 minutes of partial downtime and cost me significant confidence temporarily. What it gave back was a much more careful mental model of the difference between *Terraform applying cleanly* and *the infrastructure behaving safely during the apply*.

---

## Q4. How do you prioritize tasks during multiple incidents?

> **Method: ⏱️ Prioritization Method** → Situation → Constraints → Prioritization → Execution → Result

### 📍 Situation
A realistic multi-incident scenario: three things break simultaneously.

```
Alert 1: Payment-service — P99 latency at 6s (users cannot complete checkout)
Alert 2: Monitoring stack — Prometheus is down (we are now flying blind)
Alert 3: Jenkins pipeline — CI/CD is failing (no new deployments possible)
```

One engineer. Three incidents. Which one first?

### 🎯 Constraints
- Only one person can actively drive an incident at a time — splitting focus produces three slow resolutions instead of one fast one.
- Some incidents block resolution of others (Prometheus being down means you cannot see metrics to diagnose Alert 1).
- Business impact varies — users cannot pay is categorically worse than engineers cannot deploy.

### ⚖️ Prioritization Framework

Not all incidents are equal. Evaluate on two axes:

```
               HIGH USER IMPACT
                      │
         Alert 1      │
         (Payment)    │
                      │
LOW EFFORT ───────────┼─────────────── HIGH EFFORT
  TO FIX              │
                      │         Alert 2
         Alert 3      │         (Prometheus)
         (Jenkins)    │
                      │
               LOW USER IMPACT
```

**Priority 1 — Alert 1 (Payment-service latency)**

Directly blocking user revenue. Every minute this persists is measurable business loss.

```bash
# Immediate check — is this a regression from a recent deployment?
kubectl rollout history deployment/payment-service -n production
# If a recent rollout → rollback immediately while investigating

kubectl rollout undo deployment/payment-service -n production
```

**Priority 2 — Alert 2 (Prometheus down)**

Prometheus being down does not affect users — but it blinds the team. If payment-service cannot be resolved quickly, we need metrics to diagnose it. Fix Prometheus before spending more than 5 minutes blind.

```bash
kubectl get pods -n monitoring -l app=prometheus
kubectl describe pod prometheus-xxx -n monitoring
# Most common: OOMKilled — increase memory limit or reduce retention
```

**Priority 3 — Alert 3 (Jenkins CI/CD)**

Jenkins down means no new deployments — which means no new bugs but also no hotfixes. This is serious but does not affect running production workloads. Address after the user-facing incidents are resolved.

---

### ⚙️ Execution — Communication Is Part of Incident Management

When handling multiple incidents alone:

```
Slack post immediately:
"Multiple alerts active. Prioritising:
1. Payment-service latency — actively working
2. Prometheus — will address after payment-service is stable
3. Jenkins — queued, not yet started
Single point of contact: me. Updates every 10 minutes."
```

This prevents teammates from duplicating effort and keeps stakeholders informed without requiring them to ask.

---

### 🛡️ Result and Prevention

The payment-service rollback resolved Alert 1 in under 5 minutes. Alert 2 (Prometheus OOMKill) was fixed by increasing the memory limit — 10 minutes. Alert 3 (Jenkins) was a disk space issue resolved in 15 minutes.

Total time: **30 minutes** for three simultaneous incidents.

Post-incident preventions introduced:
- Prometheus memory limit set to match observed p99 usage + 40% headroom.
- Jenkins agent disk cleanup job scheduled via cron — runs at midnight daily.
- PagerDuty escalation configured so if I am unresponsive in 5 minutes, a second engineer is paged — removes single-point-of-failure on the human side.

The most important lesson: **sequentialise parallel incidents**. Trying to context-switch between three active incidents produces three slow resolutions. Commit fully to the highest-impact one, get to a stable state, then move to the next.

---

## Q5. How do you ensure knowledge sharing within your team?

> **Method: 👥 Ownership Method** → Situation → Responsibility → Action → Team Impact

### 📍 Situation
At Hisan Labs, knowledge about the infrastructure lived inside individual engineers' heads. When I joined, there was no documentation for how the Jenkins pipelines were structured, no runbooks for common incidents, and no consistent onboarding path for a new engineer to understand the system. If a senior engineer was on leave, basic incident resolution took 3–4x longer because people did not know where to look.

This is not a rare problem. It is the default state of most small engineering teams moving fast.

### 🎯 Responsibility
I was not the team lead. I could not mandate documentation. But I could make documentation the path of least resistance — so that writing it down was easier than being asked the same question repeatedly.

### ⚙️ Action

**1. Runbooks written immediately after every incident**

```markdown
# Runbook: Payment-service connection pool exhaustion

## Symptoms
- P99 latency > 3s on payment-service
- RDS pg_stat_activity shows >80% connections in 'idle in transaction' state
- HikariPool timeout errors in logs

## Immediate Steps
1. kubectl rollout undo deployment/payment-service -n production
2. Confirm rollback: kubectl rollout status deployment/payment-service -n production
3. Check RDS connections: SELECT state, count(*) FROM pg_stat_activity GROUP BY state;

## Root Cause Checklist
- [ ] New code missing @Transactional annotation?
- [ ] New query using LAZY loading in a non-transactional context?
- [ ] Connection pool size recently reduced?

## Prevention
- Pool connection count alert fires at 80% of max_connections
```

The rule: if it took more than 15 minutes to resolve, it gets a runbook. Runbooks live in the same Git repository as the infrastructure code — they are versioned and reviewable.

---

**2. Pipeline documentation inline in Jenkinsfile**

```groovy
// Jenkinsfile — comments explain WHY, not just WHAT
pipeline {
  stages {
    stage('Parallel: Test + SonarQube') {
      // These two stages are independent of each other.
      // Running in parallel saves ~8 minutes per build.
      // Do NOT add stages here that depend on test output — they will run concurrently.
      parallel {
        stage('Unit Tests') { ... }
        stage('SonarQube') { ... }
      }
    }

    stage('Quality Gate') {
      // This BLOCKS deployment if SonarQube fails.
      // abortPipeline: true means the build is FAILED, not UNSTABLE.
      // Do not change this to abortPipeline: false without team discussion.
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
```

A new engineer reading this Jenkinsfile understands not just what each stage does, but why it exists and what the consequences of changing it would be.

---

**3. Weekly internal knowledge session — 20 minutes, one topic**

Not a formal presentation. A short walkthrough in a huddle:

```
Week 1: How does HPA work and why did it fail last sprint?
Week 2: Walk through what happens when a pod CrashLoopBackOff occurs
Week 3: Terraform state — what is it, why does locking matter?
```

Each session produced one wiki page. Over 3 months: 12 pages of living documentation.

---

**4. Pull request descriptions as knowledge transfer**

```markdown
## PR: Add database connection pool monitoring

### Why this change
We had a production incident (incident-2025-03-15) where connection pool exhaustion
caused 8 minutes of partial downtime. This PR adds a Prometheus alert that fires
when active connections exceed 80% of max_connections.

### What this does
- Adds a recording rule to compute connection utilisation per RDS instance
- Adds an alert with a runbook link (runbooks/rds-connection-pool.md)
- Sets threshold at 80% — empirically, exhaustion always preceded by 10+ minutes at >80%

### How to test
kubectl port-forward svc/prometheus -n monitoring 9090:9090
Navigate to /alerts — "RDSConnectionPoolHigh" should be visible (in inactive state)

### Rollback
kubectl delete -f monitoring/rds-connection-rules.yaml
```

A PR description like this means the reviewer understands the context without asking. It also means anyone reading the Git history 6 months later understands why the alert exists.

---

### 📊 Team Impact

- **Incident resolution time for common issues**: reduced by ~40% because runbooks removed the "where do I look first?" delay.
- **Onboarding:** a new engineer joining the team can understand the full pipeline architecture from Jenkinsfile comments + wiki without needing a dedicated walkthrough session.
- **Single points of failure on people**: reduced — two engineers can handle any common incident using the runbooks, regardless of who wrote the original code.

The most important framing: **documentation is not a nice-to-have — it is part of the work.** An incident that is resolved but not documented has a higher probability of recurring. An automation that works but is not explained cannot be maintained by anyone else. Writing it down is not extra effort on top of the real work — it IS part of the real work.

---

*Behavioral answers are the most personal section of any interview. These answers are structured frameworks — adapt them to your actual experiences and speak them in your own voice. Interviewers detect rehearsed scripts immediately. The structure should be invisible; the story should sound natural.*