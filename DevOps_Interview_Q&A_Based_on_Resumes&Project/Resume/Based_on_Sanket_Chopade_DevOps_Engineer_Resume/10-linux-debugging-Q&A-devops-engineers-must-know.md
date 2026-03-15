# Linux Debugging Scenario

## Question 1

A Linux server becomes very slow. What commands will you run first?

## Identify

Users report that applications are responding slowly or SSH login takes a long time. Monitoring tools may also show high resource usage.

## Diagnose

Check system load and running processes:

```bash
top
```

Check load average:

```bash
uptime
```

Check CPU usage per process:

```bash
ps aux --sort=-%cpu | head
```

Check memory usage:

```bash
free -h
```

Check disk I/O usage:

```bash
iostat -x 1 5
```

Check disk space:

```bash
df -h
```

Check running services:

```bash
systemctl list-units --type=service --state=running
```

## Fix

If a process is consuming excessive CPU or memory, stop or restart it:

```bash
kill -9 <PID>
```

Restart the affected service if needed:

```bash
systemctl restart <service-name>
```

If disk space is full, remove unnecessary files or logs.

## Prevent

Implement monitoring and alerting using tools like Prometheus or CloudWatch. Configure log rotation and resource monitoring to detect abnormal usage early.

---

## Question 2

How do you identify the process consuming the most CPU?

## Identify

System becomes slow or monitoring alerts show high CPU usage. Users may report application latency or SSH lag.

## Diagnose

Check real-time CPU usage:

```bash
top
```

Sort processes by CPU usage:

```bash
ps aux --sort=-%cpu | head
```

Interactive process viewer:

```bash
htop
```

Check system load:

```bash
uptime
```

## Fix

If a process is consuming excessive CPU, identify its PID and terminate or restart it.

Kill the process:

```bash
kill -9 <PID>
```

Restart related service:

```bash
systemctl restart <service-name>
```

## Prevent

Implement monitoring with Prometheus, Grafana, or CloudWatch. Configure alerts for high CPU usage and analyze application performance to prevent CPU spikes.

---

## Question 3

How do you check memory usage in Linux?

## Identify

Users may report application slowness, crashes, or "Out Of Memory" errors. Monitoring tools may show high RAM usage.

## Diagnose

Check overall memory usage:

```bash
free -h
```

Check real-time memory usage by processes:

```bash
top
```

Sort processes by memory consumption:

```bash
ps aux --sort=-%mem | head
```

Check detailed memory statistics:

```bash
vmstat
```

Check memory information from the kernel:

```bash
cat /proc/meminfo
```

## Fix

If a process is consuming excessive memory, identify its PID and terminate or restart it.

Kill the process:

```bash
kill -9 <PID>
```

Restart related service if necessary:

```bash
systemctl restart <service-name>
```

Clear cached memory if needed (use carefully in production):

```bash
sudo sync
sudo sysctl -w vm.drop_caches=3
```

## Prevent

Implement monitoring using tools like Prometheus, Grafana, or CloudWatch to track memory usage. Set alerts for high memory consumption and optimize applications to prevent memory leaks.

---

## Question 4

A service is not starting. How will you debug it?

## Identify

Users report that an application or service is unavailable. Attempting to start the service fails or it stops immediately after starting.

## Diagnose

Check the service status to see error messages:

```bash
systemctl status <service-name>
```

Check detailed logs using journalctl:

```bash
journalctl -u <service-name> --no-pager
```

Check if the service configuration file has issues:

```bash
cat /etc/<service-name>/<config-file>
```

Check if the required port is already in use:

```bash
sudo ss -tulnp | grep <port>
```

Verify permissions for required files or directories:

```bash
ls -l /path/to/service/files
```

## Fix

Fix configuration errors or free the required port.

Restart the service:

```bash
sudo systemctl restart <service-name>
```

If the service failed previously, reload systemd and start again:

```bash
sudo systemctl daemon-reload
sudo systemctl start <service-name>
```

## Prevent

Implement monitoring and alerting for service failures. Validate configuration files before deployment and use automated configuration management tools like Ansible or Terraform to maintain consistent setups.

---

## Question 5

How do you find large files consuming disk space?

## Identify

Disk usage alerts trigger or applications start failing due to insufficient disk space. System monitoring may show disk utilization above safe thresholds (e.g., 90%).

## Diagnose

Check overall disk usage:

```bash
df -h
```

Identify large directories:

```bash
sudo du -h --max-depth=1 / | sort -hr
```

Drill down into a directory consuming space:

```bash
sudo du -h --max-depth=1 /var | sort -hr
```

Find large files (greater than 500 MB):

```bash
sudo find / -type f -size +500M 2>/dev/null
```

List the largest files in the system:

```bash
sudo du -ah / | sort -rh | head -n 20
```

## Fix

Remove unnecessary large files or compress logs.

Remove file:

```bash
sudo rm -f /path/to/large-file
```

Clear logs safely:

```bash
sudo truncate -s 0 /var/log/syslog
```

## Prevent

Configure log rotation:

```bash
cat /etc/logrotate.conf
```

Implement disk monitoring using tools like Prometheus, Grafana, or CloudWatch and set alerts when disk usage exceeds 80%.

---

## Question 6

How do you check open ports on a server?

## Identify

Applications may not be reachable, or a service might fail to start due to port conflicts. Users may report connection failures to the server.

## Diagnose

Check all listening ports:

```bash
ss -tuln
```

Check listening ports with associated processes:

```bash
sudo ss -tulnp
```

Use netstat to check open ports (older systems):

```bash
netstat -tulnp
```

Check if a specific port is in use:

```bash
sudo lsof -i :<port>
```

## Fix

If a port conflict exists, identify the process using the port and stop it.

Kill the process:

```bash
kill -9 <PID>
```

Restart the required service:

```bash
sudo systemctl restart <service-name>
```

## Prevent

Maintain proper port management and documentation. Monitor services and configure firewalls correctly to allow only required ports. Use monitoring tools to detect port conflicts early.

---

## Question 7

A server cannot connect to another server. How will you debug the network?

## Identify

Application fails to communicate with another server. Users may see connection timeout errors or API failures between services.

## Diagnose

Check basic network connectivity using ping:

```bash
ping <destination-ip>
```

Check DNS resolution:

```bash
nslookup <hostname>
```

Trace the network path to the destination:

```bash
traceroute <destination-ip>
```

Check if the required port is reachable:

```bash
nc -zv <destination-ip> <port>
```

Check routing table:

```bash
ip route
```

Check firewall rules:

```bash
sudo iptables -L -n
```

Check listening services on the destination server:

```bash
ss -tulnp
```

## Fix

If DNS is incorrect, update DNS configuration.

If firewall blocks traffic, allow the required port:

```bash
sudo iptables -A INPUT -p tcp --dport <port> -j ACCEPT
```

Restart networking service if needed:

```bash
sudo systemctl restart network
```

## Prevent

Implement network monitoring and alerts. Maintain proper firewall rules and security groups documentation. Use infrastructure automation to ensure consistent network configurations.

---

## Question 8

How do you check system logs in Linux?

## Identify

Applications or services fail, the system behaves unexpectedly, or alerts indicate errors. Logs are required to understand what happened in the system.

## Diagnose

Check system logs using journalctl (systemd-based systems):

```bash
journalctl
```

Check logs for a specific service:

```bash
journalctl -u <service-name>
```

Check the latest logs in real time:

```bash
journalctl -f
```

Check logs from the current boot:

```bash
journalctl -b
```

Check traditional log files in /var/log:

```bash
ls -l /var/log
```

View system messages log:

```bash
tail -f /var/log/syslog
```

View authentication logs:

```bash
tail -f /var/log/auth.log
```

Search for errors in logs:

```bash
grep -i "error" /var/log/syslog
```

## Fix

Identify errors from logs and resolve configuration issues, restart failed services, or correct permissions.

Restart service after fixing issues:

```bash
sudo systemctl restart <service-name>
```

## Prevent

Enable centralized logging and monitoring using tools such as the ELK stack (Elasticsearch, Logstash, Kibana) or Grafana Loki. Configure log rotation to prevent logs from consuming excessive disk space.

---

## Question 9

A cron job is not running. What will you check?

## Identify

A scheduled task expected to run automatically does not execute. Reports, backups, or scripts scheduled through cron are missing.

## Diagnose

Check the current user's cron jobs:

```bash
crontab -l
```

Check system-wide cron jobs:

```bash
cat /etc/crontab
```

Check cron directories:

```bash
ls -l /etc/cron.d
ls -l /etc/cron.daily
ls -l /etc/cron.hourly
```

Verify the cron service status:

```bash
sudo systemctl status cron
```

Check cron logs:

```bash
grep CRON /var/log/syslog
```

Verify script permissions:

```bash
ls -l /path/to/script.sh
```

## Fix

Start or restart the cron service if it is stopped:

```bash
sudo systemctl restart cron
```

Ensure the script has execute permissions:

```bash
chmod +x /path/to/script.sh
```

Correct the cron schedule syntax if necessary.

## Prevent

Validate cron syntax before deployment and monitor scheduled jobs. Use logging within scripts to track execution results and implement monitoring alerts for critical scheduled tasks.

---

## Question 10

How do you check running processes in Linux?

## Identify

The system may be slow, applications may be consuming excessive resources, or a service might not behave as expected. Checking running processes helps identify what is currently executing on the system.

## Diagnose

View all running processes in real time:

```bash
top
```

Enhanced interactive process viewer (if installed):

```bash
htop
```

List all running processes:

```bash
ps aux
```

Find processes sorted by CPU usage:

```bash
ps aux --sort=-%cpu | head
```

Find processes sorted by memory usage:

```bash
ps aux --sort=-%mem | head
```

Search for a specific process:

```bash
ps aux | grep <process-name>
```

## Fix

If a process is misbehaving or consuming excessive resources, identify its PID and stop or restart it.

Terminate the process gracefully:

```bash
kill <PID>
```

Force kill the process if needed:

```bash
kill -9 <PID>
```

Restart the related service if required:

```bash
sudo systemctl restart <service-name>
```

## Prevent

Implement system monitoring using tools like Prometheus, Grafana, or CloudWatch. Set alerts for abnormal CPU or memory usage and regularly review running processes to detect issues early.

---