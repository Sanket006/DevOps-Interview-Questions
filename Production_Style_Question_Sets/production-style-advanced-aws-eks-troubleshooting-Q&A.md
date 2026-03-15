# 🚀 Amazon EKS (AWS Kubernetes) Advanced Troubleshooting – STAR Method

Now we move to Amazon EKS (AWS Kubernetes) Advanced Troubleshooting – STAR Method

These are real production-level scenarios commonly asked in interviews for AWS DevOps / Cloud Engineer roles.

---

## ⭐ Scenario 1: Pods Not Starting in Amazon EKS

### ❓ Interview Question:

“Tell me about a time when pods were not starting in EKS.”

4

### ✅ STAR Answer

**S – Situation:**
In our production cluster running on Amazon Elastic Kubernetes Service, newly deployed pods were stuck in Pending state.

**T – Task:**
Identify why pods were not being scheduled and restore deployment.

**A – Action:**

* Ran kubectl describe pod and saw “Insufficient CPU” error.
* Checked worker node capacity.
* Found Auto Scaling Group had reached max capacity.
* Increased ASG desired count.
* Enabled Cluster Autoscaler.
* Set proper resource requests and limits in deployment YAML.

**R – Result:**
Pods scheduled successfully, and Cluster Autoscaler now scales nodes automatically during high demand.

---

## ⭐ Scenario 2: EKS Node Group Suddenly Not Ready

### ❓ Interview Question:

“What would you do if your EKS node group becomes NotReady?”

4

### ✅ STAR Answer

**S – Situation:**
One of our managed node groups in EKS started showing NotReady nodes.

**T – Task:**
Ensure zero downtime and restore cluster health.

**A – Action:**

* Checked kubectl get nodes for affected nodes.
* Drained problematic nodes.
* Investigated logs via Amazon CloudWatch.
* Found networking issue in VPC CNI plugin.
* Restarted aws-node daemonset.
* Verified Auto Scaling Group health in Amazon EC2.
* Enabled node auto-recovery.

**R – Result:**
Workloads were rescheduled automatically and cluster stability improved.

---

## ⭐ Scenario 3: EKS Application Not Accessible via Load Balancer

### ❓ Interview Question:

“Users can’t access application in EKS. Pods are running fine. What will you check?”

4

### ✅ STAR Answer

**S – Situation:**
Application deployed in EKS was healthy internally but not accessible externally.

**T – Task:**
Identify networking or load balancer issue.

**A – Action:**

* Checked kubectl get svc and verified LoadBalancer type.
* Verified target groups in Elastic Load Balancing.
* Found security group inbound rule missing port 80.
* Checked Ingress controller logs.
* Validated subnets were tagged properly for ALB.

**R – Result:**
After fixing security group rule, application became accessible immediately.

---

## ⭐ Scenario 4: EKS Cluster Upgrade Failure

### ❓ Interview Question:

“Have you handled EKS version upgrade issues?”

4

### ✅ STAR Answer

**S – Situation:**
During EKS minor version upgrade, some deployments failed due to deprecated APIs.

**T – Task:**
Ensure smooth cluster upgrade without downtime.

**A – Action:**

* Checked compatibility matrix before upgrade.
* Identified deprecated API versions in YAML.
* Updated manifests to supported API versions.
* Upgraded node groups gradually.
* Used rolling update strategy.
* Tested in staging before production rollout.

**R – Result:**
Upgrade completed successfully with zero downtime.

---

## ⭐ Scenario 5: EKS Cost Optimization & Over-Provisioning

### ❓ Interview Question:

“How did you optimize cost in EKS?”

### ✅ STAR Answer

**S – Situation:**
Our EKS cluster cost increased significantly due to over-provisioned nodes.

**T – Task:**
Reduce cost without impacting performance.

**A – Action:**

* Analyzed usage using AWS Cost Explorer.
* Identified underutilized EC2 instances.
* Switched workloads to Spot Instances.
* Implemented Cluster Autoscaler.
* Right-sized node instance types.
* Cleaned unused Load Balancers.

**R – Result:**
Reduced infrastructure cost by 30% while maintaining performance.

---

## ⭐ Scenario 6: IAM Role for Service Account (IRSA) Misconfiguration

### ❓ Interview Question:

“Application cannot access S3 from EKS pod. What would you check?”

4

### ✅ STAR Answer

**S – Situation:**
Application pod needed to access S3 but was getting AccessDenied error.

**T – Task:**
Fix IAM authentication securely without embedding credentials.

**A – Action:**

* Verified ServiceAccount configuration.
* Checked IAM role trust relationship.
* Validated IRSA configuration.
* Ensured correct policy attached for Amazon S3.
* Restarted deployment after fix.

**R – Result:**
Application accessed S3 securely using IAM role without static credentials.

---

# 🔥 Advanced Interview Strategy for EKS

When answering EKS questions:

Always mention:

* Cluster Autoscaler
* IAM Roles for Service Accounts (IRSA)
* VPC & Security Groups
* CloudWatch monitoring
* Rolling updates
* Zero downtime approach
