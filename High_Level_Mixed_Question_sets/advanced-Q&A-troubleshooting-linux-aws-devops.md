# DevOps Troubleshooting Interview Questions & Model Answers (1–100)

This README is a **complete, interview‑ready troubleshooting handbook** for DevOps Engineers. It contains **100 real‑world troubleshooting scenarios** with **step‑by‑step model answers**, exactly in the format interviewers expect.

---

## 100 real-world troubleshooting interview questions for a DevOps Engineer, exactly the kind interviewers use to test problem-solving + hands-on thinking.
I’ve grouped them by domain so you can practice systematically.

---

### 🔹 Linux / System Troubleshooting (1–20)

1. A Linux server is running out of disk space suddenly. How do you investigate?

2. A service fails to start after a reboot. How do you debug it?

3. The system load average is very high. What steps do you take?

4. A process is consuming 100% CPU. How do you identify and fix it?

5. Users complain that SSH login is very slow. What could be wrong?

6. A cron job is not running. How do you troubleshoot?

7. A file system is mounted as read-only. What are the possible reasons?

8. You can ping a server but cannot SSH into it. What do you check?

9. How do you troubleshoot memory leaks on a Linux system?

10. A server time is out of sync. How do you fix it?

11. df -h shows space available, but applications say disk is full. Why?

12. A log file is growing very fast. How do you handle it?

13. How do you debug permission issues in Linux?

14. A system rebooted unexpectedly. How do you find the cause?

15. Network connectivity works intermittently. How do you debug it?

16. A package installation fails due to dependency issues. What do you do?

17. Swap usage is very high. What does it indicate?

18. How do you troubleshoot a zombie process?

19. A server becomes unreachable after a firewall change. How do you recover?

20. How do you diagnose kernel panic issues?

### 🔹 Git / GitHub Troubleshooting (21–30)

21. A Git push fails due to conflicts. How do you resolve it?

22. Someone force-pushed to main and broke production. What do you do?

23. A GitHub Actions workflow fails intermittently. How do you debug?

24. A commit was accidentally pushed with secrets. What steps do you take?

25. Merge conflicts appear frequently in your team. How do you reduce them?

26. A Git repo is very slow to clone. What could be the reason?

27. A CI job is triggered multiple times unnecessarily. Why?

28. Git history is messy. How do you clean it up?

29. How do you recover a deleted branch?

30. GitHub webhook is not triggering your pipeline. How do you debug?

### 🔹 Docker Troubleshooting (31–50)

31. A Docker container exits immediately after starting. How do you debug?

32. Docker image size is very large. How do you reduce it?

33. A container cannot connect to another container. What do you check?

34. Docker build fails with “no space left on device”. How do you fix it?

35. A container is running but the application is not accessible.

36. Environment variables are not available inside a container. Why?

37. A container uses too much memory. How do you control it?

38. Docker daemon is not starting. How do you troubleshoot?

39. Volume data is missing after container restart. What went wrong?

40. Containers work locally but fail in production. Why?

41. Docker logs are not appearing. What could be the issue?

42. A container keeps restarting (CrashLoop). How do you debug?

43. Docker networking is not working as expected. What steps do you take?

44. How do you debug DNS issues inside containers?

45. Image pulls are very slow. How do you optimize?

46. How do you handle orphaned Docker resources?

47. A container cannot access the internet. Why?

48. How do you troubleshoot permission denied errors in containers?

49. Docker Compose services fail to start in correct order. Why?

50. How do you debug multi-stage Docker builds?

### 🔹 Kubernetes Troubleshooting (51–75)

51. A pod is stuck in Pending state. How do you troubleshoot?

52. A pod is in CrashLoopBackOff. What are your steps?

53. Service is running but not accessible externally. Why?

54. Pods are running but traffic is not reaching them.

55. Node is in NotReady state. How do you fix it?

56. Deployment is not scaling as expected. Why?

57. ConfigMap changes are not reflected in pods. Why?

58. Secrets are not injected into the container. What do you check?

59. A pod cannot pull an image. How do you debug?

60. Kubernetes cluster is slow. How do you investigate?

61. How do you debug high memory usage in a pod?

62. Persistent Volume is not mounting. What are the reasons?

63. Kubernetes API server is not responding. What do you do?

64. Pods are evicted frequently. Why?

65. How do you troubleshoot network policies blocking traffic?

66. A rolling update causes downtime. How do you prevent it?

67. HPA is not scaling pods. Why?

68. How do you debug CoreDNS issues?

69. A namespace deletion is stuck. How do you resolve it?

70. Ingress is created but not working. What could be wrong?

71. Pods restart after node reboot. How do you ensure stability?

72. How do you debug RBAC permission issues?

73. A job or cronjob fails repeatedly. How do you investigate?

74. etcd disk is full. What happens and how do you fix it?

75. How do you troubleshoot certificate expiration in Kubernetes?

### 🔹 AWS / Cloud Troubleshooting (76–90)

76. An EC2 instance is unreachable. How do you troubleshoot?

77. Application behind ALB returns 502 errors. Why?

78. Auto Scaling Group is not launching instances. Why?

79. EC2 instance fails health checks. What do you check?

80. IAM user has permissions but still gets “Access Denied”. Why?

81. S3 access is slow. How do you investigate?

82. RDS is running but application cannot connect.

83. CloudWatch alarms are not triggering. Why?

84. An EC2 instance is running but billing is very high. Why?

85. How do you debug VPC networking issues?

86. NAT Gateway is not allowing internet access. Why?

87. How do you troubleshoot Route 53 DNS issues?

88. EKS pods cannot access AWS services. Why?

89. Load balancer works but backend is unhealthy.

90. AWS API calls fail intermittently. What could be wrong?

### 🔹 CI/CD & DevOps Process Troubleshooting (91–100)

91. A CI pipeline passes locally but fails in CI. Why?

92. Deployment succeeds but application crashes. How do you debug?

93. Rollback fails after a bad deployment. What do you do?

94. Blue-Green deployment causes downtime. Why?

95. Canary deployment shows errors only for some users.

96. Jenkins job is stuck in queue. Why?

97. Pipeline execution time suddenly increases. What changed?

98. Secrets are exposed in pipeline logs. How do you fix it?

99. How do you debug environment-specific failures?

100. Production issue occurs but logs are missing. How do you troubleshoot?

**🔥 How to use this list (recommended)**

* Practice explaining your approach step-by-step
* Always mention commands, tools, and logs
* Follow this structure in answers:

    `Symptom → Investigation → Root cause → Fix → Prevention`

---

## 📌 How to Answer in Interviews

Use this structure **every time**:

**Symptom → Investigation → Root Cause → Fix → Prevention**

Always:

* Say commands out loud
* Mention logs and metrics
* Explain *why* you take each step

---

## 🔹 Linux / System Troubleshooting (1–20)

### 1️⃣ Server running out of disk space suddenly

**Investigation**

```bash
df -h
du -sh /* | sort -h
du -sh /var/* | sort -h
```

**Root Cause**

* Log files, Docker images, backups, temp files growing unexpectedly

**Fix**

```bash
logrotate -f /etc/logrotate.conf
```

Remove unnecessary files

**Prevention**

* Log rotation
* Disk monitoring alerts

---

### 2️⃣ Service fails to start after reboot

**Investigation**

```bash
systemctl status <service>
journalctl -xe
```

**Root Cause**

* Missing dependency, config error, permission issue

**Fix**

```bash
systemctl restart <service>
```

**Prevention**

* Test services after reboot
* Proper systemd dependencies

---

### 3️⃣ High system load average

**Investigation**

```bash
uptime
top
htop
```

**Root Cause**

* CPU‑bound process, I/O wait, memory pressure

**Fix**

* Kill or optimize heavy process
* Scale resources

**Prevention**

* Performance monitoring
* Capacity planning

---

### 4️⃣ Process consuming 100% CPU

**Investigation**

```bash
top
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu
```

**Root Cause**

* Application bug or infinite loop

**Fix**

```bash
kill -9 <pid>
```

**Prevention**

* Resource limits
* Code optimization

---

### 5️⃣ Slow SSH login

**Investigation**

```bash
ssh -vvv user@server
```

**Root Cause**

* DNS resolution delay, PAM modules, high load

**Fix**

```bash
UseDNS no
```

**Prevention**

* SSH config tuning
* Load monitoring

---

### 6️⃣ Cron job not running

**Investigation**

```bash
crontab -l
grep CRON /var/log/syslog
```

**Root Cause**

* Wrong path, missing env vars, permissions

**Fix**

* Use full paths
* Redirect output to logs

**Prevention**

* Manual testing
* Logging

---

### 7️⃣ File system mounted as read‑only

**Investigation**

```bash
mount | grep ro
dmesg | tail
```

**Root Cause**

* Disk errors

**Fix**

```bash
fsck /dev/<disk>
mount -o remount,rw /
```

**Prevention**

* Disk health checks
* Backups

---

### 8️⃣ Ping works but SSH fails

**Investigation**

```bash
systemctl status sshd
iptables -L
```

**Root Cause**

* Firewall or SSH service down

**Fix**

* Restart SSH
* Open port 22

**Prevention**

* Firewall validation
* Service monitoring

---

### 9️⃣ Memory leak on Linux system

**Investigation**

```bash
free -m
top
ps aux --sort=-%mem
```

**Root Cause**

* Application not releasing memory

**Fix**

* Restart app
* Tune memory

**Prevention**

* Memory profiling
* Alerts

---

### 🔟 System time out of sync

**Investigation**

```bash
timedatectl
```

**Root Cause**

* NTP disabled

**Fix**

```bash
timedatectl set-ntp true
```

**Prevention**

* NTP enabled everywhere

---

### 1️⃣1️⃣ Disk shows free but app says full

**Investigation**

```bash
df -h
lsof | grep deleted
```

**Root Cause**

* Deleted files held by running process

**Fix**

* Restart process

**Prevention**

* Proper log rotation

---

### 1️⃣2️⃣ Log file growing too fast

**Investigation**

```bash
ls -lh /var/log
```

**Root Cause**

* Debug logging enabled

**Fix**

* Reduce log level
* Rotate logs

**Prevention**

* Log size limits

---

### 1️⃣3️⃣ Permission issues

**Investigation**

```bash
ls -l
getfacl <file>
```

**Root Cause**

* Wrong ownership/permissions

**Fix**

```bash
chown user:group file
chmod 755 file
```

**Prevention**

* Permission standards

---

### 1️⃣4️⃣ Unexpected reboot

**Investigation**

```bash
last reboot
journalctl -b -1
```

**Root Cause**

* Kernel panic, OOM, hardware

**Fix**

* Fix root cause

**Prevention**

* Monitoring

---

### 1️⃣5️⃣ Intermittent network connectivity

**Investigation**

```bash
ping
traceroute
netstat -i
```

**Root Cause**

* NIC or network issues

**Fix**

* Restart network

**Prevention**

* Network monitoring

---

### 1️⃣6️⃣ Package dependency failure

**Investigation**

```bash
apt-get -f install
```

**Root Cause**

* Broken dependencies

**Fix**

* Reinstall packages

**Prevention**

* Stable repos

---

### 1️⃣7️⃣ High swap usage

**Investigation**

```bash
swapon -s
free -m
```

**Root Cause**

Low RAM

**Fix**

* Optimize memory
* Add RAM

**Prevention**

* Memory alerts

---

### 1️⃣8️⃣ Zombie process

**Investigation**

```bash
ps aux | grep Z
```

**Root Cause**

* Parent not reaping child

**Fix**

* Restart parent

**Prevention**

* Proper process handling

---

### 1️⃣9️⃣ Server unreachable after firewall change

**Investigation**

* Access via console

**Fix**

```bash
iptables -F
```

**Prevention**

* Test firewall rules

---

### 2️⃣0️⃣ Kernel panic

**Investigation**

```bash
journalctl -k
```

**Root Cause**

* Driver or hardware issue

**Fix**

* Update kernel/drivers

**Prevention**

* Stable kernels

---

## 🔹 Git / GitHub Troubleshooting (21–30)

### 2️⃣1️⃣ Git push fails due to conflicts

**Investigation**

```bash
git status
git pull origin main
```

**Root Cause**

* Remote branch has new commits conflicting with local changes

**Fix**

```bash
git pull --rebase origin main
# resolve conflicts
git add .
git rebase --continue
git push
```

**Prevention**

* Pull frequently
* Use feature branches

### 2️⃣2️⃣ Force-push to main broke production

**Investigation**

```bash
git log --oneline
git reflog
Root Cause
```

History rewritten using git push --force

**Fix**

Roll back to last good commit:

```bash
git reset --hard <commit>
git push --force
```

**Prevention**

* Protect main branch
* Disable force push

### 2️⃣3️⃣ GitHub Actions workflow fails intermittently

**Investigation**

* Check Actions logs
* Look for flaky tests or timeouts

**Root Cause**

* Network issues, race conditions, external dependencies

**Fix**

* Add retries
* Increase timeouts

**Prevention**

* Stable test data
* Isolate external dependencies

### 2️⃣4️⃣ Secrets accidentally pushed to repo

**Investigation**

```bash
git log
```

**Root Cause**

* Secrets committed in code

**Fix**

* Rotate secrets immediately
* Remove from history:

    ```bash
    git filter-repo
    ```

**Prevention**

* Use .gitignore
* Use secret scanners

### 2️⃣5️⃣ Frequent merge conflicts in team

**Investigation**

* Analyze branch workflow

**Root Cause**

* Long-lived branches
* Multiple devs editing same files

**Fix**

* Rebase often
* Smaller PRs

**Prevention**

* Git flow strategy
* Code ownership

### 2️⃣6️⃣ Git repo slow to clone

**Investigation**

```bash
git count-objects -vH
```

**Root Cause**

* Large files or history

**Fix**

* Use Git LFS
* Shallow clone:

```bash
git clone --depth=1
```

**Prevention**

* Avoid committing binaries

### 2️⃣7️⃣ CI triggered multiple times unnecessarily

**Investigation**

* Check workflow triggers

**Root Cause**

* Multiple triggers (push + PR)

**Fix**

* Refine on: conditions in YAML

**Prevention**

* Clear pipeline design

### 2️⃣8️⃣ Messy Git history

**Investigation**

```bash
git log --oneline
```

**Root Cause**

* Too many merge commits

**Fix**

```bash
git rebase -i
```

**Prevention**

* Squash commits
* PR rules

### 2️⃣9️⃣ Deleted branch recovery

**Investigation**

```bash
git reflog
```

**Fix**

```bash
git checkout -b recovered <commit
```

**Prevention**

* Tag important releases

### 3️⃣0️⃣ GitHub webhook not triggering pipeline

**Investigation**

* Check webhook delivery logs
* Verify payload URL

**Root Cause**

* Wrong URL or secret mismatch

**Fix**

* Fix webhook config

**Prevention**

* Test webhooks regularly

---

## 🔹 Docker Troubleshooting (31–40)

### 3️⃣1️⃣ Container exits immediately

**Investigation***

```bash
docker ps -a
docker logs <container>
```

**Root Cause**

* Application crashes or wrong CMD

**Fix**

* Fix entrypoint or app error

**Prevention**

* Proper health checks

### 3️⃣2️⃣ Docker image size very large

**Investigation**

```bash
docker images
```

**Root Cause**

* Unused layers, large base image

**Fix**

* Use multi-stage builds
* Use slim images

**Prevention**

* Optimize Dockerfile

### 3️⃣3️⃣ Containers cannot communicate

**Investigation**

```bash
docker network ls
docker inspect <container>
```

**Root Cause**

* Different networks

**Fix**

* Attach containers to same network

**Prevention**

* Use Docker Compose

### 3️⃣4️⃣ Docker build fails: no space left

**Investigation**

```bash
docker system df
```

**Fix**

```bash
docker system prune -a
```

**Prevention**

* Cleanup jobs
* Disk alerts

### 3️⃣5️⃣ Container running but app inaccessible

**Investigation**

```bash
docker port <container>
docker logs <container>
```

**Root Cause**

* Wrong port mapping or app binding to localhost

**Fix**

* Bind app to 0.0.0.0

**Prevention**

* Document exposed ports

### 3️⃣6️⃣ Env variables not available in container

**Investigation**

```bash
docker inspect <container>
```

**Root Cause**

* Missing -e flag or env file

**Fix**

```bash
docker run -e VAR=value
```

**Prevention**

* Use .env files

### 3️⃣7️⃣ Container uses too much memory

**Investigation**

```bash
docker stats
```

**Root Cause**

* No memory limits

**Fix**

```bash
docker run --memory=512m
```

**Prevention**

* Enforce resource limits

### 3️⃣8️⃣ Docker daemon not starting

**Investigation**

```bash
systemctl status docker
journalctl -u docker
```

**Root Cause**

* Config error or disk full

**Fix**

* Fix config
* Free disk space

**Prevention**

* Monitor daemon health

### 3️⃣9️⃣ Volume data missing after restart

**Investigation**

```bash
docker volume ls
```

**Root Cause**

* Anonymous volume used

**Fix**

* Use named volumes

**Prevention**

* Explicit volume definitions

### 4️⃣0️⃣ Containers work locally but fail in prod

**Investigation**

* Compare configs
* Check environment differences

**Root Cause**

* Env vars, permissions, architecture mismatch

**Fix**

* Align environments

**Prevention**

* Infrastructure as Code
* Same image everywhere

---

**🔥 Interview Pro Tip (Very Important)**

* For Git + Docker questions, always mention:

    * Logs

    * Config files

    * Rollback strategy

---

## 🔹 Docker Advanced Troubleshooting (41–50)

### 4️⃣1️⃣ Docker logs not appearing

**Investigation**

```bash
docker inspect <container> | grep LogConfig
docker logs <container>
```

**Root Cause**

* App logs written to file instead of stdout/stderr

**Fix**

* Update app to log to stdout/stderr
* Or mount log volume

**Prevention**

* Follow 12-factor logging

### 4️⃣2️⃣ Container keeps restarting (CrashLoop)

**Investigation**

```bash
docker ps -a
docker logs <container>
```

**Root Cause**

* App crash, wrong CMD/ENTRYPOINT

**Fix**

* Fix application error
* Correct Dockerfile CMD

**Prevention**

* Health checks
* Test containers locally

### 4️⃣3️⃣ Docker networking not working

**Investigation**

```bash
docker network inspect bridge
iptables -L
```

**Root Cause**

* Network misconfiguration or firewall rules

**Fix**

* Recreate network
* Restart Docker

**Prevention**

* Use user-defined networks

### 4️⃣4️⃣ DNS issues inside container

**Investigation**

```bash
cat /etc/resolv.conf
nslookup google.com
```

**Root Cause**

* Host DNS issue or Docker DNS misconfig

**Fix**

```bash
docker run --dns 8.8.8.8
```

**Prevention**

* Stable DNS config

### 4️⃣5️⃣ Image pulls are slow

**Investigation**

* Check registry response time
* Check network latency

**Root Cause**

* Large image size, no caching

**Fix**

* Use smaller base images
* Enable registry mirror

**Prevention**

* Image optimization
* Local caching

### 4️⃣6️⃣ Orphaned Docker resources

**Investigation**

```bash
docker system df
```

**Root Cause**

* Unused containers, images, volumes

**Fix**

```bash
docker system prune
```

**Prevention**

* Scheduled cleanup jobs

### 4️⃣7️⃣ Container cannot access internet

**Investigation**

```bash
docker exec -it <container> ping 8.8.8.8
```

**Root Cause**

* NAT/firewall rules blocking traffic

**Fix**

* Fix iptables
* Restart Docker service

**Prevention**

* Network rule testing

### 4️⃣8️⃣ Permission denied inside container

**Investigation**

```bash
id
ls -l
```
**Root Cause**

* UID/GID mismatch, non-root user

**Fix**

* Adjust file permissions
* Match host UID

**Prevention**

* Proper user mapping

### 4️⃣9️⃣ Docker Compose services start in wrong order

**Investigation**

* Check docker-compose.yml

**Root Cause**

* depends_on doesn’t wait for readiness

**Fix**

* Add health checks
* Use wait-for scripts

**Prevention**

* Readiness checks

### 5️⃣0️⃣ Multi-stage Docker build issues

**Investigation**

```bash
docker build --progress=plain .
```

**Root Cause**

* Missing artifacts between stages

**Fix**

* Correct COPY --from=builder

**Prevention**

* Validate each stage separately

---

## 🔹 Kubernetes Troubleshooting (51–60)

### 5️⃣1️⃣ Pod stuck in Pending

**Investigation**

```bash
kubectl describe pod <pod>
```

**Root Cause**

* Insufficient CPU/memory, PVC not bound

**Fix**

* Add nodes or reduce requests

**Prevention**

* Resource planning

### 5️⃣2️⃣ Pod in CrashLoopBackOff

**Investigation**

```bash
kubectl logs <pod>
kubectl describe pod <pod>
```

**Root Cause**

* App crash, config error

**Fix**

* Fix config or image

**Prevention**

* Liveness probes

### 5️⃣3️⃣ Service not accessible externally

**Investigation**

```bash
kubectl get svc
```

**Root Cause**

* Wrong service type or port

**Fix**

* Use LoadBalancer or NodePort

**Prevention**

* Service validation

### 5️⃣4️⃣ Pods running but traffic not reaching

**Investigation**

```bash
kubectl get endpoints
```

**Root Cause**

* Selector mismatch

**Fix**

* Fix labels/selectors

**Prevention**

* Label consistency

### 5️⃣5️⃣ Node in NotReady state

**Investigation**

```bash
kubectl describe node <node>
```

**Root Cause**

* Kubelet down, disk pressure

**Fix**

* Restart kubelet
* Free disk

**Prevention**

* Node monitoring

### 5️⃣6️⃣ Deployment not scaling

**Investigation**

```bash
kubectl describe deploy <deploy>
```

**Root Cause**

* HPA misconfig, resource limits

**Fix**

* Fix HPA thresholds

**Prevention**

* Load testing

### 5️⃣7️⃣ ConfigMap changes not reflected

**Investigation**

* Check pod restart

**Root Cause**

* ConfigMaps mounted at startup only

**Fix**

* Restart pods

**Prevention**

* Reload logic or rollout restart

### 5️⃣8️⃣ Secrets not injected

**Investigation**

```bash
kubectl describe pod <pod>
```

**Root Cause**

* Wrong secret name or key

**Fix**

* Fix secret reference

**Prevention**

* Validation checks

### 5️⃣9️⃣ Pod cannot pull image

**Investigation**

```bash
kubectl describe pod <pod>
```

**Root Cause**

* ImagePullSecret missing, wrong image tag

**Fix**

* Add secret or correct image

**Prevention**

* Image access testing

### 6️⃣0️⃣ Kubernetes cluster slow

**Investigation**

```bash
kubectl top nodes
kubectl top pods
```

**Root Cause**

* Resource exhaustion, etcd load

**Fix**

* Scale cluster
* Optimize workloads

**Prevention**

* Monitoring & capacity planning

---

**🔥 Interview Power Tip**

* For Docker + Kubernetes, always say:
    **“First I check logs and describe output, then verify configuration, then fix root cause.”**

---

## 🔹 Advanced Kubernetes Troubleshooting (61–75)

### 6️⃣1️⃣ High memory usage in a pod

**Investigation**

```bash
kubectl top pod <pod>
kubectl describe pod <pod>
```

**Root Cause**

* Memory leak or missing limits

**Fix**

* Set memory limits
* Optimize application

**Prevention**

* Enable HPA/VPA
* Memory monitoring

### 6️⃣2️⃣ Persistent Volume not mounting

**Investigation**

```bash
kubectl describe pvc <pvc>
```

**Root Cause**

* StorageClass mismatch or permissions

**Fix**

* Correct StorageClass
* Fix IAM permissions (cloud)

**Prevention**

* Validate storage configs

### 6️⃣3️⃣ Kubernetes API server not responding

**Investigation**

* Check control plane logs
* Check etcd health

**Root Cause**

* etcd issues, resource exhaustion

**Fix**

* Restart API server
* Fix etcd disk issues

**Prevention**

* Control-plane monitoring

### 6️⃣4️⃣ Pods evicted frequently

**Investigation**

```bash
kubectl describe pod <pod>
```

**Root Cause**

* Node memory/disk pressure

**Fix**

* Increase node capacity
* Adjust resource requests

**Prevention**

* Resource planning

### 6️⃣5️⃣ NetworkPolicies blocking traffic

**Investigation**

```bash
kubectl describe networkpolicy
```

**Root Cause**

* Missing allow rules

**Fix**

* Add correct ingress/egress rules

**Prevention**

* Policy testing

### 6️⃣6️⃣ Rolling update causes downtime

**Investigation**

```bash
kubectl describe deploy <deploy>
```

**Root Cause**

* Improper maxUnavailable

**Fix**

* Tune rolling update strategy

**Prevention**

* Readiness probes

### 6️⃣7️⃣ HPA not scaling pods

**Investigation**

```bash
kubectl describe hpa
```

**Root Cause**

* Metrics server missing or wrong thresholds

**Fix**

* Install metrics-server
* Fix metrics

**Prevention**

* HPA testing

### 6️⃣8️⃣ CoreDNS not resolving names

**Investigation**

```bash
kubectl logs -n kube-system coredns
```
**Root Cause**

* Misconfig or network issue

**Fix**

* Restart CoreDNS
* Fix ConfigMap

**Prevention**

* DNS monitoring

### 6️⃣9️⃣ Namespace deletion stuck

**Investigation**

```bash
kubectl get namespace <ns> -o json
```

**Root Cause**

* Finalizers blocking deletion

**Fix**

* Remove finalizers manually

**Prevention**

* Clean resource deletion

### 7️⃣0️⃣ Ingress created but not working

**Investigation**

```bash
kubectl describe ingress
```

**Root Cause**

* Ingress controller missing or misconfigured

**Fix**

* Deploy ingress controller
* Fix annotations

**Prevention**

* Controller validation

### 7️⃣1️⃣ Pods restart after node reboot

**Investigation**

* Check pod controllers

**Root Cause**

* No controller managing pods

**Fix**

* Use Deployment/StatefulSet

**Prevention**

* Controller-based workloads

### 7️⃣2️⃣ RBAC permission denied

**Investigation**

```bash
kubectl auth can-i <action>
```

**Root Cause**

* Missing role or binding

**Fix**

* Update Role/ClusterRole

**Prevention**

* RBAC audits

### 7️⃣3️⃣ Job or CronJob failing repeatedly

**Investigation**

```bash
kubectl logs job/<job>
```

**Root Cause**

* Script error, timeout

**Fix**

* Fix job logic
* Increase retries

**Prevention**

* Test jobs manually

### 7️⃣4️⃣ etcd disk full

**Investigation**

* Check etcd disk usage

**Root Cause**

* Too many objects, no compaction

**Fix**

* Run etcd compaction
* Increase disk

**Prevention**

* etcd monitoring

### 7️⃣5️⃣ Kubernetes certificate expired

**Investigation**

```bash
kubeadm certs check-expiration
```

**Root Cause**

* Cert not rotated

**Fix**

```bash
kubeadm certs renew all
```

**Prevention**

* Automated cert rotation

## 🔹 Serverless Troubleshooting (76–80)

### 7️⃣6️⃣ Lambda function timing out

**Investigation**

* Check CloudWatch logs

**Root Cause**

* Long execution, VPC cold start

**Fix**

* Increase timeout
* Optimize code

**Prevention**

* Performance tuning

### 7️⃣7️⃣ Lambda cannot access internet

**Investigation**

* Check VPC config

**Root Cause**

* No NAT Gateway

**Fix**

* Add NAT Gateway

**Prevention**

* VPC design review

### 7️⃣8️⃣ Lambda permission denied

**Investigation**

* Check IAM role

**Root Cause**

* Missing permissions

**Fix**

* Attach correct policy

**Prevention**

* Least privilege review

### 7️⃣9️⃣ API Gateway returns 5xx

**Investigation**

* Check API Gateway logs

**Root Cause**

* Lambda error or timeout

**Fix**

* Fix Lambda logic

**Prevention**

* Monitoring & retries

### 8️⃣0️⃣ Lambda concurrency limit reached

**Investigation**

* Check concurrency metrics

**Root Cause**

* Spike in traffic

**Fix**

* Increase concurrency
* Throttle requests

**Prevention**

* Traffic planning

---

**🔥 Interview Confidence Tip (Important)**

* When answering advanced Kubernetes questions, always say:
    **“I check describe output first, then logs, then cluster metrics.”**

---

## 🔹 AWS Troubleshooting (81–90)

### 8️⃣1️⃣ Load balancer works but backend unhealthy

**Investigation**

* Check target group health checks
* Verify security groups

**Root Cause**

* Wrong health check path/port

**Fix**

* Update health check config

**Prevention**

* Test health endpoints

### 8️⃣2️⃣ EC2 instance running but very high billing

**Investigation**

* Check instance type and usage
* Review EBS snapshots

**Root Cause**

* Over-provisioned instance or unused resources

**Fix**

* Resize instance
* Delete unused resources

**Prevention**

* Cost monitoring & budgets

### 8️⃣3️⃣ CloudWatch alarms not triggering

**Investigation**

* Verify metric namespace & threshold

**Root Cause**

* Wrong metric or evaluation period

**Fix**

* Correct alarm configuration

**Prevention**

* Alarm testing

### 8️⃣4️⃣ Auto Scaling Group not launching instances

**Investigation**

* Check ASG activity history

**Root Cause**

* Launch template error, IAM role missing

**Fix**

* Fix launch template

**Prevention**

* Template validation

### 8️⃣5️⃣ Application behind ALB returns 502

**Investigation**

* Check target health
* Review ALB logs

**Root Cause**

* App crash or timeout

**Fix**

* Fix backend app

**Prevention**

* Proper timeouts & retries

### 8️⃣6️⃣ IAM user has permissions but gets Access Denied

**Investigation**

* Check IAM policy simulator

**Root Cause**

* Explicit deny or SCP

**Fix**

* Remove deny

**Prevention**

* Policy reviews

### 8️⃣7️⃣ EC2 instance unreachable

**Investigation**

* Check security groups, NACLs, route tables

**Root Cause**

* Network misconfiguration

**Fix**

* Fix network rules

**Prevention**

* Network design review

### 8️⃣8️⃣ EKS pods cannot access AWS services

**Investigation**

* Check IAM role for service account (IRSA)

**Root Cause**

* Missing permissions

**Fix**

* Attach correct IAM policy

**Prevention**

* Use IRSA consistently

### 8️⃣9️⃣ Route 53 DNS not resolving

**Investigation**

* Check record type & TTL

**Root Cause**

* Wrong record or propagation delay

**Fix**

* Correct DNS record

**Prevention**

* DNS validation

### 9️⃣0️⃣ AWS API calls fail intermittently

**Investigation**

* Check throttling metrics

**Root Cause**

* API rate limits

**Fix**

* Implement retries & backoff

**Prevention**

* Request optimization

---

## 🔹 CI/CD Troubleshooting (91–100)

### 9️⃣1️⃣ CI passes locally but fails in pipeline

**Investigation**

* Compare environment variables
* Check runtime versions

**Root Cause**

* Environment mismatch

**Fix**

* Align CI environment

**Prevention**

* Use containers in CI

### 9️⃣2️⃣ Deployment successful but app crashes

**Investigation**

* Check application logs
* Verify config/secrets

**Root Cause**

* Missing config or runtime error

**Fix**

* Fix config or rollback

**Prevention**

* Smoke tests

### 9️⃣3️⃣ Rollback fails after bad deployment

**Investigation**

* Check deployment history

**Root Cause**

* No rollback strategy defined

**Fix**

* Manual rollback

**Prevention**

* Automated rollback strategy

### 9️⃣4️⃣ Blue-Green deployment causes downtime

**Investigation**

* Check traffic switch timing

**Root Cause**

* Health checks incomplete

**Fix**

* Wait for green to be healthy

**Prevention**

* Automated verification

### 9️⃣5️⃣ Canary deployment errors for some users

**Investigation**

* Compare versions & logs

**Root Cause**

* Version-specific bug

**Fix**

* Roll back canary

**Prevention**

* Better canary testing

### 9️⃣6️⃣ Jenkins job stuck in queue

**Investigation**

* Check executor availability

**Root Cause**

* No free agents

**Fix**

* Add executors or agents

**Prevention**

* Auto-scaling agents

### 9️⃣7️⃣ Pipeline execution suddenly slow

**Investigation**

* Compare recent pipeline runs

**Root Cause**

* New step or dependency slowdown

**Fix**

* Optimize slow stages

**Prevention**

* Pipeline metrics

### 9️⃣8️⃣ Secrets exposed in pipeline logs

**Investigation**

* Check log output

**Root Cause**

* Secrets echoed or not masked

**Fix**

* Mask secrets

* Rotate credentials

**Prevention**

* Secure secret handling

### 9️⃣9️⃣ Environment-specific failures

**Investigation**

* Compare configs across environments

**Root Cause**

* Config drift

**Fix**

* Standardize configs

**Prevention**

* IaC

### 1️⃣0️⃣0️⃣ Production issue but logs missing

**Investigation**

* Check logging agent & disk space

**Root Cause**

* Logging misconfiguration

**Fix**

* Restore logging
* Enable debug temporarily

**Prevention**

* Centralized logging

---

📌 **Use this README as your daily interview practice guide.**

---

## 🎯 Final Interview Advice

When answering **any** troubleshooting question, always mention:

* Logs
* Metrics
* Rollback
* Prevention

If you do that consistently, **you will pass DevOps interviews with confidence** 💪