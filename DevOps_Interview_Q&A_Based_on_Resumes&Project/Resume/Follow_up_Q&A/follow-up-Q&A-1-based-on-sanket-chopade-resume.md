# 📄 Documentation

- 📘 [Sanket Chopade Resume (PDF)](Sanket_Chopade_Resume.pdf)

Below are **real interview follow-up questions with clear, strong sample answers**, strictly based on **Sanket Chopade resume and experience**. These are exactly the type of **drill-down questions interviewers ask after reading your CV**.

---

# DevOps Interview Follow-Up Questions & Answers

**Candidate:** Sanket Chopade
**Role Target:** DevOps Engineer (0–2 Years)

---

## 1. You mentioned achieving 99.9% availability. How did you calculate and ensure it?

**Answer:**
I ensured 99.9% availability by designing a highly available architecture using Multi-AZ deployments in AWS. EC2 instances were distributed across multiple Availability Zones and attached to an Application Load Balancer. I configured Auto Scaling Groups with health checks to automatically replace unhealthy instances. I monitored uptime using CloudWatch metrics and logs. Based on downtime records and total monthly runtime, we maintained uptime within the 99.9% SLA threshold.

---

## 2. How did you reduce infrastructure provisioning time by 40% using Terraform?

**Answer:**
Initially, infrastructure was manually provisioned via the AWS console, which was time-consuming and error-prone. I introduced modular Terraform configurations for reusable components such as VPC, EC2, EKS, IAM, and ALB. I implemented remote state management using S3 and state locking with DynamoDB. This automation allowed faster and consistent provisioning across dev, test, and prod environments, reducing provisioning time by 40%.

---

## 3. Explain your CI/CD pipeline workflow using Jenkins.

**Answer:**
Our Jenkins pipeline followed these stages:

1. Code push to GitHub triggered a webhook.
2. Jenkins pulled the latest code.
3. Build stage compiled the application and built a Docker image.
4. Test stage executed automated tests.
5. Docker image was pushed to Amazon ECR.
6. Deployment stage updated Kubernetes manifests and deployed to EKS.

We used Declarative Pipeline for better readability and maintainability. This reduced deployment time from 45 minutes to 10 minutes.

---

## 4. How did you implement blue-green and rolling deployments in Kubernetes?

**Answer:**
For rolling deployments, I configured deployment strategies in Kubernetes with maxUnavailable and maxSurge parameters. For blue-green deployments, I created two identical environments (blue and green) and switched traffic using Kubernetes services after validating the new deployment. This ensured zero downtime and a 98% deployment success rate.

---

## 5. How did you reduce cloud costs by 25%?

**Answer:**
I optimized instance types based on CPU and memory usage metrics from CloudWatch. Implemented Auto Scaling policies to scale based on traffic. Used S3 lifecycle policies to transition data to cheaper storage classes. Identified idle resources and unused volumes. These combined efforts reduced cloud costs by 25%.

---

## 6. How did you secure your AWS environment?

**Answer:**
I implemented IAM roles with least-privilege policies, restricted security groups, configured private subnets for internal services, enabled encryption for S3 buckets, and implemented NACL rules for network segmentation. Additionally, I enabled CloudTrail for auditing and monitored suspicious activity.

---

## 7. How did you monitor and reduce MTTR by 30%?

**Answer:**
I integrated CloudWatch, Prometheus, and Grafana dashboards for centralized monitoring. Configured alerts for CPU, memory, and pod restarts. When incidents occurred, logs were analyzed using CloudWatch Logs and kubectl logs. Faster root cause identification reduced MTTR by 30%.

---

## 8. What challenges did you face while working with EKS?

**Answer:**
One challenge was pod scheduling failures due to insufficient node capacity. I resolved it by adjusting Auto Scaling node group configurations. Another issue was ImagePullBackOff due to incorrect IAM roles for service accounts, which I resolved using IRSA (IAM Roles for Service Accounts).

---

## 9. How did you automate Linux tasks using Bash?

**Answer:**
I wrote Bash scripts to automate log rotation, backup processes, user management, and service health checks. These scripts were scheduled using cron jobs, eliminating 70% of repetitive manual tasks.

---

## 10. How did CloudFront improve latency by 40%?

**Answer:**
CloudFront cached static assets globally at edge locations. Users accessed content from the nearest edge location instead of the origin server in S3, reducing response time and improving global performance by 40%.

---

## 11. What is your Terraform module structure?

**Answer:**
I follow a modular structure:

* modules/vpc
* modules/ec2
* modules/eks
* modules/iam
* environments/dev
* environments/prod

Each environment calls reusable modules with variable files, ensuring consistency and scalability.

---

## 12. How do you handle secrets in CI/CD pipelines?

**Answer:**
Secrets are stored in AWS Secrets Manager and Jenkins credentials store. Environment variables are injected securely into the pipeline. Sensitive data is never hardcoded in the repository.

---

## 13. How would you troubleshoot a failing Kubernetes pod?

**Answer:**

1. Check pod status using kubectl get pods.
2. Describe the pod for events.
3. Check logs using kubectl logs.
4. Verify resource limits and node capacity.
5. Check service endpoints and config maps.

---

## 14. How do you ensure environment consistency across dev, test, and prod?

**Answer:**
Using Terraform modules, version-controlled infrastructure code, Docker images for application packaging, and Kubernetes manifests stored in Git. This ensures identical infrastructure and application behavior across environments.

---

## 15. If production deployment fails, what is your rollback strategy?

**Answer:**
For Kubernetes, I use kubectl rollout undo to revert to the previous stable version. In blue-green deployment, I redirect traffic back to the stable environment. This minimizes downtime and business impact.

---

# Pro Tip for Interviews

While answering, always:

* Use numbers (metrics & KPIs)
* Explain tools used
* Mention business impact
* Structure answers using STAR method

---

# Advanced DevOps Follow-Up & Production Scenario Questions

**Candidate:** Sanket Chopade
**Experience Level:** DevOps Engineer (0–2 Years)
**Focus:** Advanced, Production-Grade, Scenario-Based Questions

---

# 🔥 Section 1: Advanced Architecture & AWS Design

## 1. Your application suddenly receives 10x traffic due to a marketing campaign. CPU utilization hits 90% across nodes. What will you do?

**Answer:**

1. Immediately verify metrics in CloudWatch (CPU, memory, request count, ALB target response time).
2. Confirm Auto Scaling Group scaling policies are working (target tracking or step scaling).
3. If scaling is not happening fast enough:

   * Increase desired capacity manually.
   * Adjust scaling thresholds.
4. If EKS nodes are saturated:

   * Increase node group max capacity.
   * Enable Cluster Autoscaler.
5. Verify HPA (Horizontal Pod Autoscaler) configuration.
6. After stabilization, conduct post-mortem and tune scaling policies.

---

## 2. Production latency increases but CPU and memory look normal. How do you debug this?

**Answer:**

1. Check ALB Target Response Time.
2. Analyze application logs for slow DB queries.
3. Inspect RDS metrics (IOPS, connections, locks).
4. Use distributed tracing if available.
5. Check network latency between pods and database.
6. Validate DNS resolution and external API calls.

Root cause is often DB bottleneck, connection pool exhaustion, or downstream API delay.

---

## 3. Terraform state file gets corrupted. What will you do?

**Answer:**

1. If using remote backend (S3 + DynamoDB lock), retrieve previous version from S3 versioning.
2. Restore state from backup.
3. Run terraform refresh to sync infrastructure.
4. Never manually edit state unless absolutely required.
5. Implement state file versioning & locking best practices.

---

## 4. How would you design a highly available EKS cluster across regions?

**Answer:**

1. Deploy EKS in multiple regions.
2. Use Route 53 latency-based or failover routing.
3. Use multi-region RDS or database replication.
4. Store static content in S3 with cross-region replication.
5. Use Infrastructure as Code to replicate architecture.

---

# 🚨 Section 2: Production Incident Scenarios

## 5. Pods are in CrashLoopBackOff. Logs show "OOMKilled". What is your approach?

**Answer:**

1. Check resource requests and limits.
2. Increase memory limit in deployment YAML.
3. Verify memory leak in application.
4. Monitor with Prometheus.
5. Adjust HPA if needed.

---

## 6. Jenkins pipeline succeeds, but application is not updated in production. Why?

**Answer:**
Possible causes:

* Kubernetes imagePullPolicy set to IfNotPresent.
* Same Docker image tag reused (e.g., latest).
* Deployment not triggered properly.
* Rolling update stuck.

Fix:

* Use unique image tags.
* Use kubectl rollout status.
* Force rollout restart.

---

## 7. You deployed a change and users report 500 errors. What immediate steps will you take?

**Answer:**

1. Check application logs.
2. Check ALB health checks.
3. Verify database connectivity.
4. Rollback using kubectl rollout undo.
5. Inform stakeholders.
6. Start RCA documentation.

---

## 8. EKS worker nodes are running but pods are not scheduling. What could be wrong?

**Answer:**

* Insufficient CPU/memory.
* Taints and tolerations mismatch.
* Node selector misconfiguration.
* IAM permission issues.
* CNI networking issue.

---

# 🔐 Section 3: Security & DevSecOps Scenarios

## 9. A security audit finds overly permissive IAM policies. How do you fix it?

**Answer:**

1. Identify unused permissions via IAM Access Analyzer.
2. Replace wildcard (*) policies.
3. Implement least privilege.
4. Use IAM roles instead of static credentials.
5. Rotate keys.

---

## 10. Docker image contains vulnerabilities. What will you do?

**Answer:**

1. Scan image using Trivy or Docker Scout.
2. Update base image.
3. Remove unnecessary packages.
4. Rebuild and redeploy.
5. Integrate security scanning in CI pipeline.

---

# 📈 Section 4: Monitoring & Reliability Engineering

## 11. How would you reduce MTTR further?

**Answer:**

* Implement structured logging.
* Add alert severity classification.
* Automate incident notifications (Slack, PagerDuty).
* Create runbooks.
* Perform blameless postmortems.

---

## 12. How do you design SLOs and SLAs?

**Answer:**

1. Define service availability target (e.g., 99.9%).
2. Define error budget.
3. Monitor request success rate.
4. Alert when threshold breaches.

---

# 🎯 Section 5: Expert-Level Thought Questions

## 13. When should you NOT use Kubernetes?

**Answer:**

* Small application with low traffic.
* Monolithic system with no scaling needs.
* Budget constraints.
* No container expertise in team.

---

## 14. Explain trade-offs between ECS and EKS.

**Answer:**

* ECS: Simpler, AWS-managed, lower operational overhead.
* EKS: Kubernetes standard, portable, more control but higher complexity.

---

## 15. If given full ownership of infrastructure, what improvements would you implement?

**Answer:**

* Implement GitOps (ArgoCD).
* Add automated security scanning.
* Multi-region DR strategy.
* Cost monitoring dashboards.
* Chaos engineering testing.

---

# 🎤 Interview Tip

At advanced level, interviewers expect:

* Root cause thinking
* Trade-off analysis
* Business impact understanding
* Proactive improvements

---

# DevOps Interview Follow-Up Questions & Advanced System Design Q&A

**Candidate:** Sanket Chopade
**Role Target:** DevOps Engineer (0–2 Years)
**Location:** Pune, India

---

# 📌 Resume-Based Deep Technical Follow-Up Questions

All questions are derived from professional experience listed in the resume:
📄 Sanket Chopade Resume 

---

## ☁️ AWS Architecture Deep Dive

### 1️⃣ Design a Highly Available 3-Tier Architecture on AWS

**Answer:**

### Architecture Overview:

* Route53 → CloudFront → ALB
* ALB → EC2 / EKS (App Layer)
* RDS (Multi-AZ) / DynamoDB
* S3 for static assets

### Key Design Decisions:

* Multi-AZ deployment for fault tolerance
* Auto Scaling Groups for elasticity
* Private subnets for application and database layers
* NAT Gateway for outbound internet access
* IAM roles with least privilege

### How I Would Improve It:

* Add WAF for application protection
* Enable AWS Shield
* Use Redis (ElastiCache) for caching
* Enable automated backups & cross-region replication

---

### 2️⃣ Explain Your VPC Design Strategy

**Answer:**
I designed a custom VPC with:

* 2 Public subnets (ALB, NAT)
* 2 Private subnets (App Layer)
* 2 Private DB subnets
* Internet Gateway
* NAT Gateway
* Route Tables separation

Security controls:

* Security Groups (stateful)
* NACLs (stateless)
* Bastion Host for SSH access

CIDR planning ensured no IP overlap across environments.

---

## 🚀 Advanced CI/CD & DevSecOps

### 3️⃣ How Would You Design a Production-Grade CI/CD Pipeline?

**Answer:**

### Pipeline Stages:

1. Code Commit → GitHub
2. Webhook → Jenkins Trigger
3. Static Code Analysis (SonarQube)
4. Dependency Scan (OWASP)
5. Build Docker Image
6. Push to ECR
7. Deploy to EKS (Helm)
8. Post-deployment smoke tests
9. Rollback automation if failure detected

### Security Enhancements:

* Secrets managed via AWS Secrets Manager
* IAM roles instead of static credentials
* Image scanning before deployment

---

## 🐳 Kubernetes Advanced Questions

### 4️⃣ How Would You Design a Scalable Kubernetes Architecture?

**Answer:**

* EKS with Managed Node Groups
* Separate node groups for system and workloads
* HPA based on CPU & custom metrics
* Cluster Autoscaler enabled
* PodDisruptionBudgets for HA
* Ingress via ALB Controller

### Scaling Strategy:

* Horizontal scaling for stateless apps
* Vertical scaling for stateful services

---

### 5️⃣ How Do You Troubleshoot Production Kubernetes Issues?

**Answer Framework:**

1. Check pod status → kubectl get pods
2. Describe pod → kubectl describe pod
3. Check logs → kubectl logs
4. Validate resource limits
5. Inspect node health
6. Review recent deployments

---

## 📊 Observability & Reliability Engineering

### 6️⃣ How Would You Design Observability for Microservices?

**Answer:**

Three Pillars:

* Metrics → Prometheus
* Logs → CloudWatch / ELK
* Traces → Jaeger / X-Ray

SLO Example:

* 99.9% availability
* <200ms response time

Alerting via Grafana & CloudWatch alarms.

---

# 🏗 SYSTEM DESIGN INTERVIEW SECTION

---

## 7️⃣ Design a Scalable E-Commerce Application (DevOps Perspective)

### Requirements:

* 1M daily users
* High availability
* Secure payments
* Zero downtime deployments

### Proposed Architecture:

User → CloudFront → WAF → ALB
ALB → EKS (Microservices)
EKS → RDS (Multi-AZ)
Redis for caching
S3 for product images

### DevOps Considerations:

* Blue-Green deployments
* Auto Scaling
* Backup strategy
* CI/CD automation
* Monitoring & Alerting

### Bottleneck Handling:

* Read replicas
* CDN caching
* Horizontal scaling

---

## 8️⃣ Design a Log Processing System

### Requirements:

* Collect logs from 100+ servers
* Real-time monitoring
* Long-term storage

### Architecture:

Application → Fluent Bit → Kafka
Kafka → Logstash
Logstash → Elasticsearch
Visualization → Kibana

### Scalability Strategy:

* Kafka partitioning
* Elasticsearch clustering
* Lifecycle policies for log retention

---

## 9️⃣ Design a CI/CD Platform for 50 Developers

### Requirements:

* Parallel builds
* Secure artifact storage
* Environment promotion

### Architecture:

GitHub → Jenkins Master (HA)
Jenkins Agents (Auto-scaled)
Artifacts → Nexus / ECR
Deployment → Kubernetes

### Key Improvements:

* Role-based access control
* Audit logging
* Automated rollback

---

# 🔥 Production Scenario-Based Questions

### 🔟 What Would You Do If Production Goes Down?

**Step-by-Step Response:**

1. Identify impact scope
2. Check monitoring alerts
3. Rollback recent changes
4. Scale resources if required
5. Root cause analysis
6. Postmortem documentation

---

# 📈 Advanced Interview Tips

* Always discuss trade-offs
* Mention cost optimization
* Discuss security controls
* Explain scaling strategy
* Provide metrics-based answers

---

# 🚀 Ready for Advanced DevOps Interviews (2026)

This README prepares you for:

* Startup interviews
* Product-based companies
* MNC technical rounds
* System design discussions

---

**Prepared for Senior-Level DevOps Discussions** 💼
