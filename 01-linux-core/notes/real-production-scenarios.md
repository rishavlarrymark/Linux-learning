# REAL PRODUCTION SCENARIOS  —  Production Operational Scenarios

---

## `hostnamectl`

- **Situation:** Wrong server accessed during production incident  
- **Symptom:** Commands executed on incorrect environment  
- **Root cause:** Host identity not verified  
- **Fix:**  
  ```bash
  hostnamectl
  ```

---

## `dmesg`

- **Situation:** Server freezing randomly under load  
- **Symptom:** Application hangs and system instability  
- **Root cause:** Kernel/hardware/storage-related errors  
- **Fix:**  
  ```bash
  dmesg
  ```

---

## `ulimit -a`

- **Situation:** Application throwing “Too many open files”  
- **Symptom:** Connections/files failing unexpectedly  
- **Root cause:** Process resource limits exhausted  
- **Fix:**  
  ```bash
  ulimit -a
  ```

---

## `rsync`

- **Situation:** Production backup/sync required  
- **Symptom:** Files missing between source and backup server  
- **Root cause:** Incomplete/manual copy operations  
- **Fix:**  
  ```bash
  rsync -av source/ backup/
  ```

---

## `df -h`

- **Situation:** Application unable to write data  
- **Symptom:** Disk usage alerts firing  
- **Root cause:** Filesystem reaching full capacity  
- **Fix:**  
  ```bash
  df -h
  ```

---

## `du -sh`

- **Situation:** Need to identify storage-consuming directories  
- **Symptom:** `/var` or `/` growing rapidly  
- **Root cause:** Logs/artifacts/backups consuming storage  
- **Fix:**  
  ```bash
  du -sh /var/*
  ```

---

## `find -size`

- **Situation:** Large files causing disk exhaustion  
- **Symptom:** Filesystem usage increasing abnormally  
- **Root cause:** Oversized logs/dumps/artifacts  
- **Fix:**  
  ```bash
  find /var/log -size +100M
  ```

---

## `lsof | grep deleted`

- **Situation:** Disk still full after deleting logs  
- **Symptom:** `df` high but files not visible  
- **Root cause:** Deleted files still held by processes  
- **Fix:**  
  ```bash
  lsof | grep deleted
  ```

---

## `systemctl status`

- **Situation:** Website/API unavailable  
- **Symptom:** Service failing or inactive  
- **Root cause:** Crash/config/startup failure  
- **Fix:**  
  ```bash
  systemctl status nginx
  ```

---

## `journalctl -u`

- **Situation:** Need root cause of service failure  
- **Symptom:** Service repeatedly crashing/restarting  
- **Root cause:** Config/runtime/dependency errors  
- **Fix:**  
  ```bash
  journalctl -u nginx -n 50
  ```

---

## `ss -tulnp`

- **Situation:** Application unreachable remotely  
- **Symptom:** Port connection refused/timed out  
- **Root cause:** Service not listening/bound incorrectly  
- **Fix:**  
  ```bash
  ss -tulnp
  ```

---

## `curl localhost`

- **Situation:** Need local vs remote isolation  
- **Symptom:** App inaccessible externally  
- **Root cause:** Firewall/load balancer/network issue  
- **Fix:**  
  ```bash
  curl localhost:80
  ```

---

## `ls -l`

- **Situation:** Application suddenly gets permission denied  
- **Symptom:** File access failures  
- **Root cause:** Incorrect ownership/permission bits  
- **Fix:**  
  ```bash
  ls -l file
  ```

---

## `ls -ld`

- **Situation:** File permissions look correct but access still fails  
- **Symptom:** Cannot enter/access directory  
- **Root cause:** Directory execute permission missing  
- **Fix:**  
  ```bash
  ls -ld directory
  ```

---

## `stat`

- **Situation:** Need detailed file metadata during investigation  
- **Symptom:** Unexpected permission/timestamp behavior  
- **Root cause:** Metadata inconsistencies  
- **Fix:**  
  ```bash
  stat file
  ```

---

## `id`

- **Situation:** User-specific access issue  
- **Symptom:** App/service user denied access  
- **Root cause:** Missing group membership/incorrect UID  
- **Fix:**  
  ```bash
  id appuser
  ```

---

## `ip a`

- **Situation:** Network connectivity failure  
- **Symptom:** Server unreachable  
- **Root cause:** Interface down/wrong IP assignment  
- **Fix:**  
  ```bash
  ip a
  ```

---

## `ip r`

- **Situation:** Outbound traffic failing  
- **Symptom:** Cannot reach external/internal networks  
- **Root cause:** Missing/default route issue  
- **Fix:**  
  ```bash
  ip r
  ```

---

## `tcpdump`

- **Situation:** Silent network failures during production traffic  
- **Symptom:** Requests timing out without logs  
- **Root cause:** Packet drops/firewall/routing issues  
- **Fix:**  
  ```bash
  tcpdump -i eth0 port 8080
  ```

---

## `journalctl --disk-usage`

- **Situation:** System logs consuming excessive storage  
- **Symptom:** `/var` usage continuously increasing  
- **Root cause:** Journald retention growth  
- **Fix:**  
  ```bash
  journalctl --disk-usage
  ```

---

## `find -mtime`

- **Situation:** Old logs consuming disk space  
- **Symptom:** Storage exhaustion over time  
- **Root cause:** Retention cleanup missing  
- **Fix:**  
  ```bash
  find /var/log -mtime +30
  ```

---
