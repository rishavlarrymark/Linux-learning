# 🌐 Subnets — Production Operational Scenario

## 🔹 Subnet Placement

- **Traffic Flow:** EC2 (10.0.1.15) → EC2 (10.0.11.20):443  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Internal services cannot communicate across subnets  
- **Immediate Check:** Verify both instances belong to the same VPC and correct subnets  
- **Root Cause Pattern:** Misplaced resources across unintended subnets or wrong subnet CIDR planning  
- **Fix Action:** Move resources to correct subnet or redesign subnet allocation  
- ⚠️ **Blast Radius:** Subnet


# 🌐 Route Tables — Production Operational Scenario

## 🔹 Route Decision

- **Traffic Flow:** EC2 → Destination Network (via route table)  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic fails silently or packets never reach target  
- **Immediate Check:** Inspect route table associated with the subnet  
- **Root Cause Pattern:** Missing route entry or incorrect target gateway  
- **Fix Action:** Add or correct destination → target rule in route table  
- ⚠️ **Blast Radius:** Subnet / VPC


# 🌐 Internet Access — Production Operational Scenario

## 🔹 Internet Gateway (IGW)

- **Traffic Flow:** EC2 (Public Subnet) → Internet:443  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Instance cannot reach external services or APIs  
- **Immediate Check:** Confirm route `0.0.0.0/0 → Internet Gateway` exists  
- **Root Cause Pattern:** Missing IGW attachment or missing default route  
- **Fix Action:** Attach IGW to VPC and update route table with default route  
- ⚠️ **Blast Radius:** Subnet / VPC


# 🌐 Public Subnet — Production Operational Scenario

## 🔹 Public Internet Exposure

- **Traffic Flow:** Internet Client → Load Balancer / EC2:80/443  
- **Layer:** L3 / L4  
- **Controls:** Routing / Port / Filtering  
- **Failure Symptom:** Public service unreachable from internet  
- **Immediate Check:** Verify subnet route table has `0.0.0.0/0 → IGW`  
- **Root Cause Pattern:** Subnet assumed public but lacks internet gateway route  
- **Fix Action:** Update route table and confirm correct subnet association  
- ⚠️ **Blast Radius:** Subnet


# 🌐 Private Subnet — Production Operational Scenario

## 🔹 Internal Service Isolation

- **Traffic Flow:** EC2 (Private Subnet) → Internal Service / Database:3306  
- **Layer:** L3 / L4  
- **Controls:** Routing / Port / Filtering  
- **Failure Symptom:** Application cannot reach database or backend service  
- **Immediate Check:** Validate private subnet routing and security group rules  
- **Root Cause Pattern:** Security group blocking traffic or incorrect internal routing  
- **Fix Action:** Adjust security group rules or correct route table association  
- ⚠️ **Blast Radius:** Subnet / AZ


# 🌐 Default Route — Production Operational Scenario

## 🔹 `0.0.0.0/0` Traffic Handling

- **Traffic Flow:** EC2 → Any External Destination:443  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** External connectivity failure (APIs, package updates, downloads)  
- **Immediate Check:** Verify presence of `0.0.0.0/0` default route in route table  
- **Root Cause Pattern:** Default route missing or pointing to incorrect gateway  
- **Fix Action:** Add or correct default route pointing to IGW or NAT  
- ⚠️ **Blast Radius:** Subnet / VPC
