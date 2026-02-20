# AWS Troubleshooting Interview Questions – Fresher-Friendly DevOps Guide

This README contains **commonly asked and scenario-based AWS troubleshooting interview questions** with **easy, practical, medium-length answers**, written in a **DevOps fresher interview style**.

The focus is on **real-world troubleshooting thinking**, not just theory.

---

## 📘 Part 1: 50 Commonly Asked AWS Troubleshooting Interview Questions

### 1. An EC2 instance is running but you cannot connect via SSH. What do you check?

First, I check the security group to ensure port 22 is open for my IP.

Then I verify the key pair, instance state, and public IP.

I also check network ACLs, route table, and whether the instance is in a public subnet.

---

### 2. Your EC2 instance is unreachable after reboot. What could be the reason?

The public IP may have changed if it’s not an Elastic IP.

The instance may be stuck in initializing state.

I also check if the root volume is attached and instance status checks.

---

### 3. EC2 CPU usage is very high. How do you troubleshoot?

I check CloudWatch metrics for CPU spikes.

Then I log in and use commands like `top` or `htop`.

I identify heavy processes and decide whether to optimize, scale, or upgrade the instance.

---

### 4. An EC2 instance failed to launch. What are possible causes?

It could be due to instance limit reached, invalid AMI, or insufficient capacity.

Sometimes the selected instance type is not supported in that AZ.

---

### 5. How do you recover a failed EC2 instance?

I stop the instance and check system logs.

If needed, I detach the root volume and attach it to another instance to fix issues.

Then I reattach and start the instance.

---

### 6. Your application is not accessible from the browser but works locally. What do you check?

I check security group inbound rules (port 80/443).

Then verify EC2 public IP, web server status, and firewall rules.

---

### 7. What causes EC2 status check failure?

It could be system failure, network issue, or OS crash.

System check failure is AWS-side, instance check failure is OS-level.

---

### 8. Your EC2 disk is full. What will you do?

I check disk usage using `df -h`.

Then clean logs or extend the EBS volume and resize the filesystem.

---

### 9. An EBS volume is not attaching to EC2. Why?

The volume and instance must be in the same availability zone.

Also, the instance must be in a stopped or running state.

---

### 10. EBS volume performance is slow. How do you troubleshoot?

I check volume type (gp2, gp3, io1).

Then review IOPS limits and CloudWatch metrics.

---

### 11. Your S3 bucket is not accessible publicly. What could be wrong?

The block public access setting might be enabled.

The bucket policy or object ACL might be incorrect.

---

### 12. You uploaded a file to S3 but cannot access it. Why?

The object may not have public read permission.

Also, the bucket policy may deny access.

---

### 13. How do you troubleshoot slow S3 uploads?

I check network bandwidth and file size.

Using multipart upload helps for large files.

---

### 14. An S3 static website is not loading. What do you check?

I verify index.html configuration, bucket policy, and static hosting enabled.

---

### 15. IAM user cannot access AWS resources. What do you check?

I check attached IAM policies, permissions, and explicit deny rules.

Also confirm the correct AWS region.

---

### 16. What is the most common IAM mistake?

Giving too many permissions or missing required permissions.

Also forgetting MFA or policy attachment.

---

### 17. Your Lambda function is failing. How do you debug?

I check CloudWatch logs for errors.

Then verify memory, timeout, and IAM role permissions.

---

### 18. Lambda function times out. What do you do?

I increase the timeout value.

I also optimize code or increase memory allocation.

---

### 19. API Gateway returns 500 error. What could be wrong?

The backend Lambda may be failing.

I check Lambda logs and API Gateway integration settings.

---

### 20. RDS instance is not accessible. What do you check?

I check security groups, subnet type, and public accessibility.

Also verify database endpoint and port.

---

### 21. RDS storage is full. What happens?

The database may stop accepting writes.

I increase storage or clean unused data.

---

### 22. RDS backup failed. Why?

Backup window conflict or insufficient storage.

I check backup settings and CloudWatch logs.

---

### 23. Your application cannot connect to RDS. Why?

Wrong username/password, security group issues, or incorrect endpoint.

---

### 24. High latency in RDS. How do you fix it?

I check CPU, memory, and IOPS.

I may scale instance or add read replicas.

---

### 25. ELB health checks are failing. What do you do?

I check target instance health, security groups, and health check path.

---

### 26. Load balancer shows targets as unhealthy. Why?

The application may not respond on the expected port or path.

---

### 27. Auto Scaling is not launching new instances. Why?

Scaling policy may be incorrect.

Instance limits or launch template issues could also be the reason.

---

### 28. Auto Scaling terminated a healthy instance. Why?

It follows scaling policies, not health status alone.

---

### 29. CloudWatch alarm is not triggering. Why?

Metric, threshold, or evaluation period may be wrong.

---

### 30. CloudWatch logs are missing. What do you check?

I check IAM role permissions and log group configuration.

---

### 31. VPC resources cannot access the internet. Why?

Missing Internet Gateway, route table entry, or NAT Gateway.

---

### 32. Private EC2 cannot access internet. What is required?

A NAT Gateway or NAT instance in a public subnet.

---

### 33. Two EC2 instances cannot communicate. Why?

They may be in different security groups with blocked inbound rules.

---

### 34. Route table is not working as expected. Why?

Incorrect route association or missing destination entries.

---

### 35. DNS is not resolving in EC2. What do you check?

I check VPC DNS settings and `/etc/resolv.conf`.

---

### 36. CloudFront is not serving updated content. Why?

Cache is enabled.

I invalidate the cache or wait for TTL expiry.

---

### 37. CloudFront returns access denied. Why?

S3 bucket policy may not allow CloudFront access.

---

### 38. Your AWS bill suddenly increased. How do you troubleshoot?

I check Cost Explorer and billing dashboard.

Then identify unused resources.

---

### 39. Which AWS service helps with cost troubleshooting?

AWS Cost Explorer and Budgets.

---

### 40. Elastic IP not working. Why?

It may not be associated with a running instance.

---

### 41. AMI creation failed. What could be wrong?

Instance may be stopped improperly or storage issue.

---

### 42. Snapshot is taking too long. Why?

Large data size or high disk activity.

---

### 43. SQS messages are not consumed. Why?

Consumer may be down or IAM permission issue.

---

### 44. SNS notifications are not received. Why?

Subscription not confirmed or incorrect endpoint.

---

### 45. Terraform deployment failed in AWS. What do you check?

I check error logs, IAM permissions, and resource limits.

---

### 46. ECS task stopped unexpectedly. Why?

Insufficient memory, CPU, or application crash.

---

### 47. EKS pod is in pending state. Why?

Insufficient cluster resources or node issues.

---

### 48. EKS service not accessible. Why?

Service type, security groups, or load balancer issue.

---

### 49. AWS console is slow or not loading. Why?

Browser issue, region outage, or network problem.

---

### 50. What is your general AWS troubleshooting approach?

Check logs, metrics, permissions, network, and configuration step by step.

Always isolate the problem before fixing it.

---

## 📙 Part 2: Scenario-Based AWS Troubleshooting Interview Questions

### 1. Your EC2 instance is running but SSH connection times out. How do you troubleshoot?

I first check if port 22 is open in the security group for my IP.

Then I verify the instance has a public IP and is in a public subnet with an Internet Gateway.

I also check network ACLs, correct key pair, and instance status checks.

---

### 2. After rebooting an EC2 instance, your application is down. What could be the issue?

The application service may not be configured to start on boot.

I log in and check service status using `systemctl status`.

I enable auto-start using `systemctl enable <service>`.

---

### 3. Your EC2 instance suddenly becomes unreachable. What steps do you follow?

I check the instance state and system status checks.

Then I verify security groups, route tables, and public IP.

If needed, I review system logs or attach the root volume to another instance for debugging.

---

### 4. Your website is not accessible via browser but works inside EC2. Why?

This usually happens due to security group rules.

I check if port 80 or 443 is allowed from the internet.

I also verify firewall rules and ensure the web server is running.

---

### 5. EC2 CPU usage is 100% and application is slow. What will you do?

I check CloudWatch metrics and log in to run `top`.

I identify heavy processes.

Based on the issue, I optimize the app or scale using Auto Scaling or upgrade instance type.

---

### 6. Disk is full on EC2 and application stopped working. How do you fix it?

I check disk usage using `df -h`.

I clean unnecessary files or logs.

If needed, I increase the EBS volume size and extend the filesystem.

---

### 7. Your EC2 instance failed status checks. What does it mean?

If system check fails, it is an AWS hardware issue.

If instance check fails, it is an OS-level problem.

I try rebooting or recovering the instance.

---

### 8. An EC2 instance cannot access the internet. Why?

I check if the instance is in a public subnet.

The route table must have a route to Internet Gateway.

Security groups and NACLs must allow outbound traffic.

---

### 9. Private EC2 instance needs internet access. What do you do?

I configure a NAT Gateway in a public subnet.

Then update the private subnet route table to use NAT.

---

### 10. Two EC2 instances in the same VPC cannot communicate. Why?

Security group rules may block inbound traffic.

I allow traffic between the instances’ security groups.

I also verify NACL rules.

---

### 11. S3 bucket is public but still access denied. Why?

The Block Public Access setting might be enabled.

Bucket policy or object ACL might be missing permissions.

---

### 12. Your S3 static website is not loading. How do you troubleshoot?

I check if static website hosting is enabled.

I verify index document configuration and bucket policy.

---

### 13. CloudFront is showing old content. What do you do?

CloudFront caches content using TTL.

I perform a cache invalidation or wait for TTL expiry.

---

### 14. CloudFront returns Access Denied error. Why?

S3 bucket policy may not allow CloudFront access.

I check Origin Access Control or bucket permissions.

---

### 15. RDS instance is not reachable from EC2. What could be wrong?

Security group may not allow DB port (3306/5432).

RDS might be in private subnet or wrong endpoint used.

---

### 16. Application cannot connect to RDS even though DB is running.

I verify username/password, endpoint, and port.

I check if EC2 security group is allowed in RDS security group.

---

### 17. RDS CPU usage is high and queries are slow.

I check CloudWatch metrics and slow query logs.

I optimize queries or scale the instance.

Read replicas can reduce load.

---

### 18. RDS storage is full. What happens and how to fix?

Writes may fail.

I increase allocated storage or delete old data.

---

### 19. Lambda function fails during execution. How do you debug?

I check CloudWatch logs.

I verify IAM role permissions, memory, and timeout.

---

### 20. Lambda function is timing out. What is your solution?

I increase timeout value.

I optimize code or increase memory.

---

### 21. API Gateway returns 500 error.

Backend Lambda may be failing.

I check Lambda logs and API Gateway integration settings.

---

### 22. Load Balancer shows instances as unhealthy.

Health check path or port might be incorrect.

Application may not be running on expected port.

---

### 23. Application works on EC2 but not behind ELB.

Security groups may block ELB traffic.

Health check configuration may be incorrect.

---

### 24. Auto Scaling is not launching instances during high load.

Scaling policy may not be triggered.

Instance limits or launch template issues may exist.

---

### 25. Auto Scaling terminated a healthy instance. Why?

Auto Scaling follows policies, not instance health alone.

---

### 26. CloudWatch alarm is not triggering.

Metric or threshold may be wrong.

Evaluation period may be incorrect.

---

### 27. CloudWatch logs are missing from EC2.

IAM role may not have logging permission.

CloudWatch agent may not be running.

---

### 28. VPC peering connection exists but traffic is not flowing.

Route tables may not have peering routes.

Security groups and NACLs must allow traffic.

---

### 29. EBS volume attached but not visible in EC2.

Filesystem may not be mounted.

I use `lsblk` and mount the volume.

---

### 30. AWS bill suddenly increased. How do you investigate?

I use AWS Cost Explorer.

I identify unused EC2, EBS, snapshots, or data transfer costs.

---

### 31. IAM user cannot perform an action even with policy attached.

There may be an explicit deny.

Policy might be attached to wrong user or group.

---

### 32. ECS task stopped unexpectedly.

Insufficient CPU or memory.

Application crash or wrong container command.

---

### 33. EKS pod stuck in Pending state.

Insufficient cluster resources.

Node group scaling or resource requests need adjustment.

---

### 34. EKS service not accessible externally.

Service type may not be LoadBalancer.

Security group or subnet issue.

---

### 35. Terraform fails while creating AWS resources.

IAM permission issue or resource limit reached.

I check Terraform error logs.

---

### 36. Snapshot creation is slow.

Large volume size or high disk activity.

---

### 37. SQS messages are not consumed.

Consumer service may be down.

IAM permission issue.

---

### 38. SNS notifications not delivered.

Subscription not confirmed.

Incorrect endpoint.

---

### 39. Route table is correct but traffic still not flowing.

Security groups or NACLs may block traffic.

---

### 40. What is your overall AWS troubleshooting approach?

I follow a step-by-step approach:

Check logs → metrics → permissions → networking → configuration.

I isolate the problem before applying a fix.
