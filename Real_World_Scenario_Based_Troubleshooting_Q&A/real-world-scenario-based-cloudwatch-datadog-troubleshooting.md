# CloudWatch & Datadog Troubleshooting – Interview Preparation Guide

These scenario-based **CloudWatch & Datadog troubleshooting questions** are very commonly asked in **DevOps / SRE interviews**, especially for **monitoring, alerting, and incident response roles**.

Below are **20 real-world scenarios with detailed but easy-to-understand model answers**, followed by the **Top 20 most-asked interview scenarios with perfectly structured answers** (exactly what interviewers like to hear).

---

## ✅ 20 Scenario-Based CloudWatch & Datadog Troubleshooting Questions

### (With Detailed Model Answers)

### 1. CloudWatch alarm is not triggering even though CPU usage is high

**Answer:**

* Check if the alarm threshold and comparison operator are correct
* Verify the metric namespace (`AWS/EC2`) and instance ID
* Confirm the alarm evaluation period and datapoints
* Ensure the instance is sending metrics (instance is running)
* Check if the alarm is in `INSUFFICIENT_DATA` state

---

### 2. EC2 instance is unresponsive but CloudWatch shows normal metrics

**Answer:**

* Default CloudWatch metrics are at 5-minute intervals, missing short spikes
* Enable **Detailed Monitoring** (1-minute)
* Check OS-level metrics using **CloudWatch Agent**
* Verify disk I/O wait and memory usage inside the instance

---

### 3. Memory utilization is not visible in CloudWatch

**Answer:**

* CloudWatch does not provide memory metrics by default
* Install and configure **CloudWatch Agent**
* Enable memory, disk, and swap metrics in agent config
* Restart the agent and verify logs

---

### 4. Datadog agent is running but no data is visible

**Answer:**

* Verify API key configuration
* Check agent status:

  ```bash
  datadog-agent status
  ```
* Ensure the host has internet access
* Check firewall / security group rules
* Validate correct site (`datadoghq.com` / `datadoghq.eu`)

---

### 5. Too many false alerts in CloudWatch

**Answer:**

* Increase evaluation periods
* Use **anomaly detection** instead of static thresholds
* Adjust thresholds based on historical data
* Combine multiple metrics using **composite alarms**

---

### 6. Datadog alerts firing repeatedly for the same issue

**Answer:**

* Alert recovery conditions may not be configured
* Missing alert suppression or renotification delay
* Incorrect alert query logic
* Use alert grouping and mute windows

---

### 7. Lambda function errors not triggering CloudWatch alarm

**Answer:**

* Ensure alarm is created on the **Errors** metric
* Check correct function name and region
* Verify evaluation period
* Confirm error is an actual Lambda failure, not a handled exception

---

### 8. Application latency increased but CloudWatch shows normal CPU

**Answer:**

* CPU may not be the bottleneck
* Check memory, disk I/O, and network latency
* Enable Application Load Balancer metrics
* Use **AWS X-Ray** or **Datadog APM** for tracing

---

### 9. CloudWatch Logs are not appearing

**Answer:**

* Verify IAM role permissions (`logs:PutLogEvents`)
* Check log group and stream names
* Ensure application logging path is correct
* Check CloudWatch Agent logs

---

### 10. Datadog dashboard shows gaps in metrics

**Answer:**

* Agent might be restarting
* Network connectivity issues
* Incorrect metric rollup or time window
* Host auto-scaling causing short-lived instances

---

### 11. High API latency but Datadog CPU and memory look normal

**Answer:**

* Investigate downstream dependencies (DB, external APIs)
* Enable Datadog APM tracing
* Check slow queries or connection pools
* Review error rates and request queues

---

### 12. CloudWatch alarm stuck in INSUFFICIENT_DATA

**Answer:**

* Metric not publishing data
* Wrong namespace or dimension
* Resource recently created or deleted
* Evaluation period too large

---

### 13. Datadog host missing after auto-scaling

**Answer:**

* Agent not installed in launch template
* Incorrect auto-discovery configuration
* Host tags not inherited
* Check cloud integration setup

---

### 14. Disk space alerts firing but disk looks fine

**Answer:**

* Alert may target wrong mount point
* Deleted files still held by running process
* Check inode usage
* Verify correct metric (`disk.used_percent`)

---

### 15. CloudWatch billing alerts not working

**Answer:**

* Billing alerts work only in `us-east-1`
* Ensure billing metrics are enabled
* Check SNS topic subscription
* Verify threshold and currency

---

### 16. Datadog not collecting container metrics

**Answer:**

* Enable Docker integration
* Run agent with Docker socket access
* Check container labels and tags
* Verify agent version supports container metrics

---

### 17. High network latency not visible in CloudWatch

**Answer:**

* CloudWatch provides limited network metrics
* Enable **VPC Flow Logs**
* Use Datadog Network Performance Monitoring
* Check security groups and route tables

---

### 18. CloudWatch alarm triggers too late

**Answer:**

* Evaluation period too long
* Datapoints to alarm set too high
* Metric publishing interval delay
* Use faster metrics (1-minute)

---

### 19. Datadog shows high memory usage but app is fine

**Answer:**

* Linux uses memory for caching
* Check actual free vs cached memory
* Use memory pressure metrics
* Tune alert logic instead of raw memory percentage

---

### 20. No alerts during downtime

**Answer:**

* Monitoring depends on the monitored resource
* If host is down, metrics stop
* Use heartbeat or synthetic monitoring
* Enable external uptime checks

---

## 🔥 Interview Tip (Very Important)

Always explain troubleshooting in this order:

**Alert → Metric → Threshold → Logs → Traces → Root Cause → Fix**

---

# 🔥 Top 20 Most-Asked CloudWatch & Datadog Scenarios

### (With Perfect Interview-Ready Answers)

### 1. CPU is normal but application is slow

**Perfect Answer:**

CPU alone doesn’t indicate performance. I would:

* Check memory, disk I/O, and network latency
* Review load balancer latency
* Use Datadog APM / AWS X-Ray to trace slow requests
* Look for DB bottlenecks or external API delays

---

### 2. CloudWatch alarm didn’t trigger during outage

**Perfect Answer:**

* Alarm depends on metrics; during outage metrics stop
* Alarm likely went into `INSUFFICIENT_DATA`
* Use Route53 health checks / synthetic monitoring
* Add heartbeat-based alerts for availability

---

### 3. Memory usage not visible in CloudWatch

**Perfect Answer:**

* CloudWatch doesn’t provide memory by default
* Install and configure CloudWatch Agent to collect:

  * Memory
  * Disk usage
  * Swap metrics

---

### 4. Too many false alerts

**Perfect Answer:**

* Thresholds not tuned to workload
* Increase evaluation periods
* Use Datadog anomaly detection
* Combine metrics using CloudWatch composite alarms

---

### 5. Datadog agent running but no data

**Perfect Answer:**

* Verify API key
* Check agent status
* Confirm internet access
* Validate correct Datadog site (US/EU)
* Check firewall or proxy issues

---

### 6. Alarm triggers too late

**Perfect Answer:**

* Evaluation period too long
* Metric publishing interval delay
* Enable 1-minute detailed monitoring
* Reduce datapoints required to trigger

---

### 7. Application latency increased but CPU is low

**Perfect Answer:**

* CPU isn’t the bottleneck
* Check DB response time
* Analyze request traces
* Inspect network and downstream services

---

### 8. CloudWatch Logs not appearing

**Perfect Answer:**

* Verify IAM permissions (`logs:PutLogEvents`)
* Check log group / stream names
* Validate application log path
* Review CloudWatch Agent logs

---

### 9. Datadog alerts firing repeatedly

**Perfect Answer:**

* Missing recovery condition
* No renotification delay
* Alert query not stabilized
* Implement alert grouping and mute windows

---

### 10. Metrics missing after Auto Scaling

**Perfect Answer:**

* Agent not installed in launch template
* Auto-discovery misconfigured
* Host tags missing
* Validate cloud integration

---

### 11. Disk alert shows full but space looks free

**Perfect Answer:**

* Deleted files still open by processes
* Inode exhaustion
* Wrong mount point monitored
* Verify using `lsof` and inode metrics

---

### 12. CloudWatch alarm stuck in INSUFFICIENT_DATA

**Perfect Answer:**

* Metric not emitting data
* Incorrect namespace or dimensions
* Resource recently created
* Evaluation window too large

---

### 13. High error rate but logs show no errors

**Perfect Answer:**

* Errors may be handled internally
* Check HTTP status codes
* Inspect load balancer metrics
* Use APM to find failed traces

---

### 14. Datadog dashboard shows gaps

**Perfect Answer:**

* Agent restarts
* Network issues
* Short-lived instances
* Incorrect rollup interval

---

### 15. Billing alert not working

**Perfect Answer:**

* Billing alerts work only in `us-east-1`
* Billing metrics must be enabled
* SNS subscription must be confirmed
* Threshold configuration verified

---

### 16. Lambda errors not triggering alarm

**Perfect Answer:**

* Alarm not created on Errors metric
* Wrong function name or region
* Exception handled inside code
* Evaluation period misconfigured

---

### 17. Network latency issue not visible

**Perfect Answer:**

* CloudWatch network metrics are limited
* Enable VPC Flow Logs
* Use Datadog Network Monitoring
* Validate route tables and security groups

---

### 18. High memory usage but app healthy

**Perfect Answer:**

* Linux uses memory for cache
* Check memory pressure, not raw usage
* Tune alert thresholds
* Focus on swap and OOM events

---

### 19. No alerts during complete server crash

**Perfect Answer:**

* Monitoring depends on the host
* Metrics stop when host dies
* Use external synthetic checks
* Implement heartbeat monitoring

---

### 20. Interviewer asks: “How do you debug alerts?”

**Perfect Answer (Golden Answer):**

I follow a structured approach:

**Alert → Metric → Threshold → Logs → Traces → Root Cause → Fix → Prevention**

---

## 🏆 Final Interview Tip (Very Important)

Always talk in **production-incident language**, not theory.
