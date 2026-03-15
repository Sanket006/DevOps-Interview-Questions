# 🎯 Cross-Questions for 2-DevOps (Same Level) Setup

---

## 1️⃣ “If there were only two DevOps engineers, how did you divide the work?”

### Why they ask:

To check clarity and realism.

### Safe Answer:

“We divided responsibilities based on focus areas.

One DevOps engineer mainly handled CI/CD pipelines and automation, while I focused on Dockerization, Kubernetes deployments, monitoring, and release support.

We reviewed each other’s work before deployments.”

---

## 2️⃣ “Who took final decisions without a senior DevOps?”

### Why they ask:

Risk & governance check.

### Safe Answer:

“Final decisions followed defined standards and documented processes.

For critical changes, we consulted the project manager or senior backend architect before proceeding.”

✔️ This keeps you safe.

---

## 3️⃣ “Did you have full production access?”

### Why they ask:

Security maturity test.

### Correct Answer:

“No. Production access was restricted.

Deployments were done through CI/CD pipelines, and manual changes were not allowed.

We mainly had access to monitoring and logs.”

---

## 4️⃣ “What if both of you were unavailable during an incident?”

### Why they ask:

Disaster preparedness.

### Safe Answer:

“We maintained documentation and runbooks.

Basic actions like rollbacks and restarts were automated through pipelines.

For critical issues, escalation paths were defined.”

---

## 5️⃣ “Who handled Terraform and AWS infra?”

### Why they ask:

To see if you over-claim senior skills.

### Safe Answer:

“Infrastructure was already defined using Terraform.

We mainly worked on maintaining and applying changes after peer review, not designing architecture from scratch.”

---

## 6️⃣ “How did you avoid mistakes without a senior review?”

### Why they ask:

Risk control.

### Safe Answer:

“Through peer reviews, testing in lower environments, automated pipeline checks, and staged deployments.”

---

## 7️⃣ “If your teammate made a mistake, how was it handled?”

### Why they ask:

Team maturity.

### Safe Answer:

“We treated it as a learning issue.

The problem was fixed, documented, and preventive checks were added to the pipeline.”

---

## 8️⃣ “What was the most complex task YOU handled personally?”

### Why they ask:

Depth check.

### Strong Answer:

“Handling Kubernetes deployments across multiple environments and troubleshooting pod failures using logs, metrics, and deployment history.”

---

## 9️⃣ “Did you ever bypass the pipeline to fix production quickly?”

### Why they ask:

Trap question 🚨

### Correct Answer:

“No. Even urgent fixes followed the pipeline and approval process to avoid further risk.”

---

## 🔟 “Why should we trust a 2-DevOps setup in production?”

### Why they ask:

Final confidence test.

### Perfect Answer:

“Because the system relied on automation, peer reviews, Infrastructure as Code, and controlled deployments instead of manual actions.”

---

# 🧠 Golden Rule for 2-DevOps Setup

No hierarchy ≠ No control
Control comes from process + automation + peer review
