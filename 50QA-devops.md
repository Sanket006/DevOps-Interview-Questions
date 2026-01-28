# DevOps Interview Questions & Answers (Fresher–Junior Level)

This README contains **all provided DevOps interview questions and answers**, organized clearly for **fresher to junior DevOps engineer interviews** (Accenture, TCS, Infosys, startups). **No content has been skipped, deleted, or altered** — only formatted properly for readability.

---

## Part 1: Core DevOps Interview Questions (1–30)

### 1. What is DevOps?

DevOps is a set of practices that combines development (Dev) and operations (Ops) to shorten the software development lifecycle. It focuses on automation, continuous integration, continuous delivery, monitoring, and collaboration to deliver reliable software faster.

### 2. Why is DevOps important?

DevOps improves deployment speed, reliability, and collaboration. It reduces manual errors through automation, enables faster feedback, and helps teams respond quickly to customer needs while maintaining system stability.

### 3. Explain CI/CD.

CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. CI ensures code is frequently merged and tested automatically. CD automates the process of delivering tested code to production or staging environments.

### 4. What tools are commonly used in DevOps?

Common tools include Git/GitHub (version control), Jenkins/GitHub Actions (CI/CD), Docker (containerization), Kubernetes (orchestration), Terraform (IaC), AWS/Azure/GCP (cloud), and Prometheus/Grafana (monitoring).

### 5. What is Infrastructure as Code (IaC)?

IaC is managing infrastructure using code instead of manual setup. Tools like Terraform or CloudFormation allow version control, repeatability, and automation of infrastructure provisioning.

### 6. What is Git and why is it important?

Git is a distributed version control system that tracks code changes. It enables collaboration, rollback, branching, and merging, which are essential in DevOps workflows.

### 7. What is Docker?

Docker is a containerization platform that packages applications with dependencies into containers. Containers are lightweight, portable, and ensure consistency across environments.

### 8. Difference between Docker container and VM?

Containers share the host OS kernel and are lightweight, while VMs include a full OS. Containers start faster and consume fewer resources compared to VMs.

### 9. What is Kubernetes?

Kubernetes is a container orchestration platform that manages deployment, scaling, load balancing, and self-healing of containerized applications.

### 10. What is a Kubernetes Pod?

A Pod is the smallest deployable unit in Kubernetes. It can contain one or more containers that share networking and storage.

### 11. What is Jenkins?

Jenkins is an open-source automation server used for building CI/CD pipelines. It supports plugins for building, testing, and deploying applications.

### 12. What is a Jenkins pipeline?

A Jenkins pipeline is a defined workflow written in code (Declarative or Scripted) that automates steps like build, test, and deploy.

### 13. What is Blue-Green Deployment?

Blue-Green deployment maintains two environments: Blue (current) and Green (new). Traffic is switched after testing, allowing zero downtime and easy rollback.

### 14. What is Canary Deployment?

Canary deployment releases a new version to a small subset of users first. If it works well, traffic is gradually increased to all users.

### 15. What is AWS EC2?

EC2 is a virtual server service in AWS that allows users to run applications with configurable compute capacity.

### 16. What is Auto Scaling?

Auto Scaling automatically adds or removes EC2 instances based on load, ensuring high availability and cost optimization.

### 17. What is Load Balancer?

A Load Balancer distributes incoming traffic across multiple servers to improve availability, fault tolerance, and performance.

### 18. Difference between ALB and NLB?

* **ALB** works at Layer 7 (HTTP/HTTPS) and supports routing rules.
* **NLB** works at Layer 4 and handles high-performance, low-latency traffic.

### 19. What is Linux in DevOps?

Linux is the primary OS for DevOps due to stability, security, and automation support. DevOps engineers manage services, processes, permissions, and logs in Linux.

### 20. What is a process in Linux?

A process is a running instance of a program. Linux provides tools like `ps`, `top`, and `kill` to manage processes.

### 21. What is Monitoring?

Monitoring tracks system metrics like CPU, memory, disk, and application health. Tools like Prometheus and CloudWatch help detect issues early.

### 22. What is Logging?

Logging records system and application events. Logs help in debugging, auditing, and root cause analysis.

### 23. What is Prometheus?

Prometheus is an open-source monitoring tool that collects metrics and provides alerting based on thresholds.

### 24. What is Grafana?

Grafana is a visualization tool used to create dashboards from monitoring data sources like Prometheus.

### 25. What is Terraform?

Terraform is an IaC tool used to provision and manage infrastructure across multiple cloud providers using declarative configuration files.

### 26. What is a Terraform state file?

The state file keeps track of real infrastructure and Terraform configurations to detect changes and manage resources correctly.

### 27. What is DevSecOps?

DevSecOps integrates security into DevOps by adding security checks in CI/CD pipelines, ensuring secure code and infrastructure.

### 28. What is Configuration Management?

Configuration management ensures systems are consistently configured. Tools like Ansible manage packages, services, and settings.

### 29. What is Ansible?

Ansible is an agentless automation tool used for configuration management, deployment, and orchestration using YAML playbooks.

### 30. How do you handle failures in production?

Failures are handled using monitoring, alerts, rollbacks, auto-scaling, and incident response. Root cause analysis is done to prevent future issues.

---

## Part 2: Additional DevOps Interview Questions (51–70)

### 51. What is Continuous Integration?

Continuous Integration (CI) is the practice of frequently merging code changes into a shared repository. Each merge triggers automated builds and tests to detect issues early and maintain code quality.

### 52. What is Continuous Delivery vs Continuous Deployment?

Continuous Delivery ensures code is always ready for release but requires manual approval. Continuous Deployment automatically releases every successful change to production without manual intervention.

### 53. What is a Dockerfile?

A Dockerfile is a text file containing instructions to build a Docker image. It defines the base image, dependencies, environment variables, and startup commands.

### 54. What is a Docker image?

A Docker image is a read-only template used to create containers. It includes application code, libraries, and dependencies.

### 55. What is a Kubernetes Deployment?

A Deployment manages Pods and ReplicaSets, enabling scaling, rolling updates, and rollbacks for applications.

### 56. What is a ReplicaSet?

A ReplicaSet ensures a specified number of Pod replicas are always running, improving availability and fault tolerance.

### 57. What is a Kubernetes Service?

A Service exposes Pods internally or externally and provides stable networking despite Pod restarts.

### 58. What is ConfigMap?

A ConfigMap stores non-sensitive configuration data such as environment variables or config files used by Pods.

### 59. What is a Secret in Kubernetes?

Secrets store sensitive data like passwords, tokens, or keys securely and inject them into Pods when needed.

### 60. What is AWS IAM?

AWS IAM manages users, roles, and permissions, enabling secure access to AWS resources using least privilege.

### 61. What is an IAM Role?

An IAM Role is an identity with permissions that AWS services or users can assume without long-term credentials.

### 62. What is Amazon S3?

Amazon S3 is an object storage service used to store backups, logs, static websites, and artifacts.

### 63. What is Amazon RDS?

Amazon RDS is a managed relational database service that handles backups, patching, and scaling automatically.

### 64. What is a VPC?

A Virtual Private Cloud is an isolated network in AWS where resources are securely deployed and controlled.

### 65. What is a subnet?

A subnet is a segmented network inside a VPC that organizes resources and improves security.

### 66. What is Security Group?

A Security Group acts as a virtual firewall controlling inbound and outbound traffic at the instance level.

### 67. What is NACL?

Network ACL controls traffic at the subnet level and supports both allow and deny rules.

### 68. Difference between Security Group and NACL?

Security Groups are stateful and instance-level, while NACLs are stateless and subnet-level.

### 69. What is CloudWatch?

CloudWatch is AWS’s monitoring service that collects logs, metrics, and sets alarms.

### 70. What is Backup and Disaster Recovery?

It is a strategy to protect data and recover systems after failures using backups, replication, and failover.

---

✅ **Use this README for interview revision, GitHub repositories, or DevOps notes.**