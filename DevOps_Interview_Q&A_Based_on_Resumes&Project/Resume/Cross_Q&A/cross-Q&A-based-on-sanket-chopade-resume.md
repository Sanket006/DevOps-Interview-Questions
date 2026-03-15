# DevOps Interview Q&A – Sanket Chopade

(Based on Resume Experience – Hisan Labs Private Limited)

---

# 🔥 1. AWS Infrastructure Cross-Questions

---

## 🔹 VPC Design Questions

### Q1: How many subnets did you create and why?

**Answer:**
In our production VPC, I created **6 subnets across 2 Availability Zones** to ensure high availability.

* 2 Public Subnets (1 per AZ) → For ALB and NAT Gateway
* 2 Private App Subnets → For EC2 / EKS worker nodes
* 2 Private DB Subnets → For database layer

We followed a multi-tier architecture (Web → App → DB) to isolate traffic and improve security.

---

### Q2: Difference between Public and Private Subnet in your project?

**Answer:**

* Public Subnet → Has route to Internet Gateway (IGW). Resources like ALB were deployed here.
* Private Subnet → No direct internet access. Used for backend EC2 instances and databases.

This ensured that only the load balancer was exposed to the internet, not the application servers.

---

### Q3: How did private EC2 instances access the internet?

**Answer:**
Private EC2 instances accessed the internet through a **NAT Gateway** placed in the public subnet.

Route flow:
Private Subnet → Route Table → NAT Gateway → Internet Gateway → Internet

This allowed outbound access for package updates without exposing instances publicly.

---

### Q4: NAT Gateway or NAT Instance? Why?

**Answer:**
We used **NAT Gateway** because:

* It is fully managed
* Highly available within AZ
* No patching or maintenance required
* Better performance

Since this was production infrastructure targeting 99.9% availability, managed service was preferred.

---

### Q5: How did you secure your VPC?

**Answer:**
I implemented multiple security layers:

* Security Groups (stateful firewall)
* Network ACLs (stateless layer control)
* Private subnets for backend
* No direct SSH from internet (used bastion host)
* IAM role-based access
* VPC Flow Logs enabled for monitoring

Defense-in-depth strategy was followed.

---

### Q6: What CIDR range did you choose and why?

**Answer:**
We used **10.0.0.0/16**.

Reason:

* Provides 65,536 IP addresses
* Allowed subnet segmentation
* Supports future scaling

Subnets were divided into /24 ranges per tier.

---

# 🔹 IAM Cross-Questions

---

### Q7: Difference between IAM Role and IAM User?

**Answer:**
IAM User → Permanent identity with credentials.
IAM Role → Temporary credentials assumed by AWS services or users.

In our project, EC2 and EKS used IAM Roles to securely access S3 and CloudWatch.

---

### Q8: How did EKS access other AWS services?

**Answer:**
We used **IAM Roles for Service Accounts (IRSA)**.

This allowed Kubernetes pods to assume IAM roles securely without storing credentials inside containers.

---

### Q9: What is least privilege? Example from project.

**Answer:**
Least privilege means giving only minimum required permissions.

Example:
If application needed only S3 read access, we attached policy with:

* s3:GetObject
* s3:ListBucket

We avoided full S3 access (s3:*).

---

### Q10: How would you debug AccessDenied error?

**Answer:**
Steps:

1. Check IAM policy attached to role/user
2. Check resource policy (e.g., S3 bucket policy)
3. Verify trust relationship (for roles)
4. Check CloudTrail logs
5. Use IAM Policy Simulator

---

# 🔹 ALB + Auto Scaling Cross-Questions

---

### Q11: What metric did you use for scaling?

**Answer:**
We used:

* CPU Utilization (Target 60%)
* Request Count per Target (for ALB-based scaling)

Target tracking scaling was configured.

---

### Q12: Difference between Target Tracking and Step Scaling?

**Answer:**
Target Tracking → Maintains a fixed metric value automatically.
Step Scaling → Scales based on predefined threshold steps.

Target tracking is simpler and recommended for most workloads.

---

### Q13: What happens if one AZ fails?

**Answer:**
Since we deployed ALB and Auto Scaling across 2 AZs:

* ALB routes traffic only to healthy targets
* Auto Scaling launches new instances in healthy AZ
* Application continues running

This ensures high availability.

---

### Q14: How does ALB health check work internally?

**Answer:**
ALB periodically sends HTTP/HTTPS requests to a configured health check path (e.g., /health).

If:

* Response code = 200 → Healthy
* Timeout or 5xx → Unhealthy

If instance fails consecutive checks, ALB stops routing traffic.

---

### Q15: Difference between ALB and NLB?

**Answer:**
ALB:

* Layer 7 (HTTP/HTTPS)
* Path-based routing
* Host-based routing

NLB:

* Layer 4 (TCP/UDP)
* High performance
* Static IP support

We used ALB because application required path-based routing.

---

# 🔥 End of AWS Cross-Questions Section

---

# 🔥 2. Terraform Cross-Questions

---

## Q1: What is Terraform state file?

**Answer:**
Terraform state file (terraform.tfstate) is a JSON file that stores the mapping between Terraform configuration and real-world infrastructure resources.

It keeps:

* Resource IDs
* Metadata
* Dependency information

Terraform uses this file to determine what needs to be created, updated, or destroyed.

---

## Q2: Where did you store your state file?

**Answer:**
We stored our state file in a **remote S3 backend**.

Configuration:

* S3 bucket for state storage
* DynamoDB table for state locking
* Versioning enabled on S3

This ensured collaboration, backup, and safety.

---

## Q3: What is state locking?

**Answer:**
State locking prevents multiple users from modifying the same infrastructure at the same time.

We used **DynamoDB** for locking.
When one engineer runs terraform apply, Terraform acquires a lock entry in DynamoDB.

Other users must wait until the lock is released.

---

## Q4: What happens if state file is deleted?

**Answer:**
If state file is deleted:

* Terraform loses track of existing infrastructure
* It may attempt to recreate resources
* This can cause duplication or conflicts

If S3 versioning is enabled, we can restore previous version.

---

## Q5: Difference between terraform plan and terraform apply?

**Answer:**
terraform plan:

* Shows execution plan
* Displays what will change
* No infrastructure modification

terraform apply:

* Executes changes
* Creates/updates/destroys resources

Best practice: Always review plan before apply.

---

## Q6: What are modules? Did you create custom modules?

**Answer:**
Modules are reusable Terraform components.

Yes, I created custom modules for:

* VPC
* EC2
* EKS
* ALB

This helped reduce duplication and standardize infrastructure across environments.

---

## Q7: How did you manage dev/test/prod environments?

**Answer:**
We used:

* Separate backend state files per environment
* Separate variable files (dev.tfvars, prod.tfvars)
* Workspace-based separation in some cases

Each environment had its own isolated infrastructure.

---

## Q8: How do you pass variables securely?

**Answer:**
For sensitive variables:

* Used variables with sensitive = true
* Passed secrets via environment variables
* Integrated AWS Secrets Manager where required

We avoided hardcoding credentials.

---

## Q9: What is Terraform backend?

**Answer:**
Terraform backend defines where the state file is stored.

Types:

* Local backend (default)
* Remote backend (S3, etc.)

We used S3 remote backend with DynamoDB locking.

---

# ⚠️ Trap Question

## Q10: What happens if two engineers run terraform apply at the same time?

**Answer:**
If remote backend with state locking is configured:

* First engineer acquires lock
* Second engineer gets lock error
* Must wait until lock is released

If no locking is configured:

* State corruption can occur
* Infrastructure drift may happen
* Resources may be duplicated or destroyed unintentionally

That is why state locking is critical in team environments.

---

# 🔥 End of Terraform Cross-Questions Section

---

# 🔥 3. CI/CD Cross-Questions

---

## Q1: What triggers your pipeline?

**Answer:**
Our Jenkins pipeline was triggered automatically using **GitHub Webhooks**.

Whenever:

* Code was pushed to dev branch
* Pull request was merged

GitHub sent a webhook event to Jenkins, which triggered the pipeline.

We also had manual trigger support for hotfix deployments.

---

## Q2: Explain your Jenkinsfile.

**Answer:**
We used a **Declarative Jenkinsfile** structured into multiple stages:

Stages:

1. Checkout → Pull code from GitHub
2. Build → Maven / npm build
3. Unit Test → Run test cases
4. SonarQube Scan → Code quality check
5. Docker Build → Create Docker image
6. Push to ECR → Store image
7. Deploy → Update Kubernetes deployment

We used post actions for success/failure notifications.

Pipeline was version-controlled inside the repository.

---

## Q3: Difference between Declarative and Scripted pipeline?

**Answer:**
Declarative:

* Structured syntax
* Easier to read
* Recommended for most projects
* Uses pipeline { } block

Scripted:

* Groovy-based
* More flexible
* Complex logic handling

We used Declarative pipeline for better readability and maintainability.

---

## Q4: How do you handle secrets in Jenkins?

**Answer:**
We used:

* Jenkins Credentials Manager
* Stored secrets as credentials (AWS keys, Docker registry)
* Accessed using credentialsId

Sensitive variables were never hardcoded in Jenkinsfile.

For Kubernetes, we also used Kubernetes Secrets.

---

## Q5: What happens if build fails?

**Answer:**
If build fails:

* Pipeline stops automatically
* Deployment stage does not execute
* Developer receives notification (Slack/Email)

This prevents broken code from reaching production.

---

## Q6: How do you rollback deployment?

**Answer:**
For Kubernetes deployments:

Command:
kubectl rollout undo deployment <deployment-name>

Since images were version-tagged in ECR, we could redeploy previous stable version.

We also maintained image version history.

---

## Q7: How did you reduce deployment time from 45 to 10 minutes?

**Answer:**
Optimizations included:

* Docker layer caching
* Parallel test execution
* Reduced manual approval steps
* Optimized build agents
* Incremental builds

We also removed unnecessary stages in lower environments.

---

## Q8: Did you parallelize stages?

**Answer:**
Yes.

We parallelized:

* Unit testing
* Security scanning

This significantly reduced pipeline execution time.

---

# 🔥 End of CI/CD Cross-Questions Section

---

# 🔥 4. Docker Cross-Questions

---

## Q1: What is the difference between Docker image and container?

**Answer:**
Docker Image:

* Read-only template
* Contains application code, dependencies, and runtime
* Blueprint for container

Docker Container:

* Running instance of an image
* Has writable layer
* Executes application process

In simple terms, image is like a class, container is like an object.

---

## Q2: What is Dockerfile multi-stage build?

**Answer:**
Multi-stage build allows using multiple FROM statements in a single Dockerfile.

Example flow:

1. First stage → Build application (Maven/npm)
2. Second stage → Copy only compiled artifacts into lightweight runtime image

This reduces final image size and removes unnecessary build tools.

---

## Q3: How do you reduce Docker image size?

**Answer:**
Techniques we used:

* Multi-stage builds
* Using minimal base images (Alpine)
* Removing unnecessary packages
* Combining RUN commands
* Cleaning package cache

This improved startup time and reduced ECR storage cost.

---

## Q4: Difference between CMD and ENTRYPOINT?

**Answer:**
CMD:

* Provides default command
* Can be overridden at runtime

ENTRYPOINT:

* Defines main executable
* Hard to override

Best practice:
Use ENTRYPOINT for main application and CMD for default arguments.

---

## Q5: Where did you store images?

**Answer:**
We stored Docker images in **Amazon ECR (Elastic Container Registry)**.

Flow:
Docker Build → Tag Image → Push to ECR → Kubernetes pulls image from ECR.

Images were version-tagged to support rollback strategy.

---

## Q6: What happens if container crashes?

**Answer:**
If running in Kubernetes:

* Kubelet detects container failure
* Restarts container automatically
* Based on restartPolicy

If crash persists:

* Pod may enter CrashLoopBackOff
* We check logs using kubectl logs

In standalone Docker:

* Depends on restart policy (unless-stopped, always)

Kubernetes ensures self-healing behavior.

---

# 🔥 End of Docker Cross-Questions Section

---

# 🔥 5. Kubernetes / EKS Deep Cross-Questions

---

## Q1: What is the difference between Pod, Deployment, and ReplicaSet?

**Answer:**
Pod:

* Smallest deployable unit in Kubernetes
* Contains one or more containers
* Shares network and storage

ReplicaSet:

* Ensures specified number of pod replicas are running
* Maintains desired state

Deployment:

* Manages ReplicaSets
* Provides rolling updates and rollback capability

In production, we mostly interacted with Deployments, not ReplicaSets directly.

---

## Q2: What is ClusterIP vs NodePort vs LoadBalancer?

**Answer:**
ClusterIP:

* Default service type
* Accessible only inside cluster

NodePort:

* Exposes service on a port of each node
* Accessible externally via NodeIP:Port

LoadBalancer:

* Provisions external load balancer (in AWS → ALB/NLB)
* Public access

We used LoadBalancer for public APIs and ClusterIP for internal communication.

---

## Q3: How does service discovery work?

**Answer:**
Kubernetes assigns each service a DNS name:

service-name.namespace.svc.cluster.local

Pods communicate using this DNS name instead of IP address.

This allows dynamic pod replacement without breaking communication.

---

## Q4: How does DNS work inside cluster?

**Answer:**
CoreDNS runs as a pod inside cluster.

Flow:

1. Pod requests service DNS
2. Query goes to CoreDNS
3. CoreDNS checks service registry
4. Returns ClusterIP

This enables internal service resolution.

---

## Q5: What is kube-proxy?

**Answer:**
kube-proxy runs on each node.

It:

* Maintains iptables/ipvs rules
* Routes traffic from service to backend pods
* Enables load balancing across pod replicas

It ensures traffic reaches correct pod.

---

## Q6: What happens when you run kubectl apply?

**Answer:**

1. kubectl sends request to API server
2. API server validates manifest
3. Desired state stored in etcd
4. Scheduler assigns pod to node
5. Kubelet pulls image and starts container
6. kube-proxy updates networking rules

Cluster moves toward desired state.

---

## Q7: How does rolling update work internally?

**Answer:**
Deployment creates new ReplicaSet with updated version.

Based on:

* maxSurge
* maxUnavailable

Kubernetes:

* Gradually creates new pods
* Terminates old pods step-by-step

Ensures zero downtime.

---

## Q8: What is ConfigMap vs Secret?

**Answer:**
ConfigMap:

* Stores non-sensitive configuration
* Plain text

Secret:

* Stores sensitive data
* Base64 encoded

We used Secrets for DB passwords and ConfigMaps for environment variables.

---

## Q9: How did you manage namespace isolation?

**Answer:**
We created separate namespaces for:

* dev
* test
* prod

Applied:

* Resource quotas
* Network policies
* RBAC rules

This prevented cross-environment interference.

---

## Q10: What happens if one worker node goes down?

**Answer:**

* Node marked NotReady
* Pods on that node become unavailable
* ReplicaSet creates new pods on healthy nodes
* Traffic routed only to healthy pods

Kubernetes ensures self-healing.

---

# ⚠️ Trap Question

## Q11: How does Kubernetes decide which node to schedule a pod on?

**Answer:**
The Kubernetes Scheduler evaluates multiple factors:

1. Resource availability (CPU, Memory)
2. Node selectors / labels
3. Taints and tolerations
4. Affinity / anti-affinity rules
5. Pod topology spread constraints
6. Resource requests defined in pod spec

Scheduler scores nodes and selects the best suitable node.

If no node satisfies requirements → Pod remains Pending.

---

# 🔥 End of Kubernetes / EKS Section

---

# 🔥 6. Monitoring Cross-Questions

---

## Q1: What metrics did you monitor?

**Answer:**
We monitored infrastructure, application, and Kubernetes-level metrics.

Infrastructure (CloudWatch):

* CPU Utilization
* Memory usage
* Disk I/O
* Network In/Out
* EC2 status checks

Kubernetes (Prometheus):

* Pod CPU/Memory usage
* Pod restarts
* Node resource usage
* API server latency

Application:

* HTTP request rate
* Error rate (5xx)
* Response time

We created Grafana dashboards for real-time visibility.

---

## Q2: What is the difference between push and pull model?

**Answer:**
Push Model:

* Applications push metrics to monitoring system
* Example: CloudWatch agent pushing metrics

Pull Model:

* Monitoring system scrapes metrics from targets
* Example: Prometheus scraping /metrics endpoint

We used Prometheus pull model in Kubernetes.

---

## Q3: How does Prometheus collect metrics?

**Answer:**

1. Applications expose metrics via /metrics endpoint.
2. Prometheus server scrapes metrics at defined intervals.
3. Metrics stored in time-series database.
4. Grafana queries Prometheus for visualization.

Prometheus uses service discovery to detect Kubernetes targets dynamically.

---

## Q4: What is Alertmanager?

**Answer:**
Alertmanager works with Prometheus.

It:

* Receives alerts from Prometheus
* Groups alerts
* Deduplicates alerts
* Sends notifications (Slack, Email, PagerDuty)

This helps reduce alert noise.

---

## Q5: How did you reduce MTTR?

**Answer:**
We reduced Mean Time To Resolution by:

* Setting threshold-based alerts
* Creating clear Grafana dashboards
* Defining runbooks for common incidents
* Using centralized logging
* Automated pod restarts in Kubernetes

This allowed faster root cause identification.

---

## Q6: What is the difference between logs and metrics?

**Answer:**
Metrics:

* Numerical data
* Used for monitoring trends
* Example: CPU usage 70%

Logs:

* Detailed event records
* Used for debugging
* Example: Application error stack trace

Metrics tell "what is happening".
Logs tell "why it happened".

---

# 🔥 End of Monitoring Section

---

# 🔥 7. Security Cross-Questions

---

## Q1: Difference between Security Group and NACL?

**Answer:**
Security Group:

* Attached to EC2 instances
* Stateful
* Operates at instance level
* Only allow rules (no explicit deny)

NACL (Network ACL):

* Attached to subnets
* Stateless
* Operates at subnet level
* Supports both allow and deny rules

In our project:

* Security Groups controlled application-level access
* NACLs added additional subnet-level protection

---

## Q2: What is stateless vs stateful?

**Answer:**
Stateful:

* Automatically allows return traffic
* No need to define outbound rule separately
* Example: Security Groups

Stateless:

* Inbound and outbound rules must be defined explicitly
* Example: NACL

Stateful simplifies management. Stateless provides strict control.

---

## Q3: How would you secure an EKS cluster?

**Answer:**
We applied multiple security layers:

1. Private EKS cluster (no public API exposure when possible)
2. IAM Roles for Service Accounts (IRSA)
3. RBAC for role-based access control
4. Network Policies to restrict pod-to-pod communication
5. Secrets stored securely (not in plain YAML)
6. Enabled control plane logging
7. Image scanning before deployment

Security was implemented using defense-in-depth strategy.

---

## Q4: How do you prevent hardcoding secrets?

**Answer:**
We avoided storing secrets in:

* Git repositories
* Dockerfiles
* Jenkinsfiles

Instead we used:

* Jenkins Credentials Manager
* Kubernetes Secrets
* AWS Secrets Manager
* Environment variables injected at runtime

Access was controlled using IAM roles.

---

## Q5: What is IAM policy structure?

**Answer:**
IAM policy is written in JSON format.

Main components:

* Version
* Statement

  * Effect (Allow/Deny)
  * Action
  * Resource
  * Condition (optional)

Example structure:

{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::my-bucket/*"
}
]
}

Policies define what actions are allowed or denied on specific AWS resources.

---

# 🔥 End of Security / DevSecOps Section

---

# 🔥 8. Linux & Bash Cross-Questions

---

## Q1: How do you check memory usage?

**Answer:**
Common commands I used:

* free -h → Shows total, used, and available memory in human-readable format
* top → Real-time memory and CPU usage
* htop → Interactive monitoring (if installed)
* vmstat → Memory statistics

In production debugging, I usually start with `free -h` and `top`.

---

## Q2: How do you find top 5 CPU consuming processes?

**Answer:**
Command:

ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -n 6

Explanation:

* --sort=-%cpu → Sort by highest CPU usage
* head -n 6 → Show top 5 processes (+ header)

Alternatively:

* top (then press Shift + P to sort by CPU)

---

## Q3: Difference between soft link and hard link?

**Answer:**
Hard Link:

* Direct reference to inode
* Cannot link directories
* Works only within same filesystem
* If original file deleted, hard link still works

Soft Link (Symbolic Link):

* Points to file path
* Can link directories
* Can cross filesystems
* Breaks if original file is deleted

Command:

* ln file1 hardlink
* ln -s file1 softlink

---

## Q4: What is crontab?

**Answer:**
Crontab is a Linux scheduler used to run jobs automatically at specified intervals.

Syntax format:

* * * * * command

Fields:

* Minute
* Hour
* Day of month
* Month
* Day of week

Example:
0 2 * * * /home/script.sh

Runs script daily at 2 AM.

---

## Q5: Write a bash script to check disk usage.

**Answer:**

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "Disk usage is above ${THRESHOLD}%"
else
    echo "Disk usage is under control"
fi
```

Explanation:

* df -h → Shows disk usage
* Extract percentage
* Compare with threshold
* Print alert message

This type of script can be scheduled via cron for automated monitoring.

---

# 🔥 End of Linux & Bash Section

---

# 🔥 9. Availability & Architecture Cross-Questions

---

## Q1: What does 99.9% availability mean in downtime?

**Answer:**
99.9% availability means the system can only be down for **0.1% of total time**.

Approximate downtime allowed:

* Per year → ~8.76 hours
* Per month → ~43 minutes
* Per day → ~1.44 minutes

So maintaining 99.9% means infrastructure failures must be handled quickly with automated recovery mechanisms.

---

## Q2: How did you design high availability?

**Answer:**
We implemented HA at multiple layers:

1. Multi-AZ Deployment

   * EC2 / EKS worker nodes across 2 Availability Zones

2. Load Balancer

   * ALB distributed traffic across healthy targets

3. Auto Scaling

   * Automatically replaced unhealthy instances

4. Kubernetes Self-Healing

   * ReplicaSets recreated failed pods

5. Health Checks

   * ALB and Kubernetes liveness/readiness probes

6. Infrastructure as Code

   * Terraform allowed fast recovery and reproducibility

This combination ensured minimal downtime.

---

## Q3: What is RTO and RPO?

**Answer:**
RTO (Recovery Time Objective):

* Maximum acceptable time to restore service after failure.

RPO (Recovery Point Objective):

* Maximum acceptable data loss measured in time.

Example:

* If RTO = 30 minutes → System must recover within 30 minutes.
* If RPO = 5 minutes → Only 5 minutes of data loss acceptable.

These metrics are critical for disaster recovery planning.

---

## Q4: How would you redesign for 99.99% availability?

**Answer:**
To move from 99.9% to 99.99% (~52 minutes downtime per year), I would:

1. Deploy across 3 Availability Zones
2. Use managed services wherever possible (RDS Multi-AZ, EKS managed node groups)
3. Implement multi-region failover
4. Use Route 53 health checks with failover routing
5. Enable cross-region backups
6. Add chaos testing to validate resilience

This would reduce single points of failure.

---

## Q5: How would you reduce AWS cost further?

**Answer:**
Cost optimization strategies:

1. Use Reserved Instances or Savings Plans for steady workloads
2. Use Spot Instances for non-critical workloads
3. Right-size EC2 instances based on monitoring metrics
4. Enable Auto Scaling to avoid over-provisioning
5. Use S3 lifecycle policies for storage optimization
6. Remove unused EBS volumes and snapshots
7. Use Graviton-based instances for better price-performance

We already reduced cost by 25% using Auto Scaling and monitoring-based optimization.

---

# 🔥 End of Availability & Architecture Section

---

# 🔥 10. Behavioral Trap Cross-Questions

---

## Q1: What exactly was YOUR responsibility?

**Answer (Interview Style – Clear & Honest):**

Although I worked under a Senior DevOps Engineer, I was not just observing. I had clear ownership areas.

My primary responsibilities were:

* Writing and maintaining Terraform modules
* Managing Jenkins pipelines
* Dockerizing microservices
* Deploying workloads on EKS
* Configuring monitoring dashboards
* Troubleshooting deployment failures

The senior engineer mainly handled architectural decisions and final reviews, but day-to-day execution, testing, and optimization tasks were handled by me.

So I would say I was responsible for implementation and automation, while he focused on design validation and governance.

---

## Q2: What mistake did you make?

**Answer:**

One mistake I made early in the project was modifying a Terraform variable without running `terraform plan` carefully.

It unintentionally triggered a change in a security group rule that temporarily blocked application traffic.

The issue was resolved quickly by:

* Rolling back to the previous state
* Reviewing changes properly

After that incident, I strictly followed:

* Mandatory plan review
* Peer review before apply
* Change approval process

It taught me the importance of infrastructure change validation.

---

## Q3: What was your biggest production issue?

**Answer:**

One major issue we faced was sudden high CPU usage in EKS pods during peak traffic.

Impact:

* Increased response time
* Some pods restarted

My approach:

1. Checked Prometheus metrics
2. Verified HPA configuration
3. Found resource requests were too low
4. Updated deployment with proper CPU limits
5. Adjusted Auto Scaling threshold

After optimization, performance stabilized.

This improved my real-world troubleshooting confidence.

---

## Q4: If I call your senior, what will he say about you?

**Answer:**

I believe he would say that:

* I am proactive and eager to learn
* I take ownership of assigned tasks
* I ask questions when unsure instead of assuming
* I document processes properly
* I focus on automation instead of manual fixes

He might also say that I improved significantly during the internship in terms of confidence and production troubleshooting.

---

## Q5: Why should we trust your 40% improvement number?

**Answer:**

That 40% improvement was measured based on provisioning time before and after Terraform implementation.

Earlier:

* Manual provisioning took approximately 45–50 minutes

After Terraform automation:

* Infrastructure setup completed in around 25–30 minutes

We measured pipeline logs and execution timestamps.

The improvement was not an assumption; it was based on actual deployment duration comparison.

I understand numbers should be measurable and realistic, so I only mention improvements that were tracked.

---

# 🔥 End of Behavioral / Ownership Section

---