# DevOps Interview Questions – Structured Answers

Below are professionally structured answers (fresher to mid-level DevOps Engineer perspective).

---

## 1. Tell me about yourself & your day-to-day activities

**Introduction:**
I am a DevOps Engineer with hands-on experience in AWS, Linux, Docker, Kubernetes, Terraform, Git, and CI/CD tools like Jenkins/GitHub Actions. I focus on automating infrastructure, improving deployment reliability, and maintaining scalable cloud environments.

**Day-to-day activities:**

* Managing CI/CD pipelines
* Writing and maintaining Terraform scripts
* Monitoring applications and infrastructure
* Troubleshooting production issues
* Managing Docker images and Kubernetes deployments
* Implementing security best practices (IAM, RBAC)
* Coordinating with developers for release management

---

## 2. Which cloud are you using & which AWS services?

**Cloud:** AWS

**Services Used:**

* EC2 (compute)
* S3 (storage)
* IAM (access control)
* VPC (networking)
* RDS (database)
* ECR (container registry)
* EKS (Kubernetes)
* CloudWatch (monitoring)
* Auto Scaling & Load Balancer

---

## 3. Difference between Public Subnet vs Private Subnet

| Feature         | Public Subnet              | Private Subnet       |
| --------------- | -------------------------- | -------------------- |
| Internet Access | Yes (via Internet Gateway) | No direct internet   |
| Route Table     | IGW attached               | NAT Gateway required |
| Usage           | Web servers, Bastion host  | DB, backend servers  |

---

## 4. What is a Namespace in Kubernetes?

A Namespace is a logical separation within a Kubernetes cluster used to organize resources and isolate workloads (e.g., dev, staging, prod).

---

## 5. Role of state file in Terraform

Terraform state file (.tfstate) keeps track of infrastructure resources created by Terraform and maps them to configuration files.

It helps:

* Track resource changes
* Prevent duplication
* Enable collaboration

---

## 6. Most useful Terraform commands

* terraform init
* terraform plan
* terraform apply
* terraform destroy
* terraform validate
* terraform fmt
* terraform state list

---

## 7. Have you worked on Terraform? How do you manage state file?

Yes.

State management best practice:

* Store state remotely in S3
* Enable DynamoDB for state locking
* Use versioning in S3 bucket

Architecture:

* Modular structure
* Separate files for VPC, EC2, EKS
* Variables and backend configuration

---

## 8. Same Terraform file – two people working

Use:

* Remote backend (S3)
* DynamoDB locking

Terraform will lock state during apply, preventing conflicts.

---

## 9. Changes made in AWS Console, Terraform different

Steps:

1. Run terraform refresh
2. Run terraform plan
3. Import resource if needed using terraform import
4. Update Terraform code to match actual infrastructure

---

## 10. Application technology stack

Example stack:

* Frontend: React
* Backend: Java Spring Boot
* Database: MySQL
* Containerization: Docker
* Orchestration: Kubernetes
* CI/CD: Jenkins/GitHub Actions
* Cloud: AWS

---

## 11. Which CI/CD tool are you using?

Jenkins / GitHub Actions

Pipeline stages:

* Code checkout
* Build
* Test
* Docker build
* Push to ECR
* Deploy to EKS/EC2

---

## 12. Step-by-step deploy Java app on EC2

1. Launch EC2
2. Install Java
3. Install Tomcat (if WAR)
4. Configure security group
5. Transfer artifact (scp or pipeline)
6. Start service
7. Configure Nginx (optional)
8. Enable monitoring

---

## 13. How do you connect to EC2?

Using SSH:
ssh -i key.pem ec2-user@public-ip

---

## 14. In Terraform, how do you connect to ECR?

Using AWS provider configuration:

* Define aws provider
* Create aws_ecr_repository resource
* Authenticate using IAM role

---

## 15. What is RBAC in Kubernetes?

RBAC (Role-Based Access Control) restricts access based on roles.

Components:

* Role
* ClusterRole
* RoleBinding
* ClusterRoleBinding

---

## 16. How do you monitor logs for pods in EKS?

* kubectl logs
* CloudWatch logs integration
* EFK stack (Elasticsearch, Fluentd, Kibana)

---

## 17. Are you using volume mounts?

Yes.

Used for:

* Persistent storage
* ConfigMaps
* Secrets

---

## 18. What is PVC?

Persistent Volume Claim (PVC) is a request for storage by a user in Kubernetes.

---

## 19. What is ADD in Dockerfile? CMD vs ENTRYPOINT?

ADD:

* Copies files
* Can extract tar files
* Can fetch remote URLs

CMD:

* Default command
* Can be overridden

ENTRYPOINT:

* Main command
* Hard to override

---

## 20. Ubuntu – kill running process

1. ps -ef | grep process
2. kill <PID>
3. kill -9 <PID>

---

END
