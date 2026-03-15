# Here are the Linux Production Troubleshooting Q&A
## 1️⃣ Server Is Slow (High Load)

**Model Answer**:

- First, I check the overall system load using the uptime command to understand whether the issue is CPU-related or resource-related.
- If the load is high, I check CPU usage using top or htop and identify the process consuming the most CPU.
- Then I check memory usage using free -h and verify if swap is being heavily used.
- If CPU and memory are normal, I check disk I/O using iostat.
- After identifying the root cause, I either restart the affected service or optimize the application.
- Finally, I enable monitoring and alerts to prevent future performance issues.

**Commands**:
```bash
uptime
top
free -h
iostat
```

## 2️⃣ High CPU Usage

**Model Answer**:

- When CPU usage is high, I first identify the process using top or ps aux --sort=-%cpu.
- If the process is non-critical, I stop it using kill.
- If it is a critical service, I check its logs to understand why it is consuming high CPU and then restart it safely.
- After fixing the issue, I review application behavior and add CPU alerts.

**Commands**:
```bash
top
ps aux --sort=-%cpu
kill -9 PID
```

## 3️⃣ Disk Full Issue

**Model Answer**:

- I start by checking disk usage using df -h to confirm which partition is full.
- Then I use du -sh to identify directories consuming large space, usually under /var/log.
- After identifying large files, I clean up or rotate logs safely.
- In production, I never delete files blindly.
- As a preventive step, I configure log rotation and disk usage monitoring.

**Commands**:
```bash
df -h
du -sh /var/*
```

## 4️⃣ Unable to SSH into Server

**Model Answer**:

- First, I verify network connectivity using ping.
- If the server is reachable, I check whether the SSH service is running using systemctl status sshd.
- Then I check if port 22 is listening using ss -tuln.
- I also verify firewall rules and SSH configuration files.
- Once resolved, I ensure SSH service is enabled on boot and monitored.

**Commands**:
```bash
ping <IP>
systemctl status sshd
ss -tuln | grep 22
```

## 5️⃣ Application Not Accessible on Port

**Model Answer**:

- I begin by checking whether the application service is running using systemctl status.
- Then I verify if the application is listening on the correct port using ss or netstat.
- I test the application locally using curl localhost:port.
- If local access works, I check firewall or security group rules.
- Finally, I review application logs for errors.

**Commands**:
```bash
systemctl status app
ss -tuln
curl localhost:8080
```

## 6️⃣ High Memory Usage / OOM Issue

**Model Answer**:

- I check memory usage using free -h and identify memory-consuming processes using top.
- I also check swap usage to see if the system is under memory pressure.
- If required, I restart the affected service or increase memory.
- To prevent this issue, I enable memory monitoring and tune application memory limits.

**Commands**:
```bash
free -h
top
swapon -s
```

## 7️⃣ Service Not Starting After Reboot

**Model Answer**:

- I check the service status using systemctl status.
- Then I analyze detailed logs using journalctl -xe.
- Most of the time, the issue is due to configuration errors or missing dependencies.
- After fixing the configuration, I restart the service and enable it to start on boot.

**Commands**:
```bash
systemctl status nginx
journalctl -u nginx
```

## 8️⃣ Cron Job Not Running

**Model Answer**:

- I verify the cron job using crontab -l.
- Then I check cron logs in /var/log/syslog.
- I ensure the script has executable permission and uses absolute paths.
- I also redirect cron output to a log file for debugging.
- Finally, I test the script manually.

**Commands**:
```bash
crontab -l
chmod +x script.sh
```

## 9️⃣ Filesystem Became Read-Only

**Model Answer**:

- When a filesystem becomes read-only, it usually indicates disk or file system errors.
- I check kernel messages using dmesg.
- If errors are found, I schedule a file system check using fsck.
- This protects data from corruption.
- After fixing the issue, I monitor disk health.

**Commands**:
```bash
dmesg | tail
fsck /dev/sdX
```
## 🔟 Server Fails to Boot (Emergency Mode)

**Model Answer**:

- I carefully read the error message displayed during boot.
- Most commonly, the issue is caused by incorrect entries in /etc/fstab.
- I fix the incorrect mount entry and reboot the system.
- To prevent this, I always test mounts before rebooting production servers.

**Commands**:
```bash
vi /etc/fstab
mount -a
```
