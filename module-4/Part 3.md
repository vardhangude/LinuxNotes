# Module 4 – Linux Interview Handbook (Part 3)
## Pipes | Redirection | tee | xargs
### Target: 4+ Years Linux Administrator | DevOps Engineer | Cloud Engineer

> This section focuses on how Linux commands work together. In real-world DevOps and Linux administration, you'll rarely use commands in isolation. You'll combine them using pipes and redirection to analyze logs, automate tasks, and troubleshoot systems efficiently.

---

# Table of Contents

1. Pipes (`|`)
2. Input and Output Redirection
3. tee
4. xargs
5. Combining Commands
6. Production Scenarios
7. Best Practices
8. Interview Questions

---

# 1. Pipes ( | )

## Interview Question

### What is a Pipe in Linux?

### Answer

A pipe (`|`) connects the output of one command directly to the input of another command.

Instead of saving output into a temporary file, pipes allow commands to work together efficiently.

Syntax

```bash
command1 | command2
```

Example

```bash
ps -ef | grep nginx
```

Explanation

- `ps -ef` lists all running processes.
- `grep nginx` filters only the nginx process.

---

## Interview Follow-up

### Why do we use pipes?

Pipes help us:

- Filter command output
- Reduce manual work
- Build powerful command chains
- Process large log files efficiently

---

## Production Example

Check if Jenkins is running.

```bash
ps -ef | grep jenkins
```

Check Docker containers.

```bash
docker ps | grep nginx
```

Check failed login attempts.

```bash
cat /var/log/secure | grep Failed
```

---

## Multiple Pipes

Commands can be chained together.

Example

```bash
ps -ef | grep java | grep -v grep
```

Explanation

1. List all processes.
2. Filter Java processes.
3. Exclude the grep command itself.

---

## Production Scenario

Find the top five memory-consuming processes.

```bash
ps aux | sort -nrk4 | head -5
```

Explanation

- List all processes.
- Sort by memory usage.
- Display top five.

---

# 2. Redirection

## Interview Question

### What is Redirection?

### Answer

Redirection changes where command input or output goes.

Instead of displaying output on the terminal, it can be saved into a file.

---

## Output Redirection (>)

Overwrite a file.

```bash
date > output.txt
```

Output

```
Current date stored inside output.txt
```

If the file exists, its contents are replaced.

---

## Append Output (>>)

Append data instead of replacing.

```bash
date >> output.txt
```

Useful for log files.

---

## Input Redirection (<)

Read input from a file.

```bash
sort < names.txt
```

---

## Redirect Errors (2>)

Store only error messages.

```bash
ls invalidfile 2> error.log
```

---

## Append Errors

```bash
ls invalidfile 2>> error.log
```

---

## Redirect Standard Output and Error

```bash
command > output.log 2> error.log
```

Both separately.

---

## Redirect Everything

```bash
command &> result.log
```

or

```bash
command > result.log 2>&1
```

This stores both standard output and errors.

---

## Interview Follow-up

Difference between

```bash
>
```

and

```bash
>>
```

Answer

```
>

Overwrite

>>

Append
```

---

# Production Scenario

Application deployment output needs to be saved.

```bash
./deploy.sh > deploy.log 2>&1
```

Benefits

- Output saved.
- Errors saved.
- Easy troubleshooting.

---

# 3. tee Command

## Interview Question

### Explain tee.

### Answer

tee displays command output on the terminal and simultaneously writes it into a file.

Syntax

```bash
command | tee file
```

Example

```bash
df -h | tee disk_report.txt
```

Output

Displayed on terminal

AND

Saved to

```
disk_report.txt
```

---

## Append using tee

```bash
df -h | tee -a disk_report.txt
```

---

## Production Scenario

Generate deployment logs.

```bash
./deploy.sh | tee deployment.log
```

Engineers can monitor progress live while keeping a permanent log.

---

# 4. xargs

## Interview Question

### What is xargs?

### Answer

xargs converts standard input into command-line arguments.

It allows one command's output to become arguments for another command.

Syntax

```bash
command | xargs another_command
```

---

## Example

```bash
echo "file1 file2" | xargs rm
```

Equivalent to

```bash
rm file1 file2
```

---

## Production Example

Delete old log files.

```bash
find /logs -name "*.log" | xargs rm
```

---

Another example

```bash
find . -name "*.tmp" | xargs ls -l
```

---

## Safe Alternative

When filenames contain spaces, use

```bash
find . -print0 | xargs -0 rm
```

---

## Interview Follow-up

Why use xargs?

Because many commands produce output that needs to become input arguments for another command.

---

# Combining Commands

## Scenario 1

Display top memory-consuming Java processes.

```bash
ps aux | grep java | sort -nrk4 | head
```

---

## Scenario 2

Find failed SSH logins.

```bash
grep Failed /var/log/secure | wc -l
```

---

## Scenario 3

Save disk report.

```bash
df -h | tee disk_usage.txt
```

---

## Scenario 4

Find old backup files and delete them.

```bash
find /backup -name "*.bak" | xargs rm
```

---

## Scenario 5

Display running Docker containers.

```bash
docker ps | grep Up
```

---

## Scenario 6

Monitor application logs.

```bash
tail -f app.log | grep ERROR
```

---

# Troubleshooting Scenario

## Interview Question

Application team says the server is slow.

How would you investigate?

### Answer

Step 1

Check CPU usage.

```bash
top
```

Step 2

Check memory.

```bash
free -m
```

Step 3

Check disk usage.

```bash
df -h
```

Step 4

Check processes.

```bash
ps aux | sort -nrk4 | head
```

Step 5

Search application logs.

```bash
grep ERROR app.log
```

Step 6

Save results.

```bash
grep ERROR app.log | tee errors.txt
```

This approach provides both live visibility and a saved report for later analysis.

---

# Best Practices

✅ Prefer pipes over creating unnecessary temporary files.

✅ Use `tee` when you need to monitor output and save it simultaneously.

✅ Always redirect errors during deployments.

```bash
2>&1
```

✅ Test `xargs rm` with `echo` first to avoid accidental deletion.

Example

```bash
find . -name "*.tmp" | xargs echo rm
```

If the output is correct, remove `echo`.

---

# Common Mistakes

❌ Using

```bash
>
```

when intending to append.

❌ Running

```bash
find / | xargs rm
```

without verification.

❌ Forgetting to capture errors.

```bash
2>error.log
```

---

# Top Interview Questions

1. What is a pipe?
2. Difference between `>` and `>>`.
3. Difference between `>` and `2>`.
4. What is `2>&1`?
5. Explain `tee`.
6. Difference between `tee` and `>`.
7. Explain `xargs`.
8. Why use `xargs` with `find`?
9. Why use `xargs -0`?
10. How do you save command output and display it simultaneously?
11. How do you redirect only error messages?
12. How do you redirect both output and errors?
13. How do you monitor logs in real time?
14. Explain a production use case for pipes.
15. Explain a production use case for `tee`.

---

# Quick Revision

| Command | Purpose |
|----------|---------|
| `|` | Pass output to another command |
| `>` | Overwrite output |
| `>>` | Append output |
| `<` | Read input from file |
| `2>` | Redirect errors |
| `2>>` | Append errors |
| `2>&1` | Combine stdout and stderr |
| `&>` | Redirect both output and errors |
| `tee` | Display and save output |
| `tee -a` | Append while displaying |
| `xargs` | Convert input into command arguments |

---

## Part 3 Complete ✅

**Next Part:** `sort`, `uniq`, `wc`, `tr`, File Maintenance Commands (`cp`, `mv`, `rm`, `mkdir`, `rmdir`, `touch`), File Display Commands (`cat`, `less`, `more`, `head`, `tail`), and real-world troubleshooting scenarios.
