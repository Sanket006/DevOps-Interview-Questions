# 🧑‍💻 DevOps Interview Scripting & Coding Questions (Worldpay Prep)

This document contains **commonly asked DevOps scripting/coding questions** along with solutions.
Focus is on **real-world automation, debugging, and system tasks** rather than DSA.

---

## 📌 Table of Contents

* [1. Check if Port is Open](#1-check-if-port-is-open)
* [2. Parse Logs](#2-parse-logs)
* [3. Delete Old Files](#3-delete-old-files)
* [4. API Call Script](#4-api-call-script)
* [5. Disk Usage Alert (Shell)](#5-disk-usage-alert-shell)
* [6. Execute Command on Multiple Servers](#6-execute-command-on-multiple-servers)
* [7. Find Largest File](#7-find-largest-file)
* [8. Check Running Process](#8-check-running-process)
* [9. Basic Python Logic](#9-basic-python-logic)
* [10. Retry Mechanism](#10-retry-mechanism)

---

## 1. Check if Port is Open

### ✅ Python

```python
import socket

def check_port(host, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(2)
    result = s.connect_ex((host, port))
    
    if result == 0:
        print(f"Port {port} is OPEN")
    else:
        print(f"Port {port} is CLOSED")

check_port("127.0.0.1", 80)
```

📌 **Use Case:** Health checks, monitoring systems

---

## 2. Parse Logs

### ✅ Python

```python
def count_errors(file):
    count = 0
    with open(file, 'r') as f:
        for line in f:
            if "ERROR" in line:
                count += 1
    return count

print(count_errors("app.log"))
```

📌 **Use Case:** Log monitoring, alerting

---

## 3. Delete Old Files

### ✅ Python

```python
import os
import time

def delete_old_files(folder, days):
    now = time.time()
    
    for filename in os.listdir(folder):
        path = os.path.join(folder, filename)
        
        if os.path.isfile(path):
            if now - os.path.getmtime(path) > days * 86400:
                os.remove(path)
                print(f"Deleted: {filename}")

delete_old_files("/tmp", 7)
```

📌 **Use Case:** Log rotation, disk cleanup

---

## 4. API Call Script

### ✅ Python

```python
import requests

response = requests.get("https://api.github.com")

if response.status_code == 200:
    print(response.json())
else:
    print("API failed")
```

📌 **Use Case:** Health checks, integrations

---

## 5. Disk Usage Alert (Shell)

### ✅ Bash

```bash
#!/bin/bash

usage=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

if [ $usage -gt 80 ]; then
  echo "Disk usage is above 80%"
else
  echo "Disk usage is normal"
fi
```

📌 **Use Case:** System monitoring alerts

---

## 6. Execute Command on Multiple Servers

### ✅ Bash

```bash
#!/bin/bash

for server in server1 server2 server3
do
  ssh user@$server "uptime"
done
```

📌 **Use Case:** Automation across servers

---

## 7. Find Largest File

### ✅ Python

```python
import os

def largest_file(path):
    max_size = 0
    max_file = None
    
    for file in os.listdir(path):
        full_path = os.path.join(path, file)
        
        if os.path.isfile(full_path):
            size = os.path.getsize(full_path)
            if size > max_size:
                max_size = size
                max_file = file
    
    return max_file

print(largest_file("."))
```

📌 **Use Case:** Storage analysis

---

## 8. Check Running Process

### ✅ Bash

```bash
#!/bin/bash

process="nginx"

if pgrep $process > /dev/null
then
  echo "$process is running"
else
  echo "$process is NOT running"
fi
```

📌 **Use Case:** Service monitoring

---

## 9. Basic Python Logic

### Reverse a String

```python
s = "devops"
print(s[::-1])
```

### Count Vowels

```python
s = "hello world"
count = sum(1 for c in s if c in "aeiou")
print(count)
```

📌 **Use Case:** Basic programming fundamentals

---

## 10. Retry Mechanism

### ✅ Python

```python
import time

def retry(func, retries=3):
    for i in range(retries):
        try:
            return func()
        except:
            print("Retrying...")
            time.sleep(2)

def test():
    return 10 / 0

retry(test)
```

📌 **Use Case:** CI/CD pipelines, API retries

---

# 🎯 Interview Tips

* Explain your **approach before coding**
* Focus on **real-world use cases**
* Write **clean and readable code**
* Mention where you used similar logic in projects

---

# 🚀 Final Advice

This set covers **80–90% of DevOps scripting questions** asked in interviews.

👉 Focus on:

* Logic
* Practical usage
* Confidence while explaining

---

**Good luck with your interview! 💪**
