# 🌐 Network Debugging — Production Operational Scenario

## 🔹 traceroute

- **Traffic Flow:** Client → Internet Routers → Destination Server:Any  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic stops before reaching destination; `* * *` appears repeatedly  
- **Immediate Check:** `traceroute <destination>`  
- **Root Cause Pattern:** Routing issue, ISP filtering, firewall blocking ICMP, broken network segment  
- **Fix Action:** Verify route tables, check gateway/NAT path, confirm firewall rules allowing return ICMP  
- ⚠️ **Blast Radius:** Host / Subnet / VPC

---

## 🔹 tcpdump

- **Traffic Flow:** Client → Server:Port (example: 443)  
- **Layer:** L3 / L4  
- **Controls:** Packet visibility  
- **Failure Symptom:** Service unreachable but no clarity where traffic stops  
- **Immediate Check:** `sudo tcpdump -i <interface> port <port>`  
- **Root Cause Pattern:** Packets not reaching host (security group/NACL/routing) or packets arriving but service not responding  
- **Fix Action:** If packets absent → inspect routing/firewall; if packets present → inspect service binding or application logs  
- ⚠️ **Blast Radius:** Host

---

## 🔹 Latency Observation

- **Traffic Flow:** Client → Server:Port  
- **Layer:** L3 / L4  
- **Controls:** Routing / Path efficiency  
- **Failure Symptom:** Slow response, high RTT in traceroute/ping  
- **Immediate Check:** `ping <destination>` or `traceroute <destination>`  
- **Root Cause Pattern:** Long routing path, congested network segment, overloaded gateway  
- **Fix Action:** Optimize routing path, verify NAT/Gateway performance, check ISP or upstream provider  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 Packet Loss Detection

- **Traffic Flow:** Client → Server:Port  
- **Layer:** L3 / L4  
- **Controls:** Routing / Filtering  
- **Failure Symptom:** Connection timeout, intermittent failures, dropped packets  
- **Immediate Check:** `ping -c 10 <destination>` or observe packet drops in traceroute  
- **Root Cause Pattern:** Network congestion, firewall filtering, unstable link between routers  
- **Fix Action:** Inspect firewall rules, verify route path stability, analyze network hardware or upstream connectivity  
- ⚠️ **Blast Radius:** Subnet / AZ / VPC

---

## 🔹 Hop-by-Hop Traffic Failure

- **Traffic Flow:** Client → Router → ISP → Internet → Cloud Gateway → Server:Port  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic stops at specific hop in traceroute  
- **Immediate Check:** Identify last responding hop using `traceroute`  
- **Root Cause Pattern:** Misconfigured route, NAT failure, upstream filtering, broken gateway path  
- **Fix Action:** Inspect route tables, gateway configuration, firewall rules on failing segment  
- ⚠️ **Blast Radius:** Subnet / AZ / VPC
