# 🌐 Observability & Traffic Visibility — Production Operational Scenario

## 🔹 VPC Flow Logs

- **Traffic Flow:** Source IP → Destination IP:Port  
- **Layer:** L3 / L4  
- **Controls:** Traffic Visibility / Filtering  
- **Failure Symptom:** Unable to identify blocked traffic path  
- **Immediate Check:** Flow Log ACCEPT/REJECT entries  
- **Root Cause Pattern:** Security filtering or routing issue  
- **Fix Action:** Trace rejected traffic and validate SG/NACL/routes  
- ⚠️ **Blast Radius:** Subnet / VPC

---

## 🔹 ACCEPT Traffic

- **Traffic Flow:** Client → Service Port  
- **Layer:** L3 / L4  
- **Controls:** Routing / Security Rules  
- **Failure Symptom:** Application still failing despite ACCEPT logs  
- **Immediate Check:** Service health and listening ports  
- **Root Cause Pattern:** Network allowed traffic but app failed  
- **Fix Action:** Validate application/service layer  
- ⚠️ **Blast Radius:** Host

---

## 🔹 REJECT Traffic

- **Traffic Flow:** Client → Blocked Destination Port  
- **Layer:** L3 / L4  
- **Controls:** NACL / Security Filtering  
- **Failure Symptom:** Connection timeout or denied access  
- **Immediate Check:** REJECT entries in Flow Logs  
- **Root Cause Pattern:** SG/NACL deny rule or missing allow  
- **Fix Action:** Correct filtering rules  
- ⚠️ **Blast Radius:** Subnet / VPC

---

## 🔹 Security Group Filtering

- **Traffic Flow:** Client → EC2:Port  
- **Layer:** L4  
- **Controls:** Stateful Filtering / Port Rules  
- **Failure Symptom:** Service unreachable externally  
- **Immediate Check:** Inbound/outbound SG rules  
- **Root Cause Pattern:** Missing allowed port/source  
- **Fix Action:** Add correct SG allow rules  
- ⚠️ **Blast Radius:** Host

---

## 🔹 NACL Filtering

- **Traffic Flow:** Subnet Traffic → Destination  
- **Layer:** L3 / L4  
- **Controls:** Stateless Filtering  
- **Failure Symptom:** Intermittent or asymmetric failures  
- **Immediate Check:** Inbound + outbound NACL rules  
- **Root Cause Pattern:** Missing ephemeral port rules  
- **Fix Action:** Allow required request/response ports  
- ⚠️ **Blast Radius:** Subnet

---

## 🔹 Route Table Visibility

- **Traffic Flow:** EC2 → Gateway/Target  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic never reaches destination  
- **Immediate Check:** Default route and target mapping  
- **Root Cause Pattern:** Missing or wrong route target  
- **Fix Action:** Correct route table association and routes  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 DNS Resolution Visibility

- **Traffic Flow:** Client → DNS Resolver:53  
- **Layer:** L7  
- **Controls:** Resolution  
- **Failure Symptom:** Domain unreachable but IP reachable  
- **Immediate Check:** DNS query resolution  
- **Root Cause Pattern:** Wrong/missing DNS records  
- **Fix Action:** Correct DNS configuration  
- ⚠️ **Blast Radius:** VPC / Application

---

## 🔹 tcpdump Packet Visibility

- **Traffic Flow:** Network Interface ↔ Packet Stream  
- **Layer:** L3 / L4  
- **Controls:** Packet Inspection  
- **Failure Symptom:** Unknown packet path failure  
- **Immediate Check:** Incoming/outgoing packet capture  
- **Root Cause Pattern:** Traffic not reaching interface  
- **Fix Action:** Trace upstream routing/security path  
- ⚠️ **Blast Radius:** Host / Subnet

---

## 🔹 Listening Port Validation

- **Traffic Flow:** Client → Service Port  
- **Layer:** L4  
- **Controls:** Port Binding  
- **Failure Symptom:** Connection refused  
- **Immediate Check:** `ss -tuln` listening status  
- **Root Cause Pattern:** Service not listening or wrong bind IP  
- **Fix Action:** Restart service or correct binding  
- ⚠️ **Blast Radius:** Host

---

## 🔹 HTTPS Connection Visibility

- **Traffic Flow:** Client → Server:443  
- **Layer:** L4 / L6 / L7  
- **Controls:** TLS / Filtering / Routing  
- **Failure Symptom:** HTTPS timeout or SSL failure  
- **Immediate Check:** Flow Logs + TLS validation  
- **Root Cause Pattern:** Blocked 443 or TLS misconfiguration  
- **Fix Action:** Validate SG/NACL/TLS certificate chain  
- ⚠️ **Blast Radius:** Host / Subnet / VPC

---

## 🔹 End-to-End Traffic Debugging

- **Traffic Flow:** Client → DNS → Route → Security → Service → App  
- **Layer:** L3 / L4 / L6 / L7  
- **Controls:** Resolution / Routing / Filtering / TLS  
- **Failure Symptom:** Application inaccessible  
- **Immediate Check:** Trace each traffic layer sequentially  
- **Root Cause Pattern:** Layer-specific misconfiguration  
- **Fix Action:** Isolate and repair failing layer  
- ⚠️ **Blast Radius:** Host / Subnet / AZ / VPC
