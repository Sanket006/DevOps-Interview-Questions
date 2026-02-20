# 📄 Documentation

- 📘 [My Devops Engineer Resume (PDF)](DevOps_Engineer_Resume.pdf)


Below are **real interview follow-up questions with clear, strong sample answers**, strictly based on **your resume and experience**. These are exactly the type of **drill-down questions interviewers ask after reading your CV**.

---

## 1. AWS & Cloud Infrastructure

### Q1. You mentioned designing and managing AWS infrastructure. Can you explain a typical architecture you worked on?

**Answer:**
I worked on a highly available AWS architecture using a custom VPC with public and private subnets across multiple Availability Zones. Application servers ran on EC2 in private subnets behind an Application Load Balancer. Auto Scaling Groups handled traffic spikes. S3 was used for static assets, CloudFront for CDN, IAM for access control, and CloudWatch for monitoring and alerts.

---

### Q2. How did you ensure security in your AWS environment?

**Answer:**
I followed least-privilege access using IAM roles and policies, avoided hardcoding credentials, used security groups and NACLs to restrict traffic, placed databases in private subnets, enabled CloudWatch logging, and ensured only ALB was exposed to the internet. Sensitive data was stored using Secrets or environment variables.

---

## 2. Linux & System Administration

### Q3. What Linux administration tasks did you perform regularly?

**Answer:**
I handled user and group management, file permissions, package installations, disk usage monitoring, process management using commands like `top` and `ps`, service management using `systemctl`, log analysis, and basic system hardening such as disabling unused services and securing SSH access.

---

### Q4. How do you troubleshoot high CPU or memory usage on a Linux server?

**Answer:**
I first use tools like `top`, `htop`, or `free -m` to identify resource usage. Then I check running processes, logs under `/var/log`, verify if any misconfigured services are causing the issue, and restart or tune services if required. I also check disk space and swap usage.

---

## 3. Terraform & Infrastructure as Code

### Q5. How did you manage multiple environments using Terraform?

**Answer:**
I used reusable modules along with variables and separate state files for dev, test, and prod environments. Workspaces helped isolate environments. I stored state files securely and used outputs to pass resource information between modules.

---

### Q6. What happens if the Terraform state file is lost?

**Answer:**
If the state file is lost, Terraform loses track of existing resources. The solution is to recreate the state using `terraform import` or restore it from a remote backend like S3 with versioning enabled.

---

## 4. CI/CD – Jenkins & GitHub

### Q7. Can you explain a Jenkins pipeline you built?

**Answer:**
I created a Jenkins pipeline that pulled code from GitHub, built the application, ran basic tests, built a Docker image, pushed it to a container registry, and deployed it to Kubernetes. Webhooks were used to trigger pipelines automatically on code changes.

---

### Q8. How did you manage credentials in Jenkins?

**Answer:**
I used Jenkins Credentials Manager to securely store secrets such as GitHub tokens, AWS credentials, and Docker registry passwords. These credentials were injected into pipelines at runtime instead of being hardcoded.

---

## 5. Docker & Containerization

### Q9. How did you optimize Docker images?

**Answer:**
I used lightweight base images, multi-stage builds, minimized layers, cleaned up unused dependencies, and used `.dockerignore` to reduce image size and build time.

---

### Q10. What problems does Docker solve?

**Answer:**
Docker ensures consistency across environments by packaging applications with their dependencies. It reduces "works on my machine" issues and simplifies deployment and scaling.

---

## 6. Kubernetes & EKS

### Q11. What Kubernetes objects did you work with?

**Answer:**
I worked with Pods, Deployments, Services, ConfigMaps, Secrets, Ingress, and namespaces. Deployments ensured self-healing and scalability, while Services exposed applications internally or externally.

---

### Q12. How does Kubernetes handle application failures?

**Answer:**
Kubernetes automatically restarts failed pods, reschedules them on healthy nodes, and maintains the desired replica count defined in Deployments.

---

## 7. Monitoring & Logging

### Q13. How did you implement monitoring?

**Answer:**
I used AWS CloudWatch to monitor EC2, ALB, and application logs. I created custom dashboards and alarms to notify the team when thresholds were breached, helping detect issues early.

---

## 8. Networking & Load Balancing

### Q14. Difference between ALB and NLB?

**Answer:**
ALB works at Layer 7 and supports path-based routing, while NLB works at Layer 4 and is used for ultra-low latency and high throughput.

---

## 9. Behavioral & Practical

### Q15. As an intern, how did you add value to the team?

**Answer:**
I automated repetitive tasks, improved deployment reliability through CI/CD pipelines, documented infrastructure and troubleshooting steps, and actively collaborated with seniors to resolve production and deployment issues.

---

### Q16. What would you improve if given more time?

**Answer:**
I would focus more on cost optimization, deeper monitoring with Prometheus and Grafana, stronger security automation, and improving disaster recovery strategies.

---

### Q17. Why should we hire you as a DevOps Engineer?

**Answer:**
I bring hands-on experience with real production-like environments, strong fundamentals in cloud and automation, and a proactive learning mindset. I can contribute from day one and continuously improve systems.

---
