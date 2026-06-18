# 🌐 Worldpay Interview Prep — AWS DevOps Engineer
> **Candidate:** Sanket Ajay Chopade | **Role:** Software Engineer I — CI/CD, AWS, Docker | **Team:** Platform Design & Engineering (Sales & Boarding)

---

## 📋 Table of Contents
| # | Section |
|---|---------|
| 1 | [⚙️ CI/CD Questions](#️-cicd-questions) |
| 2 | [☁️ AWS & Cloud Questions](#️-aws--cloud-questions) |
| 3 | [📦 Containers & Kubernetes](#-containers--kubernetes) |
| 4 | [🏗️ Infrastructure as Code](#️-infrastructure-as-code) |
| 5 | [🐧 Linux & Scripting](#-linux--scripting) |
| 6 | [📊 Monitoring & Logging](#-monitoring--logging) |
| 7 | [🧠 Scenario-Based Questions](#-scenario-based-questions) |
| 8 | [🏗️ System Design Questions](#️-system-design-questions) |
| 9 | [💳 Domain + Worldpay-Specific](#-domain--worldpay-specific-questions) |
| 10 | [🔐 Security & Reliability](#-security--reliability) |
| 11 | [🤝 Behavioral Questions](#-behavioral-questions) |
| 12 | [⚡ Rapid-Fire Answers](#-rapid-fire-answers) |
| 13 | [🎯 Smart Questions to Ask Them](#-smart-questions-to-ask-them) |

---

## ⚙️ CI/CD Questions

### Q1 — How have you designed or improved a CI/CD pipeline?

> 🌟 **STAR Method** | Situation → Task → Action → Result

**Situation:** At Hisan Labs, the deployment process was manual, error-prone, and averaged 45 minutes per release. Each step — build, test, Docker build, push to registry, deploy to EKS — was run by hand with no standardization.

**Task:** Design a fully automated, end-to-end Jenkins pipeline that covers build, static analysis, container security scanning, and Kubernetes rollout in a single triggered workflow.

**Action:** Built a multi-stage Declarative Jenkinsfile:

```groovy
pipeline {
    agent any
    environment {
        IMAGE_NAME = "myapp"
        ECR_REPO   = "123456789.dkr.ecr.ap-south-1.amazonaws.com"
        EKS_CLUSTER = "my-cluster"
        REGION      = "ap-south-1"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'mvn clean package -DskipTests' }
        }
        stage('Unit Tests') {
            steps { sh 'mvn test' }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }
        stage('Security Scan') {
            steps {
                sh "trivy image --exit-code 1 --severity CRITICAL ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
        stage('Push to ECR') {
            steps {
                sh """
                    aws ecr get-login-password --region ${REGION} | \
                    docker login --username AWS --password-stdin ${ECR_REPO}
                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${ECR_REPO}/${IMAGE_NAME}:${BUILD_NUMBER}
                    docker push ${ECR_REPO}/${IMAGE_NAME}:${BUILD_NUMBER}
                """
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh """
                    aws eks update-kubeconfig --region ${REGION} --name ${EKS_CLUSTER}
                    kubectl set image deployment/${IMAGE_NAME} \
                      app=${ECR_REPO}/${IMAGE_NAME}:${BUILD_NUMBER} \
                      --namespace production
                    kubectl rollout status deployment/${IMAGE_NAME} --namespace production
                """
            }
        }
    }
    post {
        failure {
            slackSend(color: 'danger', message: "Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
        success {
            slackSend(color: 'good', message: "Build PASSED: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
        }
    }
}
```

**Result:** Deployment time dropped from **45 min → 10 min (77% reduction)**. Release frequency increased 3x. Zero critical vulnerabilities shipped to production. Deployment failure rate dropped 35%.

---

### Q2 — Explain your experience with Jenkins pipelines and GitHub Actions.

> 🧠 **Concept Explanation**

**Jenkins** — Self-hosted CI server. I use Declarative Pipelines (structured, readable, supports `post` blocks and `when` conditions). Best for complex enterprise workflows with fine-grained control.

**GitHub Actions** — Cloud-native CI, triggered directly by GitHub events (push, PR, tag). Lower ops overhead — no Jenkins server to maintain.

**My GitHub Actions workflow example (used in parallel for simpler services):**

```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  AWS_REGION: ap-south-1
  ECR_REPOSITORY: my-service
  EKS_CLUSTER: my-cluster

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1

      - name: Build, tag, push Docker image
        run: |
          docker build -t $ECR_REPOSITORY:${{ github.sha }} .
          docker tag $ECR_REPOSITORY:${{ github.sha }} \
            ${{ steps.login-ecr.outputs.registry }}/$ECR_REPOSITORY:${{ github.sha }}
          docker push ${{ steps.login-ecr.outputs.registry }}/$ECR_REPOSITORY:${{ github.sha }}

      - name: Deploy to EKS
        run: |
          aws eks update-kubeconfig --region $AWS_REGION --name $EKS_CLUSTER
          kubectl set image deployment/$ECR_REPOSITORY \
            app=${{ steps.login-ecr.outputs.registry }}/$ECR_REPOSITORY:${{ github.sha }}
          kubectl rollout status deployment/$ECR_REPOSITORY
```

**Key difference I apply:** Jenkins for production microservices (more control, audit trail, manual approval gates). GitHub Actions for smaller utilities, infra linting, and PR checks.

---

### Q3 — How do you handle pipeline failures and debugging?

> 🔍 **Root Cause Method** | Identify → Diagnose → Fix → Prevent

**Identify:**
```bash
# Jenkins: Check console output for the failing stage
# GitHub Actions: Click failed step → expand logs

# Common exit codes
# exit 1   → script/test failure
# exit 137 → OOMKilled (memory)
# exit 126 → Permission denied
# exit 127 → Command not found
```

**Diagnose:**
```bash
# Reproduce locally
bash -x deploy.sh       # -x traces every command execution

# Check env vars
printenv | grep -i aws
printenv | grep -i kube

# Validate credentials
aws sts get-caller-identity
kubectl config current-context
kubectl get nodes

# If Docker stage fails
docker build --no-cache -t debug-image .   # Force fresh build
docker run --rm -it debug-image /bin/sh    # Inspect container
```

**Fix → Prevent:**
- Add `post { failure { archiveArtifacts 'logs/**' } }` in Jenkins to capture artifacts on failure
- Use `--exit-code 1` on Trivy so security failures block the pipeline explicitly
- Add Slack/email notification on failure so the team knows immediately

---

### Q4 — What is the difference between Declarative and Scripted Pipelines?

> 🧠 **Concept Explanation**

| Feature | Declarative | Scripted |
|---|---|---|
| Syntax | Structured (`pipeline {}` block) | Groovy DSL (`node {}`) |
| Error handling | Built-in `post` blocks | Manual `try/catch` |
| Readability | High — enforced structure | Lower — flexible but verbose |
| Flexibility | Lower | Full Groovy power |
| Recommended for | Most use cases | Complex dynamic logic |

**Declarative (what I use):**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps { sh 'mvn package' }
        }
    }
    post {
        failure { echo 'Build failed' }
    }
}
```

**Scripted (when I'd use it — dynamic parallel stages):**
```groovy
node {
    def services = ['service-a', 'service-b', 'service-c']
    def parallelStages = [:]

    services.each { svc ->
        parallelStages[svc] = {
            stage("Deploy ${svc}") {
                sh "kubectl apply -f k8s/${svc}/"
            }
        }
    }
    parallel parallelStages
}
```

**My default:** Declarative. Switch to Scripted only when I need dynamic stage generation or complex conditional logic that Declarative can't express cleanly.

---

### Q5 — How do you ensure secure CI/CD pipelines?

> 🔐 **Security-First Approach**

**1. Secrets management — never hardcode:**
```groovy
// Jenkins: Use credentials binding
withCredentials([
    string(credentialsId: 'db-password', variable: 'DB_PASS'),
    usernamePassword(credentialsId: 'ecr-creds', usernameVariable: 'USER', passwordVariable: 'PASS')
]) {
    sh 'deploy.sh'
}
```

```yaml
# GitHub Actions: Use encrypted secrets
env:
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
```

**2. Least privilege IAM for the CI runner:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["ecr:GetAuthorizationToken", "ecr:BatchCheckLayerAvailability",
                 "ecr:PutImage", "ecr:InitiateLayerUpload"],
      "Resource": "arn:aws:ecr:ap-south-1:123456789:repository/myapp"
    },
    {
      "Effect": "Allow",
      "Action": ["eks:DescribeCluster"],
      "Resource": "arn:aws:eks:ap-south-1:123456789:cluster/my-cluster"
    }
  ]
}
```

**3. Container image scanning before push:**
```bash
trivy image --exit-code 1 --severity CRITICAL,HIGH myapp:latest
```

**4. Static analysis — fail on quality gate:**
```bash
mvn sonar:sonar
# Configured to fail build if coverage < 80% or critical bugs > 0
```

**5. Branch protection rules:** Require PR review + passing CI before merge to `main`. No direct pushes to production branches.

---

## ☁️ AWS & Cloud Questions

### Q6 — How would you design a scalable application using AWS services?

> 🏗️ **Top-Down Design** | Requirements → Architecture → Components → Scaling → Trade-offs

**Requirements (using Worldpay S&B context as example):**
- Handle variable merchant onboarding traffic (spiky, not uniform)
- High availability (multi-AZ minimum)
- Stateless compute, stateful data layer
- Observability built in

**Architecture:**
```
Internet → Route53 → CloudFront → ALB
                                    ↓
                          EKS (auto-scaled pods)
                         /          |           \
              Service A         Service B      Service C
                  ↓                 ↓               ↓
              RDS (Multi-AZ)   ElastiCache     SQS Queue
                                                    ↓
                                              Lambda (async processing)
```

**Key AWS components and why:**

| Component | Purpose | Scaling mechanism |
|---|---|---|
| ALB | Layer 7 load balancing | Scales automatically |
| EKS | Container orchestration | HPA on CPU/memory |
| RDS Multi-AZ | Relational data | Read replicas for read scaling |
| ElastiCache | Session/cache layer | Cluster mode |
| SQS | Async task queue | Decouples spiky workloads |
| CloudFront | CDN + DDoS protection | Edge caching |
| Auto Scaling Groups | Node-level scaling | Min/max/desired config |

**Trade-off:** This architecture costs more than a single-AZ setup. Justified for payments where 99.99% availability is non-negotiable.

---

### Q7 — What is the difference between EC2, ECS, and EKS?

> 🧠 **Concept Explanation**

| Feature | EC2 | ECS | EKS |
|---|---|---|---|
| What it is | Virtual machine | AWS-managed container orchestration | AWS-managed Kubernetes |
| Control level | Full OS access | Task/service abstractions | Kubernetes API |
| Kubernetes | No | No | Yes |
| Operational overhead | High (you manage OS, patches) | Medium | Low-Medium (control plane managed) |
| Best for | Legacy apps, custom OS needs | Simpler containerized apps on AWS | Complex microservices, multi-cloud portability |
| My experience | EC2 for nodes, bastion hosts | — | Primary container platform |

**When I choose EKS over ECS:** EKS gives me Kubernetes primitives (HPA, NetworkPolicy, RBAC, Helm) that ECS doesn't offer natively. If I need to deploy the same workload on GKE or AKS later, EKS configs are portable. ECS is AWS-proprietary.

---

### Q8 — How does VPC work? Explain subnets, routing, and NAT gateways.

> 🧠 **Concept Explanation**

**VPC (Virtual Private Cloud):** An isolated network in AWS where you define the IP range, subnets, routing, and security rules.

```
VPC: 10.0.0.0/16
│
├── Public Subnet:  10.0.1.0/24  (us-east-1a)  → Internet Gateway
├── Public Subnet:  10.0.2.0/24  (us-east-1b)  → Internet Gateway
├── Private Subnet: 10.0.3.0/24  (us-east-1a)  → NAT Gateway
└── Private Subnet: 10.0.4.0/24  (us-east-1b)  → NAT Gateway
```

**Subnets:** Logical subdivisions of the VPC IP range. Public subnets have a route to the Internet Gateway. Private subnets have a route to the NAT Gateway (outbound only).

**Routing:** Each subnet has a route table.
```
# Public subnet route table
0.0.0.0/0 → igw-xxxxxx   (Internet Gateway — full inbound + outbound)

# Private subnet route table
0.0.0.0/0 → nat-xxxxxx   (NAT Gateway — outbound only, no inbound)
```

**NAT Gateway:** Allows private subnet resources (e.g., EKS worker nodes) to reach the internet for package downloads, ECR pulls — without exposing them to inbound internet traffic.

**My Terraform VPC setup:**
```hcl
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  tags = { Name = "worldpay-vpc" }
}

resource "aws_subnet" "public" {
  count                   = 2
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.${count.index + 1}.0/24"
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
}

resource "aws_subnet" "private" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 3}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
}

resource "aws_nat_gateway" "main" {
  allocation_id = aws_eip.nat.id
  subnet_id     = aws_subnet.public[0].id  # NAT lives in public subnet
}
```

---

### Q9 — How have you used Route53 in real projects?

> 🧾 **Project Walkthrough**

In ApnaKart, I used Route53 for:

**1. DNS routing to ALB:**
```hcl
resource "aws_route53_record" "app" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "api.apnakart.com"
  type    = "A"

  alias {
    name                   = aws_lb.main.dns_name
    zone_id                = aws_lb.main.zone_id
    evaluate_target_health = true
  }
}
```

**2. Health checks + failover routing:**
```hcl
resource "aws_route53_health_check" "primary" {
  fqdn              = "api.apnakart.com"
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3
  request_interval  = 30
}
```

**3. Weighted routing for canary deploys:**
- 90% traffic → stable version
- 10% traffic → canary version

This is how I'd approach Worldpay's BCS deployments — route 10% of onboarding traffic to a new BCS version before full rollout.

---

### Q10 — How do you manage IAM roles and permissions securely?

> 🔐 **Least Privilege Principle**

**Rules I follow:**

1. **Never use root account** — create admin IAM user, MFA-enable it, lock root
2. **Roles over users** for EC2/EKS workloads — no access keys on servers
3. **Least privilege** — grant only the specific actions on specific resources needed

**EKS pod-level IAM with IRSA (IAM Roles for Service Accounts):**
```hcl
# Terraform: Create OIDC-linked IAM role for a specific K8s service account
resource "aws_iam_role" "s3_reader" {
  name = "boarding-service-s3-reader"

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
            "system:serviceaccount:production:boarding-service"
        }
      }
    }]
  })
}

resource "aws_iam_role_policy" "s3_read" {
  role = aws_iam_role.s3_reader.id
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect   = "Allow"
      Action   = ["s3:GetObject", "s3:ListBucket"]
      Resource = ["arn:aws:s3:::boarding-docs/*"]
    }]
  })
}
```

```yaml
# Kubernetes ServiceAccount annotation
apiVersion: v1
kind: ServiceAccount
metadata:
  name: boarding-service
  namespace: production
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::123456789:role/boarding-service-s3-reader
```

**Result:** Pods get scoped AWS credentials via IRSA. No access keys stored anywhere. Credentials rotate automatically.

---

## 📦 Containers & Kubernetes

### Q11 — What is the difference between Docker and Kubernetes?

> 🧠 **Concept Explanation**

| Dimension | Docker | Kubernetes |
|---|---|---|
| What it does | Packages and runs containers | Orchestrates many containers across many nodes |
| Scope | Single host | Cluster of hosts |
| Scaling | Manual (`docker run` again) | Automatic (HPA) |
| Self-healing | None — dead container stays dead | Kubelet restarts failed pods |
| Networking | Bridge/host networking | Cluster DNS, Services, Ingress |
| Analogy | A shipping container | A shipping port that manages thousands of containers |

**Simple answer:** Docker creates containers. Kubernetes manages fleets of containers at scale.

---

### Q12 — Explain how Kubernetes handles scaling and self-healing.

> 🧠 **Concept Explanation**

**Scaling — HPA (Horizontal Pod Autoscaler):**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: boarding-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: boarding-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70   # Scale out when avg CPU > 70%
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

```bash
# Check HPA status
kubectl get hpa -n production
# NAME                    REFERENCE                  TARGETS   MINPODS   MAXPODS   REPLICAS
# boarding-service-hpa    Deployment/boarding-svc    45%/70%   2         10        3
```

**Self-healing mechanisms:**

| Mechanism | What it does |
|---|---|
| Liveness probe | Restarts container if health check fails |
| Readiness probe | Removes pod from service endpoints until ready |
| Deployment controller | Replaces crashed pods to maintain `replicas: N` |
| Node failure | Reschedules pods to healthy nodes |

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
  failureThreshold: 3

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

---

### Q13 — What are Pods, Namespaces, and Services in Kubernetes?

> 🧠 **Concept Explanation**

**Pod:** Smallest deployable unit. One or more containers sharing network + storage. Ephemeral — don't store state in pods.

**Namespace:** Virtual cluster inside a cluster. Used for environment isolation (dev/staging/production) or team isolation.
```bash
kubectl create namespace production
kubectl get all -n production
```

**Services:** Stable network endpoint for a set of pods (pods die and restart, IPs change — Services provide consistent DNS + load balancing).

| Service Type | Use |
|---|---|
| `ClusterIP` | Internal only — pod-to-pod communication |
| `NodePort` | Exposes service on each node's IP at a port |
| `LoadBalancer` | Creates an AWS ALB/NLB automatically |
| `ExternalName` | Maps to external DNS |

```yaml
apiVersion: v1
kind: Service
metadata:
  name: boarding-service
  namespace: production
spec:
  selector:
    app: boarding-service   # Routes to pods with this label
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP
```

---

### Q14 — How would you deploy a microservices application on Kubernetes?

> 🧾 **Project Walkthrough** (ApnaKart — 10 Spring Boot microservices)

**Step 1: Namespace setup**
```bash
kubectl create namespace production
kubectl create namespace monitoring
```

**Step 2: Secrets & ConfigMaps**
```bash
# Create DB credentials secret
kubectl create secret generic db-credentials \
  --from-literal=username=appuser \
  --from-literal=password=<from-vault> \
  --namespace production
```

**Step 3: Deployment manifest per service**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: product-service
  namespace: production
spec:
  replicas: 2
  selector:
    matchLabels:
      app: product-service
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 1
  template:
    metadata:
      labels:
        app: product-service
    spec:
      serviceAccountName: product-service-sa
      containers:
      - name: product-service
        image: 123456789.dkr.ecr.ap-south-1.amazonaws.com/product-service:v1.2.0
        ports:
        - containerPort: 8080
        env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
        - name: DB_URL
          valueFrom:
            configMapKeyRef:
              name: product-config
              key: db_url
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8080
          initialDelaySeconds: 60
        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8080
          initialDelaySeconds: 30
```

**Step 4: Ingress for external routing**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: main-ingress
  namespace: production
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: api.apnakart.com
    http:
      paths:
      - path: /products
        pathType: Prefix
        backend:
          service:
            name: product-service
            port:
              number: 80
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: order-service
            port:
              number: 80
```

**Step 5: HPA per service**
```bash
kubectl autoscale deployment product-service \
  --cpu-percent=70 --min=2 --max=8 \
  --namespace production
```

---

### Q15 — What issues have you faced with containers and how did you fix them?

> 🔍 **Root Cause Method**

**Issue 1: OOMKilled pods**
```bash
# Identify
kubectl describe pod <pod> | grep -A5 "Last State"
# Exit Code: 137 = OOMKilled

# Fix: Set proper limits after profiling actual usage
kubectl top pod <pod> --namespace production

# Update deployment
kubectl patch deployment myapp -p '{
  "spec": {"template": {"spec": {"containers": [{
    "name": "myapp",
    "resources": {"limits": {"memory": "512Mi"}, "requests": {"memory": "256Mi"}}
  }]}}}}'
```

**Issue 2: Image pull errors (ECR auth)**
```bash
# Error: ImagePullBackOff
kubectl describe pod <pod> | grep "Failed to pull image"

# Fix: Ensure node IAM role has ECR pull permissions
aws iam attach-role-policy \
  --role-name eks-node-role \
  --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
```

**Issue 3: Container port mismatch**
```bash
# App listening on 8080, Service targeting 80 — connection refused
# Fix: Align containerPort, targetPort, and application bind port
kubectl get svc myapp -o yaml | grep -A5 "ports"
# Corrected targetPort to 8080
```

**Issue 4: Large Docker image slowing CI (500MB+)**
```dockerfile
# Fix: Multi-stage build
FROM maven:3.9 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline    # Cache deps layer separately
COPY src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:17-jre-alpine   # Minimal runtime image
COPY --from=build /app/target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
# Result: 500MB → 200MB (60% reduction)
```

---

## 🏗️ Infrastructure as Code

### Q16 — What is Terraform and how does it work?

> 🧠 **Concept Explanation** | Definition → How it Works → Example

**Definition:** Terraform is a declarative IaC tool. You define the desired end-state of your infrastructure in `.tf` files. Terraform figures out what needs to be created, changed, or destroyed to reach that state.

**Core workflow:**
```
Write .tf files → terraform init → terraform plan → terraform apply → State stored
```

```bash
# 1. Initialize: Download providers, set up backend
terraform init

# 2. Plan: Show what WILL change (dry run)
terraform plan -out=tfplan
# Output shows:
# + create
# ~ update in-place
# -/+ destroy and recreate  ← READ CAREFULLY

# 3. Apply: Execute the plan
terraform apply tfplan

# 4. Destroy: Tear down resources
terraform destroy -target=module.rds
```

**How it tracks state:** Terraform writes a `terraform.tfstate` file that maps your `.tf` config to real cloud resources. This is how it knows what to change vs create from scratch.

---

### Q17 — Explain Terraform state and how you manage it.

> 🧠 **Concept Explanation**

**State file:** JSON file tracking every resource Terraform manages. Critical — if lost or corrupted, Terraform loses track of real infrastructure.

**Problem with local state:** Can't be shared across a team. No locking → two engineers run `apply` simultaneously → race condition → corrupted state.

**Solution: Remote state with S3 + DynamoDB locking:**
```hcl
terraform {
  backend "s3" {
    bucket         = "worldpay-terraform-state"
    key            = "sales-boarding/production/terraform.tfstate"
    region         = "ap-south-1"
    encrypt        = true                      # AES-256 encryption at rest
    dynamodb_table = "terraform-state-lock"   # Prevents concurrent applies
  }
}
```

```bash
# DynamoDB table for state locking
aws dynamodb create-table \
  --table-name terraform-state-lock \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

# Useful state commands
terraform state list                          # List all managed resources
terraform state show aws_eks_cluster.main    # Inspect specific resource
terraform state rm aws_s3_bucket.old         # Remove from state (without destroying)
terraform import aws_s3_bucket.existing mybucket  # Import existing resource
```

---

### Q18 — Difference between Terraform and CloudFormation.

> ⚖️ **Compare & Justify**

| Feature | Terraform | CloudFormation |
|---|---|---|
| Provider | Multi-cloud (AWS, GCP, Azure, K8s, GitHub...) | AWS only |
| Syntax | HCL (readable) | YAML/JSON (verbose) |
| State management | You manage (S3 + DynamoDB) | AWS manages |
| Plan preview | `terraform plan` — explicit diff | Change sets (less detailed) |
| Module ecosystem | Terraform Registry — large | Limited |
| New AWS features | Slight lag (days-weeks) | Day-0 support |
| Drift detection | `terraform plan` shows drift | CloudFormation Drift Detection |
| Destroy resources | `terraform destroy` | Stack deletion |

**My choice:** Terraform. Portability matters — if Worldpay's S&B team ever needs Azure resources or Kubernetes-level infra management, Terraform handles it in the same workflow. CloudFormation would require a separate toolchain.

---

### Q19 — Write a simple Terraform workflow (init → plan → apply).

> 🧠 **Concept Explanation with Example**

**Complete working example — S3 bucket with versioning:**

```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "demo/terraform.tfstate"
    region = "ap-south-1"
  }
}

provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "merchant_docs" {
  bucket = "${var.project}-merchant-documents-${var.environment}"
  tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

resource "aws_s3_bucket_versioning" "merchant_docs" {
  bucket = aws_s3_bucket.merchant_docs.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "merchant_docs" {
  bucket = aws_s3_bucket.merchant_docs.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}
```

```hcl
# variables.tf
variable "region"      { default = "ap-south-1" }
variable "project"     { default = "worldpay-sb" }
variable "environment" { default = "production" }
```

```bash
# Full workflow
terraform init                          # Download AWS provider
terraform validate                      # Syntax check
terraform fmt                           # Auto-format
terraform plan -var="environment=staging" -out=tfplan
terraform apply tfplan
terraform output                        # Show outputs
terraform destroy -target=aws_s3_bucket.merchant_docs  # Targeted destroy
```

---

### Q20 — How do you handle secrets in Terraform?

> 🔐 **Security Method**

**The problem:** Terraform state stores resource attributes in plaintext JSON — including passwords if you pass them directly.

**Wrong approach:**
```hcl
# NEVER DO THIS
resource "aws_db_instance" "main" {
  password = "mysupersecretpassword"   # Stored in state file in plaintext
}
```

**Correct approaches:**

**1. AWS Secrets Manager (preferred):**
```hcl
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "production/boarding/db-password"
}

resource "aws_db_instance" "main" {
  password = data.aws_secretsmanager_secret_version.db_password.secret_string
}
```

**2. Environment variables (for local use):**
```bash
export TF_VAR_db_password="mysecretpassword"
terraform apply
# Terraform auto-reads TF_VAR_* env vars
```

**3. Mark sensitive in variables.tf:**
```hcl
variable "db_password" {
  description = "Database password"
  type        = string
  sensitive   = true   # Redacted in plan output and logs
}
```

**4. Encrypt state at rest:**
```hcl
backend "s3" {
  encrypt = true   # AES-256 — state file encrypted
}
```

---

## 🐧 Linux & Scripting

### Q21 — What Linux commands do you frequently use in DevOps?

> 🧠 **Concept Explanation**

```bash
# Process management
ps aux | grep java                    # Find running Java processes
top / htop                            # Live CPU/memory view
kill -9 <pid>                         # Force kill process
systemctl status nginx                # Service status
journalctl -u nginx --since "1 hour ago"  # Service logs

# File operations
find /var/log -name "*.log" -mtime +7 -delete   # Delete logs older than 7 days
tail -f /var/log/app/application.log             # Live log following
grep -r "ERROR" /var/log/app/ | grep "$(date +%Y-%m-%d)"  # Today's errors
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head   # Top IPs

# Disk & memory
df -h                                 # Disk usage
du -sh /var/log/*                    # Directory sizes
free -m                              # Memory usage
lsof -p <pid>                        # Files opened by process

# Network
netstat -tlnp                        # Listening ports
ss -tlnp                             # Modern netstat
curl -I https://api.worldpay.com/health   # HTTP headers
tcpdump -i eth0 port 8080            # Packet capture
nslookup boarding.worldpay.com       # DNS resolution

# AWS-specific Linux
aws logs tail /aws/eks/my-cluster/cluster --follow
curl http://169.254.169.254/latest/meta-data/instance-id   # EC2 metadata
```

---

### Q22 — How do you debug a high CPU or memory issue in Linux?

> 🔍 **Root Cause Method** | Identify → Diagnose → Fix → Prevent

```bash
# Step 1: Identify the offending process
top -b -n 1 | head -20
# OR
ps aux --sort=-%cpu | head -10     # Sort by CPU
ps aux --sort=-%mem | head -10     # Sort by memory

# Step 2: Get the PID, inspect it
PID=12345
ls -la /proc/$PID/exe              # What binary is running?
cat /proc/$PID/cmdline | tr '\0' ' '   # Full command with args

# Step 3: Check what files/connections it has open
lsof -p $PID | wc -l              # Number of open file descriptors
lsof -p $PID | grep "ESTABLISHED"  # Active network connections

# Step 4: If Java app — thread dump
jstack $PID > /tmp/threaddump.txt
# Look for: deadlocks, threads stuck in I/O, runaway loops

# Step 5: If memory — check for leak
# Kubernetes context:
kubectl top pod --namespace production --sort-by=memory
kubectl describe pod <high-mem-pod> | grep -A5 "Limits\|Requests\|OOM"

# Step 6: Linux OOM log
dmesg | grep -i "oom\|killed"
journalctl -k | grep -i oom
```

**Prevent:** Set resource limits in Kubernetes deployments. Add Grafana alert when pod memory > 80% of limit.

---

### Q23 — Explain shell scripting basics with an example.

> 🧠 **Concept Explanation**

```bash
#!/bin/bash
# deploy-service.sh — Deploy a service to EKS with health check
# Usage: ./deploy-service.sh <service-name> <image-tag>

set -euo pipefail    # -e exit on error, -u error on unset var, -o pipefail

SERVICE_NAME="${1:?Error: service name required}"
IMAGE_TAG="${2:?Error: image tag required}"
NAMESPACE="production"
ECR="123456789.dkr.ecr.ap-south-1.amazonaws.com"
TIMEOUT=300

log() { echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1"; }

# Check AWS connectivity
if ! aws sts get-caller-identity &>/dev/null; then
    log "ERROR: AWS credentials not configured"
    exit 1
fi

log "Deploying ${SERVICE_NAME}:${IMAGE_TAG} to ${NAMESPACE}"

# Update image
kubectl set image deployment/${SERVICE_NAME} \
    ${SERVICE_NAME}=${ECR}/${SERVICE_NAME}:${IMAGE_TAG} \
    --namespace ${NAMESPACE}

# Wait for rollout
if kubectl rollout status deployment/${SERVICE_NAME} \
    --namespace ${NAMESPACE} \
    --timeout=${TIMEOUT}s; then
    log "SUCCESS: ${SERVICE_NAME} deployed"
else
    log "FAILED: Rolling back ${SERVICE_NAME}"
    kubectl rollout undo deployment/${SERVICE_NAME} --namespace ${NAMESPACE}
    exit 1
fi

# Health check
ENDPOINT=$(kubectl get svc ${SERVICE_NAME} -n ${NAMESPACE} -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
HTTP_STATUS=$(curl -s -o /dev/null -w "%{http_code}" "http://${ENDPOINT}/health")

if [[ "$HTTP_STATUS" == "200" ]]; then
    log "Health check passed: HTTP ${HTTP_STATUS}"
else
    log "Health check FAILED: HTTP ${HTTP_STATUS}"
    exit 1
fi
```

```bash
chmod +x deploy-service.sh
./deploy-service.sh boarding-gateway v2.1.0
```

---

### Q24 — How do you automate repetitive tasks using scripts?

> 🧾 **Project Walkthrough**

**Real example — automated stale pod log cleanup:**

```bash
#!/bin/bash
# cleanup-logs.sh — Archive and rotate logs older than 7 days

LOG_DIR="/var/log/app"
ARCHIVE_DIR="/var/log/archive"
RETENTION_DAYS=7

mkdir -p "${ARCHIVE_DIR}"

find "${LOG_DIR}" -name "*.log" -mtime +${RETENTION_DAYS} | while read -r logfile; do
    filename=$(basename "${logfile}")
    gzip -c "${logfile}" > "${ARCHIVE_DIR}/${filename}.$(date +%Y%m%d).gz"
    rm "${logfile}"
    echo "Archived: ${filename}"
done

# Remove archives older than 30 days
find "${ARCHIVE_DIR}" -name "*.gz" -mtime +30 -delete

echo "Cleanup complete. Disk usage: $(df -h ${LOG_DIR} | tail -1 | awk '{print $5}')"
```

```bash
# Schedule with cron — run at 2 AM daily
crontab -e
# 0 2 * * * /opt/scripts/cleanup-logs.sh >> /var/log/cleanup.log 2>&1
```

**Other automations I've implemented:**
- Auto-tagging ECR images with Git commit SHA + timestamp
- Slack notifications on failed Kubernetes rollouts
- Daily Terraform plan diffs emailed to the team to catch infrastructure drift

---

## 📊 Monitoring & Logging

### Q25 — How do you monitor applications in production?

> 🧾 **Project Walkthrough**

**My observability stack (at Hisan Labs):**

```
Metrics:  Prometheus (scrape) → Grafana (visualize) + AWS CloudWatch
Logs:     Application → Fluentd → Elasticsearch → Kibana (ELK)
Tracing:  Planned: Jaeger / AWS X-Ray
Alerting: Prometheus AlertManager → Slack / PagerDuty
```

**Three pillars:**

| Pillar | Tool | What I track |
|---|---|---|
| Metrics | Prometheus + Grafana | CPU, memory, request rate, error rate, deployment frequency |
| Logs | ELK Stack | Application errors, access logs, audit events |
| Alerts | AlertManager | Pod down, high error rate, memory > 85%, disk > 90% |

**Key dashboards I built:**
- Deployment frequency vs error rate correlation
- Per-service request latency (p50, p95, p99)
- EKS node resource utilization
- CI/CD pipeline success rate over time

---

### Q26 — Explain how Prometheus works.

> 🧠 **Concept Explanation**

**How it works:**
```
Application → /metrics endpoint (HTTP)
                     ↑
              Prometheus scrapes every 15s
                     ↓
              Time-series database (TSDB)
                     ↓
              PromQL queries → Grafana dashboards
                     ↓
              AlertManager → Slack/PagerDuty
```

**Prometheus scrape config:**
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'boarding-service'
    kubernetes_sd_configs:
    - role: pod
    relabel_configs:
    - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
      action: keep
      regex: true
    - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
      target_label: __metrics_path__
```

**Example PromQL queries I use:**
```promql
# Request rate (per second, 5m window)
rate(http_requests_total{service="boarding-service"}[5m])

# Error rate percentage
rate(http_requests_total{status=~"5.."}[5m]) /
rate(http_requests_total[5m]) * 100

# 95th percentile latency
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))

# Pod memory usage vs limit
container_memory_working_set_bytes / container_spec_memory_limit_bytes * 100
```

---

### Q27 — What is alerting and how do you configure it?

> 🧠 **Concept Explanation**

**Prometheus alert rules:**
```yaml
# alert-rules.yml
groups:
- name: production-alerts
  rules:
  - alert: PodCrashLooping
    expr: rate(kube_pod_container_status_restarts_total[15m]) * 60 * 15 > 0
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Pod {{ $labels.pod }} is crash-looping"
      description: "{{ $labels.pod }} in {{ $labels.namespace }} has restarted {{ $value }} times"

  - alert: HighErrorRate
    expr: rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) > 0.05
    for: 2m
    labels:
      severity: warning
    annotations:
      summary: "Error rate > 5% on {{ $labels.service }}"

  - alert: HighMemoryUsage
    expr: container_memory_working_set_bytes / container_spec_memory_limit_bytes > 0.85
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "Container {{ $labels.container }} memory > 85%"
```

**AlertManager routing to Slack:**
```yaml
# alertmanager.yml
route:
  group_by: ['alertname', 'namespace']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  receiver: 'slack-critical'
  routes:
  - match:
      severity: critical
    receiver: 'pagerduty-oncall'

receivers:
- name: 'slack-critical'
  slack_configs:
  - api_url: '{{ secrets.SLACK_WEBHOOK }}'
    channel: '#incidents'
    title: '{{ .GroupLabels.alertname }}'
    text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
```

---

### Q28 — Difference between logging and monitoring.

> 🧠 **Concept Explanation**

| Dimension | Logging | Monitoring |
|---|---|---|
| What it captures | Discrete events (what happened) | Continuous metrics (how the system is behaving) |
| Format | Structured/unstructured text | Numeric time-series data |
| Query pattern | Search by keyword/timestamp | Aggregate, compare, threshold |
| Tools | ELK, Loki, CloudWatch Logs | Prometheus, Grafana, Datadog |
| Best for | Debugging a specific incident | Detecting trends, setting alerts |
| Example | `ERROR: NullPointerException in BoardingService:142` | `error_rate = 3.2% over last 5 min` |

**They complement each other:** Monitoring alerts you that something is wrong. Logging tells you exactly why.
---

## 🧠 Scenario-Based Questions

### Q29 — Your deployment failed in production. What steps will you take?

> 🚨 **Action Plan** | Clarify → Immediate Action → Investigation → Communication → Prevention

**Clarify (2 min):**
```bash
# What failed? Full outage or partial?
kubectl get pods -n production
kubectl get events -n production --sort-by='.lastTimestamp' | tail -20

# Was there a recent deployment?
kubectl rollout history deployment/boarding-gateway -n production
```

**Immediate Action — Rollback first, investigate second:**
```bash
# Rollback immediately if deployment is the cause
kubectl rollout undo deployment/boarding-gateway -n production
kubectl rollout status deployment/boarding-gateway -n production --timeout=120s

# Verify service is healthy post-rollback
kubectl get pods -n production -l app=boarding-gateway
curl -f https://boarding.worldpay.com/health
```

**Investigation:**
```bash
# Inspect failed pods
kubectl describe pod <failed-pod> -n production
kubectl logs <failed-pod> -n production --previous

# Check AWS ALB target health
aws elbv2 describe-target-health \
  --target-group-arn <arn>

# Check CloudWatch for upstream errors
aws logs tail /aws/eks/production/cluster --since 30m --follow
```

**Communication:**
```
#incidents Slack:
Severity: P1
Service: boarding-gateway
Impact: Merchant onboarding blocked
Status: Rolled back to v2.0.3 — stabilizing
ETA: Investigating root cause, update in 15 min
```

**Prevention:**
- Add canary deployment step — route 5% traffic before full rollout
- Gate deployment with automated smoke test against staging first
- Set up Prometheus alert for error rate spike within 5 min of deploy

---

### Q30 — CI/CD pipeline is slow. How will you optimize it?

> 🔍 **Root Cause Method**

**Measure first — identify the bottleneck:**
```bash
# Jenkins: Check build time per stage in Blue Ocean or Pipeline Duration Trend plugin
# GitHub Actions: Review step durations in the Actions tab
```

**Common bottlenecks and fixes:**

| Problem | Fix | Impact |
|---|---|---|
| Maven re-downloads deps every build | Cache `~/.m2` between builds | -40% build time |
| Docker layers not cached | Order Dockerfile layers — COPY pom.xml before COPY src | -30% image build |
| Sequential stages that could parallel | Run unit tests + static analysis in parallel | -50% test time |
| No agent reuse | Use persistent Jenkins agents | Eliminates startup overhead |
| Full test suite on every commit | Run unit tests on PR, full suite on merge to main | -60% PR build time |

**Parallel stages in Declarative Pipeline:**
```groovy
stage('Quality Checks') {
    parallel {
        stage('Unit Tests') {
            steps { sh 'mvn test' }
        }
        stage('SonarQube') {
            steps {
                withSonarQubeEnv('SQ') { sh 'mvn sonar:sonar' }
            }
        }
        stage('Security Scan') {
            steps { sh 'trivy fs --severity HIGH,CRITICAL .' }
        }
    }
}
```

**Docker layer caching:**
```dockerfile
# Cache-optimized layer order
FROM maven:3.9 AS build
COPY pom.xml .                    # Layer 1: pom (changes rarely)
RUN mvn dependency:go-offline     # Layer 2: deps (cached until pom changes)
COPY src ./src                    # Layer 3: code (changes often)
RUN mvn package -DskipTests
```

**My result at Hisan Labs:** Combined parallel stages + Docker layer caching + Maven dep caching cut CI time from 18 min → 10 min per build.

---

### Q31 — A Kubernetes pod is crashing repeatedly. How do you debug?

> 🔍 **Root Cause Method**

```bash
# Step 1: Confirm state and get pod name
kubectl get pods -n production
# NAME                      READY   STATUS             RESTARTS
# boarding-svc-xxx-yyy      0/1     CrashLoopBackOff   7

# Step 2: Get crash details
kubectl describe pod boarding-svc-xxx-yyy -n production
# Key fields: Exit Code, Reason, Last State, Events

# Exit code cheatsheet:
# 0   = success (shouldn't crash)
# 1   = app error
# 137 = OOMKilled (memory limit hit)
# 139 = Segfault
# 126 = Permission denied
# 127 = Command not found (wrong entrypoint)

# Step 3: Get logs
kubectl logs boarding-svc-xxx-yyy -n production
kubectl logs boarding-svc-xxx-yyy -n production --previous  # Before crash

# Step 4: Resource usage
kubectl top pod boarding-svc-xxx-yyy -n production

# Step 5: If you need to exec in (only works if container starts briefly)
kubectl exec -it boarding-svc-xxx-yyy -n production -- /bin/sh

# Step 6: Check Events
kubectl get events -n production --sort-by='.lastTimestamp' | grep boarding
```

**Common fixes:**
```bash
# OOMKilled → increase memory limit
kubectl patch deployment boarding-svc -n production -p '{
  "spec":{"template":{"spec":{"containers":[{
    "name":"boarding-svc",
    "resources":{"limits":{"memory":"512Mi"},"requests":{"memory":"256Mi"}}
  }]}}}}'

# Wrong entrypoint → inspect image
docker run --rm --entrypoint /bin/sh myimage:latest -c "ls /app"
```

---

### Q32 — Website latency suddenly increases. How do you investigate?

> 🔍 **Root Cause Method**

```bash
# Step 1: Confirm and quantify
# Check Grafana: p95 latency dashboard
# PromQL: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))

# Step 2: Is it all endpoints or one?
# Filter by route in Grafana to isolate affected service

# Step 3: Check pod resource saturation
kubectl top pods -n production
kubectl top nodes

# Step 4: Check if a recent deploy correlates with latency spike
kubectl rollout history deployment/boarding-service -n production

# Step 5: Check downstream dependencies
# RDS slow query log
aws rds describe-db-log-files --db-instance-identifier boarding-db
aws rds download-db-log-file-portion \
  --db-instance-identifier boarding-db \
  --log-file-name slowquery/mysql-slowquery.log \
  --output text

# Step 6: Check network — ALB access logs
aws s3 cp s3://alb-logs/production/ /tmp/alb-logs/ --recursive
grep "0.5" /tmp/alb-logs/*.log   # Requests > 500ms

# Step 7: External dependency check
curl -w "@curl-timing.txt" -o /dev/null -s https://third-party-api.com/endpoint
# curl-timing.txt: time_namelookup, time_connect, time_starttransfer
```

**Root causes I check in order:** Database slow query → external API timeout → HPA not yet scaled (CPU spike) → network bottleneck → memory pressure causing GC pauses.

---

### Q33 — You need zero-downtime deployment. How will you implement it?

> 🏗️ **Top-Down Design**

**Option 1: Rolling Update (Kubernetes default — what I use):**
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0    # Never kill old pods before new ones are ready
    maxSurge: 1          # One extra pod during transition
```

**Option 2: Blue-Green (for high-risk releases):**
```bash
# Blue (current) is live. Deploy Green (new) side-by-side.
kubectl apply -f deployment-green.yaml

# Test green internally
kubectl port-forward svc/boarding-service-green 8080:80

# Switch traffic: Update Service selector
kubectl patch svc boarding-service -p '{"spec":{"selector":{"version":"green"}}}'

# Rollback = flip selector back to blue (instant)
kubectl patch svc boarding-service -p '{"spec":{"selector":{"version":"blue"}}}'
```

**Option 3: Canary (for production validation):**
```yaml
# Route53 weighted routing: 90% stable, 10% canary
resource "aws_route53_record" "boarding_canary" {
  weighted_routing_policy {
    weight = 10
  }
  set_identifier = "canary"
  # ...
}
```

**Pre-requisites for all strategies:**
- Readiness probes — traffic only goes to ready pods
- Graceful shutdown — pods drain existing connections before terminating
```yaml
lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 5"]  # Allow in-flight requests to complete
terminationGracePeriodSeconds: 30
```

---

### Q34 — How would you migrate an app from on-prem to AWS?

> 🏗️ **Top-Down Design** | Requirements → Architecture → Components → Scaling → Trade-offs

**Phase 1: Assess (1-2 weeks)**
```
- Inventory: What services, databases, dependencies?
- Dependencies: External APIs, LDAP, file shares?
- Data: How much? How sensitive? Compliance requirements?
- Traffic patterns: Peak loads, latency requirements?
```

**Phase 2: Foundation (2-4 weeks)**
```bash
# Set up VPC, networking
terraform apply -target=module.vpc

# Set up IAM roles
terraform apply -target=module.iam

# Set up ECR registries
aws ecr create-repository --repository-name boarding-service
```

**Phase 3: Containerize**
```dockerfile
# Containerize each service with multi-stage builds
# Test locally → push to ECR
docker build -t boarding-service:v1 .
docker tag boarding-service:v1 ${ECR_REPO}/boarding-service:v1
docker push ${ECR_REPO}/boarding-service:v1
```

**Phase 4: Data migration**
```bash
# Use AWS DMS for database migration
# Run parallel: on-prem DB (primary) + RDS (replica)
# Validate data consistency → cut over during low-traffic window
aws dms create-replication-task \
  --replication-task-identifier boarding-migration \
  --source-endpoint-arn <on-prem-endpoint> \
  --target-endpoint-arn <rds-endpoint> \
  --migration-type full-load-and-cdc
```

**Phase 5: Cutover**
```
- Update Route53 to point to AWS ALB
- Monitor for 24h with old system still running (rollback option)
- Decommission on-prem after validation period
```

---

## 🏗️ System Design Questions

### Q35 — Design a CI/CD pipeline for a microservices architecture.

> 🏗️ **Top-Down Design**

```
Developer Push → GitHub → Jenkins/GitHub Actions
                               ↓
                    ┌─── Per-Service Pipeline ───┐
                    │  1. Checkout                │
                    │  2. Unit Tests (parallel)   │
                    │  3. SonarQube (parallel)    │
                    │  4. Docker Build            │
                    │  5. Trivy Scan              │
                    │  6. Push to ECR             │
                    │  7. Deploy to Staging (EKS) │
                    │  8. Integration Tests       │
                    │  9. Manual Approval Gate    │
                    │ 10. Deploy to Production    │
                    │ 11. Smoke Test              │
                    │ 12. Rollback if failed      │
                    └─────────────────────────────┘
```

**Service-level isolation:** Each microservice has its own pipeline. Deploying the payment service doesn't trigger the notification service pipeline.

**Shared infrastructure pipeline (separate):**
```
Terraform changes → terraform plan → Manual approval → terraform apply
```

**Key design decisions:**
- ECR as image registry — immutable tags (use Git SHA, not `latest`)
- Helm charts per service for Kubernetes manifests
- ArgoCD for GitOps-based production deploys (pull-based, more secure than push)
- Environment promotion: Dev → Staging (auto) → Production (manual gate)

---

### Q36 — Design a highly available system on AWS.

> 🏗️ **Top-Down Design**

```
                    Route53 (Health Checks + Failover)
                              ↓
                    CloudFront (CDN + DDoS)
                              ↓
                    ALB (Multi-AZ)
                    /                \
            EKS Node (AZ-1)    EKS Node (AZ-2)
                    \                /
                RDS Multi-AZ (Primary + Standby)
                    ↓
            ElastiCache (Redis Cluster Mode)
```

**HA components:**

| Component | HA Mechanism |
|---|---|
| ALB | Spans multiple AZs natively |
| EKS | Nodes in 2+ AZs, pods spread via `topologySpreadConstraints` |
| RDS | Multi-AZ with automatic failover (~60s) |
| ElastiCache | Cluster mode with replicas |
| Route53 | Health checks fail over DNS if ALB unhealthy |

```yaml
# Kubernetes: Spread pods across AZs
topologySpreadConstraints:
- maxSkew: 1
  topologyKey: topology.kubernetes.io/zone
  whenUnsatisfiable: DoNotSchedule
  labelSelector:
    matchLabels:
      app: boarding-service
```

**Target: 99.99% availability** (52 min downtime/year max)

---

### Q37 — How would you deploy across multiple regions?

> 🏗️ **Top-Down Design**

**Active-Passive (simpler — Worldpay likely uses for DR):**
```
Primary: ap-south-1 (Mumbai)
DR:      eu-west-1 (London — Worldpay international HQ)

Route53 Failover routing:
- Primary health check: api.worldpay.com → ap-south-1
- If primary unhealthy → auto-fail to eu-west-1
```

**Active-Active (complex — for 146 countries scale):**
```
Route53 Latency routing:
- APAC users → ap-south-1
- European users → eu-west-1
- US users → us-east-1

Data layer: Aurora Global Database
- Primary: us-east-1 (writes)
- Read replicas: eu-west-1, ap-south-1 (<1s lag)
```

**Terraform multi-region:**
```hcl
provider "aws" {
  alias  = "mumbai"
  region = "ap-south-1"
}

provider "aws" {
  alias  = "london"
  region = "eu-west-1"
}

module "eks_mumbai" {
  source    = "./modules/eks"
  providers = { aws = aws.mumbai }
}

module "eks_london" {
  source    = "./modules/eks"
  providers = { aws = aws.london }
}
```

---

### Q38 — Design a logging and monitoring system for production apps.

> 🏗️ **Top-Down Design**

```
Application Pods
      ↓ stdout/stderr
Fluentd DaemonSet (collect from all nodes)
      ↓
Elasticsearch (store + index)
      ↓
Kibana (search + visualize logs)

Parallel:
Prometheus (scrape /metrics)
      ↓
Grafana (dashboards + alerts)
      ↓
AlertManager → Slack / PagerDuty
```

**Three tiers of alerting:**

| Tier | Condition | Action |
|---|---|---|
| P1 — Critical | Service down, error rate > 20% | PagerDuty → on-call |
| P2 — Warning | Error rate > 5%, pod restarting | Slack #incidents |
| P3 — Info | Deployment completed, scaling event | Slack #deploys |

**Log retention policy:**
- Hot (0-7 days): Elasticsearch — fast search
- Warm (7-30 days): Elasticsearch reduced replicas
- Cold (30-90 days): S3 Glacier — cost-optimized
- Delete after 90 days (compliance may override)

---

### Q39 — How would you handle 1M+ daily transactions?

> 🏗️ **Top-Down Design** (Worldpay processes 130M+/day — this is core domain)

**1M/day = ~11.5 TPS average, ~100 TPS at peak (10x factor)**

**Architecture for 100 TPS sustained:**
```
API Gateway / ALB
      ↓
Stateless service pods (EKS, HPA)
      ↓
SQS Queue (buffer spikes, decouple)
      ↓
Transaction processor (EKS workers)
      ↓
RDS Aurora (write) + Read Replicas (query)
      ↓
ElastiCache (idempotency keys, session state)
```

**Key patterns:**

**Idempotency:** Every transaction request carries a unique key. Duplicate requests return the same result without double-processing.
```python
def process_transaction(request):
    key = f"txn:{request.idempotency_key}"
    if redis.exists(key):
        return redis.get(key)  # Return cached result
    result = execute_transaction(request)
    redis.setex(key, 3600, result)  # Cache for 1 hour
    return result
```

**Database:** Aurora auto-scales storage. Read replicas handle reporting/audit queries. Write path goes only to primary.

**Circuit breaker:** If downstream payment processor is slow, fail fast and queue for retry rather than cascading timeout.

**Horizontal scaling:** HPA scales pods automatically. At 100 TPS, ~5 pods with 500m CPU each handles it comfortably.

---

## 💳 Domain + Worldpay-Specific Questions

### Q40 — What challenges arise in payment systems infrastructure?

> 🧠 **Concept Explanation**

| Challenge | Why it matters | How to address |
|---|---|---|
| **Zero downtime** | Every second of downtime = lost transactions | Multi-AZ, rolling deploys, circuit breakers |
| **Data consistency** | Double-charge or missing charge = legal liability | Idempotency keys, distributed transactions (saga pattern) |
| **Compliance (PCI-DSS)** | Card data handling has strict rules | Network segmentation, encryption at rest/transit, audit logs |
| **Scale spikes** | Black Friday, flash sales | HPA, SQS buffering, pre-scaling |
| **Latency** | Customers abandon slow checkouts | p99 < 500ms target, caching, CDN |
| **Fraud detection** | Real-time decisioning | Low-latency ML inference, async enrichment |
| **Multi-currency/region** | 135 currencies, 146 countries | Regional deployments, locale-aware routing |

---

### Q41 — What is fault tolerance and why is it critical in payments?

> 🧠 **Concept Explanation**

**Fault tolerance:** The system continues operating correctly even when individual components fail.

**Why critical in payments:** A failed transaction is not just a UX problem — it's a financial event. Partial failures (request sent, response lost) can cause double-charges or ghost transactions.

**Mechanisms I implement:**

```python
# Retry with exponential backoff + jitter
import time, random

def call_payment_processor(request, max_retries=3):
    for attempt in range(max_retries):
        try:
            return payment_api.charge(request)
        except TransientError as e:
            if attempt == max_retries - 1:
                raise
            wait = (2 ** attempt) + random.random()  # Exponential + jitter
            time.sleep(wait)
```

```yaml
# Kubernetes: Pod Disruption Budget — never take down more than 1 replica during node drain
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: boarding-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: boarding-service
```

**At Worldpay's scale (130M txn/day):** Even 0.001% failure rate = 130,000 failed transactions/day. Fault tolerance isn't optional — it's the core product guarantee.

---

### Q42 — How would you secure payment-related infrastructure?

> 🔐 **Security-First Design**

**Network segmentation:**
```
Public:  ALB only
Private: EKS workers, Lambda
Isolated: RDS (no route to internet — only from EKS workers)

Security Groups:
- ALB SG:         Inbound 443 from 0.0.0.0/0
- EKS Worker SG:  Inbound from ALB SG only
- RDS SG:         Inbound 5432 from EKS Worker SG only
```

**Encryption:**
- In transit: TLS 1.2+ enforced everywhere, internal mTLS between services
- At rest: RDS encryption (AES-256), S3 SSE, EBS encryption
- Secrets: AWS Secrets Manager, never in environment variables or code

**Kubernetes security:**
```yaml
# Non-root, read-only filesystem, dropped capabilities
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
```

**Compliance controls:**
- CloudTrail: All API calls logged
- VPC Flow Logs: All network traffic logged
- Config Rules: Detect drift from security baseline
- GuardDuty: Threat detection

---

### Q43 — How do you handle data consistency in distributed systems?

> 🧠 **Concept Explanation**

**The problem:** With 10 microservices, a "place order" request touches multiple services. If payment succeeds but inventory reservation fails — you have an inconsistent state.

**Saga Pattern (what I'd recommend for Worldpay's BCS):**

```
Orchestrator (BCS) → calls each step → handles failures with compensating transactions

Onboarding Saga:
1. Create merchant profile → success
2. Provision API credentials → success
3. Configure payment gateway → FAILURE
   → Compensating: Delete API credentials
   → Compensating: Delete merchant profile
   → Notify: Onboarding failed
```

```python
# Saga orchestrator pseudocode
class OnboardingSaga:
    def execute(self, merchant_data):
        completed_steps = []
        try:
            self.create_profile(merchant_data)
            completed_steps.append('profile')

            self.provision_credentials(merchant_data)
            completed_steps.append('credentials')

            self.configure_gateway(merchant_data)
            completed_steps.append('gateway')

        except Exception as e:
            self.compensate(completed_steps, merchant_data)
            raise

    def compensate(self, steps, data):
        for step in reversed(steps):
            compensating_actions[step](data)
```

**This is directly relevant to Worldpay's Boarding Choreography Service (BCS) goal of automating end-to-end merchant onboarding with error tolerance.**

---

## 🔐 Security & Reliability

### Q44 — What is least privilege access in IAM?

> 🧠 **Concept Explanation**

**Definition:** Grant only the minimum permissions required to perform a specific task — nothing more.

```json
// WRONG: Admin access for a service that only reads S3
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}

// CORRECT: Only what's needed
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:ListBucket"],
  "Resource": [
    "arn:aws:s3:::boarding-docs",
    "arn:aws:s3:::boarding-docs/*"
  ]
}
```

**How I enforce it:**
1. Start with zero permissions
2. Add only what the service explicitly needs — verify by testing
3. Review with `aws iam simulate-principal-policy` before applying
4. Audit quarterly with AWS Access Analyzer (flags unused permissions)

```bash
# Check what permissions are actually being used
aws iam generate-service-last-accessed-details --arn arn:aws:iam::123:role/boarding-service
aws iam get-service-last-accessed-details --job-id <job-id>
# Remove permissions not used in last 90 days
```

---

### Q45 — What is blue-green deployment vs canary?

> ⚖️ **Compare & Justify**

| Feature | Blue-Green | Canary |
|---|---|---|
| How it works | Two full environments, flip traffic 0% → 100% | Gradually shift traffic (5% → 25% → 100%) |
| Rollback speed | Instant (flip DNS back) | Fast (reduce canary weight to 0%) |
| Cost | 2x infrastructure during transition | Minimal extra |
| Risk | All-or-nothing on cutover | Risk is proportional to canary % |
| Best for | High-confidence releases, scheduled maintenance | Validating new features in production gradually |
| My use | Major version upgrades | Feature flag rollouts, algorithm changes |

**When I'd use each at Worldpay:**
- **Blue-Green:** Migrating BCS to a new version with database schema changes
- **Canary:** Rolling out a new onboarding workflow to 5% of UK merchants before global rollout

---

### Q46 — How do you ensure disaster recovery?

> 🏗️ **Top-Down Design**

**RTO (Recovery Time Objective):** How long can we be down?
**RPO (Recovery Point Objective):** How much data can we lose?

**DR strategy by tier:**

| Tier | RTO | RPO | Strategy |
|---|---|---|---|
| Critical (payment processing) | < 15 min | < 1 min | Multi-AZ + Aurora Global DB |
| Important (merchant onboarding) | < 1 hour | < 5 min | Cross-region replica + S3 backup |
| Non-critical (reporting) | < 4 hours | < 1 hour | Daily backup + restore |

**Implementation I'd apply:**
```hcl
# Aurora Global Database — primary + DR region
resource "aws_rds_global_cluster" "boarding" {
  global_cluster_identifier = "boarding-global"
  engine                    = "aurora-postgresql"
  engine_version            = "15.4"
}

# Primary cluster
resource "aws_rds_cluster" "primary" {
  cluster_identifier        = "boarding-primary"
  global_cluster_identifier = aws_rds_global_cluster.boarding.id
  engine                    = "aurora-postgresql"
  # ... in ap-south-1
}

# DR cluster
resource "aws_rds_cluster" "dr" {
  provider                  = aws.london
  cluster_identifier        = "boarding-dr"
  global_cluster_identifier = aws_rds_global_cluster.boarding.id
  engine                    = "aurora-postgresql"
  # ... in eu-west-1
}
```

**DR runbook (tested quarterly):**
1. Promote DR region Aurora cluster to primary
2. Update Route53 to point to DR ALB
3. Scale up EKS nodes in DR region
4. Verify health checks pass
5. Communicate to stakeholders

---

## 🤝 Behavioral Questions

### Q47 — Tell me about a time you solved a complex problem.

> ⭐ **STAR Method**

**Situation:** At Hisan Labs, our EKS cluster was experiencing intermittent pod scheduling failures. Some services would stay in `Pending` state for 5–10 minutes before scheduling, causing deployment delays and alerting fatigue.

**Task:** Identify and fix the scheduling bottleneck without disrupting running services.

**Action:**
```bash
# Investigate pending pods
kubectl describe pod <pending-pod> | grep Events -A 20
# Event: "0/3 nodes available: 3 Insufficient cpu"

# Check node utilization
kubectl top nodes
# All nodes at 85%+ CPU request saturation

# Root cause: CPU requests were over-provisioned
# Services requesting 500m CPU but only using 50m

kubectl top pods -n production --sort-by=cpu
# Confirmed: Most services using <10% of their requested CPU

# Fix: Implement VPA recommendations + right-size requests
kubectl apply -f vpa.yaml
# After 24h of VPA observation, applied recommended requests
```

**Result:** Pod scheduling failures dropped to zero. Node CPU request utilization went from 85% → 45%, giving 40% more headroom before scaling is needed. Saved ~$200/month in unnecessary node capacity.

---

### Q48 — Describe a situation where you improved a process.

> ⭐ **STAR Method**

**Situation:** Every time a developer merged to main, a senior engineer had to manually SSH into the server, pull the latest image, and restart the service. This was a single point of failure and took 20–30 minutes per deployment.

**Task:** Automate the full deployment process and eliminate manual intervention.

**Action:** Designed and implemented the Jenkins CI/CD pipeline (detailed in Q1). Added:
- Automated Kubernetes rollouts triggered on merge to main
- Slack notification on success/failure
- Deployment record in our tracking sheet via Jenkins post-build script
- Rollback command documented in the runbook

**Result:** Deployments became self-service. Any developer could merge and watch their changes go live in 10 minutes. Senior engineer freed from deployment babysitting. Release frequency went from 2x/week → daily.

---

### Q49 — How do you handle production incidents?

> 🚨 **Action Plan Method**

**My incident response framework:**

```
1. DETECT    → Alert fires (Prometheus/Datadog) or user reports
2. ASSESS    → Severity? Scope? How many users affected?
3. MITIGATE  → Fastest path to restore service (rollback > fix forward)
4. DIAGNOSE  → Root cause analysis with logs + metrics
5. FIX       → Permanent fix deployed + tested
6. REVIEW    → Post-mortem: what happened, why, how to prevent
```

**Communication during incident:**
```
Every 15 minutes until resolved:
"[Update] boarding-gateway: Rollback complete. Error rate back to 0.1%.
Root cause investigation ongoing. ETA for RCA: 2 hours."
```

**Blameless post-mortem:** Focus on the system failure, not who made the change. The goal is: what process/tooling change prevents this from happening again?

---

### Q50 — How do you collaborate with cross-functional teams?

> 👥 **Ownership Method**

**At Hisan Labs:** I was the DevOps engineer working with a 4-person backend team and 2 frontend engineers. My approach:

1. **Shared runbooks:** I documented every deployment, rollback, and debug procedure so developers could self-serve without involving me for every issue
2. **Infrastructure as code in the same repo:** Developers could see and comment on Terraform PRs — infra changes weren't a black box
3. **Observability for all:** Set up Grafana dashboards accessible to everyone, not just ops. Developers could see their service's metrics directly
4. **Async-first:** Used GitHub Issues for infra requests with SLA commitments (P1 same-day, P2 two-day) rather than ad-hoc Slack pings

**At Worldpay:** The S&B team description mentions cross-functional collaboration with Sales channels and third-party services integrating into BCS. I'd apply the same approach — shared visibility, documented APIs, and clear ownership boundaries.

---

### Q51 — Tell me about a failure and what you learned.

> 🔁 **Reflection Method**

**Situation:** I pushed a Terraform `apply` to production that replaced an RDS security group instead of modifying it. The plan showed `-/+` (destroy and recreate) but I misread it as `~` (update in place). This caused 8 minutes of database downtime.

**Mistake:** I didn't treat `-/+` in Terraform plan as a hard stop. I also applied directly to production without a staging validation run.

**Learning:**
```bash
# Script I now run before every production apply
terraform show -json tfplan | \
  jq '.resource_changes[] | select(.change.actions | contains(["delete"])) | .address'

# If output is non-empty: STOP, get human review
```

Also added a Jenkins stage that auto-rejects any Terraform plan containing destructive changes and requires a named engineer to override.

**Improvement:** Zero Terraform-related outages since implementing the destructive-change gate. The lesson: automate the checks you're most likely to skip when under time pressure.

---

### Q52 — How do you prioritize tasks under pressure?

> ⏱️ **Prioritization Method**

**My framework:**

| Priority | Criteria | Example |
|---|---|---|
| P1 | Production is down or degraded | Pod crash-loop in payment service |
| P2 | Blocking another team | CI pipeline broken, developers can't deploy |
| P3 | Scheduled work with deadline | Terraform module refactor for new region |
| P4 | Important but not urgent | Documentation, non-critical upgrades |

**Under pressure specifically:** I communicate constraints immediately — I don't silently queue work. If I'm handling a P1 while a P2 request comes in, I say: "I'm on a production incident, will pick this up in ~1 hour. Can you unblock yourself in the meantime with [workaround]?"

**Concrete example:** During one week at Hisan Labs, I had a production HPA misconfiguration causing scaling failures (P1), a developer blocked by a broken Docker registry credential (P2), and a sprint deadline for new EKS node group provisioning (P3). I fixed the HPA in 30 min, gave the developer the credential fix in 10 min (it was a simple `kubectl create secret` update), and deferred the sprint item by one day with a heads-up to the team lead. All three resolved without escalation.

---

## ⚡ Rapid-Fire Answers

### What is idempotency?
An operation is idempotent if executing it multiple times produces the same result as executing it once. Critical in distributed systems — if a network timeout causes a retry, a duplicate transaction shouldn't double-charge a customer. Implemented via unique idempotency keys stored in Redis or a DB.

---

### What is Infrastructure as Code?
Managing infrastructure (servers, networks, databases) through code rather than manual clicks. The code is version-controlled, peer-reviewed, and repeatable. Tools: Terraform, Ansible, CloudFormation.

---

### What is immutable infrastructure?
Instead of updating servers in place (patching, config changes), you replace them entirely with new instances running the updated image. Docker containers are immutable by design — you don't `apt-get upgrade` inside a running container, you build a new image and redeploy.

---

### What is a sidecar container?
A secondary container in the same Kubernetes pod as the main application container, sharing its network and storage. Common uses:
- **Log shipping:** Fluentd sidecar that reads app logs and ships to Elasticsearch
- **Service mesh proxy:** Envoy sidecar for mTLS and traffic management (Istio)
- **Secret injector:** Vault agent sidecar that writes secrets to a shared volume

```yaml
containers:
- name: boarding-service    # Main app
  image: boarding-service:v1
- name: log-shipper         # Sidecar
  image: fluent/fluentd:v1
  volumeMounts:
  - name: app-logs
    mountPath: /var/log/app
```

---

### Difference: Docker vs VM?

| | Docker Container | Virtual Machine |
|---|---|---|
| Isolation | Process-level (shared OS kernel) | Full OS isolation |
| Startup | Milliseconds | Minutes |
| Size | MB | GB |
| Overhead | Minimal | Full hypervisor + guest OS |
| Security | Shared kernel (less isolated) | Stronger isolation |
| Best for | Microservices, CI/CD | Legacy apps, strong isolation needs |

---

### What is autoscaling?
Automatically adjusting compute resources based on demand.

- **HPA (Horizontal Pod Autoscaler):** Adds/removes pods based on CPU/memory metrics
- **VPA (Vertical Pod Autoscaler):** Adjusts CPU/memory requests per pod
- **Cluster Autoscaler:** Adds/removes EC2 nodes when pods can't be scheduled

---

### What is load balancing?
Distributing incoming traffic across multiple servers to avoid overloading any single one.

- **Layer 4 (NLB):** Routes by IP/TCP — ultra-low latency, no HTTP awareness
- **Layer 7 (ALB):** Routes by URL path, headers, host — content-aware routing

```
ALB rule: /boarding/* → boarding-service pods
ALB rule: /payments/* → payment-service pods
```

---

## 🎯 Smart Questions to Ask Them

> These signal that you've read the JD and fact sheet carefully, not just preparing generic questions.

**1. About the engineering specifics:**
> "The JD mentions the Boarding Choreography Service (BCS) as automating end-to-end merchant onboarding. Is BCS currently an orchestrator-based saga pattern, or is it event-driven choreography? I'm curious about the failure-compensation design at that scale."

**2. About current infrastructure maturity:**
> "Where is the S&B team today on the CI/CD maturity scale — are you closer to fully automated GitOps, or is there still manual approval in most production deployment paths?"

**3. About production incidents:**
> "How does the team currently handle on-call rotations, and what's your P1 incident SLA target for the onboarding pipeline?"

**4. About the '10-min onboarding' goal:**
> "The JD mentions the '10-minute onboarding' target. What's the current average, and is the primary bottleneck on the infrastructure/pipeline side or on the integration-with-third-party-channels side?"

**5. About tooling direction:**
> "Are there plans to adopt GitOps tools like ArgoCD or Flux for production deployments, or is Jenkins the long-term CI/CD strategy for this team?"

**6. About growth:**
> "What does the DevOps engineer career path look like within the Platform Design & Engineering org — is the expectation to grow toward SRE, cloud architecture, or team lead?"

---

*Sanket Ajay Chopade | Worldpay AWS DevOps Engineer Interview | April 2026*