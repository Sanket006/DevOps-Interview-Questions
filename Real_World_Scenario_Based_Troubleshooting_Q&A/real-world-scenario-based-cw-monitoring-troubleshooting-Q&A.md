## CloudWatch Monitoring Troubleshooting & Interview Guide

This guide focuses on **real-world troubleshooting using Amazon CloudWatch**, exactly how a DevOps / SRE engineer would debug production issues.

- **Core mindset**:  
  **ALARM → METRIC → LOG → ROOT CAUSE → FIX**  
  Avoid blind restarts; always use data to drive your actions.

---

## 1. CloudWatch Troubleshooting Mindset

- **Start from the alarm**
  - Identify which metric breached (CPU, latency, 5XX, disk, memory, connections, etc.).
- **Correlate metrics**
  - Check related resources and Golden Signals (latency, traffic, errors, saturation).
- **Deep dive with logs**
  - Use **CloudWatch Logs** and **Log Insights** to find errors, timeouts, crashes.
- **Confirm root cause**
  - Distinguish between infra issues vs application issues vs configuration issues.
- **Apply a targeted fix**
  - Scale, rotate, clean up, reconfigure, or roll back instead of just “restart and pray”.

---

## 2. Common CloudWatch Troubleshooting Scenarios

### 2.1 EC2 Instance Is Slow / Application Lagging

- **Step 1 – Check EC2 metrics**
  - `CloudWatch → Metrics → EC2`

| Metric              | What It Tells You     |
|---------------------|-----------------------|
| `CPUUtilization`    | CPU bottleneck        |
| `NetworkIn/NetworkOut` | Traffic spike or network issue |
| `StatusCheckFailed` | Instance / system issue |

- If **CPU is normal but the app is slow**, it is **not** a CPU issue.

- **Step 2 – Check memory & disk (via CloudWatch Agent)**

  Default EC2 metrics do **not** show memory and disk; use **CloudWatch Agent**:

  - `mem_used_percent`
  - `disk_used_percent`

  Common real issue:

  - **Disk full → application hangs or crashes**

- **Step 3 – Check logs (CloudWatch Logs)**
  - Go to `CloudWatch Logs → Application log group`.
  - Use **Log Insights**, for example:

    ```sql
    fields @timestamp, @message
    | filter @message like /timeout|error|fail/
    | sort @timestamp desc
    ```

- **Typical root causes**
  - Memory leak
  - Disk full
  - Database connection timeout

---

### 2.2 EC2 Instance Unreachable (SSH Not Working)

- **Step 1 – Check status checks**

| Status Check          | Meaning                 |
|-----------------------|-------------------------|
| System Status Check   | AWS hardware / host issue |
| Instance Status Check | OS / kernel / instance issue |

- **Step 2 – Check network-related metrics**
  - Is `NetworkIn` and `NetworkOut` close to **0**?
  - Possible causes:
    - Security Group rules incorrect
    - Route table issues
    - NACL blocking traffic

- **Step 3 – Check system logs**
  - `CloudWatch Logs → /var/log/syslog` (or relevant system log group).
  - Look for:
    - Kernel panic
    - Disk errors
    - OOM (Out Of Memory) events

- **Possible fixes**
  - Stop / start the instance
  - Detach root volume, mount it to another instance, repair, and reattach
  - Restore from a snapshot

---

### 2.3 High CPU Alarm Triggered Suddenly

- **Step 1 – Identify the pattern**
  - Is CPU usage:
    - Gradual increase?
    - Sudden spike?
    - Constantly high?

- **Step 2 – Correlate metrics**
  - Check:
    - `NetworkIn` / `NetworkOut`
    - `RequestCount` (ALB / API Gateway)
    - Load balancer traffic
  - If traffic increased → high CPU may be expected (scale out).
  - If traffic is normal → possible **bug** or **attack**.

- **Step 3 – Analyze logs**
  - Use Log Insights:

    ```sql
    filter @message like /Exception|Error|Loop/
    ```

- **Typical fixes**
  - Enable or tune **Auto Scaling**
  - Fix **infinite loops** / inefficient code
  - Enable **rate limiting** / throttling if under attack

---

### 2.4 ALB 5XX Errors Increasing

- **Step 1 – Check ALB metrics**

| Metric                  | Meaning            |
|-------------------------|-------------------|
| `HTTPCode_ELB_5XX`      | Load balancer issue |
| `HTTPCode_Target_5XX`   | Application / target issue |

- **Step 2 – Target health**
  - Are targets unhealthy?
  - Are health checks failing?

- **Step 3 – Backend logs**
  - Look for:
    - DB connection failure
    - Backend service crash

- **Common fixes**
  - Restart or fix backend services
  - Increase backend / DB timeout
  - Scale backend instances / containers

---

### 2.5 RDS Database Slow / Connection Errors

- **Step 1 – Check RDS metrics**
  - `CPUUtilization`
  - `DatabaseConnections`
  - `ReadLatency` / `WriteLatency`

- **Step 2 – Correlate alarms**
  - Connection spike?
  - CPU normal but latency high?

- **Step 3 – App logs**
  - Log queries such as:

    ```sql
    filter @message like /DB|timeout|connection/
    ```

- **Typical fixes**
  - Increase DB instance size
  - Enable / tune **connection pooling**
  - Add **read replicas**

---

### 2.6 Memory Leak Detection

- **Pattern**
  - Memory usage increases gradually over time
  - CPU remains stable
  - Application crashes after some hours

- **Detection**
  - In CloudWatch:
    - `mem_used_percent` steadily increasing

- **Fix**
  - Restart the service (temporary)
  - Fix the code causing the leak
  - Increase instance size only after fixing the leak

---

### 2.7 Disk Full Issue

- **Detection**
  - Alarm like:

    ```text
    disk_used_percent > 85%
    ```

- **Logs**
  - Search:

    ```sql
    filter @message like /No space left/
    ```

- **Fix**
  - Clear or rotate logs
  - Increase EBS volume size
  - Implement log rotation and retention policies

---

### 2.8 Alarm Triggered but Everything Looks Fine (False Alarm)

- **Common causes**
  - Threshold too aggressive
  - Too few evaluation periods
  - Short-lived metric spikes

- **Fix**
  - Increase evaluation periods
  - Use **Composite Alarms**
  - Enable **Anomaly Detection** where appropriate

---

### 2.9 CloudWatch Logs Missing

- **Checklist**
  - Is the **CloudWatch Agent** running?
  - Is the correct **IAM role / policy** attached?
  - Is the **log file path** correct in the agent config?
  - Is there **network access** to CloudWatch endpoints?

- **Fix**
  - Restart the agent:

    ```bash
    sudo systemctl restart amazon-cloudwatch-agent
    ```

---

### 2.10 Production Troubleshooting Checklist

- ✅ Check metrics first (alarms, trends, correlations)
- ✅ Correlate multiple metrics (CPU + memory + latency + 5XX + connections)
- ✅ Use logs for **root cause**, not just symptoms
- ✅ Check recent code / config / infra **deployments**
- ✅ Avoid blind restarts
- ✅ Automate frequent or repeatable fixes

**Interview-ready summary:**

> I troubleshoot using CloudWatch by correlating alarms with metrics, analyzing OS-level data via CloudWatch Agent, querying logs with Log Insights, 
identifying root cause, and then applying targeted fixes or automation.

---

## 3. CloudWatch Live Troubleshooting Interview Scenarios

Each scenario follows this structure:

- **Question**
- **Best answer / steps**
- **What the interviewer is testing**

### 3.1 EC2 Is Slow but CPU Is Normal

- **Question**  
  Your EC2 instance is slow, but CPU utilization is only 20%. How do you troubleshoot?

- **Best answer**
  - Check **memory and disk metrics** using CloudWatch Agent.
  - If memory is high, suspect a **memory leak**.
  - Check **disk usage**, especially root volume.
  - Analyze application logs using **CloudWatch Log Insights** for errors or timeouts.
  - Check network metrics for **packet drops** or throttling.

- **Interviewer is testing**
  - OS-level monitoring knowledge
  - Understanding of CloudWatch Agent
  - Root-cause and correlation thinking

---

### 3.2 High CPU Alarm Triggered Suddenly

- **Question**  
  A CloudWatch alarm for `CPUUtilization > 80%` triggered suddenly. What will you do?

- **Best answer**
  - Check if **traffic increased** by looking at ALB `RequestCount`.
  - If traffic increased, verify **Auto Scaling** actions.
  - If traffic is normal, inspect logs for **infinite loops / stuck processes**.
  - Check any **recent deployments** or configuration changes.
  - Apply scaling or fix the application bug as needed.

- **Interviewer is testing**
  - Alarm correlation
  - Auto Scaling awareness
  - Avoiding blind restarts

---

### 3.3 Application Returns 502 / 503 Errors

- **Question**  
  Users report 502/503 errors. How do you troubleshoot using CloudWatch?

- **Best answer**
  - Check ALB **5XX metrics**:
    - `HTTPCode_ELB_5XX`
    - `HTTPCode_Target_5XX`
  - Differentiate between **ELB 5XX** (LB issue) and **Target 5XX** (app issue).
  - Check **target health status**.
  - Analyze **backend logs** for crashes or DB failures.
  - Restart or scale backend services if needed.

- **Interviewer is testing**
  - Load balancer knowledge
  - Metric interpretation
  - Backend troubleshooting skills

---

### 3.4 EC2 Instance Not Reachable via SSH

- **Question**  
  Your EC2 instance is unreachable via SSH. How do you debug without restarting?

- **Best answer**
  - Check **System** and **Instance Status Checks**.
  - Inspect **network metrics** to confirm if traffic is reaching the instance.
  - Review **CloudWatch system logs** for kernel or disk errors.
  - Validate **Security Groups** and **NACLs**.
  - If needed, **detach the root volume**, attach it to another instance, repair, and reattach.

- **Interviewer is testing**
  - AWS infrastructure knowledge
  - Safe recovery methods
  - CloudWatch logs usage

---

### 3.5 Disk Full Alert Triggered

- **Question**  
  A disk usage alarm crosses 90%. What are your steps?

- **Best answer**
  - Confirm disk metrics using **CloudWatch Agent**.
  - Check logs for `"No space left on device"`.
  - Identify large log files or data directories.
  - Clean or rotate logs.
  - Extend the **EBS volume** if required.

- **Interviewer is testing**
  - Preventive monitoring
  - Real production troubleshooting
  - Cost and stability awareness

---

### 3.6 CloudWatch Alarm Keeps Flapping

- **Question**  
  Your CloudWatch alarm keeps switching between `OK` and `ALARM`. Why?

- **Best answer**
  - Threshold is too aggressive.
  - Evaluation periods are too short.
  - Temporary spikes are triggering the alarm.
  - Fix by:
    - Increasing evaluation periods
    - Smoothing thresholds
    - Using **composite alarms**

- **Interviewer is testing**
  - Alert fatigue awareness
  - Alarm tuning knowledge

---

### 3.7 Memory Usage Increasing Gradually

- **Question**  
  Memory usage increases gradually and the application crashes every few hours. What do you do?

- **Best answer**
  - Confirm memory metrics via **CloudWatch Agent**.
  - Identify **memory leak patterns** in logs.
  - Restart the service as a **temporary** workaround.
  - Work with the dev team to **fix the code**.
  - Resize the instance only if justified after fixing leaks.

- **Interviewer is testing**
  - Pattern recognition
  - Long-running issue analysis

---

### 3.8 CloudWatch Logs Not Appearing

- **Question**  
  Logs are not showing up in CloudWatch. What could be wrong?

- **Best answer**
  - CloudWatch Agent might not be running.
  - IAM role / permissions may be missing.
  - Log file path in the config may be incorrect.
  - There may be a network access issue to CloudWatch.
  - Restart the agent and revalidate configuration.

- **Interviewer is testing**
  - Agent troubleshooting
  - IAM awareness

---

### 3.9 RDS Connections Exhausted

- **Question**  
  Your application is failing due to DB connection limits. How do you troubleshoot?

- **Best answer**
  - Check `DatabaseConnections` metric.
  - Correlate with **application traffic metrics**.
  - Analyze logs for **connection leaks** (connections not closed).
  - Enable / tune **connection pooling**.
  - Scale RDS instance or add **read replicas**.

- **Interviewer is testing**
  - Cross-service monitoring
  - Database fundamentals

---

### 3.10 No Alerts but Users Complain

- **Question**  
  Users complain about issues, but no CloudWatch alarms fired. Why?

- **Best answer**
  - Thresholds might be incorrect.
  - Missing key metrics such as **memory**, **latency**, or **app-level error rates**.
  - No **application-level monitoring** (only infra-level).
  - Add **custom metrics** and better alarms (e.g., latency, 5XX, business KPIs).

- **Interviewer is testing**
  - Proactive monitoring mindset
  - Observability maturity

---

## 4. Advanced Monitoring & SRE-Style Interview Q&A

These answers use an SRE lens:

- **Signal → Impact → Decision → Automation**

### 4.1 Monitoring vs Observability

- **Strong answer**
  - Monitoring tells you **when known problems happen** (e.g., CPU > 80%, 5XX > 5%).  
  - Observability helps you understand **why and how** a system behaves using **metrics, logs, and traces**, especially for unknown or complex 
  failures.  
  - In AWS, **CloudWatch** provides monitoring, and combined with **X-Ray**, it supports observability.

- **SRE angle**
  - SREs focus on **why** something failed, not just **that** it failed.

---

### 4.2 How Do You Decide What Metrics to Alert On?

- **Strong answer**
  - Alert only on **user-impacting** metrics, not infrastructure noise.
  - Examples:
    - High latency
    - Error rates (5XX)
    - Availability
    - Saturation (CPU, memory, disk)
  - Avoid alerts on metrics that **don’t require immediate action**.

- **SRE angle**
  - Reduce alert fatigue and focus on **symptoms**, not just low-level causes.

---

### 4.3 What Are the Four Golden Signals?

- **Strong answer**
  - The 4 Golden Signals are:
    - Latency
    - Traffic
    - Errors
    - Saturation
  - In CloudWatch:
    - Latency → `TargetResponseTime` (ALB) or similar metrics
    - Traffic → `RequestCount`
    - Errors → 5XX metrics
    - Saturation → CPU, memory, disk metrics

- **SRE angle**
  - This is a **Google SRE best practice**.

---

### 4.4 How Do You Prevent Alert Fatigue?

- **Strong answer**
  - Use **meaningful thresholds**.
  - Increase **evaluation periods**.
  - Use **composite alarms**.
  - Use **severity levels** for alerts (P1, P2, etc.).
  - Avoid alerts for **non-actionable** metrics.

- **SRE angle**
  - Alerts should be **rare but urgent**.

---

### 4.5 What Is an SLO and How Do You Monitor It?

- **Strong answer**
  - An **SLO (Service Level Objective)** defines acceptable reliability, e.g. **99.9% availability**.
  - Monitor by tracking **error rates** and **latency metrics** in CloudWatch and comparing against the SLO.

- **SRE angle**
  - SLOs drive **engineering priorities** and trade-offs.

---

### 4.6 What Is an Error Budget?

- **Strong answer**
  - **Error Budget = 100% − SLO**.  
  - It is the allowed amount of failure within a time window.
  - If the error budget is consumed too fast, **deployments pause** and focus shifts to reliability.

- **SRE angle**
  - Balances **speed vs reliability**.

---

### 4.7 How Do You Detect Unknown Failures?

- **Strong answer**
  - Use:
    - **CloudWatch Anomaly Detection**
    - Historical metric comparison
    - Log Insights **pattern analysis**
    - Distributed tracing with **X-Ray**

- **SRE angle**
  - Unknown failures need **behavior-based detection**, not static thresholds only.

---

### 4.8 How Do You Monitor Microservices?

- **Strong answer**
  - Per-service metrics (latency, errors, throughput)
  - Centralized logging
  - **Correlation IDs** in logs
  - Distributed tracing (X-Ray, OpenTelemetry stack)
  - Service-level dashboards

- **SRE angle**
  - End-to-end visibility across services is critical.

---

### 4.9 How Do You Decide Whether to Scale or Fix?

- **Strong answer**
  - If increased load causes **latency or saturation**, **scale**.
  - If metrics degrade **without traffic increase**, debug and **fix the application**.

- **SRE angle**
  - Scale for **demand**, fix for **inefficiency**.

---

### 4.10 How Do You Monitor Deployments?

- **Strong answer**
  - Compare **pre- and post-deployment metrics**.
  - Monitor **error rates** and **latency** during deployment windows.
  - Attach CloudWatch alarms to deployment stages.
  - Roll back if **thresholds are breached**.

- **SRE angle**
  - Deployments should be **observable events**, not blind changes.

---

### 4.11 What’s the Role of Automation in Monitoring?

- **Strong answer**
  - Monitoring should trigger **automated remediation**, e.g.:
    - Auto Scaling
    - Lambda-based self-healing actions
    - Automated restarts / failover
  - Humans handle **complex decisions**, not repetitive fixes.

- **SRE angle**
  - Automation reduces **MTTR** and human toil.

---

### 4.12 How Do You Measure MTTR and MTBF?

- **Strong answer**
  - **MTTR (Mean Time To Recovery)**: Time from alert to full recovery.
  - **MTBF (Mean Time Between Failures)**: Time between incidents.
  - CloudWatch timestamps and alarm history help calculate both.

- **SRE angle**
  - Reliability is **measurable** and should be tracked.

---

### 4.13 How Do You Monitor Third-Party Dependencies?

- **Strong answer**
  - Synthetic checks (health pings, API probes)
  - Latency monitoring
  - Timeout / error metrics
  - Failover and degradation alerts

- **SRE angle**
  - Your reliability depends on your external dependencies.

---

### 4.14 What Dashboards Do SREs Actually Use?

- **Strong answer**
  - Dashboards show:
    - User experience metrics
    - Service health and error budgets
    - Current incidents or degradation
  - Not just raw CPU graphs.

- **SRE angle**
  - Dashboards are for **decision-making**, not just pretty charts.

---

### 4.15 What Happens If Monitoring Itself Fails?

- **Strong answer**
  - Alerts on **missing metrics**.
  - Health checks on the **monitoring pipeline**.
  - Redundant monitoring paths or backup channels.

- **SRE angle**
  - Monitoring must be **reliable** too.

---

## 5. Power Statements for Interviews

- **Golden interview tip**

> I always correlate metrics, logs, and alarms before taking action, and I prefer data-driven fixes over blind restarts.

- **One-line CloudWatch power statement**

> CloudWatch helps me detect issues early, troubleshoot fast, and automate recovery across my infrastructure and applications.

- **Ultimate SRE statement**

> Monitoring is not about collecting data; it’s about enabling fast, reliable decisions with minimal human intervention.