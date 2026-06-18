# TCS DevOps Interview Questions with their Answers

## Round 1 – Technical Interview

### 1. Can you explain your day-to-day responsibilities in your current DevOps role?

**Type: Project Question**
**Problem → Tools Used → Implementation → Result**

**Problem:** Development teams needed a reliable way to build, test, and deploy applications quickly while maintaining stable infrastructure.

**Tools Used:** Jenkins, Docker, Kubernetes, Terraform, Git, Prometheus, Grafana, AWS.

**Implementation:** My day‑to‑day responsibilities include managing CI/CD pipelines, maintaining Kubernetes clusters, provisioning infrastructure using Terraform, and monitoring systems using Prometheus and Grafana. I also collaborate with developers to containerize applications with Docker and deploy them to Kubernetes. I review pipeline failures, optimize deployments, manage secrets, and ensure infrastructure security and reliability.

**Result:** This helped automate deployments, reduce manual work, improve release frequency, and maintain high system availability.

---

### 2. What is the difference between Kubernetes Ingress and Istio?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**

* **Ingress** is a Kubernetes API object that manages external access to services, typically HTTP/HTTPS routing.
* **Istio** is a service mesh that provides advanced traffic management, security, and observability for microservices.

**How it Works:**

* **Ingress** uses an Ingress controller (like NGINX) to route external traffic to internal Kubernetes services.
* **Istio** works at the service mesh layer using sidecar proxies (Envoy) to control service‑to‑service communication, including retries, traffic splitting, and security policies.

**Example:**
Ingress might route traffic from `example.com/api` to a backend service, while Istio could split traffic 90/10 between two versions of a service during a canary deployment.

---

### 3. Have you configured secrets or handled Terraform state files? Explain your experience.

**Type: Project Question**
**Problem → Tools Used → Implementation → Result**

**Problem:** Sensitive credentials like API keys, database passwords, and infrastructure states must be securely stored and managed.

**Tools Used:** Kubernetes Secrets, HashiCorp Vault, Terraform Remote State, AWS S3, DynamoDB locking.

**Implementation:** I configured Kubernetes Secrets to securely store application credentials and integrated them with deployments through environment variables. For Terraform, I configured remote state storage in AWS S3 with state locking using DynamoDB to prevent concurrent modifications. Access was controlled using IAM policies.

**Result:** This improved security, prevented accidental exposure of credentials, and ensured consistent infrastructure provisioning.

---

### 4. If a node has 10 pods running, where 7 are healthy and 3 are failing, how would you troubleshoot and fix the issue?

**Type: Troubleshooting Question**
**Identify → Diagnose → Fix → Prevent**

**Identify:** Check which pods are failing using:

`kubectl get pods -o wide`

**Diagnose:**

* Inspect logs: `kubectl logs <pod-name>`
* Describe pod events: `kubectl describe pod <pod-name>`
* Check node health: `kubectl describe node <node-name>`
* Verify resource usage (CPU/Memory)

**Fix:**

* If it's a resource issue, scale resources or adjust limits.
* If image/config issue, update deployment.
* If node issue, cordon and drain the node.

**Prevent:**
Implement proper resource requests/limits, health probes, monitoring alerts, and autoscaling policies.

---

### 5. What specific actions or implementations have you done to make your infrastructure more reliable?

**Type: Project Question**
**Problem → Tools Used → Implementation → Result**

**Problem:** Infrastructure needed higher reliability and minimal downtime during deployments.

**Tools Used:** Kubernetes, Terraform, AWS Auto Scaling, Load Balancers, Prometheus, Grafana.

**Implementation:** I implemented Infrastructure as Code using Terraform, configured health checks and load balancers, added monitoring dashboards and alerts with Prometheus and Grafana, and enabled auto‑scaling policies in Kubernetes.

**Result:** This reduced downtime, improved observability, and ensured automatic recovery during traffic spikes or failures.

---

### 6. If the CI stage succeeds but the CD stage fails in Jenkins, how would you debug and resolve it?

**Type: Troubleshooting Question**
**Identify → Diagnose → Fix → Prevent**

**Identify:** Confirm that CI stages (build/tests) passed but deployment failed.

**Diagnose:**

* Check Jenkins pipeline logs
* Verify deployment scripts
* Validate credentials or environment variables
* Check connectivity to Kubernetes or servers

**Fix:**

* Correct deployment configuration
* Fix authentication issues
* Update pipeline scripts or permissions

**Prevent:**
Add better pipeline logging, validation checks, and environment configuration verification before deployment.

---

### 7. Which tools have you used to build a complete CI/CD pipeline?

**Type: Project Question**
**Problem → Tools Used → Implementation → Result**

**Problem:** Automate application build, testing, and deployment.

**Tools Used:** Git, Jenkins, Docker, Kubernetes, Helm, SonarQube, Nexus/Artifactory.

**Implementation:** Developers push code to Git repositories which triggers Jenkins pipelines. Jenkins builds Docker images, runs tests, pushes images to a registry, and deploys them to Kubernetes using Helm charts.

**Result:** This enabled automated and consistent deployments, faster release cycles, and reduced manual errors.

---

### 8. If you encounter a secret-related error in your application or pipeline, how would you troubleshoot it?

**Type: Troubleshooting Question**
**Identify → Diagnose → Fix → Prevent**

**Identify:** Error related to authentication, missing environment variables, or secret access.

**Diagnose:**

* Check secret configuration in Kubernetes or pipeline
* Verify secret names and keys
* Check permissions or RBAC policies
* Review logs

**Fix:**

* Update incorrect secret references
* Recreate or update the secret
* Ensure proper access permissions

**Prevent:**
Use secret management tools like Vault, apply RBAC policies, and validate secrets during pipeline execution.

---

### 9. What is the difference between Horizontal Scaling and Vertical Scaling?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**

* **Vertical Scaling:** Increasing resources (CPU/RAM) of a single machine.
* **Horizontal Scaling:** Adding more machines or instances to distribute the load.

**How it Works:**
Vertical scaling upgrades existing hardware, while horizontal scaling distributes traffic across multiple instances using load balancing.

**Example:**
Upgrading a server from 4GB RAM to 16GB RAM is vertical scaling, while adding multiple application servers behind a load balancer is horizontal scaling.

---

### 10. What is etcd, and why is it important in Kubernetes?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**
etcd is a distributed key‑value store used by Kubernetes to store cluster configuration and state data.

**How it Works:**
The Kubernetes control plane stores all cluster information such as nodes, pods, deployments, and configurations inside etcd. It ensures data consistency across the cluster.

**Example:**
When a new pod is created using `kubectl apply`, the configuration is stored in etcd and the scheduler reads that data to schedule the pod.

---

## Round 2 – Managerial / SRE Round

### 1. What is an Error Budget, and why is it important?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**
An error budget represents the acceptable level of system failure based on the service reliability target.

**How it Works:**
If a service has a 99.9% availability SLO, the remaining 0.1% downtime becomes the error budget. Teams can use that budget to deploy changes or experiment without exceeding reliability limits.

**Example:**
If monthly uptime target is 99.9%, around 43 minutes of downtime is allowed. If that limit is exceeded, deployments may be paused to improve reliability.

---

### 2. Can you explain SLI, SLO, and SLA with real-world examples?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**

* **SLI (Service Level Indicator):** A metric used to measure service performance.
* **SLO (Service Level Objective):** Target value for the SLI.
* **SLA (Service Level Agreement):** Contract defining service guarantees and penalties.

**How it Works:**
Organizations monitor SLIs such as latency or availability and define SLO targets. SLAs formalize commitments to customers.

**Example:**
A cloud provider may define:

* SLI: API uptime
* SLO: 99.9% uptime
* SLA: If uptime drops below 99.9%, customers receive service credits.

---

### 3. What is the default Prometheus port number?

**Type: Technical Concept**

**Definition:**
Prometheus is an open‑source monitoring and alerting system used to collect metrics from applications and infrastructure.

**How it Works:**
Prometheus scrapes metrics from configured targets at regular intervals and stores them in its time‑series database.

**Example:**
The default web UI and API for Prometheus run on **port 9090**.

---

### 4. What is toil, and how do you reduce it in DevOps/SRE teams?

**Type: Technical Concept**
**Definition → How it Works → Example**

**Definition:**
Toil refers to repetitive, manual operational tasks that do not provide long‑term value.

**How it Works:**
Examples include manually restarting services, repetitive deployments, or manual incident responses.

**Example:**
To reduce toil, teams automate tasks using scripts, CI/CD pipelines, infrastructure as code, and monitoring alerts.

---

### 5. Why is horizontal scaling generally preferred over vertical scaling?

**Type: Technical Concept**

**Definition:**
Horizontal scaling adds more machines or instances instead of increasing resources on a single machine.

**How it Works:**
Load balancers distribute traffic across multiple instances, improving performance and reliability.

**Example:**
If traffic increases, Kubernetes can automatically scale pods horizontally using the Horizontal Pod Autoscaler instead of upgrading a single server.

---

### 6. What are the Golden Signals of Monitoring?

**Type: Technical Concept**

**Definition:**
The Golden Signals are four key metrics used in SRE monitoring.

**How it Works:**
They help teams quickly detect performance and reliability issues.

**Example:**
The four golden signals are:

1. **Latency** – Time taken to process a request
2. **Traffic** – Number of requests
3. **Errors** – Rate of failed requests
4. **Saturation** – Resource utilization such as CPU or memory

---