# 🌐 Internet & Private Access (IGW + NAT) — Production Operational Scenario

---

## 🔹 Internet Gateway (IGW)

- **Traffic Flow:** Internet → EC2:<public-ip>:80/443  
- **Layer:** L3 / L4  
- **Controls:** Routing / Port Filtering  
- **Failure Symptom:** Public service not reachable  
- **Immediate Check:** Route (0.0.0.0/0 → IGW), public IP, SG rules  
- **Root Cause Pattern:** Missing IGW / no route / blocked port  
- **Fix Action:**  
  aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxx --vpc-id vpc-xxxx  
  aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxx  
  aws ec2 authorize-security-group-ingress --group-id sg-xxxx --protocol tcp --port 80 --cidr 0.0.0.0/0  
- ⚠️ **Blast Radius:** Subnet / VPC  

---

## 🔹 NAT Gateway

- **Traffic Flow:** Private EC2 → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT  
- **Failure Symptom:** Private instance no internet  
- **Immediate Check:** Route (0.0.0.0/0 → NAT), NAT status  
- **Root Cause Pattern:** NAT missing / wrong route / NAT not public  
- **Fix Action:**  
  aws ec2 create-nat-gateway --subnet-id subnet-public-xxxx --allocation-id eipalloc-xxxx  
  aws ec2 create-route --route-table-id rtb-private-xxxx --destination-cidr-block 0.0.0.0/0 --nat-gateway-id nat-xxxx  
- ⚠️ **Blast Radius:** Subnet / AZ  

---

## 🔹 Route Table (Default Route)

- **Traffic Flow:** EC2 → 8.8.8.8:53  
- **Layer:** L3  
- **Controls:** Routing  
- **Failure Symptom:** External timeout  
- **Immediate Check:** 0.0.0.0/0 route target  
- **Root Cause Pattern:** Missing/wrong default route  
- **Fix Action:**  
  aws ec2 describe-route-tables  
  aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxx  
- ⚠️ **Blast Radius:** Subnet  

---

## 🔹 Private EC2 Internet Access

- **Traffic Flow:** Private EC2 → NAT → Internet:443  
- **Layer:** L3 / L4  
- **Controls:** Routing / NAT / Filtering  
- **Failure Symptom:** API/update failure  
- **Immediate Check:** NAT route + SG outbound  
- **Root Cause Pattern:** NAT missing / outbound blocked  
- **Fix Action:**  
  aws ec2 authorize-security-group-egress --group-id sg-xxxx --protocol -1 --cidr 0.0.0.0/0  
  aws ec2 describe-route-tables  
- ⚠️ **Blast Radius:** Subnet / AZ  

---

## 🔹 Public EC2 Exposure

- **Traffic Flow:** Internet → EC2:<port>  
- **Layer:** L3 / L4  
- **Controls:** Routing / Port Filtering  
- **Failure Symptom:** Service not reachable  
- **Immediate Check:** SG inbound + IGW route + public IP  
- **Root Cause Pattern:** Port blocked / no public IP  
- **Fix Action:**  
  aws ec2 authorize-security-group-ingress --group-id sg-xxxx --protocol tcp --port 443 --cidr 0.0.0.0/0  
  aws ec2 describe-instances  
- ⚠️ **Blast Radius:** Subnet / VPC  
