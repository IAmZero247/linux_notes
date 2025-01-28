# Process Temrination Using Signals And Kill

## Process life cycle

The life cycle of a process in an operating system is a critical concept. The provided diagram illustrates the various stages a process goes through:

```
# process life cycle
                        +---------+
                        |         |
                        | Created |
                        |         |
                        +----+----+
                             |
                             | Schedule
                             v
                        +----+----+
                        |         |
                        | Running |
               -------- |         | ----------
             /          +----+----+            \
           /  Error           |        Complete  \ 
          v                   |                   v
  +-------+-----+     +-------+-------+     +---------+
  |             |     |               |     |         |
  | Terminated  |<----| Interrupted   |<----|  Exit   |
  |             |     |               |     |         |
  +-------------+     +---------------+     +---------+
```

- When a process is **Created**, it enters the system and is allocated the necessary resources, but it has not yet started running.
- In the **Running** state, the process is actively executing instructions on the CPU.
- A process can be **Interrupted**, meaning it is temporarily stopped. This may occur to allow other processes to run or 
  because the process is waiting for an input or an event.
- The **Exit** state indicates that the process has completed its execution and is ready to be removed from the system.
- If a process is **Terminated**, it means that an error occurred or an explicit kill command was issued, 
  resulting in the process being forcefully stopped.

## Process Spawning

Process spawning is a fundamental aspect of any operating system. It's the action of creating a new process from an existing one. Whether it's running a command from a terminal or a process creating another, spawning processes is a routine operation in system workflows.

* The first process that initiates when a system boots up is assigned Process ID (PID) 1. 
* This initial process is known as `systemd` or `init`, based on the Linux distribution in use. 
* Being the first process, it acts as the parent to all subsequently spawned processes.

Spawning a process can be as straightforward as executing a command or running a program from the command line. For instance:

```bash
echo "Hello, world!"
```

This command creates a new process that executes the echo command, resulting in the specified text being printed on the terminal.

## Process Termination

To manage system resources effectively, it is sometimes necessary to terminate running processes. Depending on your needs, this can be accomplished in several ways.

### Terminating Processes by PID
=================================

Each process, upon its creation, is assigned a unique Process ID (PID). To stop a process using its PID, the `kill` command is used. For instance:

```bash
kill 12345
```

In this example, a termination signal is sent to the process with PID 12345, instructing it to stop execution.

### Terminating Processes by Name
==================================

If you don't know a process's PID, but you do know its name, the `pkill` command is handy. This command allows you to stop a process using its name:

```bash
pkill process_name
```

In this case, a termination signal is sent to all processes that share the specified name, effectively halting their execution.

### Specifying Termination Signals
===================================

The `kill` and `pkill` commands provide the option to specify the type of signal sent to a process. For example, to send a SIGINT signal (equivalent to Ctrl+C), you can use:

```bash
kill -SIGINT 12345

ex 
[root@localhost ~]# kill -19 16353
[root@localhost ~]# kill -18 16353
[root@localhost ~]# kill -15 16353
[root@localhost ~]# kill -9 16353
bash: kill: (16353) - No such process

```

Linux supports a variety of signals, each designed for a specific purpose. Some common signals include:
```
#kill -l 
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	

1-31   for user specific 
31-64  managed by os 
```
| Signal | Value | Description |
| --- | --- | --- |
| `SIGKILL` | (9)  | Forces immediate process termination; it cannot be ignored, blocked, or caught |
| `SIGTERM` | (15) | Forces immediate process termination; |
| `SIGSTOP` | (19) | Pauses the process; cannot be ignored |
| `SIGCONT` | (18) | Resumes paused process |

Terminating processes should be performed with caution. Some processes may be critical for system operation or hold valuable data that could be lost upon abrupt termination. 
Therefore, it's advisable to use commands such as `ps` and `top` to observe currently running processes before attempting to stop any of them. This approach allows for more 
controlled and careful system management, preventing unintended disruptions or data loss.

#### Special PID Values in `kill`

When using the `kill` command, special PID values can be used to target multiple processes:


These special values allow for more flexible management of processes and process groups, especially in scenarios where you need to signal multiple related processes at once.


```bash
# Sending SIGTERM to all processes the user can signal excluding the process itself and process ID 1 (the init proces)
kill -1 -SIGTERM

# Sending SIGTERM to all processes in the same process group as the current process [ group-wide signaling]
kill 0 -SIGTERM

# Sending SIGTERM to all processes in the process group with PGID 2
kill -2 -SIGTERM
```


## Methods for Searching Processes

In a Linux environment, various methods are available to search for processes, depending on the granularity of information you require or your personal preference. The search can be done using commands such as `ps`, `grep`, `pgrep`, and `htop`.

### Searching Processes with `ps` and `grep`

The `ps` command displays a list of currently running processes. To search for processes by name, you can combine `ps` with the `grep` command. The following command:

```bash
ps -ef | grep process_name


lab 
----

open 2 termainals
in 2nd terminal  #ping localhost

in 1st terminal 

#pidof ping 
16353

#ps -aux | grep ping
root        2651  0.0  0.3 598588 11868 tty2     Sl+  Jan27   0:04 /usr/libexec/gsd-housekeeping
root       16353  0.0  0.1 244480  3952 pts/1    S+   09:36   0:00 ping localhost
root       16362  0.0  0.0 222008  1176 pts/0    S+   09:37   0:00 grep --color=auto ping

to get details
#ps -P 16353
PID PSR TTY      STAT   TIME COMMAND
16353   2 pts/1    S+     0:00 ping localhost
```

searches and displays all processes containing the specified name (process_name in this example). Here, ps -ef lists all processes, and grep process_name filters the list to show only the processes with the specified name.

### Searching Processes with pgrep

The pgrep command offers a quick and direct method to search for processes by their names and display their PIDs. For instance, to find all running processes named chromium, you would use:

```bash
pgrep chromium

#pgrep ping 
16353

```

This command displays the PIDs for all instances of chromium currently running on the system.

### Searching Processes with htop

htop provides a real-time, interactive view of all running processes in a system. It's a robust tool for process management, offering capabilities like process searching, sorting, and termination.

To search for processes in htop:

1. Launch htop by typing `htop` in the terminal.
2. Once htop is open, press `F4` to activate the search bar.
3. Type the name of the process or the user you're searching for, then press Enter.
4. The list of processes will update to show only those matching your search criteria.

