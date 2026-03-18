# 🌐 Internet Gateway (IGW) — Production Operational Scenario

## 🔹 Internet Gateway

- **Traffic Flow:** Internet → EC2:<public-ip>:80/443  
- **Layer:** L3 / L4  
- **Controls:** Routing / Port Filtering  
- **Failure Symptom:** Public service not reachable  
- **Immediate Check:** Route, public IP, SG rules  
- **Root Cause Pattern:** Missing IGW / no route / blocked port  
- **Fix Action:**

# Attach IGW to VPC
aws ec2 attach-internet-gateway \
  --internet-gateway-id igw-xxxx \
  --vpc-id vpc-xxxx

# Add default route to IGW
aws ec2 create-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-xxxx

# Allocate public IP (Elastic IP)
aws ec2 allocate-address

# Allow inbound HTTP
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxx \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0

- ⚠️ **Blast Radius:** Subnet / VPC  

---

# 🌐 NAT Gateway — Production Operational Scenario

## 🔹 NAT Gateway

- **Traffic Flow:** Private EC2 → NAT → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT  
- **Failure Symptom:** No internet from private subnet  
- **Immediate Check:** NAT status, route  
- **Root Cause Pattern:** NAT missing / wrong subnet  
- **Fix Action:**

# Allocate Elastic IP
aws ec2 allocate-address

# Create NAT Gateway
aws ec2 create-nat-gateway \
  --subnet-id subnet-public-xxxx \
  --allocation-id eipalloc-xxxx

# Add route to NAT
aws ec2 create-route \
  --route-table-id rtb-private-xxxx \
  --destination-cidr-block 0.0.0.0/0 \
  --nat-gateway-id nat-xxxx

- ⚠️ **Blast Radius:** Subnet / AZ  

---

# 🌐 Route Table — Production Operational Scenario

## 🔹 Route Table

- **Traffic Flow:** EC2 → Route → IGW/NAT  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** Traffic stuck  
- **Immediate Check:** Routes + association  
- **Root Cause Pattern:** Missing default route  
- **Fix Action:**

# View routes
aws ec2 describe-route-tables

# Add route (IGW example)
aws ec2 create-route \
  --route-table-id rtb-xxxx \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-xxxx

# Associate subnet
aws ec2 associate-route-table \
  --subnet-id subnet-xxxx \
  --route-table-id rtb-xxxx

- ⚠️ **Blast Radius:** Subnet / VPC  

---

# 🌐 Private Subnet Internet Access — Production Operational Scenario

## 🔹 Private EC2 via NAT

- **Traffic Flow:** Private EC2 → NAT → IGW → Internet  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT / SG  
- **Failure Symptom:** API/update failure  
- **Immediate Check:** NAT route, outbound rules  
- **Root Cause Pattern:** NAT missing / SG blocked  
- **Fix Action:**

# Check outbound SG (allow all)
aws ec2 authorize-security-group-egress \
  --group-id sg-xxxx \
  --protocol -1 \
  --cidr 0.0.0.0/0

# Verify route exists
aws ec2 describe-route-tables

- ⚠️ **Blast Radius:** Subnet / AZ  

---

# 🌐 Public Subnet Exposure — Production Operational Scenario

## 🔹 Public EC2 via IGW

- **Traffic Flow:** Internet → IGW → EC2:<port>  
- **Layer:** L3 / L4  
- **Controls:** Routing / SG  
- **Failure Symptom:** Cannot access service  
- **Immediate Check:** SG + route + public IP  
- **Root Cause Pattern:** Port blocked / no public IP  
- **Fix Action:**

# Open HTTPS port
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxx \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0

# Check instance public IP
aws ec2 describe-instances

- ⚠️ **Blast Radius:** Subnet / VPC  
