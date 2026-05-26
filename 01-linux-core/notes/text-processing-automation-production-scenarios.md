# TEXT PROCESSING & AUTOMATION  —  Production Operational Scenarios

---

## `grep`

- **Situation:** Production logs too large to inspect manually  
- **Symptom:** Errors hidden inside massive log files  
- **Root cause:** Repeated failures buried in logs  
- **Fix:**  
  ```bash
  grep -i error /var/log/syslog
  ```

---

## `grep -c`

- **Situation:** Need quick incident impact estimation  
- **Symptom:** Unknown frequency of failures  
- **Root cause:** Error spike not quantified  
- **Fix:**  
  ```bash
  grep -c "error" app.log
  ```

---

## `awk`

- **Situation:** Need only operationally relevant fields  
- **Symptom:** Full output too noisy during incident  
- **Root cause:** Unfiltered process/system data  
- **Fix:**  
  ```bash
  ps aux | awk '{print $1,$2,$3,$11}'
  ```

---

## `sed`

- **Situation:** Incorrect config values after deployment  
- **Symptom:** Application pointing to wrong endpoint/path  
- **Root cause:** Old config values remained active  
- **Fix:**  
  ```bash
  sed -i 's/old/new/g' app.conf
  ```

- ⚠️ **Risk:** Incorrect replacement may corrupt production configs

---

## `xargs`

- **Situation:** Bulk cleanup required during outage  
- **Symptom:** Thousands of unwanted files/logs  
- **Root cause:** Retention cleanup missing  
- **Fix:**  
  ```bash
  find /tmp -name "*.log" | xargs rm
  ```

- ⚠️ **Risk:** Incorrect path filtering can trigger mass deletion

---

## `find`

- **Situation:** Disk usage rising rapidly  
- **Symptom:** `/var` nearing full capacity  
- **Root cause:** Oversized logs/artifacts  
- **Fix:**  
  ```bash
  find /var/log -name "*.log" -size +100M
  ```

---

## `find -mtime`

- **Situation:** Old logs consuming storage  
- **Symptom:** Retention policy failure  
- **Root cause:** Stale files never cleaned  
- **Fix:**  
  ```bash
  find /var/log -mtime +30
  ```

---

## `sort`

- **Situation:** CPU spike affecting application latency  
- **Symptom:** System slowdown under load  
- **Root cause:** High CPU-consuming process  
- **Fix:**  
  ```bash
  ps aux | sort -k3 -nr | head
  ```

---

## `wc -l`

- **Situation:** Need quick failure/event count  
- **Symptom:** Incident scale unknown  
- **Root cause:** No quantified visibility  
- **Fix:**  
  ```bash
  grep error app.log | wc -l
  ```

---

## `|` (Pipeline)

- **Situation:** Multiple operational filters needed together  
- **Symptom:** Raw outputs difficult to analyze quickly  
- **Root cause:** Manual inspection too slow during outage  
- **Fix:**  
  ```bash
  journalctl -u nginx | grep -i failed
  ```

---
