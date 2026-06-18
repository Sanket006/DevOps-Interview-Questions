# Advanced Kubernetes & AWS Outage Scenarios (DevOps / SRE Interviews)

This README contains **advanced, real-world Kubernetes (EKS) and AWS production outage scenarios** with **interview-grade answers**. These scenarios are commonly used for **mid-level and SRE-style DevOps interviews** and are aligned with my **hands-on DevOps experience**.

---

## ☸️ Kubernetes Outage Scenarios

### 🔥 Scenario 1: All Pods Running but Application Not Accessible

**Possible Causes:**

* Service misconfiguration
* Ingress/ALB issue
* Network policy restrictions

**Troubleshooting Steps:**

1. Check Service type and selectors
2. Verify endpoints using `kubectl get endpoints`
3. Validate Ingress rules and ALB target groups
4. Check security groups attached to worker nodes
5. Inspect application logs

**Prevention:**

* Use readiness probes
* Validate Ingress rules before deployment
* Automated smoke tests

---

### 🔥 Scenario 2: EKS Nodes Not Joining Cluster

**Possible Causes:**

* IAM role issues
* Security group misconfiguration
* CNI plugin failure

**Troubleshooting Steps:**

1. Verify node IAM role permissions
2. Check node bootstrap logs
3. Validate security group rules
4. Ensure correct AMI and EKS version

**Prevention:**

* Use managed node groups
* Version compatibility checks

---

### 🔥 Scenario 3: Pods Evicted Frequently

**Possible Causes:**

* Node resource pressure
* Incorrect resource requests/limits

**Troubleshooting Steps:**

1. Check node metrics
2. Inspect resource requests and limits
3. Review eviction events
4. Scale cluster or optimize workloads

**Prevention:**

* Proper capacity planning
* Enable cluster autoscaler

---

### 🔥 Scenario 4: Kubernetes Deployment Stuck in Progressing State

**Possible Causes:**

* Failing readiness/liveness probes
* Image pull errors

**Troubleshooting Steps:**

1. Describe deployment
2. Check pod events
3. Validate container image availability
4. Roll back deployment

**Prevention:**

* Test images before prod
* Canary deployments

---

## ☁️ AWS Outage Scenarios

### 🔥 Scenario 5: ALB Healthy but Users Receive 502 Errors

**Possible Causes:**

* Application crash
* Incorrect target port
* Timeout mismatch

**Troubleshooting Steps:**

1. Check ALB target health
2. Verify application logs
3. Validate listener and target group config
4. Increase timeout if needed

**Prevention:**

* Health check tuning
* Graceful shutdown handling

---

### 🔥 Scenario 6: Auto Scaling Not Triggering During Peak Load

**Possible Causes:**

* Incorrect scaling policies
* CloudWatch metric mismatch

**Troubleshooting Steps:**

1. Review scaling policies
2. Validate metrics and alarms
3. Check cooldown periods
4. Test scale-out manually

**Prevention:**

* Load testing
* Multiple scaling metrics

---

### 🔥 Scenario 7: Sudden AWS Cost Spike Overnight

**Possible Causes:**

* Infinite scaling loop
* Logging misconfiguration
* Unused resources

**Troubleshooting Steps:**

1. Review Cost Explorer
2. Check Auto Scaling activity
3. Identify idle resources
4. Stop unnecessary services

**Prevention:**

* Budget alerts
* Resource tagging

---

### 🔥 Scenario 8: EKS API Server Unreachable

**Possible Causes:**

* Network issues
* IAM authentication failure

**Troubleshooting Steps:**

1. Check AWS service health
2. Validate kubeconfig
3. Verify IAM permissions
4. Attempt access from bastion

**Prevention:**

* Multi-region DR strategy
* Access monitoring

---

### 🔥 Scenario 9: S3 Access Suddenly Denied in Production

**Possible Causes:**

* IAM policy changes
* Bucket policy override

**Troubleshooting Steps:**

1. Review recent IAM changes
2. Check bucket policy
3. Validate role attachment

**Prevention:**

* Change approval workflow
* IAM policy versioning

---

### 🔥 Scenario 10: CloudWatch Alarms Not Triggering

**Possible Causes:**

* Incorrect metric namespace
* Alarm misconfiguration

**Troubleshooting Steps:**

1. Verify metric availability
2. Check alarm thresholds
3. Test alarm manually

**Prevention:**

* Alarm testing
* Infrastructure monitoring audits

---

## 🎯 Interview Answer Framework

Use this structure:

1. **Impact control** (rollback / isolate)
2. **Investigation** (logs, metrics, events)
3. **Root cause**
4. **Fix**
5. **Long-term prevention**

---

## 🚀 Next Level Prep

* Multi-region DR failure scenarios
* Chaos engineering questions
* SRE-style on-call simulations

---

**Author:** Sanket Chopade
**Role:** DevOps Engineer
