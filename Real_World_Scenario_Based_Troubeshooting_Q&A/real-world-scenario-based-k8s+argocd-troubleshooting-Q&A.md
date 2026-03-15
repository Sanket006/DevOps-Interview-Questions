# 🚀 Kubernetes + Argo CD Troubleshooting Scenarios (STAR Method)

---

# ❓ Scenario 1: Argo CD Application Stuck in "OutOfSync"

## Interview Question

"Tell me about a time when an Argo CD application was stuck in OutOfSync state and not deploying properly. How did you troubleshoot it?"

---

## ✅ STAR Answer

### ⭐ S – Situation

In our production Amazon EKS cluster, an application managed by Argo CD was showing an "OutOfSync" status even after multiple sync attempts. The application was critical for customer-facing APIs, and deployment was blocked.

---

### ⭐ T – Task

I was responsible for:

* Identifying the root cause
* Restoring cluster to desired state
* Ensuring zero downtime
* Preventing recurrence

---

### ⭐ A – Action

I followed a structured troubleshooting approach:

1️⃣ Compared desired vs live state

* Used `argocd app diff` to identify manifest differences
* Checked for manual kubectl changes in cluster

2️⃣ Identified Root Cause

* Found that a Kubernetes Mutating Admission Controller was automatically injecting sidecar containers
* This caused live state to differ from Git manifests

3️⃣ Implemented Fix

* Configured Argo CD to ignore specific fields using resource customizations
* Updated manifests to reflect sidecar injection configuration
* Re-synced application

4️⃣ Preventive Measure

* Disabled manual kubectl access in production
* Enforced Git-only change policy

---

### ⭐ R – Result

* Application successfully synchronized without downtime
* Eliminated recurring OutOfSync alerts
* Improved GitOps reliability
* Reduced manual intervention incidents by 50%

---

# ❓ Scenario 2: Argo CD Sync Failing Due to RBAC Issue

## Interview Question

"Describe a time when Argo CD failed to deploy resources due to permission issues."

---

## ✅ STAR Answer

### ⭐ S – Situation

During deployment to a new namespace in EKS, Argo CD sync operations started failing with permission denied errors.

---

### ⭐ T – Task

I needed to:

* Diagnose permission error
* Fix RBAC configuration
* Ensure least privilege model

---

### ⭐ A – Action

1️⃣ Checked Argo CD logs

* Used `kubectl logs` on Argo CD server pod
* Identified error related to ClusterRole binding

2️⃣ Root Cause

* Argo CD ServiceAccount lacked permissions to create Custom Resource Definitions (CRDs)

3️⃣ Resolution

* Created namespace-scoped Role and RoleBinding
* Avoided granting cluster-admin access
* Tested deployment in staging first

4️⃣ Validation

* Verified successful sync
* Audited RBAC policies for other namespaces

---

### ⭐ R – Result

* Restored deployment within 20 minutes
* Maintained least privilege security posture
* Reduced future RBAC-related incidents

---

# ❓ Scenario 3: Kubernetes Pods CrashLoopBackOff After GitOps Deployment

## Interview Question

"Tell me about a time when pods went into CrashLoopBackOff after Argo CD deployment."

---

## ✅ STAR Answer

### ⭐ S – Situation

After a GitOps-based production release, newly deployed pods began entering CrashLoopBackOff state.

---

### ⭐ T – Task

I needed to:

* Identify whether issue was app-level or infra-level
* Restore service availability quickly
* Implement safer deployment practice

---

### ⭐ A – Action

1️⃣ Checked Pod Logs

* Used `kubectl logs` to inspect container errors
* Identified missing environment variable from Secret

2️⃣ Root Cause

* Secret name mismatch between manifest and cluster
* Incorrect value committed in Git

3️⃣ Immediate Fix

* Reverted Git commit to previous working version
* Argo CD automatically rolled back cluster state

4️⃣ Long-Term Prevention

* Implemented validation in CI pipeline
* Added pre-sync hook validation job
* Added readiness & liveness probes

---

### ⭐ R – Result

* Service restored within 10 minutes
* Improved rollback confidence
* Reduced production incidents caused by config errors

---

# 🎯 Why These Answers Are Strong

* Demonstrates structured troubleshooting
* Shows real production-level debugging
* Covers GitOps + Kubernetes internals
* Includes measurable impact
* Shows ownership and preventive mindset

---
