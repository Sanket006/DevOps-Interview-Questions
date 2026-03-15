# ☸️ 25 Advanced Kubernetes Interview Questions (EKS + Production Level)

## **Kubernetes Architecture – Interview Answers**

### 1. What are the core components of the Kubernetes control plane?

Definition:
The Kubernetes control plane is responsible for managing the overall state of the cluster and making global decisions such as scheduling, scaling, and responding to events.

How it Works:
The control plane components continuously monitor the cluster state and ensure the desired state matches the actual state.

Core components include:

* API Server
* etcd
* Scheduler
* Controller Manager

Example:
If a deployment specifies 3 replicas but only 2 are running, the controller manager detects the mismatch and creates another pod.

---

### 2. What is the difference between etcd, API Server, Controller Manager, and Scheduler?

Definition:
These components form the Kubernetes control plane and work together to manage cluster operations.

How it Works:

API Server

* Entry point for all Kubernetes requests
* Exposes REST API
* Validates and processes requests

etcd

* Distributed key-value store
* Stores cluster state and configuration

Scheduler

* Assigns pods to nodes based on resource availability

Controller Manager

* Runs controllers that maintain desired cluster state

Example:
When a new pod is created:

1. API server receives the request
2. Object stored in etcd
3. Scheduler selects the best node
4. Controller ensures pod runs successfully

---

### 3. How does Kubernetes scheduling work?

Definition:
Kubernetes scheduling is the process of assigning newly created pods to appropriate nodes in the cluster.

How it Works:

Step 1: Filtering (Predicates)
Scheduler filters nodes that do not meet requirements such as CPU, memory, taints, and node selectors.

Step 2: Scoring (Priorities)
Remaining nodes are scored based on resource availability, affinity rules, and policies.

Step 3: Binding
The scheduler assigns the pod to the highest scoring node.

Example:
If a pod requires 2 CPU and 4GB RAM, the scheduler only selects nodes that have sufficient resources.

---

## **Kubernetes Pods & Workloads – Interview Answers**

### 1. Difference between Pod, ReplicaSet, Deployment, StatefulSet, and DaemonSet

Definition:
These are Kubernetes workload resources used to run and manage containers in a cluster.

How it Works:

Pod

* The smallest deployable unit in Kubernetes
* Contains one or more containers sharing network and storage

ReplicaSet

* Ensures a specified number of pod replicas are always running

Deployment

* Manages ReplicaSets
* Provides rolling updates and rollbacks

StatefulSet

* Manages stateful applications
* Provides stable network identity and persistent storage

DaemonSet

* Ensures a pod runs on every node (or selected nodes)

Example:

* Pod: Single container running a simple app
* ReplicaSet: Ensures 3 replicas of a pod
* Deployment: Rolling update of a web application
* StatefulSet: Running databases like MySQL or Cassandra
* DaemonSet: Running monitoring agents like Fluentd or Prometheus node exporters

---

### 2. When should you use a StatefulSet instead of Deployment?

Definition:
StatefulSet is used when applications require persistent identity and stable storage.

How it Works:
StatefulSet provides:

* Stable pod names
* Ordered pod creation and deletion
* Persistent volumes attached to specific pods

Example:
Use StatefulSet when running databases such as MySQL, PostgreSQL, MongoDB, or Kafka clusters where each instance must maintain its own data and identity.

---

### 3. What are init containers?

Definition:
Init containers are special containers that run before the main application containers in a pod.

How it Works:

* Init containers run sequentially
* Each must complete successfully before the next starts
* The main container starts only after all init containers finish

Example:
An init container might:

* Wait for a database to become available
* Download configuration files
* Prepare required directories

---

### 4. What are sidecar containers?

Definition:
A sidecar container is a helper container that runs alongside the main application container in the same pod.

How it Works:
Sidecar containers share:

* Network namespace
* Storage volumes

They extend functionality without modifying the main application.

Example:
Common sidecar use cases:

* Log forwarding using Fluentd
* Service mesh proxies like Envoy in Istio
* Monitoring agents

---

## **Kubernetes Networking – Interview Answers**

### 1. Explain Kubernetes Networking Model

Definition:
Kubernetes networking model defines how pods communicate with each other and with services inside and outside the cluster.

How it Works:
The Kubernetes networking model follows these fundamental rules:

* Every pod gets its own IP address
* Pods can communicate with each other without NAT
* Nodes can communicate with pods
* Containers within a pod share the same network namespace

Kubernetes uses Container Network Interface (CNI) plugins such as Calico, Flannel, or Cilium to implement networking.

Example:
If Pod A in Node 1 wants to communicate with Pod B in Node 2, it can directly reach Pod B using its pod IP without NAT.

---

### 2. Difference between ClusterIP, NodePort, and LoadBalancer

Definition:
These are Kubernetes service types used to expose applications running inside the cluster.

How it Works:

ClusterIP

* Default service type
* Exposes service internally within the cluster
* Accessible only by other pods/services

NodePort

* Exposes the service on a static port on each node
* Accessible using NodeIP:NodePort

LoadBalancer

* Exposes the service externally using a cloud provider load balancer
* Automatically provisions external load balancer in cloud environments like AWS EKS

Example:
ClusterIP: Internal communication between microservices
NodePort: Testing service externally
LoadBalancer: Public-facing production application

---

### 3. What is an Ingress Controller?

Definition:
An Ingress Controller is a Kubernetes component that manages external HTTP/HTTPS access to services in the cluster.

How it Works:

* Ingress resource defines routing rules
* Ingress Controller implements those rules
* Routes traffic based on hostnames or paths

Common controllers include NGINX Ingress Controller, AWS ALB Ingress Controller, and Traefik.

Example:
example.com/api → backend service
example.com/web → frontend service

---

### 4. How does Service Discovery work in Kubernetes?

Definition:
Service discovery allows pods to find and communicate with other services using stable DNS names.

How it Works:
Kubernetes uses CoreDNS for service discovery.

Steps:

1. Service is created
2. DNS entry is registered in CoreDNS
3. Pods query the service name
4. Traffic is routed to available pod endpoints

Example:
A service named "backend" can be accessed as:
backend.default.svc.cluster.local

Applications usually just use:
[http://backend](http://backend)

---

## **Kubernetes Scaling & High Availability – Interview Answers**

### 1. How does Horizontal Pod Autoscaler (HPA) work?

Definition:
Horizontal Pod Autoscaler automatically scales the number of pod replicas based on observed metrics such as CPU utilization, memory usage, or custom metrics.

How it Works:

* HPA continuously monitors metrics from Metrics Server or Prometheus.
* It compares current metrics with the target value defined in the HPA configuration.
* If usage exceeds the threshold, HPA increases the number of replicas.
* If usage decreases, HPA scales down the replicas.

Example:
If CPU utilization crosses 70%, HPA may scale pods from 3 replicas to 6 replicas to handle increased traffic.

---

### 2. What is Vertical Pod Autoscaler (VPA)?

Definition:
Vertical Pod Autoscaler automatically adjusts the CPU and memory requests/limits of containers in a pod.

How it Works:

* VPA monitors resource usage of pods over time.
* It recommends or automatically updates CPU and memory values.
* In some modes, it may restart pods to apply the new resource configuration.

Example:
If an application constantly uses more memory than requested, VPA may increase memory requests from 512MB to 1GB.

---

### 3. What happens when a node fails in Kubernetes?

Definition:
When a node fails, Kubernetes detects the failure and reschedules the affected pods to healthy nodes.

How it Works:

* Node controller detects node failure via heartbeat checks.
* Node status becomes "NotReady".
* Pods running on that node are marked as failed.
* ReplicaSets or Deployments create new pods on healthy nodes.

Example:
If a node hosting 4 pods crashes, Kubernetes automatically recreates those pods on other available nodes.

---

### 4. How do you achieve high availability in Kubernetes clusters?

Definition:
High availability ensures that applications and cluster components remain operational even if failures occur.

How it Works:
Common strategies include:

* Running multiple control plane nodes
* Distributing worker nodes across multiple availability zones
* Using Deployments with multiple replicas
* Using LoadBalancers to distribute traffic
* Implementing health checks and auto-healing

Example:
In AWS EKS, worker nodes can run across multiple availability zones, ensuring that if one zone fails, applications continue running in other zones.

---

## **Kubernetes Security – Interview Answers**

### 1. What is RBAC in Kubernetes?

Definition:
Role-Based Access Control (RBAC) is a security mechanism used to control who can access Kubernetes resources and what actions they can perform.

How it Works:
RBAC works by defining permissions through:

* Role: Defines permissions within a namespace
* ClusterRole: Defines permissions across the entire cluster
* RoleBinding: Assigns a Role to a user, group, or service account
* ClusterRoleBinding: Assigns a ClusterRole to users across the cluster

Example:
A developer may be granted permission to read pods but not delete them.

---

### 2. What is Pod Security Policy / Pod Security Standards?

Definition:
Pod Security Policy (PSP) was a Kubernetes feature used to control security-sensitive aspects of pod specifications. It is now deprecated and replaced by Pod Security Standards.

How it Works:
Pod Security Standards define three levels of security policies:

* Privileged: Least restrictive
* Baseline: Prevents known privilege escalations
* Restricted: Strict security rules

These policies enforce rules such as:

* Prevent running containers as root
* Restrict host network usage
* Limit privileged containers

Example:
A restricted policy can prevent containers from running as the root user.

---

### 3. How do you manage secrets securely in Kubernetes?

Definition:
Kubernetes Secrets are used to store sensitive data such as passwords, API keys, and tokens.

How it Works:
Secrets can be stored and accessed in several ways:

* Environment variables
* Mounted as volumes in pods

Best practices for secure management include:

* Encrypt secrets at rest using etcd encryption
* Use external secret management tools like AWS Secrets Manager or HashiCorp Vault
* Restrict access using RBAC

Example:
A database password can be stored in a Kubernetes Secret and mounted inside a pod as an environment variable.

---

## **Kubernetes Storage – Interview Answers**

### 1. Difference between Persistent Volume (PV) and Persistent Volume Claim (PVC)

Definition:
Persistent storage in Kubernetes is managed using Persistent Volumes (PV) and Persistent Volume Claims (PVC).

How it Works:

Persistent Volume (PV)

* A PV is a piece of storage in the cluster provisioned by an administrator or dynamically using a storage class.
* It represents actual storage resources such as AWS EBS, NFS, or cloud disks.

Persistent Volume Claim (PVC)

* A PVC is a request for storage made by a user or application.
* It specifies storage size, access mode, and storage class requirements.

Relationship:

* PVC requests storage
* PV provides storage
* Kubernetes binds PVC to a suitable PV

Example:
An application requests 10GB storage using a PVC. Kubernetes binds it to an available PV that satisfies the request.

---

### 2. What is a StorageClass?

Definition:
A StorageClass defines how dynamic storage should be provisioned in Kubernetes.

How it Works:

* StorageClass defines the storage provisioner (e.g., AWS EBS, Azure Disk, GCP Persistent Disk).
* It also defines parameters like disk type, IOPS, and replication.
* When a PVC references a StorageClass, Kubernetes automatically provisions a PV.

Example:
In AWS EKS, a StorageClass may provision an EBS volume automatically when a PVC is created.

---

## **Amazon EKS – Kubernetes Interview Answers**

### 1. What is Amazon EKS?

Definition:
Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service provided by AWS that allows you to run Kubernetes clusters without managing the control plane infrastructure.

How it Works:

* AWS manages the Kubernetes control plane (API Server, etcd, scheduler, controller manager).
* The control plane runs across multiple Availability Zones for high availability.
* Users manage worker nodes where application containers run.

Example:
A company can deploy microservices on Kubernetes using EKS without worrying about maintaining the Kubernetes control plane.

---

### 2. Difference between EKS and Self‑Managed Kubernetes

Definition:
Both run Kubernetes workloads, but differ in how infrastructure and control plane are managed.

EKS

* Managed control plane by AWS
* Automatic patching and upgrades
* Integrated with AWS services (IAM, CloudWatch, VPC)

Self‑Managed Kubernetes

* User installs and manages Kubernetes on EC2 or on‑premise
* Responsible for upgrades, scaling, and maintenance

Example:
Using kubeadm on EC2 to create a cluster is self‑managed, while using EKS means AWS manages the control plane.

---

### 3. How does EKS manage worker nodes?

Definition:
Worker nodes are EC2 instances that run Kubernetes pods and communicate with the EKS control plane.

How it Works:

* Worker nodes join the cluster using the AWS IAM authentication system.
* Nodes run kubelet and container runtime.
* EKS supports:

  * Managed Node Groups
  * Self‑managed nodes
  * AWS Fargate (serverless pods)

Example:
A managed node group can automatically scale EC2 instances based on demand.

---

### 4. What is IAM Roles for Service Accounts (IRSA)?

Definition:
IRSA allows Kubernetes service accounts to assume AWS IAM roles securely without storing AWS credentials inside containers.

How it Works:

* A Kubernetes service account is associated with an IAM role.
* Pods using that service account automatically get AWS permissions.
* Uses OIDC identity provider for authentication.

Example:
A pod can access an S3 bucket securely using an IAM role attached to its service account.

---

### 5. How do you monitor EKS clusters in production?

Definition:
Monitoring ensures cluster health, performance, and reliability in production environments.

How it Works:
Common monitoring tools include:

* Prometheus for metrics collection
* Grafana for visualization
* AWS CloudWatch for logs and metrics
* AWS Container Insights for cluster monitoring

Example:
Prometheus collects pod CPU and memory metrics, and Grafana dashboards display cluster health in real time.

---

