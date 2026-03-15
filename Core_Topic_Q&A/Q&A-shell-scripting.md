# 50 Shell Scripting Interview Questions with Answers

This repository contains **50 important Shell Scripting interview questions** commonly asked in Linux and DevOps interviews.
Each answer follows a structured format:

**Definition → Working → Example**

---

# 1. What is Shell Scripting?

## Definition

Shell scripting is writing a sequence of commands in a file that the shell can execute to automate tasks in a Linux/UNIX environment.

## Working

A shell script contains commands that are executed line by line by a shell interpreter such as **bash**.

## Example

```bash
#!/bin/bash
echo "Hello World"
```

---

# 2. What is a Shebang in Shell Script?

## Definition

Shebang is the first line of a script that specifies the interpreter used to execute the script.

## Working

The operating system reads the shebang line to determine which interpreter should run the script.

## Example

```bash
#!/bin/bash
```

---

# 3. What are variables in shell scripting?

## Definition

Variables are used to store data values that can be referenced and manipulated in a script.

## Working

Variables are assigned without spaces using the assignment operator.

## Example

```bash
name="Sanket"
echo "$name"
```

---

# 4. What is the difference between local and global variables?

## Definition

Local variables exist only within a function while global variables are accessible throughout the script.

## Working

Local variables are declared using the `local` keyword inside functions.

## Example

```bash
function demo() {
  local name="DevOps"
  echo $name
}
```

---

# 5. What are positional parameters?

## Definition

Positional parameters are variables that store command-line arguments passed to a script.

## Working

They are referenced using `$1`, `$2`, `$3`, etc.

## Example

```bash
#!/bin/bash
echo "First argument: $1"
```

---

# 6. How do you read input from a user?

## Definition

User input is read using the `read` command.

## Working

The `read` command takes input from the keyboard and stores it in a variable.

## Example

```bash
read name
echo "Hello $name"
```

---

# 7. What is command substitution?

## Definition

Command substitution allows the output of a command to be used as a value.

## Working

The command is executed and its output replaces the command substitution expression.

## Example

```bash
date=$(date)
echo $date
```

---

# 8. What is a conditional statement?

## Definition

Conditional statements allow decision making in shell scripts.

## Working

The script executes different blocks depending on the condition.

## Example

```bash
if [ $a -gt $b ]
then
 echo "A is greater"
fi
```

---

# 9. What is an if-else statement?

## Definition

The if-else statement executes one block if a condition is true and another if it is false.

## Working

The condition is evaluated using test expressions.

## Example

```bash
if [ $num -gt 10 ]
then
 echo "Greater"
else
 echo "Smaller"
fi
```

---

# 10. What is a case statement?

## Definition

A case statement is used for multi-condition decision making.

## Working

The script compares a variable with multiple patterns.

## Example

```bash
case $1 in
 start) echo "Starting" ;;
 stop) echo "Stopping" ;;
esac
```

---

# 11. What is a for loop?

## Definition

A for loop executes commands repeatedly for a fixed list of values.

## Working

The loop iterates through a sequence of items.

## Example

```bash
for i in 1 2 3
 do
 echo $i
 done
```

---

# 12. What is a while loop?

## Definition

A while loop runs as long as the condition remains true.

## Working

The condition is checked before every iteration.

## Example

```bash
while [ $i -le 5 ]
 do
 echo $i
 done
```

---

# 13. What is an until loop?

## Definition

An until loop runs until a condition becomes true.

## Working

The loop stops once the condition evaluates to true.

## Example

```bash
until [ $i -gt 5 ]
 do
 echo $i
 done
```

---

# 14. What is break in shell scripting?

## Definition

Break is used to terminate a loop immediately.

## Working

When break is encountered, the loop stops executing.

## Example

```bash
break
```

---

# 15. What is continue in shell scripting?

## Definition

Continue skips the current iteration and moves to the next iteration.

## Working

It prevents execution of remaining commands in the loop body.

## Example

```bash
continue
```

---

# 16. What are functions in shell scripting?

## Definition

Functions are reusable blocks of code.

## Working

Functions are defined once and called multiple times.

## Example

```bash
function hello() {
 echo "Hello"
}
hello
```

---

# 17. What is exit status?

## Definition

Exit status is the return code of a command.

## Working

A value of `0` means success and non-zero indicates failure.

## Example

```bash
echo $?
```

---

# 18. What is a pipeline?

## Definition

A pipeline passes the output of one command as input to another.

## Working

It uses the pipe symbol `|`.

## Example

```bash
ls | grep txt
```

---

# 19. What is input redirection?

## Definition

Input redirection sends input to a command from a file.

## Working

The `<` symbol is used.

## Example

```bash
wc -l < file.txt
```

---

# 20. What is output redirection?

## Definition

Output redirection sends command output to a file.

## Working

The `>` operator overwrites the file.

## Example

```bash
echo "Hello" > file.txt
```

---

# 21. What is append redirection?

## Definition

Append redirection adds output to the end of a file.

## Working

It uses `>>` operator.

## Example

```bash
echo "Hello" >> file.txt
```

---

# 22. What is a wildcard?

## Definition

Wildcards are special characters used for pattern matching.

## Working

They allow commands to match multiple files.

## Example

```bash
ls *.txt
```

---

# 23. What is a cron job?

## Definition

Cron jobs are scheduled tasks executed automatically.

## Working

They are configured using `crontab`.

## Example

```bash
crontab -e
```

---

# 24. What is environment variable?

## Definition

Environment variables store system-wide configuration values.

## Working

They are available to all processes.

## Example

```bash
echo $PATH
```

---

# 25. What is export command?

## Definition

The export command makes a variable available to child processes.

## Working

It converts a shell variable into an environment variable.

## Example

```bash
export PATH
```

---

# 26. What is the read-only variable?

## Definition

Read-only variables cannot be modified after assignment.

## Working

The `readonly` command is used.

## Example

```bash
readonly PI=3.14
```

---

# 27. What is the unset command?

## Definition

Unset removes a variable from memory.

## Working

It deletes the variable definition.

## Example

```bash
unset name
```

---

# 28. What is a subshell?

## Definition

A subshell is a separate instance of the shell.

## Working

Commands inside parentheses run in a subshell.

## Example

```bash
(cd /tmp && ls)
```

---

# 29. What is a here document?

## Definition

A here document allows passing multi-line input to commands.

## Working

It uses `<<` operator.

## Example

```bash
cat << EOF
Hello
World
EOF
```

---

# 30. What is a here string?

## Definition

A here string passes a single string as input to a command.

## Working

It uses `<<<`.

## Example

```bash
grep "hello" <<< "hello world"
```

---

# 31. What is debugging in shell script?

## Definition

Debugging identifies errors in a script.

## Working

The `-x` option prints executed commands.

## Example

```bash
bash -x script.sh
```

---

# 32. What is set command?

## Definition

The set command modifies shell options.

## Working

Options control shell behavior.

## Example

```bash
set -x
```

---

# 33. What is getopts?

## Definition

getopts parses command-line options.

## Working

It reads flags passed to a script.

## Example

```bash
getopts "ab:" option
```

---

# 34. What is basename command?

## Definition

basename extracts the filename from a path.

## Working

It removes directory information.

## Example

```bash
basename /home/user/file.txt
```

---

# 35. What is dirname command?

## Definition

dirname extracts directory path.

## Working

It removes the filename.

## Example

```bash
dirname /home/user/file.txt
```

---

# 36. What is trap command?

## Definition

trap captures signals and executes commands.

## Working

It is used for cleanup actions.

## Example

```bash
trap "echo Interrupted" SIGINT
```

---

# 37. What is exec command?

## Definition

exec replaces the current shell process.

## Working

It executes a command without creating a new process.

## Example

```bash
exec ls
```

---

# 38. What is eval command?

## Definition

eval executes arguments as shell commands.

## Working

It expands variables before execution.

## Example

```bash
eval "echo Hello"
```

---

# 39. What is source command?

## Definition

source runs a script in the current shell.

## Working

It does not create a new shell.

## Example

```bash
source script.sh
```

---

# 40. What is alias command?

## Definition

alias creates shortcuts for commands.

## Working

It replaces long commands with short names.

## Example

```bash
alias ll='ls -la'
```

---

# 41. What is a background process?

## Definition

A background process runs independently of the terminal.

## Working

The `&` symbol runs commands in background.

## Example

```bash
sleep 60 &
```

---

# 42. What is a foreground process?

## Definition

Foreground processes run in the terminal and block it until completion.

## Working

Commands run normally without `&`.

## Example

```bash
sleep 10
```

---

# 43. What is the jobs command?

## Definition

jobs lists active background jobs.

## Working

It shows job ID and status.

## Example

```bash
jobs
```

---

# 44. What is the kill command?

## Definition

kill sends signals to processes.

## Working

It terminates or controls processes.

## Example

```bash
kill -9 1234
```

---

# 45. What is awk?

## Definition

awk is a pattern scanning and text processing tool.

## Working

It processes structured text line by line.

## Example

```bash
awk '{print $1}' file.txt
```

---

# 46. What is sed?

## Definition

sed is a stream editor used for modifying text.

## Working

It performs text transformations.

## Example

```bash
sed 's/linux/unix/' file.txt
```

---

# 47. What is grep?

## Definition

grep searches for patterns in files.

## Working

It prints matching lines.

## Example

```bash
grep "error" log.txt
```

---

# 48. What is xargs?

## Definition

xargs builds argument lists and executes commands.

## Working

It takes input from standard input.

## Example

```bash
cat files.txt | xargs rm
```

---

# 49. What is tee command?

## Definition

tee reads input and writes to both stdout and files.

## Working

It allows saving output while displaying it.

## Example

```bash
echo "Hello" | tee file.txt
```

---

# 50. What is the difference between sh and bash?

## Definition

sh is the original UNIX shell while bash is an enhanced shell with additional features.

## Working

bash provides better scripting capabilities and user-friendly features.

## Example

```bash
#!/bin/bash
```

---

# UNIX / Linux Shell Interview Questions (1–20)

This document provides structured answers to common UNIX/Linux shell interview questions. Each answer is written in a concise format suitable for technical interviews and learning.

---

## 1. How can variables be wiped out?

Variables can be removed using the `unset` command.

Example:

```bash
name="Sanket"
unset name
echo $name
```

---

## 2. What are positional parameters?

Positional parameters are special variables used in shell scripts to access command-line arguments.

| Parameter | Meaning         |
| --------- | --------------- |
| `$0`      | Script name     |
| `$1`      | First argument  |
| `$2`      | Second argument |

Example:

```bash
#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
```

---

## 3. What does the `.` indicate at the beginning of a file name?

Files starting with `.` are hidden files in UNIX/Linux.

Example:

```
.bashrc
.profile
.gitconfig
```

List hidden files:

```bash
ls -a
```

---

## 4. Generally each block in UNIX is how many bytes?

Typically one block equals **512 bytes** (traditional UNIX). Modern file systems may use **4096 bytes (4 KB)**.

---

## 5. By default, how many links does a new file or directory have?

| Type      | Links |
| --------- | ----- |
| File      | 1     |
| Directory | 2     |

---

## 6. Explain file permissions

UNIX file permissions control access to files and directories.

Permission categories:

* Owner
* Group
* Others

Permission types:

| Symbol | Meaning |
| ------ | ------- |
| r      | Read    |
| w      | Write   |
| x      | Execute |

Example:

```
-rwxr-xr--
```

---

## 7. What is a file system?

A file system is the method used by an operating system to organize, store, and manage files on storage devices.

Examples:

* ext4
* xfs
* ntfs

---

## 8. What are the different blocks of a file system?

1. **Boot Block** – Contains boot information.
2. **Super Block** – Stores file system metadata.
3. **Inode Block** – Stores file information.
4. **Data Block** – Stores actual file data.

---

## 9. What are the three security provisions provided by UNIX?

1. File permissions
2. User authentication
3. Access control using users and groups

---

## 10. What are the three editors available in UNIX?

* vi / vim
* ed
* ex

---

## 11. What are the three modes of operation of the vi editor?

### Command Mode

Used for navigation and editing commands.

### Insert Mode

Used for inserting text.

### Last Line Mode

Used for saving, quitting, and executing commands starting with `:`.

---

## 12. What is the alternative command available to `echo`?

`printf`

Example:

```bash
printf "Hello %s\n" Sanket
```

---

## 13. How to find the number of arguments passed to a script?

Use:

```bash
$#
```

Example:

```bash
echo "Total arguments: $#"
```

---

## 14. What are control instructions in shell scripting?

Control instructions manage the execution flow of a script.

Types:

* Decision control (if, if-else, case)
* Loop control (for, while, until)

---

## 15. What are loops?

Loops execute commands repeatedly.

### For loop

```bash
for i in 1 2 3
 do
 echo $i
 done
```

### While loop

```bash
while [ $i -le 5 ]
 do
 echo $i
 done
```

### Until loop

```bash
until [ $i -gt 5 ]
 do
 echo $i
 done
```

---

## 16. What is IFS?

IFS stands for **Internal Field Separator**. It is used by the shell to split input into fields.

Default separators:

* Space
* Tab
* Newline

---

## 17. What is the break statement?

`break` is used to terminate a loop immediately.

---

## 18. What is the continue statement?

`continue` skips the current iteration of a loop and continues with the next iteration.

---

## 19. What are metacharacters in shell?

Metacharacters are special characters used by the shell.

Examples:

| Symbol | Meaning                  |      |
| ------ | ------------------------ | ---- |
| `*`    | Multiple character match |      |
| `?`    | Single character match   |      |
| `>`    | Output redirection       |      |
| `<`    | Input redirection        |      |
| `      | `                        | Pipe |

---

## 20. How to execute multiple scripts?

### Sequential execution

```bash
./script1.sh
./script2.sh
```

### Using `;`

```bash
./script1.sh ; ./script2.sh
```

### Using `&&`

```bash
./script1.sh && ./script2.sh
```

### Parallel execution

```bash
./script1.sh &
./script2.sh &
```

---

## Author

Sanket Ajay Chopade
DevOps Engineer | Linux | AWS | Docker | Kubernetes
