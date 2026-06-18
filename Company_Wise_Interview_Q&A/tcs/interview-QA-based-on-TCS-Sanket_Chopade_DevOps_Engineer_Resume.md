# DevOps Interview Question Bank — Answers
**Candidate:** Sanket Ajay Chopade
**Role Target:** DevOps Engineer (0–1 Year Experience)
**Stack:** AWS · Kubernetes · Terraform · Jenkins · GitHub Actions · Argo CD · Docker · Prometheus · Grafana

---

## Table of Contents

1. [Resume Introduction Questions](#1-resume-introduction-questions)
2. [Internship-Based Questions (Hisan Labs)](#2-internship-based-questions-hisan-labs)
3. [AWS Interview Questions](#3-aws-interview-questions)
4. [Terraform Questions](#4-terraform-questions)
5. [Docker Questions](#5-docker-questions)
6. [Kubernetes Questions](#6-kubernetes-questions)
7. [GitHub Actions & GitOps Questions](#7-github-actions--gitops-questions)
8. [Monitoring & Observability Questions](#8-monitoring--observability-questions)
9. [Linux Questions](#9-linux-questions)
10. [Bash & Python Questions](#10-bash--python-questions)
11. [Security Questions](#11-security-questions)
12. [Project-Specific Questions](#12-project-specific-questions)
13. [AWS Generative AI Questions](#13-aws-generative-ai-questions)
14. [Behavioral Questions](#14-behavioral-questions)
15. [Situational Questions](#15-situational-questions)
16. [HR Questions](#16-hr-questions)

---

## 1. Resume Introduction Questions

---

**Q: Tell me about yourself.**

I'm a DevOps Engineer with hands-on experience gained during my internship at Hisan Labs, where I worked on AWS cloud infrastructure, Kubernetes-based deployments, CI/CD pipelines, Terraform, and monitoring solutions. I graduated with a B.Tech from Rashtrasant Tukadoji Maharaj Nagpur University and have built two end-to-end projects — a GitOps delivery pipeline and a serverless game hosting platform on EKS with Fargate. I'm looking to grow in a team where I can contribute to infrastructure automation and reliability.

---

**Q: Walk me through your resume.**

- **Internship (Hisan Labs, Sep 2025 – May 2026):** Worked as a DevOps Engineer Intern in a 4-member team managing a microservices-based e-commerce application on AWS. Owned 3 Jenkins pipelines, Terraform modules, Kubernetes workload management on EKS, and monitoring with Prometheus, Grafana, Datadog, and CloudWatch.
- **Projects:**
  - *Go Cloud-Native GitOps Pipeline:* GitHub Actions + Argo CD + Helm on EKS, using Distroless images.
  - *Serverless Game Hosting Platform:* EKS + AWS Fargate + IRSA + AWS Load Balancer Controller.
- **Skills:** AWS, Terraform, Docker, Kubernetes, Jenkins, Bash, Python, Linux.

---

**Q: Why did you choose DevOps as a career?**

- DevOps sits at the intersection of development and operations — it's not just one skill, it's a broad discipline covering cloud, automation, containers, networking, and security.
- I was drawn to the impact DevOps has: shipping code faster, reliably, with visibility into what's happening in production.
- During my academic projects, I found infrastructure and automation more engaging than pure application development, which led me to pursue it professionally.

---

**Q: What did you learn during your internship at Hisan Labs?**

- How real-world CI/CD pipelines are structured across multiple environments (not just local or toy projects).
- Managing Terraform state collaboratively, including remote backends and state locking to prevent conflicts.
- Operating Kubernetes in a production-like environment — not just deploying pods, but managing Ingress, HPA, Secrets, and ConfigMaps for a microservices app.
- How monitoring tools like Prometheus, Grafana, Datadog, and CloudWatch work together for full observability.
- Jenkins administration: managing plugins, credentials, GitHub webhook integration, and pipeline failure handling.

---

**Q: What were your day-to-day responsibilities?**

- Maintaining and improving 3 Jenkins pipelines (Terraform provisioning, frontend deployments, backend deployments).
- Monitoring application and infrastructure health via Prometheus, Grafana, Datadog, and CloudWatch.
- Reviewing and updating Terraform modules for AWS resource provisioning.
- Managing Kubernetes deployments on EKS — scaling, updates, and troubleshooting.
- Automating operational tasks using Bash and Python scripts.
- Managing AWS IAM roles and policies following least privilege principles.
- Updating and maintaining operational documentation.

---

**Q: What DevOps tools are you most comfortable with?**

- **Strongest:** Jenkins, Terraform, Kubernetes (kubectl), Docker, AWS (EKS, IAM, VPC, S3, RDS).
- **Comfortable:** GitHub Actions, Argo CD, Helm, Prometheus, Grafana, Bash scripting.
- **Exposure:** Datadog, CloudWatch, SonarQube, Trivy, Python automation.

---

**Q: Which project are you most proud of and why?**

The *Go Cloud-Native GitOps Delivery Pipeline* — because it was fully end-to-end and I owned the design. It combined multi-stage Dockerfiles with Distroless images to minimize attack surface, GitHub Actions for CI (build, test, code quality), and Argo CD for continuous delivery on EKS. The GitOps model means the cluster state is always reconciled from Git, which is a production-grade practice. It was the clearest demonstration of all the tools I use working together.

---

**Q: What challenges did you face during your internship?**

- **Terraform state conflicts:** When multiple team members provisioned infrastructure simultaneously, state locking prevented corruption but required us to coordinate workflow and use remote backends properly.
- **Kubernetes troubleshooting:** Diagnosing pods failing due to missing Secrets or ConfigMap misconfigurations was non-obvious at first.
- **Jenkins pipeline failures on partial deploys:** Handling rollbacks when only some stages failed required careful pipeline design with `post { failure { } }` blocks.
- **Understanding IAM trust policies:** Getting EKS node roles and IRSA permissions right required several iterations.

---

**Q: Why should we hire you for a DevOps role?**

- I have practical, not just theoretical, experience — I've worked on a real production-grade microservices application during my internship.
- I've built two end-to-end projects beyond my internship scope, showing initiative.
- I'm comfortable with the full DevOps lifecycle: infrastructure provisioning (Terraform), containerization (Docker), orchestration (Kubernetes/EKS), CI/CD (Jenkins, GitHub Actions, Argo CD), and observability (Prometheus, Grafana, Datadog).
- I adapt quickly — I picked up IRSA and OIDC integration for the Serverless project independently.

---

**Q: Where do you see yourself in the next 3–5 years?**

- Short-term: Deepen expertise in Kubernetes, cloud-native security, and SRE practices.
- Medium-term: Take on ownership of critical infrastructure and mentoring responsibilities in a team.
- Long-term: Move toward a Site Reliability Engineering or Platform Engineering role, designing self-service infrastructure platforms.

---

## 2. Internship-Based Questions (Hisan Labs)

### General

---

**Q: Explain the architecture of the e-commerce application you worked on.**

The application was a **microservices-based e-commerce platform** deployed on AWS:
- **Frontend** and **backend** were separate deployable units, each with their own Jenkins pipeline.
- All workloads ran on **Amazon EKS**, with services communicating internally via Kubernetes Services.
- **External traffic** was routed through an **Ingress** backed by an AWS ALB.
- **Databases** were hosted on **Amazon RDS** (outside the cluster, accessed via Service endpoints).
- **Static assets and artifacts** were stored in **S3**.
- Infrastructure was provisioned via **Terraform** with a remote state backend.
- Monitoring was handled by **Prometheus + Grafana** for in-cluster metrics and **Datadog + CloudWatch** for infrastructure and AWS-level metrics.

---

**Q: What was your exact role in the DevOps team?**

Part of a 4-member DevOps team. My responsibilities included:
- Designing and maintaining Jenkins pipelines.
- Developing and modifying Terraform modules.
- Managing Kubernetes workloads on EKS.
- AWS IAM management.
- Monitoring setup and maintenance.
- Bash/Python scripting for operational automation.
- Operational documentation.

---

**Q: How many environments did you manage?**

We managed multiple environments (typically dev, staging, and production). Terraform workspaces or separate state files were used per environment to maintain isolation and consistency.

---

**Q: How was the application deployed?**

1. Developer pushes code to GitHub → webhook triggers Jenkins pipeline.
2. Pipeline runs: code quality check (SonarQube) → build → Docker image creation → push to registry.
3. Kubernetes manifests updated with new image tag.
4. EKS cluster pulls and deploys the updated image via Kubernetes Deployments.
5. Argo CD (in the GitOps project) handled continuous sync from Git to cluster.

---

**Q: What responsibilities did you own independently?**

- Terraform module development for VPCs, Security Groups, S3, RDS, and EKS clusters.
- Jenkins pipeline design for frontend and backend deployments.
- Bash and Python automation scripts for operational tasks.
- Kubernetes workload configurations (ConfigMaps, Secrets, HPA).
- IAM role and policy management.

---

### CI/CD

---

**Q: Explain the Jenkins pipelines you created.**

Three pipelines:
1. **Terraform Infrastructure Pipeline:** Provisions AWS resources (VPC, EKS, RDS, S3). Stages: `init → validate → plan → apply`.
2. **Frontend Deployment Pipeline:** Builds the frontend Docker image, pushes to registry, deploys to EKS.
3. **Backend Deployment Pipeline:** Includes SonarQube code quality gate, builds backend Docker image, pushes, deploys to EKS.

---

**Q: What stages existed in your pipelines?**

Typical stages (Backend pipeline example):
```
Checkout → SonarQube Analysis → Quality Gate → Build → Docker Build → Docker Push → Deploy to EKS → Post-deploy Health Check
```

---

**Q: How did you integrate SonarQube?**

- SonarQube server was configured in Jenkins under **Global Tool Configuration**.
- Used the `withSonarQubeEnv()` wrapper in the Jenkinsfile to inject SonarQube environment variables.
- Added a **Quality Gate** stage using `waitForQualityGate()` — pipeline fails if quality gate is not passed.
- SonarQube token stored as a **Jenkins credential** (secret text).

---

**Q: What happens when a developer pushes code?**

1. GitHub webhook triggers the relevant Jenkins pipeline.
2. Jenkins checks out the latest code.
3. SonarQube analysis runs (for backend pipelines).
4. If quality gate passes, Docker image is built and pushed.
5. Kubernetes deployment is updated with the new image.
6. Deployment health is verified post-deploy.

---

**Q: How did you handle deployment failures?**

- Used `post { failure { } }` blocks in Jenkinsfile to send notifications and trigger rollback steps.
- For Kubernetes, `kubectl rollout undo deployment/<name>` was used to revert to the previous ReplicaSet.
- Terraform pipelines used plan review steps before apply to catch errors early.

---

**Q: How did GitHub webhooks trigger Jenkins?**

- In GitHub repository settings, a webhook was configured pointing to `http://<jenkins-url>/github-webhook/`.
- The **GitHub plugin** in Jenkins was configured to listen for push events.
- Each pipeline was set to trigger on `GitHub hook trigger for GITScm polling`.

---

**Q: How did you store secrets in Jenkins?**

- Used **Jenkins Credentials Store** (Manage Jenkins → Credentials).
- Secret types used: Username+Password (Docker registry), Secret Text (SonarQube token, AWS credentials), SSH keys.
- Credentials accessed in Jenkinsfile via `withCredentials([])` block — never hardcoded.

---

**Q: How did you manage Jenkins credentials?**

- Stored all credentials in **Jenkins Credentials Store**, scoped to System or specific folders.
- AWS credentials were managed using IAM roles assigned to the Jenkins EC2 instance where possible, to avoid storing long-lived keys.
- Rotated credentials periodically and revoked unused ones.

---

**Q: How would you improve your existing pipeline?**

- Add **parallel stages** for faster execution (e.g., run unit tests and SonarQube scan concurrently).
- Integrate **Trivy** for Docker image vulnerability scanning before push.
- Add **smoke tests** post-deployment to validate application health.
- Implement **pipeline as a shared library** to avoid duplication across similar pipelines.
- Use **Argo CD** for the deployment stage (GitOps model) instead of direct kubectl apply.

---

### Infrastructure

---

**Q: What AWS resources did you provision using Terraform?**

- **VPCs** with public and private subnets across multiple AZs.
- **Security Groups** for EKS nodes, RDS, and application layers.
- **S3 Buckets** for static assets and Terraform remote state.
- **RDS Instances** for the application database.
- **Amazon EKS Clusters** with node groups.
- **IAM Roles and Policies** for EKS nodes and service accounts.

---

**Q: Explain your Terraform workflow.**

```
Write/Modify .tf files → terraform init → terraform validate → terraform plan (review) → terraform apply → terraform state stored in remote backend (S3 + DynamoDB lock)
```

---

**Q: How did you manage Terraform state?**

- Used **S3 as a remote backend** to store `terraform.tfstate`.
- Used **DynamoDB** for state locking to prevent concurrent modifications.
- State file was never committed to Git.

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "envs/prod/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```

---

**Q: Why is remote state important?**

- Enables **team collaboration** — all members work against the same state.
- Prevents **state drift** caused by local state files getting out of sync.
- State locking prevents **race conditions** when multiple engineers run Terraform simultaneously.
- Remote state can be used as a **data source** by other Terraform configurations.

---

**Q: What backend did you use for Terraform state?**

**S3 backend** with a DynamoDB table for locking. S3 versioning was enabled on the bucket to allow recovery from accidental state corruption.

---

**Q: How did you handle multiple environments?**

- Used **separate state files per environment** via different S3 key paths (`envs/dev/`, `envs/staging/`, `envs/prod/`).
- Environment-specific variable files (`.tfvars`) were used to parameterize differences.
- Alternatively, **Terraform workspaces** can separate state within the same configuration.

---

### Kubernetes

---

**Q: How was the application deployed on EKS?**

- Kubernetes **Deployment** manifests defined the desired state (image, replicas, resource limits).
- **ConfigMaps** held non-sensitive configuration (environment variables, config files).
- **Secrets** held sensitive configuration (DB credentials, API keys).
- **Services** exposed deployments within the cluster.
- **Ingress** (with AWS ALB Ingress Controller) exposed the application externally.
- **HPA** automatically scaled pods based on CPU/memory metrics.

---

**Q: What Kubernetes resources did you use?**

| Resource | Purpose |
|---|---|
| Deployment | Manages stateless application pods |
| ReplicaSet | Maintains desired pod count (managed by Deployment) |
| Service (ClusterIP/NodePort) | Internal pod communication |
| Ingress | External HTTP/HTTPS routing |
| ConfigMap | Non-sensitive configuration data |
| Secret | Sensitive configuration (base64-encoded) |
| HPA | Horizontal auto-scaling based on metrics |
| Namespace | Logical isolation of resources |

---

**Q: Why did you use ConfigMaps?**

- To **decouple configuration from the container image** — changing a config value doesn't require rebuilding the image.
- Stored environment-specific values like API endpoints, feature flags, and non-sensitive config.
- Mounted as environment variables or volume files inside pods.

---

**Q: Why did you use Secrets?**

- To store **sensitive data** (DB passwords, API keys) separately from application code and images.
- Kubernetes Secrets are base64-encoded and can be mounted as env vars or volume files.
- Access to Secrets is controlled via RBAC.
- In production, Secrets are backed by AWS Secrets Manager or Vault for stronger encryption.

---

**Q: How did you expose applications externally?**

- Used a **Kubernetes Ingress** resource with the **AWS Load Balancer Controller** as the Ingress class.
- Ingress defined routing rules (path-based or host-based) to route external traffic to internal Services.
- ALB terminated TLS and forwarded traffic to pods.

---

**Q: How did Ingress work in your cluster?**

1. Ingress resource created with annotations for AWS ALB.
2. AWS Load Balancer Controller (running in the cluster) watches Ingress resources.
3. Controller provisions an Application Load Balancer in AWS.
4. ALB routes traffic based on host/path rules to target groups backed by pod IPs.

---

**Q: How did HPA scale workloads?**

- HPA (Horizontal Pod Autoscaler) monitors metrics (CPU, memory) via the **Metrics Server**.
- When average CPU utilization across pods exceeds the threshold, HPA increases the replica count.
- When load drops, HPA scales down to the minimum replica count.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

---

### Monitoring

---

**Q: What metrics did you monitor?**

- **Application:** Request rate, error rate, latency (RED methodology).
- **Kubernetes:** Pod CPU/memory usage, restart count, pod availability.
- **Infrastructure:** EC2/node CPU, memory, disk I/O, network throughput.
- **RDS:** DB connections, query latency, storage usage.
- **AWS:** ALB request count, 5xx error rates (via CloudWatch).

---

**Q: Why use Prometheus?**

- **Pull-based** scraping model — Prometheus scrapes metrics from targets on a schedule.
- Highly efficient **time-series database** purpose-built for metrics.
- Powerful **PromQL** query language for alerting and dashboards.
- Deep Kubernetes integration via **kube-state-metrics** and **node-exporter**.
- Open source with a large ecosystem of exporters.

---

**Q: Why use Grafana?**

- Grafana provides **rich, customizable dashboards** for visualizing Prometheus metrics.
- Supports multiple data sources in one interface (Prometheus, CloudWatch, Datadog).
- Pre-built community dashboards for Kubernetes (e.g., Grafana dashboard ID 315 for node metrics).
- Supports alerting and annotation of events on graphs.

---

**Q: What role did Datadog play?**

- Complemented Prometheus/Grafana with **APM (Application Performance Monitoring)** capabilities.
- Provided **infrastructure-level monitoring** with a unified agent.
- Offered **log aggregation** alongside metrics — easier correlation than separate tools.
- Useful for **cloud-wide visibility** across AWS services beyond what Prometheus covers natively.

---

**Q: How did CloudWatch complement monitoring?**

- CloudWatch is the **native AWS monitoring service** — automatically captures metrics from RDS, ALB, S3, and other managed services without any agent installation.
- Used for **AWS-specific alarms** (e.g., RDS CPU > 80%, ALB 5xx spike).
- CloudWatch Logs captured system-level logs from EC2 instances and EKS control plane logs.
- Integrated with SNS for alerting and with Jenkins for automated responses.

---

**Q: How would you investigate a performance issue?**

1. Check **Grafana dashboards** for anomalies in CPU, memory, latency, and error rate.
2. Check **Kubernetes events** with `kubectl get events -n <namespace>`.
3. Check **pod logs** with `kubectl logs <pod-name>`.
4. Check **HPA status** — is the app under-scaled?
5. Check **RDS metrics** in CloudWatch — is the database the bottleneck?
6. Use **PromQL** to drill into specific pod or node metrics.
7. Check **ALB access logs** for 5xx errors or latency spikes.
8. Check recent **deployments** — did a recent change correlate with the degradation?

---

## 3. AWS Interview Questions

### IAM

---

**Q: Difference between IAM User, Role, and Policy.**

| Concept | Description |
|---|---|
| **IAM User** | A permanent identity for a person or application. Has long-lived credentials (access keys or password). |
| **IAM Role** | A temporary identity assumed by a service, application, or user. No long-lived credentials. |
| **IAM Policy** | A JSON document defining permissions. Attached to Users, Roles, or Groups. |

---

**Q: What is least privilege access?**

Grant only the **minimum permissions** required to perform a task — nothing more.
- Example: An EKS node role should only have permissions to pull ECR images and write to CloudWatch Logs, not full S3 or EC2 access.
- Reduces the blast radius of compromised credentials.

---

**Q: How do IAM roles work?**

1. A role has a **trust policy** specifying who can assume it (e.g., EC2 service, another account, a Kubernetes service account).
2. An entity (EC2 instance, Lambda, user) **assumes the role** via `sts:AssumeRole`.
3. AWS STS returns **temporary credentials** (Access Key, Secret Key, Session Token) valid for a defined duration.
4. The entity uses those credentials to make API calls within the role's permissions.

---

**Q: Why use IAM roles instead of access keys?**

- Access keys are **long-lived** and can be leaked. Roles issue **temporary credentials** that expire automatically.
- Roles eliminate the need to embed credentials in code, config files, or environment variables.
- Easier to rotate — no credential rotation needed for roles.
- AWS services (EC2, EKS, Lambda) natively support assuming roles via instance metadata.

---

**Q: Explain a trust policy.**

A trust policy is attached to a role and defines **who can assume it**:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}
```

This allows any EC2 instance to assume the role. For IRSA, the principal is a Kubernetes service account via an OIDC provider.

---

### VPC

---

**Q: What is a VPC?**

A **Virtual Private Cloud** is a logically isolated network within AWS. You control IP ranges (CIDR), subnets, route tables, gateways, and security rules. All AWS resources are launched within a VPC.

---

**Q: Difference between public and private subnet.**

| | Public Subnet | Private Subnet |
|---|---|---|
| **Route** | Has a route to Internet Gateway | No direct internet route |
| **Use case** | Load balancers, bastion hosts | EKS nodes, RDS, application servers |
| **Internet access** | Direct (inbound + outbound) | Outbound only via NAT Gateway |

---

**Q: What is a route table?**

A route table contains rules (routes) that determine where network traffic is directed. Every subnet is associated with a route table. A public subnet's route table has a route `0.0.0.0/0 → Internet Gateway`.

---

**Q: What is an Internet Gateway?**

A horizontally scaled, redundant AWS component that allows resources in a **public subnet** to communicate with the internet (both inbound and outbound). Attached to the VPC, referenced in route tables.

---

**Q: What is a NAT Gateway?**

A managed service that allows resources in **private subnets** to initiate outbound internet connections (e.g., to download packages), while blocking inbound connections from the internet. Placed in a public subnet. Charges apply per hour and per GB of data processed.

---

**Q: Explain security groups.**

Security groups are **stateful, instance-level firewalls** in AWS:
- Rules specify allowed inbound and outbound traffic (protocol, port, source/destination).
- **Stateful** — if inbound traffic is allowed, the return traffic is automatically allowed.
- Applied to EC2 instances, RDS, EKS nodes, ALBs, etc.

---

**Q: Difference between Security Groups and NACLs.**

| Feature | Security Group | NACL |
|---|---|---|
| Level | Instance/resource level | Subnet level |
| Statefulness | Stateful | Stateless |
| Rules | Allow only | Allow and Deny |
| Evaluation | All rules evaluated | Rules evaluated in order (lowest number first) |

---

### S3

---

**Q: What is S3?**

Amazon Simple Storage Service — **object storage** service for storing and retrieving any amount of data. Objects are stored in **buckets**, addressable by unique keys. Highly durable (11 nines), scalable, and supports static website hosting, versioning, lifecycle policies, and event notifications.

---

**Q: Explain S3 storage classes.**

| Class | Use Case |
|---|---|
| S3 Standard | Frequently accessed data |
| S3 Standard-IA | Infrequently accessed, rapid retrieval |
| S3 One Zone-IA | Infrequent access, non-critical |
| S3 Glacier Instant | Archive with millisecond retrieval |
| S3 Glacier Flexible | Archive with minutes-to-hours retrieval |
| S3 Glacier Deep Archive | Long-term archive, cheapest |
| S3 Intelligent-Tiering | Automatically moves data between tiers |

---

**Q: What is versioning?**

S3 versioning maintains **multiple versions of an object** in the same bucket. Enables recovery from accidental deletes or overwrites. Each version has a unique ID. Used on our Terraform state bucket to recover from accidental state corruption.

---

**Q: What is lifecycle management?**

Lifecycle rules automatically **transition objects between storage classes** or **expire (delete) them** after a defined period. Example: move logs to Glacier after 30 days, delete after 365 days.

---

**Q: Difference between S3 and EBS.**

| | S3 | EBS |
|---|---|---|
| **Type** | Object storage | Block storage |
| **Access** | HTTP/S (API) | Attached to EC2 as a disk |
| **Use case** | Files, backups, static assets | OS volumes, databases |
| **Sharing** | Multiple clients | Single EC2 instance (typically) |
| **Persistence** | Independent of compute | Tied to AZ |

---

### RDS

---

**Q: What is Amazon RDS?**

Amazon Relational Database Service — a **managed database service** that automates patching, backups, scaling, and failover for databases like MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server.

---

**Q: Difference between RDS and EC2 database hosting.**

| | RDS | EC2-hosted DB |
|---|---|---|
| **Management** | AWS manages patching, backups, failover | You manage everything |
| **Flexibility** | Limited OS-level control | Full control |
| **HA** | Built-in Multi-AZ | Must configure manually |
| **Effort** | Lower operational overhead | Higher operational overhead |

---

**Q: What is Multi-AZ deployment?**

RDS automatically provisions a **standby replica in a different Availability Zone**. In case of primary failure, RDS automatically fails over to the standby. The endpoint DNS updates automatically — no manual intervention needed. Used for high availability, not for read scaling.

---

**Q: What are read replicas?**

Asynchronous copies of the primary database that handle **read traffic**, offloading the primary. Useful for read-heavy applications. Read replicas can be in the same or different region. Not used for failover (unlike Multi-AZ).

---

### EKS

---

**Q: What is Amazon EKS?**

Amazon Elastic Kubernetes Service — AWS's **managed Kubernetes control plane**. AWS manages the etcd cluster, kube-apiserver, and controller manager. You manage worker nodes (or use Fargate for serverless nodes).

---

**Q: Why choose EKS over self-managed Kubernetes?**

- AWS handles **control plane availability, patching, and upgrades**.
- Native integration with IAM, VPC, ALB, CloudWatch, ECR.
- Reduced operational overhead — no need to manage etcd, API server HA.
- Supports both **managed node groups** and **AWS Fargate**.

---

**Q: How does EKS architecture work?**

- **Control plane:** Managed by AWS (multi-AZ, highly available). Includes API server, etcd, scheduler, controller manager.
- **Worker nodes:** EC2 instances (managed node groups or self-managed) or Fargate profiles running kubelet and kube-proxy.
- **kubectl** communicates with the EKS API server using kubeconfig with AWS auth (IAM-based).
- **aws-auth ConfigMap** maps IAM roles/users to Kubernetes RBAC roles.

---

**Q: Explain worker nodes.**

Worker nodes are EC2 instances that run workloads (pods). Each node runs:
- **kubelet:** Registers node with control plane, manages pod lifecycle.
- **kube-proxy:** Manages iptables rules for Service networking.
- **Container runtime (containerd):** Runs containers.

Managed node groups automate node provisioning, patching, and termination.

---

**Q: Explain Fargate profiles.**

Fargate profiles define which pods run on **AWS Fargate** (serverless compute). Pods matching namespace/label selectors in the profile are scheduled on Fargate — no EC2 nodes to manage. AWS provisions, patches, and scales the underlying compute automatically. Used in the Serverless Game Hosting Platform project.

---

**Q: How does EKS integrate with IAM?**

- **Node IAM Role:** EC2 nodes assume an IAM role to call AWS APIs (ECR pull, CloudWatch Logs, etc.).
- **IRSA (IAM Roles for Service Accounts):** Individual pods assume IAM roles via Kubernetes service accounts and OIDC federation — fine-grained, pod-level AWS permissions without sharing node-level credentials.
- **aws-auth ConfigMap:** Maps IAM users/roles to Kubernetes RBAC.

---

### Load Balancing

---

**Q: What is an ALB?**

Application Load Balancer — an L7 (HTTP/HTTPS) load balancer that routes traffic based on **host, path, headers, or query strings**. Supports TLS termination, sticky sessions, and target groups backed by EC2 instances, IP addresses, or Lambda.

---

**Q: Difference between ALB and NLB.**

| Feature | ALB | NLB |
|---|---|---|
| **Layer** | L7 (HTTP/HTTPS) | L4 (TCP/UDP) |
| **Routing** | Path/host-based | IP/port-based |
| **Use case** | Web apps, microservices, WebSocket | Low-latency, high-throughput, TCP |
| **TLS termination** | Yes | Yes (passthrough also supported) |
| **Static IP** | No (DNS only) | Yes (Elastic IP) |

---

**Q: How does AWS Load Balancer Controller work?**

- A Kubernetes controller running in the EKS cluster.
- Watches for **Ingress resources** with `kubernetes.io/ingress.class: alb` annotation.
- Automatically provisions and configures **ALBs** in AWS based on Ingress rules.
- Also manages **NLBs** for Service type `LoadBalancer`.
- Requires IRSA with permissions to manage EC2, ELB, and ACM resources.

---

### CloudFront

---

**Q: What is CloudFront?**

Amazon CloudFront is a **Content Delivery Network (CDN)** that distributes content from **edge locations** globally, reducing latency for end users. Supports caching, TLS, geo-restriction, WAF integration, and Lambda@Edge.

---

**Q: Why use CloudFront with S3?**

- Reduces latency by serving content from edge locations closer to users.
- Reduces S3 bandwidth costs (edge cache absorbs repeated requests).
- Enables HTTPS for S3 static websites.
- Restricts direct S3 access via **Origin Access Control (OAC)** — users only access content through CloudFront.

---

**Q: Explain caching behavior.**

CloudFront caches responses at edge locations based on the **Cache-Control** and **TTL** settings. Cache behavior defines: which HTTP methods are cached, which headers/query strings vary the cache key, and minimum/maximum TTL values. Cache can be invalidated manually via CloudFront invalidation requests.

---

## 4. Terraform Questions

### Basics

---

**Q: What is Infrastructure as Code?**

IaC is the practice of managing and provisioning infrastructure through **machine-readable definition files** (code) rather than manual processes or UI. Benefits: version control, repeatability, consistency across environments, team collaboration, and auditability.

---

**Q: Why Terraform?**

- **Cloud-agnostic** — works with AWS, GCP, Azure, and many other providers via a unified HCL language.
- **Declarative** — you define the desired state; Terraform figures out how to get there.
- **State management** — tracks real infrastructure state.
- **Modular** — reusable modules prevent code duplication.
- **Large ecosystem** — thousands of community providers and modules.

---

**Q: Difference between Terraform and CloudFormation.**

| Feature | Terraform | CloudFormation |
|---|---|---|
| **Provider support** | Multi-cloud | AWS only |
| **Language** | HCL | JSON/YAML |
| **State management** | Explicit state file | Managed by AWS |
| **Modularity** | Modules | Nested stacks |
| **Community** | Large open-source community | AWS-specific |
| **Import** | `terraform import` | CloudFormation drift detection |

---

**Q: Explain Terraform workflow.**

```
Write .tf files → terraform init (download providers/modules) → terraform validate (syntax check) → terraform plan (preview changes) → terraform apply (provision) → terraform destroy (tear down)
```

---

### Core Concepts

---

**Q: What are providers?**

Providers are plugins that allow Terraform to interact with APIs of cloud platforms or services. Each provider defines resources and data sources.

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

**Q: What are resources?**

Resources are the core building blocks — they represent infrastructure objects (EC2 instance, S3 bucket, RDS instance).

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-app-bucket"
}
```

---

**Q: What are variables?**

Variables parameterize Terraform configurations, allowing reuse across environments without changing code.

```hcl
variable "instance_type" {
  type    = string
  default = "t3.micro"
}
```

---

**Q: What are outputs?**

Outputs expose values from a Terraform configuration for use by other modules or as information after apply.

```hcl
output "cluster_endpoint" {
  value = aws_eks_cluster.main.endpoint
}
```

---

**Q: What are modules?**

Modules are reusable, self-contained packages of Terraform configurations. They accept input variables and return outputs. Used to avoid code duplication across environments.

```
modules/
  vpc/
    main.tf
    variables.tf
    outputs.tf
```

---

**Q: Why create reusable modules?**

- **DRY (Don't Repeat Yourself)** — define VPC or EKS configuration once, reuse for dev/staging/prod.
- **Consistency** — all environments use the same module version.
- **Maintainability** — updates in one place propagate everywhere the module is used.
- **Testing** — modules can be independently tested.

---

### State Management

---

**Q: What is terraform.tfstate?**

A JSON file that maps Terraform resource definitions to real infrastructure. Tracks metadata like resource IDs, attributes, and dependencies. Terraform uses it to compute diffs between desired and actual state.

---

**Q: Why is state important?**

- Terraform cannot determine what exists in the real world without it.
- Used to generate the execution plan — shows what will be created, updated, or destroyed.
- Tracks resource dependencies for correct ordering.
- Enables `terraform destroy` to know what to remove.

---

**Q: What problems occur if state is lost?**

- Terraform loses track of existing infrastructure — running `apply` could create **duplicate resources**.
- Resources become **orphaned** — Terraform no longer manages them.
- Recovery requires `terraform import` for each resource, which is tedious and error-prone.

---

**Q: How does state locking work?**

When using a remote backend (S3 + DynamoDB), Terraform writes a lock record to DynamoDB at the start of any operation that modifies state (`plan`, `apply`, `destroy`). Other operations are blocked until the lock is released. Prevents concurrent modifications that could corrupt state.

---

**Q: Explain remote backend.**

A remote backend stores state outside the local machine — in our case, S3 + DynamoDB. S3 stores the state file, DynamoDB provides locking. Benefits: team collaboration, state versioning (S3 versioning), no local state drift, auditability.

---

### Advanced

---

**Q: Difference between count and for_each.**

| Feature | `count` | `for_each` |
|---|---|---|
| Input | Integer | Map or Set |
| Resource identity | Index-based (`[0]`, `[1]`) | Key-based (`["key"]`) |
| Add/remove | Reindexes remaining resources | Only affects the specific key |
| Use case | Identical resources | Resources with distinct configurations |

**Recommendation:** Prefer `for_each` — adding/removing items in `count` can trigger unintended replacements.

---

**Q: What are workspaces?**

Terraform workspaces maintain **separate state files within the same configuration**. Useful for lightweight environment separation.

```bash
terraform workspace new dev
terraform workspace select prod
```

Limitation: Workspaces share the same code, making large environment differences awkward. Separate directories with separate state files (using `-backend-config`) are often cleaner.

---

**Q: What is terraform import?**

Brings **existing infrastructure** under Terraform management by importing its state — without recreating the resource.

```bash
terraform import aws_s3_bucket.my_bucket my-existing-bucket-name
```

The resource block must already exist in `.tf` files.

---

**Q: What is terraform taint?**

Marks a resource as "tainted" — Terraform will **destroy and recreate** it on the next apply. Useful for forcing recreation of a resource without changing its configuration. (In Terraform 0.15.2+, replaced by `terraform apply -replace="resource.name"`.)

---

**Q: How would you manage multiple environments?**

Two common patterns:
1. **Separate directories per environment** (`envs/dev/`, `envs/staging/`, `envs/prod/`) each with their own backend config and `.tfvars`.
2. **Terraform workspaces** with a single directory and workspace-aware variables.

Pattern 1 is preferred for environments with significant differences. Used per-environment state paths (`envs/prod/terraform.tfstate`) with shared modules.

---

## 5. Docker Questions

---

**Q: What is Docker?**

Docker is a platform for building, packaging, and running applications in **containers** — isolated, portable environments that bundle application code with its dependencies.

---

**Q: Why containers?**

- **Consistency:** Runs the same way on developer laptop, CI, staging, and production.
- **Isolation:** Processes are isolated from the host and other containers.
- **Lightweight:** Shares the host kernel — far less overhead than VMs.
- **Fast startup:** Containers start in seconds.
- **Portability:** Run on any OS with Docker installed.

---

**Q: Difference between containers and VMs.**

| Feature | Container | VM |
|---|---|---|
| **Isolation** | Process-level (namespaces/cgroups) | Hardware-level (hypervisor) |
| **OS** | Shares host kernel | Own OS per VM |
| **Startup time** | Seconds | Minutes |
| **Size** | MBs | GBs |
| **Overhead** | Low | High |

---

**Q: Explain Docker architecture.**

- **Docker Client:** CLI that sends commands to Docker Daemon.
- **Docker Daemon (dockerd):** Manages images, containers, volumes, networks.
- **Docker Registry:** Stores images (Docker Hub, ECR, etc.).
- **containerd:** Low-level container runtime used by dockerd.

---

**Q: What is a Docker image?**

A **read-only template** consisting of layered filesystems, used to create containers. Built from a `Dockerfile`. Each instruction in the Dockerfile creates a new layer (cached for faster rebuilds).

---

**Q: What is a container?**

A **running instance of an image** — an isolated process with its own filesystem (from the image), network, and PID namespace. Writable layer is added on top of the image layers.

---

**Q: What is Docker Hub?**

Docker Hub is the default public Docker registry for storing and sharing images. Alternatives: AWS ECR (used in our setup), GitHub Container Registry, self-hosted Harbor.

---

**Q: Explain Dockerfile instructions.**

| Instruction | Purpose |
|---|---|
| `FROM` | Base image |
| `RUN` | Execute commands during build |
| `COPY` / `ADD` | Copy files into image |
| `ENV` | Set environment variables |
| `EXPOSE` | Document container port |
| `CMD` | Default command at runtime |
| `ENTRYPOINT` | Fixed entry process |
| `WORKDIR` | Set working directory |
| `ARG` | Build-time variable |
| `USER` | Set runtime user |

---

**Q: Difference between CMD and ENTRYPOINT.**

- **ENTRYPOINT:** The fixed executable that always runs. Cannot be overridden by `docker run` arguments (unless `--entrypoint` is used).
- **CMD:** Default arguments to ENTRYPOINT, or the default command if no ENTRYPOINT is set. Overridden easily by `docker run <image> <command>`.

```dockerfile
ENTRYPOINT ["./app"]     # Always runs ./app
CMD ["--port", "8080"]   # Default args, overridable
```

---

**Q: What are multi-stage builds?**

Multi-stage builds use **multiple `FROM` statements** in a single Dockerfile. Earlier stages compile or build the application; the final stage copies only the built artifact — discarding build tools, compilers, and source code.

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o server .

# Stage 2: Run (Distroless)
FROM gcr.io/distroless/static
COPY --from=builder /app/server /server
ENTRYPOINT ["/server"]
```

---

**Q: Why use Distroless images?**

- **Minimal attack surface** — contains only the application binary and runtime dependencies, no shell, no package manager, no OS utilities.
- **Smaller image size** — faster pulls and deploys.
- **Security compliance** — fewer vulnerabilities detectable by Trivy or similar scanners.
- Used in the Go Cloud-Native GitOps project.

---

**Q: How do you optimize image size?**

- Use **multi-stage builds** to discard build tools.
- Use **Distroless or Alpine** base images.
- Combine `RUN` commands with `&&` to reduce layers.
- Use `.dockerignore` to exclude unnecessary files.
- Remove package manager caches in the same `RUN` layer.

---

**Q: How do you debug containers?**

```bash
# Check logs
docker logs <container-id>

# Execute shell in running container
docker exec -it <container-id> /bin/sh

# Inspect container metadata
docker inspect <container-id>

# Check resource usage
docker stats <container-id>

# For Kubernetes pods
kubectl logs <pod-name> -n <namespace>
kubectl exec -it <pod-name> -- /bin/sh
```

---

**Q: What is Docker networking?**

Docker has several network drivers:
- **bridge (default):** Private internal network on the host; containers communicate via internal IP.
- **host:** Container shares host's network stack directly.
- **none:** No networking.
- **overlay:** Multi-host networking (used in Docker Swarm; Kubernetes handles its own networking).
- Custom bridge networks allow containers to reference each other by name.

---

**Q: What is a Docker volume?**

Volumes provide **persistent storage** that survives container restarts and is decoupled from the container lifecycle. Managed by Docker, stored under `/var/lib/docker/volumes/`. Preferred over bind mounts for production data persistence.

---

## 6. Kubernetes Questions

### Fundamentals

---

**Q: What is Kubernetes?**

Kubernetes (K8s) is an **open-source container orchestration platform** for automating deployment, scaling, and management of containerized applications. Manages a cluster of nodes, scheduling pods, maintaining desired state, and handling failures automatically.

---

**Q: Why Kubernetes?**

- **Self-healing:** Restarts failed containers, reschedules on healthy nodes.
- **Auto-scaling:** HPA scales pods based on load.
- **Rolling updates:** Zero-downtime deployments.
- **Service discovery & load balancing:** Built-in DNS and Services.
- **Declarative configuration:** Desired state defined in YAML; Kubernetes reconciles to it.

---

**Q: Explain cluster architecture.**

- **Control Plane:** API Server (entry point), etcd (state store), Scheduler (places pods on nodes), Controller Manager (runs control loops).
- **Worker Nodes:** kubelet (node agent), kube-proxy (networking), container runtime (containerd).
- **kubectl:** CLI for interacting with the API Server.

---

**Q: Difference between master and worker nodes.**

| | Master/Control Plane | Worker Node |
|---|---|---|
| **Purpose** | Manages cluster state | Runs application pods |
| **Components** | API server, etcd, scheduler, controller manager | kubelet, kube-proxy, container runtime |
| **Workloads** | System workloads (in managed K8s, none) | Application pods |

---

### Resources

---

**Q: What is a Pod?**

The smallest deployable unit in Kubernetes. A Pod encapsulates one or more containers that share a network namespace (same IP) and storage volumes. Containers in a pod communicate via `localhost`.

---

**Q: What is a Deployment?**

A Deployment manages a set of **identical, stateless pods** via a ReplicaSet. Provides declarative updates (rolling update, rollback), self-healing (reschedules failed pods), and scaling.

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
```

---

**Q: What is a ReplicaSet?**

A ReplicaSet ensures a **specified number of identical pod replicas** are always running. Typically managed by a Deployment — you rarely create ReplicaSets directly. If a pod fails, the ReplicaSet creates a replacement.

---

**Q: What is a Service?**

A Service provides a **stable virtual IP (ClusterIP) and DNS name** to access a group of pods, regardless of pod IP changes. Types:
- **ClusterIP:** Internal only (default).
- **NodePort:** Exposes on a static port on each node.
- **LoadBalancer:** Provisions a cloud load balancer.
- **ExternalName:** DNS alias to an external service.

---

**Q: What is an Ingress?**

An Ingress exposes **HTTP/HTTPS routes** from outside the cluster to Services inside, based on host and path rules. Requires an **Ingress Controller** (e.g., AWS Load Balancer Controller, nginx) to implement the rules.

---

**Q: What is a ConfigMap?**

A ConfigMap stores **non-sensitive configuration data** as key-value pairs or files. Decouples config from the container image. Mounted as env vars or volume files in pods.

---

**Q: What is a Secret?**

A Secret stores **sensitive data** (passwords, tokens, keys) in base64-encoded form. Mounted as env vars or volume files. Access controlled via RBAC. For stronger security, backend with AWS Secrets Manager or Vault.

---

**Q: What is a Namespace?**

A Namespace provides **logical isolation** within a cluster — separate virtual clusters. Used to separate environments (dev/staging), teams, or applications. RBAC, resource quotas, and network policies can be scoped to namespaces.

---

### Scaling

---

**Q: What is HPA?**

Horizontal Pod Autoscaler — automatically adjusts the **number of pod replicas** based on CPU/memory utilization or custom metrics. Requires Metrics Server. Checks metrics every 15 seconds by default.

---

**Q: How does Kubernetes scale applications?**

1. HPA monitors metrics via the Metrics Server.
2. If current utilization exceeds target (e.g., 70% CPU), HPA calculates needed replicas.
3. HPA updates the Deployment's `spec.replicas`.
4. Deployment controller creates new pods via the ReplicaSet.
5. Scheduler places new pods on available nodes.
6. If nodes are insufficient, **Cluster Autoscaler** provisions new EC2 nodes.

---

**Q: Difference between HPA and Cluster Autoscaler.**

| | HPA | Cluster Autoscaler |
|---|---|---|
| **What scales** | Pods | Nodes (EC2 instances) |
| **Trigger** | CPU/memory/custom metrics | Pending pods that can't be scheduled |
| **Speed** | Fast (seconds) | Slower (minutes for new nodes) |
| **Use case** | Application-level scaling | Infrastructure-level scaling |

They work **together**: HPA adds pods, Cluster Autoscaler adds nodes when pods can't be scheduled.

---

### Troubleshooting

---

**Q: Pod is not starting. What will you check?**

```bash
# Check pod status and reason
kubectl get pods -n <namespace>
kubectl describe pod <pod-name> -n <namespace>   # Events section is key

# Check logs
kubectl logs <pod-name> -n <namespace>
kubectl logs <pod-name> --previous   # Logs from previous crash

# Common causes:
# - ImagePullBackOff: wrong image name/tag, ECR auth issue
# - CrashLoopBackOff: app crashing at startup (check logs)
# - Pending: node resources insufficient (check events)
# - OOMKilled: pod exceeded memory limit
# - Error: missing Secret or ConfigMap
```

---

**Q: Application is not accessible. How do you troubleshoot?**

1. Check Pod status: `kubectl get pods`
2. Check Service: `kubectl get svc` — correct selector labels?
3. Check Ingress: `kubectl describe ingress` — correct service/port?
4. Check ALB/NLB in AWS console — target group health.
5. `kubectl port-forward svc/<service> 8080:80` — test direct Service access.
6. Check application logs for errors.
7. Verify Security Groups — allow traffic from ALB to node ports.

---

**Q: How do you inspect logs?**

```bash
kubectl logs <pod-name>                    # Current logs
kubectl logs <pod-name> -f                 # Follow (tail)
kubectl logs <pod-name> --previous         # Previous container
kubectl logs <pod-name> -c <container>     # Specific container in multi-container pod
kubectl logs -l app=my-app --all-containers  # All pods with a label
```

---

**Q: What kubectl commands do you use regularly?**

```bash
kubectl get pods/deployments/svc/ingress -n <ns>
kubectl describe pod <name>
kubectl logs <pod>
kubectl exec -it <pod> -- /bin/sh
kubectl apply -f manifest.yaml
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
kubectl scale deployment/<name> --replicas=5
kubectl top pods / kubectl top nodes
kubectl get events --sort-by=.lastTimestamp
kubectl config get-contexts
```

---

**Q: What happens when a node fails?**

1. kubelet on the failed node stops sending heartbeats to the control plane.
2. After the **node eviction timeout** (default ~5 minutes), the node is marked `NotReady`.
3. Node controller **evicts pods** from the failed node.
4. The Deployment controller schedules replacement pods on remaining healthy nodes.
5. If Cluster Autoscaler is configured and insufficient capacity exists, a new node is provisioned.

---

## 7. GitHub Actions & GitOps Questions

---

**Q: What is GitHub Actions?**

GitHub Actions is a **CI/CD platform built into GitHub**. Workflows are defined in YAML files (`.github/workflows/`) and triggered by events (push, PR, schedule, manual). Each workflow runs on **runners** (GitHub-hosted or self-hosted).

---

**Q: Explain workflow structure.**

```yaml
name: CI Pipeline           # Workflow name
on:                         # Trigger
  push:
    branches: [main]

jobs:                       # One or more jobs
  build:                    # Job name
    runs-on: ubuntu-latest  # Runner
    steps:                  # Steps within the job
      - uses: actions/checkout@v4
      - name: Run tests
        run: go test ./...
```

---

**Q: What are runners?**

Runners are compute environments that execute workflow jobs:
- **GitHub-hosted:** Ubuntu, Windows, macOS VMs managed by GitHub. Fresh environment per job.
- **Self-hosted:** Your own machines/VMs. Useful when needing access to private networks or custom tooling.

In the GitOps project, GitHub-hosted Ubuntu runners were used for Docker builds and Kubernetes manifest updates.

---

**Q: How are secrets managed?**

- Stored in **GitHub Repository Secrets** (Settings → Secrets and variables → Actions).
- Accessed in workflows via `${{ secrets.SECRET_NAME }}`.
- Never logged or exposed in workflow output.
- Used for: AWS credentials, Docker registry credentials, Kubernetes config.

---

**Q: What triggers workflows?**

Common triggers used:
- `push` to a branch
- `pull_request`
- `workflow_dispatch` (manual trigger)
- `schedule` (cron)
- `workflow_call` (called by another workflow)
- `release` (on new GitHub release)

---

**Q: What is GitOps?**

GitOps is an operational model where **Git is the single source of truth** for infrastructure and application state. All changes go through Git (PR → review → merge). A GitOps operator (Argo CD, Flux) continuously reconciles the cluster state with what's defined in Git.

---

**Q: Why GitOps?**

- **Auditability:** Every change is a Git commit with author and timestamp.
- **Rollback:** Revert to any previous state with `git revert`.
- **Consistency:** Cluster state always matches Git — no config drift.
- **Security:** Cluster doesn't need external push access — Argo CD pulls from Git.
- **Separation of concerns:** CI builds images; CD handles deployment.

---

**Q: How does Argo CD work?**

1. Argo CD runs in the Kubernetes cluster.
2. It watches a Git repository for Kubernetes manifests (or Helm charts).
3. It **continuously compares** live cluster state to the desired state in Git.
4. When it detects drift (or a new commit), it **syncs the cluster** to match Git.
5. Sync can be automatic or manual.

---

**Q: How does Argo CD detect changes?**

- Argo CD **polls the Git repository** at a configurable interval (default 3 minutes).
- For faster updates, GitHub webhooks can be configured to push notifications to Argo CD.
- Upon detecting a new commit, Argo CD marks the application as `OutOfSync` and syncs if auto-sync is enabled.

---

**Q: Difference between Jenkins and Argo CD.**

| Feature | Jenkins | Argo CD |
|---|---|---|
| **Purpose** | CI (build, test) + CD (push-based deploy) | CD only (GitOps/pull-based) |
| **Model** | Push-based | Pull-based |
| **State tracking** | No cluster state tracking | Full cluster state visibility |
| **Rollback** | Manual pipeline rerun | Git revert or one-click in UI |
| **Configuration** | Jenkinsfile | Application CR + Git |

**Our approach:** Jenkins handles CI (build, test, push image, update manifest), Argo CD handles CD (sync manifest to cluster).

---

**Q: How does Git become the source of truth?**

- Application manifests (Kubernetes YAML or Helm values) are stored in a dedicated Git repository.
- **No manual `kubectl apply`** in production — all changes go through Git.
- Argo CD enforces cluster state matches Git. Manual changes are overwritten on next sync.
- Environment promotions happen via PRs to branch or directory representing that environment.

---

## 8. Monitoring & Observability Questions

---

**Q: What is observability?**

Observability is the ability to understand the **internal state of a system** from its external outputs. A system is observable if you can answer: "what's happening, why, and where?" without deploying new code to answer the question.

---

**Q: Difference between monitoring and observability.**

| | Monitoring | Observability |
|---|---|---|
| **Question** | "Is something wrong?" | "Why is something wrong?" |
| **Approach** | Watch known metrics and alert on thresholds | Explore unknown failure modes via metrics, logs, and traces |
| **Tools** | Dashboards, alerts | Distributed tracing, correlated telemetry |
| **Scope** | Known-unknowns | Unknown-unknowns |

---

**Q: What are metrics?**

Numeric measurements of system behavior over time — e.g., CPU usage, request count, error rate, latency. Aggregated and stored in time-series databases (Prometheus). Suitable for alerting and trend analysis.

---

**Q: What are logs?**

Timestamped text records of discrete events — e.g., an HTTP request, an error message, a database query. Useful for detailed debugging. Less efficient than metrics at scale. Aggregated via tools like Loki, Datadog, or CloudWatch Logs.

---

**Q: What are traces?**

Distributed traces track the **journey of a request** across multiple services in a microservices system. Each service creates a span; spans are linked by a trace ID. Tools: Jaeger, AWS X-Ray, Datadog APM. Useful for identifying latency bottlenecks across services.

---

**Q: Explain Prometheus architecture.**

- **Prometheus Server:** Scrapes metrics endpoints (HTTP `/metrics`) at configured intervals, stores time-series data.
- **Exporters:** Expose metrics in Prometheus format. Examples: `node-exporter` (host metrics), `kube-state-metrics` (Kubernetes object metrics).
- **Alertmanager:** Receives alerts from Prometheus rules, routes to notification channels (Slack, PagerDuty, email).
- **Grafana:** Queries Prometheus via PromQL for visualization.
- **Service Discovery:** Prometheus auto-discovers Kubernetes pods/services via the Kubernetes API.

---

**Q: What is scraping?**

Scraping is Prometheus's **pull-based metric collection** mechanism. Prometheus sends HTTP GET requests to targets' `/metrics` endpoints at configured intervals (default 15s), parses the response, and stores the data as time-series.

---

**Q: What are exporters?**

Exporters are agents that expose metrics from systems that don't natively support the Prometheus format:
- **node-exporter:** Linux/host metrics (CPU, memory, disk, network).
- **kube-state-metrics:** Kubernetes object state metrics (pod count, deployment status).
- **blackbox-exporter:** Probes HTTP, TCP, ICMP endpoints for uptime/latency.
- **mysqld-exporter:** MySQL metrics.

---

**Q: What is Grafana used for?**

Grafana provides **interactive dashboards** for querying and visualizing metrics from data sources including Prometheus, CloudWatch, and Datadog. Used to:
- Monitor real-time application and infrastructure health.
- Investigate incidents by correlating metrics across services.
- Display business-level KPIs alongside technical metrics.
- Configure visual alerts.

---

**Q: What dashboards did you build?**

- **Kubernetes Cluster Overview:** Node CPU/memory, pod count, namespace resource usage.
- **Application Metrics:** Request rate, error rate, P50/P95/P99 latency.
- **EKS Pod Dashboard:** Per-pod CPU/memory, restart count, HPA status.
- **Infrastructure Dashboard:** EC2 metrics (from node-exporter), RDS connections and latency.

---

**Q: What alerts would you configure?**

| Alert | Threshold | Severity |
|---|---|---|
| Pod CrashLoopBackOff | Restart count > 5 in 10 min | Critical |
| High CPU | Node CPU > 85% for 5 min | Warning |
| High Memory | Node memory > 90% for 5 min | Warning |
| Pod not ready | Pod in non-ready state > 2 min | Critical |
| High error rate | HTTP 5xx rate > 5% for 2 min | Critical |
| RDS connections near limit | > 80% of max connections | Warning |

---

**Q: How would you investigate high CPU usage?**

1. Check Grafana — which node/pod is consuming high CPU?
2. `kubectl top pods -n <namespace>` — identify the specific pod.
3. `kubectl top nodes` — check if it's node-level saturation.
4. `kubectl describe pod <pod>` — check resource limits/requests.
5. Check application logs for infinite loops, runaway processes, or high request volume.
6. Check if HPA has already triggered — add replicas to distribute load.
7. Check if a recent deployment correlates with the spike.
8. For root-cause: check Prometheus PromQL for CPU per container, correlate with request rate.

---

## 9. Linux Questions

---

**Q: Explain the Linux boot process.**

1. **BIOS/UEFI** performs POST, finds bootable device.
2. **Bootloader (GRUB2)** loads the kernel.
3. **Kernel** initializes hardware, mounts root filesystem.
4. **init/systemd (PID 1)** starts as the first process.
5. **systemd** reads unit files, starts services in dependency order.
6. System reaches the target runlevel (multi-user or graphical).

---

**Q: What are systemd services?**

systemd services are background processes managed by systemd:
```bash
systemctl start/stop/restart/status <service>   # Manage services
systemctl enable/disable <service>              # Start on boot
journalctl -u <service>                         # View service logs
```
Service definitions are in `.service` unit files under `/etc/systemd/system/`.

---

**Q: How do you check running processes?**

```bash
ps aux                     # All processes
ps aux | grep <name>       # Filter by name
top / htop                 # Interactive process viewer
pgrep <name>               # Get PID by name
kill -9 <pid>              # Force kill
```

---

**Q: Difference between grep and awk.**

| Tool | Purpose |
|---|---|
| `grep` | Search for patterns in text. Returns matching lines. |
| `awk` | Field-based text processing. Can extract columns, perform calculations, conditional logic. |

```bash
grep "ERROR" app.log                     # Find lines with ERROR
awk '{print $1, $3}' access.log          # Print 1st and 3rd columns
awk '/ERROR/ {count++} END {print count}' app.log  # Count errors
```

---

**Q: What is a cron job?**

A cron job is a **scheduled task** in Linux, defined in the crontab:
```bash
crontab -e   # Edit user crontab
```
```
# Format: minute hour day month weekday command
0 2 * * * /scripts/backup.sh     # Run at 2 AM daily
*/5 * * * * /scripts/health.sh   # Run every 5 minutes
```

---

**Q: How do you check disk usage?**

```bash
df -h                    # Filesystem disk usage (human-readable)
du -sh /var/log/*        # Directory sizes
du -sh * | sort -rh      # Sort by size
lsblk                    # Block device information
```

---

**Q: How do you check memory usage?**

```bash
free -h                  # Total, used, free memory
vmstat -s                # Detailed memory stats
cat /proc/meminfo        # Raw memory info
top / htop               # Per-process memory usage
```

---

**Q: What is the difference between soft and hard links?**

| | Soft Link (Symlink) | Hard Link |
|---|---|---|
| **Points to** | Path of the target file | Same inode as the target |
| **Cross-filesystem** | Yes | No |
| **If target deleted** | Broken link (dangling) | File still accessible |
| **Command** | `ln -s target link` | `ln target link` |

---

**Q: Explain file permissions.**

Linux file permissions are represented as three groups (Owner, Group, Others), each with read (r=4), write (w=2), execute (x=1) bits.

```bash
ls -l file.sh
# -rwxr-xr--  1 user group 1024 Jan 01 app.sh
#  rwx = owner can read, write, execute
#  r-x = group can read, execute
#  r-- = others can read only
```

---

**Q: What does chmod 755 mean?**

- **7 (Owner):** rwx — Read + Write + Execute.
- **5 (Group):** r-x — Read + Execute.
- **5 (Others):** r-x — Read + Execute.

Typical for executables/scripts — owner can modify, everyone can read and run.

---

**Q: How do you troubleshoot a server running slowly?**

```bash
# 1. Check CPU and memory
top / htop

# 2. Find top CPU consumers
ps aux --sort=-%cpu | head -10

# 3. Check memory and swap
free -h
vmstat 1 5

# 4. Check disk I/O
iostat -x 1 5
iotop

# 5. Check disk space
df -h

# 6. Check network
netstat -tuln
iftop

# 7. Check system logs
journalctl -n 100 --no-pager
dmesg | tail -20

# 8. Check load average
uptime
```

---

## 10. Bash & Python Questions

---

**Q: Why did you use Bash scripts?**

- Bash is native to Linux — no installation required.
- Ideal for **task automation** that involves Linux commands, file manipulation, and system calls.
- Used for CI/CD support scripts, Docker build automation, Kubernetes deployment helpers, and cron-based maintenance tasks.
- Fast to write for straightforward automation.

---

**Q: What automation did you build?**

- **CI/CD support scripts:** Automated Docker image tag generation, manifest file patching with new image versions, and post-deploy health checks.
- **Kubernetes operations:** Scripts for log aggregation, namespace resource audits, and cleanup of old ReplicaSets.
- **AWS automation:** Python scripts using `boto3` for S3 cleanup, IAM policy audits, and EKS cluster status checks.
- **Linux admin:** Cron-based log rotation and disk usage alerting scripts.

---

**Q: Difference between Bash and Python.**

| Feature | Bash | Python |
|---|---|---|
| **Best for** | System commands, file operations, piping | Complex logic, APIs, data processing |
| **Readability** | Decreases with complexity | Scales well |
| **Error handling** | Limited (exit codes) | Robust (try/except) |
| **Libraries** | System tools only | Rich ecosystem (boto3, requests, etc.) |
| **Portability** | Linux/macOS | Any OS with Python installed |

**Rule of thumb:** Use Bash for glue/system tasks; switch to Python when logic gets complex or you need external APIs.

---

**Q: How do you pass arguments to a Bash script?**

```bash
#!/bin/bash
ENVIRONMENT=$1    # First argument
IMAGE_TAG=$2      # Second argument

echo "Deploying $IMAGE_TAG to $ENVIRONMENT"

# Usage:
# ./deploy.sh prod v1.2.3
```

Named arguments via `getopts` or `--flag` patterns for more complex scripts.

---

**Q: How do you schedule scripts?**

```bash
# Using cron
crontab -e
0 6 * * * /home/ubuntu/scripts/cleanup.sh >> /var/log/cleanup.log 2>&1

# Using systemd timers (more reliable, better logging)
# Create .service and .timer unit files
systemctl enable --now myscript.timer
```

---

**Q: What Python modules have you used?**

- `boto3` — AWS SDK for Python (EC2, S3, EKS, IAM operations).
- `os` / `subprocess` — OS operations and running shell commands.
- `json` — Parsing and generating JSON (AWS API responses, config files).
- `requests` — HTTP calls (Slack notifications, webhook triggers).
- `sys` — Script arguments and exit codes.
- `logging` — Structured logging in automation scripts.

---

**Q: How would you parse a JSON file in Python?**

```python
import json

with open('config.json', 'r') as f:
    data = json.load(f)

# Access values
region = data['aws']['region']
print(region)

# Handle missing keys safely
cluster_name = data.get('eks', {}).get('cluster_name', 'default')
```

---

**Q: How would you automate AWS tasks using Python?**

```python
import boto3

# List EC2 instances
ec2 = boto3.client('ec2', region_name='ap-south-1')
response = ec2.describe_instances()

for reservation in response['Reservations']:
    for instance in reservation['Instances']:
        print(f"Instance: {instance['InstanceId']}, State: {instance['State']['Name']}")

# S3 operations
s3 = boto3.client('s3')
s3.upload_file('local-file.txt', 'my-bucket', 's3-key.txt')

# EKS cluster info
eks = boto3.client('eks', region_name='ap-south-1')
clusters = eks.list_clusters()
print(clusters['clusters'])
```

---

## 11. Security Questions

---

**Q: What is DevSecOps?**

DevSecOps integrates **security practices into the DevOps pipeline** — security is everyone's responsibility, built into every stage rather than bolted on at the end. Includes SAST, DAST, image scanning, dependency scanning, secret scanning, and runtime security.

---

**Q: Why integrate SonarQube?**

- Automates **Static Application Security Testing (SAST)** in the CI pipeline.
- Detects code quality issues, security vulnerabilities, code smells, and duplicate code before they reach production.
- Enforces a **Quality Gate** — pipeline fails if code quality thresholds are not met.
- Provides trend analysis on code quality over time.

---

**Q: What issues can SonarQube detect?**

- **Security vulnerabilities:** SQL injection risks, XSS, hardcoded credentials, insecure crypto.
- **Code smells:** Complex methods, dead code, poor naming.
- **Bugs:** Null pointer dereferences, incorrect conditions, resource leaks.
- **Duplications:** Copy-pasted code blocks.
- **Coverage gaps:** Code lines not covered by tests.

---

**Q: What is Trivy?**

Trivy is an open-source **vulnerability scanner** by Aqua Security. Scans:
- Docker images (OS packages, language dependencies).
- Kubernetes manifests (misconfigurations).
- Terraform files (IaC misconfigurations).
- Git repositories (secret scanning).

```bash
trivy image my-app:latest
trivy k8s --report summary cluster
```

---

**Q: What vulnerabilities does Trivy scan?**

- **OS packages:** Known CVEs in Alpine, Ubuntu, Debian packages.
- **Application dependencies:** Vulnerabilities in npm, pip, Maven, Go modules.
- **IaC misconfigurations:** Terraform/Kubernetes config issues (publicly accessible S3, no resource limits).
- **Secrets:** Exposed API keys, tokens, credentials in files/images.

---

**Q: How do you secure Docker images?**

- Use **Distroless or minimal base images** (fewer packages = fewer vulnerabilities).
- Run as a **non-root user** (`USER nonroot` in Dockerfile).
- Use **multi-stage builds** to exclude dev tools from final image.
- Scan images with **Trivy** before pushing to registry.
- Set image tags to **specific digests** (not `latest`) in production.
- Enable **ECR image scanning** on push in AWS.

---

**Q: How do you secure Kubernetes Secrets?**

- **Encrypt at rest:** Enable etcd encryption for Secrets in the cluster.
- Use **RBAC** to restrict which service accounts and users can read Secrets.
- Use **AWS Secrets Manager or HashiCorp Vault** as the authoritative store; sync to Kubernetes via External Secrets Operator.
- Avoid logging Secret values in application logs or pipeline output.
- Use **IRSA** to avoid storing AWS credentials as Kubernetes Secrets altogether.

---

**Q: What security best practices do you follow in AWS?**

- **Least privilege IAM:** Every role/user has only required permissions.
- **No long-lived access keys** for services — use IAM roles and IRSA.
- **Private subnets** for EKS nodes and RDS — no direct internet exposure.
- **Security Groups** tightly scoped (e.g., RDS only accessible from EKS node security group).
- **S3 bucket policies:** Block public access enabled; bucket policy restricts to specific principals.
- **CloudTrail** enabled for API audit logging.
- **ECR image scanning** on push.
- **Encrypted EBS volumes and RDS storage** at rest.

---

**Q: What is IAM least privilege?**

Least privilege means granting **exactly the permissions needed to perform a task** — no more. Examples:
- A Jenkins agent that only deploys to EKS should have `eks:DescribeCluster` but not `eks:DeleteCluster`.
- An EKS node role should have `ecr:GetAuthorizationToken` but not `iam:*`.

Achieved by writing scoped IAM policies rather than attaching `AdministratorAccess`.

---

**Q: How would you secure a CI/CD pipeline?**

- **Secrets:** Store in Jenkins Credentials or GitHub Secrets — never in code or logs.
- **Image scanning:** Trivy scan in pipeline before push.
- **Code quality gates:** SonarQube blocking merges on vulnerabilities.
- **Least privilege:** Jenkins/GitHub Actions service accounts with minimal permissions.
- **Branch protection:** Require PR reviews before merge to main.
- **Signed commits:** Enforce commit signing in repositories.
- **Pipeline integrity:** Pin action versions (`@v4`, not `@main`); pin Docker base image digests.
- **Network isolation:** Jenkins in private subnet, not publicly accessible.

---

## 12. Project-Specific Questions

### Go Cloud-Native GitOps Delivery Pipeline

---

**Q: Explain the complete project architecture.**

```
Developer pushes code to GitHub (main branch)
            ↓
GitHub Actions CI Workflow:
  - Checkout code
  - Run unit tests
  - SonarQube / code quality checks
  - Build multi-stage Dockerfile (Distroless final image)
  - Push image to ECR with commit SHA tag
  - Update Helm values.yaml in GitOps repo with new image tag
  - Commit and push to GitOps repo
            ↓
Argo CD (running in EKS) detects change in GitOps repo
            ↓
Argo CD syncs Helm chart to EKS cluster
  - Applies Deployment, Service, Ingress resources
            ↓
AWS ALB routes external traffic to Go app pods
```

---

**Q: Why did you choose GitHub Actions?**

- **Native GitHub integration:** No separate server to manage; workflows live in the same repo.
- **Free for open-source** and generous minutes for private repos.
- **Rich marketplace:** Pre-built actions for Docker, AWS, Helm, etc.
- **Secrets management** built in.
- Ideal for CI (build, test, image push) — pairs cleanly with Argo CD handling CD.

---

**Q: Why use Argo CD?**

- **GitOps model:** Git is always the source of truth for cluster state.
- **Automatic drift detection and reconciliation:** Prevents manual changes from persisting.
- **Visual UI:** Clear view of sync status, resource health, and deployment history.
- **Built-in rollback:** Roll back to any previous Git commit with one click.
- **Pull-based model:** More secure than push-based — cluster pulls from Git, no external systems need cluster access.

---

**Q: Why use Helm?**

- Helm **templates** Kubernetes YAML — avoids duplication across environments.
- Values files (`values.yaml`) parameterize deployments (image tag, replica count, resource limits) cleanly.
- Helm manages versioned **releases** — easy upgrades and rollbacks.
- Argo CD has native Helm support — renders charts and applies to cluster.

---

**Q: Explain your deployment workflow.**

1. Developer pushes to `main` → GitHub Actions CI triggers.
2. Tests pass, Docker image built and pushed to ECR as `app:<commit-sha>`.
3. GitHub Actions updates `values.yaml` in the GitOps repository: `image.tag: <commit-sha>`.
4. Argo CD detects the new commit in the GitOps repo (poll or webhook).
5. Argo CD renders the Helm chart with updated values, applies to EKS.
6. Deployment rolling-updates pods with the new image.
7. Argo CD reports sync status as `Synced` and application health as `Healthy`.

---

**Q: How did you handle rollbacks?**

- **Argo CD rollback:** In the Argo CD UI, navigate to History and Rollback → select a previous revision → sync to that commit.
- **Kubernetes rollback:** `kubectl rollout undo deployment/<name>` reverts to the previous ReplicaSet.
- **Git revert:** `git revert <commit>` in the GitOps repo triggers Argo CD to re-sync to the reverted state (auditable).

---

**Q: Why use Distroless images?**

- The Go binary compiles to a **self-contained static binary** — no OS libraries needed at runtime.
- Distroless `gcr.io/distroless/static` contains only the binary and CA certificates — no shell, no package manager.
- Trivy scans show minimal or zero OS CVEs.
- Smallest possible attack surface for a production container.

---

**Q: What happens after code is pushed?**

1. GitHub webhook fires → GitHub Actions workflow starts.
2. Code checked out, unit tests executed.
3. Docker multi-stage build: Go compiler stage → Distroless final stage.
4. Image tagged with `sha-<commit-hash>`, pushed to ECR.
5. GitOps repo `values.yaml` updated with new image tag.
6. Argo CD detects change, syncs cluster.
7. Kubernetes rolling update deploys new pods.
8. Old pods terminated after new pods pass readiness checks.

---

### Serverless Game Hosting Platform

---

**Q: Why use EKS with Fargate?**

- **No node management:** No EC2 node groups to provision, patch, or right-size.
- **Per-pod billing:** Pay only for vCPU and memory used by running pods.
- **Automatic isolation:** Each pod gets its own Fargate VM — stronger isolation than shared nodes.
- **Suitable for bursty or event-driven workloads** where predicting node count is difficult.

---

**Q: What is serverless Kubernetes?**

Serverless Kubernetes (via Fargate) means you define pods and their resource requirements, and AWS automatically provisions the underlying compute. You don't see, manage, or pay for EC2 nodes — the unit of billing is the pod, not the node.

---

**Q: How does AWS Fargate work?**

1. You define a **Fargate profile** in EKS specifying which pod namespaces/labels run on Fargate.
2. When a matching pod is scheduled, AWS automatically provisions a **dedicated micro-VM** for that pod.
3. The VM runs exactly the specified vCPU and memory resources.
4. When the pod terminates, the VM is released. No persistent node exists.

---

**Q: What is IRSA?**

IAM Roles for Service Accounts — a mechanism to assign **AWS IAM permissions to specific Kubernetes pods** via service accounts, without using node-level IAM roles or storing credentials in Secrets.

**How it works:**
1. EKS cluster has an **OIDC provider** registered with AWS IAM.
2. A Kubernetes service account is annotated with an IAM role ARN.
3. When a pod using that service account starts, the AWS SDK automatically fetches **temporary credentials** via the projected service account token and OIDC federation.

---

**Q: Why use OIDC?**

OIDC (OpenID Connect) provides a **trust relationship between the EKS cluster and AWS IAM**. IAM can validate tokens issued by the cluster's OIDC provider, enabling fine-grained, workload-level AWS permissions. Without OIDC, all pods on a node share the node's IAM role — less secure.

---

**Q: How does AWS Load Balancer Controller work?**

- Deployed as a Kubernetes controller (Helm chart) in the EKS cluster.
- Watches for Ingress resources and Service type LoadBalancer.
- Provisions and configures **ALBs or NLBs** in AWS automatically.
- Requires IRSA with IAM permissions to manage ELB, EC2, ACM, and WAF resources.
- Annotation-driven: `kubernetes.io/ingress.class: alb` on an Ingress triggers ALB provisioning.

---

**Q: Explain the deployment process step-by-step.**

1. `eksctl create cluster` — EKS cluster created with Fargate profile for the game namespace.
2. IAM OIDC provider associated with the cluster.
3. AWS Load Balancer Controller IAM policy created; IRSA service account configured.
4. AWS Load Balancer Controller deployed via Helm.
5. 2048 game Deployment and Service manifests applied via `kubectl apply`.
6. Ingress resource created → Load Balancer Controller provisions ALB.
7. ALB DNS name resolves to the game application.
8. Fargate schedules each game pod on a dedicated micro-VM.

---

## 13. AWS Generative AI Questions

---

**Q: What is Amazon Bedrock?**

Amazon Bedrock is a **fully managed service** that provides access to foundation models (FMs) from leading AI companies (Anthropic, Meta, Mistral, Amazon) via a single API. No infrastructure to manage — pay per token/request. Supports text, image, and embedding models.

---

**Q: What foundation models are available in Bedrock?**

- **Anthropic Claude** (Claude 3, 3.5, 3.7 series)
- **Meta Llama** (Llama 2, 3 series)
- **Mistral AI** (Mistral, Mixtral)
- **Amazon Titan** (text, image, embedding)
- **Stability AI** (Stable Diffusion for image generation)
- **AI21 Labs** (Jurassic models)

---

**Q: What is Amazon Q Developer?**

Amazon Q Developer is an **AI-powered coding assistant** integrated into IDEs (VS Code, JetBrains) and the AWS console. It provides code suggestions, security vulnerability scanning, code explanations, and unit test generation. For DevOps specifically, it can help with Terraform, CDK, and CloudFormation code generation.

---

**Q: How can Amazon Q help DevOps engineers?**

- **Code generation:** Write Terraform, Bash, Python, or CDK code with natural language prompts.
- **Security scanning:** Detects vulnerabilities in IaC code and suggests fixes.
- **CLI assistance:** `/aws` commands in the IDE explain AWS CLI options.
- **Troubleshooting:** Explains error messages and suggests remediation.
- **Documentation:** Explains unfamiliar AWS services or APIs inline.

---

**Q: What is SageMaker?**

Amazon SageMaker is a **fully managed platform for building, training, and deploying ML models**. Provides Jupyter notebooks, managed training jobs, model registries, real-time endpoints, and MLOps pipelines (SageMaker Pipelines). More hands-on than Bedrock — you bring or train your own models.

---

**Q: Difference between Bedrock and SageMaker.**

| Feature | Amazon Bedrock | Amazon SageMaker |
|---|---|---|
| **Use case** | Use pre-built foundation models via API | Train, fine-tune, and deploy custom ML models |
| **Models** | Pre-built FMs from AWS and partners | Bring your own or use built-in algorithms |
| **Infrastructure** | Fully serverless | Managed but you choose instance types |
| **Target user** | App developers, DevOps, non-ML teams | ML engineers, data scientists |
| **Entry barrier** | Low | Higher (requires ML knowledge) |

---

**Q: Have you used any GenAI services practically?**

My exposure to Amazon Bedrock, Amazon Q Developer, and SageMaker is at a foundational level from AWS training and documentation. I have not deployed production GenAI workloads, but I understand the service boundaries, use cases, and how they integrate with existing AWS infrastructure. I'm actively building practical familiarity as these services become core to modern cloud workflows.

---

**Q: How would you build an AI-powered DevOps assistant?**

A practical approach:
1. **Amazon Bedrock (Claude)** as the LLM backend for natural language understanding.
2. **Tool use / function calling** to connect the LLM to real systems: Jenkins API, Kubernetes (`kubectl`), Prometheus (PromQL), AWS APIs.
3. A **chat interface** (Slack bot or web app) as the front end.
4. Example capabilities: "Show pods in CrashLoopBackOff", "Scale frontend to 5 replicas", "What's the P95 latency for the last hour?"
5. IAM roles for the backend service with scoped permissions to the AWS resources it needs to query.

---

## 14. Behavioral Questions

---

**Q: Tell me about a difficult problem you solved.**

During the internship, we encountered a situation where Terraform apply was failing intermittently due to state lock conflicts — two pipeline runs were triggering simultaneously due to a misconfigured webhook. The issue was non-obvious because the error (DynamoDB lock timeout) looked like an infrastructure issue, not a pipeline configuration issue. I traced it back to duplicate webhook deliveries, added deduplication logic in Jenkins, and introduced a `terraform plan` approval step that serialized applies. No further conflicts after that.

---

**Q: Tell me about a time you made a mistake.**

Early in the internship, I ran `terraform destroy` targeting what I thought was the dev environment's EKS node group, but the workspace wasn't switched correctly — it pointed to staging. Caught it before apply completed (the plan output showed staging resources). Fixed it immediately by switching to the correct workspace. After that, I added a workspace verification step at the beginning of every Terraform pipeline and added a confirmation prompt for destructive operations.

---

**Q: Describe a situation where a deployment failed.**

A backend deployment failed mid-rollout because a new environment variable was added in the application code but wasn't reflected in the Kubernetes ConfigMap. Pods were coming up in `CrashLoopBackOff`. I identified the issue via `kubectl logs` (missing env var error), updated the ConfigMap and redeployed. Post-incident, we added a config validation step in the CI pipeline to compare required env vars in code against the deployed ConfigMap.

---

**Q: Tell me about a conflict within a team.**

There was a disagreement about whether to use Terraform workspaces or separate directories for environment management. I preferred separate directories for clarity; a teammate preferred workspaces for simplicity. We each documented the tradeoffs, brought it to the team, and agreed on separate directories because our environments had meaningfully different configurations. The key was framing it as a technical decision with documented reasoning, not a personal preference.

---

**Q: How do you prioritize tasks?**

- **Severity first:** Production issues override everything.
- **Impact vs. effort:** High-impact, low-effort tasks get done before complex tasks with marginal benefit.
- **Dependencies:** Unblock teammates' work before advancing independent tasks.
- **Communication:** If priorities conflict, I surface the conflict to the team lead early rather than silently choosing.

---

**Q: How do you handle pressure?**

During incidents I focus on: clearly stating what I know and don't know, making decisions with available information rather than waiting for certainty, and communicating status updates to the team at regular intervals. After the incident, I do a brief root cause analysis to close the loop and prevent recurrence.

---

**Q: Describe a time you learned a new technology quickly.**

I learned Argo CD and the GitOps model while building the Go Cloud-Native pipeline project. I had no prior experience with it. I spent a weekend reading the official docs, deployed Argo CD locally on a minikube cluster to understand the sync mechanics, and had a working setup in the project by the following week. The key was building a working example early, even if imperfect, rather than reading until I felt "ready."

---

**Q: Tell me about a time you worked with developers.**

During the internship, a developer reported that their feature branch deployments were failing in the staging pipeline. I worked directly with them to trace the issue — their Dockerfile was referencing a build artifact path that changed in a recent refactor. I walked them through the fix in the Dockerfile, updated the Jenkins pipeline to add a validation step, and documented the pattern for the rest of the team. It reinforced that DevOps works best when the feedback loop with developers is short.

---

**Q: What motivates you?**

Solving infrastructure problems that have direct impact on developer productivity and application reliability. Seeing a deployment pipeline that was flaky become reliable, or a monitoring gap that was causing 3 AM alerts get properly addressed — these are concrete, measurable improvements that motivate me.

---

**Q: What are your strengths?**

- **Systematic debugging:** I follow a structured approach rather than guessing — check logs, events, resource states in order.
- **Documentation:** I maintain operational runbooks so knowledge is not siloed.
- **Cross-tool integration:** Comfortable connecting multiple tools (Jenkins + Terraform + EKS + Prometheus) into cohesive workflows.

---

**Q: What is your biggest weakness?**

I sometimes over-engineer solutions — wanting the "perfect" approach before shipping a working one. I'm improving this by setting explicit timeboxes: if a "good enough" solution is available, ship it and iterate, rather than blocking on the ideal design.

---

**Q: Why are you leaving your internship?**

The internship (Sep 2025 – May 2026) concluded as a fixed-term position. I'm looking for a full-time role where I can apply and deepen what I've built during that time — taking on greater ownership, working at scale, and continuing to grow technically.

---

## 15. Situational Questions

---

**Q: Production deployment failed. What would you do?**

1. **Immediately assess severity** — is the application down, degraded, or just the deployment stuck?
2. **Rollback first if users are impacted:** `kubectl rollout undo deployment/<name>` — restore service, then investigate.
3. Check **Jenkins/GitHub Actions logs** for the failure stage.
4. Check **Kubernetes events and pod logs** for the new pods.
5. Check **Grafana/CloudWatch** for error rate and latency changes.
6. Identify root cause: bad image, broken config, missing secret, resource limits.
7. Fix in a feature branch, test in staging, re-deploy with the fix.
8. Post-incident: document root cause, add pipeline checks to catch it earlier.

---

**Q: Terraform state file gets corrupted. How would you recover?**

1. **Do not run terraform apply** — could create duplicate or conflicting resources.
2. Retrieve the **previous version of the state file** from S3 versioning (`aws s3api list-object-versions`).
3. Restore the previous version: `aws s3 cp s3://bucket/terraform.tfstate terraform.tfstate.backup`.
4. If state is partially corrupted, use `terraform state list` and `terraform state show` to audit surviving resources.
5. For resources no longer in state, use `terraform import` to re-add them.
6. Run `terraform plan` to verify the restored state matches real infrastructure before applying anything.

---

**Q: A Kubernetes pod is in CrashLoopBackOff. What will you check?**

```bash
# 1. Get details and events
kubectl describe pod <pod-name> -n <namespace>

# 2. Check current logs
kubectl logs <pod-name> -n <namespace>

# 3. Check previous container logs (before crash)
kubectl logs <pod-name> --previous -n <namespace>
```

**Common causes and fixes:**
- **Application error at startup:** Fix application code/config.
- **Missing environment variable:** Add to ConfigMap or Secret.
- **Wrong image:** Check image tag, confirm it exists in ECR.
- **OOMKilled:** Increase memory limit in resource spec.
- **Liveness probe failing:** Fix probe endpoint or increase initial delay.
- **Permission error:** Check RBAC or file system permissions.

---

**Q: CPU utilization suddenly spikes. What is your approach?**

1. **Confirm in Grafana/CloudWatch** — which node/pod is spiking?
2. `kubectl top pods -n <namespace>` — identify the specific pod(s).
3. Check if **HPA has triggered** — adding replicas distributes load.
4. Check **application logs** for unusual request patterns or errors.
5. Check **recent deployments** — did a new release correlate with the spike?
6. Check **incoming request volume** on the ALB — is it a traffic spike or a runaway process?
7. If runaway process: rolling restart (`kubectl rollout restart deployment/<name>`).
8. If traffic spike: ensure HPA is configured and Cluster Autoscaler can add nodes.
9. Short-term: manually scale replicas. Long-term: tune HPA thresholds.

---

**Q: Jenkins server is down. What steps would you take?**

1. Check EC2 instance state in AWS console — is it running?
2. SSH into the Jenkins EC2 instance.
3. Check systemd service: `systemctl status jenkins`.
4. Check logs: `journalctl -u jenkins -n 100`.
5. Check disk space (`df -h`) — Jenkins can fill disk with build artifacts.
6. Check memory — Jenkins (JVM) OOM is common.
7. Restart if service crashed: `systemctl restart jenkins`.
8. If EC2 is unreachable: check EC2 system status checks in console, restart if needed.
9. **Immediate mitigation:** Notify team, pause deployments, communicate ETA.
10. **Preventive:** Set up Jenkins on HA configuration or use managed CI (GitHub Actions) as backup.

---

**Q: Database becomes unreachable after deployment. How would you troubleshoot?**

1. Check if the **RDS instance is running** in AWS console.
2. Check the **Security Group** for RDS — does it allow inbound on 5432/3306 from the EKS node security group?
3. Check if the **DB endpoint** in the application ConfigMap/Secret is correct.
4. Test connectivity from a pod: `kubectl exec -it <pod> -- nc -zv <rds-endpoint> 5432`.
5. Check **RDS CloudWatch metrics** — CPU, connections, storage.
6. Check if **max connections** was reached (common when new pods spun up rapidly).
7. Check if a **parameter group or security group change** was made in the same deployment.
8. Check application logs for connection timeout or authentication errors.

---

**Q: Developers cannot access AWS resources. What would you investigate?**

1. **What resource?** S3, EC2, EKS — narrow the scope.
2. Check the developer's **IAM user or role** — what policies are attached?
3. Check **CloudTrail** for recent denied actions (`AccessDenied` events).
4. Verify the **AWS region** — wrong region is a common issue.
5. Check if there's an **SCP (Service Control Policy)** at the organization level blocking access.
6. Check **resource-based policies** (S3 bucket policy, RDS parameter groups).
7. Test with `aws iam simulate-principal-policy` to confirm what permissions are granted.
8. Add the minimum required permissions to the developer's role/policy.

---

**Q: Argo CD is not syncing changes. What would you check?**

1. Check Argo CD UI — application sync status and error message.
2. Check if Argo CD can reach the **Git repository** — credentials or network issue?
3. Check **Argo CD logs**: `kubectl logs -n argocd -l app.kubernetes.io/name=argocd-application-controller`.
4. Run a **manual sync** in the UI — note any error.
5. Verify **Helm chart renders correctly**: `helm template` locally.
6. Check if there's a **YAML validation error** in the manifests.
7. Check Argo CD **RBAC** — does it have permission to apply resources in the target namespace?
8. Check if the application is in **degraded or suspended** state.

---

**Q: Application is slow but pods are healthy. What next?**

This is a deeper performance investigation:
1. Check **P95/P99 latency** in Grafana — when did it start?
2. Check **RDS metrics** — slow queries, connection saturation, high I/O wait.
3. Check **ALB access logs** — is latency added by the load balancer or the application?
4. Run `kubectl top pods` — any pods near their CPU/memory limits (throttling)?
5. Check **JVM garbage collection** (if Java app) or equivalent GC/memory metrics.
6. Use **distributed tracing** (X-Ray/Datadog APM) to identify which service or DB query is slow.
7. Check **network latency** between services if cross-AZ calls are happening.
8. Check if **HPA hasn't scaled** yet because CPU is within limits but latency is high — tune HPA to scale on custom latency metrics.

---

**Q: A security vulnerability is found in a Docker image. What would you do?**

1. **Assess severity** — CVSS score, is it exploitable in our context?
2. **Check Trivy report** for the affected package and fix version.
3. **Immediate:** If critical, roll back the deployment to the previous image.
4. **Fix:** Update the base image or the vulnerable package in the Dockerfile.
5. **Rebuild and rescan** with Trivy — confirm vulnerability is resolved.
6. **Re-deploy** through the normal pipeline.
7. **Process improvement:** Add Trivy to the CI pipeline as a blocking step — fail the build on HIGH/CRITICAL CVEs.
8. **Document** in the security register and notify relevant stakeholders.

---

## 16. HR Questions

---

**Q: Tell me about yourself.**

I'm a DevOps Engineer with internship experience at Hisan Labs, where I worked on AWS, Kubernetes, Terraform, and CI/CD automation. I have a B.Tech from Nagpur University and have built end-to-end projects in GitOps and serverless Kubernetes. I'm looking for a full-time DevOps role where I can contribute to infrastructure automation and grow toward platform engineering.

---

**Q: Why DevOps?**

I find the combination of systems thinking, automation, and direct impact on software delivery compelling. DevOps sits at the intersection of development velocity and operational reliability — improving either one requires understanding both. The breadth of skills required (cloud, networking, security, scripting, containers) keeps the work continuously challenging.

---

**Q: Why our company?**

*(Personalize for each company — research their stack, engineering blog, and culture.)*

Example structure:
- "I read your engineering blog post about how you migrated to Kubernetes — the challenges you described around stateful workloads are exactly what I've been working on."
- "Your commitment to [observability / platform engineering / open source] aligns with where I want to grow."
- "The scale at which your infrastructure operates would push me to go deeper than what's possible in smaller environments."

---

**Q: What are your salary expectations?**

I'm open to discussing this based on the role scope, total compensation package, and growth opportunities. I'd like to understand the full offer before committing to a number. That said, I've researched the market for DevOps Engineer roles in [city] with my experience level and my expectation is in the range of ₹X–₹Y LPA.

*(Research Glassdoor/LinkedIn salary data for the specific city and company tier before interviews.)*

---

**Q: Are you willing to relocate?**

Yes, I'm open to relocation for the right opportunity. *(Or: I'm currently based in Pune and prefer roles in Pune/Bangalore/remote, but I'm open to discussing.)*

---

**Q: What are your career goals?**

- **Short-term (1–2 years):** Deepen expertise in Kubernetes, cloud-native security, and SRE practices. Take ownership of production systems.
- **Medium-term (3–4 years):** Move toward a Senior DevOps or Platform Engineer role, mentoring junior engineers.
- **Long-term:** Build or contribute to internal developer platforms — infrastructure that makes other engineers more productive.

---

**Q: Why should we hire you?**

- Practical experience: I've worked on real production infrastructure — not just lab exercises.
- End-to-end capability: I can own a problem from infrastructure provisioning (Terraform) to deployment (Kubernetes/Argo CD) to monitoring (Prometheus/Grafana).
- Initiative: I built two projects beyond my internship scope, covering GitOps and serverless Kubernetes.
- Learning velocity: I consistently pick up new technologies by building, not just reading.

---

**Q: What makes you different from other candidates?**

- I don't just know the tools — I understand **why** they exist and what problems they solve. I can reason about tradeoffs (e.g., HPA vs. Cluster Autoscaler, count vs. for_each in Terraform).
- I have **project evidence** for the tools on my resume — GitOps pipeline and serverless platform are deployed, not hypothetical.
- I document my work — operational documentation is part of how I work, not an afterthought.

---

**Q: What are your strengths and weaknesses?**

**Strengths:**
- Systematic, evidence-based troubleshooting.
- Clear documentation and communication of technical decisions.
- Ability to connect multiple tools into cohesive, end-to-end workflows.

**Weakness:**
- Tendency to over-engineer — I'm working on shipping working solutions first and iterating, rather than seeking perfection upfront.

---

**Q: Do you have any questions for us?**

Strong questions to ask:
- "What does the on-call rotation look like, and how are incidents handled post-mortems?"
- "What's the current state of your infrastructure — are you early in the cloud-native journey or already heavily Kubernetes-based?"
- "What does the first 90 days look like for someone joining this team?"
- "What are the biggest infrastructure challenges the team is focused on right now?"
- "How does the team approach knowledge sharing and documentation?"

---

*End of Interview Question Bank*

---

> **Tip for all answers:** Always tie abstract answers back to what you did at **Hisan Labs** or in your **projects**. Concrete, specific examples will always outperform theoretically correct but generic answers in any interview.