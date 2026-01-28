# Kubernetes Troubleshooting Interview Guide

This README contains **real-world, scenario-based Kubernetes troubleshooting interview questions with clear, step-by-step model answers**. These are **very commonly asked** for **DevOps / Kubernetes Engineer (Fresher–Mid)** roles.

---

## 20 Real-World Scenario-Based Kubernetes Troubleshooting Questions (With Step-by-Step Answers)

### 1️⃣ Pod is stuck in Pending state

**Scenario:**

A Pod is created but remains in Pending state for a long time.

**Model Answer:**

First, check pod details:

```bash
kubectl describe pod <pod-name>
```

**Common causes:**

* No available nodes
* Insufficient CPU/memory
* Node selector or taints not matching

Check node resources:

```bash
kubectl describe nodes
```

**Solution:**

* Free up resources
* Add more nodes
* Fix resource requests/limits or node selectors

---

### 2️⃣ Pod is in CrashLoopBackOff

**Scenario:**

A Pod keeps restarting and shows CrashLoopBackOff.

**Model Answer:**

Check logs:

```bash
kubectl logs <pod-name>
```

Check previous crash logs:

```bash
kubectl logs <pod-name> --previous
```

**Common reasons:**

* Application crash
* Wrong environment variables
* Incorrect command/entrypoint

**Fix:**

* Correct application config
* Fix Docker image or startup command

---

### 3️⃣ Service is not accessible externally

**Scenario:**

A Service is created but users can’t access it from the browser.

**Model Answer:**

Check service type:

```bash
kubectl get svc
```

* Ensure correct type (NodePort / LoadBalancer)
* Check target port matches container port

Verify Pod is running and labels match:

```bash
kubectl describe svc <service-name>
kubectl get pods --show-labels
```

* Check firewall / cloud load balancer configuration

---

### 4️⃣ Pod cannot connect to another Pod

**Scenario:**

One Pod cannot communicate with another Pod.

**Model Answer:**

Test connectivity:

```bash
kubectl exec -it <pod> -- curl <service-name>
```

Check:

* Service exists
* Correct DNS name
* Network policies blocking traffic

If NetworkPolicy exists:

```bash
kubectl get networkpolicy
```

Update policy to allow traffic.

---

### 5️⃣ ImagePullBackOff error

**Scenario:**

Pod fails with ImagePullBackOff.

**Model Answer:**

Check image name:

```bash
kubectl describe pod <pod-name>
```

**Common causes:**

* Wrong image name or tag
* Private registry authentication issue

**Fix:**

* Correct image name
* Create image pull secret:

```bash
kubectl create secret docker-registry
```

---

### 6️⃣ Pod is running but application not working

**Scenario:**

Pod is in Running state but app is not responding.

**Model Answer:**

* Check container logs
* Verify container port exposure

Check readiness probe:

```bash
kubectl describe pod <pod-name>
```

If readiness probe fails, Service won’t send traffic.

**Fix:**

* Fix probe path, port, or timeout

---

### 7️⃣ Node shows NotReady

**Scenario:**

A node becomes NotReady.

**Model Answer:**

Check node status:

```bash
kubectl describe node <node-name>
```

**Common reasons:**

* Kubelet stopped
* Disk/memory pressure
* Network issue

**Fix:**

* Restart kubelet
* Free disk space
* Restore network connectivity

---

### 8️⃣ Pods are evicted automatically

**Scenario:**

Pods keep getting evicted from nodes.

**Model Answer:**

Check node conditions:

```bash
kubectl describe node
```

**Common causes:**

* Memory pressure
* Disk pressure

**Solution:**

* Increase node resources
* Set proper resource requests/limits

---

### 9️⃣ Deployment rollout stuck

**Scenario:**

Deployment update is stuck and not completing.

**Model Answer:**

Check rollout status:

```bash
kubectl rollout status deployment <name>
```

* Check ReplicaSet and Pod status

**Common causes:**

* Failed readiness probes
* Image pull issues

Rollback if needed:

```bash
kubectl rollout undo deployment <name>
```

---

### 🔟 ConfigMap changes not reflecting in Pod

**Scenario:**

Updated ConfigMap but Pod still uses old values.

**Model Answer:**

* ConfigMaps are not auto-reloaded

Restart Pod or rollout Deployment:

```bash
kubectl rollout restart deployment <name>
```

Or use tools like reloader for auto updates.

---

### 1️⃣1️⃣ Secret not accessible inside Pod

**Scenario:**

Application fails to read Kubernetes Secret.

**Model Answer:**

Check Secret exists:

```bash
kubectl get secret
```

* Verify mounting method (env or volume)
* Check pod YAML for correct key name
* Ensure correct namespace

---

### 1️⃣2️⃣ Pod fails due to permission issue

**Scenario:**

Pod fails with permission denied errors.

**Model Answer:**

Check security context:

```yaml
securityContext:
  runAsUser: 1000
```

* Check file permissions in container

**Fix:**

* Update Dockerfile or security context

---

### 1️⃣3️⃣ Service has no endpoints

**Scenario:**

Service exists but shows no endpoints.

**Model Answer:**

Check endpoints:

```bash
kubectl get endpoints <service-name>
```

**Cause:**

* Pod labels don’t match Service selector

**Fix:**

* Update labels or selector correctly

---

### 1️⃣4️⃣ Pod DNS not working

**Scenario:**

Pod cannot resolve service DNS names.

**Model Answer:**

Test DNS:

```bash
nslookup kubernetes.default
```

Check CoreDNS:

```bash
kubectl get pods -n kube-system
```

Restart CoreDNS if required.

---

### 1️⃣5️⃣ High CPU usage in Pods

**Scenario:**

Pods consume high CPU and performance degrades.

**Model Answer:**

Check metrics:

```bash
kubectl top pod
```

**Causes:**

* No CPU limits
* Infinite loops in app

**Fix:**

* Set CPU requests/limits
* Optimize application

---

### 1️⃣6️⃣ HPA not scaling Pods

**Scenario:**

Horizontal Pod Autoscaler is not scaling.

**Model Answer:**

Check HPA:

```bash
kubectl describe hpa
```

* Ensure metrics server is installed
* Verify CPU requests are set in Pod spec

---

### 1️⃣7️⃣ Pod stuck in Terminating

**Scenario:**

Pod takes too long to terminate.

**Model Answer:**

Check finalizers:

```bash
kubectl describe pod <pod-name>
```

* Check if app handles SIGTERM

Force delete if needed:

```bash
kubectl delete pod <pod-name> --force --grace-period=0
```

---

### 1️⃣8️⃣ Ingress not routing traffic

**Scenario:**

Ingress is created but traffic doesn’t reach Service.

**Model Answer:**

Check Ingress controller:

```bash
kubectl get pods -n ingress-nginx
```

* Verify host/path rules
* Ensure Service and endpoints exist

---

### 1️⃣9️⃣ StatefulSet Pod not starting

**Scenario:**

StatefulSet Pod is stuck or failing.

**Model Answer:**

Check PVC status:

```bash
kubectl get pvc
```

* Ensure storage class exists
* StatefulSets depend on stable storage

---

### 2️⃣0️⃣ Cluster suddenly becomes unreachable

**Scenario:**

kubectl cannot connect to cluster.

**Model Answer:**

Check kubeconfig:

```bash
kubectl config view
```

* Verify API server is running
* Check network / VPN
* For cloud clusters, verify control plane health

---

## 🔥 Interview Tip for You (Important)

When answering:

* Say the symptom
* Mention the command
* Explain the root cause
* Give the fix

---

## Top 20 MOST-ASKED Kubernetes Real-World Interview Scenarios (STAR Style)

Use the flow:

**Problem → Diagnosis → Root Cause → Fix**

---

### 1️⃣ Pod stuck in Pending

**Perfect Answer:**

This usually happens when the scheduler can’t find a suitable node.

I run:

```bash
kubectl describe pod <pod>
```

I check node capacity and resource requests.

Common causes are insufficient CPU/memory, node selectors, or taints.

I fix it by adjusting resource requests or adding nodes.

---

### 2️⃣ Pod in CrashLoopBackOff

**Perfect Answer:**

CrashLoopBackOff means the container keeps crashing after startup.

I check logs:

```bash
kubectl logs <pod> --previous
```

I verify environment variables and startup commands.

Most often it’s an app crash or misconfiguration.

Fix is correcting config or rebuilding the image.

---

### 3️⃣ Service not accessible

**Perfect Answer:**

If a Service isn’t reachable, I verify connectivity step-by-step.

Check Service type and ports:

```bash
kubectl get svc
```

Verify selector labels match Pods.

Ensure Pods are Ready.

Fix mismatched ports or labels.

---

### 4️⃣ Service has no endpoints

**Perfect Answer:**

This clearly indicates selector mismatch.

I run:

```bash
kubectl get endpoints <svc>
```

Then verify Pod labels.

Fix is aligning Service selectors with Pod labels.

---

### 5️⃣ ImagePullBackOff

**Perfect Answer:**

This happens when Kubernetes can’t pull the image.

I check image name and tag.

If private registry, I verify imagePullSecrets.

Fix by correcting image name or registry authentication.

---

### 6️⃣ Pod running but app not responding

**Perfect Answer:**

Running state doesn’t guarantee app readiness.

I check readiness probe status.

I verify container ports and health endpoints.

Fix usually involves correcting readiness probe config.

---

### 7️⃣ Node becomes NotReady

**Perfect Answer:**

A NotReady node usually means kubelet or system issue.

I check:

```bash
kubectl describe node <node>
```

Common causes: disk pressure, memory pressure, kubelet stopped.

Fix is restarting kubelet or freeing resources.

---

### 8️⃣ Pods getting Evicted

**Perfect Answer:**

Eviction happens due to resource pressure.

I check node conditions.

Usually caused by memory or disk pressure.

Fix is increasing node capacity or setting proper limits.

---

### 9️⃣ Deployment rollout stuck

**Perfect Answer:**

Rollouts get stuck when new Pods don’t become Ready.

I check rollout status:

```bash
kubectl rollout status deployment <name>
```

Root cause is often failing probes.

Fix is correcting probes or rolling back.

---

### 🔟 ConfigMap update not applied

**Perfect Answer:**

ConfigMap updates don’t auto-reload.

I restart the Pods or rollout Deployment.

For production, I use reload mechanisms or checksum annotations.

---

### 1️⃣1️⃣ Pod can’t access Secret

**Perfect Answer:**

This is usually due to incorrect mounting or key name.

I verify Secret exists in the same namespace.

Check env or volume mount.

Fix incorrect key or namespace.

---

### 1️⃣2️⃣ Pod-to-Pod communication fails

**Perfect Answer:**

Networking issues usually involve DNS or NetworkPolicies.

I test connectivity using Service DNS.

I check NetworkPolicies.

Fix is allowing traffic in policy.

---

### 1️⃣3️⃣ DNS not working in cluster

**Perfect Answer:**

DNS issues point to CoreDNS.

I check CoreDNS pods:

```bash
kubectl get pods -n kube-system
```

Restart CoreDNS if needed.

---

### 1️⃣4️⃣ HPA not scaling Pods

**Perfect Answer:**

HPA relies on metrics.

I check metrics server.

Verify CPU requests are defined.

Fix by installing metrics server and setting requests.

---

### 1️⃣5️⃣ High CPU or memory usage

**Perfect Answer:**

This is usually due to missing limits.

I check metrics:

```bash
kubectl top pod
```

Fix by setting proper requests and limits.

---

### 1️⃣6️⃣ Pod stuck in Terminating

**Perfect Answer:**

This happens when the app doesn’t exit gracefully.

I check finalizers and SIGTERM handling.

Force delete if required.

---

### 1️⃣7️⃣ Ingress not routing traffic

**Perfect Answer:**

Ingress depends on controller.

I check Ingress controller pods.

Verify host/path rules.

Fix misconfigured rules or controller issues.

---

### 1️⃣8️⃣ StatefulSet Pod not starting

**Perfect Answer:**

StatefulSets depend on storage.

I check PVC status.

Ensure StorageClass exists.

Fix storage configuration.

---

### 1️⃣9️⃣ Cluster suddenly unreachable

**Perfect Answer:**

This is usually kubeconfig or API server issue.

I verify kubeconfig context.

Check API server health.

Fix network or control plane issues.

---

### 2️⃣0️⃣ Pods not scheduling on specific nodes

**Perfect Answer:**

This is due to taints or node selectors.

I check taints:

```bash
kubectl describe node
```

Add tolerations or update selectors.

---

## 🔥 FINAL INTERVIEW GOLDEN RULE

Always speak in this order:

**Command → Observation → Root Cause → Fix**
