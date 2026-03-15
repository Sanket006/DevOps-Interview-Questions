# 🚀 AWS EC2 Interview Questions & Answers (With STAR-Based Scenario Answers)

---

# 🎯 STAR-Based Production Scenarios (EC2)

---

## ⭐ Scenario 1: EC2 Instance Unreachable (SSH Timeout)

### 🟢 Situation:

During a production deployment, the EC2 instance became unreachable via SSH, causing deployment delay.

### 🟢 Task:

Restore connectivity quickly without impacting running application services.

### 🟢 Action:

* Verified instance state (running)
* Checked Security Group inbound rules (port 22 open?)
* Verified NACL rules at subnet level
* Checked route table and Internet Gateway association
* Used EC2 Instance Connect / Session Manager to access instance
* Restarted SSH service (`sudo systemctl restart sshd`)
* Checked disk space using `df -h`

### 🟢 Result:

Identified SSH service crash due to disk space issue. Cleaned logs, restored service within 15 minutes, preventing prolonged downtime.

---

## ⭐ Scenario 2: High CPU Utilization (100%)

### 🟢 Situation:

Production EC2 instance CPU reached 100%, causing application latency.

### 🟢 Task:

Reduce CPU usage and maintain application availability.

### 🟢 Action:

* Checked CloudWatch metrics
* Used `top` and `htop` to identify high process
* Identified memory leak in application
* Restarted service temporarily
* Configured Auto Scaling policy based on CPU > 70%
* Upgraded instance type from t3.medium to t3.large

### 🟢 Result:

Reduced latency by 60% and prevented recurrence by implementing scaling policy.

---

## ⭐ Scenario 3: Designing Highly Available Architecture (99.99%)

### 🟢 Situation:

Application needed high availability with minimal downtime.

### 🟢 Task:

Design EC2 architecture with fault tolerance.

### 🟢 Action:

* Deployed EC2 instances across 2 Availability Zones
* Created Application Load Balancer
* Configured Auto Scaling Group
* Enabled health checks
* Used EBS snapshots for backup
* Implemented CloudWatch alarms

### 🟢 Result:

Achieved 99.99% uptime SLA and zero downtime during instance failure testing.

---

## ⭐ Scenario 4: Spot Instance Termination in Production

### 🟢 Situation:

Spot instance terminated unexpectedly, affecting background jobs.

### 🟢 Task:

Prevent job interruption and improve fault tolerance.

### 🟢 Action:

* Detected termination notice via metadata endpoint
* Implemented lifecycle hooks
* Shifted workload to On-Demand instances
* Used mixed instance policy in Auto Scaling

### 🟢 Result:

Reduced cost by 40% while maintaining workload continuity.

---

## ⭐ Scenario 5: Migrating EC2 to Another Region

### 🟢 Situation:

Business required deployment in another AWS region for latency improvement.

### 🟢 Task:

Migrate infrastructure safely with minimal downtime.

### 🟢 Action:

* Created AMI of running instance
* Copied AMI to target region
* Launched new EC2 instance
* Updated DNS records
* Performed smoke testing

### 🟢 Result:

Successfully migrated with less than 5 minutes of downtime.

---

## ⭐ Scenario 6: Security Breach Prevention

### 🟢 Situation:

Unusual login attempts detected on EC2 instance.

### 🟢 Task:

Secure instance and prevent unauthorized access.

### 🟢 Action:

* Restricted Security Group SSH access to specific IP
* Disabled password authentication
* Enabled IAM role instead of access keys
* Rotated credentials
* Enabled CloudTrail logging

### 🟢 Result:

Prevented further unauthorized attempts and improved security posture.

---

# 🔥 Interviewer Cross-Questions (With Model Answers)

### ❓ Why didn’t you just reboot the instance in Scenario 1?

**Answer:** Rebooting may not fix configuration or disk-related issues. Root cause analysis ensures long-term stability.

### ❓ Why upgrade instance instead of optimizing application?

**Answer:** Immediate mitigation required scaling. Application optimization was planned as long-term fix.

### ❓ Why use Multi-AZ instead of single AZ with backup?

**Answer:** Multi-AZ ensures real-time redundancy and failover without manual intervention.

### ❓ How do you detect Spot termination before it happens?

**Answer:** Use EC2 metadata endpoint and CloudWatch Events to detect 2-minute termination notice.

### ❓ How do you ensure zero downtime during region migration?

**Answer:** Use blue-green deployment strategy and Route53 weighted routing.

---

# 🎯 DevOps Tip

In interviews, always:

* Quantify impact (percentage improvement, downtime reduced)
* Explain monitoring used
* Mention cost optimization
* Mention security considerations

---

End of Document
