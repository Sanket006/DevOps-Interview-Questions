# Linux Interview Questions – DevOps / Linux Admin (Fresher → Intermediate)

This README contains **Linux interview questions with clear, medium-length answers**, tailored for **DevOps and Linux Admin fresher to intermediate level**. The content is structured, complete, and suitable for **interview revision, GitHub portfolios, and DevOps preparation**.

---

## 1. What is Linux?

Linux is an open-source, Unix-like operating system kernel. It manages system resources like CPU, memory, storage, and devices, and allows users to run applications. Popular Linux distributions include Ubuntu, CentOS, RHEL, and Amazon Linux.

---

## 2. What is the difference between Linux and Unix?

Linux is open-source and free, while Unix is mostly proprietary. Linux supports a wide variety of hardware and distributions, whereas Unix systems are usually vendor-specific and used mainly in enterprise environments.

---

## 3. What is the Linux kernel?

The kernel is the core of the Linux operating system. It handles hardware interaction, memory management, process scheduling, file systems, and system calls between applications and hardware.

---

## 4. What are the main components of Linux?

The main components are:

* Kernel
* Shell
* File system
* System utilities
* Applications

Each component works together to provide a stable OS.

---

## 5. What is a shell in Linux?

A shell is a command-line interpreter that allows users to interact with the Linux system. Common shells include Bash, Zsh, and Sh. Bash is the default shell in most Linux distributions.

---

## 6. What is the root user?

The root user is the superuser with full administrative privileges. Root can read, write, modify, or delete any file and perform system-level tasks like installing software and managing services.

---

## 7. What is the Linux file system hierarchy?

Linux follows a hierarchical file system starting from `/`. Important directories include:

* `/etc` – configuration files
* `/var` – logs
* `/home` – user files
* `/bin` – binaries
* `/usr` – user programs

---

## 8. What is the difference between /bin and /usr/bin?

`/bin` contains essential commands required for system boot and repair, while `/usr/bin` contains non-essential user commands and applications.

---

## 9. What is a process in Linux?

A process is a running instance of a program. Each process has a unique Process ID (PID) and consumes system resources like CPU and memory.

---

## 10. What is the difference between a process and a thread?

A process is an independent execution unit with its own memory space, while a thread is a lightweight unit that shares memory with other threads of the same process.

---

## 11. What is the use of the ps command?

The `ps` command displays information about active processes. Common usage includes `ps -ef` or `ps aux` to view all running processes.

---

## 12. What does the top command do?

top shows real-time system performance including CPU usage, memory usage, load average, and running processes. It helps in monitoring system health.

---

## 13. What is a daemon process?

A daemon is a background process that runs continuously and performs system tasks, such as `sshd`, `cron`, or `httpd`.

---

## 14. What is the purpose of the chmod command?

`chmod` is used to change file or directory permissions. Permissions include read (r), write (w), and execute (x) for owner, group, and others.

---

## 15. What is the difference between chown and chmod?

`chmod` changes file permissions, while `chown` changes file ownership (user and/or group).

---

## 16. What are symbolic links and hard links?

A hard link points directly to a file’s inode, while a symbolic (soft) link points to the file path. If the original file is deleted, a hard link still works but a symbolic link breaks.

---

## 17. What is the use of the df command?

`df` displays disk space usage of file systems. `df -h` shows the output in a human-readable format.

---

## 18. What is the du command?

`du` shows disk usage of files and directories. It helps identify which directories are consuming more disk space.

---

## 19. What is swap space?

Swap space is disk space used as virtual memory when RAM is full. It helps prevent system crashes but is slower than physical memory.

---

## 20. What is the difference between kill and kill -9?

`kill` sends a termination signal (SIGTERM) allowing the process to exit gracefully, while `kill -9` (SIGKILL) forcefully stops the process.

---

## 21. What is a runlevel or systemd target?

Runlevels (in SysV) and targets (in systemd) define the system’s state, such as multi-user mode or graphical mode.

---

## 22. What is systemctl?

`systemctl` is used to manage services in systemd-based Linux systems. It can start, stop, restart, enable, and check the status of services.

---

## 23. What is cron and crontab?

Cron is a scheduler that runs commands at specified times. `crontab` is used to create, edit, and manage cron jobs.

---

## 24. What is the use of the grep command?

`grep` searches for patterns in files or command output. It is commonly used with pipes to filter logs and text data.

---

## 25. What is SSH?

SSH (Secure Shell) is a secure protocol used to remotely access and manage Linux servers over an encrypted connection.

---

## 26. What is the difference between scp and rsync?

`scp` copies files securely but transfers entire files every time, while `rsync` transfers only changed data, making it faster and more efficient.

---

## 27. What is /etc/passwd?

`/etc/passwd` stores user account information such as username, UID, GID, home directory, and shell.

---

## 28. What is /etc/shadow?

`/etc/shadow` stores encrypted user passwords and password expiry information. It is readable only by root for security reasons.

---

## 29. What is the purpose of the mount command?

`mount` attaches a file system to a directory in Linux, allowing access to storage devices like disks or network file systems.

---

## 30. What is a package manager?

A package manager is a tool used to install, update, and remove software. Examples include `apt`, `yum`, and `dnf`.

---

# Additional Linux Interview Questions (Intermediate Level)

## 51. What is the difference between su and sudo?

`su` switches to another user (usually root) and requires that user’s password. `sudo` allows permitted users to run specific commands as root using their own password, offering better security and auditing.

---

## 52. What is the purpose of /etc/fstab?

`/etc/fstab` defines how and where file systems are mounted at boot time. It contains information about devices, mount points, file system types, and mount options.

---

## 53. What is inode in Linux?

An inode stores metadata about a file such as permissions, ownership, size, and timestamps, but not the filename or file data.

---

## 54. What happens when a file is deleted in Linux?

When a file is deleted, the link to its inode is removed. If no hard links remain and the file is not in use, the data blocks are freed.

---

## 55. What is the nice and renice command?

`nice` starts a process with a specific priority, while `renice` changes the priority of an already running process. Lower nice values mean higher priority.

---

## 56. What is load average in Linux?

Load average represents the average number of processes waiting for CPU or I/O over 1, 5, and 15 minutes. High load indicates system stress.

---

## 57. What is the difference between df and du?

`df` reports disk usage at the filesystem level, while `du` reports disk usage at the file and directory level.

---

## 58. What is SELinux?

SELinux (Security-Enhanced Linux) is a security module that enforces access control policies to limit what processes can do, even if they run as root.

---

## 59. What are Linux file permissions?

Linux permissions define who can read, write, or execute a file. Permissions are set for owner, group, and others using `r`, `w`, and `x`.

---

## 60. What is umask?

`umask` defines default permissions for newly created files and directories by subtracting permissions from system defaults.

---

## 61. What is the difference between soft link and hard link?

A hard link points directly to the inode, while a soft link points to the file path. Hard links remain valid even if the original file is deleted.

---

## 62. What is the purpose of tar?

`tar` is used to archive multiple files into a single file. It is commonly used with compression tools like `gzip` or `bzip2`.

---

## 63. What is the free command?

`free` shows memory usage including total, used, free, and available memory, along with swap usage.

---

## 64. What is swapiness (swappiness)?

Swappiness controls how aggressively the kernel moves processes from RAM to swap. Lower values prefer RAM usage; higher values use swap more often.

---

## 65. What is the uptime command?

`uptime` shows how long the system has been running, number of logged-in users, and load average.

---

## 66. What is journald?

`journald` is a systemd service that collects and manages system logs in binary format, accessible via `journalctl`.

---

## 67. What is the purpose of journalctl?

`journalctl` is used to view and analyze logs collected by systemd, including service-specific logs.

---

## 68. What is the difference between service and systemctl?

`service` works with SysVinit scripts, while `systemctl` manages services in systemd-based systems and provides more control and features.

---

## 69. What is an environment variable?

An environment variable stores configuration values that affect system and application behavior, such as `PATH` or `HOME`.

---

## 70. What is the purpose of the export command?

`export` makes a shell variable available to child processes and scripts.

---

✅ **This README is suitable for GitHub, interview preparation, and DevOps/Linux admin revision.**
