# 🐧 Real-World Production Linux Scenarios (STAR Method)

---

## ⭐ Scenario 1: High CPU Usage on Production Server

**Interview Question:**
Tell me about a time when a Linux production server experienced high CPU usage. How did you handle it?

### ✅ STAR Answer

**S – Situation:**
In our production environment, a critical application server started experiencing 95–100% CPU utilization, causing application latency and timeout errors for users.

**T – Task:**
My responsibility was to identify the root cause quickly and restore system performance without downtime.

**A – Action:**

* Checked system load using `uptime` and `top`.
* Identified a Java process consuming excessive CPU.
* Used `ps aux --sort=-%cpu | head` to confirm.
* Analyzed logs in `/var/log/` and application logs.
* Found a memory leak causing excessive garbage collection.
* Restarted the service during a low-traffic window.
* Implemented monitoring using `htop` and configured alerts via CloudWatch.

**R – Result:**
CPU usage dropped to normal (30–40%), application response time improved, and we prevented recurrence by tuning JVM parameters.

---

## ⭐ Scenario 2: Disk Full in Production (100% Utilization)

**Interview Question:**
Describe a situation where your production server disk became full.

### ✅ STAR Answer

**S – Situation:**
Our production EC2 instance stopped accepting new requests. Investigation showed `/` partition was 100% full.

**T – Task:**
Free up disk space immediately and ensure minimal downtime.

**A – Action:**

* Checked disk usage using `df -h`.
* Used `du -sh /*` to identify large directories.
* Found `/var/log` consuming 20GB due to unrotated logs.
* Cleared old logs safely and compressed them.
* Configured `logrotate` for automated log rotation.

**R – Result:**
Freed 25GB space, restored application functionality, and prevented future disk-full incidents.

---

## ⭐ Scenario 3: Service Not Starting After Reboot

**Interview Question:**
Tell me about a time when a service failed to start after a system reboot.

### ✅ STAR Answer

**S – Situation:**
After a scheduled reboot, our Nginx service failed to start, causing downtime.

**T – Task:**
Quickly restore the web service.

**A – Action:**

* Checked service status using `systemctl status nginx`.
* Observed configuration syntax error.
* Verified using `nginx -t`.
* Fixed configuration file under `/etc/nginx/nginx.conf`.
* Restarted service and enabled auto-start using `systemctl enable nginx`.

**R – Result:**
Service restored within 10 minutes, and implemented pre-deployment config validation.

---

## ⭐ Scenario 4: Permission Denied Error in Production

**Interview Question:**
Explain a situation where application failed due to permission issues.

### ✅ STAR Answer

**S – Situation:**
Deployment pipeline succeeded, but application returned “Permission Denied” while writing logs.

**T – Task:**
Identify and fix permission issue without breaking security.

**A – Action:**

* Checked file permissions using `ls -l`.
* Found incorrect ownership.
* Updated ownership using `chown appuser:appgroup /var/app/logs`.
* Adjusted permissions using `chmod 750`.
* Validated with test logs.

**R – Result:**
Application resumed normal logging, and permission best practices were documented.

---

## ⭐ Scenario 5: SSH Access Issue to Production Server

**Interview Question:**
Tell me about a time when you couldn’t SSH into a production server.

### ✅ STAR Answer

**S – Situation:**
One of our production servers became inaccessible via SSH.

**T – Task:**
Restore secure access quickly.

**A – Action:**

* Verified security group rules.
* Checked server status via cloud console.
* Found SSH service not running.
* Used cloud serial console to access server.
* Restarted SSH using `systemctl restart sshd`.
* Checked `/var/log/secure` for root cause.

**R – Result:**
Access restored within 15 minutes and added monitoring alert for SSH service.

---

# 🎯 Pro Interview Tip

While answering:

* Mention exact commands
* Explain monitoring tools used
* Highlight prevention steps
* Quantify results (time saved, downtime reduced)

---

# ☁️ Linux + AWS Production Failures (STAR Method)

---

## ⭐ Scenario 1: EBS Volume Performance Degradation (IOPS Bottleneck)

**Interview Question:**
Tell me about a time when your production application slowed down due to EBS issues.

### ✅ STAR Answer

**S – Situation:**
Our production application hosted on an EC2 instance started responding very slowly. Users experienced timeouts during peak traffic hours.

**T – Task:**
Identify whether the issue was application-related or infrastructure-related and restore performance quickly.

**A – Action:**

* Checked system load using `top`, `uptime`, and observed high I/O wait.
* Used `iostat -x 1` to confirm disk latency.
* Verified disk utilization using `df -h` and `lsblk`.
* Checked CloudWatch metrics and observed EBS Burst Balance was exhausted.
* Identified that the volume type was gp2 with limited baseline IOPS.
* Took snapshot of the EBS volume.
* Upgraded volume type to gp3 with higher provisioned IOPS.
* Monitored performance after modification.

**R – Result:**
Disk latency reduced significantly, application response time improved by 60%, and we implemented proactive CloudWatch alerts for Burst Balance.

---

## ⭐ Scenario 2: EC2 Instance Crash Due to Memory Exhaustion

**Interview Question:**
Describe a situation where a Linux EC2 instance crashed in production.

### ✅ STAR Answer

**S – Situation:**
A production EC2 instance became unreachable and stopped serving traffic during high workload.

**T – Task:**
Restore service immediately and determine the root cause.

**A – Action:**

* Checked EC2 status checks in AWS Console.
* Used CloudWatch to analyze CPU and memory metrics.
* Identified memory exhaustion leading to OOM killer termination.
* Rebooted instance to restore service.
* Analyzed `/var/log/messages` and `dmesg` for OOM logs.
* Increased instance type from t3.medium to t3.large.
* Configured swap space temporarily.
* Implemented memory monitoring alerts.

**R – Result:**
Service restored within 20 minutes, prevented recurrence, and improved system stability during peak traffic.

---

## ⭐ Scenario 3: NFS Mount Failure (EFS Not Mounting)

**Interview Question:**
Tell me about a time when an NFS or EFS mount failed in production.

### ✅ STAR Answer

**S – Situation:**
After deploying a new EC2 instance in an Auto Scaling Group, the application failed to start because the shared storage (EFS) was not mounting.

**T – Task:**
Ensure the NFS mount is successful so application can access shared files.

**A – Action:**

* Checked mount status using `mount | grep nfs`.
* Attempted manual mount using `mount -t nfs4`.
* Verified security group rules allowing NFS (port 2049).
* Checked `/etc/fstab` configuration.
* Ensured EFS mount targets were available in correct subnets.
* Installed `amazon-efs-utils` and used recommended mount helper.
* Restarted instance and validated auto-mount.

**R – Result:**
EFS mounted successfully, application nodes scaled correctly, and we updated AMI to include proper mount configuration.

---

## ⭐ Scenario 4: Root Filesystem Corruption After Sudden Shutdown

**Interview Question:**
Explain a situation where a Linux root filesystem was corrupted.

### ✅ STAR Answer

**S – Situation:**
A production EC2 instance was forcefully stopped due to an AWS infrastructure issue. After restart, it failed to boot.

**T – Task:**
Recover the instance with minimal downtime and prevent data loss.

**A – Action:**

* Checked system logs via EC2 console.
* Detached the root EBS volume.
* Attached it to a rescue instance.
* Ran `fsck` to repair filesystem errors.
* Verified integrity and reattached volume.
* Restarted original instance.
* Enabled automated snapshots.

**R – Result:**
Instance recovered successfully, no data loss occurred, and snapshot strategy was strengthened.

---

# 🎯 Interview Booster Tip

For AWS + Linux scenarios always mention:

* Linux command used
* AWS service involved
* Monitoring method (CloudWatch, logs)
* Preventive automation (alerts, scaling, snapshots)
* Quantifiable impact (downtime reduced, performance improved)

---
