# DevOps Engineer – Resume Based Follow-up Interview Q&A

This README contains **advanced follow-up interview questions with clear answers**, strictly based on my **DevOps Engineer resume, internship experience, and hands-on projects**. These questions represent **real drill-down questions** commonly asked in DevOps interviews after resume screening.

---

## 🔹 AWS & Cloud Infrastructure

### Q1. How did you design your VPC for high availability?

**Answer:**
I designed the VPC across multiple Availability Zones with public and private subnets. Public subnets hosted the Application Load Balancer and NAT Gateway, while EC2 instances and EKS worker nodes ran in private subnets. Route tables, Internet Gateway, and NAT Gateway ensured secure routing and outbound access.

---

### Q2. How did Auto Scaling work in your architecture?

**Answer:**
Auto Scaling Groups were attached to the Application Load Balancer. Scaling policies were based on CPU utilization and request count. Instances automatically scaled in or out based on traffic patterns to maintain performance and reduce costs.

---

### Q3. How did you manage IAM access securely?

**Answer:**
I used IAM roles instead of access keys for EC2 and EKS. Least-privilege policies were applied, and permissions were granted only as required for services like S3, CloudWatch, and ECR.

---

## 🔹 Linux System Administration

### Q4. A Linux server becomes unreachable. How do you troubleshoot?

**Answer:**
I check security groups, NACLs, route tables, and instance status checks. Then I verify disk usage, CPU and memory usage, SSH service status, and review logs such as `/var/log/messages` or `/var/log/syslog`.

---

### Q5. Difference between zombie and orphan processes?

**Answer:**
A zombie process has finished execution but still exists in the process table. An orphan process continues running after its parent process terminates and is adopted by `init` or `systemd`.

---

## 🔹 Terraform & Infrastructure as Code

### Q6. How did you structure Terraform code for multiple environments?

**Answer:**
I used reusable modules with variables for environment-specific values. Separate state files and workspaces were used for dev, test, and prod environments to maintain isolation.

---

### Q7. How did you secure Terraform state files?

**Answer:**
Terraform state files were stored in an S3 backend with versioning enabled and DynamoDB used for state locking to prevent concurrent updates.

---

### Q8. Difference between `count` and `for_each`?

**Answer:**
`count` is index-based and suitable for simple replication, while `for_each` works with maps or sets and avoids unintended resource recreation.

---

## 🔹 CI/CD – Jenkins

### Q9. What type of Jenkins pipeline did you use?

**Answer:**
I used declarative pipelines for better readability, structured stages, and built-in error handling.

---

### Q10. How did you handle rollback during a failed deployment?

**Answer:**
I rolled back to the previous stable Docker image or Kubernetes deployment revision. Jenkins pipelines were version-controlled in Git for quick recovery.

---

### Q11. How did you optimize CI/CD pipelines?

**Answer:**
I optimized Docker builds, cached dependencies, removed unnecessary stages, and parallelized jobs when possible.

---

## 🔹 Docker & Containerization

### Q12. Difference between CMD and ENTRYPOINT?

**Answer:**
ENTRYPOINT defines the main executable, while CMD provides default arguments and can be overridden easily.

---

### Q13. How do you troubleshoot a failing Docker container?

**Answer:**
I inspect logs using `docker logs`, check container configuration, run containers interactively, and verify environment variables and ports.

---

## 🔹 Kubernetes & Amazon EKS

### Q14. How does EKS simplify Kubernetes management?

**Answer:**
AWS manages the Kubernetes control plane, reducing operational overhead while allowing us to focus on workloads and worker nodes.

---

### Q15. How do ConfigMaps and Secrets differ?

**Answer:**
ConfigMaps store non-sensitive configuration, while Secrets store sensitive data like passwords and tokens.

---

### Q16. How did you expose applications in Kubernetes?

**Answer:**
Applications were exposed internally using ClusterIP services and externally using Ingress integrated with an Application Load Balancer.

---

## 🔹 Monitoring & Reliability

### Q17. How did CloudWatch help in production?

**Answer:**
CloudWatch metrics, logs, dashboards, and alarms helped detect performance issues early and enabled proactive incident response.

---

### Q18. Metrics vs Logs?

**Answer:**
Metrics are numerical time-series data, while logs provide detailed event information useful for debugging.

---

## 🔹 Security & Best Practices

### Q19. How did you implement security best practices?

**Answer:**
I applied least-privilege IAM, restricted network access, secured secrets, avoided hardcoded credentials, and followed secure deployment practices.

---

### Q20. How do you prevent accidental deletion of infrastructure?

**Answer:**
By using IAM restrictions, Terraform state locking, S3 versioning, approval steps in CI/CD pipelines, and regular backups.

---

## 🔹 Behavioral & Internship Experience

### Q21. How did you contribute as a DevOps intern?

**Answer:**
I automated repetitive tasks, supported CI/CD pipelines, improved infrastructure reliability, documented processes, and collaborated with senior engineers during deployments and issue resolution.

---

### Q22. Why are you ready for a DevOps Engineer role?

**Answer:**
I have hands-on experience with real-world tools, understand production systems, follow DevOps best practices, and continuously work on improving my skills.

---

## 📌 How to Use This File

* Revise daily before interviews
* Practice answering in **30–60 seconds**
* Use examples from your projects when responding

---

### 🚀 Next Steps

* Add **project-specific scenarios**
* Create **company-specific interview Q&A**
* Conduct **mock interviews with scoring**

---

**Author:** Sanket Chopade
**Role:** DevOps Engineer
