# 📄 Documentation

- 📘 [Devops Resume (PDF)](DevOps_Resume.pdf)


# DevOps Resume-Based Follow-Up Questions & Answers

---

## 🔹 1️⃣ Based on: “Delivered 30% faster release cycles using CI/CD”

### ❓ Follow-up Questions:

### Q1. How exactly did you measure 30% faster release cycles?

**Answer:**
We compared deployment frequency and average release time before and after CI/CD implementation. Earlier, releases were manual and took around 2–3 hours including build, test, and deployment. After implementing Jenkins and GitHub Actions pipelines with automated testing and deployment, we reduced it to around 1.5 hours. This improvement was tracked over multiple sprints.

---

### Q2. Why did you use multiple tools like Jenkins, GitHub Actions, and GitLab CI?

**Answer:**
Different teams were using different repositories. Some projects were hosted on GitHub, others on GitLab. Jenkins was used for more complex pipelines requiring custom agents. I ensured pipeline standardization while choosing tools based on project requirements.

---

### Q3. How would you secure a CI/CD pipeline?

**Answer:**

* Store secrets in secure credential managers
* Avoid hardcoding secrets in pipeline files
* Use IAM roles instead of access keys
* Enable branch protection rules
* Implement code review approval before merge
* Scan images using tools like Trivy

---

## 🔹 2️⃣ Based on: “Managed AWS Infrastructure (EC2, VPC, IAM, ALB, Auto Scaling, EKS, CloudFront)”

### ❓ Follow-up Questions:

### Q4. How did you design high availability architecture in AWS?

**Answer:**
I deployed EC2 instances across multiple Availability Zones inside a VPC. I used:

* Application Load Balancer for traffic distribution
* Auto Scaling Group for automatic scaling
* Multi-AZ EKS cluster
* Health checks to replace unhealthy instances

This ensured fault tolerance and 99.9% uptime.

---

### Q5. Difference between Security Groups and NACLs?

**Answer:**

| Security Groups                       | NACL                           |
| ------------------------------------- | ------------------------------ |
| Stateful                              | Stateless                      |
| Instance-level                        | Subnet-level                   |
| Allow rules only                      | Allow & Deny rules             |
| Automatically tracks response traffic | Must define inbound & outbound |

---

### Q6. What happens if one Availability Zone fails?

**Answer:**
Traffic automatically shifts to healthy instances in other AZs via ALB. Auto Scaling ensures required capacity is maintained.

---

## 🔹 3️⃣ Based on: “Reduced manual provisioning by 60% using Terraform”

### ❓ Follow-up Questions:

### Q7. What is Terraform state file? Why is it important?

**Answer:**
Terraform state file (terraform.tfstate) stores mapping between infrastructure and configuration. It tracks resources created and prevents duplication. For team usage, we stored it in remote backend like S3 with DynamoDB locking.

---

### Q8. What are Terraform modules?

**Answer:**
Modules are reusable Terraform configurations. For example:

* VPC module
* EKS module
* EC2 module

This improves reusability and consistency across environments (dev, staging, prod).

---

### Q9. How do you manage multiple environments in Terraform?

**Answer:**

* Separate variable files
* Use workspaces
* Maintain different backend configs
* Modular structure

---

## 🔹 4️⃣ Based on: “Containerized applications using Docker & Kubernetes”

### ❓ Follow-up Questions:

### Q10. What is CrashLoopBackOff and how do you debug it?

**Answer:**
It means container is repeatedly crashing.
Debug steps:

* kubectl describe pod <pod-name>
* kubectl logs <pod-name>
* Check environment variables
* Verify resource limits
* Check image version

---

### Q11. What is difference between Deployment and StatefulSet?

| Deployment        | StatefulSet               |
| ----------------- | ------------------------- |
| Stateless apps    | Stateful apps             |
| Random pod names  | Ordered pod names         |
| No stable storage | Persistent volume support |

---

### Q12. How did you achieve zero downtime deployment?

**Answer:**
Using Rolling Update strategy in Kubernetes:

* Set maxUnavailable=0
* Set maxSurge=1

This ensures new pods are ready before old pods terminate.

---

## 🔹 5️⃣ Based on: “Monitoring using CloudWatch”

### ❓ Follow-up Questions:

### Q13. What metrics did you monitor in production?

**Answer:**

* CPU Utilization
* Memory usage
* Disk I/O
* Network traffic
* ALB 5xx errors
* Pod restarts

---

### Q14. What will you do if CPU usage is 95%?

**Answer:**

* Check application logs
* Identify spike cause
* Scale horizontally using ASG
* Increase instance size if required
* Check for memory leaks

---

## 🔹 6️⃣ Based on: “IAM Least Privilege”

### ❓ Follow-up Questions:

### Q15. What is IAM Role vs IAM User?

**Answer:**
IAM User → Permanent credentials (for humans)
IAM Role → Temporary credentials (for services like EC2)

Best practice: Attach IAM roles to EC2 instead of access keys.

---

## 🔹 7️⃣ Based on: “Scripting using Bash”

### ❓ Follow-up Questions:

### Q16. Write a script to find largest file in a directory.

```bash
du -ah /directory | sort -rh | head -n 1
```

---

### Q17. How do you automate a backup using shell script?

**Answer:**

* Create tar archive
* Move to backup directory
* Schedule using cron job

---

## 🔹 8️⃣ HR Behavioral Questions

### ❓ Q18. Tell me about a production incident you handled.

**Answer Structure (STAR Method):**

* Situation: High CPU in production
* Task: Identify root cause
* Action: Checked CloudWatch, found traffic spike
* Result: Scaled ASG, resolved within 15 minutes

---

### ❓ Q19. Why should we hire you as a DevOps Engineer?

**Answer:**
Because I have hands-on experience with real production-like infrastructure, CI/CD automation, Kubernetes deployments, and cloud security best practices. I focus on automation, reliability, and continuous improvement.

---

# Advanced DevOps Interview Questions & Answers

---

## 1) How do you design Multi-Region Architecture in AWS?

### Answer:

Designing a multi-region architecture ensures high availability, disaster recovery, and low latency for global users.

### Key Components:

1. Route 53

   * Use Latency-based routing or Failover routing
   * Health checks for automatic failover

2. Application Layer

   * Deploy application in multiple AWS regions
   * Use ALB in each region
   * Auto Scaling Groups across multiple AZs

3. Data Layer

   * Use RDS Multi-Region (Read Replica or Global Database)
   * DynamoDB Global Tables
   * S3 Cross-Region Replication

4. Disaster Recovery Strategy

   * Active-Active (traffic split across regions)
   * Active-Passive (one primary, one standby)

5. Infrastructure as Code

   * Terraform modules for region replication

### Example Design Flow:

User → Route 53 → Closest Region ALB → EC2/EKS → RDS Global Database

---

## 2) How do you secure a Kubernetes Cluster?

### Answer:

Security in Kubernetes is applied at multiple layers.

### 1. Cluster Access Control

* Use RBAC
* Disable anonymous access
* Use IAM Roles for Service Accounts (IRSA in EKS)

### 2. Network Security

* Use Network Policies
* Restrict pod-to-pod communication
* Private cluster endpoint

### 3. Pod Security

* Run containers as non-root
* Use Pod Security Standards
* Resource limits and requests

### 4. Image Security

* Scan images using Trivy
* Use trusted registries

### 5. Secrets Management

* Avoid plain text secrets
* Use Kubernetes Secrets or AWS Secrets Manager

### 6. Monitoring & Auditing

* Enable audit logs
* Use Prometheus + Grafana

---

## 3) How does Application Load Balancer (ALB) work internally?

### Answer:

ALB works at Layer 7 (Application Layer).

### Internal Working:

1. Client sends HTTP/HTTPS request
2. Request hits ALB DNS name
3. ALB Listener checks rules
4. Based on path/host-based routing, traffic is forwarded to target group
5. Target group forwards request to healthy EC2 instances or pods
6. Health checks continuously monitor targets

### Key Concepts:

* Listener (Port 80/443)
* Rules (Path-based routing)
* Target Groups
* Health Checks

---

## 4) Explain Kubernetes Networking

### Answer:

Kubernetes networking ensures communication between:

1. Pod-to-Pod (same node)
2. Pod-to-Pod (different nodes)
3. Pod-to-Service
4. External-to-Service

### Important Concepts:

1. Each Pod gets unique IP
2. CNI Plugin handles networking (e.g., AWS VPC CNI)
3. Service provides stable IP
4. kube-proxy manages routing
5. Ingress manages external HTTP access

---

## 5) What happens when you run `docker run`?

### Step-by-Step Internal Process:

1. Docker CLI sends request to Docker Daemon
2. Daemon checks local image
3. If not found, pulls from Docker Hub
4. Creates container from image
5. Sets up namespaces and cgroups
6. Allocates network interface
7. Mounts file system layers
8. Starts container process

### Key Components Involved:

* Namespaces (process isolation)
* cgroups (resource limits)
* Union File System
* Docker Engine

---

End of Document
