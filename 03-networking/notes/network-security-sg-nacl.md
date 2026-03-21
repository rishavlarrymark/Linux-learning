# DAY 11 — NETWORK SECURITY (SECURITY GROUPS & NACLs)
## Lifetime Production + Interview Reference Notes

---

## 🧠 CORE PRINCIPLE

Routing decides WHERE traffic goes.
Security decides WHETHER it is allowed.

If traffic fails → check security before application.

---

## 🔐 SECURITY GROUP (SG)

### Definition
Instance-level virtual firewall.

### Properties
- Stateful
- Allow rules only (no deny)
- Return traffic automatically allowed

### Scope
Attached to:
- EC2
- Load Balancer
- RDS
- ENI (Elastic Network Interface)

---

### Example (Web Server SG)

Inbound:
- 80 (HTTP) → 0.0.0.0/0
- 443 (HTTPS) → 0.0.0.0/0
- 22 (SSH) → YOUR_IP only

Outbound:
- Allow all (default)

---

### Key Production Insight

If inbound is allowed:
→ response is automatically allowed

NO need to open ephemeral ports in SG.

---

## 🚧 NETWORK ACL (NACL)

### Definition
Subnet-level firewall.

### Properties
- Stateless
- Allow + Deny rules
- Must explicitly allow request + response

### Scope
Attached to:
- Subnets

---

### Example (HTTP Traffic)

Inbound:
- ALLOW 80 FROM 0.0.0.0/0

Outbound:
- ALLOW 1024-65535 TO 0.0.0.0/0

---

### Key Production Insight

If response ports (ephemeral) not allowed:
→ connection FAILS silently

---

## ⚖️ SG vs NACL (CRITICAL DIFFERENCE)

| Feature | Security Group | NACL |
|--------|---------------|------|
| Level | Instance | Subnet |
| Type | Stateful | Stateless |
| Return Traffic | Auto allowed | Must allow explicitly |
| Rules | Allow only | Allow + Deny |
| Complexity | Simple | Error-prone |

---

## 🔁 TRAFFIC FLOW

Internet → IGW → Subnet → NACL → Security Group → EC2 → Application

ALL layers must allow traffic.

---

## 🔥 EPHEMERAL PORTS (VERY IMPORTANT)

Range:
1024–65535

Used by:
- Client side of connection
- Return traffic

NACL must allow this range.

SG does NOT require this explicitly.

---

## 🚨 REAL FAILURE SCENARIOS

### 1. Website Not Opening
- SG inbound missing (80/443)
- NACL blocking inbound or outbound
- Wrong source CIDR

---

### 2. Works on localhost, not externally
- Service binding issue (127.0.0.1 vs 0.0.0.0)
- SG blocking inbound

---

### 3. Connection Timeout
- Traffic not reaching instance
- Likely NACL / SG / routing issue

---

### 4. Connection Refused
- Instance reachable
- But no service listening

---

### 5. Intermittent Failure
- NACL missing ephemeral ports
- Partial allow rules

---

## 🧪 DEBUG ORDER (MANDATORY)

1. Route table
2. Security Group
3. NACL
4. Service binding (ss -tuln)

Never change order.

---

## 🎯 INTERVIEW QUESTIONS

### Q1: Difference between SG and NACL?

Answer:
Security Groups are stateful and operate at instance level, automatically allowing return traffic.
NACLs are stateless, operate at subnet level, and require explicit rules for both inbound and outbound traffic.

---

### Q2: Why is my EC2 reachable internally but not from internet?

Answer:
Likely causes:
- Security Group inbound rules missing
- NACL blocking inbound
- No public IP or IGW route

---

### Q3: Why does traffic timeout?

Answer:
Timeout indicates traffic is not reaching destination, usually due to routing or firewall restrictions.

---

### Q4: Why does connection get refused?

Answer:
Connection refused means destination is reachable but no service is listening on that port.

---

## 🏗️ PRODUCTION BEST PRACTICES

- Use Security Groups as primary control
- Keep NACLs simple (or default)
- Restrict SSH access (never 0.0.0.0/0)
- Use least privilege rules
- Avoid overlapping/conflicting rules

---

## 🔒 LIFETIME RULES

1. If traffic blocked → check security before app
2. SG = stateful, NACL = stateless
3. Always allow ephemeral ports in NACL
4. Keep NACL simple, control via SG
5. Never expose backend directly to internet

---

## 🧠 FINAL MENTAL MODEL

Traffic success requires:

Route ✅
Security (SG + NACL) ✅
Service listening ✅

If any one fails → system fails.

---
