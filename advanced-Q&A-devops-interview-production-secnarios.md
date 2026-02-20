# DevOps Interview Scenarios – Production-Ready Answers

This document covers **commonly asked DevOps interview questions** and explains **what interviewers actually expect**. Each answer is written in a **clear, structured, and real-world production mindset**.

---

## 1. Pod is in `CrashLoopBackOff` – How Will You Debug?

### Step-by-step approach:

1. **Check pod details and events**

```bash
kubectl describe pod <pod-name>
```

* Look at the **Events** section
* Common causes:

  * Application crash
  * Missing configuration
  * Wrong image or command
  * Resource limits

2. **Check container logs**

```bash
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
```

* Helps identify application-level errors

3. **Verify configuration**

* ConfigMaps and Secrets
* Environment variables
* Correct image version

4. **Check resource usage**

```bash
kubectl top pod <pod-name>
```

* Container may be killed due to insufficient CPU or memory

5. **Reproduce locally (if required)**

* Run the same container locally to isolate issues

**Key Interview Signal:**

> I debug problems layer by layer — infrastructure, container, and then application.

---

## 2. CPU Usage Is 95% in Production – What’s Your Approach?

### Step-by-step approach:

1. **Identify the scope of the issue**

```bash
kubectl top pod
kubectl top node
```

* Is it a single pod, node, or the entire cluster?

2. **Analyze workload behavior**

* Sudden traffic spike
* Inefficient code or infinite loops
* Background jobs or batch processes

3. **Immediate mitigation**

* Scale the application

```bash
kubectl scale deployment <app-name> --replicas=5
```

* Enable or tune **Horizontal Pod Autoscaler (HPA)**

4. **Long-term optimization**

* Optimize application code
* Define proper CPU **requests and limits**
* Introduce caching or async processing

5. **Monitoring and alerting**

* Alerts at 70–80% CPU
* Avoid reacting only at critical levels

**Key Interview Signal:**

> I balance short-term stability with long-term performance optimization.

---

## 3. How Do You Design Zero-Downtime Deployment?

### Common strategies:

* Rolling Updates
* Blue-Green Deployment
* Canary Deployment

### Rolling Update (Kubernetes example):

```yaml
strategy:
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

### Key components:

* **Readiness probes** to control traffic
* **Liveness probes** to restart unhealthy containers
* Load balancer routes traffic only to healthy pods

### Database considerations:

* Backward-compatible schema changes
* Avoid breaking DB migrations

**Key Interview Signal:**

> Zero downtime is not just deployment — it includes application readiness and database safety.

---

## 4. How Do You Secure a CI/CD Pipeline?

### Source control security

* Branch protection rules
* Mandatory pull request reviews
* No direct commits to main branch

### Secrets management

* Never hardcode secrets
* Use:

  * GitHub Secrets
  * HashiCorp Vault
  * Cloud secrets manager

### Pipeline access control

* Least privilege IAM roles
* Separate roles for build, test, and deploy stages

### Image and dependency security

* Scan container images (Trivy, Snyk)
* Lock dependency versions

### Auditing and approvals

* Pipeline logs and audit trails
* Manual approval gates for production

**Key Interview Signal:**

> Security is integrated into CI/CD, not added after deployment.

---

## 5. What Interviewers Actually Look For

### 🔧 Problem-Solving Ability

* Real production troubleshooting
* Logical and calm approach

### 🏗️ Architecture Clarity

* End-to-end understanding:

  * VPC → Load Balancer → Compute → Kubernetes
  * CI/CD → Docker → Kubernetes → Monitoring

### ☁️ Cloud Fundamentals

* IAM roles over access keys
* Security groups and networking
* Auto Scaling and fault tolerance

### 🔁 CI/CD Understanding

* Pipeline stages
* Artifact flow
* Rollback strategies
* Failure handling

### 🐳 Container & Kubernetes Depth

* Deployment vs StatefulSet
* RBAC
* Image optimization
* Debugging tools

### 📊 Monitoring & Logging Mindset

* Metrics before incidents
* Logs for root cause analysis
* Proactive alerting

### 🧠 Decision-Making Skills

* Justification of architectural choices
* Trade-offs between cost, performance, and complexity

---

## Final Interview Tip

> I prioritize reliability first, then scalability, and finally optimization — because production stability is non-negotiable.

---

📌 **Use this README as a revision guide before DevOps interviews.**
