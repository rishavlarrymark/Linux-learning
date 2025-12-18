## DAY 5 — Linux Processes & Signals
(ps, top, htop, nice, renice, kill, signals)

### Objective
- Understand Linux processes
- Monitor CPU and memory usage
- Control process priority
- Safely and forcefully terminate processes
- Learn Linux signals used in servers

---

### What is a Process?
A process is a running instance of a program in memory.

Each process has:
- PID (Process ID)
- User
- CPU usage
- Memory usage
- State
- Parent PID (PPID)

---

### ps — Process Status
Shows running processes.

ps
ps aux
ps aux | grep ssh
ps -p  -o pid,ppid,cmd

---

### top — Real-Time Process Monitor
Displays live CPU and memory usage.

top

Keys:
- q ? quit
- k ? kill
- P ? sort by CPU
- M ? sort by memory

---

### htop — Advanced Process Viewer
Interactive and user-friendly process monitor.

sudo apt install htop -y
htop

Keys:
- F9 ? kill
- F6 ? sort
- F5 ? tree view
- q ? quit

---

### nice — Set Priority When Starting Process
Controls CPU priority at process start.

nice -n 10 sleep 300

Priority range:
- -20 (highest)
- 19 (lowest)

---

### renice — Change Priority of Running Process
Changes priority of an existing process.

renice 5 <PID>
ps -o pid,ni,cmd -p <PID>

---

### kill — Send Signals to Process

Safe termination (SIGTERM):
kill <PID>
kill -15 <PID>

Force termination (SIGKILL):
kill -9 <PID>

---

### Linux Signals

Signal   Number   Meaning
SIGTERM  15       Graceful stop
SIGKILL  9        Force stop
SIGSTOP  19       Pause process
SIGCONT  18       Resume process
SIGHUP   1        Reload config

Pause / Resume example:
kill -19 <PID>
kill -18 <PID>

---

### Commands Summary
ps — list processes  
top — real-time monitor  
htop — advanced monitor  
nice — set process priority  
renice — change process priority  
kill — send signals  

---

### Status
? Process monitoring learned  
? Signals understood  
? Priority management practiced
