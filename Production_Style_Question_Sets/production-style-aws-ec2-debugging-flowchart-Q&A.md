# 🚀 AWS EC2 – Real Production Debugging Flowcharts

This document provides structured, production-level debugging flowcharts for common EC2 issues faced by DevOps Engineers.

---

# 🔎 1️⃣ EC2 Instance Unreachable (SSH Timeout)

```
Start
  │
  ├─► Is instance state = Running?
  │       ├─ No → Start instance
  │       └─ Yes
  │
  ├─► Security Group allows port 22 from your IP?
  │       ├─ No → Add inbound rule
  │       └─ Yes
  │
  ├─► NACL allows inbound/outbound 22?
  │       ├─ No → Fix NACL rules
  │       └─ Yes
  │
  ├─► Route table attached to IGW?
  │       ├─ No → Fix route
  │       └─ Yes
  │
  ├─► Correct Key Pair?
  │       ├─ No → Recover via volume attach method
  │       └─ Yes
  │
  ├─► Disk full? (df -h)
  │       ├─ Yes → Clean logs / expand EBS
  │       └─ No
  │
  ├─► SSH service running?
  │       ├─ No → Restart sshd
  │       └─ Yes
  │
Resolved
```

---

# 🔎 2️⃣ High CPU Utilization (100%)

```
Start
  │
  ├─► Check CloudWatch CPU metric
  │
  ├─► Use top/htop → Identify process
  │
  ├─► Application issue?
  │       ├─ Yes → Restart service / Fix bug
  │       └─ No
  │
  ├─► Traffic spike?
  │       ├─ Yes → Enable/Adjust Auto Scaling
  │       └─ No
  │
  ├─► Instance undersized?
  │       ├─ Yes → Upgrade instance type
  │       └─ No
  │
Resolved
```

---

# 🔎 3️⃣ EC2 Status Check Failed (System vs Instance)

```
Start
  │
  ├─► System Status Check Failed?
  │       ├─ Yes → AWS host issue → Stop/Start instance
  │       └─ No
  │
  ├─► Instance Status Check Failed?
  │       ├─ Yes → OS/Kernel issue
  │               ├─ Check system logs
  │               ├─ Check disk corruption
  │               └─ Repair via volume detach
  │
Resolved
```

---

# 🔎 4️⃣ Application Down but Instance Running

```
Start
  │
  ├─► Check Load Balancer health checks
  │
  ├─► Check application service status
  │       ├─ Stopped → Restart service
  │
  ├─► Check logs (/var/log)
  │
  ├─► Check security group port open?
  │
  ├─► Check database connectivity
  │
Resolved
```

---

# 🔎 5️⃣ EBS Volume Not Mounting

```
Start
  │
  ├─► Volume attached to correct instance?
  │       ├─ No → Attach volume
  │       └─ Yes
  │
  ├─► Check lsblk
  │
  ├─► Filesystem exists?
  │       ├─ No → mkfs
  │       └─ Yes
  │
  ├─► Mount manually
  │
  ├─► Update /etc/fstab
  │
Resolved
```

---

# 🔥 Production Interview Tip

When explaining debugging:

1. Always start from Network → OS → Application → AWS layer
2. Mention logs and metrics
3. Quantify downtime
4. Explain prevention (monitoring, scaling, alerts)

---

End of Document
