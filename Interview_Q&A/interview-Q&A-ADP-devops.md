# ADP – DevOps / SRE Interview Preparation (Level 1)

This document contains detailed, structured answers (paragraph + pointwise format) for common DevOps/SRE interview questions covering CI/CD, Cloud, and Kubernetes.

---

## 1️⃣ How do you configure Jenkins from scratch?

### Paragraph Answer:

Configuring Jenkins from scratch involves installing Jenkins, setting up required plugins, configuring global tools, integrating version control systems, setting up credentials securely, and creating CI/CD pipelines. The goal is to automate build, test, and deployment workflows in a structured and secure way.

### Pointwise Answer:

1. Install Java (Jenkins dependency).
2. Install Jenkins (via package manager or Docker).
3. Access Jenkins UI (port 8080) and unlock using initial admin password.
4. Install suggested plugins (Git, Pipeline, Docker, etc.).
5. Configure Global Tools (JDK, Git, Maven).
6. Add credentials (GitHub, DockerHub, AWS) securely.
7. Create a pipeline job (Freestyle or Pipeline as Code).
8. Connect to Git repository.
9. Define Jenkinsfile with stages: Build → Test → Scan → Deploy.
10. Configure webhooks for automatic triggers.

---

## 2️⃣ How does Blue/Green deployment work?

### Paragraph Answer:

Blue/Green deployment is a release strategy where two identical environments (Blue and Green) exist. One environment (Blue) runs the current production version, while the other (Green) hosts the new version. After validation, traffic is switched from Blue to Green, ensuring seamless deployment with minimal risk.

### Pointwise Answer:

1. Maintain two identical environments: Blue (live) and Green (new).
2. Deploy new version to Green.
3. Perform testing and validation.
4. Switch traffic using Load Balancer or DNS.
5. Monitor application.
6. Rollback by redirecting traffic back to Blue if needed.

---

## 3️⃣ Is there any downtime in Blue/Green deployment?

### Paragraph Answer:

Ideally, Blue/Green deployment provides zero downtime because traffic switching happens instantly at the load balancer or DNS level. However, minimal downtime may occur due to DNS propagation delays or connection draining configurations.

### Pointwise Answer:

* No downtime during ideal load balancer switching.
* Possible minor delay due to:

  * DNS propagation
  * Database schema changes
  * Improper health checks
* Proper configuration ensures near-zero downtime.

---

## 4️⃣ A user is unable to access an S3 bucket. How do you troubleshoot?

### Paragraph Answer:

Troubleshooting S3 access issues involves verifying IAM permissions, bucket policies, ACLs, public access settings, region configuration, and encryption settings. The issue is usually related to permission misconfiguration.

### Pointwise Answer:

1. Check IAM user/role permissions.
2. Verify bucket policy.
3. Check Block Public Access settings.
4. Confirm correct region.
5. Check object-level permissions.
6. Review CloudTrail logs.
7. Verify encryption (KMS permissions).

---

## 5️⃣ How do you design a highly available Kubernetes cluster?

### Paragraph Answer:

A highly available Kubernetes cluster is designed by distributing control plane components across multiple nodes, using multiple worker nodes across availability zones, implementing auto-scaling, load balancing, and monitoring.

### Pointwise Answer:

1. Multiple control plane nodes.
2. Use etcd cluster with backup.
3. Worker nodes across multiple AZs.
4. Load Balancer for API server.
5. Horizontal Pod Autoscaler.
6. Liveness & Readiness probes.
7. Monitoring (Prometheus/Grafana).

---

## 6️⃣ How do you route 50% traffic to Team A and 50% to Team B in Kubernetes?

### Paragraph Answer:

Traffic splitting can be implemented using Kubernetes Services with multiple deployments or service mesh tools like Istio that allow weighted routing between versions.

### Pointwise Answer:

1. Create two deployments (Team A, Team B).
2. Use a Service targeting both.
3. Use Ingress with weighted rules.
4. Or use Istio VirtualService for 50-50 split.

---

## 7️⃣ What are your roles and responsibilities as a DevOps Engineer?

### Paragraph Answer:

As a DevOps Engineer, my responsibility is to automate infrastructure, manage CI/CD pipelines, containerize applications, monitor systems, and ensure secure and reliable deployments.

### Pointwise Answer:

* CI/CD pipeline creation.
* Infrastructure as Code (Terraform).
* Containerization (Docker).
* Kubernetes deployment.
* Monitoring & logging.
* Cloud management.
* Security integration (DevSecOps).

---

## 8️⃣ Where do you usually deploy applications—on-premises or cloud? Why?

### Paragraph Answer:

I usually deploy applications on the cloud because it provides scalability, high availability, cost optimization, and managed services that reduce operational overhead.

### Pointwise Answer:

* Scalability.
* High availability.
* Managed services.
* Global accessibility.
* Cost optimization.

---

## 9️⃣ Write Terraform code to create an S3 bucket.

### Paragraph Answer:

Terraform uses Infrastructure as Code to provision AWS resources declaratively.

### Pointwise Answer:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "my-unique-bucket-name"
  tags = {
    Name = "MyBucket"
  }
}
```

---

## 🔟 What recent Kubernetes production issue have you troubleshot?

### Paragraph Answer:

In production, I faced a CrashLoopBackOff issue due to incorrect environment variables. By checking pod logs and describing the pod, I identified configuration errors and redeployed the application.

### Pointwise Answer:

1. Observed pod status.
2. Used kubectl logs.
3. Used kubectl describe pod.
4. Identified misconfiguration.
5. Fixed and redeployed.

---

## 1️⃣1️⃣ Which command is used to check Kubernetes pod logs?

### Paragraph Answer:

The kubectl logs command is used to check logs of a running or failed pod.

### Pointwise Answer:

* kubectl logs <pod-name>
* kubectl logs -f <pod-name>
* kubectl logs <pod-name> -c <container-name>

---

## 1️⃣2️⃣ How does ArgoCD work?

### Paragraph Answer:

ArgoCD is a GitOps tool that continuously monitors a Git repository and synchronizes Kubernetes cluster state with the desired state defined in Git.

### Pointwise Answer:

1. Connects to Git repository.
2. Monitors manifests.
3. Compares desired vs live state.
4. Syncs automatically or manually.
5. Enables rollback.

---

## 1️⃣3️⃣ Explain an end-to-end CI/CD workflow using Jenkins.

### Paragraph Answer:

An end-to-end CI/CD workflow in Jenkins starts when code is pushed to Git. Jenkins triggers a pipeline that builds the application, runs tests, scans for vulnerabilities, builds a Docker image, pushes it to a registry, and deploys it to Kubernetes.

### Pointwise Answer:

1. Code push to Git.
2. Webhook triggers Jenkins.
3. Build stage.
4. Test stage.
5. Code quality scan.
6. Docker build & push.
7. Deployment to Kubernetes.
8. Monitoring.

---

## 1️⃣4️⃣ How do you troubleshoot a server that is not responding?

### Paragraph Answer:

Troubleshooting a non-responsive server involves checking network connectivity, CPU/memory usage, disk space, running services, logs, and firewall configurations.

### Pointwise Answer:

1. Ping server.
2. SSH access.
3. Check CPU/memory (top).
4. Check disk (df -h).
5. Check services.
6. Review logs.
7. Restart services.

---

## 1️⃣5️⃣ What automations have you implemented using Ansible?

### Paragraph Answer:

Using Ansible, I automated server provisioning, package installation, application deployment, Docker setup, and configuration management to reduce manual errors and ensure consistency.

### Pointwise Answer:

* Server setup automation.
* Package installation.
* Nginx configuration.
* Docker installation.
* User management.
* CI/CD server setup.

---

# ADP – DevOps / SRE Interview Preparation (Level 2)

This document contains detailed, structured answers (paragraph + pointwise format) for advanced DevOps/SRE interview questions covering Kubernetes, Terraform, and Observability.

---

## 1️⃣ How do you list Kubernetes pods based on their age?

### Paragraph Answer:

To list Kubernetes pods based on their age, we use kubectl commands with sorting options. Kubernetes allows sorting resources by creation timestamp. This helps in identifying older pods, recently created pods, or debugging restart scenarios.

### Pointwise Answer:

1. List pods sorted by creation time:
   kubectl get pods --sort-by=.metadata.creationTimestamp
2. Reverse order (newest first):
   kubectl get pods --sort-by=.metadata.creationTimestamp -o wide
3. Use custom columns if needed.
4. Combine with namespace flag: -n <namespace>

---

## 2️⃣ Explain the Terraform workflow.

### Paragraph Answer:

Terraform follows a declarative Infrastructure as Code workflow. We define infrastructure in .tf files, initialize providers, plan the changes, apply them to provision resources, and maintain state in a state file for tracking.

### Pointwise Answer:

1. Write configuration (.tf files).
2. terraform init – Initialize providers.
3. terraform validate – Validate syntax.
4. terraform plan – Preview changes.
5. terraform apply – Provision resources.
6. terraform destroy – Remove infrastructure.
7. Maintain terraform.tfstate (remote backend recommended).

---

## 3️⃣ If a Terraform project contains 100 resources, how do you apply only one resource?

### Paragraph Answer:

Terraform allows targeted execution using the -target flag. This ensures that only a specific resource is created or modified without affecting the rest of the infrastructure.

### Pointwise Answer:

1. Use:
   terraform apply -target=aws_instance.example
2. Useful for debugging.
3. Not recommended for regular workflow.
4. Always run full plan later to ensure consistency.

---

## 4️⃣ Explain the differences between Deployment, StatefulSet, and DaemonSet.

### Paragraph Answer:

Deployment is used for stateless applications, StatefulSet is used for stateful applications requiring stable identity and storage, and DaemonSet ensures one pod runs on every node.

### Pointwise Answer:

Deployment:

* Stateless apps.
* ReplicaSets.
* Rolling updates.

StatefulSet:

* Stable network identity.
* Persistent storage.
* Ordered scaling.

DaemonSet:

* One pod per node.
* Used for logging/monitoring agents.

---

## 5️⃣ What is a headless service in Kubernetes, and how does it work?

### Paragraph Answer:

A headless service is a Kubernetes service without a ClusterIP. It directly returns pod IPs via DNS instead of load balancing, commonly used with StatefulSets.

### Pointwise Answer:

1. Set clusterIP: None.
2. No load balancing.
3. DNS returns pod IPs.
4. Used in databases like MySQL, Cassandra.

---

## 6️⃣ What are Kubernetes probes? How many types are there?

### Paragraph Answer:

Kubernetes probes are health checks that determine container status. They ensure applications are running correctly and restart or stop traffic when needed.

### Pointwise Answer:

Three types:

1. Liveness Probe – Restarts container if unhealthy.
2. Readiness Probe – Controls traffic routing.
3. Startup Probe – Used for slow-starting apps.

---

## 7️⃣ Can we create a StatefulSet without Persistent Volumes and PVCs?

### Paragraph Answer:

Yes, technically we can create a StatefulSet without PVCs, but it defeats the purpose of StatefulSets since they are mainly used for stateful workloads requiring persistent storage.

### Pointwise Answer:

* Yes, but not recommended.
* No persistent storage.
* Data loss on restart.
* Better to use Deployment if no storage needed.

---

## 8️⃣ Can we attach PVs and PVCs to a Deployment?

### Paragraph Answer:

Yes, Deployments can use PVCs for persistent storage. However, storage behavior depends on the access mode of the volume (ReadWriteOnce, ReadWriteMany).

### Pointwise Answer:

* PVC can be mounted in Deployment.
* RWO limits to single node.
* RWX supports multi-node.
* Depends on storage class.

---

## 9️⃣ How do you provide S3 access to an EC2 instance securely?

### Paragraph Answer:

The most secure way is to attach an IAM Role to the EC2 instance with required S3 permissions instead of hardcoding access keys.

### Pointwise Answer:

1. Create IAM role with S3 policy.
2. Attach role to EC2 instance.
3. Avoid storing access keys.
4. Use least privilege principle.

---

## 🔟 What is Backstage, and how does it work?

### Paragraph Answer:

Backstage is an open-source developer portal platform that centralizes infrastructure, services, documentation, and CI/CD pipelines into one interface.

### Pointwise Answer:

1. Service catalog.
2. Plugin-based architecture.
3. Integrates with Kubernetes, CI/CD, Git.
4. Improves developer productivity.

---

## 1️⃣1️⃣ What is APM (Application Performance Monitoring)?

### Paragraph Answer:

APM is a monitoring practice that tracks application performance, response time, errors, and transactions to ensure optimal performance.

### Pointwise Answer:

* Monitors response time.
* Tracks errors.
* Distributed tracing.
* Root cause analysis.
* Tools: New Relic, Datadog, Dynatrace.

---

## 1️⃣2️⃣ Have you worked on observability, monitoring, and distributed tracing? Explain.

### Paragraph Answer:

Yes, I have implemented monitoring using Prometheus and Grafana for metrics, centralized logging using ELK stack, and distributed tracing using tools like Jaeger to track microservices communication and identify bottlenecks.

### Pointwise Answer:

1. Metrics: Prometheus.
2. Dashboards: Grafana.
3. Logging: ELK stack.
4. Tracing: Jaeger.
5. Alerts: Alertmanager.

---

📌 This README can be used for advanced interview revision

