# 🚀 DevOps Interview Answers – CI/CD & Automation

---

## ❓ Q1: You mentioned reducing deployment time by 77%—how exactly did you achieve this?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

The improvement was achieved in a microservices-based application deployed on AWS EKS where deployments were previously semi-manual and slow.

#### 🚀 Immediate Action

I redesigned the CI/CD pipeline using Jenkins by automating build, test, security, and deployment stages.

#### 🔍 Investigation

Key bottlenecks identified:

* Manual Docker builds
* Sequential execution of stages
* No caching or parallelization
* Manual Kubernetes deployments

#### ⚙️ Implementation

* Created a multi-stage Jenkins pipeline
* Enabled parallel execution for test stages
* Automated Docker build & push to AWS ECR
* Used Kubernetes rolling deployments
* Integrated SonarQube and Trivy scans

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t app:${BUILD_ID} .'
        sh 'docker push <ecr-repo>'
      }
    }
    stage('Deploy') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
      }
    }
  }
}
```

#### 📢 Communication

Collaborated with developers to standardize pipeline triggers and deployment practices.

#### 🛡️ Prevention

* Added monitoring (Prometheus + CloudWatch)
* Implemented rollback strategies

#### 📊 Impact

* Deployment time reduced from 45 → 10 minutes (77%)
* Release frequency increased 3x
* Deployment failures reduced by 35%

---

## ❓ Q2: Walk me through your Jenkins pipeline end-to-end.

### 🧠 Question Type: Strength (Value Pitch Method)

### ✅ Answer

#### 💪 Strength

Experienced in building end-to-end CI/CD pipelines integrating Jenkins, AWS, Docker, and Kubernetes.

#### 🧩 Example Pipeline Flow

1. **Code Checkout**

```bash
git clone <repo-url>
```

2. **Build (Maven)**

```bash
mvn clean package
```

3. **Static Code Analysis**

```bash
mvn sonar:sonar
```

4. **Security Scan**

```bash
trivy image <image-name>
```

5. **Docker Build & Push**

```bash
docker build -t app:${BUILD_ID} .
docker push <aws_account>.dkr.ecr.<region>.amazonaws.com/app:${BUILD_ID}
```

6. **Deploy to EKS**

```bash
kubectl set image deployment/app app=app:${BUILD_ID}
```

7. **Validation**

```bash
kubectl rollout status deployment/app
```

#### 🎯 Impact

* Fully automated pipeline (commit → production)
* Zero manual intervention
* Supports rolling and blue-green deployments

---

## ❓ Q3: How do you handle failures in a CI/CD pipeline?

### 🧠 Question Type: Failure / Mistake (Reflection Method)

### ✅ Answer

#### 📍 Situation

Failures occurred due to misconfigured Kubernetes manifests and failing tests.

#### ❌ Mistake

Initially, lack of proper error handling delayed debugging.

#### 📘 Learning

Importance of fail-fast strategy, logging, and alerts.

#### 🔧 Improvement

**Fail Fast**

```groovy
options {
  failFast true
}
```

**Error Handling**

```groovy
try {
  sh 'mvn test'
} catch (Exception e) {
  error "Build failed"
}
```

**Retry Logic**

```groovy
retry(3) {
  sh 'mvn test'
}
```

**Logs**

```bash
kubectl logs <pod-name>
```

**Alerts**

* Slack notifications
* Email alerts

#### 📊 Outcome

* Faster issue detection
* MTTR reduced by 30%
* Improved pipeline stability

---

## ❓ Q4: What’s the difference between Jenkins and GitHub Actions in your experience?

### 🧠 Question Type: Why / Decision (Compare & Justify Method)

### ✅ Answer

#### 🔍 Problem

Selecting the right CI/CD tool for scalability and flexibility.

#### ⚖️ Comparison

| Feature     | Jenkins         | GitHub Actions   |
| ----------- | --------------- | ---------------- |
| Setup       | Self-hosted     | Fully managed    |
| Flexibility | Very high       | Moderate         |
| Maintenance | High            | Low              |
| Integration | Any SCM         | Best with GitHub |
| Plugins     | Large ecosystem | Limited          |

#### ✅ Decision

* Jenkins for complex pipelines
* GitHub Actions for lightweight automation

**GitHub Actions Example**

```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: mvn clean package
```

#### ⚠️ Trade-offs

* Jenkins: powerful but maintenance-heavy
* GitHub Actions: simple but less flexible

#### 🎯 Conclusion

For enterprise environments like Worldpay, Jenkins is preferred for advanced control.

---

## ❓ Q5: How do you ensure rollback in your pipeline?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Rollback ensures quick recovery from failed deployments with minimal downtime.

#### 🚀 Immediate Action

Implemented Kubernetes-native rollback and versioned deployments.

#### 🔍 Investigation

Failures caused by:

* Faulty builds
* Configuration issues
* Resource limits

#### ⚙️ Implementation

**Kubernetes Rollback**

```bash
kubectl rollout undo deployment/app
```

**Versioned Images**

```bash
docker tag app:latest app:v1.2.3
```

**Health Checks**

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
```

**Helm Rollback**

```bash
helm rollback my-release 1
```

#### 📢 Communication

Notify stakeholders immediately during rollback.

#### 🛡️ Prevention

* Canary deployments
* Pre-deployment testing
* Monitoring alerts

#### 📊 Impact

* Zero downtime deployments
* Recovery within minutes
* Increased reliability

---

## ❓ Q6: Explain your EKS architecture in your project.

### 🧠 Question Type: Strength (Value Pitch Method)

### ✅ Answer

#### 💪 Strength

Hands-on experience designing production-grade AWS EKS architecture for microservices.

#### 🧩 Architecture Overview

* **VPC** with public & private subnets
* **EKS Cluster** in private subnets
* **Worker Nodes (EC2)** managed via Auto Scaling
* **ALB Ingress Controller** for external traffic
* **IAM Roles for Service Accounts (IRSA)**
* **RDS** for databases
* **ECR** for Docker images

#### 🔄 Flow

1. User → ALB
2. ALB → Ingress
3. Ingress → Kubernetes Service
4. Service → Pods

```bash
kubectl get nodes
kubectl get svc
```

#### 🎯 Impact

* Highly available and scalable architecture
* Secure networking with private subnets

---

## ❓ Q7: How do rolling updates and blue-green deployments differ?

### 🧠 Question Type: Why / Decision (Compare & Justify Method)

### ✅ Answer

#### 🔍 Problem

Need safe deployment strategies with minimal downtime.

#### ⚖️ Comparison

| Feature    | Rolling Update | Blue-Green             |
| ---------- | -------------- | ---------------------- |
| Deployment | Gradual        | Instant switch         |
| Downtime   | None           | None                   |
| Risk       | Moderate       | Low                    |
| Cost       | Low            | Higher (duplicate env) |

#### ⚙️ Rolling Update

```bash
kubectl set image deployment/app app=app:v2
```

#### ⚙️ Blue-Green

* Two environments: blue (old), green (new)
* Switch traffic via service selector

#### 🎯 Conclusion

* Rolling: efficient & default
* Blue-Green: safer for critical releases

---

## ❓ Q8: How did you achieve zero downtime deployments?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Goal was uninterrupted service during deployments on EKS.

#### 🚀 Immediate Action

Used Kubernetes-native deployment strategies.

#### ⚙️ Implementation

**Rolling Updates**

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

**Readiness Probe**

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
```

**HPA Enabled**

```bash
kubectl autoscale deployment app --cpu-percent=50 --min=2 --max=10
```

#### 📊 Impact

* Zero downtime deployments
* Seamless user experience

---

## ❓ Q9: What happens when a pod crashes?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Kubernetes ensures self-healing when pods fail.

#### 🔄 Process

1. Pod crashes
2. Kubelet detects failure
3. Pod marked unhealthy
4. ReplicaSet creates new pod

```bash
kubectl get pods
kubectl describe pod <pod-name>
```

#### 🔍 Investigation

* Check logs

```bash
kubectl logs <pod-name>
```

#### 🛡️ Prevention

* Liveness & readiness probes
* Resource limits

#### 🎯 Outcome

* Automatic recovery
* High availability maintained

---

## ❓ Q10: How does HPA work internally?

### 🧠 Question Type: Learning / Concept (Learning Framework)

### ✅ Answer

#### 📘 What

HPA (Horizontal Pod Autoscaler) automatically scales pods based on metrics.

#### ❓ Why

To handle varying load efficiently without manual scaling.

#### ⚙️ How

1. Metrics Server collects CPU/memory metrics
2. HPA controller checks metrics
3. Compares with target threshold
4. Adjusts replica count

```bash
kubectl get hpa
```

Formula:

Desired Replicas = Current Replicas × (Current Metric / Target Metric)

#### 🎯 Outcome

* Automatic scaling
* Cost optimization
* Better performance under load

---

## ❓ Q11: Explain how your Terraform code is structured (modules, state, etc.)

### 🧠 Question Type: Strength (Value Pitch Method)

### ✅ Answer

#### 💪 Strength

Designed modular, reusable Terraform architecture for AWS (EKS, VPC, RDS, IAM) with clear separation of concerns.

#### 🧩 Structure

```
terraform/
  ├── modules/
  │   ├── vpc/
  │   ├── eks/
  │   ├── rds/
  │   └── iam/
  ├── envs/
  │   ├── dev/
  │   ├── staging/
  │   └── prod/
  ├── main.tf
  ├── variables.tf
  ├── outputs.tf
  └── backend.tf
```

* **Modules**: Reusable building blocks (VPC, EKS, RDS)
* **Environments**: Separate configs per env (dev/stage/prod)
* **State**: Managed remotely for consistency

#### ⚙️ Example

```hcl
module "vpc" {
  source = "../modules/vpc"
  cidr_block = "10.0.0.0/16"
}
```

#### 🎯 Impact

* Reusable & scalable infra
* Reduced duplication by ~40%

---

## ❓ Q12: How do you manage Terraform state remotely?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Remote state ensures collaboration, locking, and consistency.

#### 🚀 Implementation

Used **S3 + DynamoDB** backend:

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "eks/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

#### 🔐 Features

* **S3** → stores state
* **DynamoDB** → state locking
* **Encryption** enabled

#### 📊 Impact

* Prevented concurrent changes
* Improved team collaboration

---

## ❓ Q13: What happens if Terraform apply fails midway?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Terraform maintains state consistency even if apply fails.

#### 🔄 Behavior

* Resources already created remain
* State file partially updated
* Terraform stops execution

#### 🔍 Investigation

```bash
terraform state list
terraform plan
```

#### 🚀 Recovery Steps

1. Fix the issue (config/permission/network)
2. Re-run:

```bash
terraform apply
```

3. If needed, manually fix state:

```bash
terraform state rm <resource>
```

#### 🛡️ Prevention

* Use `terraform plan` before apply
* Use smaller modular applies
* Enable proper IAM permissions

#### 🎯 Outcome

* Safe recovery without full rollback
* Minimal infra drift

---

## ❓ Q14: How do you handle secrets in Terraform?

### 🧠 Question Type: Security (Best Practice Method)

### ✅ Answer

#### 🔍 Problem

Avoid exposing sensitive data like passwords, keys, tokens.

#### 🔐 Best Practices

1. **Use AWS Secrets Manager / SSM Parameter Store**

```hcl
data "aws_ssm_parameter" "db_password" {
  name = "/prod/db/password"
}
```

2. **Environment Variables**

```bash
export TF_VAR_db_password="secure_value"
```

3. **Terraform Variables (Sensitive)**

```hcl
variable "db_password" {
  sensitive = true
}
```

4. **Do NOT hardcode secrets**

5. **Encrypt State (S3 backend)**

#### 🛡️ Additional Security

* IAM role-based access
* Restrict state file access

#### 🎯 Outcome

* Secure secret handling
* Compliance with DevSecOps practices

---

## ❓ Q15: Explain your VPC architecture.

### 🧠 Question Type: Strength (Value Pitch Method)

### ✅ Answer

#### 💪 Strength

Designed a secure, highly available VPC for EKS-based microservices with proper subnetting, routing, and isolation.

#### 🧩 Architecture

* **CIDR**: 10.0.0.0/16
* **Public Subnets (AZ-a, AZ-b)**: ALB, NAT Gateways
* **Private Subnets (AZ-a, AZ-b)**: EKS worker nodes, RDS
* **Internet Gateway (IGW)** attached to VPC
* **NAT Gateway** in each public subnet for egress from private subnets
* **Route Tables**:

  * Public RT → `0.0.0.0/0` via IGW
  * Private RT → `0.0.0.0/0` via NAT GW
* **Security**:

  * Security Groups (least privilege)
  * NACLs (stateless layer)

#### ⚙️ Example (Terraform)

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id
}
```

#### 🎯 Impact

* Multi-AZ high availability
* Strong network isolation for workloads

---

## ❓ Q16: Difference between ALB and NLB?

### 🧠 Question Type: Why / Decision (Compare & Justify Method)

### ✅ Answer

#### ⚖️ Comparison

| Feature         | ALB (Application LB)    | NLB (Network LB)              |
| --------------- | ----------------------- | ----------------------------- |
| Layer           | L7 (HTTP/HTTPS)         | L4 (TCP/UDP)                  |
| Routing         | Path/Host-based         | Port-based                    |
| Use Case        | Web apps, microservices | High-performance, low latency |
| TLS Termination | Yes                     | Yes (limited features)        |
| Sticky Sessions | Supported               | Limited                       |

#### 🧠 Decision

* **ALB** for Kubernetes Ingress (HTTP routing)
* **NLB** for ultra-low latency or non-HTTP protocols

#### 🎯 Conclusion

In my project, I used **ALB with Ingress Controller** for routing traffic to EKS services.

---

## ❓ Q17: How does IAM role-based access work?

### 🧠 Question Type: Concept + Practical (Learning Framework)

### ✅ Answer

#### 📘 What

IAM roles provide temporary permissions to AWS services/users without sharing credentials.

#### ⚙️ How It Works

1. Create IAM Role with policies
2. Attach role to service (EC2/EKS/Lambda)
3. AWS provides temporary credentials via STS

#### 🔐 Example (EKS IRSA)

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::123456789012:role/app-role
```

#### 🎯 Benefits

* No hardcoded credentials
* Fine-grained access control
* Improved security posture

---

## ❓ Q18: How do services inside private subnets access the internet?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Resources in private subnets do not have direct internet access.

#### 🚀 Solution

Use **NAT Gateway** in public subnet.

#### 🔄 Flow

1. Private instance sends request
2. Routed to NAT Gateway
3. NAT forwards via Internet Gateway
4. Response returns via NAT

#### ⚙️ Example Route

```hcl
route {
  cidr_block = "0.0.0.0/0"
  nat_gateway_id = aws_nat_gateway.main.id
}
```

#### 🎯 Outcome

* Secure outbound internet access
* No inbound exposure

---

## ❓ Q19: Explain Route53 routing policies.

### 🧠 Question Type: Concept (Learning Framework)

### ✅ Answer

#### 📘 What

Route53 routing policies define how DNS queries are answered.

#### 🧩 Types

1. **Simple Routing**

* Single resource

2. **Weighted Routing**

* Traffic split (e.g., 70/30)

3. **Latency-Based Routing**

* Route to lowest latency region

4. **Failover Routing**

* Primary + standby (health checks)

5. **Geolocation Routing**

* Based on user location

#### ⚙️ Example (Weighted)

```hcl
resource "aws_route53_record" "app" {
  zone_id = "Z123"
  name    = "app.example.com"
  type    = "A"
  set_identifier = "blue"
  weight  = 70
}
```

#### 🎯 Use Case

* Blue-green deployments
* Multi-region HA

#### 🎯 Outcome

* Improved availability
* Intelligent traffic routing

---

## ❓ Q20: Pipeline is failing intermittently—how do you debug?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Intermittent failures indicate non-deterministic issues like network, resource contention, or flaky tests.

#### 🚀 Immediate Action

* Check latest failed build logs
* Identify failing stage

```bash
# Jenkins logs (example)
cat /var/log/jenkins/jenkins.log
```

#### 🔍 Investigation

* Check for flaky tests
* Verify external dependencies (APIs, DB)
* Check resource utilization

```bash
top
kubectl top pods
```

* Check network/DNS issues

#### 🧪 Isolation

* Re-run pipeline with same commit
* Run failing stage independently

#### 📢 Communication

Inform team if issue impacts releases

#### 🛡️ Prevention

* Add retries for flaky steps

```groovy
retry(2) {
  sh 'mvn test'
}
```

* Stabilize test cases
* Add better logging

#### 📊 Outcome

* Reduced flaky failures
* Improved pipeline reliability

---

## ❓ Q21: Deployment succeeded but app is not working—what will you check?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Deployment success ≠ application health. Need to validate runtime.

#### 🚀 Immediate Action

* Check pod status

```bash
kubectl get pods
```

#### 🔍 Investigation

1. **Pod Logs**

```bash
kubectl logs <pod-name>
```

2. **Pod Events**

```bash
kubectl describe pod <pod-name>
```

3. **Service & Endpoints**

```bash
kubectl get svc
kubectl get endpoints
```

4. **Ingress / ALB**

* Check routing rules

5. **Health Checks**

* Readiness/Liveness probes

6. **Environment Variables / Secrets**

* Misconfiguration

#### 🧪 Validation

* Curl inside cluster

```bash
kubectl exec -it <pod> -- curl localhost:8080
```

#### 📢 Communication

Update stakeholders about impact

#### 🛡️ Prevention

* Add proper readiness probes
* Add smoke tests post-deployment

#### 📊 Outcome

* Faster root cause identification
* Reduced downtime

---

## ❓ Q22: Build time suddenly increased—what could be the reasons?

### 🧠 Question Type: Troubleshooting (Root Cause Method)

### ✅ Answer

#### 🔍 Possible Causes

1. **Dependency Issues**

* New libraries added
* No caching

2. **Infrastructure Issues**

* Low CPU/memory on build agents

```bash
free -m
```

3. **Network Latency**

* Slow artifact downloads

4. **Pipeline Changes**

* Added new stages (security scans, tests)

5. **Docker Build Inefficiency**

* No layer caching

6. **Test Suite Growth**

* More/slow tests

#### 🚀 Debug Steps

* Compare previous vs current builds
* Analyze stage timing in Jenkins
* Check agent performance

#### ⚙️ Optimization

**Enable Maven Cache**

```bash
mvn dependency:go-offline
```

**Docker Layer Caching**

```dockerfile
COPY pom.xml .
RUN mvn dependency:resolve
```

**Parallel Execution**

```groovy
parallel {
  stage('Test1') { steps { sh 'mvn test' } }
  stage('Test2') { steps { sh 'mvn test' } }
}
```

#### 📊 Outcome

* Reduced build time
* Improved pipeline efficiency

---

## ❓ Q23: Pods are restarting continuously—how do you troubleshoot?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Continuous restarts usually indicate **CrashLoopBackOff** or failing probes.

#### 🚀 Immediate Action

```bash
kubectl get pods
kubectl describe pod <pod-name>
```

#### 🔍 Investigation

1. **Check Restart Reason**

```bash
kubectl get pod <pod-name> -o wide
```

2. **Logs (current + previous)**

```bash
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
```

3. **Events**

```bash
kubectl describe pod <pod-name>
```

4. **Common Causes**

* App crash (exceptions)
* Wrong env/config/secret
* Failing **liveness/readiness** probes
* OOMKilled (memory limits)
* Image issues

5. **Resource Checks**

```bash
kubectl top pod <pod-name>
```

#### 🧪 Isolation

* Run container locally / with debug flags
* Temporarily relax probes to validate startup

#### 🛡️ Fixes

* Correct configs/secrets
* Tune resources (requests/limits)
* Adjust probes (initialDelaySeconds, timeoutSeconds)

#### 📊 Outcome

* Restored pod stability
* Reduced restart loops

---

## ❓ Q24: Service is not accessible externally—debug steps?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

External access involves **Ingress/ALB → Service → Pods** chain.

#### 🚀 Immediate Action

```bash
kubectl get svc
kubectl get ingress
```

#### 🔍 Investigation

1. **Service Type & Ports**

```bash
kubectl describe svc <service-name>
```

* Ensure correct **type** (LoadBalancer/NodePort)
* Port ↔ targetPort mapping

2. **Endpoints**

```bash
kubectl get endpoints <service-name>
```

* Should list pod IPs (else selector mismatch)

3. **Pod Health**

```bash
kubectl get pods
kubectl logs <pod-name>
```

4. **Ingress / ALB**

* Rules, host/path mapping
* Target group health

5. **Security Groups / NACLs**

* Allow inbound from internet to ALB
* Allow ALB → node/pod CIDR

6. **DNS (Route53)**

* Record points to ALB DNS

#### 🧪 Validation

```bash
kubectl exec -it <pod> -- curl localhost:<port>
```

* If works internally → issue is at ingress/LB layer

#### 🛡️ Fixes

* Correct selectors/ports
* Fix ingress annotations/rules
* Open SG ports

#### 📊 Outcome

* Restored external connectivity
* Faster RCA for networking issues

---

## ❓ Q25: High latency in your application—how do you investigate?

### 🧠 Question Type: Troubleshooting (Root Cause Method)

### ✅ Answer

#### 🔹 Clarify

Latency can originate from **app, network, or infrastructure** layers.

#### 🚀 Immediate Action

* Check dashboards (Prometheus/Grafana/CloudWatch)
* Identify affected service and timeframe

#### 🔍 Investigation

1. **Application Level**

* Slow endpoints, thread pools, DB queries
* Check logs/APM traces

2. **Pod/Node Resources**

```bash
kubectl top pods
kubectl top nodes
```

* CPU throttling, memory pressure

3. **Autoscaling**

```bash
kubectl get hpa
```

* Are replicas scaling as expected?

4. **Network**

* Ingress/ALB latency, retries, timeouts
* Cross-AZ traffic

5. **Database/Dependencies**

* RDS latency, connection pool saturation

6. **Deployment Changes**

* Recent releases causing regression

#### 🧪 Validation

* Load test specific endpoint
* Compare before/after metrics

#### 🛠️ Mitigation

* Scale pods (HPA/manual)

```bash
kubectl scale deployment app --replicas=5
```

* Optimize queries/caching
* Tune resources (requests/limits)

#### 🛡️ Prevention

* SLOs + alerts (p95/p99 latency)
* Canary deployments
* Capacity planning

#### 📊 Outcome

* Reduced latency
* Improved user experience and stability

---

## ❓ Q26: EC2 instance is unreachable—what will you check?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

“Unreachable” can mean SSH not working, app port not reachable, or instance not passing health checks.

#### 🚀 Immediate Action

* Check instance status (running/2 status checks)
* Verify public IP / DNS

```bash
aws ec2 describe-instance-status --instance-ids <id>
```

#### 🔍 Investigation

1. **Security Groups (stateful)**

* Inbound allow SSH (22) / app ports (80/443)

2. **NACLs (stateless)**

* Allow inbound/outbound ephemeral ports

3. **Route Table / IGW**

* Public subnet has route `0.0.0.0/0 → IGW`

4. **Public vs Private Subnet**

* If private → need Bastion or SSM Session Manager

5. **OS / SSH Service**

* Instance boot issues or sshd down

6. **Disk / CPU Issues**

* High CPU or full disk can block access

#### 🧪 Debug Options

* **SSM Session Manager** (no SSH needed)

```bash
aws ssm start-session --target <instance-id>
```

* **EC2 Serial Console / Instance Screenshot**

#### 🛠️ Fixes

* Update SG/NACL rules
* Restart sshd or instance
* Attach IAM role for SSM if missing

#### 📊 Outcome

* Restored connectivity
* Identified network vs OS-level root cause

---

## ❓ Q27: Application is down in one AZ—how do you handle it?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Partial outage limited to a single Availability Zone indicates AZ-specific failure or imbalance.

#### 🚀 Immediate Action

* Check health across AZs (ALB target groups / EKS nodes)
* Shift traffic away from unhealthy AZ if needed

#### 🔍 Investigation

1. **Load Balancer Health**

* Target group health per AZ

2. **EKS / EC2 Capacity**

```bash
kubectl get nodes -o wide
kubectl get pods -o wide
```

* Are pods/nodes in that AZ unhealthy?

3. **Auto Scaling Groups**

* Capacity reduced or instance failures in that AZ

4. **Dependencies**

* RDS/Aurora AZ health, subnet issues

5. **Networking**

* Subnet route tables, NACLs, SG differences

#### 🧪 Mitigation

* Scale up in healthy AZs

```bash
kubectl scale deployment app --replicas=6
```

* Temporarily disable AZ in LB (if severe)

#### 🛡️ Prevention

* Multi-AZ deployments (EKS node groups across AZs)
* Health checks + auto-replacement
* Route53 failover/latency routing

#### 📊 Outcome

* Traffic served from healthy AZs
* Maintained availability during partial outage

---

## ❓ Q28: Sudden spike in AWS cost—how do you analyze?

### 🧠 Question Type: Troubleshooting (Root Cause Method)

### ✅ Answer

#### 🔹 Clarify

Cost spikes typically come from compute, data transfer, storage, or misconfigured scaling.

#### 🚀 Immediate Action

* Open **Cost Explorer** and filter by service/time
* Identify top contributors (EC2, EKS, RDS, S3, Data Transfer)

#### 🔍 Investigation

1. **EC2 / EKS**

* Unexpected scale-out
* Large instance types

2. **Data Transfer**

* Cross-AZ or internet egress

3. **EBS / Snapshots**

* Unused volumes or frequent snapshots

4. **RDS**

* High IOPS / storage autoscaling

5. **S3**

* Storage class misuse / high requests

6. **Recent Changes**

* New deployments, HPA thresholds, batch jobs

#### 🧪 Validation

* Tag-based cost breakdown
* Compare with previous period

#### 🛠️ Optimization

* Rightsize instances
* Set autoscaling limits
* Use Savings Plans / Reserved Instances
* Cleanup unused resources

#### 🛡️ Prevention

* Budgets & alerts
* Mandatory tagging
* Cost anomaly detection

#### 📊 Outcome

* Identified cost drivers
* Reduced unnecessary spend

---

## ❓ Q29: What are multi-stage builds? Why did you use them?

### 🧠 Question Type: Concept + Practical (Learning Framework)

### ✅ Answer

#### 📘 What

Multi-stage builds allow using multiple `FROM` statements in a single Dockerfile to separate **build** and **runtime** environments.

#### ❓ Why

* Remove build-time dependencies (Maven, Node, compilers)
* Produce smaller, secure runtime images

#### ⚙️ How (Example)

```dockerfile
# Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn -q -e -DskipTests dependency:go-offline
COPY src ./src
RUN mvn -q -DskipTests package

# Stage 2: Runtime
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
COPY --from=builder /app/target/app.jar ./app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
```

#### 🎯 Why I Used It (Project)

* Reduced image size by ~60%
* Faster pull times in EKS → quicker rollouts
* Lower attack surface (no build tools in final image)

---

## ❓ Q30: Difference between CMD and ENTRYPOINT?

### 🧠 Question Type: Why / Decision (Compare & Justify Method)

### ✅ Answer

#### ⚖️ Comparison

| Feature     | CMD                    | ENTRYPOINT                      |
| ----------- | ---------------------- | ------------------------------- |
| Purpose     | Default arguments      | Main executable                 |
| Overridable | Yes (`docker run ...`) | Partially (with `--entrypoint`) |
| Typical Use | Provide defaults       | Enforce app startup             |

#### ⚙️ Examples

**CMD (can be overridden)**

```dockerfile
FROM nginx:alpine
CMD ["nginx", "-g", "daemon off;"]
```

**ENTRYPOINT (fixed executable)**

```dockerfile
FROM alpine:3.19
ENTRYPOINT ["echo"]
CMD ["hello"]
```

Run behavior:

```bash
# Uses ENTRYPOINT + CMD
docker run image
# Overrides CMD only
docker run image world
# Overrides ENTRYPOINT
docker run --entrypoint /bin/sh image
```

#### 🧠 Best Practice

* Use **ENTRYPOINT** for the main process
* Use **CMD** for default args

#### 🎯 In My Project

Used ENTRYPOINT for the Java app and CMD for JVM flags when needed.

---

## ❓ Q31: How do you reduce Docker image size?

### 🧠 Question Type: Troubleshooting / Optimization (Root Cause Method)

### ✅ Answer

#### 🔍 Techniques

1. **Multi-stage builds** (biggest impact)
2. **Use slim/alpine base images**

```dockerfile
FROM node:20-alpine
```

3. **Leverage layer caching**

```dockerfile
COPY package.json package-lock.json ./
RUN npm ci --only=production
COPY . .
```

4. **.dockerignore**

```
.git
node_modules
*.log
```

5. **Remove unnecessary packages/files**

```dockerfile
RUN apt-get update && apt-get install -y curl \
 && rm -rf /var/lib/apt/lists/*
```

6. **Use distroless/minimal runtime images**

```dockerfile
FROM gcr.io/distroless/java17-debian12
```

7. **Minimize layers**

* Combine RUN commands

#### 🧪 Verify Size

```bash
docker images
docker history <image>
```

#### 🎯 Outcome

* Smaller images → faster CI/CD and deployments
* Reduced bandwidth and storage costs
* Improved security (fewer packages)

---

## ❓ Q32: How do you check memory/CPU usage in Linux?

### 🧠 Question Type: Practical (Hands-on Method)

### ✅ Answer

#### 🔍 CPU Usage

```bash
top
htop
mpstat -P ALL 1
```

* `top/htop` → real-time processes
* `mpstat` → per-core CPU stats

#### 🔍 Memory Usage

```bash
free -h
vmstat 1
```

* `free -h` → total/used/free memory
* `vmstat` → swap, IO, CPU context

#### 🔍 Process-level Analysis

```bash
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

#### 🔍 Disk + IO (often correlated)

```bash
iostat -xz 1
```

#### 🎯 In Practice

* During incidents, I correlate **high CPU/memory** with specific pods/services and validate via Kubernetes:

```bash
kubectl top pods
```

---

## ❓ Q33: What happens when disk is full?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

When disk reaches 100%, system operations fail (writes, logs, services).

#### 🚀 Immediate Impact

* Applications may crash
* Logs stop writing
* System may become unresponsive

#### 🔍 Investigation

```bash
df -h            # disk usage
du -sh /*        # directory usage
lsof +L1         # deleted files still open
```

#### 🛠️ Immediate Fix

* Clean logs/temp files

```bash
rm -rf /var/log/*.log
```

* Truncate large logs

```bash
: > /var/log/app.log
```

#### 🔎 Root Causes

* Log rotation not configured
* Large temp/cache files
* Docker images/containers buildup

```bash
docker system prune -a
```

#### 🛡️ Prevention

* Enable logrotate
* Disk monitoring + alerts
* Set retention policies

#### 📊 Outcome

* Restored disk availability
* Prevented future outages

---

## ❓ Q34: Explain systemd services.

### 🧠 Question Type: Concept + Practical (Learning Framework)

### ✅ Answer

#### 📘 What

`systemd` is the init system used in modern Linux (RHEL/Amazon Linux) to manage services, processes, and system startup.

#### ⚙️ Key Components

* **Unit files**: service definitions (`.service`)
* Located in:

```bash
/etc/systemd/system/
/lib/systemd/system/
```

#### ⚙️ Example Service File

```ini
[Unit]
Description=My App Service
After=network.target

[Service]
ExecStart=/usr/bin/java -jar /opt/app/app.jar
Restart=always
User=ec2-user

[Install]
WantedBy=multi-user.target
```

#### 🔧 Common Commands

```bash
systemctl start myapp
systemctl stop myapp
systemctl restart myapp
systemctl status myapp
systemctl enable myapp
systemctl daemon-reload
```

#### 🔍 Logs

```bash
journalctl -u myapp -f
```

#### 🎯 In Practice

* Used systemd to manage app services on EC2
* Ensured auto-restart and boot-time startup

#### 📊 Benefits

* Reliable service management
* Centralized logging
* Automatic recovery on failure

---

## ❓ Q35: Write a script to check service health.

### 🧠 Question Type: Practical (Hands-on Coding)

### ✅ Answer

#### 🐚 Bash Script (HTTP Health Check)

```bash
#!/usr/bin/env bash
set -euo pipefail

URL=${1:-"http://localhost:8080/health"}
TIMEOUT=${TIMEOUT:-5}

status_code=$(curl -s -o /dev/null -w "%{http_code}" --max-time "$TIMEOUT" "$URL" || true)

if [[ "$status_code" == "200" ]]; then
  echo "OK: Service healthy (HTTP $status_code)"
  exit 0
else
  echo "CRITICAL: Service unhealthy (HTTP $status_code)"
  exit 2
fi
```

#### 🐍 Python Script (with timeout & JSON validation)

```python
#!/usr/bin/env python3
import sys, json
import urllib.request

url = sys.argv[1] if len(sys.argv) > 1 else "http://localhost:8080/health"
req = urllib.request.Request(url)

try:
    with urllib.request.urlopen(req, timeout=5) as resp:
        code = resp.getcode()
        body = resp.read().decode("utf-8")
        # optional: validate JSON response
        try:
            data = json.loads(body)
            healthy = data.get("status", "").lower() in ("ok", "up", "healthy")
        except Exception:
            healthy = (200 <= code < 300)

        if code == 200 and healthy:
            print(f"OK: Service healthy (HTTP {code})")
            sys.exit(0)
        else:
            print(f"CRITICAL: Unhealthy (HTTP {code}) body={body[:200]}")
            sys.exit(2)
except Exception as e:
    print(f"CRITICAL: Request failed: {e}")
    sys.exit(2)
```

#### 🎯 In Practice

* Integrated as **smoke test** post-deployment in Jenkins
* Used for readiness checks and alerting

---

## ❓ Q36: How would you automate log cleanup?

### 🧠 Question Type: Scenario-Based (Action Plan Method)

### ✅ Answer

#### 🔹 Clarify

Goal is to prevent disk exhaustion by rotating and removing old logs safely.

#### 🚀 Approach 1: logrotate (preferred)

**Config** `/etc/logrotate.d/myapp`

```conf
/var/log/myapp/*.log {
  daily
  rotate 7
  compress
  missingok
  notifempty
  copytruncate
}
```

Run/verify:

```bash
logrotate -d /etc/logrotate.conf   # dry-run
logrotate -f /etc/logrotate.conf   # force
```

#### 🚀 Approach 2: Cron + find

```bash
#!/usr/bin/env bash
LOG_DIR="/var/log/myapp"
# delete logs older than 7 days
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -print -delete
```

Crontab:

```bash
0 2 * * * /usr/local/bin/cleanup_logs.sh
```

#### 🛡️ Safety

* Avoid deleting active logs (use `copytruncate` or app log rotation)
* Monitor disk usage

```bash
df -h
```

#### 🎯 Outcome

* Prevented disk-full incidents
* Predictable log retention

---

## ❓ Q37: Difference between Bash and Python for automation?

### 🧠 Question Type: Why / Decision (Compare & Justify Method)

### ✅ Answer

#### ⚖️ Comparison

| Aspect         | Bash                         | Python                               |
| -------------- | ---------------------------- | ------------------------------------ |
| Best For       | OS-level tasks, glue scripts | Complex logic, APIs, data processing |
| Readability    | Lower for large scripts      | High                                 |
| Libraries      | Limited                      | Rich ecosystem (requests, boto3)     |
| Performance    | Fast for shell ops           | Better for complex workflows         |
| Error Handling | Basic                        | Robust (exceptions)                  |

#### 🧠 Decision

* Use **Bash** for simple automation (file ops, cron jobs, quick checks)
* Use **Python** for complex workflows (API calls, AWS automation, parsing)

#### ⚙️ Example (AWS with Python)

```python
import boto3

ec2 = boto3.client('ec2')
resp = ec2.describe_instances()
print(len(resp.get('Reservations', [])))
```

#### 🎯 In My Practice

* Bash for CI/CD glue and maintenance scripts
* Python for scalable automation and integrations

---

# 🧩 DevOps System Design Answers (Beginner–Intermediate)

---

## ❓ Q38: Design a CI/CD pipeline for microservices architecture

### 🧠 Question Type: System Design (Structured Approach)

### ✅ Answer

#### 🔹 Requirements

* Multiple microservices
* Independent deployments
* Fast and reliable releases

#### 🏗️ High-Level Design

1. **Source Control**

* GitHub / GitLab (each service repo or mono-repo)

2. **CI Layer (Build & Test)**

* Jenkins / GitHub Actions

3. **Artifact Storage**

* Docker images stored in AWS ECR

4. **CD Layer (Deployment)**

* Kubernetes (EKS)

5. **Observability**

* Prometheus + Grafana + CloudWatch

#### 🔄 Pipeline Flow

1. Developer pushes code
2. CI triggers build + test
3. Docker image build + push
4. Deploy to EKS via kubectl/Helm
5. Run smoke tests

#### ⚙️ Example Commands

```bash
docker build -t app:v1 .
docker push <ecr-repo>
kubectl apply -f deployment.yaml
```

#### 🎯 Best Practices

* Separate pipelines per service
* Use Helm for deployments
* Add security scans (Trivy, SonarQube)

#### 📊 Outcome

* Faster releases
* Independent service scaling

---

## ❓ Q39: How would you design a highly available system on AWS?

### 🧠 Question Type: System Design

### ✅ Answer

#### 🔹 Principles

* Eliminate single point of failure
* Use multi-AZ architecture

#### 🏗️ Design

* **Route53** → DNS routing
* **ALB** → distribute traffic
* **EC2 / EKS** → across multiple AZs
* **Auto Scaling Groups**
* **RDS Multi-AZ**
* **S3** for static assets

#### 🔄 Flow

User → Route53 → ALB → EC2/EKS → RDS

#### 🛡️ Enhancements

* Health checks
* Auto recovery
* Backup & snapshots

#### 🎯 Outcome

* 99.9%+ availability

---

## ❓ Q40: How would you deploy applications across multiple regions?

### 🧠 Question Type: System Design

### ✅ Answer

#### 🔹 Goal

Disaster recovery + global low latency

#### 🏗️ Design

* Deploy app in multiple regions (e.g., ap-south-1, us-east-1)
* Use Route53 routing policies

#### 🔄 Routing Options

* Latency-based routing
* Failover routing
* Weighted routing

#### ⚙️ Data Strategy

* RDS cross-region replication
* S3 cross-region replication

#### 🎯 Outcome

* Reduced latency
* High fault tolerance

---

## ❓ Q41: How do you ensure zero downtime deployment?

### 🧠 Question Type: System Design

### ✅ Answer

#### 🔹 Techniques

1. **Rolling Updates (Kubernetes)**

```yaml
strategy:
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

2. **Blue-Green Deployment**

* Two environments
* Switch traffic instantly

3. **Canary Deployment**

* Gradual rollout

4. **Health Checks**

* Readiness probes

5. **Load Balancer Integration**

#### 🎯 Outcome

* No service interruption
* Safe deployments

---

## ❓ Q42: Tell me about a production incident you handled.

### 🧠 Question Type: Behavioral (STAR Method)

### ✅ Answer

#### 📍 Situation

During a release on EKS, users reported intermittent 5xx errors after deployment.

#### 🎯 Task

Quickly restore service and identify root cause with minimal impact.

#### ⚙️ Action

* Checked ALB target health and Kubernetes rollout status

```bash
kubectl rollout status deployment/app
```

* Inspected pod logs and events

```bash
kubectl logs <pod>
kubectl describe pod <pod>
```

* Found readiness probe misconfiguration causing pods to receive traffic too early
* Triggered rollback

```bash
kubectl rollout undo deployment/app
```

* Fixed probe config and re-deployed

#### 📊 Result

* Restored service within minutes
* Added stricter readiness checks → prevented recurrence
* Improved MTTR by ~30%

---

## ❓ Q43: Describe a time when your deployment failed.

### 🧠 Question Type: Behavioral (Failure / Reflection)

### ✅ Answer

#### 📍 Situation

A deployment failed due to a misconfigured environment variable for DB connection.

#### ❌ Mistake

Missed validation of environment variables before deployment.

#### ⚙️ Action

* Identified failure via logs

```bash
kubectl logs <pod>
```

* Corrected configuration in ConfigMap/Secrets
* Re-deployed using pipeline

#### 📘 Learning

* Added pre-deployment validation checks
* Introduced smoke tests post-deployment

#### 📊 Result

* Reduced similar failures
* Improved deployment reliability

---

## ❓ Q44: How do you handle on-call pressure?

### 🧠 Question Type: Behavioral (Ownership)

### ✅ Answer

#### 🧠 Approach

* Stay calm and follow structured debugging (no panic decisions)

#### ⚙️ Action

1. **Prioritize impact** (P1 vs P2)
2. **Quick triage** using dashboards/logs
3. **Communicate clearly** with stakeholders
4. **Mitigate first, fix later** (rollback/scale)

#### 🧰 Tools Used

* CloudWatch / Grafana alerts
* kubectl logs / metrics

#### 🎯 Example

Handled a late-night alert where CPU spiked → scaled pods temporarily and then optimized queries.

#### 📊 Result

* Reduced downtime
* Maintained composure under pressure

---

## ❓ Q45: How do you prioritize multiple issues?

### 🧠 Question Type: Behavioral (Decision Making)

### ✅ Answer

#### 🧠 Strategy

Use **impact vs urgency matrix**.

#### ⚙️ Steps

1. Identify severity:

   * P1: Production down
   * P2: Partial impact
   * P3: Minor

2. Act accordingly:

* Resolve P1 immediately
* Delegate/queue P2
* Schedule P3

3. Communicate priorities with team

#### 🎯 Example

Handled simultaneous issues (deployment bug + monitoring alert):

* Fixed production issue first
* Then investigated alert

#### 📊 Result

* Efficient incident handling
* Reduced chaos during outages

---

## ❓ Q46: Tell me about a conflict with a developer team.

### 🧠 Question Type: Behavioral (Collaboration)

### ✅ Answer

#### 📍 Situation

Developers wanted faster deployments without adding validation checks.

#### ⚙️ Action

* Explained risks of skipping validation (failed deployments, downtime)
* Proposed compromise:

  * Fast pipeline for dev
  * Strict checks for staging/production

#### 🤝 Collaboration

* Conducted discussion and aligned on best practices

#### 📊 Result

* Balanced speed + reliability
* Improved team trust and collaboration

#### 📘 Learning

* Communication is key in DevOps
* Always align with business impact, not just technical preference

---

## ❓ Q47: You improved deployment success rate—what challenges did you face?

### 🧠 Question Type: Behavioral + Technical (STAR Method)

### ✅ Answer

#### 📍 Situation

While improving CI/CD pipelines, deployments initially had frequent failures (~35%) due to inconsistent environments and manual steps.

#### 🎯 Task

Increase deployment success rate and reliability for Kubernetes-based deployments.

#### ⚙️ Action

* Standardized pipeline using Jenkins + GitHub workflows
* Introduced **automated validation stages** (build, test, security scan)
* Implemented **rolling + blue-green deployments**
* Added **readiness probes** to prevent premature traffic

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
```

* Improved logging and observability

#### ⚠️ Challenges Faced

* Environment drift between dev and prod
* Misconfigured Kubernetes manifests
* Lack of proper health checks

#### 📊 Result

* Deployment success rate improved to ~98%
* Reduced failures by ~35%
* More stable production releases

---

## ❓ Q48: How did you ensure 0 critical vulnerabilities?

### 🧠 Question Type: Security (DevSecOps Approach)

### ✅ Answer

#### 🔹 Approach

Integrated security directly into CI/CD pipeline (shift-left security).

#### ⚙️ Implementation

1. **Static Code Analysis (SonarQube)**

```bash
mvn sonar:sonar
```

2. **Container Image Scanning (Trivy)**

```bash
trivy image <image-name>
```

3. **Dependency Checks**

* Identified vulnerable libraries

4. **IAM Best Practices**

* Least privilege roles

5. **Regular Updates**

* Patched base images and dependencies

#### 🛡️ Controls

* Fail pipeline on critical vulnerabilities
* Enforced security gates before deployment

#### 📊 Result

* Achieved **0 critical vulnerabilities in production**
* Improved compliance and security posture

---

## ❓ Q49: What was your biggest DevOps mistake?

### 🧠 Question Type: Behavioral (Failure + Reflection)

### ✅ Answer

#### 📍 Situation

Early in my project, I deployed an application without proper readiness checks.

#### ❌ Mistake

Pods started receiving traffic before they were fully ready, causing temporary failures.

#### ⚙️ Action

* Investigated using:

```bash
kubectl describe pod <pod>
```

* Identified missing readiness probe
* Implemented proper health checks

#### 📘 Learning

* Never rely only on deployment success
* Always validate application readiness

#### 🛠️ Improvement

* Added readiness & liveness probes
* Added smoke tests in pipeline

#### 📊 Result

* Eliminated similar incidents
* Improved reliability of deployments

#### 🎯 Key Takeaway

Mistakes helped me build a stronger, production-ready mindset

---

## ❓ Q50: You said 99.9% availability—how did you measure it?

### 🧠 Question Type: Trap (Deep Validation)

### ✅ Answer

#### 📊 Definition

Availability = (Total Time - Downtime) / Total Time × 100

#### ⚙️ Measurement Approach

* Used **monitoring tools (Prometheus + CloudWatch)**
* Tracked:

  * Service uptime
  * Error rates (5xx)
  * Health check failures

#### ⚙️ Example

* Total time = 30 days
* Downtime = ~43 minutes

```
Availability = (43200 - 43) / 43200 ≈ 99.9%
```

#### 🧰 Tools

* Prometheus uptime metrics
* ALB health checks
* Grafana dashboards

#### 🎯 Key Point

Availability was calculated using **real production metrics**, not assumptions.

---

## ❓ Q51: You used HPA—what metrics did you configure?

### 🧠 Question Type: Trap (Concept + Practical)

### ✅ Answer

#### ⚙️ Metrics Used

* **CPU Utilization (primary)**
* Memory (in some cases via custom metrics)

#### ⚙️ Example

```bash
kubectl autoscale deployment app --cpu-percent=50 --min=2 --max=10
```

#### 🧠 How It Works

* Metrics Server collects CPU usage
* HPA compares against target (50%)
* Scales pods accordingly

#### 🔍 Advanced (Mention to Impress)

* Custom metrics (via Prometheus Adapter)
* Request rate / queue length scaling

#### 🎯 In My Project

Used CPU-based scaling to handle variable traffic spikes.

---

## ❓ Q52: Why did you choose EKS over ECS?

### 🧠 Question Type: Why / Decision (Compare & Justify)

### ✅ Answer

#### ⚖️ Comparison

| Feature       | EKS               | ECS         |
| ------------- | ----------------- | ----------- |
| Orchestration | Kubernetes        | AWS-native  |
| Flexibility   | High              | Moderate    |
| Portability   | Yes               | Limited     |
| Ecosystem     | Huge (Helm, etc.) | AWS-focused |

#### 🧠 Decision

Chose **EKS** because:

* Needed Kubernetes features (HPA, RBAC, Ingress)
* Industry-standard platform
* Better portability across clouds

#### ⚠️ Trade-off

* EKS more complex than ECS

#### 🎯 Conclusion

EKS was better suited for microservices and scalability requirements.

---

## ❓ Q53: Why Terraform instead of CloudFormation?

### 🧠 Question Type: Why / Decision (Compare & Justify)

### ✅ Answer

#### ⚖️ Comparison

| Feature          | Terraform    | CloudFormation      |
| ---------------- | ------------ | ------------------- |
| Provider Support | Multi-cloud  | AWS only            |
| Syntax           | HCL (simple) | JSON/YAML (verbose) |
| State Management | Explicit     | Managed by AWS      |
| Modularity       | Strong       | Moderate            |

#### 🧠 Decision

Chose **Terraform** because:

* Multi-cloud flexibility
* Reusable modules
* Better readability

#### ⚠️ Trade-off

* Need to manage state separately

#### 🎯 Conclusion

Terraform provided more flexibility and scalability for infrastructure management.

---

## ❓ Q54: Why Jenkins when GitHub Actions exists?

### 🧠 Question Type: Why / Decision (Trap)

### ✅ Answer

#### ⚖️ Comparison

| Feature     | Jenkins      | GitHub Actions |
| ----------- | ------------ | -------------- |
| Control     | Full control | Limited        |
| Setup       | Self-managed | Managed        |
| Flexibility | Very high    | Moderate       |

#### 🧠 Decision

Used **Jenkins** because:

* Required complex pipelines
* Needed plugin ecosystem
* Better control over execution environment

#### ⚠️ Trade-off

* Higher maintenance effort

#### 🎯 Balanced View

* Jenkins for complex CI/CD
* GitHub Actions for simple workflows

#### 💡 Interview Tip

Mentioning both shows **practical understanding, not bias**

---

## ❓ Q55: What is idempotency?

### 🧠 Question Type: Rapid-Fire

### ✅ Answer

Idempotency means performing the same operation multiple times results in the **same outcome**.

#### ⚙️ Example

```bash
terraform apply
```

Running it multiple times does not change infrastructure if no changes exist.

#### 🎯 Use Case

* CI/CD pipelines
* API retries

---

## ❓ Q56: What is blue-green vs canary?

### 🧠 Question Type: Rapid-Fire

### ✅ Answer

* **Blue-Green**:

  * Two environments (old + new)
  * Switch traffic instantly

* **Canary**:

  * Gradual rollout to small % of users
  * Monitor before full release

#### 🎯 Key Difference

* Blue-Green = instant switch
* Canary = gradual + safer testing

---

## ❓ Q57: What is Infrastructure as Code (IaC)?

### 🧠 Question Type: Rapid-Fire

### ✅ Answer

IaC is the practice of managing infrastructure using **code instead of manual processes**.

#### ⚙️ Tools

* Terraform
* CloudFormation

#### 🎯 Benefits

* Version control
* Automation
* Consistency

---

## ❓ Q58: What is immutable infrastructure?

### 🧠 Question Type: Rapid-Fire

### ✅ Answer

Immutable infrastructure means **servers are never modified after deployment**.

#### ⚙️ Approach

* Replace old instances with new ones
* No in-place updates

#### 🎯 Benefit

* Predictable deployments
* Reduced configuration drift

---

## ❓ Q59: What is a sidecar container?

### 🧠 Question Type: Rapid-Fire

### ✅ Answer

A sidecar container runs alongside the main container in the same pod to provide **supporting functionality**.

#### ⚙️ Examples

* Logging (Fluentd)
* Monitoring agents
* Service mesh (Envoy)

#### 🎯 Benefit

* Separation of concerns
* Reusable components

---

## ❓ Q60: Smart Questions to Ask the Interviewer

### 🧠 Question Type: Behavioral (Candidate Evaluation)

### ✅ Why This Matters

Asking thoughtful questions shows:

* Ownership mindset
* Genuine interest in the role
* Understanding of DevOps culture

---

### 💡 Questions You Should Ask

#### 1️⃣ CI/CD Maturity

> How would you describe your current CI/CD maturity, and what improvements are you planning next?

#### 🎯 Why Ask?

* Understand pipeline complexity
* Shows your interest in automation and optimization

---

#### 2️⃣ Reliability Challenges

> What are the biggest reliability or scaling challenges your team is currently facing?

#### 🎯 Why Ask?

* Aligns with SRE mindset
* Opens discussion on real production problems

---

#### 3️⃣ On-Call Structure

> How is on-call structured, and what does a typical incident response look like?

#### 🎯 Why Ask?

* Understand workload and expectations
* Shows readiness for production responsibility

---

#### 4️⃣ Future Tech Stack

> Are there any new tools or technologies the team is planning to adopt?

#### 🎯 Why Ask?

* Shows curiosity and growth mindset
* Helps you understand future learning opportunities

---

### 🚫 What NOT to Ask (Important)

* Salary (in technical round)
* Basic company info (already available online)

---

### 🎯 Final Tip

Always end with:

> "Is there anything in my profile you'd like me to elaborate on?"

This gives you a chance to clarify and leave a strong impression.
