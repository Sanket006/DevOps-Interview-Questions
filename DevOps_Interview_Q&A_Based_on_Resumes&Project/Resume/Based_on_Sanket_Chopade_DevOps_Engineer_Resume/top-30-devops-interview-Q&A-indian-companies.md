# 🚀 Top 30 DevOps Interview Questions with Structured Answers (Indian Companies)  

## **DevOps Fundamentals (1–5)**

### 1. What is DevOps and why is it important in modern software development?

Definition:
DevOps is a culture and set of practices that combine development (Dev) and operations (Ops) to improve collaboration, automate workflows, and deliver software faster and more reliably.

How it Works:
DevOps integrates development, testing, deployment, and operations into a continuous cycle using automation, CI/CD pipelines, monitoring, and infrastructure automation.

Example:
In a DevOps workflow, developers push code to GitHub. A CI/CD pipeline automatically builds, tests, and deploys the application to AWS using Docker and Kubernetes.

---

### 2. Explain the DevOps lifecycle.

Definition:
The DevOps lifecycle is a continuous process that integrates development and operations to deliver software efficiently.

Stages:
Plan → Code → Build → Test → Release → Deploy → Operate → Monitor

Explanation:

* Plan: Define features and requirements.
* Code: Developers write application code.
* Build: Code is compiled and packaged.
* Test: Automated testing ensures quality.
* Release: Application is prepared for deployment.
* Deploy: Application is deployed to production.
* Operate: System runs in production.
* Monitor: Logs and metrics are monitored for performance and reliability.

Example:
A team uses Git for code, Jenkins for CI/CD, Docker for containers, Kubernetes for orchestration, and Prometheus + Grafana for monitoring.

---

### 3. Difference between Continuous Integration, Continuous Delivery, and Continuous Deployment

Continuous Integration (CI):
Definition: Developers frequently merge code into a shared repository where automated builds and tests run.
Example: Every Git push triggers a Jenkins pipeline that runs tests.

Continuous Delivery (CD):
Definition: Code is automatically built, tested, and prepared for release but requires manual approval to deploy to production.
Example: After CI pipeline success, deployment to production requires approval.

Continuous Deployment:
Definition: Every change that passes automated tests is automatically deployed to production without manual intervention.
Example: After CI pipeline success, the application is automatically deployed to Kubernetes.

---

### 4. What is Infrastructure as Code (IaC) and why is it useful?

Definition:
Infrastructure as Code is the practice of managing and provisioning infrastructure using code instead of manual configuration.

How it Works:
Tools like Terraform, AWS CloudFormation, and Ansible allow engineers to define infrastructure in configuration files which can be version controlled and automatically deployed.

Example:
Instead of manually creating EC2 instances, a Terraform script can automatically provision EC2, VPC, subnets, and security groups.

Benefits:

* Automation
* Consistency
* Version control
* Faster infrastructure provisioning

---

### 5. What are the main challenges DevOps solves?

1. Slow software delivery
   Traditional development and operations teams work in silos, causing delays in releases.

2. Environment inconsistencies
   Applications may work in development but fail in production due to configuration differences.

3. Manual processes
   Manual deployments increase the risk of errors and downtime.

4. Lack of collaboration
   DevOps improves communication between developers and operations teams.

5. Scalability and reliability
   Automation and cloud infrastructure help applications scale efficiently.

Example:
Using CI/CD pipelines and Docker containers ensures consistent deployments across development, staging, and production environments.

---

## **Linux (6–10)**

### 6. Difference between Hard Link and Soft Link

Definition:
In Linux, links are used to reference files in the filesystem.

Hard Link:

* A hard link is another reference to the same inode (actual file data).
* Both files share the same inode number.
* If the original file is deleted, the hard link still works.
* Cannot link directories and cannot span across different filesystems.

Soft Link (Symbolic Link):

* A soft link is a shortcut that points to the original file path.
* It has a different inode.
* If the original file is deleted, the soft link becomes broken.
* Can link directories and work across filesystems.

Example:
ln file1.txt hardlink.txt
ln -s file1.txt softlink.txt

---

### 7. How do you check CPU, Memory, and Disk usage in Linux?

CPU Usage:
Command:
top
or
htop

Example:
top
Shows real-time CPU usage and running processes.

Memory Usage:
Command:
free -h

Example:
free -h
Displays total, used, and available memory in human-readable format.

Disk Usage:
Command:
df -h

Example:
df -h
Shows disk usage of mounted filesystems.

---

### 8. Difference between top, ps, and htop

Definition:
These commands are used to monitor processes in Linux.

Top:

* Real-time process monitoring
* Shows CPU and memory usage
* Default tool available on most Linux systems

ps:

* Displays snapshot of running processes
* Not real-time

Example:
ps -ef
Lists all running processes.

htop:

* Interactive process viewer
* Easier to read than top
* Allows sorting, searching, and killing processes

Example:
htop

---

### 9. How do you schedule tasks using cron jobs?

Definition:
Cron is a time-based job scheduler in Linux used to automate repetitive tasks.

How it Works:
Cron jobs are defined in the crontab file and executed at specified times.

Command:
crontab -e

Example:
0 2 * * * /home/user/backup.sh

Explanation:
This runs the backup script every day at 2 AM.

Cron Format:
Minute Hour Day Month Weekday Command

Example:
*/5 * * * * script.sh
Runs every 5 minutes.

---

### 10. How do you find the largest files in Linux?

Method 1:
du -sh * | sort -rh | head

Explanation:

* du -sh * shows file sizes
* sort -rh sorts by size (largest first)
* head shows the top results

Method 2:
find / -type f -exec du -h {} + | sort -rh | head

Example:
Used to identify large files that may be consuming disk space on a server.

---

## **Git & Version Control (11–14)**

### 11. What is Git and why is it used?

Definition:
Git is a distributed version control system used to track changes in source code and enable collaboration among multiple developers.

How it Works:
Git stores snapshots of files in repositories. Developers can create branches, make changes independently, and later merge them into the main branch.

Why it is used:

* Tracks code changes
* Enables collaboration
* Maintains version history
* Allows rollback to previous versions

Example:
In a DevOps workflow, developers push code to a Git repository like GitHub. A CI/CD pipeline (Jenkins/GitHub Actions) automatically builds and tests the code after every push.

---

### 12. Difference between git fetch and git pull

Definition:
Both commands are used to update the local repository with changes from a remote repository.

Git Fetch:

* Downloads the latest changes from the remote repository
* Does NOT merge changes into the current branch
* Safe way to review changes before merging

Example:
Git fetch origin

Git Pull:

* Downloads changes AND automatically merges them into the current branch
* Equivalent to: git fetch + git merge

Example:
Git pull origin main

Interview Tip:
Developers often use git fetch first to review changes before merging.

---

### 13. What is a merge conflict and how do you resolve it?

Definition:
A merge conflict occurs when two branches modify the same part of a file and Git cannot automatically decide which change to keep.

How it Happens:
Two developers edit the same lines of code in different branches and try to merge them.

Resolution Steps:

1. Git shows the conflicting files
2. Open the file and review conflict markers
3. Choose the correct code or combine both changes
4. Remove conflict markers
5. Add and commit the resolved file

Example Commands:
Git status
Git add filename
Git commit

Example Scenario:
Developer A modifies a function in main branch while Developer B modifies the same function in feature branch, causing a merge conflict during merge.

---

### 14. What is Git branching strategy (GitFlow)?

Definition:
GitFlow is a branching strategy that organizes how developers work with branches in a Git repository.

Main Branches:
Main / Master

* Contains production-ready code

Develop

* Integration branch where features are combined

Supporting Branches:
Feature Branch

* Used to develop new features

Release Branch

* Used to prepare code for production release

Hotfix Branch

* Used to quickly fix issues in production

Example Workflow:

1. Create feature branch from develop
2. Develop feature
3. Merge feature branch back to develop
4. Create release branch
5. Deploy to production

Benefit:
GitFlow helps teams manage parallel development and maintain stable production code.

---

## **Docker & Containers (15–19)**

### 15. What is Docker?

Definition:
Docker is a containerization platform that allows developers to package applications and their dependencies into lightweight containers so they can run consistently across different environments.

How it Works:
Docker uses container technology based on Linux kernel features such as namespaces and cgroups to isolate applications. Containers share the host OS but run in isolated environments, making them lightweight and fast compared to virtual machines.

Example:
A developer builds a Docker image for a Node.js application. The same image is deployed in development, testing, and production environments, ensuring the application behaves the same everywhere.

---

### 16. Difference between Docker Image and Docker Container

Definition:
Docker images and containers are core components of Docker.

Docker Image:

* A read-only template used to create containers
* Contains application code, libraries, dependencies, and environment configuration
* Built using a Dockerfile

Example Command:
docker build -t myapp:1.0 .

Docker Container:

* A running instance of a Docker image
* It is isolated and executes the application

Example Command:
docker run -d -p 80:80 myapp:1.0

Interview Summary:
Image = Blueprint
Container = Running instance

---

### 17. What is a Dockerfile?

Definition:
A Dockerfile is a text file containing instructions used to build a Docker image.

How it Works:
Docker reads the Dockerfile and executes instructions step by step to create image layers.

Common Instructions:
FROM
COPY
RUN
WORKDIR
EXPOSE
CMD

Example:
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm","start"]

This builds a Docker image for a Node.js application.

---

### 18. What is a Docker volume?

Definition:
A Docker volume is a persistent storage mechanism used to store data outside the container filesystem.

Why it is Needed:
Container data is lost when containers are deleted. Volumes allow data to persist independently of containers.

Example Use Case:
Database containers like MySQL or PostgreSQL store their data in volumes so data remains even if the container restarts.

Example Commands:
docker volume create myvolume

docker run -v myvolume:/data mysql

---

### 19. How do you reduce Docker image size?

Methods:

1. Use smaller base images
   Example: Use alpine images instead of full OS images
   FROM node:18-alpine

2. Use multi-stage builds
   Build dependencies in one stage and copy only necessary files to the final image.

3. Remove unnecessary files
   Delete temporary files and caches during build.

Example:
RUN apt-get clean

4. Combine RUN commands
   Reduce number of image layers.

5. Use .dockerignore
   Exclude unnecessary files like logs, node_modules, or build artifacts.

Example:
node_modules
.git
logs

Result:
Smaller images reduce storage usage and speed up deployments in CI/CD pipelines.

---

## **Kubernetes (20–25)**

### 20. What is Kubernetes and why is it used?

Definition:
Kubernetes is an open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications.

How it Works:
Kubernetes groups containers into units called Pods and manages them across a cluster of machines (nodes). It automatically handles scheduling, scaling, networking, and failure recovery of containers.

Why it is Used:

* Automates container deployment
* Provides scalability
* Ensures high availability
* Supports rolling updates and rollbacks

Example:
A company deploys multiple Docker containers of a web application. Kubernetes automatically distributes them across nodes and restarts containers if they crash.

---

### 21. Difference between Pod and Deployment

Pod:
Definition:
A Pod is the smallest deployable unit in Kubernetes. It can contain one or more containers that share the same network and storage.

Characteristics:

* Runs containers
* Temporary by nature
* Usually managed by higher-level controllers

Deployment:
Definition:
A Deployment is a Kubernetes object used to manage and maintain multiple replicas of Pods.

Characteristics:

* Ensures desired number of Pods are running
* Supports rolling updates and rollbacks
* Automatically replaces failed Pods

Example:
A Deployment may maintain 3 replicas of a web application Pod to ensure availability.

---

### 22. What is a Kubernetes Service?

Definition:
A Kubernetes Service is an abstraction that provides a stable network endpoint to access a set of Pods.

Why it is Needed:
Pods are dynamic and their IP addresses change when they restart. A Service provides a stable IP and DNS name to access them.

Types of Services:

* ClusterIP
* NodePort
* LoadBalancer

Example:
A Service exposes backend Pods so that frontend applications can communicate with them reliably.

---

### 23. What is Ingress?

Definition:
Ingress is a Kubernetes resource used to manage external HTTP and HTTPS access to services inside the cluster.

How it Works:
Ingress uses an Ingress Controller (such as NGINX) to route incoming traffic to the correct service based on rules like hostnames or paths.

Example:
example.com/api → backend service
example.com/web → frontend service

Benefits:

* Centralized routing
* SSL termination
* Path-based routing

---

### 24. How does Kubernetes achieve self-healing?

Definition:
Self-healing is the ability of Kubernetes to automatically detect and recover from failures.

Mechanisms:

* Restart failed containers
* Replace crashed Pods
* Reschedule Pods to healthy nodes
* Use liveness and readiness probes

Example:
If a container crashes, Kubernetes automatically restarts it or creates a new Pod to maintain the desired state.

---

### 25. What is Horizontal Pod Autoscaler (HPA)?

Definition:
Horizontal Pod Autoscaler automatically adjusts the number of Pod replicas based on resource utilization such as CPU or memory.

How it Works:
HPA monitors metrics from the Kubernetes metrics server and increases or decreases the number of Pods depending on workload demand.

Example:
If CPU usage exceeds 70%, Kubernetes automatically scales the application from 3 Pods to 6 Pods.

Benefit:
Improves scalability and ensures applications can handle increased traffic without manual intervention.

---

## **AWS & Cloud (26–30)**

### 26. What is EC2?

Definition:
Amazon EC2 (Elastic Compute Cloud) is a cloud service provided by AWS that allows users to run virtual servers in the cloud.

How it Works:
EC2 provides scalable computing capacity where users can launch virtual machines called instances. Users can choose instance types based on CPU, memory, and storage requirements.

Example:
A DevOps engineer launches an EC2 instance to host a web application or run Docker containers.

---

### 27. What is S3 and what are its use cases?

Definition:
Amazon S3 (Simple Storage Service) is an object storage service used to store and retrieve large amounts of data.

How it Works:
Data is stored as objects inside buckets. Each object consists of the file, metadata, and a unique key.

Use Cases:

* Static website hosting
* Backup and disaster recovery
* Storing application logs
* Data lakes and analytics
* Storing CI/CD build artifacts

Example:
A company stores application backups and logs in an S3 bucket.

---

### 28. What is IAM and why is it important?

Definition:
AWS IAM (Identity and Access Management) is a service used to securely control access to AWS resources.

How it Works:
IAM allows administrators to create users, groups, and roles with specific permissions using policies.

Why it is Important:

* Provides secure access control
* Implements least privilege principle
* Prevents unauthorized access

Example:
A DevOps engineer creates an IAM role that allows an EC2 instance to access an S3 bucket.

---

### 29. Difference between Security Group and Network ACL

Security Group:

* Instance-level firewall
* Controls inbound and outbound traffic
* Stateful (automatically allows response traffic)

Example:
Allow HTTP traffic on port 80 to EC2 instance.

Network ACL (NACL):

* Subnet-level firewall
* Controls traffic entering and leaving the subnet
* Stateless (rules must be defined for both directions)

Example:
Block a specific IP range from accessing the subnet.

Interview Summary:
Security Group = Instance-level, Stateful
Network ACL = Subnet-level, Stateless

---

### 30. What is Auto Scaling and how does it work?

Definition:
AWS Auto Scaling automatically adjusts the number of EC2 instances based on demand.

How it Works:
Auto Scaling monitors metrics such as CPU utilization using CloudWatch. Based on scaling policies, it launches new instances when demand increases and terminates instances when demand decreases.

Components:

* Launch Template
* Auto Scaling Group
* Scaling Policies

Example:
If CPU usage exceeds 70%, Auto Scaling automatically launches additional EC2 instances to handle traffic.

---

## **⭐ Question Almost Every Interviewer Asks**

### Explain the DevOps pipeline you implemented in your project

Problem:
In traditional development workflows, deployments were manual, slow, and error-prone. The goal of the project was to automate the entire application delivery process using DevOps tools and practices.

Tools Used:
GitHub, Jenkins, Docker, AWS ECR, Kubernetes (EKS), Prometheus, Grafana

Implementation (Pipeline Flow):

1. Code Commit (Version Control)
   Developers push application code to a GitHub repository. This acts as the central version control system.

2. CI/CD Pipeline Trigger
   A webhook triggers the Jenkins pipeline automatically whenever new code is pushed to the repository.

3. Build Stage
   Jenkins pulls the latest code from GitHub and builds the application.

4. Automated Testing
   Unit tests and basic validation checks run automatically to ensure the code is stable.

5. Docker Image Build
   A Dockerfile is used to package the application and its dependencies into a Docker image.

Example command:
docker build -t myapp:1.0 .

6. Push Image to Container Registry
   The Docker image is pushed to AWS ECR (Elastic Container Registry).

Example command:
docker push <aws_account_id>.dkr.ecr.region.amazonaws.com/myapp:1.0

7. Deployment to Kubernetes (EKS)
   Kubernetes deployment files are used to deploy the application containers to the EKS cluster.

Example:
kubectl apply -f deployment.yaml

Kubernetes ensures:

* Desired number of pods are running
* Rolling updates
* Self-healing if containers fail

8. Monitoring and Observability
   Prometheus collects metrics from the Kubernetes cluster and Grafana visualizes them using dashboards.

Metrics monitored:

* CPU usage
* Memory usage
* Pod health
* Application performance

Result:
This automated DevOps pipeline enables faster and reliable deployments, reduces manual errors, and allows continuous delivery of the application.

---