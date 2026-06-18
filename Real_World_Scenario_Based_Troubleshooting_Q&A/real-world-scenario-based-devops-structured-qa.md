# DevOps Interview Answers (Structured)

---

## 1. What exactly happens when a Kubernetes pod gets OOMKilled?

**Type:** Technical Concept

**Answer:**

**Definition:** OOMKilled happens when a container exceeds its memory limit and the Linux OOM (Out Of Memory) killer terminates it.

**How it Works:** Kubernetes enforces memory limits via cgroups. When memory usage exceeds the limit, the kernel selects a process (usually inside the container) to kill. Kubernetes then marks the container as OOMKilled and restarts it based on restartPolicy.

**Example:** A Java app with memory leak exceeds its 512Mi limit → container killed → pod restarts repeatedly (CrashLoopBackOff).

---

## 2. Your CI/CD pipeline works in staging but fails in production with permission denied - how do you debug?

**Type:** Troubleshooting

**Answer:**

**Identify:** Error: "permission denied" only in production.

**Diagnose:** Compare IAM roles, service accounts, file permissions, secrets, environment variables between staging and prod. Check logs and audit trails.

**Fix:** Update permissions (IAM policy, RBAC, file perms), ensure correct credentials are used.

**Prevent:** Use environment parity, automated policy validation, least privilege with testing.

---

## 3. How does Kubernetes DNS work across namespaces, and what can break it?

**Type:** Technical Concept

**Answer:**

**Definition:** Kubernetes DNS resolves service names to cluster IPs using CoreDNS.

**How it Works:** Services are accessible via FQDN like `service.namespace.svc.cluster.local`. Pods query CoreDNS via kube-dns service.

**Example:** `db.default.svc.cluster.local` resolves to DB service IP. Failures: CoreDNS crash, wrong namespace, network policies, misconfigured resolv.conf.

---

## 4. Terraform state lock is stuck - how do you recover safely?

**Type:** Troubleshooting

**Answer:**

**Identify:** Terraform shows "state locked" error.

**Diagnose:** Check backend (S3 + DynamoDB, etc.) for active lock, confirm no running process.

**Fix:** Manually remove lock (e.g., delete DynamoDB 
lock entry or use `terraform force-unlock`).

**Prevent:** Use proper state backend, avoid abrupt termination, enable locking and retries.

---

## 5. How do you design observability (metrics, logs, traces) for high-scale systems?

**Type:** System Design

**Answer:**

**Requirements:** High throughput, low latency, real-time insights, cost efficiency.

**Architecture:** Use Prometheus (metrics), ELK/EFK 
(logs), Jaeger/Tempo (traces). Centralized pipeline with agents (Fluentd, OpenTelemetry).

**Tools:** Prometheus, Grafana, Loki, Jaeger, OpenTelemetry.

**Scaling:** Sharding, sampling, retention policies, tiered storage.

---

## 6. A secret was exposed in GitHub - what’s your incident response plan?

**Type:** Troubleshooting / Behavioral Hybrid

**Answer:**

**Identify:** Secret committed publicly.

**Diagnose:** Determine scope, access logs, affected systems.

**Fix:** Revoke/rotate credentials immediately, remove secret from repo history.

**Prevent:** Use secret scanning, vaults, pre-commit hooks, avoid hardcoding.

---

## 7. How does HPA work internally in Kubernetes?

**Type:** Technical Concept

**Answer:**

**Definition:** Horizontal Pod Autoscaler scales pods 
based on metrics.

**How it Works:** HPA queries metrics server (CPU/memory/custom), compares with target, calculates desired replicas using formula, updates deployment.

**Example:** CPU target 50%, current 80% → scale up replicas.

---

## 8. What is error budget & burn rate in SRE?

**Type:** Technical Concept

**Answer:**

**Definition:** Error budget = allowed failure based on SLO. Burn rate = how fast budget is consumed.

**How it Works:** If SLO is 99.9%, 0.1% downtime allowed. Burn rate tracks consumption over time.

**Example:** High burn rate → alerts → slow deployments.

---

## 9. Your service has latency spikes every 60 seconds - how do you debug?

**Type:** Troubleshooting

**Answer:**

**Identify:** Periodic latency spike every 60s.

**Diagnose:** Check cron jobs, GC cycles, autoscaling events, metrics scraping intervals.

**Fix:** Optimize or reschedule jobs, tune GC, adjust scraping.

**Prevent:** Add monitoring, remove synchronized workloads.

---

## 10. Push vs Pull CD - how does GitOps (ArgoCD) actually work?

**Type:** Technical Concept

**Answer:**

**Definition:** Push CD deploys via pipeline; Pull CD (GitOps) uses agents that sync desired state from Git.

**How it Works:** ArgoCD watches Git repo, compares desired vs actual state, applies changes via Kubernetes API.

**Example:** Update YAML in Git → ArgoCD sync → cluster updated.

---

## 11. What are Dockerfile best practices for security and performance?

**Type:** Technical Concept

**Answer:**

**Definition:** Guidelines to build efficient and secure container images.

**How it Works:** Use small base images, multi-stage builds, non-root user, minimal layers.

**Example:** Alpine base + multi-stage reduces image size and attack surface.

---

## 12. What is Platform Engineering and how would you design an IDP?

**Type:** System Design

**Answer:**

**Requirements:** Developer productivity, self-service, scalability.

**Architecture:** Internal Developer Platform with 
APIs, templates, CI/CD, observability, RBAC.

**Tools:** Backstage, Kubernetes, Terraform, ArgoCD.

**Scaling:** Modular services, golden paths, automation.

---
