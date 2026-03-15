# AWS Interview Questions – DevOps / Cloud Engineer (Fresher–Mid Level)

This README contains **50 AWS interview questions with clear, medium-length, interview-ready answers**, focused on real-world relevance for **DevOps and Cloud Engineer roles (fresher to mid-level)**.

---

## Part 1: Core AWS Interview Questions (1–30)

### 1. What is AWS and why is it popular?

AWS (Amazon Web Services) is a cloud computing platform that provides on-demand services like compute, storage, networking, databases, and security. It is popular because it offers scalability, pay-as-you-go pricing, high availability, global infrastructure, and a wide range of managed services.

### 2. What is the difference between EC2 and Lambda?

EC2 provides virtual servers where you manage the OS and runtime, while Lambda is a serverless service where AWS manages everything and you only run code. EC2 is best for long-running applications, whereas Lambda is ideal for event-driven and short-duration tasks.

### 3. What is an AMI?

An AMI (Amazon Machine Image) is a template used to launch EC2 instances. It contains the operating system, application software, and configurations. Using AMIs helps create consistent environments quickly and supports scaling and automation.

### 4. What is Auto Scaling?

Auto Scaling automatically adjusts the number of EC2 instances based on demand. It improves availability and cost efficiency by scaling out during high traffic and scaling in during low traffic using defined policies.

### 5. What is an Application Load Balancer?

An Application Load Balancer (ALB) distributes incoming HTTP/HTTPS traffic across multiple targets like EC2 instances or containers. It works at Layer 7 and supports features like path-based routing, host-based routing, and SSL termination.

### 6. What is the difference between ALB and NLB?

ALB operates at Layer 7 and is suitable for web applications, while NLB works at Layer 4 and is designed for high-performance, low-latency traffic. NLB handles millions of requests per second and supports TCP/UDP traffic.

### 7. What is Amazon S3?

Amazon S3 is an object storage service used to store and retrieve any amount of data. It offers high durability (99.999999999%), scalability, and security, making it suitable for backups, static websites, and data lakes.

### 8. What is the difference between S3 and EBS?

S3 is object storage accessed via APIs, while EBS is block storage attached to EC2 instances. EBS is used like a hard disk for OS and databases, whereas S3 is used for storing files and objects.

### 9. What is IAM?

IAM (Identity and Access Management) allows you to manage users, roles, and permissions in AWS. It follows the principle of least privilege and uses policies to control access to AWS resources securely.

### 10. What is the difference between IAM user and IAM role?

An IAM user represents a person or service with long-term credentials, while an IAM role provides temporary permissions and is assumed by AWS services or users. Roles are more secure and preferred for EC2 or Lambda access.

### 11. What is VPC?

A VPC (Virtual Private Cloud) is a logically isolated network in AWS where you can launch resources. It allows you to define IP ranges, subnets, route tables, and security rules for better control and security.

### 12. What is the difference between public and private subnet?

A public subnet has a route to an Internet Gateway, allowing internet access. A private subnet does not have direct internet access and is typically used for databases or backend services.

### 13. What is a Security Group?

A Security Group acts as a virtual firewall for EC2 instances. It controls inbound and outbound traffic using allow rules only and works at the instance level.

### 14. What is a Network ACL?

A Network ACL controls traffic at the subnet level. It supports both allow and deny rules and is stateless, meaning inbound and outbound rules are evaluated separately.

### 15. What is the difference between Security Group and NACL?

Security Groups are stateful and attached to instances, while NACLs are stateless and attached to subnets. Security Groups are more commonly used for fine-grained access control.

### 16. What is Amazon RDS?

Amazon RDS is a managed relational database service that supports engines like MySQL, PostgreSQL, and Oracle. It handles backups, patching, and replication, reducing administrative overhead.

### 17. What is Multi-AZ in RDS?

Multi-AZ creates a standby replica of the database in another availability zone. If the primary fails, AWS automatically switches to the standby, ensuring high availability.

### 18. What is CloudWatch?

Amazon CloudWatch is a monitoring service that collects metrics, logs, and events. It helps track performance, set alarms, and automate responses using alerts and triggers.

### 19. What is CloudTrail?

CloudTrail records API calls made in your AWS account. It is mainly used for auditing, security analysis, and compliance tracking.

### 20. What is the difference between CloudWatch and CloudTrail?

CloudWatch monitors performance and resource metrics, while CloudTrail tracks user activity and API calls. CloudWatch focuses on “how resources perform,” and CloudTrail focuses on “who did what.”

### 21. What is Elastic Beanstalk?

Elastic Beanstalk is a Platform-as-a-Service that deploys and manages applications automatically. Developers upload code, and AWS handles infrastructure, scaling, and monitoring.

### 22. What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code in response to events. You are charged only for execution time, and it automatically scales without server management.

### 23. What is Amazon EKS?

Amazon EKS is a managed Kubernetes service that simplifies running Kubernetes clusters on AWS. AWS manages the control plane, while users manage worker nodes and workloads.

### 24. What is Infrastructure as Code (IaC)?

IaC is the practice of managing infrastructure using code and configuration files. Tools like AWS CloudFormation and Terraform allow version control, automation, and repeatable deployments.

### 25. What is AWS CloudFormation?

CloudFormation is an IaC service that uses templates to provision AWS resources automatically. It helps maintain consistency, reduce manual errors, and support automation.

### 26. What is the shared responsibility model?

AWS is responsible for the security of the cloud (infrastructure), while customers are responsible for security in the cloud (data, applications, configurations). This model clearly divides security responsibilities.

### 27. What is an Availability Zone?

An Availability Zone is a physically separate data center within an AWS region. Using multiple AZs increases fault tolerance and availability.

### 28. What is the difference between Region and AZ?

A region is a geographical area containing multiple availability zones. AZs are isolated locations within a region designed to prevent failures from spreading.

### 29. What is AWS Cost Optimization?

Cost optimization involves choosing the right instance types, using Auto Scaling, reserved instances, and monitoring usage. It ensures efficient resource usage and reduced cloud spending.

### 30. How does AWS help in high availability?

AWS provides multiple AZs, load balancers, Auto Scaling, and managed services like RDS Multi-AZ. These features ensure applications remain available even during failures.

---

## Part 2: Additional AWS Interview Questions (31–50)

### 31. What is an Internet Gateway?

An Internet Gateway (IGW) allows communication between resources in a VPC and the internet. It supports inbound and outbound traffic for public subnets and enables instances with public IPs to access the internet.

### 32. What is a NAT Gateway?

A NAT Gateway allows instances in a private subnet to access the internet for updates or downloads while preventing inbound internet traffic. It improves security by keeping backend resources private.

### 33. What is Route Table?

A route table contains rules that determine how network traffic is directed within a VPC. Each subnet must be associated with a route table to control where traffic is routed.

### 34. What is an Elastic IP?

An Elastic IP is a static, public IPv4 address that can be attached to an EC2 instance. It is useful when you need a fixed IP address even after stopping or restarting instances.

### 35. What is the difference between Stateful and Stateless services?

Stateful services remember previous requests, while stateless services do not. Security Groups are stateful, meaning return traffic is automatically allowed, whereas NACLs are stateless.

### 36. What is AWS CLI?

AWS CLI is a command-line tool used to manage AWS services using commands. It enables automation, scripting, and faster operations compared to using the AWS Management Console.

### 37. What is an AWS SDK?

AWS SDK provides language-specific libraries that allow developers to interact with AWS services programmatically. SDKs simplify API usage in applications written in languages like Python, Java, and JavaScript.

### 38. What is Amazon SNS?

Amazon SNS is a messaging service used for sending notifications or messages to multiple subscribers. It supports email, SMS, Lambda, and HTTP endpoints for event-driven communication.

### 39. What is Amazon SQS?

Amazon SQS is a fully managed message queue service that decouples application components. It improves reliability and scalability by allowing services to communicate asynchronously.

### 40. What is the difference between SNS and SQS?

SNS is a pub/sub service that pushes messages to subscribers, while SQS is a queue service that pulls messages. SNS is used for fan-out notifications, and SQS is used for buffering and decoupling systems.

### 41. What is Amazon CloudFront?

CloudFront is a Content Delivery Network (CDN) that delivers content with low latency using edge locations. It improves performance and security for web applications.

### 42. What is AWS WAF?

AWS WAF is a web application firewall that protects applications from common attacks like SQL injection and cross-site scripting. It works with CloudFront and ALB.

### 43. What is AWS Shield?

AWS Shield provides protection against DDoS attacks. Shield Standard is enabled by default, while Shield Advanced offers enhanced protection and cost safeguards.

### 44. What is Amazon ECR?

Amazon Elastic Container Registry (ECR) is a managed Docker container registry. It securely stores, manages, and deploys container images for ECS and EKS.

### 45. What is Amazon ECS?

Amazon ECS is a container orchestration service that manages Docker containers. It supports both EC2 and serverless Fargate launch types.

### 46. What is AWS Fargate?

AWS Fargate is a serverless compute engine for containers. It removes the need to manage EC2 instances and automatically scales container workloads.

### 47. What is Amazon VPC Peering?

VPC Peering allows communication between two VPCs using private IPs. It is useful for connecting services across different VPCs securely.

### 48. What is AWS Direct Connect?

AWS Direct Connect provides a dedicated network connection between on-premises infrastructure and AWS. It offers lower latency and more consistent performance than internet-based connections.

### 49. What is AWS Backup?

AWS Backup is a centralized service that automates backups across AWS services like EBS, RDS, and DynamoDB. It helps with compliance and disaster recovery.

### 50. What is AWS Organizations?

AWS Organizations allows you to manage multiple AWS accounts centrally. It helps with consolidated billing, security policies, and governance.

---

✅ **Use this README for interview revision, GitHub portfolio documentation, or DevOps interview preparation.**