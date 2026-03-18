# DAY 10 — LINUX NETWORKING  —  Production Operational Scenarios

---
## `ip a`

- **Situation:** Service unreachable  
- **Symptom:** No response from server  
- **Root cause:** Interface down or IP not assigned  
- **Fix:** `sudo ip link set eth0 up` , `sudo dhclient eth0`  
---

---
## `ip r`

- **Situation:** Server cannot reach external network  
- **Symptom:** Ping to external IP fails  
- **Root cause:** Missing or incorrect default route  
- **Fix:** `sudo ip route add default via <gateway-ip>`  
---

---
## `ping 8.8.8.8`

- **Situation:** Network connectivity check  
- **Symptom:** No replies  
- **Root cause:** Routing issue or firewall blocking ICMP  
- **Fix:** `sudo ip route add default via <gateway-ip>` , `sudo ufw allow out`  
---

---
## `ping google.com`

- **Situation:** Domain not resolving  
- **Symptom:** Temporary failure in name resolution  
- **Root cause:** DNS misconfiguration  
- **Fix:** `echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf`  
---

---
## `ss -tulnp`

- **Situation:** Service not accessible  
- **Symptom:** Connection refused  
- **Root cause:** Port not listening  
- **Fix:** `sudo systemctl restart <service>`  
---

---
## `curl localhost:PORT`

- **Situation:** Application health check  
- **Symptom:** No response or error  
- **Root cause:** Application crash or wrong binding  
- **Fix:** `sudo systemctl restart <service>` , update bind to `0.0.0.0`  
---

---
## `lsof -i :PORT`

- **Situation:** Port already in use  
- **Symptom:** Service fails to start  
- **Root cause:** Another process bound to same port  
- **Fix:** `kill <PID>` , `kill -9 <PID>`  
- ⚠️ **Risk:** Force kill can corrupt application state  
---

---
## `traceroute google.com`

- **Situation:** Intermittent connectivity issues  
- **Symptom:** Timeout at specific hop  
- **Root cause:** Routing issue or upstream block  
- **Fix:** `sudo ip route add <network> via <gateway>`  
---

---
## `cat /etc/resolv.conf`

- **Situation:** DNS resolution failure  
- **Symptom:** Domain not resolving  
- **Root cause:** Invalid or missing DNS server  
- **Fix:** `sudo nano /etc/resolv.conf` → add `nameserver 8.8.8.8`  
---

---
## `ip route get <ip>`

- **Situation:** Traffic not reaching destination  
- **Symptom:** Wrong interface used  
- **Root cause:** Incorrect routing table  
- **Fix:** `sudo ip route del <wrong-route>` , `sudo ip route add <correct-route>`  
---
