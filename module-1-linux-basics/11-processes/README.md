# Task 11 — Processes and Performance Monitoring

## Objective

The purpose of this task is to understand Linux processes, process monitoring, process termination, and process scheduling priority.

The following commands were practiced:

* `ps`
* `top`
* `htop`
* `pgrep`
* `kill`
* `pkill`
* `nice`
* `renice`

---

## 1. Process Identification

A safe test process was created using:

```bash
sleep 300 &
```

The process PID was identified and inspected using:

```bash
ps -p <PID> -f
```

Custom process information was displayed using:

```bash
ps -p <PID> -o pid,ppid,user,%cpu,%mem,stat,cmd
```

The following information was investigated:

* PID
* Parent PID (PPID)
* Process owner
* CPU usage
* Memory usage
* Process state
* Process command

---

## 2. Process Monitoring

### `ps`

`ps` provides a snapshot of currently running processes.

```bash
ps
```

A specific process can be inspected with:

```bash
ps -p <PID> -f
```

### `top`

`top` provides continuously updated, real-time information about running processes.

```bash
top
```

It can be used to monitor:

* CPU usage
* Memory usage
* Process IDs
* Process states
* Process priorities

Press `q` to exit.

### `htop`

`htop` is an interactive process monitoring tool that provides a more user-friendly interface for viewing processes.

```bash
htop
```

Press `q` to exit.

---

## 3. Finding Processes

Multiple safe `sleep` processes were started:

```bash
sleep 300 &
sleep 300 &
sleep 300 &
```

Processes were located using:

```bash
pgrep sleep
```

The `pgrep` command searches for processes by name or pattern.

---

## 4. Terminating Processes

### Terminating by PID

A specific process was terminated using:

```bash
kill <PID>
```

`kill` normally sends `SIGTERM`, requesting the process to terminate gracefully.

### Terminating by Process Name

Multiple test processes were terminated using:

```bash
pkill sleep
```

`pkill` searches for matching processes and sends a signal to them.

Only safe test processes created during this exercise were terminated.

---

## 5. Process Priority

A process was started with a modified nice value:

```bash
nice -n 10 sleep 300 &
```

Its priority information was checked with:

```bash
ps -p <PID> -o pid,ni,pri,stat,cmd
```

The `NI` column represents the nice value.

A higher nice value generally means the process is given less preference for CPU scheduling compared with processes having lower nice values.

---

## 6. Changing Priority with `renice`

An existing test process was changed from nice value `10` to `15`:

```bash
renice 15 -p <PID>
```

The result was verified using:

```bash
ps -p <PID> -o ni,pri
```

### Difference

```text
nice
→ Starts a new process with a specified nice value.

renice
→ Changes the nice value of an existing process.
```

---

## 7. Important Concepts

### PID

PID stands for **Process ID**.

It is the unique numerical identifier assigned to a running process.

### Parent Process

A parent process is the process that creates another process.

The parent process ID is shown by `PPID`.

### Zombie Process

A zombie process is a process that has finished execution but whose parent has not yet collected its exit status.

Its process state is represented by `Z`.

### SIGTERM

`SIGTERM` is signal `15`.

It requests that a process terminate gracefully and gives the application an opportunity to perform cleanup.

### SIGKILL

`SIGKILL` is signal `9`.

It forces a process to terminate immediately and cannot be caught or ignored by the process.

### Why `kill -9` should not normally be the first choice

`kill -9` should normally be avoided as the first option because the process does not get an opportunity to perform normal cleanup.

The preferred approach is generally:

```text
SIGTERM → wait/check → SIGKILL if necessary
```

---

## 8. `kill`, `pkill`, and `killall`

| Command          | Purpose                                      |
| ---------------- | -------------------------------------------- |
| `kill <PID>`     | Targets a specific process by PID            |
| `pkill <name>`   | Targets processes matching a name or pattern |
| `killall <name>` | Targets processes by process name            |

Before using name-based termination commands, processes should be inspected to make sure the correct processes will be affected.

Example:

```bash
pgrep -a sleep
```

---

## 9. Process Priority and CPU Scheduling

Process priority influences how the Linux scheduler allocates CPU time between processes.

A process with a higher nice value is generally less favored for CPU scheduling.

This can be useful for background tasks that should consume CPU without interfering heavily with more important workloads.

---

## Result

Task 11 demonstrated practical process management and monitoring using safe `sleep` processes. Process identification, monitoring, termination, process searching, and CPU scheduling priority were all practiced.
