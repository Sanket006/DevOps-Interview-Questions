# Real-World Production Scenario-Based Interview Questions

## Git & GitHub – STAR Format Answers (DevOps / Cloud Engineer Level)

---

## ⭐ Scenario 1: Accidental Push to Main Branch

### ❓ Interview Question:

“Tell me about a time when someone accidentally pushed broken code directly to the main branch in production.”

### ✅ STAR Answer

**S – Situation:**
In our production repository hosted on GitHub, a developer accidentally pushed untested code directly to the `main` branch, which triggered the CI/CD pipeline and deployed to production.

**T – Task:**
As the DevOps engineer, my responsibility was to immediately stabilize production, prevent downtime, and implement controls to avoid such incidents in the future.

**A – Action:**

* Immediately identified the problematic commit using `git log`.
* Reverted the commit using `git revert <commit-id>` instead of `git reset` to preserve history.
* Temporarily disabled the GitHub Actions deployment workflow.
* Enabled branch protection rules in GitHub:

  * Required Pull Requests before merge
  * Enabled required reviews (minimum 2 reviewers)
  * Required status checks to pass before merging
  * Disabled direct pushes to main
* Conducted a short training session on Git branching strategy.

**R – Result:**

* Production was restored within 15 minutes.
* No data loss occurred.
* Post-incident, we had zero direct pushes to main.
* Improved governance and audit compliance.

---

## ⭐ Scenario 2: Resolving Major Merge Conflict in Release Branch

### ❓ Interview Question:

“Describe a time when you handled a complex merge conflict before a production release.”

### ✅ STAR Answer

**S – Situation:**
Before a quarterly release, multiple feature branches were merged into the release branch, resulting in heavy merge conflicts in configuration and environment files.

**T – Task:**
Ensure a clean release build without breaking production functionality.

**A – Action:**

* Used `git fetch` and `git diff` to analyze differences.
* Checked conflicting sections carefully.
* Coordinated with developers to understand intended changes.
* Created a temporary integration branch for safe resolution.
* Performed local build + test before pushing.
* Used GitHub Pull Request review for validation.

**R – Result:**

* Successfully resolved conflicts without regression issues.
* Release deployed successfully.
* Introduced GitFlow branching strategy moving forward.

---

## ⭐ Scenario 3: Large Sensitive File Committed to Repository

### ❓ Interview Question:

“Tell me about a time when a secret or large file was accidentally committed to GitHub.”

### ✅ STAR Answer

**S – Situation:**
A developer accidentally committed AWS access keys and a large `.env` file to the public repository.

**T – Task:**
Remove sensitive data immediately and secure infrastructure.

**A – Action:**

* Immediately revoked exposed AWS keys.
* Used `git filter-repo` (or BFG Repo Cleaner) to remove the file from entire history.
* Forced push cleaned history.
* Enabled GitHub Secret Scanning.
* Added `.env` to `.gitignore`.
* Implemented pre-commit hooks.
* Moved secrets to AWS Secrets Manager.

**R – Result:**

* No unauthorized access occurred.
* Improved security posture.
* Passed security audit review.

---

## ⭐ Scenario 4: CI Pipeline Failing After Merge

### ❓ Interview Question:

“Tell me about a time when your GitHub Actions pipeline failed after a merge.”

### ✅ STAR Answer

**S – Situation:**
After merging a feature branch, GitHub Actions failed during Docker build stage.

**T – Task:**
Identify root cause and restore CI pipeline.

**A – Action:**

* Reviewed GitHub Actions logs.
* Identified missing environment variable.
* Checked workflow YAML file.
* Fixed incorrect environment reference.
* Re-ran pipeline.
* Added validation step in PR stage.

**R – Result:**

* Pipeline restored within 30 minutes.
* Reduced future pipeline failures by adding lint checks.

---

## ⭐ Scenario 5: Production Rollback Using Git Tags

### ❓ Interview Question:

“How did you handle a production rollback using Git?”

### ✅ STAR Answer

**S – Situation:**
A new release caused application crashes in production.

**T – Task:**
Roll back safely to previous stable version.

**A – Action:**

* Identified previous stable Git tag (`v1.2.3`).
* Redeployed using tagged Docker image.
* Created hotfix branch from stable tag.
* Applied patch and tested.
* Merged fix via PR.

**R – Result:**

* Downtime limited to 10 minutes.
* Implemented mandatory staging validation before production release.

---

# 🔥 Advanced Production-Level Topics You Should Also Prepare

* Branch protection policies
* GitHub Actions security best practices
* GPG commit signing
* Monorepo vs multi-repo strategy
* Git submodules
* Code Owners file
* PR approval workflow
* Protected environments in GitHub
* Disaster recovery using Git backups

---

# 💡 Interview Tip (For DevOps Roles)

When answering Git/GitHub production scenarios:

* Focus on prevention + automation
* Mention governance & compliance
* Highlight monitoring & audit logs
* Show ownership mindset

---

# Advanced Production Scenarios

## GitHub Enterprise Security + GitOps (ArgoCD) Failure Recovery

### STAR Format – Senior DevOps / Platform Engineer Level

---

# 🔐 PART 1: GitHub Enterprise Security Scenarios

---

## ⭐ Scenario 1: Unauthorized Access Detected in GitHub Enterprise

### ❓ Interview Question:

“Tell me about a time you detected suspicious activity in GitHub Enterprise.”

### ✅ STAR Answer

**S – Situation:**
In GitHub Enterprise Cloud, we received alerts from audit logs indicating multiple failed login attempts followed by a successful login from an unfamiliar IP address.

**T – Task:**
Investigate potential breach, secure repositories, and prevent data exfiltration.

**A – Action:**

* Immediately revoked the user’s access token.
* Forced password reset and enforced SSO re-authentication.
* Enabled mandatory 2FA for all users.
* Reviewed audit logs for repo cloning or unusual downloads.
* Rotated organization-level secrets.
* Implemented IP allow-list policy.
* Enabled Dependabot and secret scanning across org.

**R – Result:**

* No data exfiltration detected.
* Security posture improved.
* Passed subsequent compliance audit (SOC2).

---

## ⭐ Scenario 2: Compromised GitHub Actions Runner

### ❓ Interview Question:

“How did you handle a compromised self-hosted GitHub Actions runner?”

### ✅ STAR Answer

**S – Situation:**
One of our self-hosted runners in Kubernetes was suspected of executing malicious code from a pull request.

**T – Task:**
Isolate runner, prevent lateral movement, and secure CI environment.

**A – Action:**

* Immediately removed runner from GitHub org.
* Terminated underlying Kubernetes pod.
* Rotated all CI secrets.
* Switched to ephemeral runners.
* Restricted PR workflows from forks.
* Used environment protection rules requiring approvals.

**R – Result:**

* Eliminated risk of secret leakage.
* CI security hardened.
* Reduced attack surface significantly.

---

## ⭐ Scenario 3: Preventing Secret Leakage in Enterprise

### ❓ Interview Question:

“How did you prevent secret leakage in large enterprise repositories?”

### ✅ STAR Answer

**S – Situation:**
Multiple teams were pushing microservices to a monorepo. Risk of secret exposure increased.

**T – Task:**
Implement enterprise-grade secret governance.

**A – Action:**

* Enabled GitHub Advanced Security.
* Enforced secret scanning & push protection.
* Integrated HashiCorp Vault for dynamic secrets.
* Implemented pre-commit hooks using gitleaks.
* Enforced branch protection rules.
* Restricted admin privileges.

**R – Result:**

* Zero secret leakage incidents post-implementation.
* Improved compliance and DevSecOps maturity.

---

# 🚀 PART 2: GitOps + ArgoCD Failure Recovery Scenarios

---

## ⭐ Scenario 4: ArgoCD Out-of-Sync Production Cluster

### ❓ Interview Question:

“Tell me about a time when ArgoCD showed OutOfSync in production.”

### ✅ STAR Answer

**S – Situation:**
ArgoCD dashboard showed production application in OutOfSync state after a hotfix deployment.

**T – Task:**
Identify drift and restore cluster state safely.

**A – Action:**

* Compared live state vs Git desired state.
* Found manual kubectl change in cluster.
* Reverted manual change.
* Disabled kubectl direct access to production.
* Enforced Git-only changes via RBAC.

**R – Result:**

* Cluster restored to Git as single source of truth.
* Eliminated configuration drift.

---

## ⭐ Scenario 5: Failed ArgoCD Deployment Due to Broken Manifest

### ❓ Interview Question:

“How did you recover from a failed GitOps deployment?”

### ✅ STAR Answer

**S – Situation:**
A wrong Kubernetes manifest was merged into Git, causing ArgoCD sync failure and CrashLoopBackOff.

**T – Task:**
Restore production quickly without impacting users.

**A – Action:**

* Identified faulty commit using Git history.
* Reverted commit via Pull Request.
* Triggered ArgoCD sync.
* Added mandatory manifest validation (kubeval + policy checks).
* Implemented staging sync before production auto-sync.

**R – Result:**

* Production stabilized within 20 minutes.
* Prevented similar issues via pre-merge validation.

---

## ⭐ Scenario 6: ArgoCD Application Deleted Accidentally

### ❓ Interview Question:

“An ArgoCD application was accidentally deleted. What did you do?”

### ✅ STAR Answer

**S – Situation:**
An engineer accidentally deleted ArgoCD Application CR from cluster.

**T – Task:**
Restore application without downtime.

**A – Action:**

* Verified Git still contained application definition.
* Reapplied Application manifest.
* Enabled App-of-Apps pattern.
* Enabled auto-prune + self-heal.

**R – Result:**

* Application restored in minutes.
* Improved GitOps resilience model.

---

# 🔥 Senior-Level Discussion Points

* GitOps as single source of truth
* Drift detection & reconciliation
* RBAC hardening
* OIDC-based GitHub authentication
* Signed commits & supply chain security
* SLSA framework
* Zero-trust CI/CD pipelines

---

# 🎯 How to Impress Interviewer

Mention:

* Prevention > Reaction
* Automation > Manual Fix
* Governance + Compliance
* Root Cause Analysis (RCA)
* Metrics improvement

---
