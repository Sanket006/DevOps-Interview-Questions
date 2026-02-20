# 🚀 Advanced DevOps & Cloud Engineer Interview Q&A

## Target Company: Searce

## Candidate: Sanket Ajay Chopade

---

# ☁️ Google Cloud (GCP) – Advanced Questions

## 1️⃣ What is the difference between IAM Roles and IAM Policies in GCP?

IAM Policy is a collection of role bindings attached to a resource. It defines who (member) has what access (role) on which resource.

IAM Role is a collection of permissions. Roles can be:

* Primitive (Owner, Editor, Viewer)
* Predefined (e.g., Compute Admin)
* Custom roles

A policy binds a member to a role.

---

## 2️⃣ How does auto-scaling work in GCP?

In GCP, auto-scaling is commonly configured using Managed Instance Groups (MIGs).

Steps:

* Create instance template
* Create Managed Instance Group
* Enable auto-scaling policy
* Define metrics (CPU utilization, load balancing capacity, etc.)

The system automatically adds or removes instances based on traffic.

---

## 3️⃣ What is the difference between Regional and Zonal resources in GCP?

Zonal resources exist in a single zone (e.g., VM instance).
Regional resources span multiple zones within a region (e.g., Regional managed instance group).

Regional resources provide higher availability.

---

## 4️⃣ What is VPC in GCP and how is it different from AWS VPC?

GCP VPC is a global resource, meaning subnets can exist in different regions under the same VPC.

In AWS, VPC is regional.

GCP allows centralized networking with global routing.

---

# 🐳 Docker – Advanced Questions

## 5️⃣ What is the difference between CMD and ENTRYPOINT in Docker?

CMD provides default arguments to the container.
ENTRYPOINT defines the main executable.

If both are used:
ENTRYPOINT defines the command
CMD provides default parameters

---

## 6️⃣ How do you reduce Docker image size?

Best practices:

* Use minimal base images (e.g., alpine)
* Use multi-stage builds
* Remove unnecessary files
* Use .dockerignore
* Combine RUN commands

---

# ☸️ Kubernetes – Advanced Questions

## 7️⃣ What is a Pod in Kubernetes?

A Pod is the smallest deployable unit in Kubernetes. It can contain one or more containers sharing:

* Network namespace
* Storage volumes

Pods are ephemeral and managed by controllers like Deployments.

---

## 8️⃣ What is the difference between Deployment and StatefulSet?

Deployment:

* Used for stateless applications
* Pods are interchangeable

StatefulSet:

* Used for stateful applications
* Maintains stable network identity
* Ordered deployment and scaling

---

## 9️⃣ What is Ingress in Kubernetes?

Ingress manages external HTTP/HTTPS access to services inside the cluster.

It provides:

* URL routing
* SSL termination
* Load balancing

---

## 🔟 What is HPA in Kubernetes?

Horizontal Pod Autoscaler (HPA) automatically scales pods based on metrics such as CPU utilization or custom metrics.

---

# 🏗️ Terraform – Advanced Questions

## 1️⃣1️⃣ What is Terraform Remote Backend?

A remote backend stores Terraform state file in a shared location.

Examples:

* GCS
* S3
* Terraform Cloud

Benefits:

* Collaboration
* State locking
* Security

---

## 1️⃣2️⃣ What are Terraform Modules?

Modules are reusable Terraform configurations.

Benefits:

* Code reusability
* Maintainability
* Standardization

---

# 🔐 Security & SRE

## 1️⃣3️⃣ What is Zero Trust Security?

Zero Trust assumes no implicit trust. Every request must be authenticated and authorized.

Principles:

* Verify identity
* Least privilege access
* Continuous monitoring

---

## 1️⃣4️⃣ What is Observability?

Observability is the ability to understand system state through:

* Metrics
* Logs
* Traces

Tools:

* Prometheus
* Grafana
* Cloud Monitoring

---

# 🔄 CI/CD & GitOps

## 1️⃣5️⃣ What is GitOps?

GitOps is a deployment methodology where Git is the single source of truth for infrastructure and application configuration.

Tools:

* ArgoCD
* Flux

Changes are automatically applied to clusters when code is merged.

---

## 1️⃣6️⃣ What is the difference between Continuous Delivery and Continuous Deployment?

Continuous Delivery:

* Manual approval before production deployment

Continuous Deployment:

* Automatic deployment to production without manual approval

---

# 🧠 Scenario-Based Questions

## 1️⃣7️⃣ Your Kubernetes pods are crashing repeatedly. What will you do?

Steps:

* Check pod logs (kubectl logs)
* Describe pod (kubectl describe pod)
* Check resource limits
* Verify environment variables
* Check liveness/readiness probes

---

## 1️⃣8️⃣ How would you migrate an on-prem application to GCP?

Steps:

* Assessment and dependency mapping
* Choose migration strategy (Rehost, Replatform, Refactor)
* Create VPC and networking
* Set up IAM
* Migrate data (Database migration tools)
* Deploy application (GCE/GKE/Cloud Run)
* Monitoring and optimization

---

# 👤 Behavioral Questions

## 1️⃣9️⃣ Describe a challenging DevOps issue you faced.

Explain a real issue such as pipeline failure, container crash, or networking issue. Structure answer using STAR method:
Situation → Task → Action → Result

---

## 2️⃣0️⃣ How do you handle production incidents?

* Identify issue
* Communicate with stakeholders
* Mitigate impact
* Perform root cause analysis
* Implement preventive measures

---

# 🏁 Final Advice for Searce Interview

Since Searce is heavily focused on Google Cloud and automation:

* Emphasize GCP knowledge
* Demonstrate Kubernetes understanding
* Explain CI/CD clearly
* Show troubleshooting mindset
* Talk about automation and scalability

---

# 🎯 Interview Mindset

Speak confidently.
Think architecturally.
Answer structurally.
Focus on reliability, scalability, and automation.

You are ready 🚀
