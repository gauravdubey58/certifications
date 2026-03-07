# 🏗️ AZ-305: Designing Microsoft Azure Infrastructure Solutions

> **Certification:** Microsoft Certified: Azure Solutions Architect Expert  
> **Prerequisite:** Microsoft Certified: Azure Administrator Associate (AZ-104)  
> **Pass Score:** 700 / 1000 | **Renewal:** Annual (free online assessment)  
> **Exam Updated:** January 14, 2026 (minor accuracy review; major update October 18, 2024)  
> **Official Study Guide:** [learn.microsoft.com/credentials/certifications/resources/study-guides/az-305](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-305)

🏠 [← Back to Microsoft Index](../index.md) | 🗂️ [← Master Index](../../index.md)

---

## 📊 Domain Weightings

| # | Domain | Weight | Notes File |
|---|--------|--------|-----------|
| 1 | Design Identity, Governance, and Monitoring Solutions | **25–30%** | [01-identity-governance-monitoring.md](./01-identity-governance-monitoring.md) |
| 2 | Design Data Storage Solutions | **25–30%** | [02-data-storage.md](./02-data-storage.md) |
| 3 | Design Business Continuity Solutions | **10–15%** | [03-business-continuity.md](./03-business-continuity.md) |
| 4 | Design Infrastructure Solutions | **25–30%** | [04-infrastructure.md](./04-infrastructure.md) |

---

## 📁 All Files

| File | Topics Covered |
|------|---------------|
| [01-identity-governance-monitoring.md](./01-identity-governance-monitoring.md) | Entra ID, authentication, authorisation, RBAC, governance, Azure Policy, monitoring, logging |
| [02-data-storage.md](./02-data-storage.md) | Relational (Azure SQL), semi-structured (Cosmos DB), unstructured (Blob), data integration (Data Factory, Synapse) |
| [03-business-continuity.md](./03-business-continuity.md) | Backup (Azure Backup, Site Recovery), high availability, SLA design, RPO/RTO |
| [04-infrastructure.md](./04-infrastructure.md) | Compute (VMs, AKS, App Service), app architecture, migrations (Azure Migrate), networking (VNet, DNS, load balancing) |
| [mcqs.md](./mcqs.md) | 50 Practice MCQs across all 4 domains with answers and explanations |

---

## 🎯 Candidate Profile

AZ-305 is an **Expert-level** exam for Azure Solutions Architects. You should be able to:
- Translate business requirements into secure, scalable, and reliable Azure designs
- Advise stakeholders on Azure services selection trade-offs
- Design solutions spanning identity, storage, networking, compute, and resiliency
- Partner with Administrators, Developers, and Security teams to implement solutions
- Have significant experience designing and implementing Azure solutions (typically 3+ years)

> ⚠️ AZ-305 focuses on **design decisions and justifications** — not implementation syntax. Expect scenario-based questions asking *which service*, *why*, and *how to configure* — not code.

---

## 📊 Progress Tracker

| Domain | Status | Confidence |
|--------|--------|-----------|
| 1 – Identity, Governance & Monitoring | ⬜ Not Started | ⬜ |
| 2 – Data Storage | ⬜ Not Started | ⬜ |
| 3 – Business Continuity | ⬜ Not Started | ⬜ |
| 4 – Infrastructure | ⬜ Not Started | ⬜ |

> Update each row: ⬜ Not Started → 🟡 In Progress → ✅ Revised

---

## ⚡ Quick Revision — Key Design Decisions

| Scenario | Answer |
|----------|--------|
| SQL DB that can failover automatically to another region | Azure SQL — Failover Group with auto-failover |
| Managed identity for a VM to access Key Vault | System-assigned Managed Identity + Key Vault RBAC |
| Grant temporary, just-in-time privileged access | Microsoft Entra PIM (Privileged Identity Management) |
| Govern all subscriptions with mandatory policies | Azure Policy at Management Group scope |
| Centralised log storage for all Azure resources | Log Analytics Workspace |
| Application health + dependency map | Application Insights + Application Map |
| Backup VMs across regions | Azure Backup + Recovery Services Vault |
| DR with minimal data loss (RPO near 0) | Azure Site Recovery (continuous replication) |
| Lift-and-shift hundreds of VMs to Azure | Azure Migrate + Replication |
| Route traffic by URL path to different backends | Application Gateway (layer 7) |
| Global HTTP load balancing with WAF | Azure Front Door |
| Isolate workloads within a VNet | Network Security Groups (NSGs) + subnets |
| Connect on-premises privately to Azure | ExpressRoute (private) or VPN Gateway (encrypted public) |
| Run containers without managing infrastructure | Azure Container Apps or ACI |
| Serverless event-driven functions | Azure Functions (Consumption plan) |
| Cost estimate before deploying | Azure Pricing Calculator + Total Cost of Ownership tool |

---

## 🔗 Official Resources

| Resource | Link |
|----------|------|
| Official Study Guide | [AZ-305 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-305) |
| Microsoft Learn Path | [Azure Solutions Architect](https://learn.microsoft.com/en-us/credentials/certifications/azure-solutions-architect/) |
| Free Practice Assessment | [Practice Questions](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-305/practice/assessment?assessment-type=practice&assessmentId=15) |
| Azure Architecture Centre | [architecture.azure.com](https://learn.microsoft.com/en-us/azure/architecture/) |
| Azure Well-Architected Framework | [WAF Documentation](https://learn.microsoft.com/en-us/azure/well-architected/) |
| Exam Sandbox | [aka.ms/examdemo](https://aka.ms/examdemo) |

---

🏠 [← Back to Microsoft Index](../index.md)
