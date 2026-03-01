# Domain 2 – Describe Azure Architecture and Services `35–40%`

> ⬅️ [Back to AZ-900 Index](./index.md)

This is the **largest domain** — covers the core Azure services you need to know.

---

## 2.1 Azure Global Infrastructure

### Regions
- A **region** is a geographical area containing one or more datacenters
- Azure has **60+ regions** across the globe — largest cloud footprint
- When deploying resources, you choose a region
- Some services are **region-specific**, others are **global** (Microsoft Entra ID, Azure DNS)
- **Region pairs** — each region is paired with another for disaster recovery (e.g., East US ↔ West US)

### Availability Zones
- **Physically separate datacenters** within a single Azure region
- Each zone has independent power, cooling, and networking
- A minimum of **3 zones** per region that supports them
- Protects against **datacenter-level failures** (not just server failures)
- Services can be **zone-redundant** (auto-spread) or **zonal** (pinned to specific zone)

### Availability Sets (Legacy)
- Protects VMs against **hardware failures within a datacenter** (not datacenter-wide)
- **Fault Domains** — different physical racks (power + networking)
- **Update Domains** — VMs rebooted in different groups during planned maintenance
- Used for: VMs only; being superseded by Availability Zones for new deployments

### Geography
- A **geography** is a market (e.g., United States, Europe, Asia Pacific)
- Contains 2+ regions for data residency and compliance

### Azure Infrastructure Hierarchy

```
Geography (market)
└── Region (datacenter area)
    └── Availability Zone (isolated datacenter)
        └── Datacenter (physical building)
            └── Server Racks → Servers
```

### Azure Resource Hierarchy

```
Management Groups (top-level org governance)
└── Subscriptions (billing + access boundary)
    └── Resource Groups (logical container)
        └── Resources (VMs, storage, databases, etc.)
```

### Key Terms

| Term | Description |
|------|-------------|
| **Resource** | A single Azure service instance (e.g., one VM, one storage account) |
| **Resource Group** | Logical container for related resources; resources must be in exactly one RG |
| **Subscription** | Billing and access boundary; owns resource groups |
| **Management Group** | Container for multiple subscriptions; apply policy at scale |
| **Azure Resource Manager (ARM)** | The management layer that processes all Azure requests |

> ⭐ **Exam Tip:** Resource Groups are not a security boundary — they're an organizational/management boundary. A resource can only be in ONE resource group at a time.

---

## 2.2 Azure Compute Services

### Azure Virtual Machines (VMs)
- **IaaS** — full control over a virtualized server
- You manage: OS, software, patching, configuration
- Choose: size (CPU/RAM), OS (Windows/Linux), disk type
- Use cases: lift-and-shift migrations, custom environments, legacy apps
- **VM Scale Sets** — automatically scale identical VMs in/out based on demand

### Azure App Service
- **PaaS** — fully managed web hosting platform
- Host: web apps, REST APIs, mobile backends
- Supports: .NET, Python, Node.js, Java, PHP, Ruby, Docker containers
- Built-in: auto-scaling, load balancing, SSL, custom domains, CI/CD integration
- Use case: web applications where you don't want to manage servers

### Azure Functions
- **Serverless compute** — run code in response to events without managing servers
- You pay only for execution time (consumption plan)
- Triggers: HTTP request, timer, blob upload, queue message, Event Hub, etc.
- Stateless by default; use Durable Functions for stateful workflows
- Use case: event-driven microservices, data processing, automation

### Azure Container Instances (ACI)
- Run **Docker containers on demand** without managing VMs
- Fastest and simplest way to run a container in Azure
- Good for: isolated containers, burst workloads, CI/CD tasks
- No orchestration — just run a single container or container group

### Azure Kubernetes Service (AKS)
- **Managed Kubernetes** — Microsoft manages the control plane
- Orchestrates containers at scale with automatic scheduling, scaling, self-healing
- Use case: production microservices, complex containerized workloads
- You manage: worker nodes and applications

### Azure Virtual Desktop
- **Desktop and app virtualization** service in the cloud
- Run Windows 10/11 desktop or specific apps from any device
- Multi-session Windows — multiple users on one VM
- Use case: remote work, BYOD, VDI replacement

### Compute Comparison

| Service | Model | Control | Best For |
|---------|-------|---------|----------|
| Virtual Machines | IaaS | Full | Lift-and-shift, custom OS |
| VM Scale Sets | IaaS | Full | Auto-scaling identical VMs |
| App Service | PaaS | Medium | Web apps, APIs |
| Functions | Serverless | Low | Event-driven, short tasks |
| Container Instances | PaaS/Serverless | Low | Quick isolated containers |
| AKS | PaaS | Medium | Production container orchestration |
| Azure Virtual Desktop | SaaS-like | Low | Cloud-hosted desktops |

---

## 2.3 Azure Networking Services

### Azure Virtual Network (VNet)
- **Isolated private network** in Azure — your own network space
- Resources inside a VNet can communicate with each other by default
- Subnets divide the VNet into smaller address ranges
- VNets can be connected via **VNet Peering** (fast, private, no internet)

### Network Security Group (NSG)
- Acts as a **virtual firewall** — controls inbound and outbound traffic to resources
- Rules defined by: priority, source/destination IP, port, protocol
- Applied to subnets or individual network interfaces

### Azure Load Balancer
- Distributes **inbound traffic** across multiple VMs/instances
- Layer 4 (TCP/UDP) load balancing
- Public (internet-facing) or Internal (within VNet)
- Use case: high availability for VMs

### Azure Application Gateway
- Layer 7 (HTTP/HTTPS) load balancer and **web traffic router**
- Includes: SSL termination, URL-based routing, WAF (Web Application Firewall)
- Use case: web applications requiring advanced routing and security

### Azure VPN Gateway
- Connects Azure VNet to **on-premises network** over encrypted VPN tunnel
- Site-to-Site VPN: permanent connection (IPsec/IKE)
- Point-to-Site VPN: individual device to Azure
- Limited bandwidth compared to ExpressRoute

### Azure ExpressRoute
- **Private, dedicated connection** from on-premises to Azure (bypasses internet)
- Faster, more reliable, lower latency, higher bandwidth than VPN
- Does NOT go over the public internet
- Requires working with a connectivity provider
- Use case: enterprise, high-bandwidth, sensitive data, compliance requirements

### Azure DNS
- Host your **DNS domains** in Azure
- Uses Azure's global network of DNS servers for fast resolution
- Supports: public DNS zones and private DNS zones (for internal name resolution within VNets)

### Azure Content Delivery Network (CDN)
- Caches content at **edge locations** globally — closer to users
- Reduces latency for static content (images, CSS, JS, video)
- Partners: Akamai, Verizon, Microsoft's own CDN

### Azure DDoS Protection
- Protects against **Distributed Denial of Service attacks**
- **Basic** (free): always-on, protects Azure infrastructure
- **Standard** (paid): advanced protection for your specific resources with alerting and telemetry

### Networking Comparison

| Service | Purpose |
|---------|---------|
| VNet | Private network in Azure |
| NSG | Firewall rules for traffic control |
| Load Balancer | Distribute traffic across VMs (Layer 4) |
| Application Gateway | Advanced HTTP routing + WAF (Layer 7) |
| VPN Gateway | Encrypted tunnel to on-premises (internet) |
| ExpressRoute | Private dedicated circuit to on-premises |
| Azure DNS | DNS hosting in Azure |
| CDN | Cache content at edge globally |
| DDoS Protection | Protect against DDoS attacks |

> ⭐ **Exam Tip:** **VPN Gateway uses the internet** (encrypted); **ExpressRoute is private** (bypasses internet). ExpressRoute is faster, more reliable, and more expensive.

---

## 2.4 Azure Storage Services

### Storage Account
- The **top-level container** for all Azure Storage services
- Each storage account has a globally unique name
- Data is always replicated for durability

### Azure Blob Storage
- Object storage for **unstructured data** (any file, any format)
- Accessible via HTTP/HTTPS, REST API, SDKs
- Blob types: Block (files), Append (logs), Page (VM disks)
- Access tiers: Hot, Cool, Cold, Archive

### Azure Files
- Fully managed **cloud file shares** (SMB and NFS protocols)
- Mount as a drive on Windows/Linux/Mac
- Use case: shared configuration files, lift-and-shift of file servers

### Azure Queue Storage
- **Message queue** for decoupling application components
- Up to 64 KB per message, up to 7 days retention
- Use case: async processing, microservice communication

### Azure Table Storage
- NoSQL **key-value store** with flexible schema
- Fast access by PartitionKey + RowKey
- Use case: simple structured data, large scale, low cost

### Azure Disk Storage
- **Managed disks** attached to Azure VMs (like a hard drive)
- Types: Ultra Disk, Premium SSD, Standard SSD, Standard HDD
- Managed disks handle replication/availability automatically

### Storage Redundancy Options

| Option | Description | Copies | Protects Against |
|--------|-------------|--------|-----------------|
| **LRS** (Locally Redundant Storage) | 3 copies in one datacenter | 3 | Disk/server failure |
| **ZRS** (Zone-Redundant Storage) | 3 copies across 3 AZs in one region | 3 | Datacenter failure |
| **GRS** (Geo-Redundant Storage) | LRS + async copy to paired region | 6 | Regional disaster |
| **GZRS** (Geo-Zone-Redundant Storage) | ZRS + async copy to paired region | 6 | Best protection |
| **RA-GRS** | GRS + read access to secondary region | 6 | Regional disaster + read access |

> ⭐ **Exam Tip:** GRS replicates to a **paired region** automatically. RA-GRS adds **read access** to that secondary copy. LRS is the cheapest; GZRS is the most durable.

---

## 2.5 Azure Identity and Access Management

### Microsoft Entra ID (formerly Azure Active Directory)
- Microsoft's **cloud-based identity and access management** service
- Manages users, groups, apps, and devices
- Not the same as Windows Server Active Directory (though they can sync)
- Used for: signing into Microsoft 365, Azure portal, custom apps

### Authentication vs Authorization

| Concept | Meaning |
|---------|---------|
| **Authentication (AuthN)** | Proving who you are (identity verification) |
| **Authorization (AuthZ)** | Determining what you're allowed to do |

### Multi-Factor Authentication (MFA)
- Requires **two or more verification factors**:
  - Something you **know** (password)
  - Something you **have** (phone, authenticator app)
  - Something you **are** (fingerprint, face)
- Dramatically reduces risk of compromised credentials

### Conditional Access
- **Policy-based access control** in Microsoft Entra ID
- Grant or block access based on conditions:
  - User location (IP/country)
  - Device compliance status
  - Application being accessed
  - Risk level (from Identity Protection)
- Example: Require MFA when signing in from outside corporate network

### Role-Based Access Control (RBAC)
- Controls **who can do what** on Azure resources
- Assign roles to users, groups, or service identities
- Built-in roles:

| Role | Permissions |
|------|-------------|
| **Owner** | Full access including managing access (assign roles) |
| **Contributor** | Create and manage resources but cannot manage access |
| **Reader** | View resources but cannot make changes |
| **User Access Administrator** | Manage user access only |

- Roles are assigned at a **scope**: Management Group, Subscription, Resource Group, or Resource
- RBAC is **additive** — most permissive role at any scope applies

### Microsoft Entra ID Editions

| Edition | Key Features |
|---------|-------------|
| **Free** | Basic identity, SSO for 10 apps, MFA |
| **P1** | Conditional Access, hybrid identity, self-service password reset |
| **P2** | Identity Protection (risk-based), Privileged Identity Management (PIM) |

### Passwordless Authentication
- **Windows Hello for Business** — PIN or biometric
- **Microsoft Authenticator app** — push notification approval
- **FIDO2 security keys** — hardware token (most phishing-resistant)

---

## 2.6 Azure Security Services

### Microsoft Defender for Cloud
- **Unified security management** and threat protection platform
- Provides: security posture assessment, threat detection, compliance reporting
- **Secure Score** — measures your security posture (higher = more secure)
- Covers: Azure, on-premises, multi-cloud (AWS, GCP)
- Formerly called: Azure Security Center + Azure Defender

### Azure Key Vault
- Securely store and manage **secrets, keys, and certificates**
- Secrets: passwords, connection strings, API keys
- Keys: encryption keys (managed by Key Vault or HSM-backed)
- Certificates: SSL/TLS certificates with auto-renewal
- Access controlled via RBAC + access policies

### Azure DDoS Protection (Standard)
- Advanced protection for Azure resources
- Automatic threat mitigation tuned to your traffic patterns
- Real-time alerting and telemetry during attack
- Cost: per protected public IP + data processed

### Azure Firewall
- **Managed, stateful network firewall** service
- Fully managed by Azure — HA and scaling built in
- Filters traffic by: IP address, port, FQDN (fully qualified domain name)
- Supports: application rules, network rules, NAT rules
- Different from NSG: more advanced, centralized, supports FQDN filtering

### Microsoft Sentinel
- **Cloud-native SIEM (Security Information and Event Management)** and SOAR
- Collects security data from across the organization
- Uses AI to detect threats and respond automatically
- Integrates with: Microsoft 365, Azure, third-party tools

---

> ⬅️ [← Domain 1](./domain-1-cloud-concepts.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-management-governance.md)
