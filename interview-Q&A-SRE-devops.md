# 🚀 DevOps / SRE Interview Questions & Answers

This document contains structured answers (Paragraph + Pointwise format) for commonly asked DevOps, Cloud, CI/CD, Terraform, and Docker interview questions.
It is designed for quick revision and future reference.

---

# 📌 Day-to-Day & Cloud Basics

## 1. Explain your day-to-day activities as a DevOps/SRE engineer.

**Paragraph Answer:**
As a DevOps/SRE engineer, my daily activities focus on maintaining system reliability, automating deployments, monitoring infrastructure, and improving CI/CD processes. I collaborate with development teams to ensure smooth releases, troubleshoot production issues, and optimize cloud resources for performance and cost efficiency.

**Pointwise Answer:**

* Monitor infrastructure using CloudWatch/Prometheus.
* Manage CI/CD pipelines (Jenkins/GitHub Actions).
* Handle deployments and rollbacks.
* Troubleshoot production issues.
* Implement Infrastructure as Code (Terraform).
* Optimize cloud cost and performance.

---

## 2. What is cloud migration?

**Paragraph Answer:**
Cloud migration is the process of moving applications, data, and workloads from on-premises infrastructure to cloud platforms like AWS. The goal is to improve scalability, availability, cost-efficiency, and operational flexibility.

**Pointwise Answer:**

* Moving apps and data to cloud.
* Can be Rehost, Replatform, Refactor.
* Improves scalability and availability.
* Reduces infrastructure maintenance.

---

## 3. What is a VPC?

**Paragraph Answer:**
A VPC (Virtual Private Cloud) is a logically isolated virtual network in AWS where we can launch resources securely. It allows us to control IP ranges, subnets, routing tables, security groups, and gateways.

**Pointwise Answer:**

* Private virtual network in AWS.
* Define CIDR range.
* Create public and private subnets.
* Control traffic using route tables and security groups.

---

## 4. What is a subnet and a NAT Gateway?

**Paragraph Answer:**
A subnet is a smaller network inside a VPC that divides resources into public and private segments. A NAT Gateway allows instances in private subnets to access the internet securely without exposing them directly.

**Pointwise Answer:**

* Subnet: Segmented part of VPC.
* Public subnet: Has internet gateway route.
* Private subnet: No direct internet access.
* NAT Gateway: Provides outbound internet for private instances.

---

## 5. If the private IP you were using is no longer available, what steps will you take?

**Paragraph Answer:**
If a private IP is unavailable, I first verify whether it was dynamically assigned or manually configured. I check subnet IP availability, network configurations, and instance settings. If necessary, I reassign a new IP or modify subnet CIDR ranges.

**Pointwise Answer:**

* Check subnet IP availability.
* Verify instance network config.
* Assign new private IP.
* Modify subnet CIDR if needed.

---

## 6. Explain the different types of block storage.

**Paragraph Answer:**
Block storage provides raw storage volumes that can be attached to instances. In AWS, Amazon EBS offers different volume types optimized for performance and cost.

**Pointwise Answer:**

* gp3/gp2 – General purpose SSD.
* io1/io2 – High performance SSD.
* st1 – Throughput optimized HDD.
* sc1 – Cold HDD.

---

## 7. What is Amazon S3?

**Paragraph Answer:**
Amazon S3 is an object storage service in AWS used to store and retrieve unlimited amounts of data with high durability and availability.

**Pointwise Answer:**

* Object storage service.
* 99.999999999% durability.
* Used for backups, logs, static websites.

---

## 8. What are S3 storage classes, and which one have you used?

**Paragraph Answer:**
S3 provides multiple storage classes like Standard, Intelligent-Tiering, Standard-IA, and Glacier for different access patterns. I have used S3 Standard and Intelligent-Tiering for application assets and backups.

**Pointwise Answer:**

* Standard – Frequently accessed.
* Intelligent-Tiering – Auto cost optimization.
* Standard-IA – Infrequent access.
* Glacier – Archival storage.

---

## 9. What will you do if a file upload fails?

**Paragraph Answer:**
If a file upload fails, I check logs, network connectivity, IAM permissions, and bucket policies. I also verify file size limits and retry upload using CLI for debugging.

**Pointwise Answer:**

* Check error logs.
* Verify IAM permissions.
* Check network connectivity.
* Retry using CLI.

---

## 10. Where is cache used in AWS?

**Paragraph Answer:**
Caching in AWS is used to improve application performance and reduce database load. Services like ElastiCache (Redis/Memcached) and CloudFront are commonly used for caching.

**Pointwise Answer:**

* ElastiCache (Redis).
* CloudFront CDN.
* Reduces latency.
* Improves performance.

---

## 11. What is the difference between NLB & ALB?

**Paragraph Answer:**
NLB (Network Load Balancer) operates at Layer 4 (TCP/UDP) and is used for high-performance traffic. ALB (Application Load Balancer) operates at Layer 7 (HTTP/HTTPS) and supports advanced routing.

**Pointwise Answer:**

* NLB: Layer 4, high throughput.
* ALB: Layer 7, path-based routing.
* ALB supports host-based routing.

---

# 📌 Git, Versioning & Automation

## 12. Which branching strategy have you worked with?

**Paragraph Answer:**
I have worked with Git Flow and feature branching strategies, where developers create feature branches and merge via pull requests after review.

**Pointwise Answer:**

* Feature branches.
* Pull requests.
* Code review.
* Main/master branch protected.

---

## 13. Write a command to create a branch in Git.

**Answer:**

```
git checkout -b feature-branch
```

---

## 14. Why is versioning important?

**Paragraph Answer:**
Versioning helps track changes, manage releases, and roll back to stable versions if issues occur.

**Pointwise Answer:**

* Track changes.
* Release management.
* Rollback capability.

---

## 15. What automation tools have you used?

**Paragraph Answer:**
I have used Jenkins for CI/CD, Terraform for Infrastructure as Code, Docker for containerization, and GitHub Actions for workflow automation.

**Pointwise Answer:**

* Jenkins.
* Terraform.
* Docker.
* GitHub Actions.

---

# 📌 Jenkins & CI/CD

## 16. What are the components of Jenkins?

**Pointwise Answer:**

* Jenkins Master.
* Agents/Nodes.
* Plugins.
* Pipelines.

---

## 17. What are agents in Jenkins?

**Paragraph Answer:**
Agents are worker machines that execute build jobs assigned by the Jenkins master.

---

## 18. How do you secure your CI/CD pipeline?

**Pointwise Answer:**

* Use IAM roles.
* Store secrets in Vault/Secrets Manager.
* Enable code scanning.
* Use HTTPS.

---

## 19. Explain the stages in a CI/CD pipeline.

**Pointwise Answer:**

* Code.
* Build.
* Test.
* Scan.
* Deploy.
* Monitor.

---

## 20. Write the stages of a typical CI/CD pipeline.

* Source
* Build
* Test
* Security Scan
* Artifact Store
* Deploy

---

## 21. Difference between on and job in YAML pipeline?

**Answer:**

* `on`: Defines trigger events.
* `job`: Defines tasks to execute.

---

# 📌 Terraform

## 22. What are data blocks in Terraform?

**Paragraph Answer:**
Data blocks are used to fetch existing resource information instead of creating new resources.

---

## 23. What are Terraform modules?

**Paragraph Answer:**
Modules are reusable Terraform configurations that help organize and standardize infrastructure.

---

## 24. How do you secure Terraform code?

**Pointwise Answer:**

* Remote backend (S3 + DynamoDB).
* IAM roles.
* Secret manager.
* Code scanning.

---

## 25. What is UAT environment in Terraform?

**Paragraph Answer:**
UAT (User Acceptance Testing) is a staging-like environment where users validate application functionality before production deployment.

---

# 📌 Docker

## 26. Write a simple Dockerfile.

```
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

---

## 27. What is the difference between CMD and ENTRYPOINT?

**Paragraph Answer:**
CMD provides default commands that can be overridden, while ENTRYPOINT defines the main command that always runs when the container starts.

---

# 🚀 DevOps / SRE Interview Questions & Answers (Part 2)

This document contains structured answers (Paragraph + Pointwise format) for Kubernetes, Monitoring, Python Scripting, and Linux interview questions.

---

# 📌 Kubernetes

## 28. Explain Kubernetes architecture.

**Paragraph Answer:**
Kubernetes architecture consists of a Control Plane and Worker Nodes. The Control Plane manages cluster state, scheduling, and API communication, while Worker Nodes run application workloads inside Pods. It ensures high availability, scalability, and self-healing of applications.

**Pointwise Answer:**

* Control Plane Components:

  * API Server
  * etcd (Cluster database)
  * Scheduler
  * Controller Manager
* Worker Node Components:

  * Kubelet
  * Kube Proxy
  * Container Runtime (Docker/containerd)
* Pods run inside Worker Nodes.

---

## 29. How did you deploy an application on Kubernetes?

**Paragraph Answer:**
I containerized the application using Docker, pushed the image to a registry, created Kubernetes Deployment and Service YAML files, and applied them using kubectl. I verified pod status and exposed the service using LoadBalancer or Ingress.

**Pointwise Answer:**

* Build Docker image.
* Push to Docker Hub/ECR.
* Create Deployment YAML.
* Create Service YAML.
* Apply using `kubectl apply -f`.
* Verify using `kubectl get pods`.

---

## 30. Write basic Kubernetes commands.

```
kubectl get pods
kubectl get nodes
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl apply -f file.yaml
kubectl delete -f file.yaml
```

---

## 31. How do you secure a Kubernetes application?

**Paragraph Answer:**
To secure Kubernetes applications, I implement RBAC, use Secrets for sensitive data, enable network policies, and scan container images. I also restrict privileged containers and enable TLS for communication.

**Pointwise Answer:**

* Enable RBAC.
* Use Secrets.
* Implement Network Policies.
* Image scanning (Trivy).
* Use HTTPS/TLS.

---

## 32. What is HPA (Horizontal Pod Autoscaling)?

**Paragraph Answer:**
HPA automatically scales the number of pods based on CPU or memory usage metrics to handle varying workloads efficiently.

**Pointwise Answer:**

* Scales pods horizontally.
* Based on CPU/Memory metrics.
* Improves availability.

---

## 33. What are ConfigMaps and Secrets?

**Paragraph Answer:**
ConfigMaps store non-sensitive configuration data, while Secrets store sensitive information like passwords and API keys in encoded format.

**Pointwise Answer:**

* ConfigMap → App configs.
* Secret → Sensitive data.
* Mounted as env variables or volumes.

---

## 34. What is a Persistent Volume?

**Paragraph Answer:**
A Persistent Volume (PV) is a storage resource in Kubernetes that exists independently of pods and retains data even if pods restart.

**Pointwise Answer:**

* Storage abstraction.
* Bound via Persistent Volume Claim (PVC).
* Data persists after pod deletion.

---

## 35. What steps do you follow if a pod fails?

**Paragraph Answer:**
If a pod fails, I check pod status, describe the pod, review logs, verify resource limits, and check events. I also inspect node health and restart or redeploy if required.

**Pointwise Answer:**

* `kubectl get pods`
* `kubectl describe pod`
* `kubectl logs`
* Check resource limits.
* Restart or rollback.

---

## 36. Why do pods restart?

**Paragraph Answer:**
Pods restart due to application crashes, failed liveness probes, resource limits exceeded, or configuration errors.

**Pointwise Answer:**

* CrashLoop.
* OOMKilled.
* Liveness probe failure.
* Node issues.

---

## 37. Why do we perform rollbacks?

**Paragraph Answer:**
Rollbacks are performed to revert to a stable version if a new deployment introduces bugs or performance issues.

---

## 38. What will you do if CrashLoopBackOff occurs?

**Paragraph Answer:**
I check logs, describe the pod, verify environment variables, resource limits, and image configuration to identify the root cause before redeploying.

---

## 39. Through which Kubernetes service did you expose the application?

**Paragraph Answer:**
I exposed applications using LoadBalancer for external access and ClusterIP for internal communication. In production, I used Ingress for domain-based routing.

---

# 📌 Monitoring & SRE

## 40. What monitoring tools have you used?

**Paragraph Answer:**
I have used Prometheus and Grafana for metrics monitoring, CloudWatch for AWS resource monitoring, and ELK stack for log analysis.

**Pointwise Answer:**

* Prometheus
* Grafana
* CloudWatch
* ELK Stack

---

## 41. How do metrics get into Prometheus?

**Paragraph Answer:**
Prometheus collects metrics by scraping HTTP endpoints exposed by applications or exporters at regular intervals.

**Pointwise Answer:**

* Exporters expose `/metrics` endpoint.
* Prometheus scrapes metrics.
* Stores time-series data.

---

## 42. How did you identify issues from an SRE perspective?

**Paragraph Answer:**
I monitor SLIs, SLOs, and error budgets, analyze alerts, check dashboards, and perform root cause analysis to ensure system reliability.

---

## 43. Explain what you have worked on in AWS CloudWatch.

**Paragraph Answer:**
I created alarms for CPU, memory, and disk usage, configured dashboards, set up log groups, and integrated SNS for alert notifications.

---

# 📌 Python Scripting

## 44. Difference between list and tuple in Python?

**Paragraph Answer:**
Lists are mutable and can be modified, while tuples are immutable and cannot be changed after creation.

---

## 45. Create a list and replace the value at second position.

```
my_list = [10, 20, 30]
my_list[1] = 50
print(my_list)
```

---

## 46. Write a Python function that prints: "Hello, please enter your name"

```
def greet():
    print("Hello, please enter your name")
```

---

## 47. Write a Python script to notify when a server goes down.

```
import os
import smtplib

response = os.system("ping -c 1 google.com")
if response != 0:
    print("Server is down!")
```

---

## 48. Write a Python script to list first 10 S3 buckets in AWS.

```
import boto3

s3 = boto3.client('s3')
buckets = s3.list_buckets()['Buckets']
for bucket in buckets[:10]:
    print(bucket['Name'])
```

---

# 📌 Bash & Linux

## 49. Write a simple Bash script used in your project.

```
#!/bin/bash
DATE=$(date)
echo "Deployment started at $DATE"
```

---

## 50. What is the sed command used for?

**Paragraph Answer:**
The sed (Stream Editor) command is used for searching, replacing, and editing text in files programmatically.

---

# ✅ End of Document

This file can be used for interview preparation including basic to advanced DevOps Questions.
