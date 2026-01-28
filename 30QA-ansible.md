# Ansible Interview Questions & Answers (DevOps Engineer – Fresher to Mid-level)

This README contains **30 Ansible interview questions with medium-length, interview-ready answers**, focused on **DevOps Engineer (fresher → mid-level)** roles.

---

## 1. What is Ansible and why is it used?

Ansible is an open-source automation tool used for configuration management, application deployment, and orchestration. It helps automate repetitive IT tasks such as server provisioning and software installation. Ansible is agentless and uses SSH, which makes it simple and easy to adopt.

---

## 2. How does Ansible differ from other configuration management tools?

Ansible is agentless, meaning no software needs to be installed on managed nodes. It uses YAML for playbooks, making it human-readable. Compared to tools like Puppet or Chef, Ansible has a lower learning curve and faster setup.

---

## 3. What is an Ansible playbook?

A playbook is a YAML file that defines a set of tasks to be executed on target hosts. It describes what needs to be done, not how to do it. Playbooks can include variables, roles, handlers, and conditional logic.

---

## 4. What is an Ansible inventory?

The inventory is a list of managed hosts or nodes. It can be a static file (INI or YAML) or dynamic (from AWS, Azure, etc.). Inventories can group hosts and define variables at host or group level.

---

## 5. What is the difference between a playbook and a role?

A playbook defines tasks and execution flow, while a role is a structured way of organizing tasks, variables, handlers, templates, and files. Roles improve reusability and maintainability of Ansible code.

---

## 6. What are Ansible modules?

Modules are reusable units of code that perform specific tasks, such as installing packages, copying files, or managing services. Examples include yum, apt, service, copy, and file. Playbooks use modules to execute actions.

---

## 7. What is idempotency in Ansible?

Idempotency means that running the same playbook multiple times produces the same result without making unnecessary changes. Ansible modules are designed to check the current state before making changes, ensuring consistency.

---

## 8. How does Ansible communicate with managed nodes?

Ansible uses SSH for Linux/Unix systems and WinRM for Windows systems. It does not require any agent on the managed nodes. Authentication is typically done using SSH keys.

---

## 9. What are Ansible variables?

Variables store values that can be reused in playbooks. They make playbooks flexible and dynamic. Variables can be defined in playbooks, inventory files, group_vars, host_vars, or passed at runtime.

---

## 10. What is group_vars and host_vars?

group_vars stores variables common to a group of hosts, while host_vars stores variables specific to individual hosts. This helps in managing environment-specific configurations cleanly.

---

## 11. What is an Ansible handler?

Handlers are special tasks that run only when notified by another task. They are commonly used for restarting services after configuration changes. Handlers run at the end of a play.

---

## 12. What is Ansible Vault?

Ansible Vault is used to encrypt sensitive data such as passwords, API keys, and certificates. It ensures secure storage of secrets in playbooks and variable files. Vault files can be decrypted at runtime.

---

## 13. What is a task in Ansible?

A task is a single action executed on a host using a module. Tasks are defined inside a playbook and executed sequentially. Each task should perform one specific operation.

---

## 14. What is the purpose of facts in Ansible?

Facts are system information collected from managed nodes, such as OS type, IP address, and memory details. They are gathered automatically and can be used in playbooks for conditional logic.

---

## 15. What is gather_facts?

gather_facts enables or disables automatic fact collection. It is enabled by default. Disabling it can improve playbook execution speed when facts are not required.

---

## 16. What is a template in Ansible?

Templates use the Jinja2 engine to create dynamic configuration files. They allow variables and logic inside files. Templates are commonly used for configuration files like Nginx or Apache configs.

---

## 17. What is the difference between copy and template modules?

The copy module transfers static files to managed nodes, while the template module processes Jinja2 templates before copying. Templates are used when dynamic content is required.

---

## 18. What are Ansible tags?

Tags allow selective execution of tasks or plays. They help run specific parts of a playbook without executing the entire file. Tags are useful for debugging and partial deployments.

---

## 19. What is a role directory structure?

A typical role includes directories like tasks, handlers, vars, defaults, templates, and files. This structure standardizes code organization and improves reusability.

---

## 20. What is dynamic inventory?

Dynamic inventory fetches host information from external sources like AWS, Azure, or GCP. It automatically updates the inventory based on current infrastructure. This is useful in cloud environments.

---

## 21. How do you handle multiple environments in Ansible?

Multiple environments are managed using separate inventory files, group_vars, or directories like dev, test, and prod. Variables are customized per environment to avoid duplication.

---

## 22. What is ansible.cfg?

ansible.cfg is the configuration file for Ansible. It defines default settings such as inventory location, SSH options, privilege escalation, and retries.

---

## 23. What is privilege escalation in Ansible?

Privilege escalation allows tasks to run with elevated permissions using become. It is commonly used to run tasks as root. become_user defines the target user.

---

## 24. What is the difference between vars and defaults in roles?

defaults have the lowest priority and can be easily overridden. vars have higher priority and override defaults. Defaults are preferred for configurable values.

---

## 25. What is Ansible Galaxy?

Ansible Galaxy is a repository for sharing Ansible roles. It helps reuse community-maintained roles and speeds up development. Roles can be installed using ansible-galaxy.

---

## 26. How does Ansible support error handling?

Ansible supports error handling using ignore_errors, failed_when, and block-rescue-always. These features allow custom failure conditions and recovery steps.

---

## 27. What is block in Ansible?

A block groups multiple tasks together. It is often used with rescue and always for exception handling. This improves readability and error control.

---

## 28. What is when condition in Ansible?

when is used to execute tasks conditionally based on variables or facts. It helps create logic-based playbooks that adapt to different environments.

---

## 29. How do you debug Ansible playbooks?

Debugging is done using the debug module, verbose mode (-v, -vvv), and check mode (--check). These tools help identify errors and unexpected behavior.

---

## 30. How is Ansible used in a DevOps pipeline?

Ansible is used for configuration management, deployment automation, and environment provisioning. It integrates with CI/CD tools like Jenkins and GitHub Actions to ensure consistent deployments across environments.

---
