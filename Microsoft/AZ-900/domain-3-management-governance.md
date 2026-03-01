# Domain 3 – Describe Azure Management and Governance `30–35%`

> ⬅️ [Back to AZ-900 Index](./index.md)

---

## 3.1 Cost Management in Azure

### Factors Affecting Azure Cost

| Factor | Impact |
|--------|--------|
| **Resource type** | VMs cost more than storage; GPU VMs cost more than general-purpose |
| **Consumption** | Pay for what you use (compute hours, GB stored, requests) |
| **Region** | Prices vary by region (West US often cheaper than Brazil South) |
| **Bandwidth (Egress)** | Outbound data transfer is charged; inbound is free |
| **Reservation** | 1- or 3-year reservations = up to 72% discount vs pay-as-you-go |
| **Azure Hybrid Benefit** | Use existing Windows Server / SQL Server licenses → up to 49% savings |
| **Spot VMs** | Unused Azure capacity at up to 90% discount (can be evicted) |
| **Pricing tier / SKU** | Premium SSD vs Standard HDD; Basic App Service vs Standard |

### Azure Pricing Calculator
- **Estimate costs** before deploying resources
- Configure expected resources and get monthly cost estimates
- URL: https://azure.microsoft.com/pricing/calculator/
- Use case: budget planning, comparing architectures

### Azure Total Cost of Ownership (TCO) Calculator
- Compare cost of **on-premises infrastructure vs Azure**
- Input your current servers, storage, networking, labor costs
- Shows projected savings over 3–5 years
- URL: https://azure.microsoft.com/pricing/tco/calculator/
- Use case: business case for cloud migration

### Azure Cost Management + Billing
- **Monitor, analyze, and optimize** your Azure spending
- Features:
  - Cost analysis (break down by resource, tag, subscription)
  - Budgets and alerts (notify when spending reaches threshold)
  - Cost recommendations (right-size VMs, shut down unused resources)
  - Invoices and billing history

### Cost Saving Strategies

| Strategy | Savings | Notes |
|----------|---------|-------|
| **Reserved Instances** | Up to 72% | Commit to 1 or 3 years |
| **Azure Hybrid Benefit** | Up to 49% | Bring existing licenses |
| **Spot VMs** | Up to 90% | Interruptible workloads only |
| **Dev/Test pricing** | Significant | Non-production environments |
| **Auto-shutdown** | Varies | Stop VMs when not needed |
| **Right-sizing** | Varies | Match VM size to actual usage |
| **Tags** | Indirect | Track spend by team/project |

> ⭐ **Exam Tip:** **Pricing Calculator** = estimate future costs. **TCO Calculator** = compare on-premises vs Azure costs. **Cost Management** = monitor and optimize actual spending.

---

## 3.2 Azure Governance

### Management Groups
- **Container for subscriptions** — top of the Azure resource hierarchy
- Apply policies and RBAC at scale across multiple subscriptions
- Can be nested (up to 6 levels deep)
- Root management group contains all subscriptions in the tenant
- Use case: large organizations with multiple departments/subscriptions

### Subscriptions
- **Billing and access boundary** for Azure resources
- Every resource must be in a subscription
- One subscription = one bill
- Common patterns:
  - Separate subscriptions by: department, environment (dev/prod), business unit
  - Subscription limits apply: 980 resource groups, quota limits per resource type

### Resource Groups
- **Logical container** for related resources
- Resources must be in exactly one resource group
- Resources in a group can be in different regions
- Delete a resource group → deletes ALL resources inside it
- Use case: group resources that share the same lifecycle

### Azure Policy
- **Enforce organizational standards** on Azure resources
- Create rules (policies) that evaluate resources for compliance
- Policy effects:
  - **Deny** — prevent non-compliant resource creation
  - **Audit** — log non-compliant resources (don't block)
  - **Append** — add required fields automatically
  - **DeployIfNotExists** — auto-deploy a related resource if missing
  - **Modify** — add or update tags, properties
- **Initiatives** = a collection of related policies (e.g., "Comply with ISO 27001")
- Compliance dashboard shows which resources are compliant

### Resource Locks
- **Prevent accidental changes or deletion** of resources
- Two types:

| Lock Type | What It Prevents |
|-----------|-----------------|
| **CanNotDelete (Delete lock)** | Resource cannot be deleted; can be read and modified |
| **ReadOnly** | Resource cannot be deleted OR modified; read-only |

- Locks apply to the resource AND all child resources
- Locks override RBAC — even Owners cannot delete a locked resource (must remove lock first)
- Can be applied to: subscription, resource group, or individual resource

> ⭐ **Exam Tip:** Resource Locks **take priority over RBAC**. An Owner still cannot delete a resource with a Delete lock — they must first remove the lock.

### Azure Blueprints
- **Package of governance artifacts** to deploy consistently
- Can include: ARM templates, policies, RBAC assignments, resource groups
- Versioned and auditable
- Use case: compliant environments for new teams/projects (like a template for setting up a subscription)
- Being replaced by newer tools but still exam-relevant

### Microsoft Purview (Compliance)
- **Data governance and compliance** platform
- Discover, classify, and protect data across Azure and on-premises
- Compliance Manager: track regulatory compliance (GDPR, HIPAA, ISO)

---

## 3.3 Azure Monitoring Tools

### Azure Monitor
- **Central monitoring platform** for all Azure resources
- Collects: metrics (numeric, time-series), logs (text-based events)
- Features:
  - **Alerts** — notify when metrics cross thresholds
  - **Dashboards** — visualize key metrics
  - **Workbooks** — interactive reports
  - **Insights** — curated monitoring for VMs, containers, applications

### Log Analytics (Azure Monitor Logs)
- **Query and analyze log data** collected by Azure Monitor
- Uses **Kusto Query Language (KQL)** for querying
- Centralizes logs from VMs, containers, security events, apps
- Workspace-based — logs are stored in a Log Analytics workspace

### Azure Application Insights
- **Application Performance Monitoring (APM)** for live applications
- Automatically detects anomalies, tracks performance
- Collects: request rates, response times, failure rates, dependency tracking
- Integrates with: App Service, Functions, .NET, Java, Node.js, Python apps

### Azure Service Health
- Tracks the **health of Azure services** in your regions
- Three components:
  - **Azure Status** — global Azure service outages (public page)
  - **Service Health** — personalized view of issues affecting your services/regions
  - **Resource Health** — health of your specific individual resources

### Microsoft Azure Advisor
- **Free personalized recommendations** to optimize Azure usage
- Five categories:

| Category | Recommendations |
|----------|----------------|
| **Reliability** | Add redundancy, availability zones, backups |
| **Security** | Enable MFA, Defender for Cloud, patch vulnerabilities |
| **Performance** | Right-size resources, optimize queries |
| **Cost** | Shut down idle VMs, buy reservations, resize over-provisioned resources |
| **Operational Excellence** | Follow best practices, automate deployments |

> ⭐ **Exam Tip:** Azure Advisor gives **recommendations** but doesn't automatically fix things. It's a suggestion tool, not an automation tool.

---

## 3.4 Azure Management Tools

### Azure Portal
- **Web-based GUI** for managing Azure resources
- Visual, click-through interface
- Best for: learning, one-off tasks, visualizing resources

### Azure Cloud Shell
- **Browser-based command-line** inside the Azure Portal
- Available as: Bash (Azure CLI) or PowerShell
- Pre-authenticated — no credentials needed
- Persists files in an Azure Files share

### Azure CLI
- **Cross-platform command-line tool** (Windows, Mac, Linux)
- Uses `az` commands (e.g., `az vm create`, `az group list`)
- Best for: scripting, automation, repeatable tasks

### Azure PowerShell
- **PowerShell module** with Azure-specific cmdlets
- Uses Az module (`New-AzVM`, `Get-AzResource`)
- Best for: Windows admins, complex scripting with PowerShell logic

### Azure Mobile App
- Manage Azure resources from iOS/Android
- Run Cloud Shell commands, view resource health, receive alerts

### ARM Templates (Azure Resource Manager Templates)
- **JSON files** that define Azure resources as code
- Declarative — describe WHAT you want, not HOW to deploy
- Idempotent — same result on repeated deployments
- Enable: version control, consistency, automation

```json
{
  "$schema": "...",
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "myVM",
      "properties": { ... }
    }
  ]
}
```

### Bicep
- **Domain-specific language** for deploying Azure resources
- Simplifies ARM template syntax — much more readable
- Compiles to ARM JSON templates
- Microsoft's recommended modern approach to IaC for Azure

### Azure Arc
- **Extends Azure management** to non-Azure resources
- Manage on-premises servers, Kubernetes clusters, and other cloud VMs via Azure
- Apply Azure Policy, RBAC, Defender for Cloud to non-Azure resources

### Management Tools Comparison

| Tool | Type | Best For |
|------|------|---------|
| Azure Portal | GUI | Learning, visual management |
| Cloud Shell | Browser CLI | Quick commands without local setup |
| Azure CLI | CLI | Cross-platform scripting |
| Azure PowerShell | CLI | PowerShell scripting |
| ARM Templates | IaC | Repeatable infrastructure deployment |
| Bicep | IaC | Modern, readable ARM replacement |
| Azure Arc | Hybrid | Manage non-Azure resources via Azure |

---

## 3.5 Azure SLAs and Service Lifecycle

### Service Level Agreements (SLAs)
- **Formal commitment** by Microsoft on service availability/performance
- Expressed as a percentage uptime guarantee per month
- Common SLA tiers:

| Uptime | Max Downtime/Month |
|--------|------------------|
| 99% | ~7.2 hours |
| 99.9% | ~43.8 minutes |
| 99.95% | ~21.9 minutes |
| 99.99% | ~4.4 minutes |
| 99.999% | ~26.3 seconds |

### Composite SLAs
- When multiple services are used together, the composite SLA is LOWER
- Calculated by multiplying individual SLAs
- Example: VM (99.9%) × SQL Database (99.99%) = **99.89%** combined SLA

### Improving Your SLA
- Use **Availability Zones** to get higher SLAs (e.g., VM = 99.99% with AZ)
- Use **redundancy and failover** across multiple regions
- Add **load balancers** in front of multiple instances

### Azure Service Lifecycle

| Phase | Description |
|-------|-------------|
| **Private Preview** | Limited testing with invited customers only; not for production |
| **Public Preview** | Open to all customers; reduced/no SLA; not for production |
| **General Availability (GA)** | Fully released, supported, covered by SLAs |

> ⭐ **Exam Tip:** **Preview services** are NOT covered by SLAs and should NOT be used in production. GA services are fully supported.

---

> ⬅️ [← Domain 2](./domain-2-azure-architecture-services.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
