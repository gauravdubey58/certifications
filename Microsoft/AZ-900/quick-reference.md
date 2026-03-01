# ⚡ AZ-900 Quick Reference Cheat Sheet

> ⬅️ [Back to AZ-900 Index](./index.md)

---

## ☁️ Service Models at a Glance

| Model | You Manage | Azure Manages | Example |
|-------|-----------|--------------|---------|
| **IaaS** | OS, apps, data, runtime | Hardware, network, virtualization | Azure VMs |
| **PaaS** | Apps and data | Everything else | App Service, Azure SQL DB |
| **SaaS** | Just your data/settings | Everything | Microsoft 365, Teams |

---

## 🏗️ Cloud Deployment Models

| Model | Hosted By | Best For |
|-------|-----------|---------|
| **Public** | Cloud provider (Microsoft) | Startups, scale, no maintenance |
| **Private** | Your organization | High security, compliance, full control |
| **Hybrid** | Both | Use existing on-prem + cloud flexibility |
| **Multi-cloud** | Multiple providers | Avoid lock-in, best-of-breed services |

---

## 💰 CapEx vs OpEx

| | CapEx | OpEx |
|--|-------|------|
| When | Upfront | Ongoing |
| Example | Buy servers | Pay-as-you-go Azure |
| Risk | Over/under-provision | Scale as needed |
| Cloud model | On-prem / Private | Public cloud |

---

## 🌍 Azure Infrastructure

```
Management Group → Subscription → Resource Group → Resource
```

| Component | Description |
|-----------|-------------|
| Region | Geographical area with datacenters |
| Availability Zone | Isolated datacenter within a region (3+ per region) |
| Region Pair | Each region paired with another for DR |
| Resource | A single Azure service instance |
| Resource Group | Logical container for related resources |
| Subscription | Billing boundary; owns resource groups |
| Management Group | Container for multiple subscriptions |

---

## 💻 Compute Services

| Service | Model | Use Case |
|---------|-------|---------|
| Virtual Machines | IaaS | Full control, lift-and-shift |
| VM Scale Sets | IaaS | Auto-scale identical VMs |
| App Service | PaaS | Web apps, REST APIs |
| Azure Functions | Serverless | Event-driven code |
| Container Instances (ACI) | Serverless | Quick isolated containers |
| AKS | PaaS | Production container orchestration |
| Azure Virtual Desktop | Managed | Cloud-hosted Windows desktop |

---

## 🌐 Networking Services

| Service | Purpose |
|---------|---------|
| Virtual Network (VNet) | Private network in Azure |
| Network Security Group (NSG) | Firewall rules (Layer 3/4) |
| Load Balancer | Traffic distribution across VMs (Layer 4) |
| Application Gateway | HTTP routing + WAF (Layer 7) |
| VPN Gateway | Encrypted tunnel to on-premises (via internet) |
| ExpressRoute | Private dedicated circuit to on-premises (no internet) |
| Azure DNS | DNS hosting and resolution |
| Azure CDN | Cache content at edge locations globally |
| Azure Firewall | Managed cloud firewall (FQDN-aware) |
| DDoS Protection | Protect against DDoS attacks |

---

## 🗄️ Storage Redundancy

| Option | Copies | Locations | Best For |
|--------|--------|-----------|---------|
| LRS | 3 | 1 datacenter | Lowest cost |
| ZRS | 3 | 3 zones in 1 region | Zone failures |
| GRS | 6 | 2 regions (async) | Regional disaster |
| GZRS | 6 | 3 zones + 2nd region | Maximum durability |
| RA-GRS | 6 | 2 regions + read access | Read from secondary |

---

## 🔐 Identity and Security

| Service / Feature | Purpose |
|------------------|---------|
| Microsoft Entra ID | Cloud identity provider (users, groups, apps) |
| MFA | Require 2+ factors for authentication |
| Conditional Access | Policy-based access control based on conditions |
| RBAC | Role-based permissions on Azure resources |
| Azure Key Vault | Securely store secrets, keys, certificates |
| Defender for Cloud | Unified security posture + threat protection |
| Azure Firewall | Managed network firewall service |
| Microsoft Sentinel | Cloud SIEM + SOAR for threat detection |
| DDoS Protection Standard | Advanced DDoS mitigation |

**RBAC Roles:**
- **Owner** = Full access + can manage access
- **Contributor** = Full access, cannot manage access
- **Reader** = View only

---

## 🏛️ Governance

| Tool | Purpose |
|------|---------|
| Management Groups | Organize subscriptions, apply policy at scale |
| Azure Policy | Enforce standards; audit or deny non-compliant resources |
| Resource Locks | Prevent accidental deletion/modification |
| Azure Blueprints | Package of governance artifacts for consistent deployments |
| Microsoft Purview | Data governance and compliance management |

**Resource Lock types:**
- **CanNotDelete** = Can read and modify, cannot delete
- **ReadOnly** = Cannot modify or delete (read only)

---

## 📊 Monitoring and Management

| Tool | Purpose |
|------|---------|
| Azure Monitor | Central monitoring: metrics + logs + alerts |
| Log Analytics | Query logs with KQL |
| Application Insights | APM for live application monitoring |
| Azure Service Health | Azure service health for your regions |
| Azure Advisor | Free recommendations: cost, security, reliability, performance, ops |
| Azure Portal | Web GUI for resource management |
| Azure CLI | Cross-platform command-line (az commands) |
| Azure PowerShell | PowerShell module (Az cmdlets) |
| Cloud Shell | Browser-based CLI (authenticated) |
| ARM Templates | JSON infrastructure as code |
| Bicep | Modern, readable ARM replacement |
| Azure Arc | Manage non-Azure resources via Azure |

---

## 💸 Cost Management Tools

| Tool | Purpose |
|------|---------|
| Pricing Calculator | Estimate future Azure costs before deploying |
| TCO Calculator | Compare on-premises vs Azure costs |
| Cost Management + Billing | Monitor and optimize actual spending |
| Azure Advisor (Cost) | Recommendations to reduce spending |
| Budgets + Alerts | Get notified when spending reaches a threshold |

---

## 🎯 Top Exam Topics

| Topic | Key Point |
|-------|-----------|
| IaaS vs PaaS vs SaaS | Customer responsibility decreases from IaaS → SaaS |
| Shared Responsibility | Customer ALWAYS responsible for data and identity |
| CapEx vs OpEx | Cloud = OpEx (pay-as-you-go consumption model) |
| Availability Zones | 3+ isolated datacenters in one region |
| Region Pairs | Each region paired for disaster recovery |
| ExpressRoute vs VPN | ExpressRoute = private, no internet; VPN = encrypted, uses internet |
| Resource Locks | Override RBAC — even Owners can't delete locked resources |
| Azure Policy | Enforce/audit compliance; can Deny non-compliant resources |
| RBAC | Owner > Contributor > Reader; additive permissions |
| Microsoft Entra ID | Azure's cloud identity service (NOT Windows AD) |
| Defender for Cloud | Secure Score + multi-cloud security posture |
| Key Vault | Secrets, keys, certificates — not for RBAC |
| Pricing Calculator | Estimate FUTURE costs |
| TCO Calculator | Compare on-prem vs Azure |
| SLA composite | Multiply individual SLAs together (result is lower) |
| Preview vs GA | Previews have no SLA; not for production |
| Azure Advisor | Recommendations only — does NOT auto-fix |
| GRS storage | Replicates to a PAIRED region (async, not real-time) |
| Resource Group | Logical container; deleting RG deletes all resources inside |
| Spot VMs | Cheapest (90% off) but can be evicted anytime |
| Reserved Instances | Up to 72% discount for 1 or 3 year commitment |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
