# 🚀 Sanket Chopade – DevOps Interview README

> Based on Resume Experience at Hisan Labs Private Limited
> Role: DevOps Engineer Intern
> Location: Pune, India

---

# 🔍 AWS Follow-Up Questions & Answers

---

## 1️⃣ How did you design your VPC architecture? Explain CIDR planning.

**Answer:**

In my project, I designed a custom VPC architecture using Terraform with proper CIDR planning to support multi-tier applications.

I selected a VPC CIDR block of `10.0.0.0/16` to allow sufficient IP space for future scaling. Then I divided it into smaller subnet ranges:

* Public Subnets: `10.0.1.0/24`, `10.0.2.0/24`
* Private App Subnets: `10.0.3.0/24`, `10.0.4.0/24`
* Private DB Subnets: `10.0.5.0/24`, `10.0.6.0/24`

Each subnet was distributed across multiple Availability Zones to ensure high availability.

This design allowed:

* Isolation of application and database layers
* Better security control
* Easy scaling
* Future subnet expansion

All networking components (VPC, subnets, route tables, IGW, NAT) were provisioned using modular Terraform.

---

## 2️⃣ Why did you choose public vs private subnets?

**Answer:**

I followed a three-tier architecture approach.

* Public Subnets:

  * Used for Application Load Balancer
  * Used for NAT Gateway
  * Attached to Internet Gateway

* Private Subnets:

  * Application servers (EKS worker nodes)
  * Database layer

Reason:

* Security best practice — backend servers should not be directly accessible from the internet.
* Only ALB receives public traffic.
* Private instances access internet through NAT Gateway for updates.

This minimized attack surface and improved overall security posture.

---

## 3️⃣ How did you configure NAT Gateway?

**Answer:**

I created the NAT Gateway in a public subnet and attached an Elastic IP.

Steps:

1. Created Internet Gateway and attached to VPC
2. Created public subnet with route to IGW
3. Deployed NAT Gateway in public subnet
4. Updated private subnet route table:
   `0.0.0.0/0 → NAT Gateway`

This allowed private instances to access the internet securely without being publicly exposed.

---

## 4️⃣ How does an Application Load Balancer work internally?

**Answer:**

Application Load Balancer works at Layer 7 (HTTP/HTTPS).

Internally:

1. Receives request
2. Evaluates listener rules
3. Performs path-based or host-based routing
4. Forwards request to appropriate target group
5. Performs health checks continuously

It supports:

* SSL termination
* Sticky sessions
* WebSocket
* Integration with EKS

In my project, ALB distributed traffic across multiple EC2 instances across Availability Zones.

---

## 5️⃣ Difference between ALB and NLB?

**Answer:**

ALB:

* Layer 7
* Path-based routing
* Host-based routing
* HTTP/HTTPS only

NLB:

* Layer 4
* TCP/UDP
* Ultra-low latency
* Static IP support

I used ALB because my application required path-based routing for microservices.

---

## 6️⃣ How did Auto Scaling decide when to scale?

**Answer:**

I configured Target Tracking Scaling Policies based on:

* CPU Utilization (threshold: 70%)
* Request Count per Target

When CPU exceeded 70% for 5 minutes:

* Scale-out triggered

When CPU dropped below 40%:

* Scale-in triggered

CloudWatch metrics were connected to Auto Scaling policies.

---

## 7️⃣ What CloudWatch metrics did you use?

**Answer:**

For EC2:

* CPUUtilization
* NetworkIn/NetworkOut
* StatusCheckFailed

For ALB:

* TargetResponseTime
* RequestCount
* HTTP 5xx errors

For EKS:

* Pod CPU/Memory usage (via Prometheus)

These metrics helped reduce MTTR by 30%.

---

## 8️⃣ How did you reduce AWS cost by 25%? Give exact actions.

**Answer:**

I reduced cost through:

1. Right-sizing EC2 instances based on utilization metrics
2. Implemented Auto Scaling (avoid over-provisioning)
3. Enabled S3 lifecycle policies (move to Glacier)
4. Used Spot Instances for non-production workloads
5. Optimized ALB idle timeout
6. Deleted unused EBS volumes and snapshots

Combined impact reduced infrastructure cost by approximately 25%.

---

## 9️⃣ How would you redesign this for multi-region deployment?

**Answer:**

For multi-region architecture:

1. Deploy infrastructure in two regions
2. Use Route 53 latency-based routing
3. Enable cross-region replication (S3)
4. Use multi-region EKS clusters
5. Implement RDS Multi-AZ or Aurora Global Database
6. Store Terraform state in remote backend (S3 + DynamoDB locking)

This would ensure disaster recovery and low latency globally.

---

## 🔟 What happens when an EC2 instance fails?

**Answer:**

When EC2 fails:

1. ALB health check marks instance unhealthy
2. Traffic stops routing to that instance
3. Auto Scaling detects failure
4. New instance is launched automatically
5. Instance registers to target group

This ensures high availability with minimal downtime.

---

# ✅ End of AWS Section

---

# 🔍 Terraform Deep Follow-Up Questions & Answers

---

## 1️⃣ What is a Terraform module?

**Answer:**

A Terraform module is a reusable and self-contained set of Terraform configuration files that define a specific infrastructure component.

For example, instead of writing VPC, subnets, route tables repeatedly, I created a reusable `vpc` module.

In my project, I created modules like:

* vpc
* ec2
* eks
* alb
* autoscaling

Each module had:

* main.tf
* variables.tf
* outputs.tf

This helped:

* Avoid code duplication
* Maintain consistency across dev/test/prod
* Improve scalability and maintainability

---

## 2️⃣ How did you manage remote state?

**Answer:**

I used an S3 backend for remote state storage.

Benefits:

* Centralized state file
* Team collaboration
* Prevent state corruption

Terraform backend configuration included:

* S3 bucket
* State file path
* DynamoDB table for locking

This ensured safe multi-user access.

---

## 3️⃣ Where did you store your tfstate file?

**Answer:**

I stored the tfstate file in:

* Amazon S3 bucket (versioning enabled)

This provided:

* Backup via versioning
* Recovery from accidental deletion
* Secure storage with IAM restrictions

---

## 4️⃣ How did you handle state locking?

**Answer:**

I used DynamoDB for state locking.

When Terraform runs:

* It creates a lock entry in DynamoDB
* Prevents other users from running apply
* Releases lock after execution

This prevents race conditions and infrastructure corruption.

---

## 5️⃣ Difference between terraform refresh and terraform apply?

**Answer:**

terraform refresh:

* Updates state file to match real infrastructure
* Does NOT change infrastructure

terraform apply:

* Executes changes required to match configuration
* Creates/updates/deletes infrastructure

In short:
refresh = sync state
apply = make changes

---

## 6️⃣ How do you manage secrets in Terraform?

**Answer:**

I avoided hardcoding secrets.

Methods used:

* AWS Secrets Manager
* Environment variables
* Terraform variable files (.tfvars) excluded via .gitignore
* IAM roles instead of static credentials

Sensitive variables were marked as:
`sensitive = true`

This ensured security best practices.

---

## 7️⃣ What happens if two engineers run apply at the same time?

**Answer:**

Because we used DynamoDB state locking:

* First engineer acquires lock
* Second engineer gets lock error
* Terraform blocks execution

This prevents inconsistent infrastructure state.

---

## 8️⃣ How do you destroy only one resource?

**Answer:**

Using targeted destroy:

terraform destroy -target=aws_instance.example

However, I use this cautiously because it may create dependency issues.

Preferred approach:

* Remove resource from code
* Run terraform plan
* Then terraform apply

---

## 9️⃣ How did you structure your folder hierarchy?

**Answer:**

My Terraform structure:

infrastructure/
modules/
vpc/
eks/
alb/
ec2/
environments/
dev/
main.tf
variables.tf
test/
prod/

Each environment had:

* Separate backend configuration
* Separate variable files

This allowed environment isolation and safe production deployments.

---

# ✅ End of Terraform Section

---

# 🔍 Kubernetes Follow-Up Questions & Answers

---

## 1️⃣ Explain Kubernetes architecture.

**Answer:**

Kubernetes architecture consists of Control Plane and Worker Nodes.

Control Plane Components:

* API Server → Entry point for all requests
* etcd → Stores cluster state
* Scheduler → Assigns pods to nodes
* Controller Manager → Maintains desired state

Worker Node Components:

* Kubelet → Communicates with API server
* Kube-proxy → Manages networking rules
* Container Runtime (Docker/containerd)

In EKS, the control plane is managed by AWS, and we manage worker nodes.

---

## 2️⃣ What happens internally when you run kubectl apply?

**Answer:**

1. kubectl sends request to API Server.
2. API Server validates manifest.
3. Configuration stored in etcd.
4. Scheduler assigns pod to a node.
5. Kubelet pulls container image.
6. Pod starts running.

Kubernetes continuously compares desired state vs actual state.

---

## 3️⃣ How does Kubernetes scheduler work?

**Answer:**

Scheduler works in two phases:

1. Filtering Phase:

   * Filters nodes based on CPU, memory, taints, affinity rules.

2. Scoring Phase:

   * Scores remaining nodes.
   * Selects best node.

Then pod is bound to selected node.

---

## 4️⃣ What is kube-proxy?

**Answer:**

Kube-proxy runs on each node.

It:

* Maintains iptables or IPVS rules
* Routes traffic to correct pod
* Enables service abstraction

It ensures load balancing between pods.

---

## 5️⃣ How does DNS resolution work inside cluster?

**Answer:**

Kubernetes uses CoreDNS.

When a pod calls a service:

* DNS query goes to CoreDNS
* CoreDNS resolves service name to ClusterIP
* Kube-proxy forwards traffic to backend pod

Example:
myservice.default.svc.cluster.local

---

## 6️⃣ Difference between ClusterIP, NodePort, LoadBalancer?

**Answer:**

ClusterIP:

* Default
* Internal communication only

NodePort:

* Exposes service on node port
* Accessible via NodeIP:Port

LoadBalancer:

* Provisions cloud load balancer
* Public access

In EKS, LoadBalancer creates AWS ELB.

---

## 7️⃣ How does rolling update work internally?

**Answer:**

Rolling update:

1. Creates new ReplicaSet
2. Gradually increases new pods
3. Gradually decreases old pods
4. Maintains availability

Controlled by:

* maxUnavailable
* maxSurge

This ensures zero downtime.

---

## 8️⃣ What happens if a pod crashes?

**Answer:**

If pod crashes:

* Kubelet detects failure
* Restarts container
* If part of ReplicaSet → new pod created
* If node fails → pod rescheduled on another node

Self-healing is automatic.

---

## 9️⃣ How did you manage secrets?

**Answer:**

I used:

* Kubernetes Secrets
* AWS Secrets Manager integration

Secrets were:

* Base64 encoded
* Mounted as environment variables
* Restricted using RBAC

Avoided hardcoding credentials in images.

---

## 🔟 How did you expose your application externally?

**Answer:**

I used:

* Kubernetes Service type: LoadBalancer
* AWS ALB Ingress Controller

Flow:
Internet → ALB → Target Group → Node → Pod

This provided SSL termination and path-based routing.

---

# 🔥 Trap Question: How does Kubernetes networking work between pods on different nodes?

**Answer:**

Kubernetes uses a CNI (Container Network Interface) plugin.

In EKS, AWS VPC CNI assigns:

* Each pod gets an IP from VPC
* Pods communicate directly using VPC networking

Between different nodes:

1. Pod A sends packet
2. Routed via VPC network
3. Reaches Pod B IP directly

There is:

* No NAT between pods
* Flat network model
* Every pod can communicate with every other pod (unless NetworkPolicies applied)

This ensures efficient and scalable networking.

---

# ✅ End of Kubernetes Section

---

# 🔍 CI/CD (Jenkins) Follow-Up Questions & Answers

---

## 1️⃣ What were the pipeline stages?

**Answer:**

Our Jenkins pipeline had the following stages:

1. Code Checkout (from GitHub)
2. Build Stage (Maven / npm depending on service)
3. Unit Testing
4. Static Code Analysis (SonarQube)
5. Docker Image Build
6. Push Image to ECR
7. Deploy to EKS
8. Post-Deployment Verification

This automation reduced deployment time from 45 minutes to around 10 minutes.

---

## 2️⃣ Was it declarative or scripted pipeline?

**Answer:**

We used Declarative Pipeline.

Reason:

* Easier readability
* Structured syntax
* Built-in post conditions
* Better for team collaboration

Example structure:

* pipeline {}
* agent {}
* stages {}
* post {}

Declarative pipelines are more maintainable for production teams.

---

## 3️⃣ How did you integrate Jenkins with GitHub?

**Answer:**

Integration steps:

1. Installed GitHub plugin in Jenkins
2. Configured webhook in GitHub
3. Webhook triggered Jenkins on every push
4. Jenkins pulled code using Git credentials

This enabled automatic CI on every commit.

---

## 4️⃣ How did you manage credentials?

**Answer:**

I used Jenkins Credentials Manager.

Stored securely:

* AWS access keys (if required)
* Docker registry credentials
* GitHub tokens

Used credentials binding plugin to inject secrets into pipeline.

Secrets were never hardcoded in Jenkinsfile.

---

## 5️⃣ What happens if a pipeline fails at stage 3?

**Answer:**

If pipeline fails at stage 3 (e.g., testing stage):

* Pipeline stops execution
* Deployment does not proceed
* Notification sent via email/Slack

This ensures broken code never reaches production.

Developers fix issue and re-trigger pipeline.

---

## 6️⃣ How did you implement blue-green deployment?

**Answer:**

Blue-Green strategy:

1. Existing version = Blue
2. New version deployed as Green
3. Health checks performed
4. Switch traffic using:

   * Kubernetes service update OR
   * ALB target group switch

If failure occurs → rollback to Blue immediately.

This ensured zero downtime releases.

---

## 7️⃣ How did you implement rolling deployment?

**Answer:**

Rolling deployment used Kubernetes Deployment strategy.

Controlled using:

* maxUnavailable
* maxSurge

Jenkins updated image tag in deployment YAML.

Kubernetes gradually replaced old pods with new pods.

Ensured high availability during updates.

---

## 8️⃣ What is the difference between CI and CD?

**Answer:**

CI (Continuous Integration):

* Automatically build and test code on every commit

CD (Continuous Delivery/Deployment):

* Automatically deploy code to staging or production

CI ensures code quality.
CD ensures faster release cycle.

---

## 9️⃣ How would you design a CI/CD pipeline for microservices?

**Answer:**

For microservices architecture:

1. Separate pipeline per service
2. Dockerize each service
3. Push images to ECR
4. Use Helm or Kustomize for deployments
5. Implement version tagging
6. Add automated tests (unit + integration)
7. Use parallel stages to reduce time
8. Add approval gate for production

Optional enhancements:

* Canary deployment
* Blue-Green deployment
* Automated rollback

This ensures independent deployment and scalability.

---

# ✅ End of CI/CD Section

---

# 🔍 Docker Follow-Up Questions & Answers

---

## 1️⃣ What is the difference between CMD and ENTRYPOINT?

**Answer:**

Both CMD and ENTRYPOINT define the default command that runs when a container starts, but they behave differently.

ENTRYPOINT:

* Defines the main command
* Cannot be overridden easily
* Used when container should always run a specific executable

CMD:

* Provides default arguments to ENTRYPOINT
* Can be overridden at runtime

Example:

ENTRYPOINT ["python"]
CMD ["app.py"]

If we run:

docker run image new.py

It overrides CMD but not ENTRYPOINT.

---

## 2️⃣ What is a multi-stage Docker build?

**Answer:**

Multi-stage build allows using multiple FROM statements in one Dockerfile.

Purpose:

* Separate build environment from runtime environment
* Reduce final image size

Example:

Stage 1:

* Use Maven image to build jar file

Stage 2:

* Use lightweight JRE image
* Copy only jar file

This removes unnecessary build tools from final image.

---

## 3️⃣ How do you reduce Docker image size?

**Answer:**

Methods I used:

1. Multi-stage builds
2. Use minimal base images (Alpine)
3. Remove unnecessary packages
4. Use .dockerignore file
5. Combine RUN commands
6. Avoid storing logs/temp files

Smaller images improve:

* Faster pull time
* Faster deployment
* Lower storage cost

---

## 4️⃣ What is Docker layer caching?

**Answer:**

Docker builds images in layers.

Each instruction in Dockerfile creates a new layer.

If no change in instruction:

* Docker reuses cached layer
* Speeds up build process

Best practice:

* Put frequently changing code at bottom
* Put dependency installation at top

This optimizes build time.

---

## 5️⃣ Difference between Docker Compose and Kubernetes?

**Answer:**

Docker Compose:

* Used for local development
* Defines multi-container apps
* Runs on single host

Kubernetes:

* Production-grade orchestration
* Auto-scaling
* Self-healing
* Load balancing
* Multi-node cluster

In my project:

* Docker Compose for local testing
* Kubernetes (EKS) for production

---

## 6️⃣ How does Docker networking work?

**Answer:**

Docker provides different network drivers:

1. Bridge (default)

   * Containers communicate within same host

2. Host

   * Shares host network stack

3. Overlay

   * Multi-host communication (Swarm)

4. None

   * No networking

In Kubernetes environment:

* Networking handled by CNI plugin
* Pods communicate via cluster network

Docker assigns:

* Each container gets IP address
* DNS-based service discovery

---

# ✅ End of Docker Section

---

# 🔍 Monitoring & Production Support Questions

---

## 1️⃣ What is MTTR?

**Answer:**

MTTR stands for Mean Time To Recovery (or Resolve).

It is the average time taken to recover a system from a failure and restore normal operations.

Formula:

MTTR = Total Downtime / Number of Incidents

In our project, we reduced MTTR by 30% by:

* Proactive alerting
* Better dashboards
* Automated recovery using Auto Scaling
* Clear incident response SOP

Lower MTTR means faster recovery and better reliability.

---

## 2️⃣ What metrics did you monitor?

**Answer:**

We monitored at three levels:

Infrastructure Level:

* CPU Utilization
* Memory Usage
* Disk I/O
* Network In/Out

Application Level:

* Response Time
* Error Rate (HTTP 5xx)
* Request Count

Kubernetes Level:

* Pod CPU/Memory usage
* Pod restarts
* Node status

These metrics helped us detect performance issues early.

---

## 3️⃣ How did you configure Prometheus?

**Answer:**

We deployed Prometheus inside EKS cluster using Helm.

Configuration steps:

* Installed Prometheus via Helm chart
* Enabled kube-state-metrics
* Enabled node exporter
* Configured ServiceMonitors for application metrics

Prometheus scraped metrics at defined intervals.

We integrated Grafana for dashboard visualization.

---

## 4️⃣ What kind of alerts did you create?

**Answer:**

We created alerts for:

Critical Alerts:

* CPU > 80% for 5 minutes
* Memory usage > 85%
* Pod crash loop
* Node not ready

Application Alerts:

* HTTP 5xx error spike
* Response time threshold breach

Alerts were configured in:

* CloudWatch (for AWS services)
* Prometheus Alertmanager (for Kubernetes)

Notifications sent to email and Slack.

---

## 5️⃣ What happens if CPU is 100%?

**Answer:**

If CPU hits 100%:

1. Alert is triggered
2. Auto Scaling policy may launch new instance
3. Load redistributed by ALB

If not scaling issue:

* Check running processes
* Check memory pressure
* Check application logs
* Investigate traffic spike

If inside Kubernetes:

* Pod may get throttled
* HPA may scale new pods

Immediate action depends on root cause.

---

## 6️⃣ How do you debug a slow application?

**Answer:**

My debugging approach:

Step 1: Check Monitoring Dashboard

* CPU/Memory spike?
* High response time?

Step 2: Check Logs

* Application logs
* Container logs

Step 3: Check Infrastructure

* Network latency
* Database performance

Step 4: Check recent deployment

* Rollback if required

Step 5: Perform load testing if needed

Systematic debugging reduces downtime.

---

## 7️⃣ Difference between black-box and white-box monitoring?

**Answer:**

Black-Box Monitoring:

* External view
* Tests system from user perspective
* Example: Uptime checks

White-Box Monitoring:

* Internal metrics
* CPU, memory, request rate
* Application instrumentation

Both are important:

* Black-box detects user-facing issues
* White-box helps identify root cause

---

# ✅ End of Monitoring & Production Support Section

---

# 🔍 Security & DevSecOps Follow-Up Questions

---

## 1️⃣ How do IAM roles work?

**Answer:**

IAM Roles provide temporary security credentials to AWS services or users without sharing long-term access keys.

How it works:

1. Role contains a set of permissions (IAM policy).
2. Entity (EC2, Lambda, user, or another account) assumes the role.
3. AWS Security Token Service (STS) provides temporary credentials.
4. Service performs actions based on role permissions.

In my project:

* EC2 instances had IAM roles attached.
* EKS worker nodes used IAM roles.
* Avoided hardcoding access keys.

This follows least-privilege and secure access principles.

---

## 2️⃣ Difference between IAM user, group, and role?

**Answer:**

IAM User:

* Individual identity
* Has long-term credentials

IAM Group:

* Collection of users
* Used to assign common permissions

IAM Role:

* No long-term credentials
* Assumed temporarily by users or services

Best practice:

* Avoid using root account
* Use roles for services
* Assign users to groups

---

## 3️⃣ What is Assume Role?

**Answer:**

Assume Role means temporarily taking permissions of another role.

Example use cases:

* Cross-account access
* Temporary elevated permissions
* CI/CD pipeline accessing production

Process:

1. User calls STS AssumeRole API
2. Gets temporary credentials
3. Uses them for limited duration

This improves security and auditability.

---

## 4️⃣ How did you secure EKS?

**Answer:**

EKS Security measures implemented:

1. Private worker nodes (no public IP)
2. IAM roles for service accounts (IRSA)
3. RBAC policies for user access
4. Network policies to restrict pod communication
5. Enabled encryption at rest (EBS)
6. Restricted API server access via security groups

This minimized attack surface.

---

## 5️⃣ How did you manage secrets?

**Answer:**

Secrets were managed using:

* AWS Secrets Manager
* Kubernetes Secrets
* IAM roles instead of static credentials

Secrets were:

* Not stored in Git
* Encrypted at rest
* Access restricted via RBAC

This ensured secure secret handling.

---

## 6️⃣ What is VPC segmentation?

**Answer:**

VPC segmentation means logically separating infrastructure components into different subnets.

Example:

* Public subnet → ALB
* Private subnet → Application servers
* Private DB subnet → Database

Benefits:

* Improved security
* Reduced lateral movement risk
* Better traffic control using route tables

---

## 7️⃣ Difference between Security Group and NACL?

**Answer:**

Security Group:

* Stateful
* Instance-level firewall
* Allows only allow rules

NACL (Network ACL):

* Stateless
* Subnet-level firewall
* Supports allow and deny rules

Security Group is primary control.
NACL provides additional layer of security.

---

## 8️⃣ How would you secure a production AWS account?

**Answer:**

Steps to secure production AWS account:

1. Enable MFA for root and IAM users
2. Disable root API access
3. Implement least-privilege IAM policies
4. Use IAM roles instead of access keys
5. Enable CloudTrail logging
6. Enable GuardDuty and Security Hub
7. Use AWS Config for compliance monitoring
8. Enable VPC Flow Logs
9. Encrypt data at rest (EBS, S3, RDS)
10. Regular security audits and patching

This ensures strong cloud security posture.

---

# ✅ End of Security & DevSecOps Section

---

# 🔍 Linux & Scripting Follow-Up Questions

---

## 1️⃣ Write a script to monitor disk usage.

**Answer:**

Below is a simple Bash script to monitor disk usage and send alert if usage exceeds 80%:

```bash
#!/bin/bash

THRESHOLD=80
USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$USAGE" -gt "$THRESHOLD" ]; then
  echo "Disk usage is above threshold: $USAGE%"
else
  echo "Disk usage is normal: $USAGE%"
fi
```

This script:

* Checks root partition usage
* Compares with threshold
* Prints alert message

In production, this can be integrated with cron or monitoring tools.

---

## 2️⃣ What is crontab?

**Answer:**

Crontab is a Linux utility used to schedule recurring tasks.

It allows execution of scripts or commands at specific intervals.

Example:

```
0 2 * * * /home/user/backup.sh
```

This runs backup script every day at 2 AM.

Used for:

* Backups
* Log cleanup
* Health checks

---

## 3️⃣ Difference between soft link and hard link?

**Answer:**

Hard Link:

* Points directly to inode
* Cannot link directories
* Cannot cross file systems
* Deleting original file does not remove link

Soft Link (Symbolic Link):

* Points to file path
* Can link directories
* Can cross file systems
* If original deleted, link becomes broken

Command:

```
ln file1 file2      # hard link
ln -s file1 link1   # soft link
```

---

## 4️⃣ How does process management work?

**Answer:**

Linux manages processes using:

* PID (Process ID)
* Parent-child relationship

Key commands:

* ps aux
* top / htop
* kill
* nice / renice

When a process starts:

* Kernel assigns PID
* Allocates memory
* Schedules CPU time

If process crashes:

* It becomes zombie if parent doesn't collect status

Process scheduling ensures fair CPU distribution.

---

## 5️⃣ What happens when you run chmod 777?

**Answer:**

chmod 777 gives:

* Read (r)
* Write (w)
* Execute (x)

Permissions to:

* Owner
* Group
* Others

This means anyone can modify or execute the file.

It is highly insecure in production.

Best practice:

* Use least privilege permissions like 644 or 755.

---

## 6️⃣ How do you find memory-consuming processes?

**Answer:**

Commands used:

1. top
2. htop
3. ps aux --sort=-%mem

Example:

```
ps aux --sort=-%mem | head
```

This lists top memory-consuming processes.

Then I analyze:

* Application logs
* Memory leaks
* Restart if necessary

---

# ✅ End of Linux & Scripting Section

---

# 🔍 Availability & Architecture Deep Questions

---

## 1️⃣ What does 99.9% uptime mean in minutes?

**Answer:**

99.9% uptime means the system is allowed only 0.1% downtime.

Total minutes in 1 year:

365 × 24 × 60 = 525,600 minutes

0.1% of 525,600 = 525.6 minutes

So:

99.9% uptime allows approximately 525 minutes (~8.7 hours) of downtime per year.

Higher availability levels:

* 99.99% → ~52 minutes per year
* 99.999% → ~5 minutes per year

---

## 2️⃣ How do you calculate SLA?

**Answer:**

SLA (Service Level Agreement) is calculated as:

SLA = (Total Uptime / Total Time) × 100

Example:
If system was down for 2 hours in a month:

Total minutes in 30 days = 43,200
Downtime = 120 minutes

SLA = ((43,200 − 120) / 43,200) × 100

= 99.72%

Monitoring tools and CloudWatch logs were used to measure uptime.

---

## 3️⃣ Difference between High Availability and Fault Tolerance?

**Answer:**

High Availability (HA):

* System remains operational with minimal downtime
* Uses redundancy (Multi-AZ)
* Some small interruption may occur

Fault Tolerance:

* System continues without interruption
* No downtime
* Fully redundant components running simultaneously

HA reduces downtime.
Fault tolerance eliminates downtime.

---

## 4️⃣ How did you eliminate single points of failure?

**Answer:**

We removed single points of failure by:

1. Deploying instances across multiple Availability Zones
2. Using Application Load Balancer
3. Implementing Auto Scaling Groups
4. Using Multi-AZ database setup
5. Storing Terraform state remotely (S3 + DynamoDB)
6. Using managed EKS control plane

This ensured system resilience.

---

## 5️⃣ How would you design for 99.99% availability?

**Answer:**

To design for 99.99% availability:

1. Multi-AZ deployment
2. Multi-region disaster recovery setup
3. Route 53 health checks with failover routing
4. Database replication (Aurora Global / Cross-region replica)
5. Auto Scaling for compute
6. Infrastructure as Code for fast recovery
7. Continuous monitoring & automated alerts
8. Blue-Green or Canary deployments

For even higher reliability:

* Chaos testing
* Automated failover testing
* Zero-downtime patching

This architecture minimizes downtime and ensures business continuity.

---

# ✅ End of Availability & Architecture Section

---

# 🔥 Behavioral & Ownership Trap Questions

---

## 1️⃣ What exactly did YOU do vs Senior DevOps Engineer?

**Answer:**

As a DevOps Intern, I worked under a Senior DevOps Engineer, but I was responsible for executing and managing several core tasks independently.

What I did:

* Wrote and maintained Terraform modules for VPC, EC2, ALB, and Auto Scaling
* Configured Jenkins pipelines
* Containerized microservices using Docker
* Deployed applications to EKS
* Configured CloudWatch dashboards and alerts
* Automated Linux tasks using Bash

What Senior DevOps Engineer handled:

* Final architecture decisions
* Production approval and change management
* Security policy reviews
* Multi-region design discussions

I contributed hands-on implementation while learning architecture-level thinking from the senior.

---

## 2️⃣ What was your biggest mistake?

**Answer:**

One mistake I made was deploying a configuration change without validating resource limits in Kubernetes.

What happened:

* Pods started restarting due to memory limits
* Alert was triggered

How I handled it:

* Checked logs and metrics
* Identified memory misconfiguration
* Updated limits and redeployed

What I learned:

* Always validate resource requests/limits
* Use staging testing before production
* Review configurations carefully

It helped me become more cautious and process-driven.

---

## 3️⃣ Tell me about a production incident you handled.

**Answer:**

One incident involved sudden CPU spike on application servers.

Issue:

* CPU reached 95%
* Response time increased

Actions taken:

1. Checked CloudWatch metrics
2. Verified traffic spike
3. Confirmed Auto Scaling triggered correctly
4. Identified inefficient query in backend
5. Coordinated with backend team

Temporary fix:

* Increased Auto Scaling capacity

Permanent fix:

* Backend query optimization

Result:

* System stabilized
* MTTR reduced due to clear troubleshooting steps

---

## 4️⃣ What would you improve in that architecture?

**Answer:**

Improvements I would suggest:

1. Implement multi-region disaster recovery
2. Add canary deployments
3. Add centralized logging using ELK stack
4. Introduce Infrastructure testing (Terratest)
5. Improve cost monitoring using AWS Budgets

Architecture was stable, but there is always room for optimization and resilience improvement.

---

## 5️⃣ If I give you a blank AWS account, what will you do first?

**Answer:**

If given a blank AWS account, my first steps would be:

1. Secure the account

   * Enable MFA for root
   * Disable root API access
   * Create IAM admin role

2. Enable logging and monitoring

   * Enable CloudTrail
   * Enable AWS Config
   * Set up GuardDuty

3. Design network foundation

   * Create VPC with proper CIDR planning
   * Create public/private subnets
   * Attach Internet Gateway and NAT

4. Set up IAM roles for services

5. Define Infrastructure as Code baseline using Terraform

Security and foundational architecture come before deploying applications.

---

# ✅ End of Behavioral & Ownership Section

---

# 🎯 Most Dangerous Real Interview Question

# "Explain your entire project architecture from end to end."

---

## ✅ How I Explain It in Interview (Structured & Confident)

**Answer:**

Sure. I worked on a multi-tier, cloud-native e-commerce application deployed on AWS using DevOps best practices. I’ll explain the architecture layer by layer.

---

## 1️⃣ User & DNS Layer

• Users access the application through a domain configured in Route 53.
• Route 53 routes traffic to the Application Load Balancer (ALB).
• CloudFront was used for caching static content to reduce latency.

Flow:
User → Route 53 → CloudFront → ALB

---

## 2️⃣ Networking Layer (VPC Design)

We designed a custom VPC using Terraform.

• VPC CIDR: 10.0.0.0/16
• Public Subnets → ALB, NAT Gateway
• Private Subnets → EKS Worker Nodes
• Private DB Subnets → Database Layer

Security:
• Internet Gateway attached to public subnets
• NAT Gateway allowed private instances to access internet securely
• Security Groups & NACL for traffic control

Multi-AZ deployment ensured high availability.

---

## 3️⃣ Compute Layer

We used Amazon EKS for container orchestration.

• Microservices were containerized using Docker
• Images stored in Amazon ECR
• EKS worker nodes deployed in private subnets
• Horizontal Pod Autoscaler for scaling

Kubernetes managed:
• Pod scheduling
• Rolling updates
• Self-healing
• Service discovery

---

## 4️⃣ Load Balancing Layer

Application Load Balancer (ALB):

• Layer 7 load balancing
• Path-based routing for microservices
• Health checks
• SSL termination

Traffic flow:
ALB → Target Group → EKS Node → Pod

---

## 5️⃣ Database Layer

• Multi-AZ managed database (RDS/Aurora)
• Placed in private DB subnet
• Encrypted at rest
• Automated backups enabled

This ensured high availability and durability.

---

## 6️⃣ CI/CD Pipeline

We used Jenkins for automation.

Pipeline stages:

1. Code checkout from GitHub
2. Build & Unit Test
3. SonarQube Scan
4. Docker Image Build
5. Push to ECR
6. Deploy to EKS

Deployment strategy:
• Rolling updates
• Blue-Green for major releases

This reduced deployment time from 45 minutes to 10 minutes.

---

## 7️⃣ Infrastructure as Code

• Terraform used for provisioning
• Modular structure (VPC, EKS, ALB, ASG)
• Remote state stored in S3
• State locking via DynamoDB

This reduced manual configuration errors by 60%.

---

## 8️⃣ Monitoring & Logging

Monitoring stack:
• AWS CloudWatch (infrastructure metrics)
• Prometheus (Kubernetes metrics)
• Grafana dashboards

Alerts configured for:
• CPU spike
• Memory usage
• Pod crash
• HTTP 5xx errors

This helped reduce MTTR by 30%.

---

## 9️⃣ Security & DevSecOps

• IAM least privilege policies
• IAM roles for EC2 & EKS
• RBAC inside Kubernetes
• Private subnets for backend
• Secrets managed via AWS Secrets Manager
• Encryption enabled (EBS, S3)

Security was integrated at every layer.

---

## 🔟 High Availability & Scalability

• Multi-AZ deployment
• Auto Scaling Groups
• Horizontal Pod Autoscaler
• ALB health checks
• Self-healing Kubernetes pods

System achieved 99.9% availability.

---

# 🎯 Final Closing Line (Very Important in Interview)

"Overall, the architecture was designed to be secure, scalable, highly available, and fully automated using Infrastructure as Code and CI/CD best practices. My primary contribution was implementing Terraform modules, configuring CI/CD pipelines, managing container deployments on EKS, and monitoring production workloads while collaborating with a Senior DevOps Engineer on architectural decisions."

---

# ✅ End of End-to-End Architecture Explanation
