## 1️⃣ Designing a Highly Available Multi-AZ Architecture

**Design approach:**

* Use **VPC** spanning multiple AZs.
* Place **Application Load Balancer (ALB)** in public subnets across at least 2 AZs.
* Run **Auto Scaling Groups (ASG)** for EC2 or EKS worker nodes across multiple AZs.
* Use **RDS Multi-AZ** or **Aurora** for database high availability.
* Store static assets in **S3** with **CloudFront**.
* Use **Route 53** for health checks and DNS failover.

**What I monitor first during an outage:**

1. ALB target health (are targets failing or draining?)
2. CloudWatch metrics – 5xx errors, latency, request count
3. Auto Scaling activity (scaling events, instance launch failures)
4. Application logs (CloudWatch Logs)
5. Downstream dependencies (DB, external APIs)

---

## 2️⃣ Security Groups vs NACLs (Real-world Usage)

| Feature    | Security Group | NACL          |
| ---------- | -------------- | ------------- |
| Level      | Instance / ENI | Subnet        |
| Stateful   | Yes            | No            |
| Rules      | Allow only     | Allow + Deny  |
| Evaluation | All rules      | Order matters |

**When I actually use NACLs:**

* Blocking known malicious IP ranges at subnet level
* Adding an extra security layer for compliance-heavy environments
* Protecting public subnets from broad inbound access

In most production systems, **Security Groups handle 90% of access control**, and NACLs are used sparingly.

---

## 3️⃣ Managing IAM at Scale (Without a Security Nightmare)

**My strategy:**

* Follow **least privilege** strictly
* Use **IAM roles**, never long-term access keys
* Create **role-based access** (Dev, QA, Prod)
* Use **permission boundaries** to control max privileges
* Centralize access with **AWS SSO / IAM Identity Center**
* Enforce MFA for all human users
* Regular audits using **IAM Access Analyzer**

This avoids policy sprawl and accidental over-permissioning.

---

## 4️⃣ Cost Optimization Strategy in Production

**Core principles:**

* Right-size EC2 using CloudWatch metrics
* Use **Auto Scaling** aggressively
* Prefer **Spot Instances** for non-critical workloads
* Purchase **Savings Plans / Reserved Instances**
* Enable **S3 lifecycle policies**
* Monitor with **AWS Cost Explorer** and budgets

**Ongoing practice:**

* Monthly cost reviews
* Tag everything (env, owner, project)
* Alerts for cost anomalies

---

## 5️⃣ Debugging 5xx Errors on an ALB-backed Service

**Step-by-step approach:**

1. Check ALB metrics – `HTTPCode_ELB_5XX_Count` vs `HTTPCode_Target_5XX_Count`
2. Verify target group health checks
3. Check application logs for crashes or timeouts
4. Review recent deployments or config changes
5. Check Auto Scaling events
6. Validate downstream services (DB, cache, APIs)

This helps isolate whether the issue is **load balancer**, **application**, or **dependency-related**.

---

## 6️⃣ Designing S3 + CloudFront for Performance & Security

**Performance:**

* Use CloudFront edge locations
* Enable compression
* Use cache-control headers
* Enable Origin Shield

**Security:**

* Block public S3 access
* Use **Origin Access Control (OAC)** or OAI
* Use **WAF** with CloudFront
* Enable bucket policies with least privilege
* Enable access logs

**Result:** Low latency, secure, and globally scalable static content delivery.
