
# Linux, AWS, and DevOps Scenario-Based Interview Questions & Answers

All questions are practical, real-production style, not theory-only.

---

## Contents

- [Linux Scenarios](#linux-scenarios-35-questions)
- [AWS Scenarios](#aws-scenarios-35-questions)
- [DevOps Scenarios](#devops-scenarios-30-questions)
- [Linux Answers](#linux-answers-1-35)
- [AWS Answers](#aws-answers-36-60)
- [Serverless and DevOps Answers](#serverless-and-devops-answers-61-80)
- [Kubernetes and Advanced DevOps Answers](#kubernetes-and-advanced-devops-answers-81-100)
- [Interview Tips](#interview-tips)

---

## Linux Scenarios (35 Questions)

1. A production server is running out of disk space suddenly. How will you identify which directory or file is consuming space?
2. An application is running but users report slow performance. How do you check CPU, memory, and I/O usage?
3. A service is not starting after reboot. How will you troubleshoot it?
4. You accidentally deleted a critical file. What steps can you take to recover it?
5. A user says they cannot SSH into the server. How do you debug?
6. How do you find which process is using a specific port?
7. A cron job didn’t run last night. How will you investigate?
8. How do you monitor logs in real time for errors?
9. The root filesystem is 100% full. SSH works but commands fail. What will you do?
10. You want to restrict a user to only SFTP access. How will you configure it?
11. How do you change file permissions so only the owner can read and write?
12. An application requires Java 11 but the system has Java 8. How do you handle this?
13. How do you identify zombie processes and clean them?
14. A server reboots automatically every night. How will you find the reason?
15. How do you schedule a script to run every Sunday at midnight?
16. A service crashes intermittently. How do you capture logs before it crashes?
17. How do you check last login details of a user?
18. How do you permanently set an environment variable for all users?
19. A file is growing very fast (log file). How will you rotate logs?
20. How do you safely restart a production server?
21. How do you compare two configuration files?
22. How do you find files modified in the last 24 hours?
23. A script works manually but fails in cron. Why?
24. How do you limit CPU or memory usage for a process?
25. How do you check system boot time and services started?
26. How do you kill a hung process gracefully?
27. How do you set up password-less SSH login?
28. How do you check kernel version and OS details?
29. How do you track who deleted or modified a file?
30. A disk is added to the server. How do you mount it permanently?
31. How do you test network connectivity to another server?
32. How do you find open files of a process?
33. A package installation fails due to dependency issues. How do you fix it?
34. How do you secure SSH from brute-force attacks?
35. How do you automate daily backups using shell scripts?

---

## AWS Scenarios (35 Questions)

36. Your EC2 instance is unreachable. What checks will you perform?
37. An application needs high availability. Which AWS services will you use?
38. How do you reduce AWS EC2 cost without affecting performance?
39. A private EC2 instance needs internet access. How do you design it?
40. How do you secure S3 buckets from public access?
41. Your ALB is returning 502 errors. How do you troubleshoot?
42. How do you design a highly available RDS setup?
43. How do you rotate IAM access keys safely?
44. An EC2 instance is compromised. What immediate actions will you take?
45. How do you migrate an on-prem app to AWS?
46. How do you implement auto scaling based on CPU usage?
47. How do you store secrets securely in AWS?
48. How do you monitor EC2 disk usage?
49. An S3 bucket was accidentally deleted. How do you recover data?
50. How do you restrict IAM users to read-only S3 access?
51. How do you design VPC networking for production?
52. How do you allow one AWS account to access resources in another?
53. How do you troubleshoot slow application response in AWS?
54. How do you implement blue-green deployment in AWS?
55. How do you handle SSL certificates in AWS?
56. How do you back up EC2 instances automatically?
57. How do you identify unused AWS resources?
58. How do you deploy a containerized app on AWS?
59. How do you restrict SSH access to EC2?
60. How do you monitor AWS costs and set alerts?
61. How do you design a serverless architecture?
62. How do you troubleshoot Lambda timeout errors?
63. How do you ensure data encryption at rest and in transit?
64. How do you set up CloudWatch alarms?
65. How do you protect your AWS account from accidental deletion?
66. How do you design disaster recovery in AWS?
67. How do you migrate RDS from single-AZ to multi-AZ?
68. How do you debug IAM permission issues?
69. How do you expose a private app securely to the internet?
70. How do you implement CI/CD using AWS services?

---

## DevOps Scenarios (30 Questions)

71. A build fails in CI but works locally. How do you debug?
72. How do you design a CI/CD pipeline from scratch?
73. How do you roll back a failed production deployment?
74. How do you manage environment-specific configurations?
75. How do you version infrastructure changes?
76. How do you ensure zero downtime deployment?
77. How do you monitor application health in production?
78. How do you secure secrets in CI/CD pipelines?
79. A Docker container exits immediately. How do you debug?
80. How do you reduce Docker image size?
81. How do you handle database migrations during deployment?
82. How do you implement infrastructure as code?
83. How do you enforce code quality in CI?
84. How do you scale applications automatically?
85. How do you implement centralized logging?
86. How do you handle configuration drift?
87. How do you manage multiple environments (dev/test/prod)?
88. How do you ensure pipeline security?
89. How do you debug Kubernetes pod crash loops?
90. How do you expose services in Kubernetes?
91. How do you manage secrets in Kubernetes?
92. How do you handle node failure in Kubernetes?
93. How do you monitor containers and clusters?
94. How do you perform canary deployments?
95. How do you handle high traffic spikes?
96. How do you design DevOps for microservices?
97. How do you implement automated testing?
98. How do you handle production incidents?
99. How do you document DevOps processes?
100. How do you measure DevOps success (metrics/KPIs)?

---

### How to Use This (Recommended for You, Sanket)

Since you’re preparing for a DevOps fresher → job-ready:

- Practice 5 questions/day.
- Answer out loud (improves confidence & English).

---

## Linux Answers (1-35)

Below are clear, real-world, interview-ready answers for Linux Questions 1–35.  
They are written in practical steps, exactly how you should explain in an interview.

### 1. Production server running out of disk space

Steps:

```bash
df -h
du -sh /* 2>/dev/null
du -sh /var/* | sort -h
```

- Identify the filesystem that is full.
- Drill down to directories/files.
- Check logs, Docker data, old backups.
- Clean or archive unnecessary files.

### 2. Application slow – check CPU, memory, I/O

```bash
top
htop
free -m
vmstat 1
iostat
```

- High CPU → CPU-bound.
- High memory usage → possible memory leak.
- High I/O wait → disk bottleneck.

### 3. Service not starting after reboot

```bash
systemctl status service-name
journalctl -xe
systemctl restart service-name
```

- Check logs.
- Validate config files.
- Verify dependencies and ports.

### 4. Accidentally deleted a critical file

Possible recovery:

- Restore from backup.
- Check editor swap files (`.swp`).
- If file is still open by a process:

```bash
lsof | grep deleted
```

- As a last option: use filesystem recovery tools (e.g. `extundelete`).

### 5. User cannot SSH into server

Checks:

```bash
systemctl status sshd
ss -tlnp | grep 22
tail -f /var/log/auth.log
```

- Validate user permissions.
- Check key/password.
- Check security group / firewall.

### 6. Find process using a port

```bash
ss -tulnp | grep :8080
lsof -i :8080
```

### 7. Cron job didn’t run

```bash
crontab -l
grep CRON /var/log/syslog
```

- Check script path.
- Check environment variables.
- Verify permissions.
- Redirect output to log.

### 8. Monitor logs in real time

```bash
tail -f /var/log/syslog
journalctl -u service-name -f
```

### 9. Root filesystem 100% full

Actions:

- Remove large logs.
- Clear temp files:

```bash
rm -rf /tmp/*
```

- Delete unused packages.
- Configure and run log rotation.

### 10. Restrict user to only SFTP

Update SSH config (`/etc/ssh/sshd_config`):

```text
Match User sftpuser
  ChrootDirectory /sftp
  ForceCommand internal-sftp
```

Then restart SSH:

```bash
systemctl restart sshd
```

### 11. Only owner can read/write

```bash
chmod 600 file.txt
```

### 12. App needs Java 11 but system has Java 8

```bash
sudo apt install openjdk-11-jdk   # or equivalent
sudo update-alternatives --config java
```

Set `JAVA_HOME` per application if required.

### 13. Identify zombie processes

```bash
ps aux | grep Z
```

Then kill the parent process if appropriate and restart the service if needed.

### 14. Server reboots automatically

```bash
last reboot
journalctl --since "yesterday"
```

- Check cron.
- Check for hardware issues.
- Look for kernel panic.
- Review cloud provider events.

### 15. Schedule script every Sunday midnight

Crontab entry:

```text
0 0 * * 0 /path/script.sh
```

### 16. Service crashes intermittently

- Enable debug logs.
- Capture logs:

```bash
journalctl -u service-name
```

- Check for memory leaks and core dumps.

### 17. Check last login of user

```bash
last username
lastlog -u username
```

### 18. Set environment variable for all users

Temporary:

```bash
export VAR=value
```

Permanent (for all users), add to:

```text
/etc/profile
/etc/environment
```

### 19. Log file growing very fast

Configure `logrotate` and run a rotation:

```bash
logrotate -f /etc/logrotate.conf
```

- Compress and rotate logs.

### 20. Safely restart a production server

Steps:

- Notify stakeholders.
- Stop critical services gracefully.
- Take backup/snapshot.
- Restart:

```bash
reboot
```

- Validate services post-restart.

### 21. Compare two configuration files

```bash
diff file1.conf file2.conf
vimdiff file1.conf file2.conf
```

Use this to identify config changes before deployment.

### 22. Find files modified in last 24 hours

```bash
find /path -type f -mtime -1
```

### 23. Script works manually but fails in cron

Common reasons:

- `PATH` not set.
- Missing permissions.
- Using relative paths.

Fix (use full paths and log output):

```text
*/5 * * * * /bin/bash /full/path/script.sh >> /tmp/cron.log 2>&1
```

### 24. Limit CPU or memory usage

Using `nice`:

```bash
nice -n 10 command
```

Using cgroups via `systemd`:

```bash
systemd-run --scope -p MemoryLimit=500M command
```

### 25. Check system boot time & services

```bash
uptime
systemd-analyze
systemctl list-units --type=service
```

### 26. Kill hung process gracefully

```bash
kill PID
kill -9 PID   # last option
```

### 27. Setup password-less SSH

```bash
ssh-keygen
ssh-copy-id user@server
```

### 28. Check kernel & OS details

```bash
uname -a
cat /etc/os-release
```

### 29. Track who deleted or modified a file

Enable `auditd`:

```bash
auditctl -w /path/file -p wa
ausearch -f /path/file
```

### 30. Mount disk permanently

```bash
lsblk
blkid
```

Edit `/etc/fstab`:

```text
UUID=xxx /data ext4 defaults 0 0
```

Then:

```bash
mount -a
```

### 31. Test network connectivity

```bash
ping server
telnet host port
nc -zv host port
```

### 32. Find open files of a process

```bash
lsof -p PID
```

### 33. Package install fails (dependency issues)

```bash
yum deplist package
apt --fix-broken install
```

- Update repo cache.
- Resolve conflicting versions.

### 34. Secure SSH from brute force

- Disable root login.
- Use key-based authentication.
- Change default SSH port.
- Install `fail2ban`.

### 35. Automate daily backups

Shell script + cron:

```bash
tar -czf backup.tar.gz /data
```

Crontab:

```text
0 1 * * * /backup.sh
```

---

## AWS Answers (36-60)

### 36. EC2 unreachable

Checks:

- Instance state.
- Security Group.
- NACL.
- Route table.
- SSH service.
- Disk full.

### 37. High availability architecture

- ALB + Auto Scaling.
- Multi-AZ RDS.
- S3 + CloudFront.
- Health checks.

### 38. Reduce EC2 cost

- Right-size instances.
- Use Spot / Reserved Instances.
- Auto Scaling.
- Stop unused resources.

### 39. Private EC2 needs internet

- Place a NAT Gateway in a public subnet.
- Route private subnet traffic → NAT Gateway.

### 40. Secure S3 bucket

- Block public access.
- Use IAM policies.
- Use a restrictive bucket policy.
- Enable encryption & versioning.

---

## Interview Tips

### Interview Confidence Tip

When answering:

> In production, I would first check… then validate… finally fix…

This sounds senior & practical.

---

### AWS – Deep Scenario Answers (41–60)

#### 41. ALB returning 502 errors

Possible causes & checks:

- Target instances unhealthy.
- Application not listening on correct port.
- Security Group blocking traffic.
- Health check path incorrect.

Actions:

- Check ALB target group health.
- Verify app logs on EC2.
- Confirm listener & target port.

#### 42. Design highly available RDS

- Enable Multi-AZ.
- Use Read Replicas for read scaling.
- Enable automated backups.
- Monitor with CloudWatch.

#### 43. Rotate IAM access keys safely

Steps:

1. Create new access key.
2. Update applications.
3. Test access.
4. Disable old key.
5. Delete old key.

#### 44. EC2 compromised

Immediate actions:

- Detach instance from ALB.
- Take snapshot for forensics.
- Change IAM credentials.
- Patch vulnerabilities.
- Rebuild instance (preferred).

#### 45. Migrate on-prem app to AWS

- Assess app dependencies.
- Choose migration strategy (Rehost / Refactor).
- Setup VPC & networking.
- Migrate data.
- Test & cutover.

#### 46. Auto Scaling based on CPU

- Create Launch Template.
- Configure Auto Scaling Group.
- Set CloudWatch CPU alarm.
- Define scale out/in rules.

#### 47. Securely store secrets

- AWS Secrets Manager.
- AWS SSM Parameter Store.
- IAM-based access.
- Never hardcode secrets.

#### 48. Monitor EC2 disk usage

- Install and configure CloudWatch Agent.
- Publish custom metrics.
- Create disk usage alarms.
- Combine with OS-level monitoring.

#### 49. S3 bucket accidentally deleted

Recovery options:

- Restore from backup.
- Enable Versioning (prevention).
- Use cross-region replication.

#### 50. Read-only S3 IAM access

Example policy:

```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:ListBucket"],
  "Resource": "*"
}
```

#### 51. Production VPC design

- Public & private subnets.
- NAT Gateway.
- Multi-AZ.
- NACL + Security Groups.
- VPC Flow Logs.

#### 52. Cross-account access

- Create IAM Role.
- Define trust relationship.
- Use STS `AssumeRole`.

#### 53. Troubleshoot slow AWS app

- Check CloudWatch metrics.
- Check ELB latency.
- Check EC2 CPU/memory.
- Check RDS performance.
- Look for network bottlenecks.

#### 54. Blue-Green deployment

- Maintain two environments (blue & green).
- Route traffic via ALB / Route53.
- Switch traffic after validation.
- Rollback by reverting traffic.

#### 55. SSL certificates

- Use AWS Certificate Manager (ACM).
- Attach to ALB / CloudFront.
- Rely on auto-renewal.

#### 56. Automatic EC2 backups

- Use AWS Backup.
- Or Lambda + scheduled snapshots.
- Configure scheduled snapshot policies.

#### 57. Identify unused AWS resources

- Use Cost Explorer.
- Use Trusted Advisor.
- Find unattached EBS.
- Find idle EC2 / ELB.

#### 58. Deploy containerized app

- Use ECS / EKS.
- Use ECR for images.
- Integrate with load balancer.
- Enable auto scaling.

#### 59. Restrict SSH access

- Security Group with IP whitelisting.
- Use Bastion host.
- Disable password login.
- Use SSM Session Manager.

#### 60. Monitor AWS cost & alerts

- AWS Budgets.
- Cost Explorer.
- Billing alarms via CloudWatch.

---

### Interview Answer Framework (AWS)

**Issue → Service → Action → Result**

Example:

> 502 from ALB → checked target group → fixed health check → traffic restored

---

## Serverless and DevOps Answers (61-80)

### 61. Design a serverless architecture

- API Gateway → Lambda.
- DynamoDB / S3 for storage.
- IAM roles (least privilege).
- CloudWatch logs & alarms.
- Optional: Step Functions for workflows.

### 62. Lambda timing out

Checks:

- Increase timeout.
- Optimize code.
- Check VPC cold start.
- Increase memory (CPU scales with memory).

### 63. Encryption at rest & in transit

- At rest: KMS (EBS, S3, RDS).
- In transit: HTTPS / TLS.
- Enforce strict SSL policies.

### 64. Set up CloudWatch alarms

- Choose metric (CPU, memory).
- Define threshold.
- Configure SNS notifications.
- Test alarm.

### 65. Protect AWS account from accidental deletion

- Enable MFA.
- Use IAM permissions and roles.
- Use SCP (Service Control Policies).
- Disable direct root access.

### 66. Disaster recovery design

- Backups & snapshots.
- Multi-AZ / Multi-region.
- Pilot light / warm standby.
- Regular DR drills.

### 67. RDS Single-AZ to Multi-AZ

- Modify RDS instance.
- Enable Multi-AZ.
- Automatic failover.
- No app code change needed.

### 68. Debug IAM permission issues

- Use policy simulator.
- Review IAM policy & role.
- Look for explicit denies.
- Validate trust relationship.

### 69. Expose private app securely

- ALB + HTTPS.
- API Gateway.
- CloudFront + WAF.
- VPN or PrivateLink.

### 70. CI/CD using AWS

- CodeCommit / GitHub.
- CodeBuild.
- CodeDeploy.
- CodePipeline.
- Blue-Green deployment.

### 71. CI build fails but works locally

Reasons:

- Different environment.
- Missing dependencies.
- Wrong config files.

Fix:

- Match versions.
- Check logs.
- Use Docker builds.

### 72. Design CI/CD pipeline

- Code → Build → Test → Scan → Deploy.
- Automated triggers.
- Rollback strategy.
- Notifications.

### 73. Rollback failed deployment

- Deploy previous artifact/image.
- Use blue-green switch.
- Use `git revert`.
- Database rollback if needed.

### 74. Manage env-specific configs

- Environment variables.
- Config files.
- Parameter Store / Secrets Manager.

### 75. Version infrastructure changes

- Terraform / CloudFormation.
- Git version control.
- Plan → review → apply.

### 76. Zero downtime deployment

- Rolling updates.
- Blue-green.
- Canary deployments.
- Health checks.

### 77. Monitor app health

- Logs.
- Metrics.
- Alerts.
- Synthetic monitoring.

### 78. Secure secrets in CI/CD

- Never hardcode secrets.
- Use a secrets manager.
- Mask variables.
- Use least privilege IAM.

### 79. Docker container exits immediately

Debug:

```bash
docker logs container
docker inspect container
```

- Check `ENTRYPOINT` / `CMD`.
- Investigate app crash.

### 80. Reduce Docker image size

- Use slim base images.
- Use multi-stage builds.
- Remove unused packages.
- Use `.dockerignore`.

---

### How Interviewers Judge These Answers

They listen for:

- Production awareness.
- Security mindset.
- Automation & rollback thinking.

---

## Kubernetes and Advanced DevOps Answers (81-100)

### 81. Handle database migrations during deployment

- Use migration tools (Flyway, Liquibase).
- Run migrations before app rollout.
- Make migrations backward-compatible.
- Backup before changes.

### 82. Implement Infrastructure as Code

- Use Terraform / CloudFormation.
- Store code in Git.
- Use plan → review → apply.
- Enforce approvals.

### 83. Enforce code quality in CI

- Linting.
- Unit tests.
- Static code analysis (SonarQube).
- Fail pipeline on quality gates.

### 84. Scale applications automatically

- HPA (CPU / memory / custom metrics).
- Auto Scaling Groups.
- Load-based scaling.

### 85. Centralized logging

- EFK / ELK stack.
- CloudWatch Logs.
- Log aggregation & search.

### 86. Handle configuration drift

- Reapply IaC.
- Drift detection.
- Immutable infrastructure.
- Regular audits.

### 87. Manage multiple environments

- Separate accounts / namespaces.
- Environment-specific configs.
- Separate pipelines.

### 88. Ensure pipeline security

- Secrets management.
- Least privilege access.
- Dependency scanning.
- Image scanning.

### 89. Debug Kubernetes CrashLoopBackOff

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name> --previous
```

- Check app errors.
- Check resource limits.
- Check config issues.

### 90. Expose Kubernetes services

- `ClusterIP` (internal).
- `NodePort`.
- `LoadBalancer`.
- Ingress Controller.

### 91. Manage Kubernetes secrets

- Kubernetes `Secret` objects.
- External Secrets.
- Encrypt `etcd`.
- Use RBAC control.

### 92. Handle Kubernetes node failure

- Pod rescheduling.
- Auto Scaling Groups.
- Node health checks.
- Self-healing mechanisms.

### 93. Monitor containers & clusters

- Prometheus.
- Grafana.
- Metrics Server.
- Alerts.

### 94. Canary deployments

- Deploy to small % of users.
- Monitor metrics.
- Gradually increase traffic.
- Rollback if issues.

### 95. Handle traffic spikes

- Auto scaling.
- Caching.
- Rate limiting.
- CDN.

### 96. DevOps for microservices

- CI/CD per service.
- Service mesh.
- Centralized logging.
- Strong observability.

### 97. Automated testing

- Unit tests.
- Integration tests.
- Smoke tests.
- Run in pipeline.

### 98. Handle production incidents

- Detect → Respond → Mitigate.
- Rollback.
- Perform RCA (Root Cause Analysis).
- Prevent recurrence.

### 99. Document DevOps processes

- README.
- Architecture diagrams.
- Runbooks.
- SOPs.

### 100. Measure DevOps success (KPIs)

- Deployment frequency.
- Lead time.
- MTTR.
- Change failure rate.

---

### Final Interview Tip (Very Important)

When answering advanced DevOps questions, always include:

**Automation + Monitoring + Rollback**

This makes your answer sound senior & production-ready.

