# 🚀 20 Tricky DevOps Interview Questions Based on Your Resume

## **Terraform Interview Answers (Resume Based)**

---

## 1. Your resume says Terraform reduced provisioning time by 40%. Exactly what steps were automated that reduced this time?

Problem: Infrastructure provisioning for environments such as VPC, subnets, EC2 instances, security groups, and IAM roles was previously done manually through the AWS console, which was time‑consuming and error‑prone.

Tools Used: Terraform, AWS (EC2, VPC, IAM), Git

Implementation:

* Wrote Terraform configuration files to define infrastructure as code.
* Automated creation of VPC, subnets, route tables, internet gateways, and security groups.
* Provisioned EC2 instances automatically using Terraform resources.
* Used variables and reusable modules for environment consistency.
* Stored code in Git for version control and collaboration.

Result: Infrastructure that previously took around 30–40 minutes to create manually could now be provisioned in about 10–15 minutes using Terraform, reducing provisioning time by roughly 40% and improving consistency.

---

## 2. If two engineers run terraform apply simultaneously, what problem may occur?

Identify:
Running Terraform simultaneously can cause state conflicts.

Diagnose:
Terraform relies on a state file to track infrastructure. If two engineers run `terraform apply` at the same time using the same state file, both processes may attempt to modify the infrastructure simultaneously, causing inconsistent or corrupted state.

Fix:
Use remote state with state locking. In AWS this is typically done using an S3 backend with DynamoDB state locking.

Prevent:
Configure backend like this:

* S3 bucket to store the Terraform state file
* DynamoDB table for state locking
  This ensures that only one Terraform operation can run at a time.

---

## 3. What happens if Terraform state file gets corrupted?

Identify:
Terraform may lose track of infrastructure resources.

Diagnose:
Commands like `terraform plan` may show incorrect differences, or Terraform may attempt to recreate resources that already exist.

Fix:
Possible recovery steps:

* Restore the state file from backup
* Use `terraform state` commands to repair entries
* Reimport resources using `terraform import`

Prevent:

* Store state remotely in S3
* Enable S3 versioning
* Use DynamoDB locking
* Maintain regular backups

---

## 4. Difference between Terraform module and Terraform workspace

Terraform Module:
A module is a reusable set of Terraform configuration files that defines infrastructure components. It helps organize code and reuse infrastructure patterns.

Example:
A module may create a VPC with subnets, route tables, and gateways.

Terraform Workspace:
A workspace allows multiple state files for the same configuration. It is commonly used to manage different environments such as dev, staging, and production.

Example:
Workspaces: dev, staging, prod
Each workspace keeps separate infrastructure state.

---

## 5. How do you store Terraform remote state securely in AWS?

Definition:
Terraform remote state allows multiple team members to share infrastructure state securely.

How It Works:

* Store Terraform state in an S3 bucket
* Enable encryption using SSE-S3 or SSE-KMS
* Enable versioning for recovery
* Use a DynamoDB table for state locking
* Restrict access using IAM policies

Example backend configuration:

```
terraform {
  backend "s3" {
    bucket         = "terraform-state-bucket"
    key            = "dev/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-lock"
    encrypt        = true
  }
}
```

This setup ensures secure, versioned, and locked Terraform state management in AWS.

---

## **AWS Architecture Interview Answers**

---

## 1. Your resume mentions 99.9% availability. How did you measure and achieve it in AWS?

Requirements: High availability and minimal downtime for the application.

Architecture:

* Deployed application across multiple Availability Zones.
* Used Application Load Balancer to distribute traffic.
* Implemented Auto Scaling Group to automatically replace unhealthy instances.
* Stored static assets in S3.

Tools: AWS EC2, ALB, Auto Scaling, CloudWatch

Result: CloudWatch metrics and ALB health checks were used to monitor uptime and request success rate. Multi-AZ deployment with Auto Scaling ensured the system maintained around 99.9% availability.

---

## 2. What happens if one Availability Zone fails in your architecture?

Requirements: Maintain service availability during AZ failure.

Architecture:

* EC2 instances deployed in multiple Availability Zones.
* ALB distributes traffic across healthy targets.

Tools: ALB, Auto Scaling Group

Scaling:
If one AZ fails, ALB automatically stops routing traffic to unhealthy instances. Auto Scaling launches new instances in the remaining healthy AZs.

Result: Application remains available with minimal disruption.

---

## 3. Why did you choose ALB instead of NLB?

Definition:
Application Load Balancer operates at Layer 7 (HTTP/HTTPS) while Network Load Balancer operates at Layer 4 (TCP/UDP).

How It Works:
ALB can inspect HTTP headers, URLs, and paths and route requests accordingly.

Example:
In microservices architecture, ALB can route:
/api requests to backend service
/images requests to image service

This capability made ALB better suited for web applications.

---

## 4. Why did you choose ALB instead of Classic Load Balancer?

Definition:
Classic Load Balancer is the older generation AWS load balancer.

How It Works:
ALB provides advanced features like path-based routing, host-based routing, and improved integration with modern architectures.

Example:
ALB supports microservices routing and integrates well with Auto Scaling and container-based environments.

---

## 5. If your EC2 instance suddenly becomes unhealthy in ALB, what happens?

Identify:
ALB health checks detect the instance failure.

Diagnose:
If the instance fails the configured health checks, ALB marks it as unhealthy.

Fix:
ALB stops sending traffic to that instance.
Auto Scaling Group launches a new instance to replace the unhealthy one.

Prevent:
Configure proper health check paths and monitoring with CloudWatch alerts.

---

## 6. How does Auto Scaling know when to launch new instances?

Definition:
Auto Scaling uses scaling policies based on metrics.

How It Works:
CloudWatch monitors metrics such as CPU utilization, memory usage, or request count.

Example:
If CPU utilization exceeds a defined threshold (for example 70%) for a certain period, CloudWatch triggers the Auto Scaling policy to launch additional EC2 instances.

This ensures the system can handle increased traffic automatically.

---

## **Kubernetes Interview Answers**

---

## 1. What happens inside Kubernetes when you run `kubectl apply -f deployment.yaml`?

Definition:
`kubectl apply` is used to create or update Kubernetes resources based on a configuration file.

How it Works:

1. `kubectl` sends the deployment configuration to the Kubernetes API Server.
2. The API Server validates the request and stores the desired state in etcd.
3. The Deployment controller detects the new desired state.
4. The controller creates a ReplicaSet.
5. The ReplicaSet schedules Pods via the Scheduler.
6. The Scheduler assigns Pods to suitable worker nodes based on resources.
7. Kubelet on the selected node pulls the container image and starts the Pod.

Example:
In a microservices deployment, running `kubectl apply` for a Deployment might create 3 replicas of a web service container across multiple worker nodes.

---

## 2. If a Pod goes into CrashLoopBackOff, how do you debug it?

Identify:
Check pod status.

Command:
`kubectl get pods`

Diagnose:
Check logs and pod events.

Commands:
`kubectl logs <pod-name>`
`kubectl describe pod <pod-name>`

Common causes:

* Application crash
* Missing environment variables
* Incorrect configuration
* Image errors

Fix:
Update configuration, environment variables, or application code and redeploy.

Prevent:
Use liveness/readiness probes, proper logging, and CI/CD validation.

---

## 3. What happens if you delete a Kubernetes node?

Requirements:
Maintain application availability.

Architecture:
Pods running on the deleted node are terminated.
The Kubernetes control plane detects that the node is no longer available.

Tools:
Controller Manager and Scheduler

Scaling:
If the pods were managed by a Deployment or ReplicaSet, Kubernetes automatically schedules new pods on other healthy nodes to maintain the desired replica count.

---

## 4. Difference between ClusterIP, NodePort, and LoadBalancer

ClusterIP:
Definition: Default service type that exposes the service internally within the cluster.
Use Case: Internal communication between microservices.

NodePort:
Definition: Exposes the service on a static port on each node.
Use Case: Accessing the service externally through NodeIP:NodePort.

LoadBalancer:
Definition: Provisions an external cloud load balancer.
Use Case: Exposing applications to the internet using cloud provider load balancers.

---

## 5. How does Horizontal Pod Autoscaler decide when to scale pods?

Definition:
HPA automatically scales the number of pods in a deployment based on metrics.

How it Works:

1. Metrics Server collects resource metrics such as CPU and memory usage.
2. HPA compares the current metric values with the defined target thresholds.
3. If the metric exceeds the threshold, HPA increases the number of pod replicas.
4. If usage decreases, HPA reduces the number of replicas.

Example:
If CPU usage exceeds 70%, HPA may scale a deployment from 3 pods to 6 pods to handle increased traffic.

---

## **Jenkins CI/CD Interview Answers**

---

## 1. Your resume says deployment time reduced from 45 min to 10 min. Which pipeline stages were optimized?

Problem:
Manual build and deployment process was slow and inconsistent.

Tools Used:
Jenkins, Git, Docker, Kubernetes

Implementation:

* Automated build stage using Jenkins pipeline triggered by Git commits.
* Implemented parallel stages for build and test where possible.
* Dockerized applications to standardize environments.
* Automated image push to container registry.
* Automated deployment to Kubernetes cluster using kubectl.

Optimized Stages:

1. Build automation
2. Automated testing
3. Container image build
4. Image push to registry
5. Automated deployment

Result:
Automation and parallel execution reduced deployment time from around 45 minutes to about 10 minutes while improving reliability.

---

## 2. What happens if Jenkins master crashes during a pipeline run?

Identify:
Pipeline execution may stop depending on the pipeline type.

Diagnose:
Check Jenkins logs and pipeline status after master recovery.

Fix:
If using Jenkins Pipeline with durability enabled, the pipeline state is stored and can resume after Jenkins restarts.

Prevent:

* Enable pipeline durability settings
* Use Jenkins backup strategy
* Run Jenkins in a highly available setup (for example using Kubernetes or multiple controllers)

---

## 3. Difference between Jenkins Agent and Jenkins Executor

Jenkins Agent:
Definition:
A machine that performs build tasks on behalf of the Jenkins controller.

How it Works:
Agents connect to the Jenkins controller and execute jobs assigned by it.

Example:
A Kubernetes node or VM configured as a Jenkins agent to run build jobs.

Jenkins Executor:
Definition:
An executor is a slot on an agent that runs a build job.

How it Works:
Each executor can run one job at a time.

Example:
If an agent has 4 executors, it can run 4 pipeline jobs simultaneously.

---

## **Docker Interview Answers**

---

## 1. What happens internally when you run `docker run nginx`?

Definition:
The `docker run` command creates and starts a container from a Docker image.

How it Works:

1. Docker checks if the nginx image exists locally.
2. If not found, Docker pulls the image from Docker Hub.
3. Docker creates a writable container layer on top of the image layers.
4. It allocates namespaces and cgroups for process and resource isolation.
5. Docker sets up networking for the container.
6. The container runtime starts the nginx process inside the container.

Example:
Running `docker run nginx` launches an nginx web server inside an isolated container using the nginx image.

---

## 2. If your Docker container works locally but fails in Kubernetes, what could be the reasons?

Identify:
The application runs correctly in Docker locally but fails when deployed as a Kubernetes Pod.

Diagnose:
Check pod status, logs, and events.

Commands:
`kubectl get pods`
`kubectl logs <pod-name>`
`kubectl describe pod <pod-name>`

Common Reasons:

* Missing environment variables in Kubernetes deployment
* Incorrect container port configuration
* Image pull issues (private registry authentication)
* Resource limits causing container restarts
* Incorrect service configuration

Fix:
Update deployment YAML configuration, environment variables, resource limits, or container port settings.

Prevent:
Validate container configurations in CI/CD pipelines and test deployments in staging environments before production.

---

## **⭐ Bonus Trick Interview Question (Very Common)**

---

## Explain your entire project architecture from Git commit → Production deployment.

### Requirements

Goal was to build an automated CI/CD pipeline that takes code changes from developers and deploys them reliably to production with minimal manual intervention.

### Architecture Overview

1. Developers push code to GitHub.
2. GitHub webhook triggers Jenkins pipeline.
3. Jenkins performs build and test stages.
4. Jenkins builds Docker image.
5. Docker image is pushed to container registry.
6. Kubernetes deployment is updated with new image.
7. Kubernetes performs rolling update to production pods.
8. Application is exposed through AWS Application Load Balancer.

### Tools Used

* GitHub (Source Code Management)
* Jenkins (CI/CD pipeline)
* Docker (Containerization)
* AWS ECR or Docker Hub (Container registry)
* Kubernetes / EKS (Container orchestration)
* AWS EC2 nodes (worker nodes)
* ALB (traffic distribution)

### Detailed Flow

#### Step 1 – Code Commit

Developer pushes code changes to the GitHub repository.

#### Step 2 – Pipeline Trigger

A GitHub webhook automatically triggers a Jenkins pipeline job.

#### Step 3 – Build Stage

Jenkins pulls the latest code and builds the application.

#### Step 4 – Test Stage

Automated unit tests run to validate the build.

#### Step 5 – Docker Image Build

Jenkins builds a Docker image using the Dockerfile.

#### Step 6 – Push Image

The Docker image is pushed to a container registry such as Docker Hub or AWS ECR.

#### Step 7 – Deployment Update

Jenkins updates the Kubernetes deployment YAML with the new image tag and applies it using kubectl.

#### Step 8 – Rolling Update

Kubernetes creates new pods with the updated image while gradually terminating old pods to ensure zero downtime.

#### Step 9 – Traffic Routing

AWS Application Load Balancer routes incoming traffic to healthy Kubernetes pods.

### Result

This architecture enables fully automated deployments, faster release cycles, and high availability of the application in production.

---

## **DevOps Architecture Follow-up Interview Answers**

---

## 1. What happens if Jenkins fails during deployment?

Identify:
Deployment pipeline stops in the middle of execution.

Diagnose:
Check Jenkins job logs and pipeline stage where the failure occurred.

Fix:

* Restart Jenkins service.
* Re-run the failed pipeline stage.
* Since the application is deployed using Kubernetes rolling updates, partially deployed pods will continue running while new deployment resumes.

Prevent:

* Run Jenkins in a highly available setup.
* Use Jenkins backup and persistent storage.
* Use pipeline checkpoints or stage restart features.

---

## 2. How would you implement rollback in this pipeline?

Problem:
A newly deployed application version contains bugs and needs to revert to the previous stable version.

Tools:
Kubernetes, Jenkins

Implementation:

* Kubernetes keeps deployment revision history.
* If a deployment fails, use rollback command:

kubectl rollout undo deployment <deployment-name>

* Jenkins pipeline can include a rollback stage triggered manually or automatically if health checks fail.

Result:
Application quickly returns to the last stable version with minimal downtime.

---

## 3. How do you ensure zero-downtime deployment?

Requirements:
Application must stay available during updates.

Architecture:
Use Kubernetes rolling update strategy.

Tools:
Kubernetes Deployment

Scaling:
During update:

* New pods are created first.
* Health checks ensure new pods are ready.
* Old pods are terminated gradually.

Result:
Users experience no service interruption.

---

## 4. How do you secure secrets in Kubernetes?

Definition:
Sensitive data like passwords, API keys, and tokens must not be stored in plain text.

How it Works:
Kubernetes Secrets store sensitive information securely and can be mounted into pods as environment variables or files.

Example:
Create secret:

kubectl create secret generic db-secret --from-literal=password=MyPassword

Pods reference this secret to access credentials securely.

Additional security:

* Enable etcd encryption
* Restrict access using RBAC
* Use external secret managers like AWS Secrets Manager or HashiCorp Vault

---

## 5. How would you scale this architecture to 1 million users?

Requirements:
System must handle very high traffic with high availability.

Architecture:

* Use multi-AZ Kubernetes cluster
* Use AWS Application Load Balancer
* Use Auto Scaling for worker nodes

Tools:
Kubernetes HPA, AWS Auto Scaling, CDN

Scaling:

* Horizontal Pod Autoscaler scales pods based on CPU or request metrics.
* Cluster Autoscaler adds more nodes when cluster resources are insufficient.
* Use caching and CDN to reduce backend load.

Result:
System can handle large traffic spikes while maintaining performance and reliability.

---

## **Advanced DevOps Architecture Interview Follow-ups**

---

## 1. What happens if etcd fails in Kubernetes?

Identify:
etcd is the key-value store that holds the entire cluster state (pods, deployments, services, configs).

Impact:
If etcd fails completely:

* Kubernetes control plane cannot read or write cluster state
* New deployments or scaling operations stop working
* Existing running pods may continue running, but cluster management becomes unavailable

Fix:

* Restore etcd from backup
* Replace failed etcd node in cluster
* Restart control plane components

Prevent:

* Run etcd cluster with multiple nodes
* Enable regular etcd snapshots
* Store backups in external storage

---

## 2. How does Kubernetes scheduler decide node placement?

Definition:
Kubernetes Scheduler assigns pods to worker nodes based on resource availability and constraints.

How it Works:

1. Scheduler watches for unscheduled pods
2. Filters nodes that do not meet requirements
3. Scores remaining nodes based on policies
4. Selects best node

Factors considered:

* CPU and memory availability
* Node labels and selectors
* Taints and tolerations
* Pod affinity and anti-affinity
* Resource limits

Example:
If a pod requires 2 CPU and node has only 1 CPU available, scheduler will reject that node.

---

## 3. How do you debug a failing microservice in production?

Identify:
Service becomes slow, returns errors, or crashes.

Diagnose:

1. Check application logs
2. Check container logs
3. Inspect Kubernetes pod events
4. Monitor metrics (CPU, memory)
5. Check upstream/downstream dependencies

Commands:
kubectl logs
kubectl describe pod
kubectl get events

Fix:

* Restart failed pods
* Fix configuration issues
* Rollback deployment if new version caused issue

Prevent:

* Implement monitoring (Prometheus, Grafana)
* Use centralized logging
* Add health checks

---

## 4. How would you migrate this architecture to multi-region?

Requirements:
High availability and disaster recovery across regions.

Architecture:

* Deploy Kubernetes clusters in multiple AWS regions
* Use global DNS routing

Tools:
AWS Route 53, EKS clusters, Global load balancing

Scaling:

* Route 53 latency-based routing directs users to nearest region
* Data replication across regions

Result:
If one region fails, traffic automatically shifts to another region.

---

## 5. How would you reduce deployment risk in production?

Problem:
New deployments may introduce bugs that affect users.

Tools:
Kubernetes deployment strategies

Implementation:

* Use rolling updates
* Implement canary deployments
* Use blue-green deployment strategy
* Run automated tests before production deployment
* Monitor application health during deployment

Result:
These strategies allow safe deployments and quick rollback if issues occur.

---

## **Very Tricky DevOps Resume Cross Questions – Interview Answers**

---

## 1. Terraform – What happens if `terraform destroy` fails halfway?

Identify:
Some infrastructure resources are deleted while others remain.

Diagnose:
Terraform state file may become inconsistent with actual infrastructure.

Fix:

* Run `terraform plan` to identify remaining resources.
* Manually delete remaining resources or run `terraform destroy` again.
* Use `terraform state rm` or `terraform import` if state needs correction.

Prevent:

* Use remote state with locking (S3 + DynamoDB).
* Review dependency order before destroy.
* Use targeted destroy carefully.

---

## 2. AWS – How would you design zero‑downtime deployment?

Requirements:
Users must not experience service interruption during deployment.

Architecture:

* Use Application Load Balancer.
* Deploy application in multiple Availability Zones.
* Use Auto Scaling Group.

Tools:
ALB, EC2, Auto Scaling, CI/CD pipeline

Implementation:

* Deploy new application version to new instances.
* Perform health checks.
* Gradually shift traffic from old instances to new ones.

Result:
Users continue accessing service without downtime.

---

## 3. Kubernetes – What happens if etcd goes down?

Identify:
etcd stores the cluster's desired state.

Impact:

* Control plane cannot read or update cluster configuration.
* New pod scheduling or deployments stop working.

Behavior:
Existing running pods may continue running temporarily because kubelets manage them locally.

Fix:

* Restore etcd from snapshot.
* Replace failed etcd node.

Prevent:

* Run etcd as a highly available cluster.
* Schedule regular backups.

---

## 4. Docker – Difference between Docker container vs VM at kernel level

Docker Container:

* Shares host OS kernel.
* Uses namespaces and cgroups for isolation.
* Lightweight and starts quickly.

Virtual Machine:

* Each VM runs its own full OS and kernel.
* Uses hypervisor for virtualization.
* Heavier and slower to start.

Example:
Docker containers are ideal for microservices, while VMs are used when full OS isolation is required.

---

## 5. CI/CD – How do you implement rollback in Jenkins pipeline?

Problem:
Deployment fails after a new release.

Tools:
Jenkins, Kubernetes

Implementation:

* Jenkins pipeline deploys new image version.
* Health checks verify deployment.
* If failure detected, pipeline triggers rollback command.

Example:
`kubectl rollout undo deployment app-deployment`

Result:
Application reverts to last stable version quickly, minimizing production impact.

---