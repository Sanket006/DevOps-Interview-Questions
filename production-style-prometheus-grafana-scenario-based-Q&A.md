# Real-World Production Scenario-Based Interview Questions

## Prometheus & Grafana – STAR Format Answers

---

## ⭐ Scenario 1: Sudden Spike in CPU Usage Not Visible in Dashboard

❓ Interview Question:
"Tell me about a time when production CPU usage spiked but your dashboard did not reflect the issue immediately."

### ✅ STAR Answer

**S – Situation:**
In our Kubernetes production cluster, users reported application slowness. However, our Grafana dashboard was showing normal CPU metrics.

**T – Task:**
I had to identify why the monitoring system was not reflecting real-time CPU spikes and ensure accurate visibility.

**A – Action:**

* Checked Prometheus scrape interval (was set to 60s).
* Verified node-exporter metrics collection.
* Identified that short CPU bursts were being averaged out.
* Reduced scrape interval to 15s.
* Modified PromQL query from `avg(rate(container_cpu_usage_seconds_total[5m]))` to `rate(container_cpu_usage_seconds_total[1m])`.
* Updated Grafana panel refresh interval.

**R – Result:**
Dashboard started reflecting CPU spikes accurately. We detected a memory leak in one microservice and resolved it before SLA breach.

---

## ⭐ Scenario 2: Alerts Not Triggering During Production Outage

❓ Interview Question:
"Explain a time when Prometheus alerts failed during a production outage."

### ✅ STAR Answer

**S – Situation:**
During a critical API outage, no alerts were triggered in Slack.

**T – Task:**
Investigate why Alertmanager did not notify the team.

**A – Action:**

* Checked Prometheus alert rules using `/alerts` endpoint.
* Found alert firing state was active.
* Verified Alertmanager config.
* Discovered misconfigured Slack webhook URL.
* Implemented alert routing and fallback email notification.
* Added synthetic alert testing in CI pipeline.

**R – Result:**
Alerts were restored within 30 minutes. Introduced redundancy in alert channels.

---

## ⭐ Scenario 3: High Cardinality Causing Prometheus Crash

❓ Interview Question:
"Have you faced Prometheus performance issues due to high cardinality?"

### ✅ STAR Answer

**S – Situation:**
Prometheus server memory usage kept increasing and eventually crashed.

**T – Task:**
Identify root cause and stabilize monitoring system.

**A – Action:**

* Checked `tsdb status`.
* Identified high cardinality labels (`user_id`, `session_id`).
* Removed dynamic labels from instrumentation.
* Applied relabel_configs.
* Implemented recording rules for aggregation.
* Enabled retention policy tuning.

**R – Result:**
Reduced time series count by 70%. Memory stabilized and no further crashes.

---

## ⭐ Scenario 4: Grafana Dashboard Showing No Data After Deployment

❓ Interview Question:
"What would you do if Grafana shows no data after a new deployment?"

### ✅ STAR Answer

**S – Situation:**
After deploying a new version of an application, Grafana panels displayed "No Data".

**T – Task:**
Restore observability quickly.

**A – Action:**

* Checked Prometheus target status (`/targets`).
* Found new pod labels changed (`app=v2`).
* Existing PromQL query was filtering `app=v1`.
* Updated dashboard queries.
* Implemented label consistency standards in CI.

**R – Result:**
Metrics visibility restored in 10 minutes. Prevented future dashboard breakage.

---

## ⭐ Scenario 5: Disk Space Exhaustion Due to Prometheus Retention

❓ Interview Question:
"Describe a production incident where Prometheus storage caused issues."

### ✅ STAR Answer

**S – Situation:**
Production monitoring node ran out of disk space, affecting metric ingestion.

**T – Task:**
Restore system and prevent recurrence.

**A – Action:**

* Checked `/var/lib/prometheus` usage.
* Reduced retention from 30d to 15d.
* Enabled remote_write to long-term storage (S3 via Thanos).
* Implemented storage monitoring alert.

**R – Result:**
Freed 60% disk space. Long-term metrics preserved externally.

---

# 🔥 Advanced Enterprise-Level Scenarios

1. Multi-cluster Prometheus federation design
2. Thanos/Cortex for HA and long-term storage
3. Prometheus HA pair setup with external labels
4. Alert fatigue reduction strategy
5. SLO/SLI-based alerting implementation
6. Blackbox exporter for synthetic monitoring
7. Grafana RBAC & multi-tenant dashboard isolation
8. Incident postmortem with monitoring gap analysis

---
