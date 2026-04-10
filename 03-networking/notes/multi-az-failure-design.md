# — Multi-AZ & Failure Design  —  Production Operational Scenarios

---
## `single_az_deployment`

- **Situation:** Service deployed in single AZ only  
- **Symptom:** Complete outage, health checks failing, no reachable targets  
- **Root cause:** AZ-level failure, no redundancy  
- **Fix:** Distribute instances across ≥2 AZs + attach Load Balancer  
- ⚠️ **Risk:** Full service downtime, SLA breach  
---

## `load_balancer_az_scope`

- **Situation:** Load Balancer attached to single AZ subnet  
- **Symptom:** LB DNS resolves but traffic drops / 5xx spikes  
- **Root cause:** No cross-AZ target availability  
- **Fix:** Attach LB to multiple AZ subnets + enable cross-zone load balancing  
---

## `subnet_distribution_failure`

- **Situation:** Instances launched in one subnet (single AZ)  
- **Symptom:** No failover despite multi-AZ VPC design  
- **Root cause:** Improper subnet selection during deployment  
- **Fix:** Map ASG / instances to subnets across different AZs  
---

## `nat_gateway_single_az`

- **Situation:** One NAT Gateway used for all private subnets  
- **Symptom:** Outbound failures (API calls, package installs timeout)  
- **Root cause:** NAT in failed AZ, no redundancy  
- **Fix:** Deploy NAT per AZ + update route tables per subnet  
- ⚠️ **Risk:** All private workloads lose internet access  
---

## `asg_multi_az_misconfig`

- **Situation:** Auto Scaling Group restricted to one AZ  
- **Symptom:** No instance recovery after AZ failure  
- **Root cause:** ASG subnet config not multi-AZ  
- **Fix:** Attach ASG to multiple AZ subnets + enable health-based replacement  
---

## `db_single_az_deployment`

- **Situation:** Database deployed in single AZ  
- **Symptom:** Application errors (timeouts / connection refused)  
- **Root cause:** No failover replica available  
- **Fix:** Enable Multi-AZ (primary + standby) or cross-AZ replication  
- ⚠️ **Risk:** Data unavailability, possible data loss  
---

## `lb_health_check_failure`

- **Situation:** Unhealthy instances receiving traffic  
- **Symptom:** Random 5xx errors, partial outages  
- **Root cause:** Incorrect health check path/port/timeout  
- **Fix:** Configure correct health endpoint + tune thresholds  
---

## `cross_az_dependency_issue`

- **Situation:** App depends on service in another AZ  
- **Symptom:** Latency spikes, degraded performance under load  
- **Root cause:** Cross-AZ communication dependency  
- **Fix:** Co-locate tightly coupled services or use caching/replication  
---

## `az_failure_detection`

- **Situation:** AZ degradation impacting subset of resources  
- **Symptom:** Increased latency, partial request failures  
- **Root cause:** Underlying AZ infrastructure issue  
- **Fix:** Shift traffic via Load Balancer + scale in healthy AZs  
---
