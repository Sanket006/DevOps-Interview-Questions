## Linux Troubleshooting Interview Questions & Answers

This document contains Linux troubleshooting interview questions with simple, medium-length, interview-friendly answers. These are useful for **DevOps / Linux admin (fresher to intermediate)** roles.

- **Section 1**: 50 general Linux troubleshooting questions  
- **Section 2**: 20 additional Linux troubleshooting questions  
- **Section 3**: Scenario-based troubleshooting questions  

---

## 1. Linux Troubleshooting Interview Questions & Answers (1–50)

1. **A Linux server is slow. How will you start troubleshooting?**  
   I will first check CPU, memory, disk, and network usage using commands like `top`, `free -m`, `df -h`, and `iostat`. Then I will identify which process is consuming high resources and check system logs for errors.

2. **How do you check CPU usage in Linux?**  
   I use `top`, `htop`, or `mpstat`. These commands show CPU usage, load average, and which processes are using most CPU.

3. **How do you check memory usage?**  
   I use `free -m`, `top`, or `vmstat`. These commands show total, used, free memory, and swap usage.

4. **What will you do if memory is full?**  
   I will identify memory-consuming processes using `top` or `ps aux`. I may restart unnecessary services, clear cache, or increase RAM or swap space if needed.

5. **How do you check disk usage?**  
   I use `df -h` to check disk space and `du -sh` to find large files or directories.

6. **What if disk space is 100% full?**  
   I will find large files using `du` or `find`, remove unused logs, clean temporary files, or extend the disk if required.

7. **How do you find large files in Linux?**  
   I use:

   ```bash
   du -ah / | sort -rh | head -10
   ```

   This shows the largest files and directories.

8. **How do you check running services?**  
   I use:

   ```bash
   systemctl status <service-name>
   systemctl list-units --type=service
   ```

9. **A service is not starting. What will you do?**  
   I will check service status, logs using `journalctl -u <service-name>`, verify configuration files, and check permissions or port conflicts.

10. **How do you check logs in Linux?**  
    I check logs in `/var/log/` and use `journalctl` for systemd-based systems.

11. **What if a server is not reachable over SSH?**  
    I check network connectivity, firewall rules, SSH service status, port 22 availability, and security group settings (if cloud).

12. **How do you check network connectivity?**  
    I use `ping`, `ip a`, `ip route`, `netstat`, and `ss` commands.

13. **What will you do if ping is not working?**  
    I will check network interface status, IP configuration, firewall rules, and routing table.

14. **How do you check open ports?**  
    I use:

    ```bash
    netstat -tulnp
    ss -tulnp
    ```

15. **What if a port is already in use?**  
    I find the process using the port and stop or reconfigure it using:

    ```bash
    lsof -i :<port-number>
    ```

16. **How do you check system uptime?**  
    I use the `uptime` command.

17. **What is load average?**  
    Load average shows how many processes are waiting for CPU. High load means the system is under heavy usage.

18. **How do you check which process is using most CPU?**  
    Using `top` or:

    ```bash
    ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head
    ```

19. **How do you kill a stuck process?**  
    I use:

    ```bash
    kill <PID>
    kill -9 <PID>   # if normal kill doesn’t work
    ```

20. **What if a command is not found?**  
    I check if the package is installed or if the `PATH` variable is correctly set.

21. **How do you check filesystem errors?**  
    I use `dmesg`, `fsck` (after unmount), and system logs.

22. **What will you do if a file cannot be deleted?**  
    I check permissions, ownership, file locks, and if the filesystem is read-only.

23. **How do you check file permissions?**  
    Using:

    ```bash
    ls -l <filename>
    ```

24. **What if “permission denied” error occurs?**  
    I check file permissions, ownership, and use `chmod` or `chown` if needed.

25. **How do you check user login issues?**  
    I check `/etc/passwd`, `/etc/shadow`, logs, password expiry, and SSH configuration.

26. **What if root login is not working?**  
    I check SSH config, sudo permissions, password, and PAM settings.

27. **How do you check swap usage?**  
    I use:

    ```bash
    swapon --show
    free -m
    ```

28. **What if swap is full?**  
    I identify memory-heavy processes or increase swap size.

29. **How do you check cron job issues?**  
    I check cron logs, permissions, cron syntax, and environment variables.

30. **What if a cron job is not running?**  
    I verify cron service status, job schedule, user permissions, and logs.

31. **How do you troubleshoot high I/O wait?**  
    I use `iostat`, `vmstat`, and check disk health and heavy disk operations.

32. **How do you check disk I/O usage?**  
    Using:

    ```bash
    iostat
    iotop
    ```

33. **What if a system reboots automatically?**  
    I check logs, kernel panic messages, hardware issues, and scheduled reboots.

34. **How do you check kernel messages?**  
    Using:

    ```bash
    dmesg
    ```

35. **What is a zombie process?**  
    A zombie process has completed execution but still exists in the process table.

36. **How do you remove zombie processes?**  
    I restart or kill the parent process.

37. **How do you check environment variables?**  
    Using:

    ```bash
    printenv
    echo $VARIABLE
    ```

38. **What if hostname is not resolving?**  
    I check `/etc/hosts`, DNS settings, and `resolv.conf`.

39. **How do you check DNS issues?**  
    I use `nslookup`, `dig`, and check DNS configuration.

40. **How do you check firewall rules?**  
    Using:

    ```bash
    iptables -L
    firewall-cmd --list-all
    ```

41. **What if an application crashes frequently?**  
    I check logs, resource usage, application configuration, and dependencies.

42. **How do you check system time issues?**  
    Using:

    ```bash
    date
    timedatectl
    ```

43. **What if time is not syncing?**  
    I check NTP or `chrony` service status.

44. **How do you check mounted filesystems?**  
    Using:

    ```bash
    mount
    df -h
    ```

45. **What if filesystem is read-only?**  
    I check disk errors and remount the filesystem after fixing issues.

46. **How do you check users currently logged in?**  
    Using:

    ```bash
    who
    w
    ```

47. **How do you check last reboot reason?**  
    Using:

    ```bash
    last reboot
    journalctl
    ```

48. **What if a command hangs?**  
    I check system load, I/O wait, and try running it in debug mode.

49. **How do you troubleshoot application port issues?**  
    I check port binding, firewall, SELinux, and service configuration.

50. **What is your general Linux troubleshooting approach?**  
    I follow these steps:  
    **Identify issue → Check logs → Check resources → Isolate root cause → Apply fix → Monitor system**

---

## 2. Linux Troubleshooting Interview Questions & Answers (1–20)

1. **A Linux server is running slow. How will you troubleshoot it?**  
   I will first check CPU, memory, disk, and network usage using commands like `top`, `free -m`, and `df -h`. Then I will identify which process is consuming high resources and check logs for any errors.

2. **How do you check CPU usage in Linux?**  
   I use `top`, `htop`, or `mpstat`. These commands show CPU usage, load average, and running processes.

3. **How do you check memory usage?**  
   I use `free -m` and `top`. These commands show total, used, free memory, and swap usage.

4. **What will you do if memory usage is very high?**  
   I will find memory-consuming processes using `top` or `ps`. I may restart unnecessary services or add more RAM or swap space.

5. **How do you check disk space usage?**  
   I use `df -h` to check disk space and `du -sh` to find large directories.

6. **What will you do if disk space is full?**  
   I will identify large files, remove unused logs or temporary files, and if required, extend the disk size.

7. **How do you find large files in Linux?**  
   I use:

   ```bash
   du -ah / | sort -rh | head -10
   ```

   This shows the largest files and folders.

8. **A service is not starting. How will you troubleshoot?**  
   I check service status using `systemctl status`, review logs using `journalctl`, and verify configuration files and permissions.

9. **How do you check system logs?**  
   I check `/var/log/` directory and use `journalctl` for systemd-based systems.

10. **SSH is not working. What steps will you follow?**  
    I check network connectivity, SSH service status, firewall rules, and port 22 availability.

11. **How do you check network connectivity in Linux?**  
    I use `ping`, `ip a`, and `ip route` commands.

12. **How do you check open ports on a Linux server?**  
    I use:

    ```bash
    netstat -tulnp
    ss -tulnp
    ```

13. **What will you do if a port is already in use?**  
    I find the process using the port with `lsof -i :<port>` and stop or reconfigure it.

14. **How do you check which process is using high CPU?**  
    I use `top` or:

    ```bash
    ps -eo pid,cmd,%cpu --sort=-%cpu | head
    ```

15. **How do you kill a stuck process?**  
    I use `kill <PID>`. If it does not stop, I use `kill -9 <PID>`.

16. **What does “Permission denied” mean and how do you fix it?**  
    It means the user does not have access to the file or command. I fix it by checking permissions and ownership using `ls -l`, `chmod`, or `chown`.

17. **How do you check disk I/O issues?**  
    I use `iostat` and `iotop` to identify heavy disk usage.

18. **What is a zombie process?**  
    A zombie process has finished execution but still remains in the process table because the parent process has not cleaned it.

19. **How do you fix a zombie process?**  
    I restart or kill the parent process of the zombie.

20. **What is your general approach to Linux troubleshooting?**  
    I follow these steps:  
    **Understand the problem → Check logs → Check system resources → Find root cause → Fix the issue → Monitor the system**

---

## 3. Scenario-Based Linux Troubleshooting Questions & Answers

Below are scenario-based Linux troubleshooting interview questions with clear, step-by-step answers in easy and simple language. These are very commonly asked in real DevOps/Linux interviews.

1. **A Linux server is very slow during peak hours. What will you do?**  
   I will check CPU, memory, disk, and network usage using `top`, `free -m`, and `iostat`. Then I will identify which process is consuming high resources. I will also check logs to see if any job or application is running repeatedly.

2. **Disk space is 100% full and the application is down. How will you fix it?**  
   First, I will check disk usage using `df -h`. Then I will find large files using `du -sh`. I will delete old logs or temporary files, restart the application, and later plan disk expansion.

3. **SSH connection to the server is failing. What are your steps?**  
   I will check if the server is reachable using `ping`. Then I will check SSH service status, firewall rules, port 22 availability, and SSH configuration files.

4. **A service is not starting after server reboot. What will you do?**  
   I will check the service status using `systemctl status`. Then I will review logs using `journalctl`. I will verify configuration files, file permissions, and dependencies.

5. **A website is not accessible but the server is running fine. How will you troubleshoot?**  
   I will check if the web service is running, verify port availability, check firewall rules, review application logs, and ensure DNS is pointing to the correct IP.

6. **Memory usage is very high and the server is hanging. What will you do?**  
   I will identify memory-heavy processes using `top`. I will restart unnecessary services, clear cache if possible, or add swap memory temporarily.

7. **A user is unable to log in to the Linux server. What will you check?**  
   I will check user account status, password expiry, `/etc/passwd`, `/etc/shadow`, and SSH logs.

8. **Cron job is not running at scheduled time. What will you do?**  
   I will check cron service status, verify cron syntax, check logs, ensure correct user permissions, and confirm environment variables.

9. **Filesystem becomes read-only automatically. What could be the reason?**  
   This usually happens due to disk or filesystem errors. I will check system logs and run `fsck` after unmounting the filesystem.

10. **A process is consuming high CPU continuously. How will you handle it?**  
    I will identify the process using `top`. I will check whether it is required. If not, I will restart or kill the process and investigate the root cause.

11. **Application port is not accessible from outside. What will you check?**  
    I will check if the service is listening on the correct port, firewall rules, security groups, and SELinux settings.

12. **A Linux server reboots automatically. What will you investigate?**  
    I will check system logs, kernel panic messages, hardware issues, cron jobs, and cloud provider events.

13. **DNS is not resolving on the server. What will you do?**  
    I will check `/etc/resolv.conf`, test DNS using `nslookup` or `dig`, and verify network connectivity.

14. **A command is hanging and not responding. How will you troubleshoot?**  
    I will check system load, disk I/O wait, and running processes. If needed, I will kill the stuck command.

15. **Swap usage is very high. What does it indicate and what will you do?**  
    It indicates low memory. I will identify memory-heavy processes and consider increasing RAM or optimizing applications.

16. **Application logs are not being generated. What could be the issue?**  
    It could be permission issues, incorrect log path, disk full, or misconfigured logging settings.

17. **Network is slow on the Linux server. How will you debug it?**  
    I will check network interface status, bandwidth usage, packet loss, and firewall rules.

18. **User reports “Permission denied” while accessing a file. How will you fix it?**  
    I will check file permissions and ownership using `ls -l` and correct them using `chmod` or `chown`.

19. **Time is different on multiple servers. What problem can this cause and how will you fix it?**  
    It can cause issues in logs and authentication. I will check and enable NTP or `chrony` service.

20. **How do you generally approach a Linux troubleshooting issue?**  
    I follow this approach:  
    **Understand the issue → Check logs → Check system resources → Identify root cause → Apply fix → Monitor**

---

## ⭐ Interview Tip (Important)

Always explain **commands + reason + result**. Interviewers care more about your **approach and thought process** than just the final command.

---