# LOGS & SERVICES  —  Production Operational Scenarios

---

## `systemctl status <service>`

- **Situation:** Service not responding  
- **Symptom:** Users report downtime / connection refused  
- **Root cause:** Service crashed / failed to start / config error  
- **Fix:** Check status → identify error → move to logs  
- ⚠️ **Risk:** Restarting blindly may hide root cause  
---

## `systemctl restart <service>`

- **Situation:** Service stuck or unresponsive  
- **Symptom:** Requests timing out / partial failures  
- **Root cause:** Memory leak / hung process / stale state  
- **Fix:** Restart service to recover  
- ⚠️ **Risk:** Causes downtime, may impact users  
---

## `systemctl start <service>`

- **Situation:** Service is down  
- **Symptom:** “Connection refused” / port not listening  
- **Root cause:** Service never started / crashed earlier  
- **Fix:** Start service and verify status  
---

## `systemctl stop <service>`

- **Situation:** Need to safely stop service  
- **Symptom:** Maintenance / deployment required  
- **Root cause:** Planned shutdown  
- **Fix:** Stop service before changes  
- ⚠️ **Risk:** Immediate downtime  
---

## `journalctl -u <service>`

- **Situation:** Service failing or unstable  
- **Symptom:** Restart loops / unknown errors  
- **Root cause:** Config issues / dependency failure / permission error  
- **Fix:** Read logs → identify exact failure message  
---

## `journalctl -u <service> -n 50`

- **Situation:** Need recent failure logs  
- **Symptom:** Recent crash / deployment issue  
- **Root cause:** Latest config or runtime issue  
- **Fix:** Inspect last logs quickly for root cause  
---

## `journalctl -f`

- **Situation:** Debug live issue  
- **Symptom:** Issue occurs in real-time  
- **Root cause:** Unknown / intermittent  
- **Fix:** Follow logs while reproducing issue  
---

## `journalctl -b`

- **Situation:** System issue after reboot  
- **Symptom:** Services failing after boot  
- **Root cause:** Boot-time dependency or config issue  
- **Fix:** Check logs from last boot  
---

## `tail -f /var/log/syslog`

- **Situation:** System-level issue debugging  
- **Symptom:** Random failures / kernel or service errors  
- **Root cause:** System events / background services  
- **Fix:** Monitor logs live and correlate events  
---

## `less /var/log/syslog`

- **Situation:** Investigate historical logs  
- **Symptom:** Past errors / unknown failures  
- **Root cause:** Previous system events  
- **Fix:** Scroll safely and search logs  
---

## `grep <keyword> /var/log/syslog`

- **Situation:** Need specific error  
- **Symptom:** Large logs, hard to find issue  
- **Root cause:** Hidden error messages  
- **Fix:** Filter logs by keyword (e.g., nginx, error)  
---




# Day 12 — System Services & Scheduling (Linux)

## Objective
- Manage Linux services
- Inspect system logs
- Schedule recurring tasks

---

## Commands & Tools
- systemctl — manage system services
- journalctl — view service logs
- crontab — schedule time-based jobs
- systemd timers — reliable OS-level scheduling

---

## Key Concepts
- SSH is a systemd-managed service
- Logs are essential for debugging services
- Cron is simple but limited
- systemd timers are reboot-safe and logged

---

## Cron vs systemd Timers

| Cron | systemd Timers |
|----|----|
| Time-based | Event & time based |
| Minimal logs | journalctl logging |
| Basic | Production-grade |

---
