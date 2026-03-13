# 🌐 VPC (Virtual Private Cloud) — Production Operational Scenario

## 🔹 VPC CIDR Block

- **Traffic Flow:** EC2 (10.0.1.10) → EC2 (10.0.2.15):any  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Instances inside VPC cannot communicate  
- **Immediate Check:** Verify VPC CIDR and subnet CIDRs (`10.0.0.0/16`, etc.)  
- **Root Cause Pattern:** Overlapping CIDR, incorrect subnet allocation  
- **Fix Action:** Redesign CIDR or recreate subnets with non-overlapping ranges  
- ⚠️ **Blast Radius:** VPC

---

# 🌐 VPC Isolation — Production Operational Scenario

## 🔹 Network Isolation Boundary

- **Traffic Flow:** VPC-A (10.0.0.0/16) → VPC-B (10.0.0.0/16):any  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Instances across VPCs cannot communicate  
- **Immediate Check:** Verify if VPC Peering / Transit connectivity exists  
- **Root Cause Pattern:** VPCs are isolated by default  
- **Fix Action:** Configure VPC Peering, Transit Gateway, or PrivateLink  
- ⚠️ **Blast Radius:** VPC

---

# 🌐 Internet Gateway (IGW) — Production Operational Scenario

## 🔹 Public Internet Access

- **Traffic Flow:** EC2 (10.0.1.10) → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing  
- **Failure Symptom:** Public instance cannot access internet  
- **Immediate Check:** Route table contains `0.0.0.0/0 → IGW`  
- **Root Cause Pattern:** Missing Internet Gateway or route entry  
- **Fix Action:** Attach IGW to VPC and update route table  
- ⚠️ **Blast Radius:** Subnet / VPC

---

# 🌐 Route Table — Production Operational Scenario

## 🔹 Traffic Direction Control

- **Traffic Flow:** EC2 (10.0.1.15) → 8.8.8.8:53  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** External connectivity timeout  
- **Immediate Check:** `0.0.0.0/0` route target (IGW or NAT)  
- **Root Cause Pattern:** Wrong route target or missing default route  
- **Fix Action:** Update route table association or route entry  
- ⚠️ **Blast Radius:** Subnet

---

# 🌐 Subnet Segmentation — Production Operational Scenario

## 🔹 Network Partition

- **Traffic Flow:** Public Subnet (10.0.1.0/24) → Private Subnet (10.0.11.0/24):443  
- **Layer:** L3 / L4  
- **Controls:** Routing / Filtering  
- **Failure Symptom:** Application tier cannot reach database tier  
- **Immediate Check:** Verify subnet route tables and security groups  
- **Root Cause Pattern:** Incorrect subnet routing or blocked security rules  
- **Fix Action:** Correct route table association and allow SG rules  
- ⚠️ **Blast Radius:** Subnet / AZ

---

# 🌐 Security Boundary (Security Groups / NACL) — Production Operational Scenario

## 🔹 Traffic Permission Control

- **Traffic Flow:** Internet → EC2:80  
- **Layer:** L4  
- **Controls:** Port / Filtering  
- **Failure Symptom:** Service reachable internally but not externally  
- **Immediate Check:** Inbound rules allow TCP 80/443  
- **Root Cause Pattern:** Security Group or NACL blocking traffic  
- **Fix Action:** Update inbound/outbound rules appropriately  
- ⚠️ **Blast Radius:** Host / Subnet

---

# 🌐 Blast Radius Isolation — Production Operational Scenario

## 🔹 Environment Segmentation

- **Traffic Flow:** Dev VPC → Prod VPC:any  
- **Layer:** L3  
- **Controls:** Routing / Filtering  
- **Failure Symptom:** Dev resources accidentally access production services  
- **Immediate Check:** Verify network isolation between environments  
- **Root Cause Pattern:** Shared network or unintended peering  
- **Fix Action:** Separate VPCs and restrict cross-environment routing  
- ⚠️ **Blast Radius:** VPC
