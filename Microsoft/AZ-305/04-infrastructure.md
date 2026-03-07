# 🔴 Module 4: Design Infrastructure Solutions (25–30%)

📘 [← Back to AZ-305 Index](./index.md)

> ⚠️ This is the **largest domain** at 25–30%. It covers compute, application architecture, migrations, and networking.

---

## 4.1 Design Compute Solutions

### 💻 Compute Service Selection — Decision Framework

| Service | Type | Best For |
|---------|------|---------|
| **Azure Virtual Machines** | IaaS | Full OS control; lift-and-shift; legacy apps |
| **Azure VM Scale Sets (VMSS)** | IaaS | Auto-scaling groups of identical VMs |
| **Azure App Service** | PaaS | Web apps, APIs, mobile backends — no OS management |
| **Azure Functions** | Serverless | Event-driven; short-lived; scale to zero |
| **Azure Container Instances (ACI)** | Serverless containers | Simple, single containers; burst scenarios |
| **Azure Container Apps (ACA)** | Managed containers | Microservices; KEDA scaling; Dapr support |
| **Azure Kubernetes Service (AKS)** | Managed Kubernetes | Complex container orchestration; full control |
| **Azure Batch** | HPC job scheduling | Large-scale parallel compute jobs |
| **Azure Spring Apps** | Managed Java PaaS | Java/Spring Boot microservices |

**IaaS vs PaaS vs Serverless Decision:**

```
Do you need OS-level access or specific software?
├── Yes → Azure VMs (IaaS)
└── No → Is it a web/API workload?
          ├── Yes → App Service (PaaS) or Azure Functions (serverless)
          └── No → Is it a containerised workload?
                   ├── Simple, burst → ACI
                   ├── Microservices with scaling → Azure Container Apps
                   └── Complex orchestration → AKS
```

---

### 🖥️ Virtual Machine Design

**VM Design Considerations:**

| Factor | Options |
|--------|---------|
| **Size** | General purpose (D-series), Memory (E-series), Compute (F-series), GPU (N-series), Storage (L-series) |
| **Disk** | OS Disk + Data Disk; Premium SSD (production), Standard SSD (dev), Ultra Disk (< 1ms latency) |
| **Availability** | Availability Zones (99.99%) > Availability Sets (99.95%) > Single VM (99.9%) |
| **Scaling** | VM Scale Sets for horizontal scaling; larger SKU for vertical |
| **Networking** | VNet + subnet; NSG; Accelerated Networking for low latency |

**VM Scale Sets (VMSS) Key Points:**
- Identical VMs managed as a group
- Auto-scales based on metrics (CPU, memory, custom)
- Supports Availability Zones for zone-redundant scaling
- **Uniform mode:** All VMs identical (recommended for stateless)
- **Flexible mode:** Mix VM sizes, manual and auto-scale

---

### 🌐 App Service Design

**Plan Selection:**

| Plan | SLA | Scaling | Slots | VNet Integration |
|------|-----|---------|-------|-----------------|
| Free / Shared | None | No | No | No |
| Basic | 99.95% | Manual | No | No |
| Standard | 99.95% | Auto | Yes (5) | No |
| Premium v2/v3 | 99.95% | Auto | Yes (20) | Yes (Regional) |
| Isolated v2 (ASE) | 99.99% | Auto | Yes (20) | Yes (Dedicated VNet) |

**App Service Environment (ASE):**
- Fully isolated, dedicated environment within your VNet
- Used when: all traffic must stay on private network; compliance; maximum isolation
- Higher cost — dedicated infrastructure per customer

---

### ☸️ Azure Kubernetes Service (AKS) Design

**AKS Key Design Decisions:**

| Decision | Options |
|----------|---------|
| **Node pool OS** | Linux (default) or Windows node pools |
| **VM size for nodes** | D-series for general; N-series for GPU workloads |
| **Availability** | Zone-redundant node pools across Availability Zones |
| **Autoscaling** | Cluster Autoscaler (node-level); KEDA (pod-level) |
| **Networking** | Kubenet (basic) vs Azure CNI (pod gets VNet IP — required for integration) |
| **Authentication** | Entra ID integration for RBAC |
| **Image registry** | Azure Container Registry (ACR) — attach to AKS for seamless pull |
| **Ingress** | NGINX ingress controller or Azure Application Gateway Ingress Controller |

**Workload Identity (AKS → Azure services):**
- Replaces pod-managed identity (deprecated)
- Kubernetes service account federated with Entra Workload Identity
- Pods get tokens for Azure services without secrets

---

## 4.2 Design an Application Architecture

### 🏗️ Application Architecture Patterns

**Microservices vs Monolith vs Serverless:**

| Architecture | Characteristics | Deploy On |
|-------------|----------------|----------|
| **Monolith** | Single deployable unit; simple to start; harder to scale independently | App Service, VM |
| **Microservices** | Independent services; independent scaling and deployment | AKS, Container Apps |
| **Serverless** | No infrastructure management; event-driven; pay-per-execution | Azure Functions |
| **Event-driven** | Services communicate via events; loose coupling | Functions + Event Grid/Service Bus |

---

### 📨 Messaging and Event Services — Design Comparison

| Service | Pattern | Max Size | Ordering | When to Use |
|---------|---------|---------|---------|------------|
| **Storage Queue** | Point-to-Point | 64 KB | No | Simple, cheap, high-volume queuing |
| **Service Bus Queue** | Point-to-Point | 256 KB (Standard) | With Sessions | Enterprise messaging, DLQ, transactions |
| **Service Bus Topic** | Pub/Sub | 256 KB (Standard) | With Sessions | Multiple independent consumers |
| **Event Grid** | Reactive Pub/Sub | 1 MB | No | React to Azure events; serverless triggers |
| **Event Hubs** | Streaming | 1 MB | Per-partition | High-throughput ingestion; IoT; analytics |

**Decision Questions:**
- Need guaranteed delivery + DLQ + transactions? → **Service Bus**
- React to Azure resource events (blob created, VM started)? → **Event Grid**
- Ingest millions of events per second from IoT devices? → **Event Hubs**
- Simple decoupling with low cost? → **Storage Queue**

---

### 🔀 API Management (APIM) — Architecture Role

**APIM as the API Gateway:**
```
External Clients
    ↓
Azure Front Door (WAF + CDN)
    ↓
API Management (auth, throttling, transformation, caching)
    ↓
Backend Services (App Service, Functions, AKS, on-premises)
```

**Design Considerations:**

| Requirement | APIM Feature |
|-------------|-------------|
| Rate limit API consumers | `rate-limit` policy per subscription |
| Validate JWT tokens | `validate-jwt` policy |
| Cache responses | `cache-lookup` + `cache-store` policies |
| Route to different backends | `set-backend-service` policy |
| Mock responses during development | `mock-response` policy |
| Internal APIs only (no public exposure) | Internal VNet mode or Private Endpoint |

**APIM Tiers for AZ-305:**
- **Developer** — No SLA; non-production
- **Basic** — SLA; lower throughput
- **Standard** — SLA; production workloads
- **Premium** — Multi-region, VNet support, zone redundancy; enterprise

---

### ⚡ Azure Functions — Hosting Plan Selection

| Plan | Cold Start | VNET | Best For |
|------|-----------|------|---------|
| **Consumption** | Yes | No | Infrequent, event-driven |
| **Flex Consumption** | Minimal | Yes | Variable load + VNet |
| **Premium** | No | Yes | Consistent load, no cold start |
| **Dedicated (App Service)** | No | Yes | Always-on; predictable billing |
| **Container Apps** | No | Yes | Containerised functions |

---

### 🗃️ Azure Cache for Redis — Architecture Patterns

| Pattern | Description |
|---------|-------------|
| **Cache-aside** | App checks cache first; if miss, reads from DB, writes to cache |
| **Write-through** | Write to cache and DB simultaneously |
| **Session store** | Stateless app servers store session state in Redis |
| **Pub/Sub** | Real-time messaging between services |
| **Sorted sets** | Real-time leaderboards |

---

## 4.3 Design Migrations

### 🚚 Azure Migrate — Migration Hub

Azure Migrate is the central hub for discovering, assessing, and migrating on-premises workloads to Azure.

**Azure Migrate Tools:**

| Tool | Purpose |
|------|---------|
| **Discovery and Assessment** | Discovers on-premises VMs; assesses readiness; recommends Azure SKUs; estimates costs |
| **Server Migration** | Agent-based or agentless replication of VMs to Azure |
| **Database Migration Service (DMS)** | Migrates SQL Server, Oracle, MySQL, PostgreSQL to Azure |
| **App Service Migration Assistant** | Assesses and migrates .NET/PHP web apps to App Service |
| **Data Migration Assistant (DMA)** | Assesses SQL Server DBs for Azure SQL compatibility |

**Migration Strategies — The "7 Rs":**

| Strategy | Description | When |
|----------|-------------|------|
| **Retire** | Decommission — no longer needed | Unused or end-of-life apps |
| **Retain** | Keep on-premises for now | Compliance, latency, not ready |
| **Rehost (Lift & Shift)** | Move as-is to Azure VMs | Quick migration; minimal change |
| **Replatform** | Minimal changes to use PaaS | Move SQL Server → Azure SQL MI |
| **Repurchase** | Replace with SaaS | Move CRM → Dynamics 365 / Salesforce |
| **Refactor / Re-architect** | Redesign for cloud-native | Microservices, containerisation |
| **Rebuild** | Build from scratch | Legacy apps with no migration path |

---

### 🗄️ Database Migration Design

**Migration Paths:**

| Source | Target | Tool |
|--------|--------|------|
| SQL Server (on-prem) | Azure SQL Database | DMS (online/offline) |
| SQL Server (on-prem) | Azure SQL Managed Instance | DMS (online) |
| SQL Server (on-prem) | SQL Server on Azure VM | Backup/restore; log shipping |
| Oracle | Azure Database for PostgreSQL | SSMA for Oracle |
| MySQL (on-prem) | Azure Database for MySQL | DMS |
| PostgreSQL (on-prem) | Azure Database for PostgreSQL | DMS |
| MongoDB | Cosmos DB (MongoDB API) | Data Migration Tool; ADF |

**Online vs Offline Migration:**

| | Offline | Online |
|--|---------|--------|
| Downtime required | Yes | Minimal (cutover only) |
| How | Full backup + restore | Continuous replication + cutover |
| Best for | Small DBs; tolerable downtime | Large DBs; near-zero downtime |

---

## 4.4 Design Network Solutions

### 🌐 Azure Virtual Network (VNet) Design

**Core Networking Concepts:**

| Concept | Description |
|---------|-------------|
| **VNet** | Isolated network in Azure; defines IP address space |
| **Subnet** | Subdivision of a VNet; resources are deployed into subnets |
| **NSG (Network Security Group)** | Stateful L4 firewall — allow/deny rules on subnets or NICs |
| **ASG (Application Security Group)** | Logical grouping of VMs for NSG rules — no IP management |
| **Route Table (UDR)** | Custom routes — override Azure's default routing |
| **Service Endpoint** | Private route from VNet to Azure PaaS over Azure backbone |
| **Private Endpoint** | Private IP in your VNet for an Azure PaaS service — recommended |

---

### 🔗 VNet Connectivity Options

| Option | Description | Use When |
|--------|-------------|---------|
| **VNet Peering** | Direct, low-latency VNet-to-VNet connectivity | Same or cross-region VNet connectivity |
| **VPN Gateway** | Encrypted tunnel over public internet | On-premises to Azure; site-to-site or point-to-site |
| **ExpressRoute** | Private, dedicated connection via MSP | Mission-critical on-prem connectivity; compliance |
| **ExpressRoute + VPN Gateway** | Failover — ExpressRoute primary, VPN as backup | Maximum reliability for hybrid connectivity |
| **Azure Virtual WAN** | Managed hub-and-spoke for global connectivity | Large enterprise; many branches/VNets |
| **Hub-and-Spoke** | Central hub VNet with shared services; spokes connect via peering | Enterprise topology with shared firewall, DNS |

**VPN Gateway SKUs:**
- **Basic** — Dev/test; no zone redundancy
- **VpnGw1–5** — Production; zone-redundant variants (`VpnGw1AZ`+)
- Max throughput: VpnGw5 = 10 Gbps

**ExpressRoute vs VPN Gateway:**

| | VPN Gateway | ExpressRoute |
|--|------------|-------------|
| Connectivity | Over internet (encrypted) | Private (via MSP/co-location) |
| Max bandwidth | 10 Gbps | Up to 100 Gbps |
| Latency | Variable | Consistent, low |
| SLA | 99.9–99.99% | 99.95–99.99% |
| Cost | Lower | Higher |
| Setup time | Hours | Weeks–months |

---

### 🔒 Azure Firewall and Network Security

| Service | Layer | Purpose |
|---------|-------|---------|
| **NSG** | L4 | Subnet/NIC-level allow/deny rules |
| **Azure Firewall** | L4–L7 | Centralised, stateful firewall for entire VNet or hub |
| **Azure Firewall Premium** | L4–L7 + IDPS | TLS inspection, IDPS, URL filtering |
| **Azure DDoS Protection** | L3–L4 | Volumetric attack protection |
| **Web Application Firewall (WAF)** | L7 | OWASP protection on App Gateway / Front Door |

**Hub-and-Spoke with Azure Firewall:**
```
On-premises
    ↓ (ExpressRoute / VPN)
Hub VNet
├── Azure Firewall (all spoke traffic routes through here via UDR)
├── Azure Bastion (secure VM access)
├── VPN/ExpressRoute Gateway
└── DNS Resolver
    ↓ (VNet Peering — no transitive routing without Firewall/NVA)
Spoke VNets (workload VNets)
```

---

### 🌍 DNS Design

| Service | Use For |
|---------|---------|
| **Azure DNS** | Host public DNS zones; manage DNS records |
| **Azure Private DNS** | Name resolution within VNets (private zones) |
| **Azure DNS Private Resolver** | Forward DNS queries between on-premises and Azure private zones |
| **Custom DNS** | Use your own DNS servers (e.g., Active Directory DNS) |

**Private Endpoint DNS:**
When a private endpoint is created, a DNS record must be added to the private DNS zone (e.g., `privatelink.blob.core.windows.net`) so that clients resolve to the private IP rather than the public IP.

---

### 🛡️ Azure Bastion

Azure Bastion provides **browser-based, TLS-encrypted RDP/SSH** to Azure VMs directly from the Azure portal — without exposing VMs to the public internet.

**Tiers:**

| Tier | Features |
|------|---------|
| **Developer** | Shared infrastructure; no custom VNet |
| **Basic** | Dedicated; RDP/SSH from portal |
| **Standard** | File transfer, native client support, host scaling, custom ports |
| **Premium** | Private-only Bastion; Session Recording (Preview) |

> 💡 **AZ-305 tip:** Remove public IPs from all VMs and use Azure Bastion for access. This eliminates the attack surface of direct RDP/SSH exposure.

---

### 📊 Network Design Patterns for AZ-305

**Zero-Trust Network Design:**
- Never assume a request is safe because it comes from inside the VNet
- Verify every request: Conditional Access + NSG + Azure Firewall + Private Endpoints
- Use Private Endpoints for all PaaS services — eliminate public endpoints

**Three-tier Application Network Design:**
```
Internet
    ↓
Application Gateway (WAF) — public subnet
    ↓
App Service / AKS — app subnet (no public IP; VNet integrated)
    ↓
Azure SQL / Cosmos DB — Private Endpoint in data subnet
```

---

📘 [← Back to AZ-305 Index](./index.md)
