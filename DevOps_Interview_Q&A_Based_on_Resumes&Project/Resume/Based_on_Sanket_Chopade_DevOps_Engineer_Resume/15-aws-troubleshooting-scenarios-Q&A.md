# AWS Troubleshooting Scenario

## 1. Scenario

An EC2 instance is running but the website is not accessible.

### Identify

Users report that the website hosted on an EC2 instance is not accessible even though the instance status shows as running.

### Diagnose

1. Check instance status and health checks.

```bash
aws ec2 describe-instance-status --instance-ids <instance-id>
```

2. Verify Security Group rules to ensure HTTP/HTTPS ports are open.

3. Check if the web server (Nginx/Apache) is running.

```bash
sudo systemctl status nginx
```

or

```bash
sudo systemctl status httpd
```

4. Verify that the application is listening on the correct port.

```bash
sudo netstat -tulnp
```

5. Check instance firewall rules.

```bash
sudo iptables -L
```

6. Check web server logs.

```bash
sudo tail -f /var/log/nginx/error.log
```

or

```bash
sudo tail -f /var/log/httpd/error_log
```

### Fix

* Open required ports (80/443) in the Security Group.
* Start or restart the web server if it is stopped.

```bash
sudo systemctl start nginx
```

or

```bash
sudo systemctl restart httpd
```

* Correct firewall rules if blocking traffic.

### Prevent

* Configure CloudWatch monitoring for instance and application health.
* Set up an Application Load Balancer health check.
* Implement uptime monitoring and alerting.

---

## 2. Scenario

SSH connection to EC2 fails.

### Identify

Users are unable to connect to the EC2 instance via SSH. The SSH connection times out or is refused.

### Diagnose

1. Check the instance state to ensure it is running.

```bash
aws ec2 describe-instances --instance-ids <instance-id>
```

2. Verify Security Group rules allow SSH (port 22).

Ensure inbound rule:

Type: SSH
Protocol: TCP
Port: 22
Source: Your IP address

3. Check Network ACL rules for the subnet.

Ensure port 22 is allowed.

4. Verify the public IP or Elastic IP used for connection.

5. Check SSH key permissions locally.

```bash
chmod 400 my-key.pem
```

6. Verify correct SSH command.

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

7. Check if SSH service is running on the instance (via EC2 Instance Connect or Session Manager).

```bash
sudo systemctl status sshd
```

### Fix

* Add SSH inbound rule in Security Group if missing.
* Correct the public IP used for SSH.
* Restart SSH service if it is not running.

```bash
sudo systemctl restart sshd
```

* Correct key permissions.

### Prevent

* Use AWS Systems Manager Session Manager to access instances without SSH.
* Restrict SSH access to specific IP ranges.
* Enable CloudWatch monitoring and alerts for instance connectivity issues.

---

## 3. Scenario

An application deployed on EC2 suddenly stops responding.

### Identify

Users report that the application hosted on the EC2 instance is not responding or returns timeouts/5xx errors. The EC2 instance itself may still appear in the "running" state.

### Diagnose

1. Check EC2 instance health and status checks.

```bash
aws ec2 describe-instance-status --instance-ids <instance-id>
```

2. Check CPU, memory, and network metrics in CloudWatch to see if the instance is overloaded.

3. SSH into the instance to inspect the system.

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

4. Check running processes to confirm the application process is active.

```bash
ps aux | grep <application-name>
```

5. Check if the application port is listening.

```bash
sudo netstat -tulnp | grep <port>
```

6. Check disk space in case the application stopped due to a full disk.

```bash
df -h
```

7. Check application logs for errors.

```bash
sudo tail -f /var/log/<application-log>.log
```

8. Check system logs.

```bash
sudo journalctl -xe
```

### Fix

* Restart the application service.

```bash
sudo systemctl restart <application-service>
```

* If CPU or memory is exhausted, restart the instance or scale the instance type.

* Clear disk space if storage is full.

* Fix application errors identified in logs.

### Prevent

* Configure CloudWatch alarms for CPU, memory, and disk utilization.
* Use Auto Scaling groups to handle sudden traffic spikes.
* Implement application health checks with a load balancer.
* Enable centralized logging using CloudWatch Logs or ELK stack.

---

## 4. Scenario

High latency in application hosted on AWS.

### Identify

Users report that the application is responding slowly. Requests take longer than expected and monitoring tools show increased response times.

### Diagnose

1. Check CloudWatch metrics for EC2 instance performance.

```bash
aws cloudwatch get-metric-statistics \
--namespace AWS/EC2 \
--metric-name CPUUtilization \
--dimensions Name=InstanceId,Value=<instance-id> \
--statistics Average \
--period 300 \
--start-time <start-time> \
--end-time <end-time>
```

2. Check instance resource usage.

```bash
top
```

or

```bash
htop
```

3. Check network latency.

```bash
ping <application-endpoint>
```

4. Check if disk I/O is high.

```bash
iostat
```

5. Check application logs for slow queries or errors.

```bash
sudo tail -f /var/log/<application-log>.log
```

6. If using a database, check database performance and slow queries.

7. Verify load balancer health checks and target response times.

### Fix

* Scale EC2 instances vertically (larger instance type) or horizontally using Auto Scaling.
* Optimize application code or database queries.
* Enable caching (Redis/ElastiCache) for frequently accessed data.
* Adjust load balancer configuration if required.

### Prevent

* Configure CloudWatch alarms for latency and resource usage.
* Use Auto Scaling groups to automatically scale during traffic spikes.
* Implement caching layers to reduce database load.
* Continuously monitor application performance using AWS monitoring tools.

---

## 5. Scenario

Auto Scaling Group is not launching new instances.

### Identify

Application traffic increases but new EC2 instances are not launched by the Auto Scaling Group (ASG). Metrics show scaling conditions are met but no new instances appear.

### Diagnose

1. Check Auto Scaling activity history.

```bash
aws autoscaling describe-scaling-activities --auto-scaling-group-name <asg-name>
```

2. Verify scaling policies and alarms.

```bash
aws autoscaling describe-policies --auto-scaling-group-name <asg-name>
```

Check if CloudWatch alarms are triggering the scaling policy.

3. Check ASG capacity limits.

```bash
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names <asg-name>
```

Ensure:

* DesiredCapacity
* MaxSize

If DesiredCapacity = MaxSize, ASG cannot launch more instances.

4. Check Launch Template or Launch Configuration validity.

```bash
aws ec2 describe-launch-template-versions --launch-template-id <template-id>
```

5. Check if the subnet has available IP addresses.

6. Check EC2 service limits (instance quota).

7. Check if instance launch failures appear in scaling activities.

### Fix

* Increase Auto Scaling MaxSize.
* Fix invalid Launch Template or AMI.
* Ensure subnets have available IP addresses.
* Increase EC2 instance quota if limit reached.

Example updating ASG capacity:

```bash
aws autoscaling update-auto-scaling-group \
--auto-scaling-group-name <asg-name> \
--max-size 10
```

### Prevent

* Configure CloudWatch alarms to monitor scaling failures.
* Periodically validate Launch Templates and AMIs.
* Ensure proper capacity planning and service quotas.
* Use multiple subnets across AZs to avoid IP exhaustion.

---

## 6. Scenario

Application Load Balancer (ALB) shows unhealthy targets.

### Identify

The Application Load Balancer target group marks EC2 instances as **Unhealthy**. As a result, traffic is not routed to the instances and users may see 503 errors.

### Diagnose

1. Check target health status.

```bash
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

2. Verify health check configuration of the target group.

```bash
aws elbv2 describe-target-groups --target-group-arns <target-group-arn>
```

Check:

* HealthCheckPath
* HealthCheckPort
* HealthCheckProtocol

3. Verify that the application is running on the instance.

```bash
sudo systemctl status nginx
```

or

```bash
sudo systemctl status httpd
```

4. Verify the application port is listening.

```bash
sudo netstat -tulnp | grep <port>
```

5. Check Security Groups.

Ensure:

* ALB Security Group allows outbound traffic
* EC2 Security Group allows inbound traffic from the ALB Security Group

6. Test the health check endpoint locally on the instance.

```bash
curl http://localhost:<port>/health
```

7. Check application logs.

```bash
sudo tail -f /var/log/nginx/error.log
```

or

```bash
sudo tail -f /var/log/httpd/error_log
```

### Fix

* Start or restart the web server.

```bash
sudo systemctl restart nginx
```

* Correct the health check path (for example `/health` or `/`).
* Open required ports in the EC2 Security Group.
* Ensure the application responds with HTTP 200 for health checks.

### Prevent

* Configure proper health check endpoints in the application.
* Monitor ALB target health using CloudWatch metrics.
* Use Auto Scaling groups to replace unhealthy instances automatically.
* Implement centralized logging and monitoring for faster troubleshooting.

---

## 7. Scenario

Application cannot access S3 bucket.

### Identify

The application running on AWS (EC2, Lambda, or container) fails to read or write objects to an Amazon S3 bucket. Errors such as **AccessDenied**, **403 Forbidden**, or credential-related errors appear in logs.

### Diagnose

1. Check the IAM role attached to the compute service (EC2/Lambda/ECS).

```bash
aws ec2 describe-instances --instance-ids <instance-id>
```

Verify the IAM role attached to the instance.

2. Verify IAM role permissions for S3 access.

```bash
aws iam get-role-policy --role-name <role-name> --policy-name <policy-name>
```

Ensure permissions such as:

* s3:GetObject
* s3:PutObject
* s3:ListBucket

3. Check S3 bucket policy.

```bash
aws s3api get-bucket-policy --bucket <bucket-name>
```

4. Verify the application is using correct AWS credentials.

```bash
aws sts get-caller-identity
```

5. Check if the bucket is in a different region.

```bash
aws s3api get-bucket-location --bucket <bucket-name>
```

6. Verify network restrictions (VPC endpoint or private access policies).

### Fix

* Attach the correct IAM policy to the instance role.

Example policy permissions:

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListBucket"
  ],
  "Resource": [
    "arn:aws:s3:::<bucket-name>",
    "arn:aws:s3:::<bucket-name>/*"
  ]
}
```

* Correct the bucket policy if it denies access.
* Ensure the application uses the correct region and credentials.

### Prevent

* Use IAM roles instead of hardcoded credentials.
* Implement least-privilege IAM policies.
* Enable S3 access logging and CloudTrail for auditing.
* Monitor S3 access errors using CloudWatch alerts.

---

## 8. Scenario

RDS database connection fails from EC2.

### Identify

The application running on an EC2 instance cannot connect to the Amazon RDS database. Connection errors such as **connection timeout**, **access denied**, or **could not connect to server** appear in application logs.

### Diagnose

1. Check RDS instance status.

```bash
aws rds describe-db-instances --db-instance-identifier <db-instance-id>
```

Ensure the database state is **available**.

2. Verify Security Group rules.

* RDS Security Group must allow inbound traffic from the EC2 Security Group.
* Database port must be open (example: MySQL 3306, PostgreSQL 5432).

3. Test connectivity from EC2 instance.

```bash
nc -zv <rds-endpoint> 3306
```

or

```bash
telnet <rds-endpoint> 3306
```

4. Check DNS resolution.

```bash
nslookup <rds-endpoint>
```

5. Verify database credentials used by the application.

6. Check if the EC2 and RDS are in the same VPC or properly routed.

7. Verify subnet and Network ACL rules allow database traffic.

### Fix

* Allow EC2 Security Group in RDS Security Group inbound rules.

Example rule:

Type: MySQL
Port: 3306
Source: <EC2-Security-Group-ID>

* Correct database username or password.
* Ensure the RDS instance is in **available** state.
* Restart database connection service in the application if needed.

### Prevent

* Use security group referencing instead of IP addresses.
* Monitor RDS connection metrics in CloudWatch.
* Enable RDS performance insights for troubleshooting.
* Implement connection pooling in the application to avoid connection exhaustion.

---

## 9. Scenario

CloudFront is not serving updated content.

### Identify

Users report that the website served through Amazon CloudFront is still showing old content even after the application or files were updated in the origin (such as S3 or an EC2 web server).

### Diagnose

1. Check whether CloudFront is caching the content.

Verify the Cache-Control headers returned by the origin.

```bash
curl -I https://<cloudfront-domain>/<file>
```

2. Verify CloudFront distribution configuration.

```bash
aws cloudfront get-distribution --id <distribution-id>
```

3. Check the origin content directly to confirm the new version exists.

Example for S3:

```bash
aws s3 ls s3://<bucket-name>/<file>
```

4. Check if the object version changed but CloudFront cache has not expired.

5. Verify TTL settings in CloudFront behavior configuration.

### Fix

Create a CloudFront cache invalidation to refresh cached objects.

```bash
aws cloudfront create-invalidation \
--distribution-id <distribution-id> \
--paths "/*"
```

Alternatively invalidate specific objects.

```bash
aws cloudfront create-invalidation \
--distribution-id <distribution-id> \
--paths "/index.html"
```

Ensure correct Cache-Control headers are configured on the origin.

### Prevent

* Use versioned file names (for example: app-v2.js).
* Configure proper Cache-Control headers.
* Automate CloudFront invalidation in CI/CD pipelines.
* Monitor cache behavior and TTL settings to ensure timely content updates.

---

## 10. Scenario

Application suddenly stops after deployment.

### Identify

After a new deployment, the application becomes unavailable or crashes. Users may see errors such as **502/503**, or the service stops responding shortly after deployment.

### Diagnose

1. Check deployment logs (CI/CD tool such as Jenkins).

2. SSH into the EC2 instance and check application service status.

```bash
sudo systemctl status <application-service>
```

3. Check application logs for runtime errors.

```bash
sudo tail -f /var/log/<application-log>.log
```

4. Verify if the new version introduced configuration errors or missing environment variables.

5. Check if required dependencies are installed.

```bash
npm list
```

or

```bash
pip list
```

6. Check if the application port is running.

```bash
sudo netstat -tulnp | grep <port>
```

7. Verify health checks if the application is behind a Load Balancer.

### Fix

* Roll back to the previous stable version of the application.
* Fix configuration or dependency issues.

Restart the application service:

```bash
sudo systemctl restart <application-service>
```

* Correct environment variables or configuration files.

### Prevent

* Implement blue-green or canary deployments.
* Add automated tests in CI/CD pipelines.
* Use health checks to validate deployments.
* Monitor deployments using CloudWatch and logging tools.

---

## 11. Scenario

Lambda function execution fails.

### Identify

The AWS Lambda function fails during execution. Errors may appear in logs such as **Task timed out**, **AccessDeniedException**, **Unhandled exception**, or the function returns an error response.

### Diagnose

1. Check Lambda logs in CloudWatch.

```bash
aws logs describe-log-groups
```

Find the log group for the Lambda function and inspect logs.

```bash
aws logs filter-log-events \
--log-group-name /aws/lambda/<function-name>
```

2. Verify Lambda function configuration.

```bash
aws lambda get-function-configuration --function-name <function-name>
```

Check:

* Timeout
* MemorySize
* Runtime

3. Check IAM role permissions used by Lambda.

```bash
aws iam get-role --role-name <lambda-role>
```

Ensure required permissions exist (for example S3, DynamoDB, etc.).

4. Test the function manually.

```bash
aws lambda invoke \
--function-name <function-name> \
response.json
```

5. Verify environment variables.

```bash
aws lambda get-function-configuration \
--function-name <function-name> \
--query Environment
```

### Fix

* Increase Lambda timeout if the function exceeds execution time.
* Increase memory if the function runs out of resources.
* Fix code errors identified in logs.
* Attach correct IAM permissions.

Example update:

```bash
aws lambda update-function-configuration \
--function-name <function-name> \
--timeout 30 \
--memory-size 512
```

### Prevent

* Implement proper error handling in code.
* Monitor Lambda metrics and logs using CloudWatch.
* Use Dead Letter Queues (DLQ) for failed executions.
* Configure alerts for Lambda errors and throttling.

---

## 12. Scenario

EC2 instance CPU usage reaches 100%.

### Identify

Monitoring alerts (CloudWatch) show CPU utilization at or near 100%. Users may experience slow responses, request timeouts, or application crashes.

### Diagnose

1. Check CPU utilization metrics in CloudWatch.

```bash
aws cloudwatch get-metric-statistics \
--namespace AWS/EC2 \
--metric-name CPUUtilization \
--dimensions Name=InstanceId,Value=<instance-id> \
--statistics Average \
--period 300 \
--start-time <start-time> \
--end-time <end-time>
```

2. SSH into the instance to inspect running processes.

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

3. Identify processes consuming high CPU.

```bash
top
```

or

```bash
ps aux --sort=-%cpu | head
```

4. Check if the application or a background job is consuming resources.

5. Check system load.

```bash
uptime
```

6. Check application logs for abnormal activity.

```bash
sudo tail -f /var/log/<application-log>.log
```

### Fix

* Restart the high-CPU process or application.

```bash
sudo systemctl restart <application-service>
```

* Stop unnecessary processes.

* Scale the instance vertically (larger instance type).

* Use Auto Scaling to distribute traffic across multiple instances.

### Prevent

* Configure CloudWatch alarms for CPU thresholds.
* Implement Auto Scaling based on CPU utilization.
* Optimize application code and background jobs.
* Use load balancers to distribute traffic across instances.

---

## 13. Scenario

EBS volume gets full.

### Identify

Monitoring alerts (CloudWatch) indicate that the disk usage on the EC2 instance has reached or is close to 100%. Applications may fail to write data, logs stop updating, or services crash due to lack of disk space.

### Diagnose

1. SSH into the EC2 instance.

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

2. Check disk usage.

```bash
df -h
```

3. Identify which directory is consuming the most space.

```bash
sudo du -sh /* | sort -h
```

4. Inspect large log files.

```bash
sudo du -sh /var/log/*
```

5. Check for old Docker images or containers (if Docker is used).

```bash
docker system df
```

6. Check if temporary files or backups are consuming space.

```bash
sudo du -sh /tmp/*
```

### Fix

* Remove unnecessary files or old logs.

```bash
sudo rm -rf <large-file-or-directory>
```

* Clean Docker unused resources if applicable.

```bash
docker system prune -a
```

* Extend the EBS volume size.

```bash
aws ec2 modify-volume \
--volume-id <volume-id> \
--size <new-size-in-gb>
```

After resizing the volume, extend the filesystem.

```bash
sudo growpart /dev/xvda 1
```

```bash
sudo resize2fs /dev/xvda1
```

### Prevent

* Configure CloudWatch alarms for disk utilization.
* Implement log rotation.

```bash
sudo logrotate -f /etc/logrotate.conf
```

* Regularly clean unused Docker images and temporary files.
* Monitor disk usage using monitoring tools like CloudWatch or Prometheus.

---

## 14. Scenario

AWS bill suddenly increases unexpectedly.

### Identify

The monthly AWS bill shows an unexpected spike. Cost Explorer or billing alerts indicate a sudden increase in charges compared to previous usage.

### Diagnose

1. Check AWS Cost Explorer to identify which service caused the cost increase.

```bash
aws ce get-cost-and-usage \
--time-period Start=<start-date>,End=<end-date> \
--granularity DAILY \
--metrics "UnblendedCost"
```

2. Identify the service responsible for the spike (EC2, S3, Data Transfer, etc.).

3. Check running EC2 instances.

```bash
aws ec2 describe-instances
```

Look for:

* Unused instances
* Large instance types
* Instances running in multiple regions

4. Check EBS volumes and snapshots.

```bash
aws ec2 describe-volumes
```

5. Check S3 storage usage.

```bash
aws s3 ls --summarize --human-readable --recursive s3://<bucket-name>
```

6. Check data transfer costs and NAT Gateway usage.

7. Review Auto Scaling groups to see if too many instances were launched.

```bash
aws autoscaling describe-auto-scaling-groups
```

### Fix

* Stop or terminate unused EC2 instances.

```bash
aws ec2 stop-instances --instance-ids <instance-id>
```

* Delete unused EBS volumes or snapshots.

```bash
aws ec2 delete-volume --volume-id <volume-id>
```

* Remove unused S3 objects or move them to cheaper storage classes.

* Reduce instance sizes if over-provisioned.

### Prevent

* Set up AWS Budgets and billing alerts.
* Use Cost Explorer regularly to monitor spending.
* Implement resource tagging for cost tracking.
* Enable automatic shutdown for non-production environments.
* Use Auto Scaling and right-sizing recommendations to optimize resource usage.

---

## 15. Scenario

Application deployed in multiple Availability Zones becomes unavailable.

### Identify

The application is deployed across multiple AZs behind a load balancer, but users report downtime or intermittent failures. Even though instances exist in different AZs, the service is not reachable.

### Diagnose

1. Check Load Balancer status and target health.

```bash
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

2. Verify the load balancer listeners and routing rules.

```bash
aws elbv2 describe-listeners --load-balancer-arn <load-balancer-arn>
```

3. Check if instances are running in all configured AZs.

```bash
aws ec2 describe-instances --filters Name=availability-zone,Values=<az-name>
```

4. Check Auto Scaling group distribution across AZs.

```bash
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names <asg-name>
```

5. Verify Security Groups allow traffic between the load balancer and instances.

6. Check application health endpoints from each instance.

```bash
curl http://localhost:<port>/health
```

7. Check Route 53 DNS configuration if used.

```bash
aws route53 list-resource-record-sets --hosted-zone-id <hosted-zone-id>
```

### Fix

* Register healthy instances in the target group.
* Correct security group rules to allow load balancer traffic.
* Restart application services on instances.

```bash
sudo systemctl restart <application-service>
```

* Ensure Auto Scaling group spans multiple AZs correctly.

### Prevent

* Configure proper load balancer health checks.
* Use Auto Scaling across multiple AZs for high availability.
* Monitor target health and instance status with CloudWatch.
* Regularly test failover scenarios to ensure resilience.

---
