# 🌐 Internet Gateway (IGW) — Production Operational Scenario

## 🔹 Internet Gateway

- **Traffic Flow:** Internet → EC2:<public-ip>:80/443  
- **Layer:** L3 / L4  
- **Controls:** Routing + Security Group  
- **Failure Symptom:** Website not reachable  
- **Immediate Check:** Route table (0.0.0.0/0 → IGW), public IP  
- **Root Cause Pattern:** Missing route / no public IP / SG blocked  
- **Fix Action:** Add IGW route, assign public IP, allow SG  
- ⚠️ **Blast Radius:** Subnet / VPC  

---

# 🌐 NAT Gateway — Production Operational Scenario

## 🔹 NAT Gateway

- **Traffic Flow:** Private EC2 → NAT → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing + NAT translation  
- **Failure Symptom:** Private EC2 no internet  
- **Immediate Check:** Route table (0.0.0.0/0 → NAT), NAT status  
- **Root Cause Pattern:** Missing NAT / wrong route / NAT in private subnet  
- **Fix Action:** Create NAT in public subnet, update route  
- ⚠️ **Blast Radius:** Subnet / AZ  

---

# 🌐 Route Table — Production Operational Scenario

## 🔹 Route Table

- **Traffic Flow:** EC2 → Route → Target (IGW/NAT)  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic not reaching destination  
- **Immediate Check:** Route entries + subnet association  
- **Root Cause Pattern:** Wrong target / missing default route  
- **Fix Action:** Update correct route (IGW/NAT)  
- ⚠️ **Blast Radius:** Subnet / VPC  

---

# 🌐 Private Subnet Internet Access — Production Operational Scenario

## 🔹 Private EC2 via NAT

- **Traffic Flow:** Private EC2 → NAT → IGW → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing + NAT + SG  
- **Failure Symptom:** API calls failing / updates failing  
- **Immediate Check:** NAT route + SG outbound + IGW attached  
- **Root Cause Pattern:** NAT missing / route missing / SG blocked  
- **Fix Action:** Attach NAT route, verify outbound rules  
- ⚠️ **Blast Radius:** Subnet / AZ  

---

# 🌐 Public Subnet Exposure — Production Operational Scenario

## 🔹 Public EC2 via IGW

- **Traffic Flow:** Internet → IGW → EC2:<port>  
- **Layer:** L3 / L4  
- **Controls:** Routing + Port filtering (SG)  
- **Failure Symptom:** Cannot access public service  
- **Immediate Check:** IGW route + SG inbound rules  
- **Root Cause Pattern:** Port closed / no IGW route  
- **Fix Action:** Open port in SG, verify route  
- ⚠️ **Blast Radius:** Subnet / VPC  
