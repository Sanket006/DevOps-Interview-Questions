# 🚀 AWS EC2 Interview Questions & Answers (DevOps Focused)

---

## 1️⃣ What is Amazon EC2?

**Answer:**
Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable compute capacity in the cloud. It allows users to launch virtual servers (instances), configure networking, attach storage, and scale resources based on demand.

---

## 2️⃣ What are the main components of EC2?

**Answer:**

* AMI (Amazon Machine Image)
* Instance Types
* EBS (Elastic Block Store)
* Security Groups
* Key Pairs
* Elastic IP
* Auto Scaling
* Load Balancer

---

## 3️⃣ What is an AMI?

**Answer:**
An Amazon Machine Image (AMI) is a template used to launch EC2 instances. It contains:

* Operating System
* Application server (optional)
* Application code (optional)
* Launch permissions
* Block device mapping

---

## 4️⃣ What is the difference between Security Group and NACL?

**Answer:**

| Feature         | Security Group      | NACL                     |
| --------------- | ------------------- | ------------------------ |
| Level           | Instance level      | Subnet level             |
| Stateful        | Yes                 | No                       |
| Allow/Deny      | Allow only          | Allow and Deny           |
| Rule evaluation | All rules evaluated | Rules evaluated in order |

---

## 5️⃣ What are different EC2 instance types?

**Answer:**

* General Purpose (t3, t4g)
* Compute Optimized (c5)
* Memory Optimized (r5)
* Storage Optimized (i3)
* GPU instances (p3)

---

## 6️⃣ What is the difference between EBS and Instance Store?

**Answer:**

| Feature                  | EBS               | Instance Store       |
| ------------------------ | ----------------- | -------------------- |
| Persistence              | Persistent        | Ephemeral            |
| Data survives stop/start | Yes               | No                   |
| Backup                   | Snapshot possible | No snapshot          |
| Use case                 | Databases         | Cache/Temporary data |

---

## 7️⃣ What is an Elastic IP?

**Answer:**
An Elastic IP is a static public IPv4 address designed for dynamic cloud computing. It allows you to remap the address to another instance in case of failure.

---

## 8️⃣ What happens when you stop vs terminate an EC2 instance?

**Answer:**

* Stop: Instance shuts down, EBS volume persists, instance can be restarted.
* Terminate: Instance deleted permanently, root volume deleted (if configured).

---

## 9️⃣ How does Auto Scaling work with EC2?

**Answer:**
Auto Scaling automatically adjusts the number of EC2 instances based on demand using:

* Launch Template
* Auto Scaling Group
* Scaling Policies
* CloudWatch metrics

---

## 🔟 How do you secure an EC2 instance in production?

**Answer:**

* Use Security Groups with least privilege
* Disable password login
* Use IAM roles instead of access keys
* Enable CloudWatch monitoring
* Use private subnets
* Enable EBS encryption
* Regular patching

---

## 1️⃣1️⃣ What is the difference between On-Demand, Reserved, and Spot Instances?

**Answer:**

* On-Demand: Pay per hour/second, no commitment
* Reserved: 1–3 year commitment, cheaper
* Spot: Bid-based, cheapest but can be terminated anytime

---

## 1️⃣2️⃣ What is a Launch Template?

**Answer:**
A Launch Template defines configuration for EC2 instances such as AMI, instance type, key pair, security groups, and user data. It is used by Auto Scaling Groups.

---

## 1️⃣3️⃣ How do you troubleshoot an EC2 instance that is unreachable?

**Answer:**

1. Check instance state
2. Verify Security Group rules
3. Check NACL rules
4. Verify route table and Internet Gateway
5. Check SSH service status
6. Check system logs
7. Verify correct key pair

---

## 1️⃣4️⃣ What happens internally when you launch an EC2 instance?

**Answer:**

1. Request sent to EC2 service
2. Hypervisor allocates host
3. Network interface attached
4. EBS volume attached
5. Security group applied
6. OS boots

---

## 1️⃣5️⃣ How would you design a highly available EC2 architecture?

**Answer:**

* Deploy across multiple Availability Zones
* Use Application Load Balancer
* Use Auto Scaling Group
* Store data in EBS with snapshots or RDS
* Enable health checks

---

# 🔥 Advanced Production-Level Questions

## 1️⃣6️⃣ Your EC2 CPU is 100%. What will you check?

**Answer:**

* top/htop
* CloudWatch metrics
* Application logs
* Scaling policy
* Load balancer traffic

---

## 1️⃣7️⃣ How do you migrate an EC2 instance to another region?

**Answer:**

1. Create AMI
2. Copy AMI to target region
3. Launch new instance
4. Update DNS

---

## 1️⃣8️⃣ How does IAM Role work with EC2?

**Answer:**
IAM Role provides temporary credentials to EC2 instances via Instance Metadata Service, avoiding hardcoded access keys.

---

# 🎯 Interview Tip for DevOps Engineers

Always explain:

* Why you used EC2
* Why specific instance type
* Why specific scaling method
* Cost optimization strategy
* High availability approach

---

# 💡 Pro Tip

In interviews, connect EC2 with:

* VPC
* IAM
* Auto Scaling
* Load Balancer
* CloudWatch
* Terraform
* Docker & Kubernetes

---

End of Document
