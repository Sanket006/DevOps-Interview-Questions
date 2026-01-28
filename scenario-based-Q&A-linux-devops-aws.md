# Here the Secnario based Interview Question on Linux, AWS, DevOps
## 🔹 Scenario 1: EC2 Server Slow in Production

**Model Answer**:

- First, I check the system load using uptime to understand whether the issue is related to CPU, memory, or I/O.
- Then I analyze CPU usage using top and memory usage using free -h.
- If disk I/O is suspected, I use iostat.
- At the AWS level, I verify the EC2 instance type and check CloudWatch metrics to see if the instance is under-provisioned.
- To fix the issue, I optimize or restart the application or scale the instance.
- As a preventive measure, I enable CloudWatch alarms and Auto Scaling.

**Commands**:
```bash
uptime
top
free -h
iostat
```

## 🔹 Scenario 2: Disk Full on EC2 Root Volume

**Model Answer**:

- I first confirm the disk usage using df -h.
- Then I identify large directories using du -sh and locate large files using find.
- Most of the time, logs under /var/log cause this issue.
- At the AWS level, I extend the EBS volume and resize the filesystem without downtime.
- To prevent this, I configure log rotation and disk usage alerts.

**Commands**:
```bash
df -h
du -sh /var/*
find / -size +1G
```

## 🔹 Scenario 3: Application Down After EC2 Reboot

**Model Answer**:

- I check the service status using systemctl status.
- Then I review logs using journalctl.
- Often, the issue is that the service is not enabled on boot or dependencies are not ready.
- I enable the service using systemctl enable.
- For prevention, I use Auto Scaling with health checks so failed instances are replaced automatically.

**Commands**:
```bash
systemctl status app
journalctl -u app
systemctl enable app
```

## 🔹 Scenario 4: Unable to SSH into EC2 Instance

**Model Answer**:

- I start by checking network connectivity using ping.
- Then I verify that port 22 is open in the security group.
- On the OS side, I check the SSH service status using systemctl status sshd.
- If access is still not possible, I use AWS EC2 Instance Connect or attach the root volume to another instance to fix SSH configuration issues.
- To prevent this, I use bastion hosts and proper access monitoring.

**Commands**:
```bash
ping <IP>
systemctl status sshd
ss -tuln | grep 22
```
## 🔹 Scenario 5: High Memory Usage / OOM Kill

**Model Answer**:

- I check memory usage using free -h and identify memory-consuming processes using top.
- I verify OOM kill events using system logs.
- If required, I restart the application or increase swap temporarily.
- At the AWS level, I scale up the instance or move the application to Auto Scaling.
- As prevention, I enable memory alerts and tune application memory limits.

**Commands**:
```bash
free -h
top
dmesg | grep oom
```

## 🔹 Scenario 6: Application Accessible Locally but Not Publicly

**Model Answer**:

- I first test the application locally using curl localhost:port.
- Then I verify that the application is listening on the correct interface using ss.
- At the AWS level, I check security groups, NACLs, and load balancer target health.
- Once fixed, I expose the app using an Application Load Balancer.
- This improves security and scalability.

**Commands**:
```bash
curl localhost:8080
ss -tuln
```
## 🔹 Scenario 7: Logs Filling Disk in Production

**Model Answer**:

- I identify large log files using du.
- I safely clean or archive old logs.
- I configure logrotate for automatic rotation.
- At the AWS level, I forward logs to CloudWatch Logs for centralized monitoring.
- This prevents disk issues and improves observability.

**Commands**:
```bash
du -sh /var/log/*
logrotate -f /etc/logrotate.conf
```

## 🔹 Scenario 8: Deployment Caused Application Downtime

**Model Answer**:

- I immediately rolled back to the previous stable version.
- I check the application and system logs to identify the issue.
- To prevent downtime, I implement blue-green or rolling deployments using CI/CD pipelines and an AWS Load Balancer.
- This ensures zero-downtime releases.

**DevOps Tools**:
  - Jenkins
  - GitHub Actions
  - AWS ALB

## 🔹 Scenario 9: Cron Job Backup Not Running on EC2

**Model Answer**:

- I verify the cron job using crontab -l.
- I check cron logs and ensure scripts have executable permissions and absolute paths.
- I manually test the script.
- For reliability, I store backups in S3 and add CloudWatch alerts for failures.

Commands:
```bash
crontab -l
chmod +x backup.sh
```

## 🔹 Scenario 🔟 Single EC2 Failure Causes App Down

**Model Answer**:

- Running an application on a single EC2 instance is a single point of failure.
- I design the system with multiple EC2 instances behind an Application Load Balancer.
- Auto Scaling ensures unhealthy instances are replaced automatically.
- This provides high availability and fault tolerance.

## 🔹 Scenario 11: Permission Denied After Deployment

**Model Answer**:

- I check file permissions using ls -l.
- I fix ownership using chown and permissions using chmod.
- To prevent this, I standardize permissions in deployment scripts and avoid running apps as root.

**Commands**:
```bash
ls -l
chown user:group file
chmod 755 file
```

## 🔹 Scenario 12: Manual Fixes in Production

**Model Answer**:

- Manual fixes are risky and non-repeatable.
- I automate fixes using configuration management or Infrastructure as Code.
- In AWS, I use Terraform and Auto Scaling to replace unhealthy servers automatically.
- This improves reliability and auditability.
