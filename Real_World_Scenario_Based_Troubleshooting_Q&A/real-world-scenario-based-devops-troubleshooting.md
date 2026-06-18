# Scenario-based questions are exactly what interviewers use to judge real DevOps skills.

Below are high-quality, real-world DevOps troubleshooting scenarios with clear, structured answers (what to check → why → how to fix). These are fresher-to-mid level interview standard, used by companies like Accenture, TCS, Infosys, startups, and product companies.

---

# 🔧 Scenario-Based DevOps Troubleshooting Questions & Answers

## 1️⃣ Application is down after deployment

**Scenario:**
You deployed a new version of the application. After deployment, the app is not accessible.

**Troubleshooting Steps:**

* Check deployment status (CI/CD logs)
* Check application logs
* Verify environment variables
* Check server/service health
* Roll back if needed

**Answer:**
I first check the CI/CD pipeline logs to ensure deployment succeeded. Then I inspect application logs (docker logs, /var/log, or Kubernetes logs). I verify environment variables, config files, and dependencies. If the issue persists, I roll back to the previous stable version and fix the root cause.

---

## 2️⃣ Jenkins pipeline fails at build stage

**Scenario:**
Jenkins pipeline fails during the build stage.

**Troubleshooting Steps:**

* Check console output
* Verify build tools (Maven, Node, Java)
* Check dependency versions
* Validate Jenkinsfile syntax

**Answer:**
I review the Jenkins console logs to identify errors like missing dependencies or version conflicts. I confirm build tools are installed on the Jenkins agent and validate the Jenkinsfile. If required, I fix dependency versions or update the build environment.

---

## 3️⃣ Docker container keeps restarting

**Scenario:**
A Docker container continuously restarts.

**Troubleshooting Steps:**

* Check container logs
* Check exit code
* Verify application startup command
* Check resource limits

**Answer:**
I use docker logs and docker inspect to identify why the container exits. Common reasons include application crashes, wrong entrypoint, or insufficient memory. After fixing the issue, I rebuild and redeploy the container.

---

## 4️⃣ Kubernetes Pod is in CrashLoopBackOff

**Scenario:**
Pod is stuck in CrashLoopBackOff.

**Troubleshooting Steps:**

* kubectl describe pod
* Check container logs
* Verify environment variables & secrets
* Check probes (liveness/readiness)

**Answer:**
I describe the pod to identify events and errors. Then I check logs of the crashing container. Often it’s due to misconfigured environment variables, missing secrets, or failing health checks. I fix the configuration and redeploy.

---

## 5️⃣ Website is slow under heavy traffic

**Scenario:**
Application becomes slow when traffic increases.

**Troubleshooting Steps:**

* Monitor CPU, memory, disk
* Check auto-scaling
* Analyze load balancer
* Check database performance

**Answer:**
I analyze monitoring tools (CloudWatch/Prometheus) to find resource bottlenecks. I ensure auto-scaling is configured correctly and check database queries. Based on findings, I optimize resources, enable caching, or increase replicas.

---

## 6️⃣ Git merge causes deployment failure

**Scenario:**
After merging code, production deployment fails.

**Troubleshooting Steps:**

* Review recent commits
* Check pipeline logs
* Test build locally
* Roll back if needed

**Answer:**
I identify the last commit causing failure using Git history. I test the build locally or in staging. If required, I revert the commit and deploy a stable version before fixing the issue.

---

## 7️⃣ EC2 instance is unreachable

**Scenario:**
You cannot SSH into an EC2 instance.

**Troubleshooting Steps:**

* Check instance state
* Verify security group rules
* Check network ACL
* Confirm key pair and username

**Answer:**
I verify that the instance is running and security groups allow SSH (port 22). I check NACL rules and confirm correct key pair and username. If needed, I use EC2 Instance Connect or recovery methods.

---

## 8️⃣ CI/CD pipeline succeeds but app doesn’t update

**Scenario:**
Pipeline shows success but changes are not reflected.

**Troubleshooting Steps:**

* Check deployment target
* Verify image/tag version
* Clear cache
* Restart services

**Answer:**
I ensure the latest image or artifact is deployed and not an old cached version. I verify tags, clear caches, and restart services or pods if necessary.

---

## 9️⃣ Kubernetes service not accessible externally

**Scenario:**
Application runs in Kubernetes but is not accessible from browser.

**Troubleshooting Steps:**

* Check service type
* Verify endpoints
* Check ingress configuration
* Validate firewall rules

**Answer:**
I check whether the service is NodePort, LoadBalancer, or via Ingress. I confirm endpoints are correctly mapped to pods. I also verify ingress rules and firewall/security group settings.

---

## 🔟 Terraform apply fails

**Scenario:**
Terraform apply fails with an error.

**Troubleshooting Steps:**

* Read error carefully
* Run terraform plan
* Validate provider credentials
* Check resource conflicts

**Answer:**
I analyze the error message and run terraform plan to preview changes. Most failures happen due to permission issues, invalid variables, or resource conflicts. I fix the configuration and re-apply.

---

## 1️⃣1️⃣ Logs not appearing in monitoring system

**Scenario:**
Application logs are missing in monitoring tools.

**Troubleshooting Steps:**

* Verify log agent status
* Check log paths
* Validate permissions
* Check network connectivity

**Answer:**
I ensure the logging agent is running and configured correctly. I verify log file paths and permissions. I also check connectivity to the logging backend.

---

## 1️⃣2️⃣ Disk space full on server

**Scenario:**
Server disk space is 100% used.

**Troubleshooting Steps:**

* Check disk usage
* Identify large files
* Clear logs/temp files
* Set log rotation

**Answer:**
I use df -h and du to identify large files. I clean unused logs, enable log rotation, and set monitoring alerts to prevent future issues.

---

## 🎯 Interview Tip for You (Important)

When answering scenario questions, always follow this pattern:

**Detect → Analyze → Fix → Prevent**

Interviewers LOVE this structure.

---

# Below are 50 Advanced, real-world, scenario-based DevOps troubleshooting questions.

These are production-level scenarios asked for DevOps Engineer / Cloud Engineer roles in companies like Accenture, Infosys, Amazon partners, and product startups.

I’ve grouped them so you can mentally organize answers in interviews.

---

# 🔥 50 Advanced Scenario-Based DevOps Questions

## 🔹 CI/CD (Jenkins, GitHub Actions, GitLab)

1️⃣ A Jenkins pipeline works in staging but fails in production. How do you troubleshoot?

2️⃣ A pipeline randomly fails at different stages without code changes. What could be the cause?

3️⃣ After enabling parallel stages in Jenkins, builds start failing intermittently. Why?

4️⃣ Your CI pipeline is extremely slow. How do you optimize it?

5️⃣ Secrets are accidentally exposed in pipeline logs. How do you fix and prevent this?

---

## 🔹 Docker & Containerization

6️⃣ A Docker image works locally but fails in Kubernetes. Why?

7️⃣ Containers run fine initially but crash after a few hours. How do you debug?

8️⃣ Docker images are too large and slow to pull. How do you optimize them?

9️⃣ A container exits with code 137. What does it mean?

🔟 Application logs disappear when a container restarts. How do you persist logs?

---

## 🔹 Kubernetes (Advanced & Production)

1️⃣1️⃣ Pods are running, but traffic is not reaching the application. What do you check?

1️⃣2️⃣ A Kubernetes deployment is stuck in “Progressing” state. How do you debug?

1️⃣3️⃣ Pods are evicted frequently. What are the possible reasons?

1️⃣4️⃣ HPA is not scaling pods even under heavy load. Why?

1️⃣5️⃣ After a node failure, pods are not rescheduled. What’s wrong?

---

## 🔹 AWS & Cloud Infrastructure

1️⃣6️⃣ EC2 instance CPU is normal but application is slow. What do you investigate?

1️⃣7️⃣ Auto Scaling Group is not launching new instances. How do you troubleshoot?

1️⃣8️⃣ An AWS service works in one region but fails in another. Why?

1️⃣9️⃣ Your AWS bill suddenly increases drastically. How do you find the root cause?

2️⃣0️⃣ ELB health checks are failing, but the app works locally. Why?

---

## 🔹 Terraform & Infrastructure as Code

2️⃣1️⃣ Terraform apply fails midway. Infrastructure is partially created. How do you recover?

2️⃣2️⃣ Terraform state file is corrupted or deleted. What do you do?

2️⃣3️⃣ Terraform keeps recreating resources on every apply. Why?

2️⃣4️⃣ Multiple team members run Terraform simultaneously and cause conflicts. How do you prevent this?

2️⃣5️⃣ Terraform variables differ between environments and cause errors. How do you manage this?

---

## 🔹 Monitoring, Logging & Observability

2️⃣6️⃣ Alerts are firing, but the system is healthy. How do you fix alert noise?

2️⃣7️⃣ Application latency increases, but CPU and memory look normal. What do you check?

2️⃣8️⃣ Metrics are missing for some Kubernetes pods. Why?

2️⃣9️⃣ Logs show errors, but users don’t face issues. How do you handle this?

3️⃣0️⃣ Prometheus is running out of disk space. How do you fix it?

---

## 🔹 Security & Reliability

3️⃣1️⃣ A secret is leaked in GitHub. What immediate steps do you take?

3️⃣2️⃣ A container image has known vulnerabilities. How do you handle this?

3️⃣3️⃣ DDoS attack impacts your application. How do you respond?

3️⃣4️⃣ An S3 bucket becomes publicly accessible accidentally. How do you detect and fix it?

3️⃣5️⃣ A compromised EC2 instance is detected. What are your actions?

---

## 🔹 Performance, Scaling & High Availability

3️⃣6️⃣ Database becomes a bottleneck under load. How do you fix it?

3️⃣7️⃣ Blue-Green deployment causes downtime. Why and how do you prevent it?

3️⃣8️⃣ Rolling deployment causes partial outages. What’s wrong?

3️⃣9️⃣ Cache is enabled, but performance doesn’t improve. Why?

4️⃣0️⃣ Traffic spikes cause application crashes. How do you stabilize the system?

---

## 🔹 Disaster Recovery & Incident Handling

4️⃣1️⃣ Production goes down after a release. How do you handle the incident?

4️⃣2️⃣ You accidentally delete a production database. What do you do?

4️⃣3️⃣ Multi-AZ setup still causes downtime. Why?

4️⃣4️⃣ A Kubernetes cluster upgrade breaks applications. How do you recover?

4️⃣5️⃣ Backup exists but restore fails. How do you handle this situation?

---

## 🔹 DevOps Culture & Process

4️⃣6️⃣ Developers bypass CI/CD and deploy manually. How do you stop this?

4️⃣7️⃣ Frequent production issues happen despite automation. Why?

4️⃣8️⃣ Teams blame DevOps for every failure. How do you handle this?

4️⃣9️⃣ Too many tools create operational complexity. How do you simplify?

5️⃣0️⃣ How do you ensure continuous improvement in DevOps practices?

---

## 🎯 How to Answer in Interviews (Golden Rule)

Use this format every time:

**Symptom → Root Cause Analysis → Fix → Prevention**

Example closing line interviewers love:

“After fixing the issue, I add monitoring, alerts, and automation to prevent recurrence.”

---

# Advanced DevOps Interview Scenarios – Interview-Ready Model Answers

Below are detailed, interview-ready model answers for all **50 advanced scenario-based DevOps questions**.

I’ve written them exactly in the way **senior DevOps engineers answer in interviews** — structured, calm, and production-focused.

---

## Mental Framework Used

Use this framework mentally while answering:

**Observe → Analyze → Fix → Prevent**

---

# 🔥 Detailed Model Answers – 50 Advanced DevOps Scenarios

---

## 🔹 CI/CD (1–5)

### 1️⃣ Jenkins works in staging but fails in production

**Answer:**

I compare environment differences first—credentials, secrets, environment variables, and infrastructure. I check Jenkins logs, deployment scripts, and production-specific configs. Often failures occur due to missing secrets, stricter network rules, or different resource limits. I fix config parity and introduce environment validation checks.

---

### 2️⃣ Pipeline fails randomly without code changes

**Answer:**

This usually indicates flaky tests, unstable agents, network issues, or shared resources. I analyze historical pipeline logs, check agent health, and external dependencies. I stabilize tests, isolate jobs, and add retries only where safe.

---

### 3️⃣ Parallel stages cause failures

**Answer:**

Parallel stages often cause race conditions—shared workspace, shared ports, or shared resources. I isolate workspaces, use unique artifact names, and ensure proper locking where required.

---

### 4️⃣ CI pipeline is slow

**Answer:**

I analyze build time per stage. Common optimizations include caching dependencies, parallel execution, reducing Docker image size, and using faster agents. I also eliminate unnecessary steps.

---

### 5️⃣ Secrets exposed in pipeline logs

**Answer:**

I immediately rotate compromised secrets. I mask credentials in Jenkins, move secrets to secret managers (Vault/AWS Secrets Manager), and enforce secret scanning in pipelines.

---

## 🔹 Docker (6–10)

### 6️⃣ Docker works locally but fails in Kubernetes

**Answer:**

This is often due to missing environment variables, different networking, or file system permissions. I check pod logs, config maps, and security contexts. I ensure container paths and ports match Kubernetes configs.

---

### 7️⃣ Containers crash after a few hours

**Answer:**

Likely memory leaks, resource exhaustion, or unhandled exceptions. I monitor memory usage, enable resource limits, analyze logs, and profile the application.

---

### 8️⃣ Docker images too large

**Answer:**

I use multi-stage builds, smaller base images (Alpine/distroless), remove unnecessary files, and minimize layers. This reduces pull time and attack surface.

---

### 9️⃣ Exit code 137

**Answer:**

Exit code 137 means the container was killed due to out-of-memory (OOM). I increase memory limits or optimize application memory usage.

---

### 🔟 Logs disappear on container restart

**Answer:**

Containers are ephemeral. I configure centralized logging (ELK, CloudWatch) or mount volumes to persist logs.

---

## 🔹 Kubernetes (11–15)

### 1️⃣1️⃣ Pods running but no traffic

**Answer:**

I check service selectors, endpoints, ingress rules, and network policies. Usually it’s a mismatch between service labels and pod labels.

---

### 1️⃣2️⃣ Deployment stuck in “Progressing”

**Answer:**

I inspect rollout status and pod events. Causes include failing readiness probes or insufficient resources. I fix probes or scale resources.

---

### 1️⃣3️⃣ Pods frequently evicted

**Answer:**

Evictions occur due to memory/disk pressure. I inspect node resources, set proper requests/limits, and clean node disk usage.

---

### 1️⃣4️⃣ HPA not scaling

**Answer:**

I verify metrics server availability, correct resource requests, and scaling thresholds. Without resource requests, HPA cannot calculate scaling.

---

### 1️⃣5️⃣ Pods not rescheduled after node failure

**Answer:**

Likely node affinity, taints/tolerations, or insufficient cluster capacity. I review scheduling constraints and ensure spare capacity.

---

## 🔹 AWS (16–20)

### 1️⃣6️⃣ CPU normal but app slow

**Answer:**

I investigate memory, disk I/O, network latency, and database performance. CPU alone doesn’t indicate health.

---

### 1️⃣7️⃣ ASG not launching instances

**Answer:**

I check launch template errors, IAM permissions, quota limits, and scaling policies. Often it’s an AMI or permission issue.

---

### 1️⃣8️⃣ Service works in one region but not another

**Answer:**

Possible causes: missing resources, IAM differences, or service availability. I compare region configurations.

---

### 1️⃣9️⃣ AWS bill spikes

**Answer:**

I analyze Cost Explorer, identify high-cost services, and check for misconfigured autoscaling or unused resources. I then add budgets and alerts.

---

### 2️⃣0️⃣ ELB health checks fail

**Answer:**

I check health check path, security groups, response codes, and application startup time. Health checks often fail due to wrong endpoints.

---

## 🔹 Terraform (21–25)

### 2️⃣1️⃣ Terraform apply fails midway

**Answer:**

I review state consistency, fix the issue, and re-run apply. Terraform state tracks partial creation safely.

---

### 2️⃣2️⃣ State file deleted

**Answer:**

I restore from backend backups (S3 versioning). If unavailable, I import existing resources.

---

### 2️⃣3️⃣ Resources recreated every apply

**Answer:**

Usually caused by dynamic values or misconfigured lifecycle rules. I stabilize inputs and use ignore_changes.

---

### 2️⃣4️⃣ Multiple users cause conflicts

**Answer:**

I enable remote state with locking (S3 + DynamoDB) to prevent concurrent changes.

---

### 2️⃣5️⃣ Variable mismatch across environments

**Answer:**

I use separate tfvars files, modules, and environment-specific workspaces.

---

## 🔹 Monitoring & Logging (26–30)

### 2️⃣6️⃣ Alert noise

**Answer:**

I tune alert thresholds, use severity levels, and implement alert deduplication.

---

### 2️⃣7️⃣ Latency high, resources normal

**Answer:**

I inspect network latency, database queries, and downstream dependencies.

---

### 2️⃣8️⃣ Missing pod metrics

**Answer:**

I verify metrics server and RBAC permissions.

---

### 2️⃣9️⃣ Errors in logs but users fine

**Answer:**

I analyze error severity. Not all errors are critical. I refine log levels and alerts.

---

### 3️⃣0️⃣ Prometheus disk full

**Answer:**

I reduce retention, add persistent storage, and enable data compaction.

---

## 🔹 Security (31–35)

### 3️⃣1️⃣ Secret leaked

**Answer:**

Immediately rotate the secret, audit access, and add secret scanning.

---

### 3️⃣2️⃣ Vulnerable container image

**Answer:**

I rebuild with patched base images and enforce image scanning in CI.

---

### 3️⃣3️⃣ DDoS attack

**Answer:**

I enable WAF, rate limiting, autoscaling, and block malicious IPs.

---

### 3️⃣4️⃣ Public S3 bucket

**Answer:**

I revoke public access, audit logs, and enforce bucket policies.

---

### 3️⃣5️⃣ Compromised EC2

**Answer:**

I isolate the instance, analyze logs, rotate credentials, and rebuild from a clean AMI.

---

## 🔹 Performance & HA (36–40)

### 3️⃣6️⃣ DB bottleneck

**Answer:**

I add read replicas, indexing, caching, and optimize queries.

---

### 3️⃣7️⃣ Blue-Green downtime

**Answer:**

Likely traffic switching misconfiguration. I fix load balancer routing.

---

### 3️⃣8️⃣ Rolling deployment outage

**Answer:**

Improper readiness probes or max unavailable settings. I tune rollout strategy.

---

### 3️⃣9️⃣ Cache ineffective

**Answer:**

Cache keys or TTL may be incorrect. I analyze hit/miss ratio.

---

### 4️⃣0️⃣ Traffic spikes crash app

**Answer:**

I enable autoscaling, circuit breakers, and rate limiting.

---

## 🔹 Disaster Recovery & Culture (41–50)

### 4️⃣1️⃣ Prod down after release

**Answer:**

Rollback immediately, stabilize, then perform RCA.

---

### 4️⃣2️⃣ DB deleted

**Answer:**

Restore from backup and audit deletion controls.

---

### 4️⃣3️⃣ Multi-AZ still down

**Answer:**

Shared dependency failure. I redesign architecture.

---

### 4️⃣4️⃣ Cluster upgrade breaks apps

**Answer:**

Rollback, test in staging, and check deprecated APIs.

---

### 4️⃣5️⃣ Backup restore fails

**Answer:**

I test backups regularly and fix restore procedures.

---

### 4️⃣6️⃣ Manual deployments

**Answer:**

Restrict access and enforce CI/CD gates.

---

### 4️⃣7️⃣ Frequent prod issues

**Answer:**

Missing testing, monitoring, or process gaps.

---

### 4️⃣8️⃣ DevOps blamed

**Answer:**

I focus on collaboration, blameless postmortems.

---

### 4️⃣9️⃣ Tool sprawl

**Answer:**

Standardize tools and remove redundancy.

---

### 5️⃣0️⃣ Continuous improvement

**Answer:**

Regular retrospectives, automation, and metrics-driven improvements.

---

## 🎯 Final Interview Tip (Very Important)

End answers with:

> “After fixing it, I add monitoring, alerts, and automation to prevent recurrence.”

This line alone raises your interview score.

---

# ⭐ Top 20 Most-Asked DevOps Scenarios (Perfect Answers)

These Top 20 DevOps scenarios are the **MOST frequently asked** in real interviews (Accenture, TCS, Cognizant, startups, and product companies).

Learn these 20 properly and you’ll handle **70–80% of scenario rounds**.

---

### 1️⃣ Application down after deployment

**Perfect Answer:**

“I first check the CI/CD pipeline logs to confirm deployment success. Then I analyze application logs and verify environment variables, configs, and dependencies. If the issue impacts users, I roll back immediately and then fix the root cause before redeploying.”

---

### 2️⃣ Jenkins pipeline suddenly failing

**Perfect Answer:**

“I inspect the Jenkins console logs and recent changes. If there were no code changes, I check agent health, tool versions, and external dependencies. Many times, failures are due to environment drift or network issues.”

---

### 3️⃣ Docker container keeps restarting

**Perfect Answer:**

“I check container logs and exit codes. Continuous restarts usually indicate application crashes, wrong entrypoints, or memory limits. After fixing the issue, I rebuild and redeploy the container.”

---

### 4️⃣ Pod in CrashLoopBackOff

**Perfect Answer:**

“I describe the pod and inspect logs. Most causes are misconfigured environment variables, missing secrets, or failing health probes. I fix the configuration and redeploy.”

---

### 5️⃣ Website slow under high traffic

**Perfect Answer:**

“I analyze CPU, memory, disk I/O, and network metrics. I check autoscaling and database performance. Based on findings, I scale resources, optimize queries, or enable caching.”

---

### 6️⃣ CI/CD pipeline succeeds but app not updated

**Perfect Answer:**

“I verify whether the latest artifact or Docker image is deployed. I check image tags, caching issues, and service restarts. Often it’s an old image being reused.”

---

### 7️⃣ Cannot SSH into EC2

**Perfect Answer:**

“I verify instance state, security group rules, NACLs, and correct key pair usage. If required, I use EC2 recovery methods or instance connect.”

---

### 8️⃣ Kubernetes service not accessible externally

**Perfect Answer:**

“I check service type, endpoints, ingress rules, and firewall settings. Usually it’s a label mismatch or incorrect ingress configuration.”

---

### 9️⃣ Terraform apply fails

**Perfect Answer:**

“I carefully read the error, run terraform plan, and check provider credentials or resource conflicts. I fix the configuration and re-apply.”

---

### 🔟 Sudden AWS bill increase

**Perfect Answer:**

“I analyze AWS Cost Explorer to identify the service causing the spike. I check autoscaling, unused resources, and misconfigurations, then add budgets and alerts.”

---

### 1️⃣1️⃣ HPA not scaling pods

**Perfect Answer:**

“I verify metrics server availability and resource requests. Without proper requests, HPA cannot calculate scaling.”

---

### 1️⃣2️⃣ Logs missing in monitoring

**Perfect Answer:**

“I ensure the logging agent is running, log paths are correct, and permissions are valid. I also verify connectivity to the logging backend.”

---

### 1️⃣3️⃣ Database performance degradation

**Perfect Answer:**

“I analyze slow queries, add indexing, introduce caching, and scale read replicas if needed.”

---

### 1️⃣4️⃣ ELB health checks failing

**Perfect Answer:**

“I check health check path, response codes, security groups, and application startup time. Misconfigured health checks are the most common cause.”

---

### 1️⃣5️⃣ Disk full on server

**Perfect Answer:**

“I identify large files using disk tools, clean unused logs, enable log rotation, and set alerts to prevent recurrence.”

---

### 1️⃣6️⃣ Secrets leaked in GitHub

**Perfect Answer:**

“I immediately rotate the secret, audit access, remove it from Git history, and implement secret scanning and proper secret management.”

---

### 1️⃣7️⃣ Docker image vulnerability found

**Perfect Answer:**

“I rebuild the image with patched base images and enforce image scanning in CI/CD pipelines.”

---

### 1️⃣8️⃣ Production down after release

**Perfect Answer:**

“I roll back immediately, restore service, and then perform root cause analysis before redeploying.”

---

### 1️⃣9️⃣ Manual deployments bypassing CI/CD

**Perfect Answer:**

“I restrict production access and enforce deployments only through CI/CD with proper approvals.”

---

### 2️⃣0️⃣ How do you prevent repeated incidents?

**Perfect Answer:**

“By adding monitoring, alerts, automation, proper documentation, and performing blameless postmortems.”

---

## 🎯 FINAL INTERVIEW HACK (Memorize This Line)

> “First I stabilize production, then I analyze root cause, fix it, and add automation to prevent recurrence.”

Say this confidently — interviewers LOVE it.
