# Jenkins Interview Questions & Answers (DevOps)

This README contains **50 Jenkins interview questions with medium-length answers**, focused on **real DevOps interviews** for **freshers to 2–3 years of experience**. The content is structured for quick revision and interview preparation.

---

## 🔹 Jenkins Interview Questions & Answers (1–30)

### 1️⃣ What is Jenkins and why is it used?

Jenkins is an open-source automation server used to implement CI/CD pipelines. It helps automate building, testing, and deploying applications whenever code changes occur, improving delivery speed and reliability.

---

### 2️⃣ Explain CI/CD in Jenkins.

CI (Continuous Integration) automatically builds and tests code after each commit. CD (Continuous Delivery/Deployment) automates the release process. Jenkins orchestrates both using pipelines and plugins.

---

### 3️⃣ What are Jenkins jobs?

Jenkins jobs are tasks or projects that Jenkins executes, such as building code, running tests, or deploying applications. Examples include Freestyle jobs, Pipeline jobs, and Multibranch jobs.

---

### 4️⃣ What is a Jenkins Pipeline?

A Jenkins Pipeline is a set of automated steps written as code (Jenkinsfile) that defines the entire CI/CD process, from build to deployment, ensuring consistency and version control.

---

### 5️⃣ Difference between Freestyle job and Pipeline job?

Freestyle jobs are simple, UI-based, and less flexible. Pipeline jobs are code-driven, support complex workflows, versioning, and better error handling.

---

### 6️⃣ What is a Jenkinsfile?

A Jenkinsfile is a text file stored in the source code repository that defines pipeline stages and steps. It supports Declarative and Scripted pipeline syntax.

---

### 7️⃣ Declarative vs Scripted Pipeline?

Declarative pipelines use a structured syntax and are easier to read and maintain. Scripted pipelines use Groovy scripting and offer more flexibility but are harder to manage.

---

### 8️⃣ What are stages in Jenkins?

Stages represent different phases of the pipeline, such as Build, Test, and Deploy. They help visualize pipeline progress and identify failures quickly.

---

### 9️⃣ What are Jenkins agents (nodes)?

Agents are machines where Jenkins jobs run. The master controls scheduling, while agents perform build and deployment tasks to distribute workloads.

---

### 🔟 How does Jenkins work internally?

Jenkins monitors source code repositories, triggers jobs on changes, executes tasks on agents, stores build results, and provides logs and reports via a web UI.

---

### 1️⃣1️⃣ What are Jenkins plugins?

Plugins extend Jenkins functionality, enabling integration with Git, Docker, Kubernetes, AWS, Slack, and more. Jenkins has thousands of community-supported plugins.

---

### 1️⃣2️⃣ How do you trigger Jenkins builds?

Builds can be triggered manually, via Git webhooks, SCM polling, scheduled cron jobs, or automatically through pipeline triggers.

---

### 1️⃣3️⃣ What is SCM polling?

SCM polling periodically checks the repository for changes. If a change is detected, Jenkins triggers a build. Webhooks are preferred as they are more efficient.

---

### 1️⃣4️⃣ What is a Jenkins webhook?

A webhook notifies Jenkins immediately when code is pushed to a repository, triggering a build without polling delays.

---

### 1️⃣5️⃣ How do you secure Jenkins?

Security is managed using authentication (LDAP, OAuth), authorization (role-based access), credentials management, and HTTPS encryption.

---

### 1️⃣6️⃣ What is Jenkins credentials management?

It securely stores secrets like passwords, SSH keys, tokens, and certificates, preventing hardcoding of sensitive data in pipelines.

---

### 1️⃣7️⃣ What are environment variables in Jenkins?

Environment variables store dynamic values like build numbers, branch names, and paths. They can be global, job-level, or pipeline-specific.

---

### 1️⃣8️⃣ What is Blue-Green deployment using Jenkins?

Blue-Green deployment runs two environments. Jenkins deploys the new version to the idle environment and switches traffic after validation, minimizing downtime.

---

### 1️⃣9️⃣ How do you integrate Jenkins with Docker?

Jenkins builds Docker images, runs containers for testing, and pushes images to registries. Docker agents can be used for isolated build environments.

---

### 2️⃣0️⃣ How does Jenkins integrate with Kubernetes?

Jenkins dynamically provisions Kubernetes pods as agents, allowing scalable and containerized CI/CD execution.

---

### 2️⃣1️⃣ What is a Multibranch Pipeline?

It automatically creates pipelines for each branch in a repository, enabling separate CI/CD flows for feature, develop, and main branches.

---

### 2️⃣2️⃣ How do you handle failures in Jenkins pipelines?

Failures are handled using try-catch blocks, post conditions, notifications, retries, and rollback strategies.

---

### 2️⃣3️⃣ What is a post block in Jenkins?

The post block defines actions after pipeline execution, such as sending notifications, cleaning workspaces, or archiving artifacts.

---

### 2️⃣4️⃣ What are Jenkins artifacts?

Artifacts are files generated during a build (e.g., JAR, WAR, logs) that are stored and shared between pipeline stages.

---

### 2️⃣5️⃣ How do you schedule jobs in Jenkins?

Jobs can be scheduled using cron expressions to run at specific times or intervals.

---

### 2️⃣6️⃣ What is Jenkins master-agent architecture?

The master manages job scheduling and UI, while agents handle execution. This improves performance and scalability.

---

### 2️⃣7️⃣ How do you backup Jenkins?

Backup includes Jenkins home directory, job configurations, plugins, credentials, and build history.

---

### 2️⃣8️⃣ What is Jenkins workspace?

The workspace is a directory on the agent where Jenkins checks out code and executes build steps.

---

### 2️⃣9️⃣ How do you integrate Jenkins with AWS?

Jenkins integrates with AWS using IAM roles, EC2 agents, S3 for artifacts, ECR for Docker images, and CodeDeploy or EKS for deployments.

---

### 3️⃣0️⃣ Why is Jenkins preferred in DevOps?

Jenkins is open-source, highly customizable, plugin-rich, and widely adopted, making it ideal for automating CI/CD workflows.

---

## 🔹 Jenkins Interview Questions & Answers (31–50)

### 3️⃣1️⃣ What is Jenkins shared library?

A Jenkins Shared Library is reusable pipeline code stored in a separate repository. It helps standardize pipelines across multiple projects and reduces duplication.

---

### 3️⃣2️⃣ How do you create a shared library in Jenkins?

Create a Git repository with a vars/ or src/ directory, configure it in Jenkins under Global Pipeline Libraries, and import it using @Library() in Jenkinsfile.

---

### 3️⃣3️⃣ What is Jenkinsfile versioning?

Since Jenkinsfile is stored in source control, every pipeline change is versioned, enabling rollback, auditing, and consistent CI/CD behavior.

---

### 3️⃣4️⃣ What is the agent directive in Jenkins?

The agent directive defines where the pipeline or stage runs, such as any, none, label, docker, or kubernetes.

---

### 3️⃣5️⃣ What is parallel execution in Jenkins?

Parallel execution allows multiple stages or steps to run simultaneously, reducing pipeline execution time.

---

### 3️⃣6️⃣ How do you run jobs conditionally in Jenkins?

Conditions are applied using when blocks, environment checks, branch names, or expressions to control stage execution.

---

### 3️⃣7️⃣ What is the purpose of the options block?

The options block configures pipeline behavior, such as timeouts, retries, build discarding, and timestamps.

---

### 3️⃣8️⃣ What is build discarder in Jenkins?

Build discarder automatically deletes old builds to save disk space based on retention policies like number of builds or age.

---

### 3️⃣9️⃣ How do you integrate Jenkins with GitHub?

Integration is done using GitHub plugins, webhooks, personal access tokens, and Jenkins credentials for secure access.

---

### 4️⃣0️⃣ What is Jenkins Groovy?

Groovy is the scripting language used in Scripted pipelines and Jenkins internal logic, enabling advanced automation.

---

### 4️⃣1️⃣ What is the difference between stage and steps?

A stage defines a phase of the pipeline, while steps are the actual commands executed inside a stage.

---

### 4️⃣2️⃣ What is Jenkins rollback strategy?

Rollback reverts to the previous stable version when deployment fails, often implemented using artifacts, Git tags, or versioned Docker images.

---

### 4️⃣3️⃣ How do you send notifications from Jenkins?

Notifications are sent using plugins like Email, Slack, MS Teams, or webhooks configured in pipeline post blocks.

---

### 4️⃣4️⃣ What is Jenkins quiet period?

The quiet period delays job execution after a trigger, allowing multiple code commits to be grouped into a single build.

---

### 4️⃣5️⃣ How do you manage Jenkins secrets securely?

Secrets are stored in Jenkins Credentials Store and injected at runtime using environment variables or credential bindings.

---

### 4️⃣6️⃣ What is Jenkins throttling?

Throttling limits the number of concurrent builds to avoid resource exhaustion using the Throttle Concurrent Builds plugin.

---

### 4️⃣7️⃣ How do you clean up workspace in Jenkins?

Workspace cleanup is done using cleanWs() or post-build actions to remove files after execution.

---

### 4️⃣8️⃣ What is Jenkins build number?

The build number is a unique identifier for each job execution, commonly used for versioning artifacts.

---

### 4️⃣9️⃣ How do you optimize Jenkins performance?

Optimization includes using agents, reducing plugins, archiving logs, cleaning workspaces, and tuning JVM settings.

---

### 5️⃣0️⃣ How do you migrate Jenkins to a new server?

Migration involves backing up Jenkins home, restoring it on a new server, reinstalling plugins, and validating jobs.

---

✅ **Use this README as a complete Jenkins interview preparation guide.**
