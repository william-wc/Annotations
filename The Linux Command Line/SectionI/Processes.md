# Processes

[Prev](Permissions.md) | [Next]()

Modern operating systems are usually *multitasking*, creating a illusion of doing more than one thing simultaneously by rapidly switching from one executing program to another
The Linux kernel manages this through the use of *processes*, these are how Linux organizes the different programs waiting for their turn at the CPU

`ps` - Report a snapshot of current processes
`top` - Display tasks
`jobs` - List active jobs
`bg` - Place a job in the background
`fg` - Place a job in the foreground
`kill` - Send a signal to a process
`killall` - Kill processes by name
`shutdown` - Shut down or reboot the system

## How a Process Works

When a system starts up, the kernel initiates a few of it's own activities as processes and launches a program called *init*
It returns a series of shell scripts (located in `/etc`) called *init scripts* which start all the system services

Many of these services are implemented as *daemon programs*, they sit in the background without any user interface

> A program can launch other programs, it is expressed as a *parent process* producing a *child process*

Kernel maintains information about each process assigning a *process ID (PID)*, in ascending order. Tracks memory assigned and readiness to resume execution. Like files, processes have owners and user IDs and so on

### `ps` - Viewing Processes

The most commonly used command to view processes

```shell
$ ps
  PID TTY       TIME CMD
 5198 pts/1 00:00:00 bash
10129 pts/1 00:00:00 ps
```

Two processes `5198` and `10129`
`TTY` is short for *teletype* and refers to the *controlling terminal* of the process
`TIME` field is the amount of CPU time consumed

```shell
$ ps x
  PID TTY STAT   TIME COMMAND
 2799 ?   Ssl    0:00 /usr/libexec/bonobo-activation-server –ac
 2820 ?   Sl     0:01 /usr/libexec/evolution-data-server-1.10 --
15647 ?   Ss     0:00 /bin/sh /usr/bin/startkde
15751 ?   Ss     0:00 /usr/bin/ssh-agent /usr/bin/dbus-launch --
15754 ?   S      0:00 /usr/bin/dbus-launch --exit-with-session
15755 ?   Ss     0:01 /bin/dbus-daemon --fork --print-pid 4 –pr
15774 ?   Ss     0:02 /usr/bin/gpg-agent -s –daemon
15793 ?   S      0:00 start_kdeinit --new-startup +kcminit_start
15794 ?   Ss     0:00 kdeinit Running...
15797 ?   S      0:00 dcopserver –nosid
(...)
```

`STAT` is short for *state*

| State | Meaning |
|:------|:--------|
| R | Running or ready to run |
| S | Sleeping / Not running / waiting for an event |
| D | Uninterruptible sleep, waiting for I/O |
| T | Stopped |
| Z | Zombie process, a child that has terminated but has not been cleaned up by it's parent |
| < | High priority process, this property is called *niceness*, it is less nice since it leaves less CPU time for others |
| N | Low priority process, it is nice since it gets processor time only after other processes |

#### BSD-Style ps Column Headers

| Header | Meaning |
|:-------|:--------|
| USER  | User ID, the owner of the process |
| %CPU  | CPU usage as percent |
| $MEM  | Memory usage as a percent |
| VSZ   | Virtual memory size |
| RSS   | Resident Sent Size, amount of physical memory (RAM) it is using in kilobytes |
| START | Time when the process started, values over 24h a date is used |

### `top` - Viewing Processes Dynamically

The `top` program displays a continuously updating (by default every 3 seconds) display of the system processes listed in order of process activity
Consists of two parts: A system summary at the top of the display, followed by a table of processes sorted by CPU activity

`top` accepts a number of keyboard commands, the two most interesting are `h` (help screen) and `q` (quit `top`)

---

## Controlling Processes

### Interrupt a process in forground

To easily interrupt a process in **foreground** use CTRL-C

### Send a process to background

To send a process to **background** execute a command ending with `&`
`$ xlogo &`
It's possible to see the process running by using `ps` or `jobs`

### Send a process to foreground

To return a process to the **foreground** use `fg` followed by the running job number

```shell
$ jobs
[1]+ Running xlogo &
$ fg %1
xlogo
```

### Stop/Pause a process

To stop a process without terminating it, usually done to allow a foreground process to be moved to the background, use CTRL-Z

```shell
$ xlogo
[1]+ Stopped xlogo
```

To restore either put it to the **foreground** with `fg` or to the **background** with `bg`

```shell
$ bg %1
[1]+ xlogo &
```

---

## Signals

The `kill` command is used to terminate processes, which could be behaving badly or refusing to terminate on its own

```shell
$ xlogo &
[1] 28401
$ kill 28401
[1]+ Terminated    xlogo
```

It's also possible to specify `jobspec (%1)` instead of `PID (28041)`

### Sending Signals to Processes with `kill`

> `kill [-signal] PID...`

If no signal is specified on the command line, then the `TERM` (Terminate) signal is sent by default

#### Common Signals

| Number | Name | Meaning |
|:-------|:-----|:--------|
| 1     | HUP   | **Hang up**. This signal is used by many daemon programs to cause a reinitialization |
| 2     | INT   | **Interrupt**. Same function as CTRL-C, usualy terminate a program |
| 3     | QUIT  | **Quit** |
| 9     | KILL  | **Kill**. This signal is special, this one is never actually sent to the target program. The kernel immediately terminates the process, gives no opportunity to cleanup |
| 11    | SEGV  | Segmentation violation. This signal is sent if a program makes illegal use of memory (tried to write somewhere it was not allowed to) |
| 15    | TERM  | **Terminate**. This is the default signal sent by the `kill` command. If a program is still alive enough to receive signals, it will terminate |
| 18    | CONT  | **Continue**. This will restore a process after a `STOP` signal |
| 19    | STOP  | **Stop**. This signal causes a process to pause without terminating. Like `KILL` it is not sent to the target process |
| 20    | TSTP  | **Terminal stop**. Same as CTRL-Z. Unlike `STOP` this signal is received by the program |
| 28    | WINCH | **Window change**. Sent by the system when a window changes size |

Example use
`$ kill -1 PID`
`$ kill -INT PID`
`$ kill -SIGINT PID`

`kill -l` <- lists all signals

### Sending Signals to Multiple Processes with `killall`

> `killall [-u user] [-signal] name...`

```shell
$ xlogo &
[1] 18801
$ xlogo &
[2] 18802
$ killall xlogo
[1]-    Terminated  xlogo
[2]+    Terminated  xlogo
```

### More Process-Related Commands

| Command | Description |
|:--------|:------------|
| pstree | Outputs a process list arranged in a tree-like pattern showing the parent/child relationships |
| vmstat | Outputs a snapshot of system resource usage including memory, swap and disk I/O |
| xload  | A graphical program that draws a graph showing system load over time |
| tload  | Similar to xload but draws the graph in the terminal |

[Prev](Permissions.md) | [Next]()