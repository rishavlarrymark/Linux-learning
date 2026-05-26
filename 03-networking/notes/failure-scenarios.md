# 🌐 Failure Scenarios (INTERVIEW GOLD) — Production Operational Scenario

## 🔹 EC2 No Internet Access

- **Traffic Flow:** EC2 → NAT/IGW → Internet:443  
- **Layer:** L3 / L4 / L7  
- **Controls:** Routing / DNS / Filtering  
- **Failure Symptom:** Package install/API access timeout  
- **Immediate Check:** `ip r`, NAT route, IGW attachment  
- **Root Cause Pattern:** Missing default route or NAT failure  
- **Fix Action:** Correct route table and NAT/IGW path  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 DNS Resolution Failure

- **Traffic Flow:** Client → DNS Resolver:53  
- **Layer:** L7  
- **Controls:** Resolution  
- **Failure Symptom:** Domain unreachable but IP reachable  
- **Immediate Check:** `nslookup`, `dig`, DNS config  
- **Root Cause Pattern:** Broken DNS resolver or wrong records  
- **Fix Action:** Correct DNS settings/records  
- ⚠️ **Blast Radius:** VPC / Application

---

## 🔹 Public Website Unreachable

- **Traffic Flow:** Internet → IGW → EC2:80/443  
- **Layer:** L3 / L4 / L7  
- **Controls:** Routing / Port Filtering / Service Binding  
- **Failure Symptom:** Browser timeout  
- **Immediate Check:** Public IP, SG rules, route table, `ss -tuln`  
- **Root Cause Pattern:** Missing IGW route or blocked port  
- **Fix Action:** Correct SG/routing/service binding  
- ⚠️ **Blast Radius:** Host / Subnet

---

## 🔹 Wrong Service Binding

- **Traffic Flow:** Client → Service Port  
- **Layer:** L4  
- **Controls:** Port Binding  
- **Failure Symptom:** Service works locally but not externally  
- **Immediate Check:** `ss -tuln` bind address  
- **Root Cause Pattern:** Service bound to `127.0.0.1` only  
- **Fix Action:** Bind service to `0.0.0.0`  
- ⚠️ **Blast Radius:** Host

---

## 🔹 Security Group Block

- **Traffic Flow:** Client → EC2:Port  
- **Layer:** L4  
- **Controls:** Stateful Filtering  
- **Failure Symptom:** Connection timeout  
- **Immediate Check:** Inbound/outbound SG rules  
- **Root Cause Pattern:** Missing allowed port/source  
- **Fix Action:** Add correct SG allow rule  
- ⚠️ **Blast Radius:** Host

---

## 🔹 NACL Block

- **Traffic Flow:** Subnet ↔ External/Internal Traffic  
- **Layer:** L3 / L4  
- **Controls:** Stateless Filtering  
- **Failure Symptom:** Random/intermittent failures  
- **Immediate Check:** Inbound + outbound NACL rules  
- **Root Cause Pattern:** Missing ephemeral port allow rules  
- **Fix Action:** Correct NACL request/response rules  
- ⚠️ **Blast Radius:** Subnet

---

## 🔹 Database Connectivity Failure

- **Traffic Flow:** App Server → Database:3306  
- **Layer:** L4 / L7  
- **Controls:** Routing / Filtering / Service Binding  
- **Failure Symptom:** DB connection timeout/refused  
- **Immediate Check:** `nc -vz db-host 3306`  
- **Root Cause Pattern:** SG block, DB down, wrong bind address  
- **Fix Action:** Correct SG/service configuration  
- ⚠️ **Blast Radius:** Host / Subnet

---

## 🔹 NAT Gateway Failure

- **Traffic Flow:** Private EC2 → NAT Gateway → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / Outbound Translation  
- **Failure Symptom:** Private subnet loses outbound internet  
- **Immediate Check:** NAT status + private route table  
- **Root Cause Pattern:** NAT unavailable or wrong route target  
- **Fix Action:** Restore NAT or correct routing  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Route Table Misconfiguration

- **Traffic Flow:** EC2 → Gateway/Target  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic blackhole or unreachable host  
- **Immediate Check:** Default route destination/target  
- **Root Cause Pattern:** Wrong or missing route entry  
- **Fix Action:** Correct route mapping  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 Blast Radius Failure Analysis

- **Traffic Flow:** Failed Component → Dependent Services  
- **Layer:** L3 / L4 / L7  
- **Controls:** Isolation / Redundancy  
- **Failure Symptom:** Multiple systems impacted by one failure  
- **Immediate Check:** Shared dependency architecture  
- **Root Cause Pattern:** Single point of failure  
- **Fix Action:** Add redundancy and isolation  
- ⚠️ **Blast Radius:** Host / Subnet / AZ / VPC

---

## 🔹 End-to-End Production Debugging

- **Traffic Flow:** Client → DNS → Route → Security → Service → App  
- **Layer:** L3 / L4 / L6 / L7  
- **Controls:** Resolution / Routing / Filtering / TLS  
- **Failure Symptom:** Application inaccessible  
- **Immediate Check:** Validate every traffic layer sequentially  
- **Root Cause Pattern:** Layer-specific infrastructure failure  
- **Fix Action:** Isolate failing layer and repair systematically  
- ⚠️ **Blast Radius:** Host / Subnet / AZ / VPC
