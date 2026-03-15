# 🚨 10 DevOps Troubleshooting Questions (Real Production Scenarios)

---

## 1️⃣ Kubernetes Pod Keeps Restarting (CrashLoopBackOff)

**Scenario:**
Your Kubernetes pod is stuck in CrashLoopBackOff.

**Interview question:**
How will you debug it?

### Identify

First, identify that the pod is failing and restarting continuously.

Commands:

* kubectl get pods

Look for pod status such as CrashLoopBackOff or Error.

### Diagnose

Next, investigate the reason for the crash.

Check logs:

* kubectl logs <pod-name>

Describe the pod to view detailed information:

* kubectl describe pod <pod-name>

Check Kubernetes events for scheduling or container errors:

* kubectl get events

Common causes:

* Application crash
* Missing environment variables
* Incorrect configuration
* Image issues
* Port conflicts

### Fix

Apply a fix depending on the root cause.

Examples:

* Correct environment variables
* Fix application bug
* Update configuration
* Rebuild and redeploy Docker image

Redeploy the pod after fixing the issue.

### Prevent

To prevent future issues:

* Add readiness and liveness probes
* Use proper resource limits
* Implement monitoring and alerts
* Validate configuration in CI/CD pipeline

---

## 2️⃣ Website Suddenly Becomes Slow

**Scenario:**
Production website response time increases from 200ms to 5 seconds.

**Interview question:**
How will you investigate?

### Identify

First confirm the performance degradation using monitoring tools such as Prometheus, Grafana, or cloud monitoring. Verify that response time has increased from normal levels (e.g., 200ms) to around 5 seconds.

Check application dashboards and alerts for latency spikes.

### Diagnose

Investigate system resources on the application servers.

Check CPU and memory usage:

* top
* htop

Check disk I/O bottlenecks:

* iostat

Check network connections and possible network issues:

* netstat -tulnp

Investigate database performance:

* Check slow queries
* Verify database latency and connection pool usage

Check load balancer health and backend instance status.

Also review application logs and error logs to identify delays or timeouts.

### Fix

Apply fixes depending on the root cause.

Examples:

* If CPU usage is high → scale application servers or optimize code
* If memory is exhausted → increase memory or fix memory leaks
* If database queries are slow → optimize queries or add indexes
* If traffic spike occurs → scale infrastructure or enable autoscaling

Restart failing services if necessary and redistribute traffic.

### Prevent

* Implement monitoring for latency, CPU, memory, and database metrics
* Configure autoscaling policies
* Use caching layers like Redis
* Optimize database queries and indexing
* Set alerts for abnormal response time increases

---

## 3️⃣ Jenkins Pipeline Suddenly Fails

**Scenario:**
CI/CD pipeline that worked yesterday fails today.

**Interview question:**
What will you check first?

### Identify

First confirm that the CI/CD pipeline is failing and identify which stage of the pipeline is failing.

Check the Jenkins job build status and build history in the Jenkins dashboard.

### Diagnose

Open the failed build logs to determine the exact error.

Check possible causes:

**Code changes**

* Verify recent commits in Git
* Check if the new code introduced build or test failures

**Dependency issues**

* Check if external dependencies failed to download
* Verify package repositories and version compatibility

**Credentials issues**

* Verify Jenkins credentials used for Git, Docker registry, or cloud services
* Check if tokens or passwords have expired

**Disk space issues**
On Jenkins server or agent:

* df -h
* du -sh /var/lib/jenkins

**Agent/node issues**

* Verify Jenkins agent is online
* Check agent logs
* Ensure required tools (Docker, Maven, Node, etc.) are installed

### Fix

Fix based on root cause:

* Revert problematic code commit
* Fix dependency versions or repository access
* Update expired credentials
* Free disk space on Jenkins server or agents
* Restart Jenkins agent or reconnect node

Re-run the pipeline after applying fixes.

### Prevent

* Add pipeline validation and automated tests
* Monitor disk usage on Jenkins nodes
* Implement credential rotation monitoring
* Use dependency version locking
* Configure pipeline alerts for failures

---

## 4️⃣ EC2 Instance CPU Goes to 100%

**Scenario:**
Production EC2 server CPU suddenly spikes.

**Interview question:**
How will you troubleshoot?

### Identify

First confirm the CPU spike using monitoring tools such as AWS CloudWatch or system monitoring.

Check CPU usage directly on the EC2 instance.

Commands:

* top
* htop

Look for CPU usage close to or at 100%.

### Diagnose

Identify which process is consuming the CPU.

Commands:

* ps aux --sort=-%cpu | head

Check if the issue is caused by:

**Runaway process**

* A process stuck in continuous execution

**Memory leak**

* Application continuously consuming memory leading to high CPU usage

**Infinite loop in application code**

* Bug in code causing excessive CPU consumption

Also check application logs and system logs:

* journalctl -xe
* application logs

### Fix

Apply fixes depending on the root cause:

* Kill the runaway process
* Restart the affected service
* Roll back the application deployment if the issue started after a release
* Scale the EC2 instance or add more instances behind the load balancer

Example command:

* kill -9 <PID>

### Prevent

* Implement monitoring and alerting using CloudWatch
* Configure autoscaling policies
* Add CPU usage alerts
* Optimize application code to prevent infinite loops
* Perform load testing before production deployments

---

## 5️⃣ Kubernetes Service Not Accessible

**Scenario:**
Application is running but users cannot access it.

**Interview question:**
What will you check?

### Identify

First confirm that the application pods are running but the service is not reachable by users.

Commands:

* kubectl get pods
* kubectl get svc

Verify that pods are in Running state and the service exists.

### Diagnose

Check the service configuration.

Commands:

* kubectl describe svc <service-name>

Verify:

* Correct service type (ClusterIP, NodePort, LoadBalancer)
* Correct port and targetPort mapping

Check ingress configuration if ingress is used.

Commands:

* kubectl get ingress
* kubectl describe ingress <ingress-name>

Check whether the service endpoints correctly map to pods.

Commands:

* kubectl get endpoints <service-name>

Common causes:

* Wrong service type
* Incorrect port mapping
* Selector mismatch between service and pods
* Ingress misconfiguration

### Fix

Fix the misconfiguration depending on the issue:

* Correct the service selector to match pod labels
* Fix port and targetPort mapping
* Update service type if external access is required
* Correct ingress rules and backend service references

Apply the updated configuration and redeploy the service.

### Prevent

* Validate Kubernetes manifests during CI/CD
* Use infrastructure-as-code tools like Helm or Terraform
* Implement monitoring for service availability
* Use automated tests to verify service connectivity after deployment

---

## 6️⃣ Docker Container Stops Immediately

**Scenario:**
Container starts and exits immediately.

**Interview question:**
How will you debug?

### Identify

First confirm that the container is starting and then exiting.

Commands:

* docker ps -a

Check the container status such as Exited or Restarting and note the exit code.

### Diagnose

Check the container logs to identify the error produced by the application.

Commands:

* docker logs <container-id>

Inspect the container configuration if needed:

Commands:

* docker inspect <container-id>

Common causes:

**Wrong entrypoint or command**

* Container finishes execution immediately because the main process exits

**Application crash**

* Runtime errors in the application

**Missing dependencies**

* Required libraries or environment variables are missing

**Configuration issues**

* Incorrect environment variables or configuration files

### Fix

Apply the appropriate fix depending on the root cause:

* Correct the ENTRYPOINT or CMD in the Dockerfile
* Fix application errors and rebuild the image
* Install missing dependencies in the Docker image
* Add required environment variables

After fixing the issue, rebuild and restart the container:

* docker build -t app-image .
* docker run app-image

### Prevent

* Validate Docker images during CI/CD pipelines
* Use health checks in containers
* Add logging and error handling in applications
* Test containers locally before pushing to production

---

## 7️⃣ Deployment Fails in Kubernetes

**Scenario:**
New deployment fails during release.

**Interview question:**
How will you rollback?

### Identify

First confirm that the new deployment has failed or is not progressing.

Commands:

* kubectl get pods
* kubectl get deployment <app-name>

Look for issues such as pods not becoming Ready, image pull errors, or crash loops after the new release.

### Diagnose

Check the rollout status and history of the deployment.

Commands:

* kubectl rollout status deployment <app-name>
* kubectl rollout history deployment <app-name>

Inspect the failing pods to identify the root cause.

Commands:

* kubectl describe pod <pod-name>
* kubectl logs <pod-name>

Common causes:

* Broken application code in new version
* Incorrect environment variables
* Image pull failure
* Configuration or secret errors

### Fix

If the new deployment is broken, rollback to the previous stable version.

Commands:

* kubectl rollout undo deployment <app-name>

If multiple revisions exist, rollback to a specific revision:

* kubectl rollout undo deployment <app-name> --to-revision=<revision-number>

Verify that the previous stable pods are running successfully.

Commands:

* kubectl get pods

### Prevent

* Use rolling deployments and health checks
* Implement readiness and liveness probes
* Test releases in staging before production
* Use CI/CD pipelines with automated tests
* Monitor deployment status with alerts

---

## 8️⃣ Disk Space Full on Production Server

**Scenario:**
Application suddenly stops working.

**Interview question:**
How will you investigate?

### Identify

First confirm that the application failure is due to disk space exhaustion.

Commands:

* df -h

Check the disk usage and identify which partition is full (for example `/`, `/var`, or `/home`).

### Diagnose

Locate which files or directories are consuming the most space.

Commands:

* du -sh *
* du -sh /var/*

Common causes:

**Log files**

* Application or system logs growing indefinitely.

**Core dumps**

* Large dump files created after application crashes.

**Temporary files**

* Files left in `/tmp` or application temp directories.

**Docker images / containers**

* Unused images and containers consuming space.

Commands for Docker cleanup (if applicable):

* docker system df

### Fix

Free disk space by removing or archiving unnecessary files.

Examples:

* Remove old log files
* Delete unused temporary files
* Clean up Docker images and containers

Commands:

* rm -rf /tmp/*
* docker system prune

Restart affected services if needed after freeing disk space.

### Prevent

* Implement log rotation using `logrotate`
* Set disk usage alerts using monitoring tools
* Schedule cleanup jobs for temporary files
* Monitor disk usage in Prometheus or CloudWatch

---

## 9️⃣ Terraform Deployment Fails

**Scenario:**
terraform apply fails during infrastructure creation.

**Interview question:**
How will you debug?

### Identify

First confirm that the infrastructure deployment failed during execution.

Commands:

* terraform plan

This helps verify what Terraform is trying to create or modify.

### Diagnose

Check Terraform logs and error messages to identify the root cause.

Possible checks:

**Terraform configuration validation**

* terraform validate

**Terraform state verification**

* terraform state list

**IAM permissions**

* Ensure the IAM role or user used by Terraform has sufficient permissions to create the required resources.

**Resource conflicts**

* A resource with the same name might already exist in the cloud provider.

**API limits or throttling**

* Cloud provider API rate limits may cause failures during large infrastructure deployments.

### Fix

Apply fixes based on the issue identified:

* Correct Terraform configuration errors
* Update IAM policies to grant required permissions
* Rename conflicting resources
* Retry deployment if API throttling occurred

After fixing the issue, run:

* terraform apply

### Prevent

* Use terraform validate in CI/CD pipelines
* Implement proper IAM role policies
* Use remote state management
* Monitor API limits and resource quotas

---

## 🔟 Monitoring Alert: High Memory Usage

**Scenario:**
Prometheus alert shows memory usage above 90%.

**Interview question:**
What will you do?

### Identify

First confirm the alert from the monitoring system (for example Prometheus/Grafana) indicating memory usage above 90%.

Login to the server and verify memory usage.

Commands:

* free -m

### Diagnose

Check which processes are consuming the most memory.

Commands:

* ps aux --sort=-%mem | head

Also check system memory details:

Commands:

* top
* htop

Possible causes:

**Memory leak**

* Application continuously consuming memory without releasing it.

**High traffic or workload**

* Application handling more requests than usual.

**Misconfigured application**

* Application caching too much data in memory.

**Container memory limits**

* Containers exceeding configured memory limits.

### Fix

Apply fixes depending on the root cause:

* Restart the application or service
* Fix the memory leak in application code
* Increase server memory if required
* Scale horizontally by adding more instances

Example command:

* systemctl restart <service-name>

### Prevent

* Configure Prometheus alerts for memory thresholds
* Set resource limits for containers
* Implement autoscaling
* Perform memory profiling in applications

---

# ⭐ Most Common DevOps Troubleshooting Question

## Interview Question

"Your application is down in production. What is the first thing you do?"

## Identify

First confirm that the application is actually down using monitoring systems such as Prometheus, Grafana, or CloudWatch.

Check alerts and dashboards to see when the incident started and which components are affected.

## Diagnose

Check logs and system health to determine the root cause.

Steps:

* Check application logs
* Check server logs
* Check container or pod logs

Commands examples:

* kubectl logs <pod-name>
* docker logs <container-id>
* journalctl -xe

Check infrastructure health:

* Verify EC2 instance status
* Check Kubernetes pod status
* Check load balancer health

Commands:

* kubectl get pods
* kubectl get svc

## Fix

Apply the appropriate fix based on the identified root cause.

Examples:

* Restart the failed service
* Scale infrastructure
* Fix configuration errors
* Rollback a faulty deployment

Example rollback command:

* kubectl rollout undo deployment <app-name>

## Prevent

After resolving the incident:

* Perform root cause analysis (RCA)
* Improve monitoring and alerting
* Add health checks
* Improve CI/CD validation

---