# CI/CD Troubleshooting Answers (Identify → Diagnose → Fix → Prevent)

## **Pipeline Failures**

### 1. Your Jenkins pipeline suddenly fails. How do you troubleshoot it?

**Identify**
The Jenkins pipeline that was previously running successfully has suddenly started failing. The first step is to determine at which stage the failure occurred and what error message Jenkins provides.

**Diagnose**

* Check the Jenkins pipeline console logs.
* Identify the stage where the failure occurred (build, test, deploy).
* Check recent changes in:

  * Source code
  * Jenkinsfile
  * Dependencies
  * Infrastructure
* Verify Jenkins agent status.
* Check system resources (CPU, memory, disk).

Useful commands:

```
docker ps
df -h
free -m
```

**Fix**

* Correct the failing stage issue (code error, dependency issue, configuration problem).
* Restart failed Jenkins agent if required.
* Re-run the pipeline after fixing the root cause.

**Prevent**

* Add pipeline stage validations.
* Implement automated tests before deployment.
* Enable monitoring and alerting for Jenkins.

---

### 2. A pipeline is stuck in running state. What could be the cause?

**Identify**
The pipeline is triggered but remains in the running state without progressing to the next stage.

**Diagnose**
Possible causes include:

* Jenkins executor shortage
* Agent not available
* Infinite loop in pipeline script
* Waiting for manual approval
* External service dependency delay

Check:

* Jenkins executor availability
* Pipeline logs
* Agent status

Commands:

```
ps aux
kubectl get pods
```

**Fix**

* Increase Jenkins executors.
* Restart stuck pipeline job.
* Fix pipeline script logic.
* Resolve external service dependency.

**Prevent**

* Configure pipeline timeouts.
* Implement pipeline monitoring.
* Use resource limits for builds.

---

### 3. A pipeline fails during Docker build stage.

**Identify**
The pipeline fails during the Docker image build process.

**Diagnose**
Common causes:

* Dockerfile syntax errors
* Missing dependencies
* Network issues while pulling base image
* Insufficient disk space

Check build logs and try building locally.

Commands:

```
docker build .
docker images
df -h
```

**Fix**

* Correct Dockerfile syntax.
* Install missing dependencies.
* Ensure correct base image.

**Prevent**

* Use Dockerfile linting.
* Implement caching in builds.
* Add automated Docker build tests in CI pipeline.

---

## **Code Integration** 

### 4. Code merges successfully but build fails.

**Identify**
The code was successfully merged into the repository (e.g., main or develop branch), but the CI/CD pipeline build stage fails after the merge. This usually indicates issues introduced by the new code or dependency conflicts.

**Diagnose**

* Check CI/CD pipeline logs to identify the failing stage.
* Review recent commit changes that were merged.
* Verify dependency versions.
* Check if tests are failing.
* Try building locally.

Commands:

```
npm install
npm run build
mvn clean install
```

**Fix**

* Resolve code errors introduced in the merge.
* Fix dependency conflicts.
* Update configuration files if required.
* Re-run the pipeline after fixing the issue.

**Prevent**

* Implement pull request validation.
* Enforce automated tests before merging.
* Use branch protection rules.

---

### 5. Pipeline does not trigger after Git commit.

**Identify**
A commit is pushed to the repository, but the CI/CD pipeline does not start automatically.

**Diagnose**
Possible causes include:

* Webhook misconfiguration
* CI/CD trigger not enabled
* Branch filtering rules
* Repository permissions

Check:

* Webhook settings in GitHub/GitLab/Bitbucket.
* CI/CD pipeline trigger configuration.
* Jenkins Git plugin configuration.

Commands:

```
git log
git branch
```

**Fix**

* Configure or correct the webhook URL.
* Enable pipeline triggers for the target branch.
* Verify repository access permissions.

**Prevent**

* Test webhook delivery regularly.
* Use automated trigger validation.
* Monitor pipeline trigger events.

---

## **Deployment Issues**

### 6. CI pipeline works but deployment fails in Kubernetes.

**Identify**
The CI stages such as build and image creation succeed, but the deployment stage fails when deploying the application to the Kubernetes cluster.

**Diagnose**

* Check Kubernetes deployment status.
* Verify pod status.
* Inspect logs of failing pods.
* Check image pull errors or configuration issues.

Commands:

```
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events
```

Common causes:

* ImagePullBackOff
* CrashLoopBackOff
* Incorrect environment variables
* Resource limits exceeded

**Fix**

* Correct image repository or tag.
* Fix environment variables or configuration.
* Increase resource limits if required.
* Re-deploy the application.

**Prevent**

* Implement pre-deployment validation.
* Use health checks and readiness probes.
* Monitor Kubernetes events and logs.

---

### 7. New deployment causes application crash.

**Identify**
After deploying a new version of the application, the application crashes or becomes unavailable.

**Diagnose**

* Check pod status in Kubernetes.
* Review application logs.
* Verify configuration changes.
* Check database connectivity or external services.

Commands:

```
kubectl get pods
kubectl logs <pod-name>
kubectl describe pod <pod-name>
```

Common causes:

* Application bug in new release
* Misconfigured environment variables
* Dependency version conflicts
* Resource limits too low

**Fix**

* Roll back to the previous stable version.
* Fix application configuration or code issue.
* Update resource limits.

**Prevent**

* Implement blue-green or canary deployments.
* Use automated testing before deployment.
* Monitor application health checks.

---

### 8. How do you rollback a failed deployment?

**Identify**
A new deployment introduces issues in production, so the goal is to quickly restore the previously working version.

**Diagnose**

* Confirm deployment failure using pod status and logs.
* Identify the last stable deployment revision.

Commands:

```
kubectl rollout history deployment <deployment-name>
kubectl rollout status deployment <deployment-name>
```

**Fix**
Rollback to the previous deployment revision.

```
kubectl rollout undo deployment <deployment-name>
```

Or rollback to a specific revision.

```
kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>
```

Verify rollback:

```
kubectl rollout status deployment <deployment-name>
```

**Prevent**

* Use deployment strategies like Blue-Green or Canary.
* Implement automated rollback policies.
* Use monitoring and alerts to detect failures early.

---

## **Performance Issues**

### 9. CI/CD pipeline takes 1 hour to complete. How will you optimize it?

**Identify**
The CI/CD pipeline execution time is very long (around 1 hour), which slows down development and deployment cycles.

**Diagnose**

* Identify which stage of the pipeline takes the most time (build, test, deploy).
* Analyze pipeline execution logs and stage timings.
* Check if builds run sequentially instead of parallel.
* Verify dependency installation time and caching usage.
* Check resource usage on build agents.

Commands:

```
top
docker stats
```

**Fix**

* Implement parallel stages in the pipeline.
* Enable dependency caching.
* Use incremental builds.
* Use lightweight Docker images.
* Run tests in parallel.

**Prevent**

* Continuously monitor pipeline duration.
* Optimize build scripts regularly.
* Use distributed build agents.

---

### 10. Multiple pipelines run simultaneously and Jenkins becomes slow.

**Identify**
When multiple CI/CD pipelines run at the same time, Jenkins performance degrades and builds become slow.

**Diagnose**

* Check Jenkins system resource usage (CPU, memory, disk).
* Check number of executors configured.
* Verify if builds are running on a single node.
* Check queue length in Jenkins.

Commands:

```
top
free -m
df -h
```

**Fix**

* Increase Jenkins executors.
* Add additional Jenkins agents (horizontal scaling).
* Optimize heavy build tasks.
* Use Kubernetes-based dynamic agents.

**Prevent**

* Implement build resource limits.
* Use distributed Jenkins architecture.
* Monitor Jenkins performance with tools like Prometheus and Grafana.

---

## **Security & Credentials**

### 11. Pipeline cannot access Docker registry.

**Identify**
The CI/CD pipeline fails when trying to pull or push Docker images to the Docker registry (Docker Hub, AWS ECR, or a private registry).

**Diagnose**

* Check pipeline logs for authentication errors.
* Verify Docker login configuration in the pipeline.
* Confirm registry permissions.

Commands:

```
docker login
docker pull <image>
```

**Fix**

* Configure correct registry credentials in the CI/CD tool.
* Use credential stores or secret managers.

**Prevent**

* Store credentials securely in Jenkins Credentials Manager or secret manager.
* Use IAM roles or service accounts where possible.

---

### 12. Pipeline fails due to expired credentials.

**Identify**
The pipeline previously worked but now fails due to authentication errors caused by expired credentials.

**Diagnose**

* Check authentication error messages.
* Verify credential expiry in the secret manager or credential store.

**Fix**

* Update or rotate the expired credentials.
* Reconfigure the pipeline with the new credentials.

**Prevent**

* Implement automated credential rotation.
* Use short-lived tokens with automatic refresh.

---

### 13. Secrets are exposed in pipeline logs. How will you fix this?

**Identify**
Sensitive information such as passwords, API keys, or tokens appears in pipeline logs, creating a security risk.

**Diagnose**

* Check pipeline scripts where secrets are used.
* Identify commands that print environment variables.

**Fix**

* Mask secrets in pipeline configuration.
* Use secret management tools (Vault, AWS Secrets Manager).

**Prevent**

* Avoid printing sensitive variables in logs.
* Implement secret masking in CI/CD tools.
* Use environment variable protection and access control.

---

## **Infrastructure Issues**

### 14. Pipeline fails during Terraform apply.

**Identify**
The CI/CD pipeline executes Terraform commands, but the failure occurs during the `terraform apply` stage when provisioning or updating infrastructure.

**Diagnose**

* Check Terraform logs in the pipeline output.
* Run `terraform plan` to identify configuration errors.
* Verify cloud credentials and permissions (AWS IAM / service accounts).
* Check state file issues or locking problems.

Commands:

```
terraform init
terraform validate
terraform plan
terraform apply
```

Common causes:

* Invalid Terraform configuration
* Missing IAM permissions
* Remote state lock
* Resource conflicts

**Fix**

* Correct Terraform configuration errors.
* Grant required IAM permissions.
* Unlock Terraform state if stuck.

Example:

```
terraform force-unlock <LOCK_ID>
```

**Prevent**

* Use `terraform validate` in the pipeline before apply.
* Implement remote state with locking (S3 + DynamoDB).
* Add approval gates before production apply.

---

### 15. Infrastructure deployment works locally but fails in CI/CD.

**Identify**
Infrastructure provisioning works successfully on the developer's local machine but fails when executed in the CI/CD pipeline environment.

**Diagnose**

* Compare local and CI/CD environments.
* Check Terraform version differences.
* Verify environment variables and credentials in the pipeline.
* Confirm network access to cloud APIs.

Commands:

```
terraform version
env
```

Common causes:

* Different Terraform versions
* Missing environment variables
* Missing credentials in CI/CD
* Network restrictions

**Fix**

* Use the same Terraform version in CI/CD.
* Configure environment variables and credentials properly.
* Ensure CI/CD runners have required network access.

**Prevent**

* Use Dockerized Terraform environments for consistency.
* Manage variables through CI/CD secrets.
* Document environment configuration requirements.

---

## **Artifact Management**

### 16. Build artifacts are not available for deployment.

**Identify**
The CI pipeline successfully completes the build stage, but the deployment stage fails because the required build artifacts (e.g., JAR, WAR, Docker image, or compiled package) are not found.

**Diagnose**

* Check whether artifacts were generated during the build stage.
* Verify artifact storage configuration (e.g., Nexus, Artifactory, S3, or Jenkins artifacts).
* Check pipeline logs for artifact upload errors.
* Confirm artifact path configuration in the pipeline.

Commands:

```
ls -la
find . -name "*.jar"
```

Common causes:

* Incorrect artifact path in pipeline configuration
* Artifact upload failure
* Artifact retention policy deleting files

**Fix**

* Correct artifact path in the pipeline configuration.
* Ensure artifact upload step runs successfully.
* Rebuild the project and store artifacts correctly.

**Prevent**

* Use centralized artifact repositories (Nexus/Artifactory).
* Validate artifact existence before deployment.
* Configure proper artifact retention policies.

---

### 17. Wrong version of artifact is deployed.

**Identify**
The deployment pipeline completes successfully, but the application runs an incorrect artifact version instead of the expected one.

**Diagnose**

* Check artifact version used in the deployment stage.
* Verify pipeline variables or tags controlling artifact selection.
* Check artifact repository for multiple versions.

Commands:

```
docker images
kubectl describe deployment <deployment-name>
```

Common causes:

* Incorrect version tag
* Pipeline using cached artifact
* Deployment script referencing "latest" tag

**Fix**

* Deploy the correct artifact version.
* Update pipeline configuration to reference explicit version tags.

**Prevent**

* Avoid using "latest" tags in production.
* Implement versioned artifacts with build numbers.
* Add validation checks before deployment.

---

## **Production Release**

### 18. Application deploys successfully but users see old version.

**Identify**
The deployment pipeline completes successfully and new pods or services are created, but end users still see the old version of the application.

**Diagnose**

* Check if the correct container image tag is deployed.
* Verify Kubernetes deployment image version.
* Check caching layers (CDN, browser cache, reverse proxy).
* Verify if rolling update completed successfully.

Commands:

```
kubectl describe deployment <deployment-name>
kubectl get pods
kubectl rollout status deployment <deployment-name>
```

Common causes:

* Old image tag being reused
* CDN or browser caching
* Rolling update not completed

**Fix**

* Deploy with a new unique image tag.
* Clear CDN or cache.
* Restart deployment.

Example:

```
kubectl rollout restart deployment <deployment-name>
```

**Prevent**

* Use immutable image tags (build numbers or commit SHA).
* Implement cache invalidation during deployment.
* Enable proper deployment monitoring.

---

### 19. Deployment works in staging but fails in production.

**Identify**
The application deploys successfully in the staging environment but fails when deployed to production.

**Diagnose**

* Compare configuration between staging and production.
* Check environment variables and secrets.
* Verify infrastructure differences (database endpoints, network rules).

Commands:

```
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

Common causes:

* Different environment configurations
* Missing secrets or credentials
* Network restrictions or firewall rules

**Fix**

* Synchronize environment configurations.
* Add missing secrets or variables.
* Update infrastructure permissions.

**Prevent**

* Use Infrastructure as Code for environment consistency.
* Use configuration management tools.
* Implement automated environment validation.

---

### 20. CI/CD pipeline accidentally deploys broken code to production.

**Identify**
The pipeline successfully deploys an application, but the deployed version contains bugs or broken functionality.

**Diagnose**

* Check if automated tests ran during the pipeline.
* Verify code review and merge approval process.
* Check monitoring alerts and error logs.

Commands:

```
kubectl logs <pod-name>
kubectl rollout history deployment <deployment-name>
```

**Fix**

* Roll back to the last stable deployment.

```
kubectl rollout undo deployment <deployment-name>
```

* Fix the bug in code and redeploy.

**Prevent**

* Add automated testing stages (unit, integration, security).
* Implement approval gates before production deployment.
* Use canary or blue-green deployments to reduce risk.

---