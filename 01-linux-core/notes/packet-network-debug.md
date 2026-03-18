# DAY 11 — PACKET & NETWORK DEBUG  —  Production Operational Scenarios

---
## `tcpdump -i eth0`

- **Situation:** Network issue reported, need raw traffic visibility  
- **Symptom:** Service unreachable, unclear if traffic exists  
- **Root cause:** Unknown (no visibility at packet level)  
- **Fix:** Capture live traffic on interface and observe flow direction  
---

## `tcpdump -i eth0 icmp`

- **Situation:** Connectivity test (ping not behaving as expected)  
- **Symptom:** Ping fails or inconsistent  
- **Root cause:** Either no request sent or no reply received  
- **Fix:** Verify ICMP request (echo) and reply presence  
---

## `tcpdump -i eth0 host 8.8.8.8`

- **Situation:** Specific external host unreachable  
- **Symptom:** Service/API not responding  
- **Root cause:** Traffic not reaching or response not returning  
- **Fix:** Check bidirectional packets between client and target host  
---

## `tcpdump -i eth0 port 80`

- **Situation:** HTTP service not working  
- **Symptom:** curl fails / no response  
- **Root cause:** Request not reaching server or response blocked  
- **Fix:** Capture HTTP traffic and confirm request/response flow  
---

## `tcpdump -i eth0 -c 20`

- **Situation:** Need limited packet capture for quick analysis  
- **Symptom:** Continuous output overwhelming terminal  
- **Root cause:** Unlimited packet stream  
- **Fix:** Capture fixed number of packets for focused inspection  
---

## `tcpdump -i eth0 -nn`

- **Situation:** Slow or confusing tcpdump output  
- **Symptom:** DNS resolution delays / unreadable hostnames  
- **Root cause:** Name resolution enabled by default  
- **Fix:** Disable DNS resolution for faster, raw IP-level output  
---

## `tcpdump -i eth0 src <IP>`

- **Situation:** Debug outgoing traffic from specific source  
- **Symptom:** Requests suspected but not confirmed  
- **Root cause:** Source system may not be sending packets  
- **Fix:** Filter packets by source IP to verify outbound traffic  
---

## `tcpdump -i eth0 dst <IP>`

- **Situation:** Verify if traffic reaches a specific destination  
- **Symptom:** Target service not receiving requests  
- **Root cause:** Routing/firewall blocking inbound path  
- **Fix:** Filter packets by destination IP to confirm delivery  
---

## `tcpdump -i eth0 port 53`

- **Situation:** DNS resolution failure  
- **Symptom:** Domain names not resolving  
- **Root cause:** DNS requests not sent or responses blocked  
- **Fix:** Capture DNS traffic and verify query/response  
---

## `tcpdump -i eth0 port 8080`

- **Situation:** Custom app port not accessible  
- **Symptom:** Connection timeout / refused  
- **Root cause:** Service not listening or firewall blocking  
- **Fix:** Observe traffic on port 8080 to confirm request/response  
---
