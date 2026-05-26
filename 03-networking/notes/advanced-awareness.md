# 🌐 Advanced Awareness — Production Operational Scenario

## 🔹 VPC Endpoint

- **Traffic Flow:** Private EC2 → AWS Service:S3/DynamoDB  
- **Layer:** L3 / L7  
- **Controls:** Private Routing / Service Access  
- **Failure Symptom:** Private instance requires NAT/internet for AWS service access  
- **Immediate Check:** Endpoint attachment and route association  
- **Root Cause Pattern:** Missing VPC endpoint configuration  
- **Fix Action:** Create and associate VPC endpoint  
- ⚠️ **Blast Radius:** Subnet / VPC

---

## 🔹 VPC Peering

- **Traffic Flow:** VPC-A → VPC-B  
- **Layer:** L3  
- **Controls:** Routing / Private Connectivity  
- **Failure Symptom:** Cross-VPC communication failure  
- **Immediate Check:** Peering status + route tables  
- **Root Cause Pattern:** Missing routes or overlapping CIDR  
- **Fix Action:** Add peering routes and avoid CIDR overlap  
- ⚠️ **Blast Radius:** VPC

---

## 🔹 Transit Gateway (TGW)

- **Traffic Flow:** VPC → TGW → Another VPC  
- **Layer:** L3  
- **Controls:** Centralized Routing  
- **Failure Symptom:** Large-scale VPC routing complexity  
- **Immediate Check:** TGW attachment and propagation  
- **Root Cause Pattern:** Missing TGW route propagation  
- **Fix Action:** Configure TGW routing correctly  
- ⚠️ **Blast Radius:** Multi-VPC

---

## 🔹 VPN Connectivity

- **Traffic Flow:** Office Network → VPN Tunnel → AWS VPC  
- **Layer:** L3 / L4  
- **Controls:** Encryption / Secure Tunneling  
- **Failure Symptom:** Hybrid connectivity unstable or disconnected  
- **Immediate Check:** Tunnel status and route advertisement  
- **Root Cause Pattern:** Internet instability or tunnel misconfiguration  
- **Fix Action:** Restore tunnel/routing configuration  
- ⚠️ **Blast Radius:** VPC / Hybrid Network

---

## 🔹 Direct Connect

- **Traffic Flow:** Data Center → Direct Connect → AWS  
- **Layer:** L3  
- **Controls:** Dedicated Private Connectivity  
- **Failure Symptom:** Enterprise connectivity outage  
- **Immediate Check:** DX virtual interface and provider status  
- **Root Cause Pattern:** Physical/provider-side failure  
- **Fix Action:** Restore dedicated circuit or failover path  
- ⚠️ **Blast Radius:** Region / Enterprise Network

---

## 🔹 VPN vs Direct Connect

- **Traffic Flow:** On-Prem → AWS  
- **Layer:** L3 / L4  
- **Controls:** Encryption / Dedicated Routing  
- **Failure Symptom:** High latency or unstable hybrid connectivity  
- **Immediate Check:** Internet dependency vs dedicated link health  
- **Root Cause Pattern:** Wrong connectivity choice for workload  
- **Fix Action:** Use DX for stable enterprise workloads  
- ⚠️ **Blast Radius:** Hybrid Infrastructure

---

## 🔹 PrivateLink

- **Traffic Flow:** Consumer VPC → PrivateLink → Provider Service  
- **Layer:** L4 / L7  
- **Controls:** Private Service Exposure  
- **Failure Symptom:** Internal service exposed publicly unnecessarily  
- **Immediate Check:** Endpoint service accessibility  
- **Root Cause Pattern:** Public exposure instead of private connectivity  
- **Fix Action:** Use PrivateLink for private service access  
- ⚠️ **Blast Radius:** VPC / Multi-VPC

---

## 🔹 Overlapping CIDR Failure

- **Traffic Flow:** VPC-A ↔ VPC-B  
- **Layer:** L3  
- **Controls:** Routing / Address Planning  
- **Failure Symptom:** Peering/TGW routing failure  
- **Immediate Check:** CIDR comparison between networks  
- **Root Cause Pattern:** Duplicate IP ranges  
- **Fix Action:** Redesign non-overlapping CIDRs  
- ⚠️ **Blast Radius:** Multi-VPC

---

## 🔹 Hybrid Cloud Routing

- **Traffic Flow:** On-Prem → VPN/DX → AWS Subnets  
- **Layer:** L3  
- **Controls:** Routing / Connectivity  
- **Failure Symptom:** On-prem cannot reach cloud resources  
- **Immediate Check:** Route propagation and gateway association  
- **Root Cause Pattern:** Missing propagated routes  
- **Fix Action:** Correct hybrid routing configuration  
- ⚠️ **Blast Radius:** Hybrid Network / VPC

---

## 🔹 Enterprise Hub-and-Spoke Architecture

- **Traffic Flow:** Multiple VPCs → Transit Gateway  
- **Layer:** L3  
- **Controls:** Centralized Routing / Isolation  
- **Failure Symptom:** Complex peering management and route conflicts  
- **Immediate Check:** TGW route tables and attachments  
- **Root Cause Pattern:** Mesh architecture scaling failure  
- **Fix Action:** Implement TGW hub-and-spoke design  
- ⚠️ **Blast Radius:** Multi-VPC / Region

---

## 🔹 Private AWS Service Access

- **Traffic Flow:** Private Subnet → AWS Internal Network → AWS Service  
- **Layer:** L3 / L7  
- **Controls:** Private Connectivity / Routing  
- **Failure Symptom:** AWS service traffic forced through NAT/internet  
- **Immediate Check:** Endpoint route association  
- **Root Cause Pattern:** Missing private endpoint architecture  
- **Fix Action:** Use Gateway/Interface VPC Endpoints  
- ⚠️ **Blast Radius:** Subnet / VPC
