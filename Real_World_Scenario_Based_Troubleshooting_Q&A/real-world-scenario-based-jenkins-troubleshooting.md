# Jenkins & CI/CD Interview Troubleshooting Guide

This README contains **real-world, scenario-based Jenkins and CI/CD troubleshooting questions with clear, interview-ready answers**. The content is structured exactly as expected in DevOps Engineer (Fresher–Mid) interviews.

---

## 🔹 Jenkins Troubleshooting Scenarios (20 Questions)

### 1. Jenkins job is stuck in the queue. What will you check?

**Answer:**

I would check:

* Executor availability – Ensure Jenkins nodes have free executors.
* Node labels – Verify the job’s label matches available nodes.
* Offline nodes – Check if the assigned node is offline.
* Concurrency limits – Check if “Do not allow concurrent builds” is enabled.
* Resource constraints – CPU/RAM issues on agent.

---

### 2. Jenkins build fails with “permission denied” error. How do you fix it?

**Answer:**

Steps:

* Check file permissions of the script or workspace.
* Ensure executable permission (`chmod +x script.sh`).
* Verify Jenkins user ownership.
* If Docker is used, check Docker socket permissions.
* Confirm correct user is running the Jenkins agent.

---

### 3. Jenkins pipeline fails after agent restart. Why?

**Answer:**

Possible reasons:

* Agent lost connection during build
* Workspace got deleted
* Agent Java version mismatch
* Agent authentication token expired

**Fix:**

Reconnect agent, reconfigure credentials, and re-run the job.

---

### 4. Jenkins is running but UI is not accessible. What will you do?

**Answer:**

* Check Jenkins service status.
* Verify port availability (8080).
* Check firewall/security group.
* Inspect Jenkins logs.
* Confirm disk space availability.

---

### 5. Jenkins build works manually but fails via webhook. Why?

**Answer:**

Reasons:

* Incorrect webhook URL
* Jenkins job not configured for SCM polling
* Missing credentials
* Wrong branch configuration
* Network access issue (GitHub → Jenkins)

---

### 6. Jenkins pipeline fails at docker build step. What could be wrong?

**Answer:**

Possible issues:

* Docker not installed on agent
* Jenkins user not in docker group
* Docker daemon not running
* Insufficient disk space
* Invalid Dockerfile syntax

---

### 7. Jenkins shows “No space left on device”. How do you resolve it?

**Answer:**

Steps:

* Clean old build artifacts
* Delete unused workspaces
* Remove old Docker images
* Configure log rotation
* Increase disk size if needed

---

### 8. Jenkins job randomly fails sometimes but passes other times. Why?

**Answer:**

Common causes:

* Resource contention
* Network instability
* Race conditions in scripts
* External dependency failures
* Unstable test cases

**Solution:**

Add retries, improve error handling, and stabilize tests.

---

### 9. Jenkins pipeline cannot access AWS even with credentials. Why?

**Answer:**

Possible reasons:

* Wrong AWS credentials scope
* Incorrect IAM permissions
* Environment variables not exported
* Jenkins agent role mismatch
* Region not specified

---

### 10. Jenkins is very slow. What checks will you perform?

**Answer:**

* Check CPU and RAM usage
* Analyze number of running jobs
* Check plugins causing delays
* Review disk I/O
* Increase executor or node count

---

### 11. Jenkins pipeline fails after Git repository migration. Why?

**Answer:**

Possible issues:

* SSH key mismatch
* HTTPS credentials outdated
* Webhook URL not updated
* Repository permissions missing
* Branch name changed

---

### 12. Jenkins job cannot find environment variables. What’s wrong?

**Answer:**

Reasons:

* Variables defined in wrong scope
* Missing environment block in pipeline
* Not exported in shell script
* Credentials not properly bound

---

### 13. Jenkins build fails with “script not found”. How to debug?

**Answer:**

Steps:

* Verify script path
* Check workspace location
* Ensure SCM checkout succeeded
* Confirm correct branch
* Check file case sensitivity

---

### 14. Jenkins agent is online but builds don’t run on it. Why?

**Answer:**

Possible reasons:

* Label mismatch
* Executor count is zero
* Node is restricted
* Job restricted to master
* Node temporarily disabled

---

### 15. Jenkins pipeline fails after plugin update. What will you do?

**Answer:**

Steps:

* Check plugin compatibility
* Roll back plugin version
* Review Jenkins logs
* Test pipeline syntax
* Restart Jenkins if needed

---

### 16. Jenkins job keeps failing at npm install. Why?

**Answer:**

Possible issues:

* Node.js not installed on agent
* Wrong Node version
* Proxy/network issues
* Permission problems
* Corrupted cache

---

### 17. Jenkins pipeline times out unexpectedly. How do you fix it?

**Answer:**

Steps:

* Check pipeline timeout settings
* Increase timeout duration
* Optimize long-running steps
* Split pipeline stages
* Check agent resource limits

---

### 18. Jenkins webhook is triggered but job doesn’t start. Why?

**Answer:**

Possible causes:

* Job disabled
* Wrong branch filter
* SCM trigger not enabled
* Token mismatch
* Jenkins queue blocked

---

### 19. Jenkins fails with “java.lang.OutOfMemoryError”. What to do?

**Answer:**

Steps:

* Increase JVM heap size
* Reduce plugin load
* Clean old builds
* Add more memory
* Restart Jenkins

---

### 20. Jenkins pipeline fails only in production environment. Why?

**Answer:**

Reasons:

* Environment variable mismatch
* Permission differences
* Network/firewall restrictions
* Different dependency versions
* Secrets missing

---

### 🔥 Interview Tip

Always answer in **3 parts**:

* Possible cause
* How to verify
* How to fix

---

## 🔹 Top 20 CI/CD Pipeline Scenario-Based Interview Questions

### 1. CI/CD pipeline suddenly fails after a code merge. What do you do?

**Perfect Answer:**

I first check which stage failed (build, test, or deploy). Then I:

* Review the error logs
* Identify the commit that caused the failure
* Roll back or fix the faulty code
* Re-run the pipeline
* Add tests to prevent similar issues

---

### 2. Pipeline passes in CI but fails in CD (deployment). Why?

**Perfect Answer:**

This usually happens due to environment differences.

I check:

* Environment variables
* Configuration files
* Permissions
* Network/firewall rules
* Dependency versions

Ensuring environment parity fixes this issue.

---

### 3. Pipeline is too slow. How do you optimize it?

**Perfect Answer:**

I optimize by:

* Running tests in parallel
* Caching dependencies
* Splitting pipelines into stages
* Removing unnecessary steps
* Using faster build agents

---

### 4. A deployment failed in production. What is your immediate action?

**Perfect Answer:**

I immediately:

* Roll back to the last stable version
* Ensure service availability
* Analyze logs and metrics
* Fix the root cause
* Re-deploy after validation

---

### 5. Pipeline fails randomly. How do you troubleshoot?

**Perfect Answer:**

Random failures usually mean:

* Resource constraints
* Network instability
* Flaky tests
* External service dependency

I add retry logic, improve test stability, and monitor resources.

---

### 6. Secrets are exposed in pipeline logs. How do you prevent this?

**Perfect Answer:**

I:

* Store secrets in secret managers (Vault, AWS Secrets Manager)
* Use Jenkins credentials / GitHub secrets
* Mask secrets in logs
* Never hard-code secrets in code or scripts

---

### 7. CI pipeline fails due to test failures. What’s your approach?

**Perfect Answer:**

I:

* Identify failing test cases
* Check if tests are flaky or valid
* Fix code or test logic
* Re-run pipeline
* Improve test coverage and reliability

---

### 8. Pipeline works on one branch but not on another. Why?

**Perfect Answer:**

Possible reasons:

* Different pipeline configs
* Missing files
* Different environment variables
* Branch-specific scripts

I compare both branches and standardize configuration.

---

### 9. Docker image build fails in pipeline. How do you fix it?

**Perfect Answer:**

I check:

* Dockerfile syntax
* Base image availability
* Disk space
* Docker daemon access
* Permissions

Then rebuild and test locally.

---

### 10. Kubernetes deployment fails from CI/CD pipeline. Why?

**Perfect Answer:**

Common causes:

* Invalid YAML
* Missing namespaces
* Incorrect kubeconfig
* Insufficient RBAC permissions

I validate manifests and test deployment manually.

---

### 11. Pipeline succeeds but application is not accessible. What next?

**Perfect Answer:**

I check:

* Application logs
* Service and ingress
* Load balancer health
* Security groups/firewall
* Environment variables

---

### 12. How do you handle zero-downtime deployments in CI/CD?

**Perfect Answer:**

I use:

* Blue-Green deployment
* Rolling updates
* Canary deployments

This ensures traffic is always served during deployment.

---

### 13. Pipeline fails due to dependency version conflicts. How do you solve it?

**Perfect Answer:**

I:

* Lock dependency versions
* Use dependency managers
* Cache dependencies
* Test changes in staging before production

---

### 14. Pipeline triggers but does not deploy. Why?

**Perfect Answer:**

Possible reasons:

* Conditional stages failing
* Approval gate pending
* Manual intervention required
* Environment restrictions

I check pipeline conditions and logs.

---

### 15. CI/CD pipeline breaks after infrastructure change. What do you do?

**Perfect Answer:**

I:

* Review infrastructure changes
* Validate IaC state
* Update pipeline configs
* Test in staging
* Re-run pipeline

---

### 16. Pipeline fails only during peak hours. Why?

**Perfect Answer:**

This indicates:

* Resource exhaustion
* High traffic load
* API rate limits

I scale infrastructure and optimize pipeline execution.

---

### 17. How do you ensure code quality in CI/CD pipelines?

**Perfect Answer:**

I integrate:

* Static code analysis
* Unit tests
* Code coverage checks
* Security scanning

Pipelines fail if quality gates are not met.

---

### 18. Deployment succeeds but monitoring shows errors. What do you do?

**Perfect Answer:**

I:

* Roll back if needed
* Analyze logs and metrics
* Identify root cause
* Fix and redeploy
* Improve alerting

---

### 19. Pipeline fails due to insufficient permissions. How do you fix it?

**Perfect Answer:**

I:

* Review IAM/RBAC roles
* Grant least-privilege access
* Validate credentials
* Test permissions manually

---

### 20. How do you make CI/CD pipelines reliable and scalable?

**Perfect Answer:**

I ensure:

* Modular pipelines
* Infrastructure as Code
* Parallel execution
* Monitoring and alerts
* Proper rollback strategies

---

### 🔥 Winning Interview Formula

**Problem → Cause → Action → Prevention**

---

## 🔹 Advanced CI/CD Production Outage Scenarios

### 1. A bad deployment took production down. What do you do first?

**Perfect Answer:**

First, I stop the pipeline immediately to prevent further damage.

Then I:

* Roll back to the last known stable version
* Restore service availability
* Inform stakeholders
* Analyze logs and metrics
* Fix root cause before redeployment

---

### 2. Pipeline deploys code, but production traffic is failing. Why?

**Perfect Answer:**

This usually indicates:

* Configuration mismatch
* Missing environment variables
* Secrets not injected
* Incompatible dependencies

I compare staging vs production configs and fix parity issues.

---

### 3. CI/CD pipeline accidentally deploys to production instead of staging. How do you prevent this?

**Perfect Answer:**

I prevent this by:

* Adding approval gates for production
* Using environment-specific credentials
* Restricting branch-based deployments
* Enforcing RBAC
* Adding deployment confirmations

---

### 4. Deployment succeeded but monitoring shows error spikes. What next?

**Perfect Answer:**

I:

* Pause traffic or rollout
* Check application and infra metrics
* Compare with previous release
* Roll back if errors persist
* Improve canary analysis

---

### 5. Pipeline fails during database migration in production. How do you handle it?

**Perfect Answer:**

I:

* Immediately stop deployment
* Roll back application (not DB blindly)
* Restore DB from backup if needed
* Fix migration scripts
* Re-run in staging before prod

---

### 6. CI/CD outage caused by expired secrets or certificates. How do you fix and prevent it?

**Perfect Answer:**

I:

* Rotate secrets immediately
* Restart affected services
* Use centralized secret managers
* Enable expiry alerts
* Automate secret rotation

---

### 7. Production deployment causes high CPU and memory usage. Why?

**Perfect Answer:**

Possible causes:

* Memory leaks
* Misconfigured autoscaling
* Infinite loops
* Incorrect resource limits

I analyze metrics, roll back, and optimize resources.

---

### 8. Rollback also fails. What do you do?

**Perfect Answer:**

I:

* Manually deploy last stable artifact
* Redirect traffic temporarily
* Restore infra state using IaC
* Perform deep root cause analysis
* Improve rollback strategy

---

### 9. Canary deployment shows mixed results. How do you decide?

**Perfect Answer:**

I:

* Compare latency, error rate, and throughput
* Validate business metrics
* Stop rollout if degradation exists
* Roll back or continue based on SLOs

---

### 10. Pipeline is healthy, but production is broken. Why?

**Perfect Answer:**

Because CI/CD only validates delivery, not runtime behavior.

Possible reasons:

* Missing runtime configs
* Infra drift
* External service failure

I rely on monitoring and runtime checks.

---

### 11. Production outage occurs during peak traffic deployment. What went wrong?

**Perfect Answer:**

Likely causes:

* No deployment window
* No traffic shaping
* Insufficient autoscaling

I schedule deployments during low traffic and use progressive rollouts.

---

### 12. CI/CD pipeline blocks hotfix deployment during outage. How do you handle it?

**Perfect Answer:**

I:

* Temporarily bypass non-critical stages
* Deploy hotfix with approval
* Restore pipeline rules afterward
* Document the exception

---

### 13. CI/CD outage caused by shared pipeline infrastructure failure. What do you do?

**Perfect Answer:**

I:

* Switch to backup CI/CD runners
* Restore pipeline services
* Isolate pipeline infrastructure
* Implement high availability

---

### 14. Production outage due to failed feature toggle. How do you fix it?

**Perfect Answer:**

I:

* Disable the feature toggle immediately
* Restore normal traffic
* Fix toggle logic
* Add validation checks

---

### 15. CI/CD pipeline deploys incompatible microservices. Why?

**Perfect Answer:**

Because of:

* Contract mismatch
* API version drift
* Independent releases

I use contract testing and versioning.

---

### 16. Deployment causes data inconsistency across services. How do you handle it?

**Perfect Answer:**

I:

* Pause further deployments
* Roll back affected services
* Reconcile data if needed
* Improve transaction handling

---

### 17. Pipeline passed security scan, yet production is breached. Why?

**Perfect Answer:**

Because:

* Security scans are limited
* Runtime threats exist

I add runtime security, monitoring, and regular audits.

---

### 18. CI/CD outage due to misconfigured IaC change. What’s your response?

**Perfect Answer:**

I:

* Roll back IaC change
* Restore infra state
* Fix configuration
* Add validation and plan reviews

---

### 19. Production deployment breaks backward compatibility. How do you prevent this?

**Perfect Answer:**

I:

* Enforce backward-compatible changes
* Use versioned APIs
* Test upgrades in staging
* Apply canary deployments

---

### 20. What post-mortem steps do you take after a CI/CD production outage?

**Perfect Answer:**

I:

* Document incident timeline
* Identify root cause
* Define corrective actions
* Update pipeline safeguards
* Share learnings with the team

---

## 🏆 Interview Gold Line

> **“My priority in any CI/CD outage is service recovery first, root cause second, and prevention forever.”**
