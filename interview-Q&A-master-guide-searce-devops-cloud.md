# 🚀 Searce DevOps / Cloud Engineer – Final Interview Master Q&A Guide

## Candidate: Sanket Ajay Chopade

This document contains topic-specific technical questions with well-structured answers and scenario-based answers using the STAR method to help crack the interview.

---

# ☁️ GOOGLE CLOUD PLATFORM (GCP)

## 1️⃣ Explain GCP Resource Hierarchy

GCP follows a structured hierarchy:
Organization → Folder → Project → Resources

* Organization: Root node linked to company domain
* Folder: Used to group projects logically
* Project: Logical container for resources
* Resources: VM, Storage, Database, etc.

IAM policies are inherited downward, ensuring centralized access control.

---

## 2️⃣ Difference Between GCE, GKE, and Cloud Run

GCE (Compute Engine):

* Provides virtual machines
* Suitable for lift-and-shift workloads

GKE (Google Kubernetes Engine):

* Managed Kubernetes service
* Best for microservices architecture

Cloud Run:

* Fully managed serverless container platform
* Scales automatically including scale-to-zero

---

## 3️⃣ How Do You Design Highly Available Architecture in GCP?

To ensure high availability:

* Deploy across multiple zones
* Use Regional Managed Instance Groups
* Use HTTP(S) Load Balancer
* Enable auto-scaling
* Use Cloud SQL with HA configuration
* Configure monitoring and alerts

---

## 4️⃣ What is IAM and Best Practice?

IAM controls access to resources.

Best Practices:

* Principle of least privilege
* Use predefined roles over primitive roles
* Avoid using Owner role unnecessarily
* Enable audit logging

---

# ☸️ KUBERNETES

## 5️⃣ Explain Kubernetes Architecture

Control Plane:

* API Server
* Scheduler
* Controller Manager
* etcd

Worker Node:

* Kubelet
* Kube-proxy
* Container runtime

Kubernetes ensures self-healing, scaling, and rolling updates.

---

## 6️⃣ What is the Difference Between Deployment and StatefulSet?

Deployment:

* Stateless apps
* Pods are interchangeable

StatefulSet:

* Stateful apps
* Stable identity
* Ordered scaling

---

## 7️⃣ What is Ingress?

Ingress manages external access to services.
Provides:

* URL-based routing
* SSL termination
* Load balancing

---

## 8️⃣ What is HPA?

Horizontal Pod Autoscaler automatically scales pods based on CPU or custom metrics.

---

# 🐳 DOCKER

## 9️⃣ Explain Docker Architecture

Docker uses:

* Docker Client
* Docker Daemon
* Docker Images
* Containers
* Docker Registry

Client communicates with daemon to build and run containers.

---

## 🔟 CMD vs ENTRYPOINT

CMD: Provides default arguments.
ENTRYPOINT: Defines main executable.

ENTRYPOINT + CMD allows parameterized containers.

---

# 🏗️ TERRAFORM

## 1️⃣1️⃣ What is Terraform State File?

Stores infrastructure metadata.

Used for:

* Tracking resources
* Updating infrastructure
* Destroy operations

Best Practice: Use remote backend with state locking.

---

## 1️⃣2️⃣ What are Terraform Modules?

Reusable Terraform configurations.

Benefits:

* Code reusability
* Standardization
* Maintainability

---

# 🚀 CI/CD

## 1️⃣3️⃣ Explain CI/CD Pipeline Clearly

Pipeline Flow:

1. Code push to Git
2. Trigger pipeline
3. Build application
4. Run tests
5. Security scan
6. Build Docker image
7. Push image to registry
8. Deploy to Kubernetes
9. Monitor application

Goal: Fast, reliable, automated delivery.

---

## 1️⃣4️⃣ Continuous Delivery vs Continuous Deployment

Continuous Delivery:

* Manual approval before production

Continuous Deployment:

* Fully automated production release

---

# 🔐 SECURITY

## 1️⃣5️⃣ How Do You Secure Kubernetes Cluster?

* RBAC enabled
* Use namespaces
* Restrict API server access
* Enable network policies
* Use secrets management
* Enable audit logging

---

# 🧠 SCENARIO-BASED QUESTIONS (STAR METHOD)

---

## Scenario 1️⃣: Production CPU Usage 95%

Situation:
Production application CPU usage spiked to 95%, causing slow response times.

Task:
Ensure application stability and minimize downtime.

Action:

* Checked monitoring dashboard.
* Identified high-traffic spike.
* Scaled horizontally using auto-scaling.
* Optimized inefficient queries.

Result:
CPU reduced to 55%, response time improved, and system stabilized without downtime.

---

## Scenario 2️⃣: Kubernetes Pod CrashLoopBackOff

Situation:
Application pods were continuously restarting in production.

Task:
Identify root cause and restore service.

Action:

* Checked logs using kubectl logs.
* Described pod using kubectl describe.
* Found misconfigured environment variable.
* Updated deployment and redeployed.

Result:
Pods stabilized and application became accessible within minutes.

---

## Scenario 3️⃣: CI/CD Pipeline Failure

Situation:
Deployment failed in production pipeline.

Task:
Restore deployment pipeline.

Action:

* Checked pipeline logs.
* Found Docker image build error.
* Fixed dependency version issue.
* Re-ran pipeline.

Result:
Pipeline completed successfully and deployment resumed.

---

## Scenario 4️⃣: On-Prem to GCP Migration

Situation:
Legacy application needed migration to cloud.

Task:
Migrate application with minimal downtime.

Action:

* Assessed dependencies.
* Designed VPC and IAM roles.
* Migrated database using migration tools.
* Deployed application on GKE.
* Configured monitoring.

Result:
Application successfully migrated with improved scalability and reliability.

---

# 👤 HR QUESTIONS

## Tell Me About Yourself

I am Sanket Ajay Chopade, a DevOps and Cloud Engineer with hands-on experience in AWS, GCP fundamentals, Docker, Kubernetes, Terraform, Git, and CI/CD pipelines. I enjoy automating infrastructure and building scalable cloud-native solutions. My goal is to contribute to enterprise cloud transformation projects and continuously improve system reliability and efficiency.

---

## Why Searce?

Searce is a leading Google Cloud partner focused on cloud-native engineering and automation. I want to work in an environment where I can contribute to real cloud transformation projects while strengthening my GCP and DevOps expertise.

---

# 🏁 FINAL INTERVIEW STRATEGY

* Answer structurally
* Think in terms of scalability and reliability
* Emphasize automation
* Show troubleshooting mindset
* Speak confidently and clearly
