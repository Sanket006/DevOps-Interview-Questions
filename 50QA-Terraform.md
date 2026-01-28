# Terraform Interview Questions & Answers (DevOps-Focused)

This README contains **50 Terraform interview questions with medium-length, interview-ready answers**, focused on **real-world DevOps usage**. The content is structured for quick revision, interview preparation, and practical understanding.

---

## 1. What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define, provision, and manage infrastructure using declarative configuration files. It supports multiple cloud providers and services, enabling consistent and automated infrastructure deployment.

---

## 2. How does Terraform work?

Terraform reads configuration files, creates an execution plan, and then applies the changes to reach the desired state. It compares the desired state defined in code with the current infrastructure state stored in the state file.

---

## 3. What is Infrastructure as Code (IaC)?

IaC is the practice of managing infrastructure using code instead of manual processes. It ensures consistency, repeatability, version control, and faster provisioning across environments.

---

## 4. What is a Terraform provider?

A provider is a plugin that allows Terraform to interact with APIs of cloud platforms or services like AWS, Azure, GCP, Kubernetes, or GitHub. Providers define the resources Terraform can manage.

---

## 5. What is a Terraform resource?

A resource represents a piece of infrastructure such as an EC2 instance, S3 bucket, or VPC. Resources are declared in `.tf` files and managed by Terraform.

---

## 6. What is the Terraform state file?

The state file (`terraform.tfstate`) tracks the real infrastructure Terraform manages. It maps resources defined in code to actual resources in the cloud and helps Terraform detect changes.

---

## 7. Why is the state file important?

Without the state file, Terraform cannot determine what already exists, leading to resource duplication or deletion. It enables dependency tracking and efficient updates.

---

## 8. What is remote state in Terraform?

Remote state stores the state file in a centralized backend like S3, Terraform Cloud, or Azure Blob Storage. It enables team collaboration and state locking.

---

## 9. What is a backend in Terraform?

A backend defines where Terraform stores the state file and how state locking is handled. Common backends include S3, GCS, Azure Blob, and Terraform Cloud.

---

## 10. What is terraform init?

`terraform init` initializes a Terraform project by downloading required providers, configuring backends, and preparing the working directory.

---

## 11. What is terraform plan?

`terraform plan` shows the changes Terraform will make before applying them. It helps review and validate infrastructure changes safely.

---

## 12. What is terraform apply?

`terraform apply` executes the planned changes and provisions or updates the infrastructure to match the configuration.

---

## 13. What is terraform destroy?

`terraform destroy` removes all resources managed by Terraform in a configuration. It is useful for cleaning up environments.

---

## 14. What are Terraform variables?

Variables allow dynamic and reusable configurations. They can be defined in `variables.tf`, passed via CLI, environment variables, or `.tfvars` files.

---

## 15. What is a Terraform output?

Outputs expose useful information such as instance IP addresses or resource IDs after provisioning. They help in integration with other tools.

---

## 16. What are Terraform modules?

Modules are reusable, self-contained Terraform configurations. They help organize code, reduce duplication, and improve maintainability.

---

## 17. What is the difference between count and for_each?

`count` creates multiple identical resources using numeric indexing, while `for_each` creates resources based on map or set keys, making resource tracking more predictable.

---

## 18. What is a data source in Terraform?

A data source allows Terraform to fetch and use information from existing resources without managing them directly.

---

## 19. How does Terraform handle dependencies?

Terraform automatically determines resource dependencies based on references. Explicit dependencies can be defined using `depends_on`.

---

## 20. What is terraform refresh?

`terraform refresh` updates the state file with the current real infrastructure values without changing resources.

---

## 21. What is state locking?

State locking prevents multiple users from modifying the same state simultaneously, avoiding conflicts and corruption.

---

## 22. What are workspaces in Terraform?

Workspaces allow managing multiple environments (like dev, test, prod) using the same configuration with separate state files.

---

## 23. What is terraform taint?

`terraform taint` marks a resource as damaged so Terraform recreates it during the next apply.

---

## 24. What is terraform import?

`terraform import` brings existing infrastructure under Terraform management without recreating resources.

---

## 25. What is a provisioner in Terraform?

Provisioners execute scripts or commands on resources after creation. They are discouraged for regular use and recommended only as a last resort.

---

## 26. What is the difference between locals and variables?

Variables accept external input, while locals define reusable expressions within the configuration to simplify code.

---

## 27. How do you secure sensitive data in Terraform?

Sensitive data can be protected using environment variables, secret managers, encrypted backends, and `sensitive = true` outputs.

---

## 28. What is the difference between terraform apply and terraform apply -auto-approve?

`terraform apply` requires manual approval, while `-auto-approve` skips confirmation and is commonly used in CI/CD pipelines.

---

## 29. How do you manage multiple environments in Terraform?

Multiple environments can be managed using workspaces, separate directories, or separate state backends for each environment.

---

## 30. What are some Terraform best practices?

Use remote state, version control, modular design, meaningful naming, state locking, and CI/CD integration to ensure scalable and maintainable infrastructure.

---

## 31. What is the Terraform dependency graph?

Terraform builds a dependency graph to understand the order in which resources should be created, updated, or destroyed. This graph is generated automatically based on resource references and ensures resources are provisioned in the correct sequence.

---

## 32. What is depends_on and when should you use it?

`depends_on` explicitly defines resource dependencies when Terraform cannot infer them automatically, such as with external scripts or implicit relationships not referenced in arguments.

---

## 33. What are lifecycle rules in Terraform?

Lifecycle rules control resource behavior, such as `create_before_destroy`, `prevent_destroy`, and `ignore_changes`, helping manage safe updates and avoid accidental deletions.

---

## 34. What is ignore_changes used for?

`ignore_changes` tells Terraform to ignore specific attribute changes, often used when values are modified outside Terraform or by autoscaling services.

---

## 35. What is create_before_destroy?

This lifecycle rule ensures a new resource is created before the old one is destroyed, minimizing downtime during updates.

---

## 36. What is prevent_destroy?

`prevent_destroy` protects critical resources from accidental deletion by blocking destroy actions unless explicitly removed.

---

## 37. What is Terraform Cloud?

Terraform Cloud is a managed service that provides remote state management, secure variable storage, policy enforcement, and CI/CD-style Terraform runs.

---

## 38. What is Sentinel in Terraform?

Sentinel is a policy-as-code framework used with Terraform Cloud and Enterprise to enforce governance rules, such as restricting instance types or regions.

---

## 39. What is the .terraform directory?

The `.terraform` directory stores downloaded provider plugins, module caches, and backend configuration details required for Terraform operations.

---

## 40. What is provider version locking?

Provider version locking ensures consistent behavior across environments by specifying provider versions using the `required_providers` block.

---

## 41. What is the required_version setting?

`required_version` restricts which Terraform CLI versions can be used, preventing incompatibility issues across teams.

---

## 42. What is the difference between terraform fmt and terraform validate?

`terraform fmt` formats configuration files, while `terraform validate` checks syntax and internal consistency without accessing cloud resources.

---

## 43. What is terraform graph?

`terraform graph` generates a visual representation of the dependency graph, useful for understanding complex infrastructures.

---

## 44. How does Terraform handle resource drift?

Terraform detects drift by comparing the real infrastructure with the state file during plan or apply and shows the differences.

---

## 45. What is terraform state list?

`terraform state list` displays all resources tracked in the current state file.

---

## 46. What is terraform state rm?

`terraform state rm` removes a resource from the state file without deleting the actual resource, often used for manual state cleanup.

---

## 47. What is terraform state mv?

`terraform state mv` moves resources within the state file, useful when refactoring code or moving resources into modules.

---

## 48. What are dynamic blocks in Terraform?

Dynamic blocks allow conditional or repeated nested blocks using loops, making configurations more flexible and DRY.

---

## 49. What is the difference between null_resource and local-exec?

`null_resource` acts as a placeholder resource, while `local-exec` is a provisioner that runs commands on the local machine.

---

## 50. How do you integrate Terraform with CI/CD pipelines?

Terraform is integrated into CI/CD pipelines by running `init`, `plan`, and `apply` stages with remote state, version control, approvals, and secrets management.

---

### 📌 Usage

* Ideal for **DevOps interviews** (Junior to Mid-level)
* Can be used as **revision notes**

---