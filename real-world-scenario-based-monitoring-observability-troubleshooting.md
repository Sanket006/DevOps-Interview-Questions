# Real-World Monitoring & Observability Troubleshooting Scenarios

Monitoring & Observability scenarios are very common in DevOps interviews, especially with tools like Prometheus, Grafana, CloudWatch, ELK, and Datadog.

Below are three curated sets of real-world, scenario-based Monitoring & Observability troubleshooting questions with detailed, interview-ready model answers written in simple and clear language:

- **Section 1**: Core Monitoring & Observability Troubleshooting Scenarios (20 Q&A)
- **Section 2**: Top 20 “Perfect Answer” Monitoring & Observability Scenarios
- **Section 3**: Advanced Prometheus + Grafana Deep-Dive Scenarios (20 Q&A)

---

## Section 1: Core Monitoring & Observability Troubleshooting Scenarios (20 Q&A)

### 1. Grafana dashboard is blank but Prometheus is running. What do you check?

**Answer:**

First, I check if Prometheus is added as a data source in Grafana and is showing “Connected.”

Then I verify the Prometheus URL and ensure Grafana can reach it.

Next, I check whether the query time range is correct and data actually exists.

Finally, I run the same query directly in Prometheus to confirm metrics are available.

---

### 2. Prometheus is running but no metrics are being scraped. How do you troubleshoot?

**Answer:**

I check the **Targets** page in Prometheus (`/targets`).

If targets are down, I inspect the `prometheus.yml` scrape configuration.

Then I verify network connectivity, firewall rules, and correct ports.

I also check if exporters are running and accessible.

---

### 3. Alert is firing even though the service is healthy. What could be wrong?

**Answer:**

The alert rule may have incorrect thresholds or missing conditions.

I check the alert expression and verify it against actual metric values.

I also look at the evaluation interval and duration (`for` clause).

Sometimes stale metrics or incorrect labels cause false alerts.

---

### 4. Alerts are not firing even when the service is down. What do you check?

**Answer:**

I first confirm whether the alert rule is loaded successfully in Prometheus.

Then I check if the metric exists and is updating.

I verify Alertmanager is running and properly configured.

Finally, I check logs to see if alerts are being suppressed or silenced.

---

### 5. CPU usage is high, but application performance is normal. How do you analyze this?

**Answer:**

High CPU alone doesn’t always mean an issue.

I check load average, thread usage, and context switching.

Then I correlate CPU metrics with latency, error rate, and throughput.

If user experience is unaffected, it may be expected workload behavior.

---

### 6. Memory usage keeps increasing over time. What does this indicate?

**Answer:**

This often indicates a memory leak.

I observe memory trends over a longer time period.

Then I check application logs, garbage collection metrics, and container memory limits.

If memory is not released after traffic drops, it confirms a leak.

---

### 7. Metrics are delayed in Grafana. How do you troubleshoot?

**Answer:**

I check Prometheus scrape intervals and Grafana refresh rates.

Then I look for high load on Prometheus causing slow ingestion.

I verify system resources like CPU, memory, and disk I/O.

Network latency between components can also cause delays.

---

### 8. Too many alerts are firing at once. What do you do?

**Answer:**

This is an alert storm.

I first group alerts using Alertmanager grouping rules.

Then I add severity levels and reduce noisy alerts.

I also review alert thresholds and remove non-actionable alerts.

---

### 9. Logs show errors but no alerts are triggered. Why?

**Answer:**

Metrics-based alerts may not be configured for those errors.

I check if log-based alerts are enabled (e.g., ELK or Datadog).

Then I verify error metrics like **5xx** counters exist.

If not, I add proper error instrumentation.

---

### 10. Prometheus disk usage is growing rapidly. What’s the cause?

**Answer:**

High cardinality metrics usually cause this.

I check metrics with too many labels like user IDs or request IDs.

Then I reduce unnecessary labels and adjust retention policies.

I may also enable data compaction and remote storage.

---

### 11. Node exporter is running but metrics are missing. What do you check?

**Answer:**

I check if Prometheus is scraping the node exporter endpoint.

Then I verify correct port (`9100`) and permissions.

I inspect firewall rules and ensure the exporter process is healthy.

I also check exporter logs for errors.

---

### 12. Latency is increasing but CPU and memory look normal. How do you debug?

**Answer:**

Latency issues often come from network, database, or external services.

I check network latency, DNS resolution time, and database query duration.

Then I use distributed tracing to find slow components.

This helps identify the exact bottleneck.

---

### 13. A Grafana alert is not sending notifications. What do you check?

**Answer:**

I verify the alert rule is in firing state.

Then I check the notification channel configuration (Slack, Email, PagerDuty).

I inspect Grafana logs for delivery errors.

I also test the notification channel manually.

---

### 14. Metrics disappeared after a restart. Why?

**Answer:**

Prometheus stores data locally, so restarting without persistent storage can wipe data.

I ensure persistent volumes are configured.

I also check retention settings.

Using remote storage like Thanos can prevent data loss.

---

### 15. Service is slow only during peak hours. How do you monitor this?

**Answer:**

I compare metrics between normal and peak traffic times.

I monitor request rate, response time, error rate, and saturation.

I also check autoscaling behavior and resource limits.

This helps identify capacity issues.

---

### 16. Kubernetes pod restarts frequently. How do you find the cause?

**Answer:**

I check pod events using `kubectl describe pod`.

Then I review logs for crashes or OOM errors.

I monitor memory and CPU usage over time.

Probes (liveness/readiness) misconfiguration can also cause restarts.

---

### 17. Too many logs are generated, increasing costs. What do you do?

**Answer:**

I review log levels and reduce debug or info logs in production.

Then I apply log filtering and sampling.

I remove unnecessary fields and enable log rotation.

This reduces storage and ingestion costs.

---

### 18. Monitoring shows everything healthy, but users complain. Why?

**Answer:**

Monitoring may focus only on infrastructure metrics.

I add user-centric metrics like page load time and API latency.

I also implement synthetic monitoring and real user monitoring (RUM).

This provides better visibility into user experience.

---

### 19. How do you correlate logs, metrics, and traces?

**Answer:**

I use a common request ID or trace ID across all systems.

Metrics show **what** is wrong, logs show **why**, and traces show **where**.

This correlation enables faster root cause analysis.

Tools like OpenTelemetry help unify observability.

---

### 20. What is your step-by-step approach to monitoring troubleshooting?

**Answer:**

I follow a structured approach:

1. Confirm the issue and impact  
2. Check alerts and dashboards  
3. Correlate metrics, logs, and traces  
4. Identify the bottleneck or anomaly  
5. Apply fix and monitor again  
6. Improve alerts to prevent recurrence  

This structured approach ensures faster resolution.

---

### Interview Tip for You, Sanket

When answering monitoring questions, always mention **correlation**:

- Metrics → Logs → Traces  

This shows real-world DevOps maturity 💯

---

## Section 2: Top 20 Monitoring & Observability Scenarios with “Perfect Answers”

These are exactly what interviewers ask for Monitoring & Observability rounds, especially for DevOps / SRE roles.

Below are the **Top 20 most-asked Monitoring & Observability scenarios** with **crisp, structured, and easy-to-remember answers**.

---

### 1. Monitoring shows CPU is normal, but users report slowness. Why?

**Perfect Answer:**

CPU alone doesn’t reflect user experience.

I check latency, error rate, database response time, and network metrics.

I also review distributed traces to find slow downstream services.

This helps identify bottlenecks beyond CPU.

---

### 2. Prometheus is up, but no metrics are visible in Grafana.

**Perfect Answer:**

I verify Prometheus is added as a Grafana data source and is connected.

Then I check Prometheus `/targets` to ensure scraping is successful.

I also validate the dashboard queries and time range.

---

### 3. Alerts are firing repeatedly even though the issue is fixed.

**Perfect Answer:**

This is usually due to alert conditions not resetting.

I check the alert expression, `for` duration, and metric freshness.

I also look for stale metrics and ensure Alertmanager is not silencing incorrectly.

---

### 4. Alert didn’t fire when production went down. What went wrong?

**Perfect Answer:**

I verify whether the alert rule was loaded and active.

Then I check if the metric existed during the failure.

I also confirm Alertmanager connectivity and notification channels.

Missing instrumentation is a common cause.

---

### 5. High memory usage keeps increasing over time.

**Perfect Answer:**

This indicates a memory leak.

I analyze memory trends and garbage collection metrics.

Then I inspect application logs and container memory limits.

If memory doesn’t drop after traffic reduces, it confirms the leak.

---

### 6. Prometheus disk usage is growing rapidly.

**Perfect Answer:**

This is usually caused by high-cardinality metrics.

I identify metrics with excessive labels.

Then I reduce unnecessary labels and adjust retention policies.

Remote storage can also help.

---

### 7. Too many alerts are firing at once (alert storm).

**Perfect Answer:**

I use Alertmanager grouping and deduplication.

Then I apply severity levels and remove non-actionable alerts.

Only alerts that require action should reach on-call engineers.

---

### 8. Logs show errors, but alerts never triggered.

**Perfect Answer:**

Metrics-based alerts may not cover log errors.

I check if error metrics (like 5xx counters) exist.

If not, I implement log-based alerts or proper error instrumentation.

---

### 9. Grafana dashboards are slow to load.

**Perfect Answer:**

I check query complexity and time range.

Then I optimize PromQL queries and reduce dashboard panels.

I also verify Prometheus performance and system resources.

---

### 10. Service latency increases only during peak traffic.

**Perfect Answer:**

I compare metrics during normal vs peak hours.

I analyze request rate, saturation, autoscaling behavior, and database load.

This usually indicates capacity or scaling issues.

---

### 11. Node exporter is running, but no metrics are scraped.

**Perfect Answer:**

I verify Prometheus scrape configuration and target status.

Then I check firewall rules and exporter port (`9100`).

Exporter logs also help identify permission issues.

---

### 12. Metrics disappeared after Prometheus restart.

**Perfect Answer:**

Prometheus stores data locally.

If persistent storage is not configured, metrics are lost on restart.

I ensure persistent volumes or use remote storage like Thanos.

---

### 13. Kubernetes pods restart frequently.

**Perfect Answer:**

I check pod events and container logs.

Then I monitor memory usage for OOM kills.

Incorrect liveness or readiness probes can also cause restarts.

---

### 14. Everything looks healthy, but customers complain.

**Perfect Answer:**

Infrastructure metrics don’t show user experience.

I add SLIs/SLOs, synthetic monitoring, and real user monitoring.

This bridges the gap between system health and user perception.

---

### 15. Prometheus scraping fails intermittently.

**Perfect Answer:**

I check network stability and DNS resolution.

Then I review scrape timeout and interval settings.

High load on Prometheus can also cause intermittent failures.

---

### 16. Logs are consuming too much storage and cost.

**Perfect Answer:**

I reduce log levels in production.

Then I apply filtering, sampling, and log rotation.

Only useful logs should be retained long-term.

---

### 17. How do you find the root cause of a production issue quickly?

**Perfect Answer:**

I follow the **metrics → logs → traces** approach.

Metrics identify the problem, logs explain it, and traces locate it.

This gives faster and accurate root cause analysis.

---

### 18. Alert notifications are not reaching Slack or Email.

**Perfect Answer:**

I verify the alert is firing.

Then I test the notification channel configuration.

I also check Alertmanager or Grafana logs for delivery failures.

---

### 19. How do you reduce false alerts?

**Perfect Answer:**

I tune thresholds using historical data.

I add `for` durations and severity levels.

Only actionable alerts should wake up engineers.

---

### 20. What is your monitoring troubleshooting mindset?

**Perfect Answer:**

I focus on impact first, then signals.

I correlate metrics, logs, and traces.

After fixing, I improve alerts to prevent recurrence.

Monitoring is not just detection—it’s prevention.

---

### 🔑 Golden Interview Formula (Remember This)

- Metrics tell **WHAT**  
- Logs tell **WHY**  
- Traces tell **WHERE**  

Say this once in an interview and you instantly stand out 💯

---

## Section 3: Advanced Prometheus + Grafana Deep-Dive Scenarios (20 Q&A)

These **Prometheus + Grafana deep-dive scenarios** are exactly what interviewers ask when they want to test **real production experience**, not just theory.

Below are **20 advanced, real-world Prometheus + Grafana scenarios** with deep, structured answers.

---

### 1. Prometheus is running but `/targets` shows many targets as DOWN

**Deep-Dive Answer:**

I first check the error message on the targets page (timeout, connection refused, 404).

Then I verify network connectivity, DNS resolution, firewall rules, and correct ports.

I also validate scrape configs and labels in `prometheus.yml`.

Exporter health and permissions are common root causes.

---

### 2. Grafana dashboards show “No data” but Prometheus has metrics

**Deep-Dive Answer:**

I confirm the data source connection in Grafana.

Then I check the dashboard’s time range and query filters.

I run the same PromQL query in Prometheus directly.

Mismatch in labels or variables often causes this.

---

### 3. Prometheus disk usage is growing uncontrollably

**Deep-Dive Answer:**

This indicates high-cardinality metrics.

I use `tsdb_status` to identify top series.

Then I remove unnecessary labels and reduce scrape frequency.

Retention tuning or remote storage (Thanos) is applied.

---

### 4. Grafana panels load very slowly

**Deep-Dive Answer:**

I inspect the query complexity and reduce time range.

I optimize PromQL using recording rules.

Then I check Prometheus CPU and memory usage.

Too many panels and heavy queries slow dashboards.

---

### 5. Alert fires late even though the issue occurred earlier

**Deep-Dive Answer:**

I check the scrape interval and evaluation interval.

Long scrape intervals delay alert detection.

I also inspect Prometheus load and rule execution time.

Tuning intervals improves alert responsiveness.

---

### 6. Alert fires repeatedly for the same issue

**Deep-Dive Answer:**

I verify Alertmanager grouping and deduplication.

Then I check alert expressions for proper reset logic.

Stale metrics or missing `for` duration cause repetition.

Correct labeling prevents duplicate alerts.

---

### 7. Metrics disappear after Prometheus restart

**Deep-Dive Answer:**

Prometheus uses local storage.

Without persistent volumes, data is lost on restart.

I configure persistent storage or use Thanos for durability.

Retention settings are also reviewed.

---

### 8. High CPU usage in Prometheus itself

**Deep-Dive Answer:**

This usually means too many time series or heavy queries.

I reduce label cardinality and scrape frequency.

I move expensive queries to recording rules.

Scaling Prometheus or sharding may be required.

---

### 9. Grafana alert works but Prometheus alert does not

**Deep-Dive Answer:**

Grafana alerts and Prometheus alerts are independent.

I verify the alert rule is loaded in Prometheus.

Then I check rule syntax and evaluation errors.

Alertmanager connectivity is also verified.

---

### 10. Service latency is high but CPU and memory are normal

**Deep-Dive Answer:**

Latency is often caused by downstream dependencies.

I analyze request duration histograms.

Then I use tracing to identify slow services or databases.

Network and DNS latency are also checked.

---

### 11. Prometheus scraping fails intermittently

**Deep-Dive Answer:**

I check network stability and DNS reliability.

Then I inspect scrape timeouts and intervals.

High Prometheus load can cause scrape drops.

Exporter performance also matters.

---

### 12. Too many metrics but no meaningful insights

**Deep-Dive Answer:**

This means poor metric design.

I define clear SLIs aligned with business impact.

Then I remove unused metrics and dashboards.

Observability should focus on outcomes, not noise.

---

### 13. Grafana variables are not populating correctly

**Deep-Dive Answer:**

I test the variable query in Prometheus.

Then I check label names and case sensitivity.

Time range mismatch can also cause empty variables.

Permissions and data source issues are verified.

---

### 14. Prometheus memory usage keeps increasing

**Deep-Dive Answer:**

High series count causes memory growth.

I inspect label cardinality and unused exporters.

Retention period and scrape frequency are tuned.

Vertical scaling may be needed temporarily.

---

### 15. Dashboards break after label changes

**Deep-Dive Answer:**

Hardcoded label names cause this.

I update queries or use flexible label matching.

Recording rules help stabilize dashboards.

Change management for metrics is important.

---

### 16. Alertmanager sends alerts but without useful context

**Deep-Dive Answer:**

I enrich alerts using labels and annotations.

Then I add runbook URLs and summary descriptions.

Good alerts should guide engineers to resolution quickly.

---

### 17. Different teams see different values for the same metric

**Deep-Dive Answer:**

This usually comes from inconsistent queries or time ranges.

I standardize dashboards and use recording rules.

Shared SLIs ensure consistency across teams.

---

### 18. Prometheus queries are expensive at large time ranges

**Deep-Dive Answer:**

I reduce raw queries over long ranges.

Recording rules pre-compute results.

I also limit dashboard default time ranges.

This improves performance significantly.

---

### 19. Metrics exist but alerts never fire

**Deep-Dive Answer:**

I test the alert expression manually.

Then I check threshold logic and `for` duration.

Evaluation intervals and missing labels are reviewed.

Alert rule syntax errors are common.

---

### 20. How do you design a production-grade Prometheus + Grafana setup?

**Deep-Dive Answer:**

I ensure:

- Low cardinality metrics  
- Clear SLIs and SLOs  
- Recording rules for performance  
- Actionable alerts  
- Persistent storage or Thanos  
- Clean, focused dashboards  

This ensures scalability and reliability.

---

### 🧠 Interview Power Line (Use This)

> “I focus on low-cardinality metrics, meaningful SLIs, and actionable alerts, not just collecting everything.”

Say this and you sound like a senior DevOps/SRE 💯