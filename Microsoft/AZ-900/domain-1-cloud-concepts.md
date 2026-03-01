# Domain 1 – Describe Cloud Concepts `25–30%`

> ⬅️ [Back to AZ-900 Index](./index.md)

---

## 1.1 What is Cloud Computing?

Cloud computing is the **delivery of computing services over the internet** — including servers, storage, databases, networking, software, and analytics — offering faster innovation, flexible resources, and economies of scale.

### Key Benefits of Cloud Computing

| Benefit | Description |
|---------|-------------|
| **High Availability** | Services remain accessible despite failures. Defined by SLAs (e.g., 99.9% uptime). |
| **Scalability** | Ability to increase resources to handle more load. Vertical (scale up) or horizontal (scale out). |
| **Elasticity** | Automatically scale resources up AND down based on demand in real time. |
| **Agility** | Rapidly provision and deploy resources — from weeks to minutes. |
| **Geo-distribution** | Deploy apps close to users worldwide for lower latency. |
| **Disaster Recovery** | Replicate and restore data/services across regions with minimal downtime. |
| **Reliability** | Redundant, globally distributed infrastructure ensures consistent performance. |
| **Predictability** | Cost and performance are predictable and can be planned for. |
| **Security** | Broad set of tools, policies, and controls to protect data and infrastructure. |
| **Governance** | Enforce organizational standards and compliance across cloud resources. |
| **Manageability** | Manage resources via portal, CLI, APIs, or automation. |

### Scalability vs Elasticity vs Agility

| Term | Meaning |
|------|---------|
| **Scalability** | Ability to handle increased load by adding resources (planned) |
| **Elasticity** | Automatically scales up/down with demand in real time (dynamic) |
| **Agility** | Speed of provisioning and deploying new resources |

> ⭐ **Exam Tip:** Elasticity is *automatic* scaling. Scalability can be manual or automatic. Agility is about *speed* of provisioning.

---

## 1.2 Cloud Service Models

### IaaS – Infrastructure as a Service
- You rent **virtualized hardware** (VMs, storage, networking)
- You manage: OS, runtime, middleware, data, applications
- Cloud manages: physical servers, networking, virtualization
- **Most flexible** — maximum control
- Examples: Azure Virtual Machines, Azure Virtual Network, Azure Blob Storage

### PaaS – Platform as a Service
- You get a **managed platform** to build and run applications
- You manage: applications and data
- Cloud manages: OS, runtime, middleware, infrastructure
- Great for developers — focus on code, not infrastructure
- Examples: Azure App Service, Azure SQL Database, Azure Functions (partially), Azure Kubernetes Service

### SaaS – Software as a Service
- You use **ready-to-use software** over the internet
- You manage: your data and access settings
- Cloud manages: everything else
- Examples: Microsoft 365, Dynamics 365, OneDrive, Teams

### Comparison Table

| Aspect | IaaS | PaaS | SaaS |
|--------|------|------|------|
| You manage | OS, apps, data | Apps, data | Just usage/data |
| Provider manages | Hardware, network | Hardware + OS + platform | Everything |
| Flexibility | Highest | Medium | Lowest |
| Ease of use | Complex | Medium | Easiest |
| Example | Azure VM | Azure App Service | Microsoft 365 |
| Use case | Lift & shift, full control | App development | Email, collaboration |

> ⭐ **Exam Tip:** A pizza analogy is often used:
> - **IaaS** = renting a kitchen (you cook everything)
> - **PaaS** = ordering a kit meal (ingredients provided, you assemble)
> - **SaaS** = ordering delivery (everything done for you)

---

## 1.3 Cloud Deployment Models

### Public Cloud
- Resources owned and operated by a **third-party provider** (Microsoft, AWS, Google)
- Shared infrastructure accessible over the internet
- You pay only for what you use
- Pros: no upfront cost, infinite scale, no maintenance
- Cons: less control, shared environment

### Private Cloud
- Cloud infrastructure **dedicated to a single organization**
- Can be on-premises or hosted by a third party
- Full control over security and compliance
- Pros: maximum control, compliance, security
- Cons: higher cost, requires IT staff to manage

### Hybrid Cloud
- **Combines public and private cloud** with orchestration between them
- Data and apps can move between the two environments
- Pros: flexibility, leverage existing on-prem investment, sensitive data can stay on-prem
- Cons: complexity, networking requirements

### Multi-Cloud
- Using **multiple public cloud providers** (e.g., Azure + AWS)
- Avoid vendor lock-in, use best services from each provider
- More complex to manage

| Model | Who Owns Infrastructure | Control | Cost |
|-------|------------------------|---------|------|
| Public | Cloud provider | Low | Pay-as-you-go |
| Private | Your organization | High | High (CapEx) |
| Hybrid | Both | Medium | Mixed |
| Multi-cloud | Multiple providers | Variable | Variable |

---

## 1.4 CapEx vs OpEx

### CapEx – Capital Expenditure
- **Upfront spending** on physical infrastructure (buying servers, data centers)
- Value depreciates over time
- Predictable but large initial investment
- Traditional on-premises model

### OpEx – Operational Expenditure
- **Pay-as-you-go** spending on services as needed
- No upfront cost — expenses happen over time
- Deducted in the same period
- Cloud computing model

### Consumption-Based Model
- You only pay for **what you actually use**
- No need to predict demand in advance
- Azure charges by: compute time, storage used, network egress, service tier

| Aspect | CapEx | OpEx |
|--------|-------|------|
| Payment | Upfront lump sum | Ongoing usage-based |
| Risk | High (over/under-provision) | Low (scales with use) |
| Flexibility | Low | High |
| Tax treatment | Depreciated over time | Deducted immediately |
| Cloud model | On-premises / Private cloud | Public cloud |

> ⭐ **Exam Tip:** Moving to Azure converts CapEx to OpEx. Cloud's consumption model means you only pay for what you use.

---

## 1.5 Shared Responsibility Model

Responsibility for security is **shared between Microsoft and the customer** — and who is responsible for what depends on the service model.

| Responsibility | On-Premises | IaaS | PaaS | SaaS |
|---------------|-------------|------|------|------|
| Physical security | Customer | **Microsoft** | **Microsoft** | **Microsoft** |
| Network controls | Customer | Customer | **Shared** | **Microsoft** |
| OS | Customer | Customer | **Microsoft** | **Microsoft** |
| Applications | Customer | Customer | Customer | **Microsoft** |
| Data | Customer | Customer | Customer | Customer |
| Identities | Customer | Customer | Customer | Customer |

> ⭐ **Exam Tip:** The **customer is ALWAYS responsible for their data, accounts, and access management** regardless of service model (IaaS, PaaS, or SaaS). Microsoft is always responsible for physical datacenter security.

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-azure-architecture-services.md)
