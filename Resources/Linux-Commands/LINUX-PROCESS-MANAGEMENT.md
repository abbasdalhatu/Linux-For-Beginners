
# LINUX PROCESS MANAGEMENT

## 1. What Is a Process?

A process is a program that is currently running on a computer.

Examples:
- A running web browser.
- A terminal window.
- A background system service.

Every process has a unique Process ID (PID).

## 2. View Running Processes

### Display processes
```bash
ps
```

### Display all processes
```bash
ps aux
```

### Display processes in real time
```bash
top
```

### Search for a process
```bash
pgrep firefox
```

## 3. Understanding Process Information

The command `ps aux` displays several columns:

- USER: Account running the process.
- PID: Process identification number.
- %CPU: CPU usage.
- %MEM: Memory usage.
- STAT: Process status.
- COMMAND: Program being executed.

## 4. Stop a Process

### Stop using its PID
```bash
kill 1234
```

Replace `1234` with the actual PID.

### Forcefully terminate a process
```bash
kill -9 1234
```

Use force termination only when normal termination fails.

### Stop using its name
```bash
pkill firefox
```

Be careful: this may stop multiple matching processes.

## 5. Process Priorities

Linux uses priority values called nice values.

- Lower nice value = higher priority.
- Higher nice value = lower priority.

### Start a process with a nice value
```bash
nice -n 10 command
```

### Change the priority of a running process
```bash
renice 5 -p 1234
```

## 6. Background and Foreground Processes

### Run a command in the background
```bash
command &
```

### Display background jobs
```bash
jobs
```

### Bring a job to the foreground
```bash
fg
```

### Continue a stopped job in the background
```bash
bg
```

## 7. Process Monitoring Tools

| Command | Purpose |
|---|---|
| ps aux | Lists running processes |
| top | Live process monitoring |
| htop | Interactive process monitoring |
| pgrep | Searches for process IDs |
| pidof | Finds a program's PID |
| kill | Sends a signal to a process |
| uptime | Displays system uptime and load |

## 8. Important Signals

| Signal | Number | Purpose |
|---|---:|---|
| SIGTERM | 15 | Requests a process to terminate |
| SIGKILL | 9 | Forcefully terminates a process |
| SIGSTOP | 19 | Stops a process |
| SIGCONT | 18 | Continues a stopped process |

Example:
```bash
kill -15 1234
```

## 9. Cybersecurity Applications

Process management helps security professionals:

- Identify suspicious running programs.
- Investigate unusual CPU or memory usage.
- Monitor background services.
- Identify unauthorized processes.
- Stop malicious processes during incident response.

Important: Do not terminate unfamiliar system processes without investigating them first.

## 10. Practice Exercises

1. Display all running processes.
2. Open the process monitor.
3. Find the PID of a running application.
4. Explain the difference between SIGTERM and SIGKILL.
5. Explain what a background process is.

## Summary

Process management is an important Linux administration skill. It helps users monitor programs, control execution, manage system resources, and investigate suspicious activity.
