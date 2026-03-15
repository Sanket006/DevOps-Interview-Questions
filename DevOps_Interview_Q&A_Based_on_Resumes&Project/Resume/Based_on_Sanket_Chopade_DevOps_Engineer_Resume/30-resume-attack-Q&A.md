# DevOps Resume Attack Questions – Interview Ready Answers

---

## **Architecture Attack Questions**

---

## 1. Draw the complete architecture diagram of your ApnaKart project from user request to backend service.

Type: System Design
Method: Requirements → Architecture → Tools → Scaling

Requirements:
The system should host a scalable e‑commerce web application with high availability, automated deployments, and monitoring.

Architecture:
User → CloudFront → S3 (React frontend) → API Gateway / Ingress → EKS Cluster → Microservices Pods → Database

Tools:
AWS S3, CloudFront, EKS, Docker, Kubernetes, Jenkins, Terraform, ECR, Datadog

Scaling:
Kubernetes HPA automatically scales pods based on CPU or memory usage.

---

## 2. What happens step-by-step when a user opens your website?

Type: System Design
Method: Requirements → Architecture → Tools → Scaling

Requirements:
Serve the frontend quickly and route API requests to backend services.

Architecture:

1. User sends request through browser
2. DNS resolves the CloudFront domain
3. CloudFront fetches frontend files from S3
4. React application loads in the browser
5. React sends API request to backend
6. Request goes through Kubernetes Ingress
7. Ingress routes traffic to appropriate microservice pod
8. Microservice interacts with database and returns response

Tools:
Route53, CloudFront, S3, Kubernetes Ingress Controller

Scaling:
CloudFront caches static assets and Kubernetes scales pods automatically.

---

## 3. Why did you choose EKS instead of ECS or plain EC2?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Amazon EKS is a managed Kubernetes service used for container orchestration.

How it Works:
EKS manages the Kubernetes control plane while worker nodes run containerized applications. It supports features like auto‑scaling, service discovery, and rolling deployments.

Example:
In my project, each microservice runs as a Kubernetes deployment inside the EKS cluster, allowing independent scaling and updates.

---

## 4. Why did you host React on S3 + CloudFront instead of EC2?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
S3 + CloudFront provides a serverless and globally distributed way to host static websites.

How it Works:
S3 stores the static frontend files and CloudFront distributes them across edge locations to reduce latency.

Example:
In the project, React build files were uploaded to S3 and served via CloudFront, improving page load speed and reducing server management.

---

## 5. If CloudFront cache becomes outdated, how will you update it?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
Users see old content even after new deployment.

Diagnose:
CloudFront edge locations are serving cached objects.

Fix:
Create a CloudFront cache invalidation request to clear outdated objects.

Prevent:
Use versioned file names during deployments so browsers automatically fetch updated assets.

---

## **Terraform Attack Questions**

---

## 1. Show the folder structure of your Terraform project.

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
We needed a well-organized Terraform structure to manage AWS infrastructure such as VPC, EKS cluster, networking, and IAM in a scalable and reusable way.

Tools Used:
Terraform, AWS, GitHub

Implementation:
The project was structured using a modular approach.

terraform/
├── modules/
│   ├── vpc/
│   ├── eks/
│   ├── iam/
│   └── security-groups/
│
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── terraform.tfvars
│   │
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       └── terraform.tfvars
│
├── backend.tf
├── providers.tf
└── outputs.tf

This structure allowed reuse of modules across environments.

Result:
Infrastructure became modular, reusable, and easier to maintain.

---

## 2. What variables did you define in your Terraform modules?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Terraform variables are used to parameterize infrastructure configuration and make modules reusable.

How it Works:
Variables are defined in variables.tf and passed through terraform.tfvars or environment-specific configuration.

Example:
Some important variables used in the project included:

* region
* vpc_cidr
* public_subnet_cidrs
* private_subnet_cidrs
* cluster_name
* node_instance_type
* desired_node_count

This allowed us to reuse the same module for different environments.

---

## 3. Where did you store the Terraform state file?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Terraform state stores the mapping between Terraform configuration and real infrastructure resources.

How it Works:
Instead of storing state locally, we configured a remote backend in AWS.

Example:
The state file was stored in an S3 bucket and state locking was enabled using DynamoDB.

Benefits:

* Prevents concurrent modifications
* Enables team collaboration
* Maintains centralized state management

---

## 4. What happens if someone manually changes infrastructure in AWS after Terraform deploys it?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
Infrastructure configuration in AWS no longer matches the Terraform state.

Diagnose:
Running `terraform plan` will detect differences between the Terraform configuration and actual resources.

Fix:
Terraform will propose changes to bring the infrastructure back to the desired state defined in code.

Prevent:
Use strict IAM policies and enforce Infrastructure-as-Code practices so all infrastructure changes go through Terraform.

---

## 5. How do you avoid Terraform state conflicts in teams?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
State conflicts occur when multiple users try to modify Terraform state simultaneously.

How it Works:
We used remote state storage with locking mechanisms.

Example:

* Terraform state stored in S3
* State locking enabled with DynamoDB
* CI/CD pipelines used for controlled Terraform execution

This ensures only one operation can modify the state at a time.

---

## **CI/CD Attack Questions**

---

## 1. Explain your Jenkins pipeline stages step-by-step.

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
We needed an automated CI/CD pipeline to build, test, scan, and deploy microservices whenever developers push code to GitHub.

Tools Used:
Jenkins, GitHub, Docker, SonarQube, AWS ECR, Kubernetes (EKS)

Implementation:

1. Code Checkout – Jenkins pulls the latest code from GitHub repository.
2. Build Stage – Application is built using the required build tool.
3. Code Quality Scan – SonarQube scans the code for bugs, vulnerabilities, and code smells.
4. Docker Build – Jenkins builds a Docker image for the application.
5. Push to ECR – The Docker image is pushed to Amazon ECR.
6. Deployment – Kubernetes deployment is updated to pull the latest image.

Result:
The pipeline automated build, testing, security scanning, and deployment which reduced manual work and ensured faster and consistent releases.

---

## 2. How does Jenkins know when new code is pushed to GitHub?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Jenkins detects code changes using webhooks or polling mechanisms.

How it Works:
In our setup, GitHub webhook was configured to notify Jenkins whenever new commits were pushed to the repository.

Example:
When a developer pushes code to GitHub, the webhook sends an HTTP request to Jenkins which automatically triggers the pipeline.

---

## 3. What happens if the build stage fails but deployment still runs?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
Deployment occurs even though the application build failed.

Diagnose:
The pipeline stages may not have proper failure conditions or stage dependencies.

Fix:
Configure the Jenkins pipeline so that deployment runs only if previous stages succeed.

Example Fix:
Use stage conditions like `when` or ensure pipeline stops on failure.

Prevent:
Implement pipeline gates and proper error handling so that deployment cannot proceed if build or tests fail.

---

## 4. How did you integrate SonarQube in the pipeline?

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
We wanted to ensure code quality and detect security vulnerabilities before deployment.

Tools Used:
Jenkins, SonarQube, Sonar Scanner

Implementation:

1. SonarQube server was installed and configured.
2. Jenkins SonarQube plugin was added.
3. SonarQube credentials were configured in Jenkins.
4. Pipeline stage executed Sonar Scanner to analyze code.

Example command used:

sonar-scanner 
-Dsonar.projectKey=apnakart 
-Dsonar.sources=. 
-Dsonar.host.url=[http://sonarqube:9000](http://sonarqube:9000)

Result:
The pipeline automatically checks code quality and prevents merging code with critical issues.

---

## 5. How did Jenkins authenticate with Kubernetes or EKS?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Jenkins must authenticate with the Kubernetes cluster to deploy applications.

How it Works:
Authentication is done using kubeconfig credentials or IAM roles in the case of AWS EKS.

Example:
In our setup, Jenkins used AWS IAM role permissions and kubeconfig to access the EKS cluster. The pipeline used kubectl commands to apply Kubernetes manifests and update deployments.

Example deployment command:

kubectl apply -f deployment.yaml

---

## **Kubernetes Attack Questions**

## 1. Show the Kubernetes YAML structure you used.

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
We needed Kubernetes manifests to define how containers should run, scale, and communicate inside the cluster.

Tools Used:
Kubernetes, Docker, EKS, kubectl

Implementation:
The typical Kubernetes YAML files used in the project were:

k8s/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
├── configmap.yaml
└── secret.yaml

Each file had a specific responsibility:

* deployment.yaml → manages application pods
* service.yaml → exposes pods internally in the cluster
* ingress.yaml → exposes services externally
* configmap.yaml → stores non-sensitive configuration
* secret.yaml → stores sensitive data like credentials

Result:
This structure made deployments reproducible and easy to manage across environments.

---

## 2. What is inside your deployment.yaml?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
A Deployment YAML defines how Kubernetes should create and manage pods running the application containers.

How it Works:
The deployment specifies the container image, number of replicas, resource limits, environment variables, and update strategy.

Example Structure:

apiVersion: apps/v1
kind: Deployment
metadata:
name: apnakart-backend
spec:
replicas: 3
selector:
matchLabels:
app: apnakart
template:
metadata:
labels:
app: apnakart
spec:
containers:
- name: backend
image: <aws_account>.dkr.ecr.<region>.amazonaws.com/apnakart:latest
ports:
- containerPort: 3000
envFrom:
- configMapRef:
name: app-config
- secretRef:
name: db-secret

---

## 3. What happens internally when you run `kubectl apply -f deployment.yaml`?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
`kubectl apply` sends the desired configuration to the Kubernetes API server.

How it Works:

1. kubectl reads the YAML manifest.
2. It sends the configuration to the Kubernetes API server.
3. The API server validates the request.
4. Desired state is stored in etcd.
5. The Deployment controller creates or updates ReplicaSets.
6. ReplicaSets create the required number of Pods.
7. Scheduler assigns pods to nodes.
8. Kubelet on the node starts containers using the container runtime.

Example:
Running `kubectl apply -f deployment.yaml` for the ApnaKart backend creates three pods across cluster nodes.

---

## 4. How does Kubernetes perform rolling updates?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Rolling update is a deployment strategy that gradually replaces old pods with new ones without downtime.

How it Works:

1. A new ReplicaSet is created with the updated container image.
2. Kubernetes slowly increases new pods while decreasing old pods.
3. Health checks ensure new pods are ready before terminating old ones.

Example:
If replicas = 3, Kubernetes may create 1 new pod first, verify it becomes healthy, then terminate 1 old pod until all pods run the new version.

---

## 5. How do ConfigMaps and Secrets get injected into pods?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
ConfigMaps and Secrets allow external configuration to be injected into containers.

How it Works:
They can be injected as environment variables or mounted as files inside containers.

Example:

Using environment variables:

spec:
containers:

* name: app
  envFrom:

  * configMapRef:
    name: app-config
  * secretRef:
    name: db-secret

Using volume mounts:

volumes:

* name: config-volume
  configMap:
  name: app-config

Containers can then read the configuration at runtime without rebuilding the image.

---

## **Docker Attack Questions**

---

## 1. Show the Dockerfile you used for your microservices.

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
Each microservice needed to be packaged into a portable container image so it could run consistently across development, CI/CD pipeline, and Kubernetes clusters.

Tools Used:
Docker, Node.js (example microservice), AWS ECR, Jenkins

Implementation:
Example Dockerfile used for a backend microservice:

FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]

Key points:

* Used lightweight alpine base image
* Installed only production dependencies
* Exposed the application port

Result:
The application could be built as a Docker image and pushed to Amazon ECR, then deployed to Kubernetes.

---

## 2. How did you reduce Docker image size?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Reducing Docker image size improves build speed, deployment speed, and reduces storage usage.

How it Works:
Several best practices were used:

* Use lightweight base images (e.g., alpine)
* Remove unnecessary build tools
* Install only production dependencies
* Use multi-stage builds

Example:
Multi-stage build example:

FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
RUN npm install --production

This keeps the final image smaller by excluding build dependencies.

---

## 3. What happens if a container keeps restarting?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
The container repeatedly restarts, often called a CrashLoopBackOff in Kubernetes.

Diagnose:
Check logs and container status:

kubectl logs <pod-name>

kubectl describe pod <pod-name>

Common reasons include:

* Application crash
* Missing environment variables
* Incorrect configuration
* Port conflicts

Fix:
Correct the configuration, environment variables, or application bug causing the crash.

Prevent:
Add proper health checks, configuration validation, and monitoring to detect issues early.

---

## 4. How does Docker communicate with Kubernetes?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Docker provides the container images, while Kubernetes orchestrates and manages those containers.

How it Works:

1. Developers build Docker images.
2. Images are pushed to a container registry such as Amazon ECR.
3. Kubernetes pulls the image from the registry.
4. The container runtime on the node runs the container inside a pod.

Example:
In the ApnaKart project:

* Jenkins builds a Docker image.
* Image is pushed to Amazon ECR.
* Kubernetes deployment pulls the image and runs it inside the EKS cluster.

This allows Kubernetes to manage scaling, networking, and updates for Docker containers.

---

## **Monitoring Attack Questions**

---

## 1. What metrics were you collecting in Prometheus?

Type: Technical Concept
Method: Definition → How it Works → Example

Definition:
Prometheus is a monitoring system that collects time-series metrics from applications, infrastructure, and Kubernetes components.

How it Works:
Prometheus scrapes metrics from exporters and application endpoints such as /metrics at regular intervals and stores them as time-series data.

Example:
In the project we collected several important metrics:

Infrastructure Metrics:

* CPU usage
* Memory usage
* Disk I/O
* Network traffic

Kubernetes Metrics:

* Pod CPU and memory utilization
* Pod restart count
* Node resource usage
* API server latency

Application Metrics:

* HTTP request rate
* Response latency
* Error rate (5xx errors)

These metrics helped monitor both application health and cluster performance.

---

## 2. What alerts did you configure in Grafana or Datadog?

Type: Project Question
Method: Problem → Tools Used → Implementation → Result

Problem:
We needed real-time alerts to quickly detect system failures, high resource usage, and application errors.

Tools Used:
Prometheus, Grafana, Datadog

Implementation:
We configured alerts for critical scenarios such as:

Infrastructure Alerts:

* CPU usage > 80%
* Memory usage > 85%
* Disk space low

Kubernetes Alerts:

* Pod CrashLoopBackOff
* Pod restart count increasing
* Node not ready

Application Alerts:

* High error rate (5xx responses)
* High request latency

Alerts were sent through integrated notification channels for quick response.

Result:
The team could detect issues early and respond quickly before they impacted users.

---

## 3. What exactly helped reduce MTTR by 30%?

Type: Behavioral / Project
Method: STAR (Situation → Task → Action → Result)

Situation:
Previously when an application issue occurred it took longer to identify the root cause because monitoring and alerting were limited.

Task:
The goal was to improve observability so incidents could be detected and resolved faster.

Action:
We implemented centralized monitoring using Prometheus and dashboards in Grafana/Datadog. We also configured automated alerts for high CPU usage, pod failures, and application error rates so engineers were immediately notified.

Result:
Because issues were detected earlier and dashboards provided clear insights into system health, the average incident resolution time was reduced by about 30%.

---

## **Linux & Troubleshooting Attack Questions**

---

## 1. If CPU suddenly spikes in production, what commands will you run first?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
The first step is to confirm that CPU usage is actually high and determine which processes are consuming resources.

Diagnose:
Commands I would run:

1. Check overall CPU usage

top

or

htop

2. Identify high CPU processes

ps aux --sort=-%cpu | head

3. Check system load

uptime

4. Check container or Kubernetes resource usage

kubectl top pods

kubectl top nodes

Fix:
After identifying the process or container causing the spike, I would investigate the application logs or restart the problematic service if needed.

Prevent:

* Configure resource limits in Kubernetes
* Implement autoscaling
* Set monitoring alerts for CPU thresholds

---

## 2. If a pod is not reachable from outside, what will you check?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
Users cannot access the service exposed by the Kubernetes pod.

Diagnose:
Check the following step-by-step:

1. Verify pod status

kubectl get pods

2. Check service configuration

kubectl get svc

3. Verify endpoints

kubectl get endpoints

4. Check ingress rules

kubectl describe ingress

5. Verify pod logs

kubectl logs <pod-name>

6. Check security groups or network policies if using cloud infrastructure.

Fix:
Correct service type, port mappings, or ingress configuration so traffic routes correctly to the pod.

Prevent:
Use proper service definitions, health checks, and automated deployment validation.

---

## 3. If a deployment fails during release, how do you rollback?

Type: Troubleshooting
Method: Identify → Diagnose → Fix → Prevent

Identify:
The new deployment causes application errors or pod failures.

Diagnose:
Check rollout status and logs:

kubectl rollout status deployment <deployment-name>

kubectl describe pod <pod-name>

kubectl logs <pod-name>

Fix:
Rollback to the previous stable version using:

kubectl rollout undo deployment <deployment-name>

Prevent:

* Use rolling updates
* Implement readiness and liveness probes
* Add CI/CD pipeline checks before deployment

These practices ensure safer deployments and quick recovery from failures.

---

## **Most Dangerous Interview Question**

---

## "Explain exactly what YOU did in the project."

Type: Behavioral / Project
Method: STAR (Situation → Task → Action → Result)

Situation:
During my DevOps internship project, we needed to deploy a microservices-based e-commerce application called ApnaKart in a scalable, automated, and production-like environment. The goal was to move from manual deployments to a fully automated DevOps pipeline using cloud infrastructure and container orchestration.

Task:
My responsibility was to design and implement the DevOps workflow including infrastructure provisioning, containerization of services, CI/CD pipeline setup, Kubernetes deployment, and monitoring.

Action:

1. I containerized the microservices using Docker and created optimized Dockerfiles for each service.
2. I provisioned the cloud infrastructure on AWS using Terraform, including networking components and the EKS cluster.
3. I configured a Jenkins CI/CD pipeline that automatically builds Docker images, runs code quality checks with SonarQube, pushes images to Amazon ECR, and deploys them to the EKS cluster.
4. I created Kubernetes manifests such as deployments, services, and ingress configurations to manage application deployment and networking.
5. I implemented monitoring using Prometheus and visualized metrics through Grafana/Datadog dashboards.

Result:
The project achieved automated end-to-end deployment from code commit to production deployment. The system became scalable and easier to maintain, deployments became faster, and monitoring improved incident detection, helping reduce mean time to resolution (MTTR).

---