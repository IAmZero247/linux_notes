## Processes

1. Processes represent an instance of a running program.
 
	It also includes the current activity, including the program counter, the contents of the processor's registers, 
and the variable storage (stack and heap). Each process is assigned a unique process identifier (PID).

1. Modern computing systems, with their powerful processors, can handle a multitude of processes at the same time. 
	
	This parallel execution is managed through a technique known as rapid context switching, where the operating system's scheduler assigns 
CPU time slices to each process and switches between them so quickly that it gives the illusion of simultaneous execution.

1. In the context of multicore CPUs, each core can handle its own set of processes independently. This multi-core processing enables even 
more efficient and faster handling of multiple processes.

Processes in a system can be broadly categorized into two types:
================================================================

- A **Shell Job** refers to a process started from a user's command line interface or shell. 

- In contrast, a **Daemon** is a background process that usually starts at system boot and runs with elevated privileges. 
	
	Daemons do not interact directly with the user interface; instead, they operate silently in the background, handling various system-related tasks. 

## Process Management Commands

Process management is integral to system administration. The `ps` and `top` commands are pivotal for this purpose.

## The `ps` Command

The `ps` command displays information about active processes. Here are some variants of the command:

1. `ps` - present terminal process

```
#ps 

PID   TTY          TIME  CMD
4100  pts/0    	00:00:00 bash
11172 pts/0    	00:00:00 ps

```

2. `ps -A` or `ps -ef`

```
#ps -A

This outputs a list of all currently running processes, showing the process ID (PID), 
terminal associated with the process (TTY), CPU time (TIME), and the executable name (CMD).

PID TTY          TIME CMD
1   ?        00:00:12 systemd
2   ?        00:00:00 kthreadd
3   ?        00:00:00 rcu_gp
...

Below  command will give more details 
=====================================
#ps -ef 
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 07:06 ?        00:00:12 /usr/lib/systemd/systemd --switched-root --system --deserialize 17
root           2       0  0 07:06 ?        00:00:00 [kthreadd]
root           3       2  0 07:06 ?        00:00:00 [rcu_gp]
root           4       2  0 07:06 ?        00:00:00 [rcu_par_gp]
...	
```


3. Detailed Process Information ```ps -A --format uid,pid,ppid,%cpu,cmd```

```
#ps -A --format uid,pid,ppid,%cpu,cmd


This provides detailed information including User ID (UID), Process ID (PID), Parent Process ID (PPID), CPU usage (%CPU), and the command path (CMD).

   UID     PID    PPID %CPU CMD
    0       1       0  0.0 /usr/lib/systemd/systemd --switched-root --system --deserialize 17
    0       2       0  0.0 [kthreadd]
    0       3       2  0.0 [rcu_gp]
	...
```

4. To get user info `ps -u` 
```
#ps -u

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        2374  0.0  0.2 427692  7884 tty2     Ssl+ 07:07   0:00 /usr/libexec/gdm-wayland-session --register-session gnome-session
root        2380  0.0  0.5 1008012 19252 tty2    Sl+  07:07   0:00 /usr/libexec/gnome-session-binary
root        2429  1.0  9.2 4306576 342708 tty2   Sl+  07:07   8:16 /usr/bin/gnome-shell
```

5. More

```
-e	Displays information about all processes.						ps -e
-f	Provides full-format listing.								ps -f
-l	Displays long format listing.								ps -l
-j	Displays jobs format.									ps -j
-o	User-defined format.									ps -o pid,comm
-p	Select by PID.										ps -p 1234
-t	Select by TTY.										ps -t pts/1
-u	Select by effective user ID (EUID).							ps -u root
-x	List processes without controlling terminals (like daemons).				ps -x
-C	Select by command name.									ps -C bash
-L	Show threads, possibly with LWP and NLWP columns.					ps -L
```

6. WRT user `ps -u username`
```
#ps -u root 
PID TTY          TIME CMD
1 ?        00:00:12 systemd
2 ?        00:00:00 kthreadd
3 ?        00:00:00 rcu_gp
...
```



7. Enhanced Process Summaries `ps aux` & `ps fax`

```
#ps aux

This gives a condensed summary of all active processes, including details like user, PID, CPU, and memory usage.

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 169560  5892 ?        Ss   Apr10   0:01 /sbin/init
...
```




```
#ps fax


This displays the process hierarchy in a tree structure, showing parent-child relationships.

PID TTY      STAT   TIME COMMAND
  2 ?        S      0:00 [kthreadd]
/-+- 3951 ?        S      0:07  \_ [kworker/u8:2]
...
```

## The `pstree` Command
```
#pstree
systemd─┬─ModemManager───2*[{ModemManager}]
        ├─NetworkManager───2*[{NetworkManager}]
        ├─VGAuthService
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─alsactl
        ├─atd
        ├─auditd─┬─sedispatch
        │        └─2*[{auditd}]
        ├─avahi-daemon───avahi-daemon
        ├─bluetoothd
        ├─colord───2*[{colord}]
        ├─crond
        ├─cupsd
        ├─dbus-daemon───{dbus-daemon}
        ├─dnsmasq───dnsmasq
        ├─firewalld───{firewalld}
        ├─fprintd───4*[{fprintd}]
        ├─gdm─┬─gdm-session-wor─┬─gdm-wayland-ses─┬─gnome-session-b─┬─gnome-shell─┬─Xwayland
        │     │                 │                 │                 │             ├─ibus-daemon─┬─ibus-dconf───3*[{ibus-dconf}]
        │     │                 │                 │                 │             │             ├─ibus-engine-sim───2*[{ibus-engine-sim}]
        │     │                 │                 │                 │             │             ├─ibus-extension-───3*[{ibus-extension-}]
        │     │                 │                 │                 │             │             └─2*[{ibus-daemon}]
        │     │                 │                 │                 │             └─18*[{gnome-shell}]
        │     │                 │                 │                 ├─gnome-software───3*[{gnome-software}]
        │     │                 │                 │                 ├─gsd-a11y-settin───3*[{gsd-a11y-settin}]
        │     │                 │                 │                 ├─gsd-account───3*[{gsd-account}]
        │     │                 │                 │                 ├─gsd-clipboard───2*[{gsd-clipboard}]
        │     │                 │                 │                 ├─gsd-color───3*[{gsd-color}]
        │     │                 │                 │                 ├─gsd-datetime───3*[{gsd-datetime}]
        │     │                 │                 │                 ├─gsd-disk-utilit───2*[{gsd-disk-utilit}]
        │     │                 │                 │                 ├─gsd-housekeepin───3*[{gsd-housekeepin}]
        │     │                 │                 │                 ├─gsd-keyboard───3*[{gsd-keyboard}]
        │     │                 │                 │                 ├─gsd-media-keys───3*[{gsd-media-keys}]
        │     │                 │                 │                 ├─gsd-mouse───3*[{gsd-mouse}]
        │     │                 │                 │                 ├─gsd-power───3*[{gsd-power}]
        │     │                 │                 │                 ├─gsd-print-notif───2*[{gsd-print-notif}]
        │     │                 │                 │                 ├─gsd-rfkill───2*[{gsd-rfkill}]
        │     │                 │                 │                 ├─gsd-screensaver───2*[{gsd-screensaver}]
        │     │                 │                 │                 ├─gsd-sharing───3*[{gsd-sharing}]
        │     │                 │                 │                 ├─gsd-smartcard───5*[{gsd-smartcard}]
        │     │                 │                 │                 ├─gsd-sound───3*[{gsd-sound}]
        │     │                 │                 │                 ├─gsd-wacom───3*[{gsd-wacom}]
        │     │                 │                 │                 ├─gsd-xsettings───3*[{gsd-xsettings}]
        │     │                 │                 │                 ├─tracker-miner-a───4*[{tracker-miner-a}]
        │     │                 │                 │                 ├─tracker-miner-f───4*[{tracker-miner-f}]
        │     │                 │                 │                 └─3*[{gnome-session-b}]
        │     │                 │                 └─2*[{gdm-wayland-ses}]
        │     │                 └─3*[{gdm-session-wor}]
        │     └─2*[{gdm}]
        ├─geoclue───2*[{geoclue}]
        ├─gnome-keyring-d───3*[{gnome-keyring-d}]
        ├─gsd-printer───3*[{gsd-printer}]
        ├─gssproxy───5*[{gssproxy}]
        ├─ibus-x11───2*[{ibus-x11}]
        ├─irqbalance───{irqbalance}
        ├─ksmtuned───sleep
        ├─lsmd
        ├─mcelog
        ├─packagekitd───2*[{packagekitd}]
        ├─polkitd───7*[{polkitd}]
        ├─pulseaudio───2*[{pulseaudio}]
        ├─rpcbind
        ├─rsyslogd───2*[{rsyslogd}]
        ├─rtkit-daemon───2*[{rtkit-daemon}]
        ├─smartd
        ├─sshd
        ├─sssd_kcm
        ├─systemd─┬─(sd-pam)
        │         ├─at-spi-bus-laun─┬─dbus-daemon───{dbus-daemon}
        │         │                 └─3*[{at-spi-bus-laun}]
        │         ├─at-spi2-registr───2*[{at-spi2-registr}]
        │         ├─dbus-daemon───{dbus-daemon}
        │         ├─dconf-service───2*[{dconf-service}]
        │         ├─evolution-addre─┬─evolution-addre───5*[{evolution-addre}]
        │         │                 └─4*[{evolution-addre}]
        │         ├─evolution-calen─┬─evolution-calen───8*[{evolution-calen}]
        │         │                 └─4*[{evolution-calen}]
        │         ├─evolution-sourc───3*[{evolution-sourc}]
        │         ├─gnome-shell-cal───5*[{gnome-shell-cal}]
        │         ├─gnome-terminal-─┬─bash───pstree
        │         │                 └─3*[{gnome-terminal-}]
        │         ├─goa-daemon───3*[{goa-daemon}]
        │         ├─goa-identity-se───3*[{goa-identity-se}]
        │         ├─gvfs-afc-volume───3*[{gvfs-afc-volume}]
        │         ├─gvfs-goa-volume───2*[{gvfs-goa-volume}]
        │         ├─gvfs-gphoto2-vo───2*[{gvfs-gphoto2-vo}]
        │         ├─gvfs-mtp-volume───2*[{gvfs-mtp-volume}]
        │         ├─gvfs-udisks2-vo───3*[{gvfs-udisks2-vo}]
        │         ├─gvfsd───2*[{gvfsd}]
        │         ├─gvfsd-fuse───5*[{gvfsd-fuse}]
        │         ├─gvfsd-metadata───2*[{gvfsd-metadata}]
        │         ├─ibus-portal───2*[{ibus-portal}]
        │         ├─tracker-store───4*[{tracker-store}]
        │         └─xdg-permission-───2*[{xdg-permission-}]
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-machine
        ├─systemd-udevd
        ├─tuned───4*[{tuned}]
        ├─udisksd───4*[{udisksd}]
        ├─upowerd───2*[{upowerd}]
        ├─2*[vmtoolsd───3*[{vmtoolsd}]]
        ├─vmware-vmblock-───2*[{vmware-vmblock-}]
        └─wpa_supplicant

```

## The `top` Command

For real-time process monitoring, top command is used. Press Z to set different color to each pages

	This shows a dynamic list of processes, usually sorted by CPU usage. It includes system summary information (like CPU and memory usage) 
and a detailed list of processes.

	TO navigate from one page to another `Shift+G`


```
#top

top - 21:13:10 up 14:06,  2 users,  load average: 0.06, 0.10, 0.09
Tasks: 245 total,   1 running, 244 sleeping,   0 stopped,   0 zombie
%Cpu(s):  6.1 us,  2.1 sy,  0.0 ni, 90.5 id,  0.3 wa,  0.8 hi,  0.1 si,  0.0 st
MiB Mem :   3633.7 total,   1865.5 free,   1002.3 used,    765.9 buff/cache
MiB Swap:   3072.0 total,   3072.0 free,      0.0 used.   2372.7 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                                                     
   2429 root      20   0 4306636 340848 129892 S  27.7   9.2  10:52.76 gnome-shell                                                                                                                                 
   4095 root      20   0  748532  52104  36132 S   2.0   1.4   0:30.62 gnome-terminal-                                                                                                                             
  12193 root      20   0  264312   4500   3568 R   1.3   0.1   0:00.52 top                                                                                                                                         
    911 root      20   0  636576  11680   9768 S   0.7   0.3   4:32.07 vmtoolsd                                                                                                                                    
   1102 root      20   0  696204  33164  16696 S   0.7   0.9   4:25.54 tuned                                                                                                                                       
   2786 root      20   0  732796  38840  31716 S   0.3   1.0   4:19.59 vmtoolsd                                                                                                                                    
  10981 root      20   0  400976  40056   8292 S   0.3   1.1   0:06.84 sssd_kcm         
...
```

Shift+F

```
ields Management for window 1:Def, whose current sort field is %CPU
   Navigate with Up/Dn, Right selects for move then <Enter> or Left commits,
   'd' or <Space> toggles display, 's' sets sort.  Use 'q' or <Esc> to end!

* PID     = Process Id             PGRP    = Process Group Id       OOMs    = OOMEM Score current 
* USER    = Effective User Name    TTY     = Controlling Tty        ENVIRON = Environment vars    
* PR      = Priority               TPGID   = Tty Process Grp Id     vMj     = Major Faults delta  
* NI      = Nice Value             SID     = Session Id             vMn     = Minor Faults delta  
* VIRT    = Virtual Image (KiB)    nTH     = Number of Threads      USED    = Res+Swap Size (KiB) 
* RES     = Resident Size (KiB)    P       = Last Used Cpu (SMP)    nsIPC   = IPC namespace Inode 
* SHR     = Shared Memory (KiB)    TIME    = CPU Time               nsMNT   = MNT namespace Inode 
* S       = Process Status         SWAP    = Swapped Size (KiB)     nsNET   = NET namespace Inode 
* %CPU    = CPU Usage              CODE    = Code Size (KiB)        nsPID   = PID namespace Inode 
* %MEM    = Memory Usage (RES)     DATA    = Data+Stack (KiB)       nsUSER  = USER namespace Inode
* TIME+   = CPU Time, hundredths   nMaj    = Major Page Faults      nsUTS   = UTS namespace Inode 
* COMMAND = Command Name/Line      nMin    = Minor Page Faults      LXC     = LXC container name  
  PPID    = Parent Process pid     nDRT    = Dirty Pages Count      RSan    = RES Anonymous (KiB) 
  UID     = Effective User Id      WCHAN   = Sleeping in Function   RSfd    = RES File-based (KiB)
  RUID    = Real User Id           Flags   = Task Flags <sched.h>   RSlk    = RES Locked (KiB)    
  RUSER   = Real User Name         CGROUPS = Control Groups         RSsh    = RES Shared (KiB)    
  SUID    = Saved User Id          SUPGIDS = Supp Groups IDs        CGNAME  = Control Group name  
  SUSER   = Saved User Name        SUPGRPS = Supp Groups Names      NU      = Last Used NUMA node 
  GID     = Group Id               TGID    = Thread Group Id     
  GROUP   = Group Name             OOMa    = OOMEM Adjustment 
```

