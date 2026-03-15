# Scenario-Based Production Failure Questions & Answers  
## ApnaKart – Enterprise E-Commerce Platform (DevOps)

This document covers **real-world production outage and failure scenarios** encountered in an **enterprise-scale microservices-based e-commerce platform** like **ApnaKart**.  
These scenarios are commonly asked in **DevOps / SRE interviews** to assess troubleshooting skills, production ownership, and decision-making under pressure.

---

## 📌 Project Context

- Project: **ApnaKart**
- Architecture: Microservices
- Platform: AWS
- Container Orchestration: Kubernetes (EKS)
- CI/CD: Jenkins
- Infrastructure as Code: Terraform
- Monitoring: Datadog
- Role: **Sole DevOps Engineer (End-to-End Ownership)**

---

## 🚨 1. Production Website Down After Deployment

### Scenario  
After deploying a new version to production, users report that the website is not loading.

### Approach  
1. Check Kubernetes deployment rollout status  
2. Inspect pod logs and events  
3. Verify readiness and liveness probes  
4. Check ingress and service routing  
5. Roll back to the previous stable release if required  

### Root Cause Example  
Misconfigured environment variables in the new deployment.

### Resolution  
Rolled back to the last stable version, fixed configuration, validated in staging, and redeployed.

---

## 🚨 2. High Traffic Causing Application Slowness

### Scenario  
A flash sale causes a sudden traffic spike and the application becomes slow.

### Approach  
1. Monitor CPU, memory, and latency metrics  
2. Check HPA scaling behavior  
3. Review pod resource requests and limits  
4. Scale replicas manually if required  

### Root Cause Example  
HPA maximum replica limit was too low.

### Resolution  
Increased HPA limits and optimized resource allocation.

---

## 🚨 3. Database Connection Errors in Production

### Scenario  
Backend services start failing with database connection errors.

### Approach  
1. Check database health and connection limits  
2. Inspect application logs for timeout issues  
3. Verify security groups and network access  
4. Restart affected pods if needed  

### Root Cause Example  
Database connection pool exhaustion.

### Resolution  
Optimized connection pool configuration and scaled database resources.

---

## 🚨 4. One Microservice Crashing Repeatedly

### Scenario  
A specific microservice keeps restarting while others are stable.

### Approach  
1. Analyze pod logs and Kubernetes events  
2. Check memory and CPU limits  
3. Review recent code changes  
4. Validate health probe configurations  

### Root Cause Example  
OOMKilled due to insufficient memory allocation.

### Resolution  
Increased memory limits and optimized JVM settings.

---

## 🚨 5. CI/CD Pipeline Fails During Production Deployment

### Scenario  
The production Jenkins pipeline fails during deployment.

### Approach  
1. Stop the pipeline to prevent partial deployment  
2. Review Jenkins build logs  
3. Verify Docker image availability and tags  
4. Validate Kubernetes manifests  

### Root Cause Example  
Incorrect Docker image tag referenced in deployment.

### Resolution  
Fixed image tag and re-triggered the pipeline.

---

## 🚨 6. SSL Certificate Expired in Production

### Scenario  
Users see browser security warnings due to expired SSL certificates.

### Approach  
1. Verify certificate expiration  
2. Check ingress TLS configuration  
3. Renew or reissue certificate  
4. Reload ingress controller  

### Root Cause Example  
Auto-renewal process failure.

### Resolution  
Renewed certificate and configured monitoring alerts for expiration.

---

## 🚨 7. Critical Production Bug Requiring Hotfix

### Scenario  
A critical bug affects payment functionality in production.

### Approach  
1. Create a hotfix branch from production  
2. Apply and test the fix  
3. Deploy via CI/CD pipeline  
4. Merge hotfix back into dev and test branches  

### Root Cause Example  
Unhandled edge case in payment service logic.

### Resolution  
Hotfix deployed and service restored quickly.

---

## 🚨 8. Monitoring Alerts for High Pod Restarts

### Scenario  
Monitoring alerts indicate frequent pod restarts.

### Approach  
1. Check pod restart reasons  
2. Inspect liveness and readiness probe settings  
3. Analyze application logs  
4. Review node resource pressure  

### Root Cause Example  
Overly aggressive liveness probe configuration.

### Resolution  
Adjusted probe thresholds and stabilized the service.

---

## 🚨 9. Kubernetes Worker Node Failure

### Scenario  
A Kubernetes worker node becomes unavailable.

### Approach  
1. Check node health and status  
2. Ensure pods are rescheduled to healthy nodes  
3. Verify node auto-scaling group behavior  

### Root Cause Example  
Underlying EC2 instance failure.

### Resolution  
Node automatically replaced via autoscaling.

---

## 🚨 10. Emergency Production Rollback

### Scenario  
A new production release introduces severe issues.

### Approach  
1. Pause further deployments  
2. Roll back to the last stable version  
3. Communicate with stakeholders  
4. Perform root cause analysis  

### Root Cause Example  
Regression introduced by a new feature.

### Resolution  
Rollback completed, RCA documented, and test coverage improved.

---

## 🎯 Key Interview Takeaway

> **“In production, my priority is service restoration first, followed by root cause analysis, and then preventive measures through automation and monitoring.”**

---

## ✅ Skills Demonstrated

- Production incident handling  
- Kubernetes troubleshooting  
- CI/CD failure recovery  
- AWS infrastructure reliability  
- Monitoring and alert-driven response  
- Calm decision-making under pressure  

---

📌 *This document is ideal for DevOps/SRE interview preparation, GitHub portfolios, and real-world production discussion.*

---

# Scenario-Based Production Failure Questions & Answers

This document covers **real-world production outage and failure scenarios** based on the **ApnaKart enterprise e-commerce platform**. These scenarios are commonly asked in **DevOps / SRE interviews** to evaluate troubleshooting, decision-making, and production ownership.

---

## 1. Website is Down After a New Deployment

**Scenario:**
After deploying a new release to production, users report that the website is not loading.

**How I Would Handle It:**

1. Check application and ingress logs to confirm whether pods are starting correctly.
2. Verify Kubernetes deployment rollout status.
3. Check readiness and liveness probes.
4. Inspect recent CI/CD changes and image tags.
5. Roll back to the last stable version if needed.

**Root Cause Example:**
Misconfigured environment variables in the new deployment.

**Resolution:**
Rolled back the deployment, fixed the configuration, validated in staging, and redeployed.

---

## 2. Sudden Traffic Spike Causes Application Slowness

**Scenario:**
A flash sale causes a sudden spike in traffic, and the application becomes slow.

**How I Would Handle It:**

1. Monitor CPU and memory metrics using Datadog.
2. Check HPA scaling behavior.
3. Inspect pod resource requests and limits.
4. Scale replicas manually if required.

**Root Cause Example:**
HPA limits were set too low for peak traffic.

**Resolution:**
Increased HPA max replicas and optimized resource allocation.

---

## 3. Database Connection Errors in Production

**Scenario:**
Backend services start throwing database connection errors.

**How I Would Handle It:**

1. Check RDS health and connection limits.
2. Inspect application logs for timeout errors.
3. Verify security groups and network policies.
4. Restart affected pods if necessary.

**Root Cause Example:**
Database connection pool exhaustion.

**Resolution:**
Optimized connection pool settings and scaled the database.

---

## 4. One Microservice is Crashing Repeatedly

**Scenario:**
A specific microservice keeps restarting in production.

**How I Would Handle It:**

1. Check pod logs and events.
2. Verify resource limits.
3. Inspect recent code changes.
4. Validate health probes.

**Root Cause Example:**
Insufficient memory limit causing OOMKills.

**Resolution:**
Increased memory limits and optimized JVM settings.

---

## 5. CI/CD Pipeline Fails During Production Release

**Scenario:**
The production pipeline fails during deployment.

**How I Would Handle It:**

1. Analyze Jenkins logs.
2. Check Docker image availability.
3. Validate Kubernetes manifests.
4. Stop the pipeline to prevent partial deployment.

**Root Cause Example:**
Incorrect image tag referenced in deployment.

**Resolution:**
Fixed the tag, revalidated the pipeline, and redeployed.

---

## 6. SSL Certificate Expired

**Scenario:**
Users see security warnings due to an expired SSL certificate.

**How I Would Handle It:**

1. Confirm certificate expiration.
2. Check ingress TLS configuration.
3. Renew or reissue the certificate.
4. Reload ingress controller.

**Root Cause Example:**
Certificate auto-renewal failure.

**Resolution:**
Renewed certificate and fixed auto-renewal monitoring.

---

## 7. Hotfix Required for Critical Production Bug

**Scenario:**
A critical bug is reported in production that must be fixed immediately.

**How I Would Handle It:**

1. Create a hotfix branch from production.
2. Apply and test the fix.
3. Deploy via CI/CD pipeline.
4. Merge changes back to dev and test branches.

**Root Cause Example:**
Unvalidated edge case in payment service.

**Resolution:**
Applied fix, validated with business team, and restored service.

---

## 8. Monitoring Alert for High Pod Restarts

**Scenario:**
Datadog alerts show frequent pod restarts.

**How I Would Handle It:**

1. Review pod restart reasons.
2. Check node resource pressure.
3. Analyze application logs.
4. Adjust probes or resources.

**Root Cause Example:**
Aggressive liveness probe settings.

**Resolution:**
Tuned probe intervals and stabilized pods.

---

## 9. Kubernetes Node Goes Down

**Scenario:**
A worker node becomes unavailable.

**How I Would Handle It:**

1. Verify node status.
2. Ensure pods are rescheduled.
3. Check autoscaling group health.
4. Replace the failed node.

**Root Cause Example:**
Underlying EC2 instance failure.

**Resolution:**
Node replaced automatically via autoscaling.

---

## 10. Production Rollback Under Pressure

**Scenario:**
A production deployment introduces severe issues.

**How I Would Handle It:**

1. Pause deployments.
2. Roll back to last stable version.
3. Communicate with stakeholders.
4. Perform root cause analysis.

**Root Cause Example:**
Undetected regression in a new feature.

**Resolution:**
Rollback completed, RCA documented, and test coverage improved.

---

## Final Notes

These scenarios demonstrate **real-world production ownership**, including monitoring, troubleshooting, rollback strategies, and communication under pressure. They reflect the responsibilities of a **DevOps / SRE engineer in enterprise systems**.
