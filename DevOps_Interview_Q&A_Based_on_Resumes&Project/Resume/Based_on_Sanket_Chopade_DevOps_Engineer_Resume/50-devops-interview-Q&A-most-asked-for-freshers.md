# 50 DevOps Interview Questions (Most Asked for Freshers)

---

## **DevOps Fundamentals – Interview Ready Answers**

---

## 1. What is DevOps?

Definition: DevOps is a culture and set of practices that combines Development and Operations to improve collaboration, automate workflows, and deliver software faster and more reliably.

How it Works: DevOps focuses on automation, continuous integration, continuous delivery, monitoring, and collaboration between teams.

Example: In my project, we used Docker for containerization, Kubernetes for orchestration, Jenkins for CI/CD, and Terraform for infrastructure provisioning to automate the deployment pipeline.

---

## 2. What are the benefits of DevOps?

Definition: DevOps improves software delivery speed, reliability, and collaboration between teams.

How it Works: By automating build, test, deployment, and monitoring processes.

Example: In my project, using CI/CD pipelines reduced manual deployment steps and allowed faster feature releases.

Key Benefits:

* Faster deployments
* Improved collaboration
* Reduced failures
* Faster recovery
* Better scalability

---

## 3. What is the DevOps lifecycle?

Definition: The DevOps lifecycle represents the stages involved in continuous software development and delivery.

How it Works: It includes planning, coding, building, testing, releasing, deploying, operating, and monitoring.

Example: A typical lifecycle in my project:
Plan → Code → Build → Test → Release → Deploy → Operate → Monitor

---

## 4. What is CI/CD?

Definition: CI/CD stands for Continuous Integration and Continuous Delivery/Deployment, a practice that automates the software development pipeline.

How it Works:

* Continuous Integration automatically builds and tests code when developers commit changes.
* Continuous Delivery prepares code for deployment.
* Continuous Deployment automatically releases code to production.

Example: In Jenkins, when developers push code to GitHub, the pipeline automatically builds Docker images and deploys them to Kubernetes.

---

## 5. Difference between Continuous Integration, Continuous Delivery, and Continuous Deployment

Continuous Integration:
Developers frequently merge code into a shared repository where automated builds and tests run.

Continuous Delivery:
Code is automatically prepared for production but requires manual approval before release.

Continuous Deployment:
Every change that passes testing is automatically deployed to production without manual intervention.

---

## 6. What is Infrastructure as Code (IaC)?

Definition: Infrastructure as Code is the practice of managing infrastructure using code instead of manual configuration.

How it Works: Infrastructure resources like servers, networks, and databases are defined using scripts or configuration files.

Example: Using Terraform, we can create AWS EC2 instances, VPCs, and Kubernetes clusters automatically.

---

## 7. What is Configuration Management?

Definition: Configuration Management ensures systems are configured consistently across environments.

How it Works: Tools automate server configuration, package installation, and system setup.

Example: Tools like Ansible or Puppet configure servers automatically instead of manual setup.

---

## 8. What are the key DevOps tools used in industry?

Version Control: Git, GitHub

CI/CD Tools: Jenkins, GitHub Actions

Containerization: Docker

Container Orchestration: Kubernetes

Infrastructure as Code: Terraform

Monitoring: Prometheus, Grafana

Cloud Platforms: AWS, Azure, GCP

---

## **Linux Questions – DevOps Interview Ready Answers**

---

## 9. What is Linux and why is it preferred in DevOps?

Definition: Linux is an open-source operating system based on the Unix architecture that manages hardware resources and provides services for applications.

How it Works: Linux allows multiple users to run processes simultaneously, provides strong security, and supports powerful command-line tools for automation.

Example: In DevOps environments, Linux servers are widely used to host applications, run Docker containers, manage Kubernetes clusters, and automate tasks through shell scripting.

Why it is preferred in DevOps:

* Open-source and cost effective
* Highly stable and secure
* Powerful CLI for automation
* Strong support for DevOps tools like Docker, Kubernetes, and Jenkins

---

## 10. Difference between Soft Link and Hard Link

Soft Link (Symbolic Link):

* A pointer to another file.
* Can link across file systems.
* If the original file is deleted, the soft link becomes broken.

Hard Link:

* Direct reference to the inode of the file.
* Cannot cross file systems.
* Even if the original file is deleted, the hard link still works.

Example:
ln -s file.txt softlink.txt   # Soft link
ln file.txt hardlink.txt     # Hard link

---

## 11. What does this command do?

chmod 755 file.sh

Explanation:
This command changes the file permissions of file.sh.

Permission breakdown:
7 = read + write + execute for owner
5 = read + execute for group
5 = read + execute for others

Result:

* Owner can read, write, and execute the file.
* Group and others can read and execute it.

Example:
Used to make shell scripts executable.

---

## 12. How do you check CPU, Memory, and Disk usage?

CPU Usage:
top
htop

Memory Usage:
free -h

Disk Usage:
df -h

Example:
If a server becomes slow, these commands help identify whether CPU, memory, or disk is causing the issue.

---

## 13. Difference between top, ps, and htop

top:
Displays real-time system processes and resource usage.

ps:
Shows a snapshot of currently running processes.

htop:
An interactive and more user-friendly version of top with better visualization.

Example:
htop allows sorting processes by CPU or memory usage quickly.

---

## 14. How do you find large files in Linux?

Command:
find / -type f -size +100M

Explanation:

* find searches files
* / searches from root directory
* -type f specifies files
* -size +100M finds files larger than 100MB

Example:
Used to identify disk space issues on production servers.

---

## 15. What is cron and how do you schedule jobs?

Definition:
Cron is a time-based job scheduler in Linux used to automate tasks.

How it Works:
Cron runs commands or scripts automatically at scheduled times defined in the crontab file.

Example:
Open crontab:
crontab -e

Schedule a job:
0 2 * * * /home/user/backup.sh

Explanation:
This runs the backup script every day at 2 AM.

Cron format:
Minute Hour Day Month Day-of-week Command

---

## **Git & GitHub – DevOps Interview Ready Answers**

---

## 16. What is Git?

Definition: Git is a distributed version control system used to track changes in source code and enable collaboration among developers.

How it Works: Git stores snapshots of the project history in repositories. Developers can create branches, commit changes, and merge code while maintaining a complete history of modifications.

Example: In DevOps workflows, developers push code to a GitHub repository, which then triggers CI/CD pipelines (for example Jenkins) to build, test, and deploy applications.

---

## 17. Difference between git pull and git fetch

git fetch:

* Downloads changes from the remote repository.
* Does NOT automatically merge changes into the current branch.
* Used when you want to review changes before merging.

Example:
`git fetch origin`

git pull:

* Fetches changes from the remote repository AND merges them into the current branch.
* Combines `git fetch` + `git merge`.

Example:
`git pull origin main`

---

## 18. What is a merge conflict?

Definition: A merge conflict occurs when Git cannot automatically merge changes because multiple developers modified the same lines of a file.

How it Works: Git marks the conflicting sections in the file and asks the developer to manually resolve them.

Example:
Two developers modify the same function in a file. When merging branches, Git highlights the conflicting code sections so they can be resolved manually.

Steps to resolve:

1. Identify the conflicting file
2. Edit the file and resolve the conflict
3. Add the file: `git add file`
4. Commit the merge

---

## 19. Difference between fork and clone

Fork:

* Creates a copy of a repository in your GitHub account.
* Used when contributing to open-source projects.
* Maintains a connection to the original repository.

Clone:

* Downloads a repository from GitHub to your local machine.
* Used to work on the project locally.

Example:
`git clone https://github.com/user/repo.git`

Typical workflow:
Fork repository → Clone it locally → Make changes → Push → Create Pull Request.

---

## 20. What is Git branching strategy?

Definition: A Git branching strategy defines how teams organize and manage branches during development.

How it Works: Different branches are used for development, testing, and production releases.

Common branching strategies:

Git Flow:

* main: production code
* develop: integration branch
* feature branches: new features

GitHub Flow:

* main branch
* feature branches
* pull requests for merging

Example:
A developer creates a feature branch to implement a new feature, tests it, and then merges it into the main branch through a pull request.

---

## **Docker – DevOps Interview Ready Answers**

---

## 21. What is Docker?

Definition: Docker is a containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers.

How it Works: Docker uses OS-level virtualization where containers share the host operating system kernel but run in isolated environments. Applications are packaged as Docker images and run as containers.

Example: In DevOps projects, Docker is used to containerize microservices so they can run consistently across development, testing, and production environments.

---

## 22. Difference between Docker Image and Docker Container

Docker Image:

* A read-only template used to create containers.
* Contains application code, libraries, dependencies, and configuration.
* Built using a Dockerfile.

Example: `nginx`, `node`, or custom application images.

Docker Container:

* A running instance of a Docker image.
* Executes the application in an isolated environment.

Example: When we run `docker run nginx`, it creates and starts a container from the nginx image.

---

## 23. What is a Dockerfile?

Definition: A Dockerfile is a text file that contains instructions to build a Docker image.

How it Works: It defines the base image, application dependencies, configuration steps, and commands required to run the application.

Example Dockerfile:

FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]

This builds an image that runs a Node.js application.

---

## 24. What is the purpose of docker build, docker run, docker ps?

`docker build`:

* Builds a Docker image from a Dockerfile.
  Example:
  `docker build -t myapp .`

`docker run`:

* Creates and starts a container from an image.
  Example:
  `docker run -d -p 80:80 nginx`

`docker ps`:

* Lists running containers.
  Example:
  `docker ps`

---

## 25. What is Docker Volume?

Definition: Docker volume is a mechanism used to persist data generated by containers.

How it Works: Volumes store data outside the container filesystem so that data remains even if the container is deleted.

Example: Databases like MySQL or PostgreSQL store their data in Docker volumes to prevent data loss when containers restart.

Example command:
`docker volume create myvolume`

---

## 26. Difference between Docker Compose and Docker Swarm

Docker Compose:

* Used to define and run multi-container Docker applications.
* Uses a `docker-compose.yml` file.
* Mainly used for development and testing environments.

Example:
Running an application with web server + database containers.

Docker Swarm:

* Docker’s native container orchestration tool.
* Used to manage clusters of Docker nodes.
* Supports scaling, load balancing, and high availability.

Example:
Running containers across multiple servers in a production cluster.

---

## **Kubernetes – DevOps Interview Ready Answers**

---

## 27. What is Kubernetes?

Definition: Kubernetes is an open-source container orchestration platform used to automate deployment, scaling, and management of containerized applications.

How it Works: Kubernetes groups containers into logical units called pods and manages them across a cluster of nodes. It handles scheduling, scaling, networking, and health monitoring automatically.

Example: In DevOps environments, Docker containers are deployed and managed on Kubernetes clusters such as AWS EKS to ensure high availability and scalability.

---

## 28. What are the main components of Kubernetes?

Kubernetes has two main parts: Control Plane and Worker Nodes.

Control Plane Components:

API Server:
Acts as the entry point for all Kubernetes commands and communication.

Scheduler:
Decides which node should run a new pod based on available resources.

Controller Manager:
Maintains the desired state of the cluster.

etcd:
A distributed key-value store that stores cluster configuration and state.

Worker Node Components:

Kubelet:
Agent that communicates with the control plane and ensures containers run properly.

Kube-proxy:
Handles networking and load balancing for services.

Container Runtime:
Software that runs containers (for example Docker or containerd).

---

## 29. Difference between Pod and Deployment

Pod:

* The smallest deployable unit in Kubernetes.
* Contains one or more containers.
* Usually temporary and not used directly in production.

Deployment:

* Manages multiple pod replicas.
* Provides rolling updates and rollbacks.
* Ensures desired number of pods are always running.

Example:
A Deployment may maintain 3 replicas of a pod to ensure high availability.

---

## 30. What is a Kubernetes Service?

Definition: A Service is an abstraction that exposes a group of pods as a network service.

How it Works: Since pods are ephemeral and their IP addresses change, a Service provides a stable IP address and DNS name for communication.

Example:
A frontend service communicates with backend pods using a Kubernetes Service.

Common types:

* ClusterIP
* NodePort
* LoadBalancer

---

## 31. What is Ingress?

Definition: Ingress is an API object that manages external HTTP and HTTPS access to services inside a Kubernetes cluster.

How it Works: It routes external traffic to internal services based on hostnames or URL paths.

Example:
example.com/api → backend service
example.com/web → frontend service

---

## 32. What is Horizontal Pod Autoscaler (HPA)?

Definition: HPA automatically scales the number of pods in a deployment based on resource usage such as CPU or memory.

How it Works: Kubernetes monitors metrics and increases or decreases pod replicas when thresholds are crossed.

Example:
If CPU usage exceeds 70%, Kubernetes may increase pods from 3 to 6.

---

## 33. How does Kubernetes achieve self-healing?

Kubernetes automatically maintains the desired state of the system.

Self-healing mechanisms:

Restarting containers if they fail.

Replacing unhealthy pods.

Rescheduling pods if a node fails.

Running health checks using liveness and readiness probes.

Example:
If a pod crashes, Kubernetes automatically creates a new pod to replace it.

---

## **AWS Cloud – DevOps Interview Ready Answers**

---

## 34. What is AWS?

Definition: Amazon Web Services (AWS) is a cloud computing platform that provides on-demand computing resources such as servers, storage, networking, databases, and analytics over the internet.

How it Works: AWS allows organizations to provision infrastructure using a pay-as-you-go model instead of maintaining physical data centers.

Example: In DevOps projects, AWS services like EC2, S3, EKS, and RDS are commonly used to host applications and infrastructure.

---

## 35. What is EC2?

Definition: Amazon EC2 (Elastic Compute Cloud) is a service that provides virtual servers in the cloud.

How it Works: Users can launch virtual machines called instances with different CPU, memory, and storage configurations.

Example: DevOps engineers use EC2 instances to host applications, run Docker containers, or deploy Kubernetes clusters.

---

## 36. What is S3?

Definition: Amazon S3 (Simple Storage Service) is an object storage service used to store and retrieve large amounts of data.

How it Works: Data is stored as objects inside buckets and can be accessed via HTTP or APIs.

Example: S3 is commonly used to store application backups, static website files, logs, and CI/CD artifacts.

---

## 37. What is IAM?

Definition: IAM (Identity and Access Management) is a service used to manage access to AWS resources securely.

How it Works: IAM allows administrators to create users, groups, roles, and policies that define permissions for accessing AWS services.

Example: A DevOps engineer may create IAM roles for EC2 instances so they can securely access S3 without storing credentials.

---

## 38. What is a VPC?

Definition: A Virtual Private Cloud (VPC) is a logically isolated network in AWS where users can launch resources securely.

How it Works: A VPC allows users to define IP address ranges, subnets, route tables, and security controls.

Example: Applications can be deployed in private subnets while load balancers are placed in public subnets.

---

## 39. Difference between Security Group and Network ACL

Security Group:

* Acts as a virtual firewall for EC2 instances.
* Operates at the instance level.
* Supports only allow rules.
* Stateful (return traffic is automatically allowed).

Network ACL:

* Acts as a firewall at the subnet level.
* Supports allow and deny rules.
* Stateless (both inbound and outbound rules must be defined).

---

## 40. What is an Elastic Load Balancer?

Definition: Elastic Load Balancer (ELB) distributes incoming traffic across multiple servers to improve availability and fault tolerance.

How it Works: The load balancer monitors the health of instances and routes traffic only to healthy ones.

Example: In a web application, ELB distributes traffic across multiple EC2 instances running the application.

---

## 41. What is Auto Scaling?

Definition: Auto Scaling automatically adjusts the number of EC2 instances based on demand.

How it Works: When traffic increases, new instances are launched. When demand decreases, unnecessary instances are terminated.

Example: During high traffic periods, Auto Scaling may increase instances from 2 to 10 to maintain performance.

---

## **CI/CD – DevOps Interview Ready Answers**

## 42. What is Jenkins?

Definition: Jenkins is an open-source automation server used to implement Continuous Integration and Continuous Delivery (CI/CD) pipelines.

How it Works: Jenkins automates the process of building, testing, and deploying applications whenever code changes are pushed to a repository.

Example: In a typical DevOps workflow, when developers push code to GitHub, Jenkins automatically triggers a pipeline that builds the application, runs tests, creates Docker images, and deploys them to Kubernetes.

---

## 43. What are the stages of a CI/CD pipeline?

A typical CI/CD pipeline includes the following stages:

Source:
Code is stored in a version control system like GitHub.

Build:
Application is compiled and dependencies are installed.

Test:
Automated tests are executed to ensure code quality.

Package:
Application artifacts or Docker images are created.

Deploy:
The application is deployed to staging or production environments.

Monitor:
Monitoring tools track application performance and detect issues.

Example:
In a DevOps project, Jenkins builds a Docker image after a code commit and deploys it to a Kubernetes cluster.

---

## 44. Difference between Pipeline and Job

Pipeline:

* A sequence of automated stages in CI/CD.
* Defines the complete workflow from build to deployment.
* Usually written using a Jenkinsfile.

Job:

* A single task or unit of work in Jenkins.
* Example: running tests or building an application.

Example:
A pipeline may include multiple jobs such as build job, test job, and deployment job.

---

## 45. What is Webhook in CI/CD?

Definition: A webhook is an automated trigger that notifies a system when a specific event occurs.

How it Works: When developers push code to a Git repository, the webhook sends a signal to Jenkins to start the CI/CD pipeline automatically.

Example:
GitHub webhook triggers Jenkins pipeline whenever a new commit is pushed to the repository.

---

## **Monitoring & Logging – DevOps Interview Ready Answers**

---

## 46. Why is monitoring important in DevOps?

Definition: Monitoring is the practice of continuously observing the performance, availability, and health of applications and infrastructure.

How it Works: Monitoring tools collect metrics such as CPU usage, memory usage, response time, and error rates from systems and applications.

Example: If CPU usage suddenly spikes in production, monitoring tools can alert DevOps engineers so they can investigate and resolve the issue quickly.

Benefits:

* Early detection of failures
* Improved system reliability
* Faster troubleshooting
* Better performance optimization

---

## 47. What tools are used for monitoring?

Common monitoring tools used in DevOps include:

Prometheus:
Used for collecting and storing metrics from systems and applications.

Grafana:
Used for visualizing metrics and creating dashboards.

ELK Stack (Elasticsearch, Logstash, Kibana):
Used for centralized logging and log analysis.

Datadog / New Relic:
Commercial tools for infrastructure and application monitoring.

Example:
Prometheus collects Kubernetes metrics and Grafana displays them on dashboards for easier monitoring.

---

## 48. Difference between Prometheus and Grafana

Prometheus:

* Monitoring and metrics collection system
* Stores time-series data
* Uses PromQL for querying metrics

Grafana:

* Visualization and dashboard tool
* Displays metrics collected by Prometheus or other sources
* Used for creating monitoring dashboards

Example:
Prometheus collects CPU usage metrics from Kubernetes nodes, while Grafana visualizes those metrics in dashboards.

---

## **DevOps Scenario Questions – Interview Ready Answers**

---

## 49. Your deployment failed in production. What steps will you take?

Identify:
First, I would check the CI/CD pipeline logs to see at which stage the deployment failed.

Diagnose:
I would analyze logs from the deployment tool (for example Jenkins) and check application logs or Kubernetes pod logs using:

kubectl logs <pod-name>

I would also verify configuration issues such as environment variables, container image versions, or resource limits.

Fix:
Once the root cause is identified, I would fix the configuration or code issue and redeploy the application.

Rollback:
If the issue impacts production users, I would immediately rollback to the previous stable version using the CI/CD pipeline or Kubernetes rollback command.

Prevent:
To avoid similar issues, I would add additional testing stages, monitoring alerts, and validation checks in the pipeline.

---

## 50. Your server CPU suddenly spikes to 100%. How will you investigate?

Identify:
First, I would confirm CPU usage using monitoring tools like Prometheus or Linux commands.

Commands:

Top or htop to identify processes consuming CPU

ps aux --sort=-%cpu

Diagnose:
After identifying the process causing high CPU usage, I would check application logs, system logs, and recent deployments that might have caused the issue.

Fix:
Depending on the cause, I might restart the service, scale the application, optimize the process, or allocate more resources.

Prevent:
To prevent recurrence, I would implement monitoring alerts and auto-scaling policies.

---

## **⭐ Most Important Question (Almost Every Interview)**

---

## Explain your project architecture and DevOps pipeline.

### Overview

In my DevOps project, I implemented an automated CI/CD pipeline to build, test, containerize, and deploy microservices to a Kubernetes cluster on AWS. The pipeline ensures faster, reliable, and repeatable deployments.

### Step‑by‑Step Architecture

1. Code Commit
   Developers push code changes to a GitHub repository. Git is used for version control and collaboration.

2. Pipeline Trigger
   A GitHub webhook automatically triggers the Jenkins CI/CD pipeline whenever new code is pushed.

3. Build Stage
   Jenkins pulls the latest code from GitHub and builds the application.

4. Test Stage
   Automated tests are executed to ensure the code works correctly and does not break existing functionality.

5. Docker Image Build
   After successful tests, Jenkins builds a Docker image using a Dockerfile that contains the application and its dependencies.

6. Push to Container Registry
   The Docker image is pushed to a container registry such as Docker Hub or Amazon ECR so it can be used during deployment.

7. Infrastructure Provisioning
   Terraform is used to provision cloud infrastructure such as VPC, EC2 instances, and Kubernetes clusters.

8. Deployment to Kubernetes
   The application is deployed to a Kubernetes cluster using Kubernetes manifests or Helm charts. Kubernetes manages pod scheduling, scaling, and service discovery.

9. Monitoring and Observability
   Prometheus collects metrics from Kubernetes and application services, while Grafana provides dashboards for visualization and monitoring.

### Result

This automated pipeline improves deployment speed, ensures consistency across environments, and allows quick rollback and monitoring of application health.

---