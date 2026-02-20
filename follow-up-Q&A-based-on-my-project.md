# Enterprise E-Commerce Platform – DevOps Project
## Project Overview

**ApnaKart** is an enterprise-level, scalable e-commerce platform developed for **Nova Technologies**. The platform supports multiple categories including fashion, electronics, groceries, and daily essentials. It was designed to handle high traffic, frequent deployments, and long-term business growth, with a strong focus on scalability, reliability, and maintainability.

---

## Team & Development Model

* **Team Size:** 15 members
* **Methodology:** Agile (3-week sprints)
* **Team Composition:**

  * 3 Frontend Developers
  * 6 Backend Developers
  * 2 QA Engineers
  * 1 UI/UX Designer
  * 1 Product Owner
  * 1 Project Manager
  * **1 DevOps Engineer (My Role – End-to-End Ownership)**

JIRA was used for sprint planning, task tracking, defect management, and release coordination.

---

## Architecture & Technology Stack

### Microservices Architecture

The application follows a **microservices-based architecture** to enable independent deployments, horizontal scalability, and high availability.

* **Frontend:**

  * React-based applications
  * Hosted on **Amazon S3**

* **Backend:**

  * 10 microservices developed using **Java & Spring Boot**
  * Deployed on **Amazon EKS (Kubernetes)**

### Core Microservices

* Authentication
* Product Catalog
* Inventory
* Cart
* Order Management
* Payments
* Shipping
* Notifications

### Database Strategy

* **Database-per-Service Pattern**
* Each microservice owns its own **MySQL database**
* Databases hosted on **Amazon RDS**
* Ensures loose coupling, fault isolation, and independent scaling

---

## CI/CD Pipeline & Automation

A fully automated CI/CD pipeline was implemented using **Jenkins**.

### Pipeline Flow

1. Code push to **GitHub** triggers Jenkins
2. Application build using **Maven**
3. Code quality analysis with **SonarQube**
4. Docker image creation
5. Image pushed to container registry
6. Deployment to **EKS** cluster

This automation significantly reduced manual effort, improved release frequency, and minimized deployment errors.

---

## Infrastructure as Code (IaC)

All cloud infrastructure was provisioned using **Terraform**, following IaC best practices.

### AWS Resources Managed

* Amazon EKS
* Amazon RDS
* IAM roles & policies
* VPC, subnets, routing
* Security groups & networking

This ensured reproducibility, version control, and easy environment creation.

---

## Monitoring, Scaling & Reliability

* **Monitoring Tool:** Datadog

  * Application performance metrics
  * Container and infrastructure health
  * Alerting and incident detection

* **Kubernetes Capabilities:**

  * Auto-healing
  * Horizontal Pod Autoscaler (HPA)
  * High availability deployments

---

## Git Branching & Environment Strategy

An environment-based branching model was used:

**dev → test → uat → prod**

An additional **hotfix** branch was used for urgent production issues.

### Branch Details

* **Dev:**

  * Feature development
  * Pull requests, code reviews, CI validation, SonarQube checks

* **Test:**

  * QA testing (manual & automated)
  * Defect tracking in JIRA

* **UAT:**

  * Business and client validation
  * Production-like environment

* **Prod:**

  * Live environment
  * Strict access control
  * Deployments only via approved CI/CD pipelines

* **Hotfix:**

  * Created from prod
  * Merged back into dev and test after fix

Each branch was mapped to a **separate CI/CD pipeline and Kubernetes namespace**, ensuring isolation, safer deployments, and quick rollbacks.

---

## My Role & Responsibilities (DevOps Engineer)

I owned the DevOps lifecycle end-to-end.

### Key Responsibilities

* Designed AWS architecture with EKS, RDS, S3
* Provisioned infrastructure using Terraform
* Built and managed Jenkins CI/CD pipelines
* Integrated GitHub, Maven, Docker, SonarQube, AWS, Kubernetes
* Dockerized all backend microservices
* Managed Kubernetes resources:

  * Deployments
  * Services
  * Ingress
  * HPA
  * ConfigMaps & Secrets
* Worked with Helm charts, RBAC, and Ingress Controllers
* Managed GitHub access controls and branch protection rules
* Configured IAM permissions
* Monitored production using Datadog
* Handled production incidents and root cause analysis
* Created documentation, runbooks, and optimized pipelines
* Collaborated closely with developers and QA teams

---

## Final Summary

In this project, I acted as a bridge between development and operations, ensuring:

* Smooth code promotion across environments
* Automated, reliable CI/CD pipelines
* High availability and scalability
* Secure and stable production systems

This project provided strong hands-on experience with **AWS, Kubernetes, Terraform, CI/CD, and real-world DevOps practices in a production-grade enterprise system**.

---

# ApnaKart – Interview Follow-Up Questions & Answers  
*(Enterprise E-Commerce Platform – DevOps Project)*

This document contains commonly asked **interview follow-up questions and answers** based on my **ApnaKart enterprise-level e-commerce project**. These questions are typically asked after a high-level project explanation and focus on **architecture, DevOps practices, AWS, Kubernetes, CI/CD, and production support**.

---

## 📌 Project Overview

**ApnaKart** is a scalable, multi-category enterprise e-commerce platform developed for **Nova Technologies**.  
It supports fashion, electronics, groceries, and daily essentials and is designed to handle **high traffic, frequent deployments, and business growth**.

- Architecture: Microservices
- Cloud: AWS
- Container Orchestration: Kubernetes (EKS)
- CI/CD: Jenkins
- IaC: Terraform
- Monitoring: Datadog
- Team Size: 15 members
- Role: **Sole DevOps Engineer (End-to-End Ownership)**

---

## 🏗 Architecture & Design – Follow-Ups

### Q1. Why did you choose microservices over monolithic architecture?
**Answer:**  
Microservices allow independent deployments, better scalability, and fault isolation. Since ApnaKart had multiple business domains like orders, payments, inventory, and shipping, microservices helped teams work independently and reduced the impact of failures.

---

### Q2. Why did each microservice have its own database?
**Answer:**  
We followed the database-per-service pattern to avoid tight coupling. This ensured data ownership, independent scaling, safer schema changes, and better fault isolation between services.

---

### Q3. Why did you choose Amazon EKS instead of EC2-based deployments?
**Answer:**  
EKS provides auto-healing, auto-scaling, rolling deployments, and better container orchestration. Managing multiple microservices manually on EC2 would have been complex and error-prone.

---

## 🔄 CI/CD Pipeline – Follow-Ups

### Q4. How did you ensure code quality in the pipeline?
**Answer:**  
We integrated SonarQube with Jenkins. Every pull request triggered static code analysis, and builds failed if quality gates were not met, ensuring clean and maintainable code.

---

### Q5. How did you handle failed deployments?
**Answer:**  
We used Kubernetes rolling updates with readiness and liveness probes. If a deployment failed, Kubernetes automatically stopped the rollout and retained the previous stable version.

---

### Q6. How did you manage environment separation?
**Answer:**  
Each Git branch (dev, test, uat, prod) mapped to a separate CI/CD pipeline and Kubernetes namespace, ensuring complete isolation and safer deployments.

---

## ☸ Kubernetes – Follow-Ups

### Q7. How did you handle traffic spikes?
**Answer:**  
We configured Horizontal Pod Autoscaler (HPA) based on CPU and memory metrics. Kubernetes automatically scaled pods during peak traffic and scaled down when traffic normalized.

---

### Q8. How did services communicate securely?
**Answer:**  
Services communicated via Kubernetes Services and internal DNS. Sensitive data was stored using Kubernetes Secrets, and RBAC ensured least-privilege access.

---

### Q9. Why did you use Ingress instead of LoadBalancer services?
**Answer:**  
Ingress allowed centralized routing, SSL termination, and cost optimization by exposing multiple services through a single entry point.

---

## ☁ AWS & Infrastructure – Follow-Ups

### Q10. Why did you use Terraform?
**Answer:**  
Terraform enabled Infrastructure as Code, ensuring consistency, repeatability, version control, and quick environment recreation with minimal manual errors.

---

### Q11. How did you ensure database high availability?
**Answer:**  
We used Amazon RDS with Multi-AZ deployments, enabling automatic failover and minimal downtime.

---

### Q12. How did you manage secrets and access control?
**Answer:**  
IAM roles were used for AWS access, Kubernetes Secrets for credentials, RBAC for cluster permissions, and branch protection rules for GitHub.

---

## 📊 Monitoring & Production Support – Follow-Ups

### Q13. How did you monitor production systems?
**Answer:**  
We used Datadog to monitor application performance, container health, and infrastructure metrics. Alerts were configured for latency, CPU spikes, and pod failures.

---

### Q14. Describe a production issue you handled.
**Answer:**  
During peak traffic, order service latency increased. Datadog showed CPU saturation. I increased HPA limits and optimized resource requests, stabilizing performance without downtime.

---

## 👨‍💻 Role & Responsibilities – Follow-Ups

### Q15. How did you manage as the only DevOps engineer?
**Answer:**  
I focused on automation, reusable pipelines, Terraform modules, Helm charts, and clear documentation to reduce manual effort and ensure smooth collaboration.

“Yes, I was the only DevOps engineer on the project. That was possible because we focused heavily on automation from day one.

All infrastructure was created using Terraform, deployments were fully automated using Jenkins, and Kubernetes handled auto-healing and auto-scaling. Because of this, manual intervention was minimal, and a single DevOps engineer was sufficient to manage the environments efficiently.”

---

### Q16. How did you collaborate with developers and QA?
**Answer:**  
I worked closely during sprint planning, supported deployments, debugged issues, optimized pipelines, and ensured test environments closely matched production.

---

## 🎯 Final Interview Question

### Q17. What did this project teach you as a DevOps engineer?
**Answer:**  
This project gave me hands-on experience managing production-grade systems, ensuring scalability, automating deployments, and owning system reliability end-to-end.

---

## ✅ Key Takeaways

- Enterprise-level DevOps experience
- End-to-end ownership of infrastructure and CI/CD
- Strong AWS, Kubernetes, Terraform, and monitoring skills
- Real-world production incident handling

---

📌 *This document can be used for interview preparation, GitHub portfolio, or project explanation during DevOps interviews.*
