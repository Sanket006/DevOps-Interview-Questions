# Real‑world EC2 & Linux Troubleshooting Q&A

This README contains **real‑world EC2 & Linux troubleshooting commands with interview‑ready explanations**, exactly as used by DevOps / Cloud engineers in production.

---

## 🖥️ CPU / Memory / Disk Troubleshooting

### 1️⃣ Check CPU & Memory usage on EC2

```bash
top
htop        # if installed (better UI)
free -h
vmstat 1
```

**What to say in interview**:

* I first check real-time CPU and memory usage using top or htop, and verify memory pressure using free -h and vmstat.

---

### 2️⃣ Find which process is consuming most CPU

```bash
top
# press P (sort by CPU)
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head
```

---

### 3️⃣ Check available disk space & inode usage

```bash
df -h       # disk space
df -i       # inode usage
```

**Common trap**:

* Disk looks free but app fails → inode exhaustion.

---

### 4️⃣ Monitor EBS volume performance from EC2

```bash
iostat -xz 1
lsblk
```

**CloudWatch metrics to mention**:

* VolumeReadOps
* VolumeWriteOps
* BurstBalance
* VolumeQueueLength

---

### 5️⃣ Detect a memory leak in EC2

```bash
top
ps aux --sort=-%mem | head
watch -n 5 free -h
```

**Interview answer**:

* If memory keeps increasing without release and swap usage grows, it indicates a memory leak.

---

### 6️⃣ Identify process causing high load average

```bash
uptime
top
ps -eo pid,cmd,%cpu,%mem --sort=-%cpu | head
```

**Key concept**:

* High load ≠ high CPU → could be I/O wait.

---

### 7️⃣ Find EC2 uptime

```bash
uptime
who -b
```

---

### 8️⃣ Check which user started a process

```bash
ps aux | grep process_name
ps -o user,pid,cmd -p <PID>
```

---

### 9️⃣ Check swap memory status

```bash
swapon --show
free -h
```

---

### 🔟 Top 10 memory-consuming processes

```bash
ps aux --sort=-%mem | head -10
```

---

## 🌐 Network & Application Troubleshooting

### 1️⃣1️⃣ Troubleshoot web app on port 8080

```bash
ss -tulnp | grep 8080
curl -v localhost:8080
systemctl status app_name
```

**Interview flow**:

* Port → Process → Service → Logs

---

### 1️⃣2️⃣ Find process listening on a port

```bash
ss -tulnp | grep 8080
lsof -i :8080
```

---

### 1️⃣3️⃣ Verify EC2 connectivity to another instance

```bash
ping <private-ip>
nc -zv <ip> <port>
telnet <ip> <port>
```

---

### 1️⃣4️⃣ Test external API / S3 connectivity

```bash
curl https://google.com
curl https://s3.amazonaws.com
aws s3 ls
```

---

### 1️⃣5️⃣ Fix “connection refused” errors

**Checklist**:

* Service running?
* Port listening?
* Security Group inbound rule?
* NACL allows traffic?
* Firewall (iptables/firewalld)?

```bash
systemctl status service
ss -tulnp
iptables -L
```

---

### 1️⃣6️⃣ Find EC2 public & private IP

```bash
ip a
curl http://169.254.169.254/latest/meta-data/public-ipv4
curl http://169.254.169.254/latest/meta-data/local-ipv4
```

---

### 1️⃣7️⃣ Verify DNS resolution inside EC2

```bash
nslookup google.com
dig google.com
cat /etc/resolv.conf
```

---

### 1️⃣8️⃣ Check inbound & outbound connections

```bash
ss -antp
netstat -antp
```

---

### 1️⃣9️⃣ Capture live network packets

```bash
tcpdump -i eth0 port 80
tcpdump -nn -i eth0
```

---

## ⚠️ Production caution: Short duration only.

### 2️⃣0️⃣ Verify SG / NACL issue from EC2

```bash
curl <target-ip>:<port>
nc -zv <ip> <port>
```

* If traffic fails but service is running → network layer issue.

---

## 📜 Logs & Service Debugging

### 2️⃣1️⃣ Find logs of failed services

```bash
journalctl -u service_name
systemctl status service_name
```

---

### 2️⃣2️⃣ Analyze EC2 boot logs

```bash
journalctl -b
dmesg
```

---

### 2️⃣3️⃣ Check application crash logs

```bash
tail -n 100 /var/log/app.log
journalctl -xe
```

---

### 2️⃣4️⃣ Monitor logs continuously

```bash
tail -f /var/log/syslog
```

---

### 2️⃣5️⃣ Search error keywords in all logs

```bash
grep -R "ERROR" /var/log/
grep -R "failed" /var/log/
```

---

### 2️⃣6️⃣ Kernel / hardware errors

```bash
dmesg | tail
journalctl -k
```

---

### 2️⃣7️⃣ Troubleshoot systemctl start failure

```bash
systemctl status service
journalctl -xe
```

---

### 2️⃣8️⃣ Find recently failed processes

```bash
journalctl -p err -b
```

---

### 2️⃣9️⃣ Track service restart history

```bash
journalctl -u service_name
```

---

### 3️⃣0️⃣ Debug permission errors

```bash
ls -l file
id user
getenforce
ausearch -m avc -ts recent   # SELinux
```

---

✅ **This README is suitable for:**

* DevOps Engineer interviews (Fresher–Mid)
* EC2 production troubleshooting
* Linux system administration practice
* Personal DevOps notes repository
