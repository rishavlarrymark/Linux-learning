# 🌐 VPC (Virtual Private Cloud) — Production Operational Scenario

## 🔹 VPC CIDR Block

- **Traffic Flow:** EC2 (10.0.1.10) → EC2 (10.0.2.15):any  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Instances inside VPC cannot communicate  
- **Immediate Check:** Verify VPC CIDR and subnet CIDRs (`10.0.0.0/16`, etc.)  
- **Root Cause Pattern:** Overlapping CIDR, incorrect subnet allocation  
- **Fix Action:**

# Check VPC CIDR
aws ec2 describe-vpcs

# Check subnets
aws ec2 describe-subnets

# (Fix requires redesign → recreate subnet)
aws ec2 create-subnet \
  --vpc-id vpc-xxxx \
  --cidr-block 10.0.20.0/24

- ⚠️ **Blast Radius:** VPC

---

# 🌐 VPC Isolation — Production Operational Scenario

## 🔹 Network Isolation Boundary

- **Traffic Flow:** VPC-A → VPC-B:any  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Instances across VPCs cannot communicate  
- **Immediate Check:** Check peering / TGW  
- **Root Cause Pattern:** No connectivity configured  
- **Fix Action:**

# Create VPC Peering
aws ec2 create-vpc-peering-connection \
  --vpc-id vpc-aaaa \
  --peer-vpc-id vpc-bbbb

# Accept Peering
aws ec2 accept-vpc-peering-connection \
  --vpc-peering-connection-id pcx-xxxx

# Add route
aws ec2 create-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 10.1.0.0/16 \
  --vpc-peering-connection-id pcx-xxxx

- ⚠️ **Blast Radius:** VPC

---

# 🌐 Internet Gateway (IGW) — Production Operational Scenario

## 🔹 Public Internet Access

- **Traffic Flow:** EC2 → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing  
- **Failure Symptom:** No internet access  
- **Immediate Check:** Route table entry  
- **Root Cause Pattern:** Missing IGW / route  
- **Fix Action:**

# Attach IGW
aws ec2 attach-internet-gateway \
  --internet-gateway-id igw-xxxx \
  --vpc-id vpc-xxxx

# Add route
aws ec2 create-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-xxxx

- ⚠️ **Blast Radius:** Subnet / VPC

---

# 🌐 Route Table — Production Operational Scenario

## 🔹 Traffic Direction Control

- **Traffic Flow:** EC2 → 8.8.8.8:53  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Timeout  
- **Immediate Check:** Default route  
- **Root Cause Pattern:** Missing/wrong route  
- **Fix Action:**

# Check routes
aws ec2 describe-route-tables

# Add default route
aws ec2 create-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-xxxx

- ⚠️ **Blast Radius:** Subnet

---

# 🌐 Subnet Segmentation — Production Operational Scenario

## 🔹 Network Partition

- **Traffic Flow:** Public → Private:443  
- **Layer:** L3 / L4  
- **Controls:** Routing / Filtering  
- **Failure Symptom:** App cannot reach DB  
- **Immediate Check:** Route + SG  
- **Root Cause Pattern:** Blocked SG / wrong route  
- **Fix Action:**

# Allow SG traffic
aws ec2 authorize-security-group-ingress \
  --group-id sg-db \
  --protocol tcp \
  --port 443 \
  --source-group sg-app

# Verify routes
aws ec2 describe-route-tables

- ⚠️ **Blast Radius:** Subnet / AZ

---

# 🌐 Security Boundary (Security Groups / NACL) — Production Operational Scenario

## 🔹 Traffic Permission Control

- **Traffic Flow:** Internet → EC2:80  
- **Layer:** L4  
- **Controls:** Port / Filtering  
- **Failure Symptom:** Not reachable externally  
- **Immediate Check:** SG / NACL rules  
- **Root Cause Pattern:** Port blocked  
- **Fix Action:**

# Allow HTTP
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxx \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0

# Allow outbound
aws ec2 authorize-security-group-egress \
  --group-id sg-xxxx \
  --protocol -1 \
  --cidr 0.0.0.0/0

- ⚠️ **Blast Radius:** Host / Subnet

---

# 🌐 Blast Radius Isolation — Production Operational Scenario

## 🔹 Environment Segmentation

- **Traffic Flow:** Dev → Prod:any  
- **Layer:** L3  
- **Controls:** Routing / Filtering  
- **Failure Symptom:** Dev accessing prod  
- **Immediate Check:** Peering / routes  
- **Root Cause Pattern:** Shared network  
- **Fix Action:**

# Delete peering
aws ec2 delete-vpc-peering-connection \
  --vpc-peering-connection-id pcx-xxxx

# Remove route
aws ec2 delete-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 10.0.0.0/16

- ⚠️ **Blast Radius:** VPC
