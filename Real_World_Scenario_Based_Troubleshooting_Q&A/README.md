# 🔍 Real-World Scenario-Based Troubleshooting Q&A

> **Actual production incidents and debugging stories** — not hypothetical scenarios, but the kinds of real failures DevOps teams face in live environments every week.

---

## 📌 Purpose

This folder goes beyond theory. Each file is based on **real operational incidents** — outages, failures, degraded services, and debugging marathons that DevOps engineers actually experience. Working through these builds the **debugging mindset** interviewers look for in experienced candidates.

---

## 📂 Files in This Folder

### ☁️ AWS Troubleshooting

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-aws-troubleshooting.md` | EC2, S3, IAM, VPC, ELB outages and debugging |
| `real-world-ec2&linux-troubleshooting-Q&A.md` | EC2 instance + Linux OS combined debugging |
| `real-world-scenario-based-linux-aws-devops-troubleshooting.md` | Cross-stack Linux + AWS + DevOps incidents |

### 🐧 Linux Troubleshooting

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-linux-troubleshooting.md` | CPU spikes, OOM kills, disk full, process hangs |

### 🐳 Docker & Kubernetes

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-docker-troubleshooting.md` | Container crashes, image pull failures, networking |
| `real-world-scenario-based-kubernetes-troubleshooting.md` | Pod CrashLoopBackOff, node NotReady, PVC issues |
| `real-world-scenario-based-k8s+argocd-troubleshooting-Q&A.md` | ArgoCD sync failures + K8s combined incidents |

### 🔄 CI/CD — Jenkins

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-jenkins-troubleshooting.md` | Pipeline failures, agent disconnects, plugin issues |

### 🏗️ Terraform

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-terraform-troubleshooting.md` | State corruption, lock issues, plan/apply failures |

### 📊 Monitoring & Observability

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-monitoring-observability-troubleshooting.md` | Alert storms, missing metrics, dashboard failures |
| `real-world-scenario-based-cloudwatch-datadog-troubleshooting.md` | CloudWatch + Datadog incident debugging |
| `real-world-scenario-based-cw-monitoring-troubleshooting-Q&A.md` | CloudWatch-specific monitoring Q&A |

### ⚙️ General DevOps

| File | Incidents Covered |
|---|---|
| `real-world-scenario-based-devops-troubleshooting.md` | Broad real-world DevOps challenges |
| `real-world-devops-challenges.md` | Common day-to-day DevOps engineering problems |
| `real-world-scenario-based-devops-structured-qa.md` | Structured Q&A: OOMKill, HPA, SRE error budgets, GitOps, platform engineering |

---

## 💡 How to Use

> [!TIP]
> Treat each scenario like a real on-call incident — you are the engineer on duty.

```
Step 1 — Read the problem description
Step 2 — What is your FIRST action? (Write it down)
Step 3 — What commands would you run? (Write them out)
Step 4 — Identify the root cause hypothesis
Step 5 — Read the provided resolution
Step 6 — Note what you would do differently next time
```

---

## ⬅️ Before This Folder

Ensure you've completed:
→ [`Core_Topic_Q&A/`](../Core_Topic_Q&A/) — tool fundamentals
→ [`Production_Style_Question_Sets/`](../Production_Style_Question_Sets/) — structured scenario practice

---

*Part of the [Interview_Questions](../README.md) preparation hub.*
