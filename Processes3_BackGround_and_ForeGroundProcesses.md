# Foreground and Background Jobs

The tasks running on your system can be in one of two states, either running in the 'foreground' or in the 'background'. These two states provide flexibility for multi-tasking and efficient system utilization.

- When a process is running as a **Foreground Process**, it actively executes and interacts with the terminal's input and output. These are typically tasks that the user has initiated and is directly interacting with in the terminal.
- In contrast, a **Background Process** operates without direct interaction with the terminal's input and output. This functionality enables users to run multiple processes at the same time, allowing them to initiate new tasks without waiting for the completion of others.

```
+------------------------+
|                        |
| Start Job in Foreground|
|                        |
+-----------+------------+
            |
            | Ctrl+Z or bg
            v
+-----------+------------+
|                        |
|   Job Running in       |
|   Background           |
|                        |
+-----------+------------+
            |
            | fg or job completes
            v
+-----------+------------+
|                        |
|   Job Completes        |
|   or Returns to        |
|   Foreground           |
|                        |
+------------------------+
```

## Controlling Job Execution

You can direct a program to run in the background right from its initiation by appending an ampersand `&` operator after the command. Consider the following example:

```bash
sleep 1000 &
```

Here, the sleep command will operate in the background, waiting for 1000 seconds before terminating. The output typically resembles this:

```bash
[1] 3241
```

The number enclosed in square brackets (like `[1]`) represents the job number, while the subsequent number (like 3241) is the process ID (PID).

## Job and Process Management Commands

To view all active jobs in the system, use the `jobs` command.

If you wish to bring a background job to the foreground, utilize the fg command followed by the job number. For instance, to bring job number 3 to the foreground, you would use:

```bash
fg 3
```

To convert a foreground process to a background process, you can use Ctrl+Z. This operation pauses the process and allows you to resume it in the background using the bg command followed by the job number. For instance:

```bash
bg 1
```

This command will resume job number 1 in the background. Understanding and managing foreground and background jobs helps to increase productivity and efficiency when working in a shell environment.





## Lab-Challenges

1. Find the process ID (PID) of a specific process by its name.
2. List all processes that belong to a specific user.
3. Start a process in the background, bring it to the foreground, and then move it back to the background.
4. Terminate a process using both its PID and its name.
5. Use the `top` command to identify the process with the highest CPU usage and terminate it.
6. Pause a running process using the appropriate signal and then resume it.
7. Find the parent process ID (PPID) of a specific process.
8. Explain the difference between a shell job and a daemon process.
9. Use the `htop` command to filter processes by a specific string and then sort them by memory usage.
