# 🟡 Module 3: Design Business Continuity Solutions (10–15%)

📘 [← Back to AZ-305 Index](./index.md)

---

## Key Concepts — RPO and RTO

Before diving into services, understand these two critical metrics:

| Metric | Definition | Question it answers |
|--------|-----------|-------------------|
| **RPO** (Recovery Point Objective) | Maximum acceptable data loss measured in time | "How much data can we afford to lose?" |
| **RTO** (Recovery Time Objective) | Maximum acceptable downtime before service must be restored | "How quickly must we recover?" |

**Design Rule:**
- Lower RPO → more frequent backups or continuous replication → **higher cost**
- Lower RTO → pre-provisioned hot standby → **higher cost**
- Balance both against business impact and budget

---

## 3.1 Design a Solution for Backup and Disaster Recovery

### 💾 Azure Backup

Azure Backup is a centralised, cloud-native backup service. Data is stored in a **Recovery Services Vault** or **Backup Vault**.

**What Azure Backup Protects:**

| Workload | Vault Type | Backup Frequency | Retention |
|---------|-----------|-----------------|-----------|
| Azure VMs | Recovery Services Vault | Daily (Enhanced: hourly) | Up to 99 years |
| Azure SQL in VM | Recovery Services Vault | Up to every 15 min (log) | Up to 99 years |
| Azure SQL Database | Recovery Services Vault | Full/Differential/Log | Up to 35 days (PITR) |
| Azure Files | Recovery Services Vault | Daily or hourly | Up to 180 days |
| Azure Blobs | Backup Vault | Continuous (operational) | 1–360 days |
| Azure Disks | Backup Vault | Every 1/2/4/8/12/24 hrs | Up to 30 days |
| Azure Kubernetes Service | Backup Vault | Daily/hourly | Up to 360 days |
| On-premises (via MARS agent) | Recovery Services Vault | Up to 3x/day | Up to 99 years |

**Vault Types Comparison:**

| | Recovery Services Vault | Backup Vault |
|--|------------------------|-------------|
| Supports VMs, SQL, Files | ✅ | ❌ |
| Supports Blobs, Disks, AKS | ❌ | ✅ |
| Geo-redundant storage | ✅ (default) | ✅ |
| Cross-region restore | ✅ (optional) | ✅ |

**Backup Storage Redundancy:**

| Option | Copies | Regions | Use When |
|--------|--------|---------|---------|
| **LRS** (Locally Redundant) | 3 | 1 (same DC) | Dev/test; not recommended for production |
| **ZRS** (Zone Redundant) | 3 | 1 (3 zones) | Production — zone-level resilience |
| **GRS** (Geo-Redundant) | 6 | 2 | Production — region-level DR; default |
| **GZRS** | 6 | 2 (3 zones primary) | Maximum durability |

> 💡 **AZ-305 tip:** GRS is the default for Recovery Services Vaults and allows **Cross-Region Restore** — restoring backups to the paired Azure region if the primary region is unavailable.

---

### 🔒 Backup Security Design

| Feature | Description |
|---------|-------------|
| **Soft Delete** | Deleted backup data retained for 14 days; protects against ransomware |
| **Multi-User Authorisation (MUA)** | Requires a second user to approve destructive operations (delete, disable) |
| **Immutable Vault** | Backup data cannot be deleted or modified for a defined period |
| **Private Endpoints** | Backup traffic stays on private network — no public internet |
| **Encryption** | Backups encrypted at rest with platform keys by default; Customer-managed keys (CMK) in Key Vault available |

**Backup Center:** Centralised management portal for all backup instances across subscriptions and vaults — enabling at-scale monitoring, alerting, and reporting.

---

### 🌐 Azure Site Recovery (ASR)

Azure Site Recovery provides **disaster recovery** through continuous replication — enabling failover to Azure (or another Azure region) with near-zero RPO.

**Supported Scenarios:**

| Source | Target | Description |
|--------|--------|-------------|
| Azure VMs → Azure | Another Azure region | Cross-region DR for Azure VMs |
| On-premises VMware/Hyper-V VMs | Azure | Migrate or DR to Azure |
| Physical servers | Azure | Bare-metal DR to cloud |

**ASR Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Replication** | Continuous block-level replication to target region (RPO typically < 1 min) |
| **Recovery Plan** | Ordered, scripted failover of multiple VMs as a group |
| **Test Failover** | Non-disruptive DR drill using a test VNet — no impact on production |
| **Failover** | Planned (zero data loss) or Unplanned (possible data loss if source unavailable) |
| **Failback** | Reprotect and return to original region after primary is restored |
| **Recovery Point** | App-consistent (VSS snapshot) or Crash-consistent checkpoint |

**ASR vs Azure Backup:**

| | Azure Backup | Azure Site Recovery |
|--|-------------|-------------------|
| Purpose | Data protection (restore files/VMs) | DR orchestration (fast failover) |
| RPO | Hours (daily backup) | Minutes (continuous replication) |
| RTO | Hours (restore from backup) | Minutes (pre-replicated ready to start) |
| Data loss | Up to 24 hours (or backup frequency) | Typically < 5 minutes |
| Use case | Accidental deletion, corruption | Region outage, complete site failure |

> 💡 **AZ-305 design rule:** Use **both** Azure Backup AND Azure Site Recovery for complete protection — backup for data recovery, ASR for rapid DR failover.

---

## 3.2 Design for High Availability

### 📊 SLA Design and Availability Zones

**Availability Zones:** Physically separate datacentres within an Azure region, connected by high-speed private networks. Zone failures are independent.

**SLA Uplift from Availability Zones:**

| Configuration | Approximate SLA |
|---------------|----------------|
| Single VM (Premium SSD) | 99.9% |
| VMs in Availability Set | 99.95% |
| VMs in Availability Zones | 99.99% |

**Composite SLA Calculation:**
When services are chained in series, multiply individual SLAs:
```
App Service (99.95%) × SQL Database (99.99%) × Key Vault (99.99%)
= 0.9995 × 0.9999 × 0.9999 = 99.93%
```

When services are in parallel (redundant), availability increases:
```
1 - (1 - 0.999) × (1 - 0.999) = 1 - 0.000001 = 99.9999%
```

---

### ⚖️ Load Balancing Services — Design Comparison

| Service | Layer | Scope | Use Case |
|---------|-------|-------|---------|
| **Azure Load Balancer** | L4 (TCP/UDP) | Regional | Internal or public load balancing of VMs |
| **Application Gateway** | L7 (HTTP/HTTPS) | Regional | URL-path routing, SSL offload, WAF for web apps |
| **Azure Front Door** | L7 (HTTP/HTTPS) | Global | Global HTTP load balancing, WAF, CDN acceleration |
| **Azure Traffic Manager** | DNS-based | Global | Route DNS traffic across regions/endpoints |

**Decision Tree:**

```
Is it HTTP/HTTPS traffic?
├── No  → Azure Load Balancer (L4)
└── Yes → Is it global (multi-region)?
          ├── No  → Application Gateway (regional WAF + path routing)
          └── Yes → Azure Front Door (global WAF + performance)
                    OR Traffic Manager (DNS failover between regions)
```

**Traffic Manager Routing Methods:**

| Method | Description |
|--------|-------------|
| **Priority** | Route to primary; failover to secondary if primary unhealthy |
| **Weighted** | Distribute traffic across endpoints by percentage |
| **Performance** | Route to lowest-latency endpoint for the user |
| **Geographic** | Route based on user's geographic location (data sovereignty) |
| **Multivalue** | Return all healthy endpoints; client selects |
| **Subnet** | Map IP ranges to specific endpoints |

---

### 🌍 Multi-Region Architecture Patterns

| Pattern | Description | RTO | RPO | Cost |
|---------|-------------|-----|-----|------|
| **Hot-Hot (Active-Active)** | Both regions serve live traffic | Near-zero | Near-zero | Highest |
| **Hot-Warm (Active-Passive with pre-provisioned)** | Secondary region is pre-provisioned, receives no traffic until failover | Minutes | Minutes | High |
| **Hot-Cold (Active-Passive with re-provisioning)** | Secondary spun up from IaC only after failure | Hours | Hours | Lower |

**Azure Paired Regions:**
- Every Azure region is paired with another region in the same geography
- Pair regions are used for GRS storage replication and sequential updates (not both updated simultaneously)
- Example pairs: East US ↔ West US 2; UK South ↔ UK West; West Europe ↔ North Europe

---

### 🏢 Availability Sets vs Availability Zones

| Feature | Availability Set | Availability Zone |
|---------|-----------------|------------------|
| Protects against | Rack/server failure (fault domain) | Datacentre failure |
| SLA uplift | 99.95% | 99.99% |
| VM location | Same datacentre | Different datacentre |
| Network latency | Very low | Low |
| Cost | No extra charge | Potential egress charges |
| Recommendation | Legacy — use for older VM generations | **Preferred for new deployments** |

---

### 📋 Well-Architected Framework — Reliability Pillar

The Azure Well-Architected Framework (WAF) Reliability pillar guides resilient architecture design:

| Principle | Key Practices |
|-----------|--------------|
| **Design for failure** | Assume components will fail; design redundancy |
| **Observe application health** | Implement health probes, monitoring, alerting |
| **Drive automation** | Automate recovery; avoid manual failover steps |
| **Define targets** | Define explicit RPO/RTO/SLO for every tier |
| **Test regularly** | Run chaos experiments and DR drills (ASR test failover) |

---

📘 [← Back to AZ-305 Index](./index.md)
