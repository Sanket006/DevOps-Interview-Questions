# Jenkins Interview Questions – Detailed Answers

---

## 1️⃣ How do you design Jenkins for high availability and disaster recovery?

I design Jenkins as production-grade infrastructure:

* **Controller High Availability**: Use Jenkins controller on Kubernetes or EC2 with persistent storage (EBS/EFS).
* **Externalize State**: Store Jenkins home on shared storage (EFS or NFS).
* **Backup Strategy**: Scheduled backups of Jenkins home, plugins, job configs, credentials (using backup plugins or snapshots).
* **Configuration as Code (JCasC)**: Manage Jenkins config declaratively to recreate environments quickly.
* **Immutable Infrastructure**: Rebuild controllers via Terraform/CloudFormation.
* **Monitoring**: Integrate with Prometheus + Grafana.
* **Disaster Recovery Plan**: Document RTO/RPO, test restore procedures regularly.

---

## 2️⃣ Declarative vs Scripted Pipeline — when would you choose one over the other?

**Declarative Pipeline:**

* Structured and easier to maintain
* Built-in validation
* Best for standardized CI/CD workflows
* Better for teams

**Scripted Pipeline:**

* More flexible
* Advanced logic and dynamic behavior
* Complex workflows or conditional orchestration

👉 I use Declarative by default and Scripted only when advanced control flow is required.

---

## 3️⃣ How do you manage secrets securely in Jenkins without hardcoding?

* Use **Jenkins Credentials Manager**
* Store secrets in AWS Secrets Manager / HashiCorp Vault
* Inject secrets via environment variables
* Never store secrets in Jenkinsfile
* Use withCredentials block
* Restrict credential scope and apply RBAC

---

## 4️⃣ How do you handle pipeline failures in long-running CI/CD workflows?

* Break pipelines into stages
* Use retry() for transient failures
* Use timeout() to avoid hanging jobs
* Add proper logging and error handling
* Use post { failure { } } for notifications
* Rollback strategy for failed deployments

---

## 5️⃣ How do you scale Jenkins with dynamic agents?

* Use Kubernetes Plugin for dynamic pods
* EC2 Plugin for auto-scaling agents
* Use ephemeral agents (destroy after build)
* Separate build types via labels
* Optimize resource allocation per job

---

## 6️⃣ How do you optimize slow pipelines and identify bottlenecks?

* Analyze stage timing
* Enable pipeline stage view
* Parallelize independent stages
* Cache dependencies
* Use lightweight checkout
* Profile resource usage (CPU, Memory)

---

## 7️⃣ How do you version and review Jenkinsfiles like application code?

* Store Jenkinsfile in Git repo
* Pull request reviews mandatory
* Branch-based pipelines
* Shared libraries for reusable logic
* Enforce code reviews before merge

---

## 8️⃣ How do you implement approvals and manual gates for production releases?

* Use input step in pipeline
* Restrict approvers via RBAC
* Integrate with change management tools
* Add timeout to avoid blocking builds

---

## 9️⃣ How do you integrate Jenkins with SonarQube, security scans, and quality gates?

* Add SonarQube stage
* Use quality gate step
* Fail pipeline if gate fails
* Integrate SAST/DAST tools
* Publish reports as artifacts

---

## 🔟 How do you migrate from freestyle jobs to pipeline-as-code safely?

* Inventory existing jobs
* Convert one job at a time
* Use Jenkinsfile from SCM
* Test in lower environment
* Version control everything
* Gradual rollout and monitoring

---

These answers focus on production-grade DevOps practices rather than just Jenkins syntax.
