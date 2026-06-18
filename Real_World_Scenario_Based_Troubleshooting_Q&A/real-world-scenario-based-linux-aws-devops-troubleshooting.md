# Scenario-Based Linux, AWS & DevOps Troubleshooting Interview Guide

This README contains **complete, real-world, scenario-based interview questions and answers** for **Linux, AWS, DevOps, Kubernetes, Terraform**, written in **easy, interview-friendly language**.

> ⚠️ **Note:** All content below is preserved exactly as requested. No questions, answers, or explanations have been skipped or removed.

---

## 🔧 Scenario-Based Linux, AWS & DevOps Troubleshooting (20 Q&A)

### 🐧 LINUX TROUBLESHOOTING (1–7)

### 1. Server is running but users can’t SSH into it

**Answer:**

* Check if SSH service is running

```bash
systemctl status sshd
```

* Verify port 22 is listening

```bash
ss -tulpn | grep 22
```

* Check firewall rules

```bash
firewall-cmd --list-all
```

* Inspect logs

```bash
journalctl -u sshd
```

* Check disk space (`/` or `/var` full can block login)

---

### 2. Linux server is very slow

**Answer:**

* Check CPU & memory

```bash
top / htop
```

* Check disk I/O

```bash
iostat -x
```

* Look for zombie or high-CPU processes
* Check disk space

```bash
df -h
```

* Review system logs for errors

---

### 3. Application fails after reboot

**Answer:**

* Check service status

```bash
systemctl status app.service
```

* Ensure service is enabled

```bash
systemctl enable app.service
```

* Check logs

```bash
journalctl -u app.service
```

* Verify environment variables and dependencies

---

### 4. Disk is full and system is not responding

**Answer:**

* Identify large files

```bash
du -sh /* | sort -h
```

* Clean logs

```bash
journalctl --vacuum-time=7d
```

* Remove unused files
* Restart affected services
* Add disk or enable log rotation

---

### 5. Cron job is not running

**Answer:**

* Verify cron service

```bash
systemctl status crond
```

* Check cron syntax
* Ensure correct user cron
* Redirect output to log
* Check `/var/log/cron`

---

### 6. File permission denied error

**Answer:**

* Check ownership

```bash
ls -l file
```

* Fix permissions

```bash
chmod / chown
```

* Check SELinux

```bash
sestatus
```

* Adjust context if required

---

### 7. Server reboots unexpectedly

**Answer:**

* Check system logs

```bash
last reboot
```

* Review hardware errors

```bash
dmesg
```

* Check memory and disk health
* Look for OOM killer logs

---

## ☁️ AWS TROUBLESHOOTING (8–14)

### 8. EC2 instance is running but not reachable

**Answer:**

* Check Security Group (SSH/HTTP allowed?)
* Check NACL rules
* Verify instance has public IP
* Confirm route table → Internet Gateway
* Check instance firewall

---

### 9. EC2 CPU usage is 100%

**Answer:**

* Identify process inside EC2

```bash
top
```

* Check CloudWatch metrics
* Restart heavy services
* Scale instance or use Auto Scaling
* Enable load balancing

---

### 10. Application Load Balancer shows unhealthy targets

**Answer:**

* Verify health check path
* Check security groups between ALB & EC2
* Ensure application listens on correct port
* Review app logs

---

### 11. S3 access denied error

**Answer:**

* Check IAM policy
* Verify bucket policy
* Confirm correct object path
* Check encryption permissions (KMS)

---

### 12. RDS connection timeout

**Answer:**

* Check security group rules
* Ensure DB is in same VPC or peered
* Verify DB endpoint
* Check max connections

---

### 13. Auto Scaling not launching instances

**Answer:**

* Check launch template
* Verify AMI permissions
* Check instance limits
* Review scaling activity logs

---

### 14. CloudWatch alarm not triggering

**Answer:**

* Verify metric namespace
* Check evaluation period
* Confirm threshold value
* Ensure correct region

---

## ⚙️ DEVOPS / CI-CD TROUBLESHOOTING (15–20)

### 15. Jenkins pipeline fails after code push

**Answer:**

* Check Jenkins console output
* Verify credentials
* Check SCM webhook
* Validate pipeline syntax
* Re-run with debug logs

---

### 16. Docker container keeps restarting

**Answer:**

* Check logs

```bash
docker logs container
```

* Verify CMD/ENTRYPOINT
* Check memory limits
* Ensure app doesn’t exit immediately

---

### 17. Docker image build fails

**Answer:**

* Check Dockerfile syntax
* Verify base image availability
* Check network access
* Review build logs carefully

---

### 18. Kubernetes pod stuck in CrashLoopBackOff

**Answer:**

* Check pod logs

```bash
kubectl logs pod
```

* Describe pod

```bash
kubectl describe pod
```

* Verify config maps & secrets
* Check resource limits

---

### 19. Deployment succeeded but app not accessible

**Answer:**

* Check service type
* Verify endpoints
* Check ingress rules
* Confirm container port mapping

---

### 20. CI pipeline is slow

**Answer:**

* Identify slow stages
* Enable parallel jobs
* Use caching
* Optimize Docker layers
* Scale build agents

---

## ✅ Interview Tip for DevOps Freshers

Always explain troubleshooting in this order:

**Problem → Check logs → Verify configuration → Fix → Prevent**

---

## ⭐ Top 20 Most-Asked Linux + AWS + DevOps Scenarios (Perfect Answers)

### 🐧 LINUX (1–7)

1. **Server is up but SSH is not working**

**Perfect Answer:**

I first check whether the SSH service is running using `systemctl status sshd`. Then I verify if port 22 is listening and allowed in the firewall. I also check authentication logs and ensure disk space is not full. Finally, I confirm the correct key and user permissions.

2. **Linux server becomes slow suddenly**

**Perfect Answer:**

I check CPU, memory, and load average using `top` or `htop`. Then I analyze disk I/O and disk space. If required, I identify resource-consuming processes and check logs for abnormal behavior before restarting or scaling resources.

3. **Disk is 100% full**

**Perfect Answer:**

I identify large directories using `du`, clean old logs, temporary files, and unused data. I also ensure log rotation is enabled and recommend increasing disk size to avoid recurrence.

4. **Application not starting after reboot**

**Perfect Answer:**

I check the service status and logs using `systemctl` and `journalctl`. Then I verify dependencies, environment variables, and ensure the service is enabled to start on boot.

5. **Cron job not executing**

**Perfect Answer:**

I confirm the cron service is running, verify correct user cron, check permissions, environment variables, and redirect cron output to logs for debugging.

6. **Permission denied error**

**Perfect Answer:**

I check file ownership and permissions, correct them using `chown` and `chmod`, and also verify SELinux policies if enabled.

7. **Server reboots automatically**

**Perfect Answer:**

I analyze reboot history, system logs, and kernel messages to check for hardware issues, OOM killer events, or scheduled reboots.

---

### ☁️ AWS (8–14)

8. **EC2 instance is running but not reachable**

**Perfect Answer:**

I verify security group rules, NACLs, route table, public IP assignment, and instance-level firewall to ensure proper network access.

9. **EC2 CPU utilization is high**

**Perfect Answer:**

I identify the process inside the instance, review CloudWatch metrics, restart services if needed, and scale vertically or horizontally using Auto Scaling.

10. **Load Balancer shows unhealthy targets**

**Perfect Answer:**

I check health check configuration, security groups between ALB and EC2, application port, and application logs.

11. **S3 Access Denied error**

**Perfect Answer:**

I review IAM permissions, bucket policies, object-level permissions, and encryption settings such as KMS policies.

12. **RDS database connection timeout**

**Perfect Answer:**

I confirm security group rules, correct endpoint usage, VPC connectivity, and check max connection limits on the database.

13. **Auto Scaling group not launching instances**

**Perfect Answer:**

I verify launch template configuration, AMI permissions, instance limits, and review Auto Scaling activity logs.

14. **CloudWatch alarm not triggering**

**Perfect Answer:**

I validate metric name, namespace, threshold, evaluation period, and ensure the resource is in the correct region.

---

### ⚙️ DEVOPS / CI-CD (15–20)

15. **Jenkins pipeline fails after code push**

**Perfect Answer:**

I review Jenkins console output, validate pipeline syntax, check credentials, SCM connectivity, and rerun the pipeline after fixing issues.

16. **Docker container keeps restarting**

**Perfect Answer:**

I check container logs, verify the CMD or ENTRYPOINT, ensure the application runs continuously, and confirm memory limits are sufficient.

17. **Docker image build fails**

**Perfect Answer:**

I inspect Dockerfile syntax, base image availability, network connectivity, and analyze build logs step by step.

18. **Kubernetes pod stuck in CrashLoopBackOff**

**Perfect Answer:**

I check pod logs and events, validate environment variables, secrets, config maps, and confirm resource limits are correctly set.

19. **Deployment successful but app not accessible**

**Perfect Answer:**

I verify service type, target port, endpoints, ingress configuration, and security rules.

20. **CI/CD pipeline is very slow**

**Perfect Answer:**

I identify slow stages, enable parallel execution, optimize Docker layers, use caching, and scale build agents.

---

## 🎯 How to Answer in Interview (Golden Formula)

**Identify → Verify → Fix → Prevent**

---

## 🚀 Advanced Scenarios: Kubernetes + Terraform + AWS (Top 20)

### ☸️ KUBERNETES (1–8)

1. **Pod stuck in Pending state**

**Perfect Answer:**

I describe the pod to check scheduling errors. Common reasons are insufficient CPU/memory, node selector mismatch, or missing PersistentVolumes. I verify node capacity, resource requests, and storage classes.

2. **Pods restarting with CrashLoopBackOff**

**Perfect Answer:**

I check pod logs and events, verify application startup command, environment variables, secrets, and config maps. I also review resource limits and readiness/liveness probes.

3. **Service exists but traffic not reaching pods**

**Perfect Answer:**

I verify service selector labels, target port, endpoints, and ensure the container listens on the correct port. I also check network policies if configured.

4. **Kubernetes deployment succeeded but rollout is stuck**

**Perfect Answer:**

I inspect rollout status and events, check readiness probes, image pull issues, and ensure the new pods become ready before old pods are terminated.

5. **Node goes NotReady**

**Perfect Answer:**

I check node conditions, kubelet status, disk pressure, and memory pressure. I also inspect cloud provider health and restart kubelet if required.

6. **High latency inside the cluster**

**Perfect Answer:**

I check pod resource limits, node utilization, DNS resolution, network plugins, and service mesh metrics if used.

7. **Ingress not working**

**Perfect Answer:**

I verify ingress controller is running, ingress rules are correct, DNS points to the load balancer, and TLS secrets exist if HTTPS is used.

8. **Secrets updated but pods still using old values**

**Perfect Answer:**

Kubernetes does not automatically restart pods on secret changes. I restart pods or trigger a rollout to apply updated secrets.

---

### 🏗️ TERRAFORM (9–14)

9. **Terraform apply fails due to state lock**

**Perfect Answer:**

I verify whether another apply is running. If it’s a stale lock, I safely remove it using `terraform force-unlock` after confirming no active process is using the state.

10. **Terraform resources recreated unexpectedly**

**Perfect Answer:**

I check for changes in resource arguments, provider versions, and lifecycle rules. I also review state file drift and ensure resources are not modified manually outside Terraform.

11. **Terraform state file corrupted**

**Perfect Answer:**

I restore from the latest backend backup (S3 with versioning). This is why I always use remote state with locking and backups enabled.

12. **Multiple teams modifying Terraform**

**Perfect Answer:**

I enforce remote state, state locking, workspaces, and code reviews. I also separate environments using modules and directories.

13. **Terraform apply succeeds but resources not visible**

**Perfect Answer:**

I confirm correct AWS account, region, provider configuration, and credentials. Many issues occur due to wrong region or profile.

14. **Terraform plan shows huge changes**

**Perfect Answer:**

I carefully review the plan, check for unintended variable or module changes, provider updates, and validate lifecycle rules before applying.

---

### ☁️ AWS (15–20)

15. **EKS cluster created but nodes not joining**

**Perfect Answer:**

I check node IAM role permissions, security group rules, subnet routing, and verify aws-auth ConfigMap mapping for worker nodes.

16. **Application works in private subnet but not public**

**Perfect Answer:**

I check route tables, NAT gateway configuration, security groups, and NACLs. Often outbound internet access is missing.

17. **ALB returns 502/503 errors**

**Perfect Answer:**

I check target health, security groups, application response codes, and ensure health check paths are correct.

18. **AWS cost suddenly spikes**

**Perfect Answer:**

I analyze Cost Explorer, identify high-cost services, check for over-provisioned resources, unused EBS volumes, and missing auto-scaling policies.

19. **IAM role works locally but fails in EC2**

**Perfect Answer:**

I ensure the correct IAM role is attached to the instance and that no hardcoded credentials override instance profile credentials.

20. **Production outage after Terraform deployment**

**Perfect Answer:**

I rollback using version control, check recent Terraform changes, inspect resource replacements, and restore from backups if required. I also add safeguards like `prevent_destroy`.

---

## 🧠 How Seniors Answer These Questions

**Root cause → Evidence → Fix → Prevention**

---

🚀 You are now practicing **real-world, senior-level DevOps interview scenarios**.

Happy Learning & Best of Luck!
