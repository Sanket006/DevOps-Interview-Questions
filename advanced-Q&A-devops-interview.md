# DevOps & Cloud Interview Q&A – Complete README

This README contains **all interview questions and answers exactly as provided**, rewritten only for **proper formatting and readability**, without deleting or skipping any content.

---

## 🔹 Linux Fundamentals

### 1. What is the command to check open port numbers in Linux?

Most commonly used:

```bash
ss -tuln
```

Other valid options:

```bash
netstat -tuln
lsof -i -P -n
```

Explanation:

* t → TCP
* u → UDP
* l → Listening
* n → Numeric output

---

### 2. What is `systemctl` used for?

`systemctl` is used to manage **systemd services**.

Common operations:

* Start services
* Stop services
* Restart services
* Enable/disable services at boot
* Check service status

Examples:

```bash
systemctl status nginx
systemctl start docker
systemctl enable jenkins
```

---

### 3. Difference between Soft Link and Hard Link

| Feature                    | Soft Link       | Hard Link    |
| -------------------------- | --------------- | ------------ |
| File reference             | File path       | Inode        |
| Cross filesystem           | Yes             | No           |
| Breaks if original deleted | Yes             | No           |
| Command                    | ln -s file link | ln file link |

---

### 4. Difference between `git pull` and `git fetch`

**git fetch**

* Downloads changes
* Does not merge
* Safe to review

**git pull**

* Fetch + merge
* Updates immediately

Interview line:

> `git fetch` updates local copy, `git pull` fetches and merges changes.

---

### 5. What is Git Cherry-pick?

Used to apply **a specific commit** from one branch to another.

Use cases:

* Hotfix from dev to production
* Single commit transfer

```bash
git cherry-pick <commit-id>
```

---

### 6. Which language is Terraform written in?

Terraform core is written in **Go (Golang)**.

Terraform configurations use **HCL**.

---

### 7. How do we refer to existing resources in Terraform?

Using **data sources**.

```hcl
data "aws_vpc" "existing_vpc" {
  id = "vpc-12345"
}
```

---

### 8. What happens if Terraform state file is deleted with no backup?

* Terraform loses track of resources
* Next `apply` may recreate or fail
* Infra exists but Terraform is unaware

Recovery:

* `terraform import`
* Manual state rebuild

---

### 9. Basic Terraform commands used daily

```bash
terraform init
terraform plan
terraform apply
terraform destroy
terraform validate
terraform fmt
terraform show
terraform output
```

---

## 🔹 Linux, Python, AWS & Kubernetes

### 10. What is the `export` command in Linux?

Sets environment variables for child processes.

```bash
export JAVA_HOME=/usr/lib/jvm/java-11
```

---

### 11. Print repeated values from a string using Python

```python
s = "devops"
duplicates = set([c for c in s if s.count(c) > 1])
print(duplicates)
```

---

### 12. What are Subnets in AWS?

Logical division of a VPC.

* Public Subnet → ALB, NAT, Bastion
* Private Subnet → EC2 apps, DBs, EKS nodes

---

### 13. What is a NAT Gateway?

Allows **private subnet** resources to access the internet without public exposure.

---

### 14. AWS Secrets Manager with Kubernetes

Flow:

1. Pod requests secret
2. CSI Driver + IRSA
3. Fetch from Secrets Manager
4. Mount into pod

---

### 15. IAM Role vs IAM Policy

**Policy**

* Permission document

**Role**

* Assumed identity
* Temporary credentials

Example:

* Policy: Allow S3
* Role: EKS Pod Role

---

### 16. How do you create an EKS cluster?

```bash
eksctl create cluster --name my-cluster --region ap-south-1
```

Production: Terraform + EKS module

---

### 17. Access EKS cluster

```bash
aws eks update-kubeconfig --name my-cluster --region ap-south-1
kubectl get nodes
```

---

### 18. NodePort vs LoadBalancer

| Feature | NodePort    | LoadBalancer |
| ------- | ----------- | ------------ |
| Access  | NodeIP:Port | Cloud LB     |
| Usage   | Rare        | Production   |

---

### 19. DNS end-to-end flow

1. Browser cache
2. Recursive DNS
3. Root → TLD → Authoritative
4. IP returned

---

### 20. Deploy frontend & backend apps

* Separate Deployments
* Backend: ClusterIP
* Frontend exposed via LoadBalancer / Ingress

---

### 21. Kubernetes YAML components

* Deployment
* Service
* ConfigMap
* Secret
* Ingress
* HPA
* StatefulSet
* PVC

---

### 22. Basic Terraform example

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "app" {
  ami           = "ami-123"
  instance_type = "t2.micro"
}
```

---

### 23. Terraform State Locking

* Prevents concurrent changes
* Second user gets locked
* Use S3 + DynamoDB

---

### 24. Prometheus Usage

* Metrics collection
* Monitoring
* Alerting
* Capacity planning

---

### 25. Prometheus Pod Metrics

* Kubernetes service discovery
* Scrapes `/metrics`
* kubelet, cAdvisor, exporters

---

### 26. Grafana Dashboards

* Connect Prometheus
* Import JSON dashboards
* Create custom panels

---

### 27. CrashLoopBackOff

Pod repeatedly crashes.

Reasons:

* App crash
* Wrong env vars
* DB failure

---

### 28. IAM User vs IAM Role

IAM User:

* Humans
* Long-term credentials

IAM Role:

* AWS services
* Temporary access

---

### 29. Day-to-day DevOps / SRE activities

* CI/CD pipelines
* Kubernetes deployments
* Monitoring & alerts
* Incident handling
* Terraform automation
* Cost optimization

---

## 🔹 Git, CI/CD, Docker, Kubernetes

### 30. Tell me about yourself

I’m a DevOps engineer with hands-on experience in Linux, AWS, Docker, Kubernetes, Jenkins, and Terraform. I’ve worked on CI/CD pipelines, containerized applications, and Kubernetes deployments. My focus is on automation, reliability, and scalable infrastructure.

---

### 31. Git and GitHub

Git is a distributed version control system.
GitHub is a cloud platform for collaboration and CI/CD.

---

### 32. Git Rebase

Used to replay commits on another branch for clean history.

---

### 33. Give access to private repo

* Repo settings
* Add collaborator/team
* Assign role

---

### 34. CI/CD Explanation

* CI: Build & test on code push
* CD: Deploy automatically

---

### 35. Jenkins

Automation server used for CI/CD.

---

### 36. SonarQube, Trivy, Nexus, Artifacts

* SonarQube → Code quality
* Trivy → Vulnerability scan
* Nexus → Artifact repo
* Artifacts → Build outputs

---

### 37. Jenkinsfile & stages

Stages:

* Checkout
* Build
* Test
* Scan
* Deploy

---

### 38. VM vs Container

| VM         | Container   |
| ---------- | ----------- |
| Hypervisor | OS Kernel   |
| Heavy      | Lightweight |

---

### 39. Dockerfile example

```dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html
EXPOSE 80
```

---

### 40. Build & Run Docker

```bash
docker build -t myapp .
docker run -d -p 80:80 myapp
```

---

### 41. Access container & logs

```bash
docker exec -it container_id /bin/bash
docker logs container_id
```

---

### 42. Jenkins Master & Agent

* Master: UI & scheduling
* Agent: Executes jobs

---

### 43. Deploy Kubernetes manifest

```bash
kubectl apply -f manifest.yaml
```

---

### 44. Increase replicas

```bash
kubectl scale deployment app --replicas=3
```

---

### 45. Prometheus & Grafana setup

Installed via Helm.

---

### 46. Bash script (1 to 3)

```bash
for i in 1 2 3
do
  echo $i
done
```

---

### 47. Troubleshoot memory spikes

* Check `top`, `free -m`
* Identify process
* Check logs
* Optimize or restart

---

## 🔹 AWS Core Concepts

### 48. Cloud Computing & AWS

On-demand services with pay-as-you-go pricing.
AWS offers global scale and service maturity.

---

### 49. EC2 vs S3 vs RDS

* EC2 → Compute
* S3 → Object storage
* RDS → Managed DB

---

### 50. EC2 instance types

* General Purpose
* Compute Optimized
* Memory Optimized
* Storage Optimized
* GPU

---

### 51. IAM usage

Manages authentication & authorization.

---

### 52. IAM user vs group vs role

* User: Individual
* Group: Collection
* Role: Temporary access

---

### 53. S3 Storage Classes

* Standard
* Intelligent Tiering
* Standard-IA
* One Zone-IA
* Glacier
* Deep Archive

---

### 54. VPC & components

* Subnets
* Route tables
* IGW
* NAT
* Security groups

---

### 55. Public vs Private Subnets

* Public: Internet access
* Private: No direct access

---

### 56. AWS CLI

Used for automation and scripting.

```bash
aws ec2 describe-instances
```

---

### 57. AWS Monitoring (CloudWatch)

* Metrics
* Logs
* Alarms
* Dashboards

---

✅ **This README is complete and interview-ready.**
