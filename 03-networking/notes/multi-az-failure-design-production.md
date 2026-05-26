# 🌐 Multi-AZ & Failure Design — Production Operational Scenario

## 🔹 Availability Zone (AZ)

- **Traffic Flow:** Client → Load Balancer → EC2 in AZ  
- **Layer:** L3 / L4 / L7  
- **Controls:** Routing / Health Checks / Failover  
- **Failure Symptom:** Entire application unavailable after AZ outage  
- **Immediate Check:** Are workloads distributed across multiple AZs?  
- **Root Cause Pattern:** Single-AZ deployment architecture  
- **Fix Action:** Deploy redundant resources in multiple AZs  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Multi-AZ Load Balancer

- **Traffic Flow:** Internet → ALB → Healthy Targets Across AZs  
- **Layer:** L4 / L7  
- **Controls:** Health Checks / Traffic Distribution  
- **Failure Symptom:** Traffic routed to failed instances  
- **Immediate Check:** Target group health status  
- **Root Cause Pattern:** Targets unhealthy or single-AZ registration  
- **Fix Action:** Register healthy targets in multiple AZs  
- ⚠️ **Blast Radius:** AZ / VPC

---

## 🔹 Public Subnet Per AZ

- **Traffic Flow:** Internet → IGW → Public Subnet  
- **Layer:** L3  
- **Controls:** Routing / Internet Access  
- **Failure Symptom:** Public traffic unavailable in one AZ  
- **Immediate Check:** Route table + subnet association  
- **Root Cause Pattern:** Missing subnet redundancy  
- **Fix Action:** Create public subnet in every AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Private Subnet Per AZ

- **Traffic Flow:** App Server → NAT Gateway → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT Access  
- **Failure Symptom:** Private instances lose outbound internet  
- **Immediate Check:** NAT route path for affected AZ  
- **Root Cause Pattern:** Shared NAT across AZs  
- **Fix Action:** Deploy dedicated NAT per AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 NAT Gateway High Availability

- **Traffic Flow:** Private EC2 → NAT Gateway → IGW → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / Outbound Translation  
- **Failure Symptom:** Package install/API access fails from private subnet  
- **Immediate Check:** NAT Gateway state + route table  
- **Root Cause Pattern:** Single NAT Gateway dependency  
- **Fix Action:** Use one NAT Gateway per AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Route Table Failover Logic

- **Traffic Flow:** Subnet → Route Table → Gateway/Target  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic blackhole after AZ failure  
- **Immediate Check:** Default route target availability  
- **Root Cause Pattern:** Route points to failed component  
- **Fix Action:** Use AZ-local resilient routing design  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 Database Multi-AZ

- **Traffic Flow:** App Server → Database Endpoint:3306  
- **Layer:** L4 / L7  
- **Controls:** Replication / Failover  
- **Failure Symptom:** Database outage during primary failure  
- **Immediate Check:** Standby replica health  
- **Root Cause Pattern:** Single-instance database deployment  
- **Fix Action:** Enable Multi-AZ database replication  
- ⚠️ **Blast Radius:** AZ / VPC

---

## 🔹 Health Check Failure

- **Traffic Flow:** Load Balancer → Target Health Endpoint  
- **Layer:** L7  
- **Controls:** Health Validation / Traffic Steering  
- **Failure Symptom:** Healthy app removed from load balancer  
- **Immediate Check:** Health endpoint response  
- **Root Cause Pattern:** Wrong health check path/port  
- **Fix Action:** Correct health check configuration  
- ⚠️ **Blast Radius:** Host / AZ

---

## 🔹 Single Point of Failure (SPOF)

- **Traffic Flow:** Client → Single Resource → Service  
- **Layer:** L3 / L4 / L7  
- **Controls:** Redundancy / Failover  
- **Failure Symptom:** Entire system outage after one component failure  
- **Immediate Check:** Dependency architecture review  
- **Root Cause Pattern:** No redundancy implemented  
- **Fix Action:** Add multi-AZ redundancy and failover  
- ⚠️ **Blast Radius:** VPC

---

## 🔹 Blast Radius Isolation

- **Traffic Flow:** Failure → Dependent Components  
- **Layer:** L3 / L4 / L7  
- **Controls:** Isolation / Segmentation  
- **Failure Symptom:** One failure impacts multiple services  
- **Immediate Check:** Shared dependency mapping  
- **Root Cause Pattern:** Over-coupled architecture  
- **Fix Action:** Isolate workloads across AZs/subnets/services  
- ⚠️ **Blast Radius:** Subnet / AZ / VPC# 🌐 Multi-AZ & Failure Design — Production Operational Scenario

## 🔹 Availability Zone (AZ)

- **Traffic Flow:** Client → Load Balancer → EC2 in AZ  
- **Layer:** L3 / L4 / L7  
- **Controls:** Routing / Health Checks / Failover  
- **Failure Symptom:** Entire application unavailable after AZ outage  
- **Immediate Check:** Are workloads distributed across multiple AZs?  
- **Root Cause Pattern:** Single-AZ deployment architecture  
- **Fix Action:** Deploy redundant resources in multiple AZs  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Multi-AZ Load Balancer

- **Traffic Flow:** Internet → ALB → Healthy Targets Across AZs  
- **Layer:** L4 / L7  
- **Controls:** Health Checks / Traffic Distribution  
- **Failure Symptom:** Traffic routed to failed instances  
- **Immediate Check:** Target group health status  
- **Root Cause Pattern:** Targets unhealthy or single-AZ registration  
- **Fix Action:** Register healthy targets in multiple AZs  
- ⚠️ **Blast Radius:** AZ / VPC

---

## 🔹 Public Subnet Per AZ

- **Traffic Flow:** Internet → IGW → Public Subnet  
- **Layer:** L3  
- **Controls:** Routing / Internet Access  
- **Failure Symptom:** Public traffic unavailable in one AZ  
- **Immediate Check:** Route table + subnet association  
- **Root Cause Pattern:** Missing subnet redundancy  
- **Fix Action:** Create public subnet in every AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Private Subnet Per AZ

- **Traffic Flow:** App Server → NAT Gateway → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT Access  
- **Failure Symptom:** Private instances lose outbound internet  
- **Immediate Check:** NAT route path for affected AZ  
- **Root Cause Pattern:** Shared NAT across AZs  
- **Fix Action:** Deploy dedicated NAT per AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 NAT Gateway High Availability

- **Traffic Flow:** Private EC2 → NAT Gateway → IGW → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / Outbound Translation  
- **Failure Symptom:** Package install/API access fails from private subnet  
- **Immediate Check:** NAT Gateway state + route table  
- **Root Cause Pattern:** Single NAT Gateway dependency  
- **Fix Action:** Use one NAT Gateway per AZ  
- ⚠️ **Blast Radius:** AZ

---

## 🔹 Route Table Failover Logic

- **Traffic Flow:** Subnet → Route Table → Gateway/Target  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic blackhole after AZ failure  
- **Immediate Check:** Default route target availability  
- **Root Cause Pattern:** Route points to failed component  
- **Fix Action:** Use AZ-local resilient routing design  
- ⚠️ **Blast Radius:** Subnet / AZ

---

## 🔹 Database Multi-AZ

- **Traffic Flow:** App Server → Database Endpoint:3306  
- **Layer:** L4 / L7  
- **Controls:** Replication / Failover  
- **Failure Symptom:** Database outage during primary failure  
- **Immediate Check:** Standby replica health  
- **Root Cause Pattern:** Single-instance database deployment  
- **Fix Action:** Enable Multi-AZ database replication  
- ⚠️ **Blast Radius:** AZ / VPC

---

## 🔹 Health Check Failure

- **Traffic Flow:** Load Balancer → Target Health Endpoint  
- **Layer:** L7  
- **Controls:** Health Validation / Traffic Steering  
- **Failure Symptom:** Healthy app removed from load balancer  
- **Immediate Check:** Health endpoint response  
- **Root Cause Pattern:** Wrong health check path/port  
- **Fix Action:** Correct health check configuration  
- ⚠️ **Blast Radius:** Host / AZ

---

## 🔹 Single Point of Failure (SPOF)

- **Traffic Flow:** Client → Single Resource → Service  
- **Layer:** L3 / L4 / L7  
- **Controls:** Redundancy / Failover  
- **Failure Symptom:** Entire system outage after one component failure  
- **Immediate Check:** Dependency architecture review  
- **Root Cause Pattern:** No redundancy implemented  
- **Fix Action:** Add multi-AZ redundancy and failover  
- ⚠️ **Blast Radius:** VPC

---

## 🔹 Blast Radius Isolation

- **Traffic Flow:** Failure → Dependent Components  
- **Layer:** L3 / L4 / L7  
- **Controls:** Isolation / Segmentation  
- **Failure Symptom:** One failure impacts multiple services  
- **Immediate Check:** Shared dependency mapping  
- **Root Cause Pattern:** Over-coupled architecture  
- **Fix Action:** Isolate workloads across AZs/subnets/services  
- ⚠️ **Blast Radius:** Subnet / AZ / VPC
