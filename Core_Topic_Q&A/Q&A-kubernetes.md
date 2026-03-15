# Kubernetes Interview Questions & Answers (DevOps Engineer – Fresher to Intermediate)

This README contains **60 Kubernetes interview questions with medium-length answers**, structured for **DevOps Engineer (Fresher–Intermediate)** roles. The content is suitable for quick revision, interview preparation, and conceptual clarity.

---

## Questions 1–30: Kubernetes Fundamentals

### 1. What is Kubernetes and why is it used?

Kubernetes is an open-source container orchestration platform used to automate deployment, scaling, and management of containerized applications. It helps maintain application availability, handles container failures, enables self-healing, and supports rolling updates without downtime.

---

### 2. What are the main components of Kubernetes architecture?

Kubernetes has control plane components (API Server, Scheduler, Controller Manager, etcd) and worker node components (kubelet, kube-proxy, container runtime). The control plane manages the cluster, while worker nodes run application workloads.

---

### 3. What is a Pod in Kubernetes?

A Pod is the smallest deployable unit in Kubernetes. It can contain one or more tightly coupled containers that share the same network namespace, IP address, and storage volumes. Pods are usually managed by higher-level objects like Deployments.

---

### 4. What is the difference between a Pod and a Deployment?

A Pod runs one or more containers but has no self-healing or scaling capability. A Deployment manages Pods by ensuring desired replicas are running, supports rolling updates, rollbacks, and automatically recreates Pods if they fail.

---

### 5. What is a Namespace in Kubernetes?

A Namespace is a logical isolation mechanism used to organize cluster resources. It allows multiple teams or applications to share the same cluster without conflict and helps apply RBAC, resource quotas, and policies per namespace.

---

### 6. What is a Service in Kubernetes?

A Service provides a stable network endpoint to access Pods. Since Pods are ephemeral, Services ensure consistent communication by routing traffic to healthy Pods using labels and selectors.

---

### 7. Types of Kubernetes Services?

* **ClusterIP** – Internal access only
* **NodePort** – Exposes service via node IP and port
* **LoadBalancer** – Integrates with cloud load balancers
* **ExternalName** – Maps service to an external DNS name

---

### 8. What is kubelet?

kubelet is an agent running on each worker node. It communicates with the API Server and ensures that containers defined in Pod specs are running and healthy on that node.

---

### 9. What is etcd?

etcd is a distributed key-value store used by Kubernetes to store cluster state, configuration data, and metadata. It is critical for cluster consistency and should be backed up regularly.

---

### 10. What is a ReplicaSet?

A ReplicaSet ensures that a specified number of identical Pods are running at all times. It automatically creates or deletes Pods to match the desired replica count and is typically managed by Deployments.

---

### 11. What is a ConfigMap?

A ConfigMap stores non-sensitive configuration data like environment variables, config files, or command-line arguments. It allows separating configuration from application code.

---

### 12. What is a Secret?

A Secret is used to store sensitive data such as passwords, API keys, and tokens. Secrets are base64-encoded and can be mounted as volumes or injected as environment variables.

---

### 13. What is the difference between ConfigMap and Secret?

ConfigMaps store non-sensitive data, while Secrets store sensitive information. Secrets provide better access control and security practices compared to ConfigMaps.

---

### 14. What is a StatefulSet?

A StatefulSet is used for stateful applications like databases. It provides stable network identities, persistent storage, and ordered deployment and scaling of Pods.

---

### 15. What is a DaemonSet?

A DaemonSet ensures that a specific Pod runs on all (or selected) nodes in the cluster. Common use cases include logging agents, monitoring agents, and node-level services.

---

### 16. What is Horizontal Pod Autoscaler (HPA)?

HPA automatically scales the number of Pod replicas based on metrics such as CPU or memory usage. It helps maintain application performance during varying workloads.

---

### 17. What is a Volume in Kubernetes?

A Volume provides persistent or shared storage to Pods. Unlike container storage, volumes persist even if containers restart. Examples include emptyDir, hostPath, and PersistentVolumes.

---

### 18. What is a PersistentVolume (PV)?

A PersistentVolume is a cluster-wide storage resource provisioned by an administrator or dynamically via storage classes. It abstracts underlying storage like EBS, NFS, or Azure Disk.

---

### 19. What is a PersistentVolumeClaim (PVC)?

A PVC is a request for storage by a user. It defines size, access mode, and storage class. Kubernetes binds a PVC to a suitable PV automatically.

---

### 20. What is Ingress?

Ingress manages external HTTP/HTTPS access to services within a cluster. It supports features like path-based routing, SSL termination, and virtual hosts using an Ingress Controller.

---

### 21. What is a rolling update in Kubernetes?

A rolling update gradually replaces old Pods with new ones without downtime. Kubernetes ensures a minimum number of Pods are always available during updates.

---

### 22. What is a rollback in Kubernetes?

Rollback allows reverting a Deployment to a previous stable version if a new release fails. Kubernetes maintains revision history for this purpose.

---

### 23. What is RBAC in Kubernetes?

RBAC (Role-Based Access Control) controls access to Kubernetes resources using roles and permissions. It ensures users and services have only the permissions they need.

---

### 24. What is a Node in Kubernetes?

A Node is a worker machine (VM or physical) that runs Pods. Each node includes kubelet, kube-proxy, and a container runtime.

---

### 25. What is kubectl?

kubectl is the command-line tool used to interact with Kubernetes clusters. It allows users to create, inspect, update, and delete cluster resources.

---

### 26. What is a Label and Selector?

Labels are key-value pairs attached to resources. Selectors are used to filter and group resources based on labels, enabling service discovery and management.

---

### 27. What is CrashLoopBackOff?

CrashLoopBackOff occurs when a container repeatedly fails and restarts. It usually indicates application errors, misconfigurations, or missing dependencies.

---

### 28. Difference between Docker Swarm and Kubernetes?

Docker Swarm is simpler and easier to set up, while Kubernetes is more powerful and scalable with advanced features like autoscaling, self-healing, and ecosystem support.

---

### 29. What is a Job in Kubernetes?

A Job runs a Pod to completion for batch or one-time tasks like database migration or backups. It retries failed Pods until the task completes successfully.

---

### 30. What is a CronJob?

A CronJob schedules Jobs to run at specific times or intervals, similar to Linux cron. It is commonly used for periodic tasks like backups and reports.

---

## Questions 31–60: Intermediate to Advanced Kubernetes

### 31. What is the Kubernetes API Server?

The API Server is the central management component of Kubernetes. All cluster operations—such as creating Pods, Deployments, or Services—are handled through the API Server, which validates and processes requests and updates etcd.

---

### 32. What is kube-proxy?

kube-proxy is a network component that runs on each node. It maintains network rules to allow communication to Services and load-balances traffic to backend Pods using iptables or IPVS.

---

### 33. What is a ServiceAccount?

A ServiceAccount provides an identity for Pods to interact with the Kubernetes API. It is commonly used by applications or controllers that need to access cluster resources securely.

---

### 34. What is a Liveness Probe?

A liveness probe checks if a container is still running properly. If the probe fails, Kubernetes restarts the container automatically to recover from failure.

---

### 35. What is a Readiness Probe?

A readiness probe determines if a container is ready to accept traffic. If it fails, the Pod is temporarily removed from Service endpoints without restarting the container.

---

### 36. Difference between Liveness and Readiness probes?

Liveness probes restart unhealthy containers, while readiness probes control traffic routing. Readiness failures do not restart containers; they only stop traffic.

---

### 37. What is a Startup Probe?

A startup probe is used for slow-starting containers. It disables liveness and readiness checks until the application has fully started, preventing premature restarts.

---

### 38. What is a Deployment strategy in Kubernetes?

Deployment strategies define how updates are applied. Common strategies include RollingUpdate (default) and Recreate. RollingUpdate ensures zero downtime by gradually replacing Pods.

---

### 39. What is Recreate deployment strategy?

In the Recreate strategy, all existing Pods are terminated before new Pods are created. This causes downtime but is sometimes required for applications that cannot run multiple versions.

---

### 40. What is Helm?

Helm is a Kubernetes package manager that simplifies application deployment using charts. It allows templating, versioning, and easy rollback of Kubernetes manifests.

---

### 41. What is a Helm Chart?

A Helm Chart is a collection of Kubernetes YAML files bundled together. It defines application resources, values, and templates for reusable and configurable deployments.

---

### 42. What is the purpose of a StorageClass?

A StorageClass defines storage provisioning parameters for dynamic volume creation. It allows users to request storage without knowing the underlying storage details.

---

### 43. What is dynamic volume provisioning?

Dynamic provisioning automatically creates PersistentVolumes when a PVC is requested. It eliminates the need for manual PV creation.

---

### 44. What is a Taint in Kubernetes?

A taint prevents Pods from being scheduled on a node unless they tolerate the taint. It is used to reserve nodes for special workloads.

---

### 45. What is a Toleration?

A toleration allows a Pod to be scheduled on a tainted node. Tolerations work together with taints to control Pod placement.

---

### 46. What is Node Affinity?

Node affinity defines rules that control which nodes a Pod can be scheduled on based on node labels. It is more expressive than nodeSelector.

---

### 47. What is Pod Affinity and Anti-Affinity?

Pod affinity ensures Pods are scheduled together, while anti-affinity ensures Pods are separated across nodes. These are useful for high availability and performance.

---

### 48. What is a Network Policy?

A NetworkPolicy controls traffic flow between Pods and namespaces. It improves security by allowing or denying ingress and egress traffic based on rules.

---

### 49. What happens when a node goes down?

Kubernetes marks the node as NotReady, evicts Pods after a timeout, and reschedules them on healthy nodes if managed by a controller like Deployment.

---

### 50. What is a Resource Request and Limit?

Requests define minimum resources a container needs, while limits define maximum resources it can use. They help with scheduling and resource control.

---

### 51. What is OOMKilled?

OOMKilled occurs when a container exceeds its memory limit. Kubernetes terminates the container to protect node stability.

---

### 52. What is the Kubernetes Scheduler?

The scheduler assigns Pods to nodes based on resource availability, constraints, affinity rules, and policies.

---

### 53. What is a Custom Resource Definition (CRD)?

A CRD allows users to define custom resources in Kubernetes. It extends Kubernetes functionality without modifying core components.

---

### 54. What is an Operator in Kubernetes?

An Operator is a custom controller that manages complex applications using CRDs. It automates tasks like deployment, scaling, and recovery.

---

### 55. What is kube-controller-manager?

The controller manager runs background controllers that monitor cluster state and ensure the desired state matches the actual state.

---

### 56. What is a Sidecar container?

A sidecar container runs alongside the main container in the same Pod and provides supporting functionality like logging, monitoring, or proxying.

---

### 57. What is Init Container?

Init containers run before the main application containers start. They are used for setup tasks like waiting for dependencies or initializing data.

---

### 58. What is Blue-Green deployment in Kubernetes?

Blue-Green deployment maintains two environments (blue and green). Traffic is switched from the old version to the new version once it is verified, enabling zero downtime.

---

### 59. What is Canary deployment?

Canary deployment releases a new version to a small subset of users first. If successful, it is gradually rolled out to all users, reducing risk.

---

### 60. How do you troubleshoot a failing Pod?

Start by checking Pod status using `kubectl describe pod`, view logs with `kubectl logs`, check events, resource limits, probes, and node health to identify issues.

---

## 📌 Usage

* Ideal for **Kubernetes interviews**
* Useful for **DevOps revision notes**

---
