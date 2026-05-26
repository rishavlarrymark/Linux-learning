# 🌐 NETWORKING — QUICK RECALL (Day 1–15)

---

# — How Networking Works

- Network → devices communicating
- Client → initiates request
- Server → responds
- IP → machine identity
- Port → service door
- Protocol → communication rules
- TCP → reliable (connection-oriented)
- UDP → fast (connectionless)
- Private IP → internal only
- Public IP → internet reachable
- Port 80 → HTTP
- Port 443 → HTTPS
- Port 22 → SSH
- Port 53 → DNS

### Traffic Flow
Client → DNS → IP → TCP → TLS → HTTP → Response

### Failure Signals
- `Could not resolve host` → DNS issue
- `Connection refused` → no service
- `Connection timed out` → firewall / routing
- Slow response → latency

### Debug Order
DNS → IP → TCP → TLS → HTTP

---

# — OSI & TCP/IP

- L3 → IP / Routing
- L4 → TCP / UDP
- L5 → Session control
- L6 → TLS / Encryption
- L7 → Application (HTTP, DNS, SSH)
- TCP/IP → Application / Transport / Internet / Network Access

### Layer Mapping
- `No route to host` → L3
- Port blocked → L4
- `SSL handshake failed` → L6
- `5xx error` → L7
- `Connection refused` → L4

### Debug Order
L3 → L4 → L6 → L7

---

# — IP & Subnetting

- IPv4 → 32-bit address
- CIDR → network size indicator
- /16 → ~65k IPs
- /24 → 256 IPs
- /28 → 16 IPs
- Smaller number → bigger network
- VPC CIDR → base block
- Subnet → CIDR division
- Overlap → routing conflict
- IP exhaustion → scaling stops
- 0.0.0.0/0 → default internet route

### Private Ranges
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

### Public vs Private
- Public subnet → route to IGW
- Private subnet → no IGW
- NAT → outbound only

### Failure Signals
- Overlapping CIDR → peering fails
- No IP left → instance launch fails
- Wrong subnet → unreachable host

### Debug Order
Check CIDR → Check Subnet → Check Route

---

# — DNS

- DNS → Name to IP
- Happens before TCP
- Port 53 → UDP / TCP
- A → IPv4
- AAAA → IPv6
- CNAME → alias
- TTL → cache duration
- Resolver → recursive lookup

### Resolution Flow
Client → Resolver → Root → Authoritative → IP

### Failure Signals
- `NXDOMAIN` → missing record
- Wrong IP → bad A record
- IP works, domain fails → DNS
- Some users fail → TTL cache
- `Temporary failure in name resolution` → DNS blocked
- SSL mismatch after DNS change → cached IP

### Debug Order
dig → Check IP → TTL → Follow CNAME → curl -v

---

# — Linux Networking (Server Debug)

- `ip a` → check IP & interface state
- `ip r` → check routing table
- `ping <gateway>` → test subnet reachability
- `ping 8.8.8.8` → test internet (L3)
- `ping domain.com` → test DNS + routing
- `ss -tuln` → check listening ports
- `curl localhost` → test local service
- `curl <private-ip>` → test service exposure
- `127.0.0.1` → loopback
- `0.0.0.0` → all interfaces
- default route → exit path

### Failure Signals
- `Network unreachable` → routing issue
- `Connection refused` → no service listening
- `Connection timed out` → firewall / block
- `Temporary failure in name resolution` → DNS issue

### Debug Order
IP → Route → Ping IP → Ping Domain → Port → Service

give me impotant 
---

# — Ports, Services & Reachability (Layer 4 Reality)

- Service listening → process bound to port
- Port → transport-layer entry point
- Binding → IP + Port combination
- 127.0.0.1 → local-only binding
- 0.0.0.0 → all interfaces
- Listening ≠ reachable
- Reachable IP ≠ reachable service
- Firewall → traffic filter (stateful/stateless)
- Connection refused → port closed
- Connection timeout → packet dropped
- TCP handshake → SYN → SYN-ACK → ACK

### Service Exposure Flow
Client → Route (L3) → Server IP → Port (L4) → Process → Response

### Failure Signals
- Works on localhost, fails externally → wrong binding
- Immediate failure → no listener
- Timeout → firewall / SG / NACL
- `curl` works, `ping` fails → ICMP blocked
- Port open but 5xx → application issue

### Debug Order
Check L3 first → Check listening port → Check binding → Check firewall → Test with curl → Inspect app logs

### Deep Rule
Layer 3 delivers to machine  
Layer 4 delivers to service

Both must succeed.

### Production Thinking
Listening + Allowed + Routed = Reachable

---

# — Network Debugging (Engineer Mode)

- traceroute → shows hop-by-hop path
- tcpdump → packet visibility on interface
- latency → response delay (RTT)
- packet loss → dropped packets
- hop → router step in traffic path
- ICMP → diagnostic protocol used by ping/traceroute
- path failure → traffic stops before destination
- stars (* * *) → hop not responding
- packets seen in tcpdump → traffic reached host
- no packets in tcpdump → traffic blocked before host

### Traffic Trace Flow
Client → Router → ISP → Internet → Cloud Gateway → Server → Application

### Failure Signals
- traceroute stops mid-path → routing issue
- continuous `* * *` → firewall / ICMP blocked
- high RTT in hops → latency
- packet drops in ping → packet loss
- tcpdump shows SYN but no response → service issue
- tcpdump shows nothing → traffic not reaching host

### Debug Order
Traceroute → Identify failing hop → tcpdump → Confirm packet arrival → Inspect routing / firewall → Verify service

### Deep Rule
Traceroute shows where traffic stops  
tcpdump shows whether traffic reached the host

### Production Thinking
Trace path + Confirm packets + Identify boundary = Root cause


---

# — AWS VPC (Network Isolation)

- VPC → private network inside cloud
- CIDR → defines VPC address space
- Subnets → network segmentation
- Route table → traffic direction
- IGW → internet connectivity
- NAT → private subnet internet access
- Security Group → stateful filtering
- NACL → stateless subnet firewall
- AZ → physical datacenter zone
- VPC → region-scoped network
- Default state → fully isolated
- Resources communicate using private IP

### Traffic Flow
Client → Internet → IGW → Route Table → Subnet → EC2 → Service

### Isolation Model
Internet → Public Subnet → Load Balancer → Private Subnet → Application → Database

### Failure Signals
- No internet access → missing IGW route
- Public instance unreachable → SG blocking inbound
- Private instance cannot reach internet → NAT missing
- Cross-VPC communication fails → no peering / TGW
- Subnet resources unreachable → wrong route table association

### Debug Order
Check VPC CIDR → Check Subnet → Check Route Table → Check IGW/NAT → Check Security Group → Check NACL

### Deep Rule
Routing decides **where traffic goes**  
Security decides **whether traffic is allowed**

### Production Thinking
Isolation + Routing + Security = Cloud Network

# — Subnets & Routing (Traffic Flow)

- VPC CIDR → base network range  
- Subnet → smaller network inside VPC  
- Subnets divide network for organization  
- Each subnet belongs to **one AZ**  
- Every subnet uses a **route table**  
- Route table → decides next hop  
- `Destination → Target` rule format  
- `10.0.0.0/16 → local` → internal traffic  
- `0.0.0.0/0 → IGW` → internet route  
- Public subnet → route to IGW  
- Private subnet → no direct IGW  
- NAT → private subnet outbound internet  
- Routing happens at **Layer 3**

### Traffic Flow

Internet  
→ Internet Gateway  
→ Route Table  
→ Public Subnet  
→ Load Balancer  
→ Private Subnet  
→ Application Server

### Public vs Private Logic

Public subnet  
- route to IGW  
- internet reachable

Private subnet  
- no IGW route  
- outbound via NAT

### Failure Signals

- Instance can't reach internet → missing `0.0.0.0/0`  
- Public service unreachable → subnet not public  
- Private server can't update packages → NAT missing  
- Internal service unreachable → wrong subnet / SG block

### Debug Order

Check Subnet  
→ Check Route Table  
→ Check Default Route  
→ Check IGW / NAT  
→ Check Security Group

### Deep Rule

Subnets decide **where resources live**  
Route tables decide **where traffic goes**

### Production Thinking

Correct subnet + correct route = reachable service

# — Internet & Private Access (IGW + NAT)

- IGW → connects VPC to internet  
- IGW → inbound + outbound  
- NAT → private → internet only  
- NAT → blocks inbound  
- Public subnet → route to IGW  
- Private subnet → route to NAT  
- NAT must be in public subnet  
- NAT uses Elastic IP  
- Internet access needs route + gateway  
- Routing happens at L3  

### Traffic Flow

Public:
Internet → IGW → EC2  

Private:
EC2 → NAT → IGW → Internet  

---

### Public vs Private Logic

Public subnet  
- route → IGW  
- inbound + outbound  

Private subnet  
- route → NAT  
- outbound only  

---

### Failure Signals

- Public instance no internet → IGW missing  
- Public service unreachable → no public IP / SG block  
- Private instance no internet → NAT missing  
- Timeout → route / firewall issue  
- Works locally, fails externally → exposure issue  

---

### Debug Order

Check Subnet  
→ Check Route Table  
→ Check IGW / NAT  
→ Check Security Group  
→ Test connectivity  

---

### Deep Rule

IGW → full internet access  
NAT → outbound-only access  

---

### Production Thinking

Public = exposed  
Private = protected  

Route + Gateway + Security = internet access

---

# — Network Security (Security Groups & NACLs)

- Security Group (SG) → instance-level firewall  
- NACL → subnet-level firewall  
- SG → stateful (auto allow return traffic)  
- NACL → stateless (explicit allow needed both ways)  
- SG → allow rules only  
- NACL → allow + deny rules  
- SG attached to → EC2 / ENI / LB / RDS  
- NACL attached to → subnet  
- Ephemeral ports → 1024–65535  
- Traffic passes → NACL → SG → Instance  

---

### Failure Signals

- Website not opening → SG inbound missing (80/443)  
- Works locally, not externally → SG or binding issue  
- Timeout → NACL / SG / routing block  
- Intermittent failure → missing ephemeral ports  
- EC2 cannot access internet → outbound blocked  

---

### Debug Order

Check Route Table  
→ Check Security Group  
→ Check NACL  
→ Check Service Binding  

---

# — Multi-AZ & Failure Design

- AZ → isolated datacenter zone
- Multi-AZ → high availability architecture
- One subnet per AZ
- Public subnet per AZ
- Private subnet per AZ
- Load Balancer → distributes traffic across AZs
- Health checks → remove unhealthy targets
- NAT should exist per AZ
- Single AZ deployment → SPOF
- Multi-AZ DB → failover support
- Failover → traffic shifts to healthy AZ
- Redundancy → avoids outage
- Blast radius → impact scope of failure

### Traffic Flow

Client  
→ Load Balancer  
→ Healthy AZ  
→ Application Server  
→ Database

### Failure Signals

- Entire app down after AZ failure → single-AZ deployment
- Private subnet lost internet → NAT failure
- Traffic routed to dead instance → failed health checks
- One AZ overloaded → uneven target distribution
- DB outage during failover → standby unhealthy

### Debug Order

Check AZ health  
→ Check target health  
→ Check subnet per AZ  
→ Check NAT availability  
→ Check failover routing

### Deep Rule

High availability = redundancy across AZs

### Production Thinking

Design for failure, not perfect uptime

---

# — Observability & Traffic Visibility

- VPC Flow Logs → network traffic metadata
- ACCEPT → traffic allowed
- REJECT → traffic blocked
- Flow Logs show:
  - source IP
  - destination IP
  - port
  - protocol
  - action
- Flow Logs ≠ packet payload
- SG/NACL failures visible in Flow Logs
- tcpdump → packet visibility
- `ss -tuln` → listening ports
- Observability → evidence-based debugging

### Traffic Flow

Client  
→ DNS  
→ Route  
→ Security  
→ Service  
→ Application

### Failure Signals

- REJECT on 443 → HTTPS blocked
- ACCEPT but app fails → application issue
- No packets in tcpdump → traffic not reaching host
- Domain fails, IP works → DNS issue
- Connection refused → service not listening

### Debug Order

Check Flow Logs  
→ ACCEPT or REJECT  
→ Check Route  
→ Check SG/NACL  
→ Check Listening Service  
→ Check Application

### Deep Rule

If you cannot see traffic, you are guessing

### Production Thinking

Logs + packet visibility = real debugging

---

# — Failure Scenarios (INTERVIEW GOLD)

- DNS failure → domain unreachable
- Route failure → no traffic path
- SG block → port denied
- NACL block → subnet traffic denied
- Wrong binding → localhost-only service
- NAT failure → no outbound internet
- Connection refused → no listener
- Timeout → dropped traffic
- Blast radius → scope of impact
- Production debugging → layer isolation

### Failure Flow

Client  
→ DNS  
→ Route  
→ Security  
→ Service  
→ Application

### Failure Signals

- `Could not resolve host` → DNS
- `No route to host` → routing issue
- `Connection refused` → service issue
- `Connection timed out` → SG/NACL/firewall
- Works locally only → wrong bind IP
- Private EC2 no internet → NAT missing

### Debug Order

DNS  
→ Route  
→ Security  
→ Service Binding  
→ Application

### Deep Rule

Every failure exists at a specific layer

### Production Thinking

Never guess — isolate the failing layer

---

# — Advanced Awareness

- VPC Endpoint → private AWS service access
- Peering → VPC ↔ VPC connectivity
- Transit Gateway → centralized VPC routing hub
- VPN → encrypted internet tunnel
- Direct Connect → dedicated private AWS link
- PrivateLink → private service exposure
- TGW → hub-and-spoke architecture
- Peering → no transitive routing
- Overlapping CIDR → routing conflict
- Hybrid cloud → on-prem ↔ AWS

### Traffic Flow

On-Prem  
→ VPN / Direct Connect  
→ AWS VPC  
→ Application

### Private AWS Access Flow

Private EC2  
→ VPC Endpoint  
→ AWS Service

### Failure Signals

- VPCs can't communicate → missing peering/TGW route
- Private EC2 uses internet for S3 → no VPC endpoint
- Hybrid traffic unstable → VPN issue
- Peering fails → overlapping CIDR
- Large routing complexity → mesh peering problem

### Debug Order

Check Connectivity Type  
→ Check Route Tables  
→ Check CIDR Overlap  
→ Check Gateway/TGW  
→ Check Security Rules

### Deep Rule

Advanced networking exists to simplify secure connectivity

### Production Thinking

Scale connectivity without exposing infrastructure
