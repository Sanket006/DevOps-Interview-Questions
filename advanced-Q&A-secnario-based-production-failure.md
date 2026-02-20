# Scenario-Based Production Failure Interview Questions (DevOps)

This README contains **real-world, scenario-based production failure questions with clear, practical answers**, based on my **DevOps Engineer resume, tools, and hands-on experience**. These scenarios reflect **actual production incidents** commonly discussed in DevOps interviews.

---

## 🔥 Scenario 1: Website Suddenly Down

### Situation:

Users report that the application hosted on AWS is not accessible.

### How do you troubleshoot?

**Answer:**

1. Check Application Load Balancer target health
2. Verify EC2 instance status checks
3. Review security groups and NACL rules
4. Check Auto Scaling Group activity
5. Inspect application and system logs
6. Verify DNS and Route 53 configuration
7. Check recent deployments or configuration changes

---

## 🔥 Scenario 2: High CPU Usage on EC2 Instances

### Situation:

CloudWatch alarms show CPU utilization above 90%.

### How do you handle this?

**Answer:**

* Identify processes using `top` or `htop`
* Review application logs for abnormal behavior
* Scale out using Auto Scaling if traffic increased
* Optimize application or add caching
* Restart misbehaving services if required

---

## 🔥 Scenario 3: Jenkins Pipeline Failing Suddenly

### Situation:

A previously successful Jenkins pipeline starts failing.

### How do you troubleshoot?

**Answer:**

* Check recent code changes or merged pull requests
* Review Jenkins console logs
* Validate credentials and secrets
* Verify disk space on Jenkins server
* Check plugin updates or dependency changes

---

## 🔥 Scenario 4: Docker Container Crashing

### Situation:

Docker containers exit immediately after starting.

### How do you fix it?

**Answer:**

* Check container logs using `docker logs`
* Verify CMD/ENTRYPOINT configuration
* Run container interactively for debugging
* Check environment variables and ports
* Confirm image compatibility

---

## 🔥 Scenario 5: Kubernetes Pods in CrashLoopBackOff

### Situation:

Pods in EKS are restarting continuously.

### How do you investigate?

**Answer:**

* Describe pod using `kubectl describe pod`
* Check container logs using `kubectl logs`
* Verify ConfigMaps and Secrets
* Check resource limits and requests
* Roll back to the previous stable deployment

---

## 🔥 Scenario 6: Terraform Apply Deleted Resources Accidentally

### Situation:

Terraform removes production resources unexpectedly.

### How do you respond?

**Answer:**

* Stop further Terraform runs immediately
* Review Terraform plan output
* Restore state from S3 versioning
* Re-import resources using `terraform import`
* Implement approval steps for production

---

## 🔥 Scenario 7: Load Balancer Shows Unhealthy Targets

### Situation:

ALB reports targets as unhealthy.

### How do you troubleshoot?

**Answer:**

* Check health check path and port
* Verify security group rules
* Confirm application is listening on correct port
* Check application logs
* Test endpoint locally

---

## 🔥 Scenario 8: Application Not Scaling During Traffic Spike

### Situation:

Traffic increases but instances are not scaling.

### How do you fix it?

**Answer:**

* Check Auto Scaling policies
* Verify CloudWatch metrics
* Ensure correct launch template
* Confirm instance limits
* Validate IAM permissions

---

## 🔥 Scenario 9: Logs Not Appearing in CloudWatch

### Situation:

Application logs are missing in CloudWatch.

### How do you resolve it?

**Answer:**

* Verify CloudWatch agent status
* Check IAM role permissions
* Confirm log file paths
* Restart CloudWatch agent

---

## 🔥 Scenario 10: Production Deployment Failed

### Situation:

Deployment fails and users are impacted.

### How do you respond?

**Answer:**

* Roll back immediately
* Notify stakeholders
* Analyze root cause
* Fix and test in lower environment
* Document incident and prevention steps

---

## 🔥 Scenario 11: Disk Space Full on Production Server

### Situation:

Server disk usage reaches 100%.

### How do you handle it?

**Answer:**

* Identify large files using `du` and `df`
* Clear old logs and temporary files
* Increase disk size if required
* Set up monitoring alerts

---

## 🔥 Scenario 12: Secrets Exposed in Git Repository

### Situation:

AWS credentials are accidentally committed.

### How do you respond?

**Answer:**

* Revoke compromised credentials immediately
* Rotate secrets
* Remove secrets from Git history
* Implement secret scanning tools
* Educate team on secure practices

---

## 📌 Interview Tip

Always explain:

* **Immediate action**
* **Root cause analysis**
* **Long-term prevention**

---

### 🚀 Next Enhancements

* Add **company-specific scenarios**
* Convert to **STAR-format answers**
* Practice **live mock incident interviews**

---

**Author:** Sanket Chopade
**Role:** DevOps Engineer
