# Kubernetes Troubleshooting

## Question 1

A pod is stuck in Pending state. What could be the reasons?

### 1️⃣ Identify

Check pod status.

```bash
kubectl get pods
```

Look for pods with `Pending` status.

### 2️⃣ Diagnose

Check detailed pod events.

```bash
kubectl describe pod <pod-name>
```

Common reasons:

* Insufficient CPU or memory on nodes
* Node selector or affinity rules not matching
* Taints preventing scheduling
* PersistentVolume not available

Check node resources:

```bash
kubectl get nodes
kubectl describe node <node-name>
```

Check PVC status if volumes are used:

```bash
kubectl get pvc
```

### 3️⃣ Fix

Depending on the root cause:

Scale cluster nodes if resources are insufficient.

Fix resource requests in deployment YAML.

Remove incorrect nodeSelector or affinity rules.

If taints block scheduling, add tolerations or remove taints.

Example:

```bash
kubectl taint nodes <node-name> key=value:NoSchedule-
```

If PVC not bound, create or fix PersistentVolume.

### 4️⃣ Prevent

* Monitor cluster capacity
* Use cluster autoscaler
* Define proper resource requests and limits
* Validate YAML configuration in CI/CD pipeline

---

## Question 2

A pod shows CrashLoopBackOff. How do you debug it?

### 1️⃣ Identify

Check pod status.

```bash
kubectl get pods
```

Look for pods with `CrashLoopBackOff` status.

### 2️⃣ Diagnose

Check container logs to identify the error.

```bash
kubectl logs <pod-name>
```

If multiple containers exist:

```bash
kubectl logs <pod-name> -c <container-name>
```

Check previous container logs (very important for CrashLoop issues):

```bash
kubectl logs <pod-name> --previous
```

Check pod events and configuration:

```bash
kubectl describe pod <pod-name>
```

Common reasons:

* Application crash
* Wrong environment variables
* Missing configuration or secrets
* Database connection failure
* Incorrect command or entrypoint
* Port already in use

### 3️⃣ Fix

Depending on the root cause:

If environment variables or configuration are wrong, update the deployment:

```bash
kubectl edit deployment <deployment-name>
```

Or apply corrected YAML:

```bash
kubectl apply -f deployment.yaml
```

Restart pods after fix:

```bash
kubectl rollout restart deployment <deployment-name>
```

### 4️⃣ Prevent

* Add liveness and readiness probes
* Use proper error handling in the application
* Implement logging and monitoring
* Validate configuration during CI/CD pipeline

---

## Question 3

Your Deployment is created but pods are not starting. What will you check?

### 1️⃣ Identify

First verify whether the deployment and pods are created.

```bash
kubectl get deployments
kubectl get pods
```

Check if the desired replicas are created and observe pod status.

---

### 2️⃣ Diagnose

Check deployment details and events.

```bash
kubectl describe deployment <deployment-name>
```

Check ReplicaSet created by the deployment.

```bash
kubectl get rs
kubectl describe rs <replicaset-name>
```

Check pod events for errors.

```bash
kubectl describe pod <pod-name>
```

Check container logs if pods are starting but crashing.

```bash
kubectl logs <pod-name>
```

Common reasons:

* Image pull error
* Wrong container image name
* Insufficient cluster resources
* CrashLoopBackOff due to application failure
* Node scheduling issues
* Incorrect configuration in deployment YAML

Check node resources:

```bash
kubectl get nodes
kubectl describe node <node-name>
```

---

### 3️⃣ Fix

Depending on the issue:

If image name is incorrect:

```bash
kubectl set image deployment/<deployment-name> <container-name>=<correct-image>
```

If resources are insufficient:

* Scale cluster nodes
* Adjust resource requests and limits

If configuration issue exists, update deployment YAML:

```bash
kubectl apply -f deployment.yaml
```

Restart deployment if required:

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Validate YAML before deployment
* Implement CI/CD checks for configuration errors
* Use resource monitoring
* Implement proper logging and monitoring
* Use readiness and liveness probes

---

## Question 4

A service is running but users cannot access the application. What might be wrong?

### 1️⃣ Identify

Check if the service and pods are running.

```bash
kubectl get svc
kubectl get pods
```

Verify the service type (ClusterIP, NodePort, LoadBalancer) and ensure pods are in `Running` state.

---

### 2️⃣ Diagnose

Check service configuration.

```bash
kubectl describe svc <service-name>
```

Verify that the service selector matches the pod labels.

```bash
kubectl get pods --show-labels
```

Check service endpoints to ensure pods are registered.

```bash
kubectl get endpoints <service-name>
```

If endpoints are empty, the service is not linked to pods.

Check port configuration.

```bash
kubectl get svc <service-name> -o yaml
```

Verify:

* `port`
* `targetPort`
* `nodePort` (if NodePort service)

If using NodePort, test access from node.

```bash
curl http://<node-ip>:<nodePort>
```

If using LoadBalancer, check external IP.

```bash
kubectl get svc
```

Check pod logs if application is not responding.

```bash
kubectl logs <pod-name>
```

---

### 3️⃣ Fix

Fix label mismatch between service and pods.

Update service selector or pod labels and reapply configuration.

```bash
kubectl apply -f service.yaml
```

If ports are misconfigured, correct `targetPort` or container port.

Restart deployment if needed.

```bash
kubectl rollout restart deployment <deployment-name>
```

Ensure firewall or security group allows traffic to NodePort or LoadBalancer.

---

### 4️⃣ Prevent

* Use consistent labeling strategy
* Validate service configuration in CI/CD
* Implement readiness probes
* Monitor service endpoints
* Use automated integration tests before deployment

---

## Question 5

Pods are running but application logs show database connection errors. How will you debug?

### 1️⃣ Identify

Verify pod status and confirm application error.

```bash
kubectl get pods
```

Check application logs for database connection errors.

```bash
kubectl logs <pod-name>
```

---

### 2️⃣ Diagnose

Check environment variables used for database connection.

```bash
kubectl describe pod <pod-name>
```

Look for variables such as:

* DB_HOST
* DB_PORT
* DB_USER
* DB_PASSWORD

Check if secrets or configmaps are mounted correctly.

```bash
kubectl get secrets
kubectl get configmap
```

Verify network connectivity from the pod to the database.

Enter the pod shell.

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

Test connectivity.

```bash
ping <db-host>
```

or

```bash
nc -zv <db-host> <db-port>
```

Check if database service exists in the cluster.

```bash
kubectl get svc
```

If database is external, verify DNS resolution.

```bash
nslookup <db-host>
```

---

### 3️⃣ Fix

Fix incorrect environment variables in deployment.

```bash
kubectl edit deployment <deployment-name>
```

Update secrets or configmaps if credentials are wrong.

```bash
kubectl apply -f secret.yaml
kubectl apply -f configmap.yaml
```

Restart pods after updating configuration.

```bash
kubectl rollout restart deployment <deployment-name>
```

Ensure database security group or firewall allows traffic from Kubernetes nodes.

---

### 4️⃣ Prevent

* Store credentials securely using Kubernetes Secrets
* Validate database connectivity during CI/CD testing
* Implement retry logic in application
* Use monitoring and alerting for database failures
* Document environment configuration

---

## Question 6

ImagePullBackOff error appears in a pod. What causes it?

### 1️⃣ Identify

Check pod status.

```bash
kubectl get pods
```

Look for pods with `ImagePullBackOff` or `ErrImagePull` status.

---

### 2️⃣ Diagnose

Check detailed pod events.

```bash
kubectl describe pod <pod-name>
```

Look at the **Events** section for the exact error message.

Common causes:

* Incorrect container image name
* Wrong image tag
* Private registry authentication failure
* Image does not exist in registry
* Network connectivity issues to container registry

Check the image configured in deployment.

```bash
kubectl get deployment <deployment-name> -o yaml
```

If using a private registry, verify imagePullSecrets.

```bash
kubectl get secrets
```

---

### 3️⃣ Fix

If image name or tag is incorrect, update the deployment image.

```bash
kubectl set image deployment/<deployment-name> <container-name>=<correct-image>:<tag>
```

If using private registry, create or update imagePullSecret.

```bash
kubectl create secret docker-registry regcred \
  --docker-server=<registry-url> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>
```

Attach the secret to the deployment YAML.

```yaml
imagePullSecrets:
- name: regcred
```

Restart the deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Use verified image tags
* Automate image validation in CI/CD
* Store registry credentials securely
* Use image scanning and monitoring
* Implement proper deployment testing before production

---

## Question 7

A node becomes NotReady. What steps will you take?

### 1️⃣ Identify

Check node status.

```bash
kubectl get nodes
```

Look for nodes with `NotReady` status.

---

### 2️⃣ Diagnose

Check node details and conditions.

```bash
kubectl describe node <node-name>
```

Review the **Conditions** section for issues such as:

* MemoryPressure
* DiskPressure
* PIDPressure
* NetworkUnavailable

Check if kubelet is running on the node.

```bash
systemctl status kubelet
```

Check kubelet logs for errors.

```bash
journalctl -u kubelet -f
```

Verify node resource usage.

```bash
top
free -m
df -h
```

Check container runtime status.

```bash
systemctl status containerd
```

---

### 3️⃣ Fix

If kubelet is not running, restart it.

```bash
sudo systemctl restart kubelet
```

If disk is full, clean up unused images and containers.

```bash
docker system prune -a
```

If network issue exists, restart networking service.

```bash
sudo systemctl restart network
```

If node remains unhealthy, cordon and drain the node.

```bash
kubectl cordon <node-name>
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
```

Then investigate or recreate the node.

---

### 4️⃣ Prevent

* Monitor node health with Prometheus
* Implement node auto-healing
* Use cluster autoscaler
* Monitor disk and memory usage
* Configure alerting for node failures

---

## Question 8

Kubernetes application is running but traffic is not reaching the pod.

### 1️⃣ Identify

Check whether pods are running.

```bash
kubectl get pods
```

Verify service exists.

```bash
kubectl get svc
```

---

### 2️⃣ Diagnose

Check service selectors and labels.

```bash
kubectl describe svc <service-name>
```

Check pod labels.

```bash
kubectl get pods --show-labels
```

Verify service endpoints.

```bash
kubectl get endpoints <service-name>
```

If endpoints are empty, service is not connected to pods.

Check network policies.

```bash
kubectl get networkpolicy
```

Check container port configuration.

```bash
kubectl describe pod <pod-name>
```

Check if application inside pod is listening on expected port.

```bash
kubectl exec -it <pod-name> -- netstat -tulnp
```

---

### 3️⃣ Fix

Fix label mismatch between service and pods.

Update service or deployment configuration.

```bash
kubectl apply -f service.yaml
```

If port configuration is wrong, correct `targetPort`.

Restart deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

If network policy blocks traffic, update policy rules.

---

### 4️⃣ Prevent

* Use consistent labeling standards
* Validate service‑pod connectivity in CI/CD tests
* Implement monitoring for service endpoints
* Document networking configuration

---

## Question 9

ConfigMap changes are not reflected in pods. Why?

### 1️⃣ Identify

Check whether the ConfigMap has been updated.

```bash
kubectl get configmap <configmap-name>
```

View its contents.

```bash
kubectl describe configmap <configmap-name>
```

Check whether the pods are still running with old configuration.

```bash
kubectl get pods
```

---

### 2️⃣ Diagnose

Determine how the ConfigMap is used in the pod.

Check pod configuration.

```bash
kubectl describe pod <pod-name>
```

ConfigMaps can be used in two ways:

1. Environment variables
2. Mounted as volumes

Important behavior:

* If ConfigMap is used as **environment variables**, pods must be restarted to see changes.
* If ConfigMap is mounted as a **volume**, Kubernetes updates files automatically but the application must reload them.

Check if pods are still using old environment variables.

```bash
kubectl exec -it <pod-name> -- env | grep <VARIABLE_NAME>
```

---

### 3️⃣ Fix

If ConfigMap is used as environment variables, restart the deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

Or delete pods so new ones start with updated configuration.

```bash
kubectl delete pod <pod-name>
```

If using mounted volumes, ensure the application reloads configuration files.

---

### 4️⃣ Prevent

* Use rolling restarts after ConfigMap updates
* Automate restart in CI/CD pipelines
* Use configuration reload mechanisms in applications
* Document configuration update procedures

---

## Question 10

A Secret is not accessible inside the container.

### 1️⃣ Identify

Check if the secret exists in the namespace.

```bash
kubectl get secrets
```

Check the pod status.

```bash
kubectl get pods
```

---

### 2️⃣ Diagnose

Verify the secret configuration.

```bash
kubectl describe secret <secret-name>
```

Check if the secret is correctly referenced in the pod or deployment.

```bash
kubectl describe pod <pod-name>
```

If the secret is mounted as environment variables, verify inside the container.

```bash
kubectl exec -it <pod-name> -- env | grep <SECRET_KEY>
```

If the secret is mounted as a volume, check the mounted files.

```bash
kubectl exec -it <pod-name> -- ls /etc/secrets
```

Common causes:

* Secret not created in the correct namespace
* Incorrect secret name in deployment
* Incorrect key name
* Pod started before secret was created

---

### 3️⃣ Fix

Create the secret if it does not exist.

```bash
kubectl create secret generic <secret-name> \
--from-literal=username=<username> \
--from-literal=password=<password>
```

If deployment configuration is wrong, update it.

```bash
kubectl edit deployment <deployment-name>
```

Restart pods to reload the secret.

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Store secrets in a secure secret management system
* Validate secret references in CI/CD pipelines
* Use consistent naming conventions
* Document secret management procedures

---

## Question 11

A pod consumes too much CPU and memory. How will you investigate?

### 1️⃣ Identify

Check current resource usage of pods.

```bash
kubectl top pods
```

Identify the pod consuming high CPU or memory.

---

### 2️⃣ Diagnose

Check detailed pod information.

```bash
kubectl describe pod <pod-name>
```

Check container logs for abnormal behavior.

```bash
kubectl logs <pod-name>
```

Enter the container to inspect running processes.

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

Inside the container check processes.

```bash
top
ps aux
```

Verify resource requests and limits.

```bash
kubectl get pod <pod-name> -o yaml
```

Check if limits are missing or too high.

---

### 3️⃣ Fix

If application has a bug or memory leak, fix the application code.

Adjust resource requests and limits in deployment.

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

Apply updated configuration.

```bash
kubectl apply -f deployment.yaml
```

Restart the deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Define resource requests and limits for all containers
* Use Horizontal Pod Autoscaler (HPA)
* Monitor resource usage with Prometheus and Grafana
* Implement performance testing before production deployment

---

## Question 12

Your Horizontal Pod Autoscaler (HPA) is not scaling pods. What could be wrong?

### 1️⃣ Identify

Check the HPA status.

```bash
kubectl get hpa
```

Check current metrics and desired replicas.

```bash
kubectl describe hpa <hpa-name>
```

---

### 2️⃣ Diagnose

Verify that the metrics server is running.

```bash
kubectl get pods -n kube-system | grep metrics-server
```

Check if metrics are available.

```bash
kubectl top pods
kubectl top nodes
```

Check resource requests in the deployment.

```bash
kubectl get deployment <deployment-name> -o yaml
```

HPA requires CPU or memory **requests** to be defined.

Check if the HPA target is correct.

```bash
kubectl describe hpa <hpa-name>
```

Look for:

* target CPU utilization
* current CPU utilization

Check if the load is actually increasing.

---

### 3️⃣ Fix

If metrics server is missing, install it.

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Add resource requests to the deployment.

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
```

Update and apply the deployment.

```bash
kubectl apply -f deployment.yaml
```

Verify scaling again.

```bash
kubectl get hpa -w
```

---

### 4️⃣ Prevent

* Ensure metrics-server is installed in the cluster
* Always define resource requests in deployments
* Monitor HPA metrics using Prometheus and Grafana
* Test autoscaling during staging environment deployments

---

## Question 13

Pods restart after every deployment. How do you debug?

### 1️⃣ Identify

Check pod restart count.

```bash
kubectl get pods
```

Look at the **RESTARTS** column.

Check rollout status of deployment.

```bash
kubectl rollout status deployment <deployment-name>
```

---

### 2️⃣ Diagnose

Check pod events and configuration.

```bash
kubectl describe pod <pod-name>
```

Check container logs for crash reasons.

```bash
kubectl logs <pod-name>
```

Check previous container logs.

```bash
kubectl logs <pod-name> --previous
```

Verify probes configuration (very common cause).

```bash
kubectl get deployment <deployment-name> -o yaml
```

Look for:

* livenessProbe
* readinessProbe

If probes fail, Kubernetes restarts the container.

Check resource limits.

```bash
kubectl describe pod <pod-name>
```

Look for **OOMKilled** events.

---

### 3️⃣ Fix

If liveness probe is misconfigured, correct probe settings.

Example:

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
```

If application startup is slow, increase delay values.

If memory limits are too low, increase limits.

Apply corrected configuration.

```bash
kubectl apply -f deployment.yaml
```

Restart deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Test health probes before deployment
* Define proper resource limits
* Monitor pod restart metrics
* Implement staging environment testing before production

---

## Question 14

Ingress is configured but domain is not resolving.

### 1️⃣ Identify

Check whether the Ingress resource exists.

```bash
kubectl get ingress
```

Verify the hostname and address assigned to the ingress.

```bash
kubectl describe ingress <ingress-name>
```

---

### 2️⃣ Diagnose

Check if the Ingress controller is running.

```bash
kubectl get pods -n ingress-nginx
```

Verify the external IP of the ingress service.

```bash
kubectl get svc -n ingress-nginx
```

Check DNS resolution from local system.

```bash
nslookup <domain-name>
```

or

```bash
dig <domain-name>
```

Verify the DNS record points to the load balancer or ingress external IP.

Check ingress rules.

```bash
kubectl describe ingress <ingress-name>
```

Ensure host and path rules are correct.

---

### 3️⃣ Fix

If DNS record is incorrect, update DNS to point to ingress external IP or load balancer.

Example DNS record:

```
example.com -> <load-balancer-ip>
```

If ingress controller is not installed, deploy it.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Verify ingress configuration and reapply.

```bash
kubectl apply -f ingress.yaml
```

---

### 4️⃣ Prevent

* Use automated DNS management
* Monitor ingress controller health
* Validate ingress configuration in CI/CD
* Document DNS and ingress architecture

---

## Question 15

Application works locally but fails when deployed to Kubernetes.

### 1️⃣ Identify

Check pod status after deployment.

```bash
kubectl get pods
```

Verify if pods are running, crashing, or stuck in states like `CrashLoopBackOff`.

Check application logs.

```bash
kubectl logs <pod-name>
```

---

### 2️⃣ Diagnose

Inspect pod details and events.

```bash
kubectl describe pod <pod-name>
```

Common issues when an app works locally but fails in Kubernetes:

* Missing environment variables
* Incorrect ConfigMap or Secret configuration
* Incorrect container port
* Service or DNS misconfiguration
* File paths different inside container
* Database or external service connectivity issues

Verify environment variables.

```bash
kubectl exec -it <pod-name> -- env
```

Check container port configuration.

```bash
kubectl get deployment <deployment-name> -o yaml
```

Test connectivity to dependent services from inside the pod.

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

Then run:

```bash
curl <service-name>:<port>
```

or

```bash
ping <service-name>
```

---

### 3️⃣ Fix

Fix configuration mismatches between local and Kubernetes environment.

Examples:

Update environment variables in deployment.

```bash
kubectl edit deployment <deployment-name>
```

Fix service endpoints or ports if incorrect.

Rebuild container image if application dependencies are missing.

```bash
docker build -t <image-name>:<tag> .
docker push <image-name>:<tag>
```

Update deployment image.

```bash
kubectl set image deployment/<deployment-name> <container-name>=<image-name>:<tag>
```

---

### 4️⃣ Prevent

* Ensure environment parity between local and Kubernetes
* Use ConfigMaps and Secrets for configuration management
* Implement CI/CD testing in staging environments
* Add health checks and logging
* Document required environment variables and dependencies

---

## Question 16

A pod is running but health checks fail.

### 1️⃣ Identify

Check pod status and restart count.

```bash
kubectl get pods
```

Check if the pod is restarting due to probe failures.

---

### 2️⃣ Diagnose

Inspect pod events.

```bash
kubectl describe pod <pod-name>
```

Look for probe failure messages under **Events**.

Check the probe configuration in the deployment.

```bash
kubectl get deployment <deployment-name> -o yaml
```

Verify the following probe settings:

* `livenessProbe`
* `readinessProbe`
* `startupProbe`

Common issues:

* Incorrect probe path
* Wrong port
* Application startup delay
* Application not exposing health endpoint

Test the endpoint from inside the pod.

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

Then run:

```bash
curl http://localhost:<port>/<health-path>
```

---

### 3️⃣ Fix

Correct probe configuration if path or port is wrong.

Example:

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 20
  periodSeconds: 10
```

If application takes longer to start, increase probe delay values.

Apply updated configuration.

```bash
kubectl apply -f deployment.yaml
```

Restart deployment.

```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Implement proper health endpoints in application
* Test probes in staging environments
* Configure startupProbe for slow-start applications
* Monitor probe failures using Prometheus

---

## Question 17

Kubernetes cluster suddenly runs out of resources.

### 1️⃣ Identify

Check overall node resource usage.

```bash
kubectl top nodes
```

Check pod resource consumption.

```bash
kubectl top pods -A
```

Identify nodes or pods consuming excessive CPU or memory.

---

### 2️⃣ Diagnose

Check node capacity and allocatable resources.

```bash
kubectl describe node <node-name>
```

Look for:

* CPU usage
* Memory usage
* Pod capacity

Check if many pods are stuck in `Pending` state.

```bash
kubectl get pods -A
```

Check if any workloads are consuming abnormal resources.

```bash
kubectl describe pod <pod-name>
```

Verify if resource **requests and limits** are configured.

```bash
kubectl get deployment <deployment-name> -o yaml
```

Check for runaway jobs or batch workloads.

```bash
kubectl get jobs
kubectl get cronjobs
```

---

### 3️⃣ Fix

Scale the cluster by adding more nodes.

If using managed Kubernetes (EKS/GKE/AKS), increase node group size.

Alternatively remove unnecessary workloads.

```bash
kubectl delete pod <pod-name>
```

Adjust resource limits for misbehaving pods.

Apply updated configuration.

```bash
kubectl apply -f deployment.yaml
```

If cluster autoscaler is enabled, verify it is functioning.

---

### 4️⃣ Prevent

* Define resource requests and limits for all containers
* Implement cluster autoscaler
* Monitor resource usage with Prometheus and Grafana
* Use quotas and limits for namespaces
* Perform capacity planning for workloads

---

## Question 18

Rolling update causes downtime. Why?

### 1️⃣ Identify

Check the deployment rollout status.

```bash
kubectl rollout status deployment <deployment-name>
```

Check how many pods are running during the update.

```bash
kubectl get pods -w
```

Observe if old pods are terminated before new pods become ready.

---

### 2️⃣ Diagnose

Inspect deployment strategy configuration.

```bash
kubectl get deployment <deployment-name> -o yaml
```

Look for the **rollingUpdate** strategy settings.

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

Common causes of downtime:

* `maxUnavailable` set too high
* Readiness probes missing
* New pods taking too long to start
* Insufficient cluster resources for new pods

Check readiness probe configuration.

```bash
kubectl describe pod <pod-name>
```

If readiness probes are missing, Kubernetes may route traffic to pods before they are ready.

---

### 3️⃣ Fix

Configure safe rolling update parameters.

Example:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

Ensure readiness probes are configured.

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

Apply updated deployment.

```bash
kubectl apply -f deployment.yaml
```

Monitor rollout.

```bash
kubectl rollout status deployment <deployment-name>
```

---

### 4️⃣ Prevent

* Use readiness probes to ensure pods receive traffic only when ready
* Configure proper rolling update parameters
* Perform load testing before production deployment
* Monitor deployment rollouts using CI/CD pipelines

---

## Question 19

A Persistent Volume is not mounting to the pod.

### 1️⃣ Identify

Check the pod status.

```bash
kubectl get pods
```

Look for pods stuck in `ContainerCreating` or `Pending`.

Check PVC status.

```bash
kubectl get pvc
```

---

### 2️⃣ Diagnose

Describe the pod to see volume-related events.

```bash
kubectl describe pod <pod-name>
```

Check PVC details.

```bash
kubectl describe pvc <pvc-name>
```

Verify if PVC is **Bound** to a PersistentVolume.

Check available PersistentVolumes.

```bash
kubectl get pv
```

Common issues:

* PVC is in `Pending` state
* Storage class mismatch
* Access mode mismatch
* Volume already mounted by another pod
* Node cannot access underlying storage

Check storage class.

```bash
kubectl get storageclass
```

---

### 3️⃣ Fix

If PVC is not bound, create or correct the PersistentVolume.

```bash
kubectl apply -f pv.yaml
```

Ensure storage class matches.

Update PVC configuration.

```bash
kubectl apply -f pvc.yaml
```

Restart the pod to retry mounting.

```bash
kubectl delete pod <pod-name>
```

---

### 4️⃣ Prevent

* Use dynamic provisioning with StorageClasses
* Validate PVC configuration in CI/CD
* Monitor storage usage
* Document storage architecture and access modes

---

## Question 20

Kubernetes cluster becomes very slow. How will you investigate?

### 1️⃣ Identify

Check cluster resource usage.

```bash
kubectl top nodes
kubectl top pods -A
```

Look for nodes or pods consuming excessive CPU or memory.

Check node status.

```bash
kubectl get nodes
```

---

### 2️⃣ Diagnose

Inspect node details.

```bash
kubectl describe node <node-name>
```

Look for conditions like:

* MemoryPressure
* DiskPressure
* PIDPressure

Check system-level usage on the node.

```bash
top
free -m
df -h
```

Check kube-system components.

```bash
kubectl get pods -n kube-system
```

Look for issues with:

* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager

Check cluster events.

```bash
kubectl get events -A
```

Check API server responsiveness.

```bash
kubectl get pods --request-timeout=10s
```

---

### 3️⃣ Fix

If nodes are overloaded, scale the cluster.

Add more worker nodes or increase node size.

If specific pods consume excessive resources, limit them.

Update deployment resource limits.

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

Restart problematic workloads if necessary.

```bash
kubectl rollout restart deployment <deployment-name>
```

If control plane components are overloaded, optimize etcd storage and API server configuration.

---

### 4️⃣ Prevent

* Implement cluster autoscaler
* Monitor cluster performance using Prometheus and Grafana
* Configure resource requests and limits for all workloads
* Regularly clean up unused resources
* Perform capacity planning for cluster workloads

---

