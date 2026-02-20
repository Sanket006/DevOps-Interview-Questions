# 🚀 Advanced Kubernetes Troubleshooting STAR Scenarios

Now we’re entering advanced Kubernetes troubleshooting scenarios — this is where real DevOps engineers prove their skills.

These are production-level STAR answers that interviewers love.

---

## ⭐ Scenario 1: Pods in CrashLoopBackOff

### ❓ Interview Question:

“Tell me about a time when pods were crashing repeatedly.”

4

### ✅ STAR Answer

**S – Situation:**
In our production cluster running on Kubernetes, newly deployed pods started showing CrashLoopBackOff status after a release.

**T – Task:**
As the DevOps engineer, I needed to quickly identify the root cause and restore application availability.

**A – Action:**

* Ran kubectl get pods to identify affected pods.
* Used kubectl describe pod to check events.
* Checked logs using kubectl logs.
* Found missing environment variable for database connection.
* Updated ConfigMap and Secret.
* Redeployed using rolling update.
* Added readiness and liveness probes to prevent bad deployments in future.

**R – Result:**
Pods stabilized within 10 minutes and downtime was minimized. After implementing validation checks, similar issues were prevented in future deployments.

---

## ⭐ Scenario 2: Node Not Ready

### ❓ Interview Question:

“What would you do if a Kubernetes node becomes NotReady?”

4

### ✅ STAR Answer

**S – Situation:**
One worker node in our cluster suddenly showed NotReady status.

**T – Task:**
Ensure workloads are not impacted and restore node health.

**A – Action:**

* Ran kubectl get nodes to confirm status.
* Drained the node safely using kubectl drain.
* Checked kubelet service logs on the node.
* Found disk space exhaustion.
* Cleaned unused Docker images.
* Added monitoring alert using Prometheus.
* Configured auto node replacement in cloud provider.

**R – Result:**
Workloads were rescheduled automatically, and no user impact occurred.

---

## ⭐ Scenario 3: High CPU Usage in Pods

### ❓ Interview Question:

“Describe a time when pods were consuming high CPU.”

4

### ✅ STAR Answer

**S – Situation:**
During peak traffic, application pods were consuming 95% CPU, causing slow response times.

**T – Task:**
Prevent performance degradation and ensure scalability.

**A – Action:**

* Ran kubectl top pods to verify resource usage.
* Checked metrics in Grafana.
* Identified missing resource limits in deployment YAML.
* Set CPU/memory requests and limits.
* Enabled Horizontal Pod Autoscaler (HPA).
* Optimized application caching.

**R – Result:**
CPU stabilized below 60% and application handled peak traffic smoothly.

---

## ⭐ Scenario 4: Service Not Accessible

### ❓ Interview Question:

“Users cannot access the application even though pods are running. What would you do?”

4

### ✅ STAR Answer

**S – Situation:**
Pods were running fine, but users reported that the application was not accessible externally.

**T – Task:**
Identify networking misconfiguration.

**A – Action:**

* Checked service using kubectl get svc.
* Found Service type incorrectly set to ClusterIP.
* Changed it to LoadBalancer.
* Verified endpoints using kubectl describe svc.
* Checked Ingress controller configuration.
* Validated security group rules in cloud provider.

**R – Result:**
Application became accessible within minutes and no further connectivity issues occurred.

---

## ⭐ Scenario 5: Failed Rolling Update

### ❓ Interview Question:

“Have you handled a failed rolling deployment?”

4

### ✅ STAR Answer

**S – Situation:**
A new version deployment caused application errors during rolling update.

**T – Task:**
Prevent downtime and revert to stable version.

**A – Action:**

* Checked rollout status using kubectl rollout status.
* Identified failing pods due to configuration error.
* Rolled back using kubectl rollout undo.
* Implemented canary deployment strategy.
* Added automated pre-deployment testing in CI pipeline.

**R – Result:**
Rollback completed in under 5 minutes, preventing major user impact. Future deployments became safer.

---

# 🔥 Advanced Interview Strategy for Kubernetes Questions

When answering:

* Mention debugging commands (kubectl logs, describe, top)
* Mention monitoring tools (Prometheus, Grafana)
* Mention scalability (HPA)
* Mention high availability
* Mention rollback strategy
* Always end with measurable result
