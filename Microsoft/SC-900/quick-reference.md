# ⚡ SC-900 Quick Reference Cheat Sheet

> ⬅️ [Back to SC-900 Index](./index.md)

---

## 🛡️ Core Security Models

| Model | Core Idea | Key Principle |
|-------|-----------|--------------|
| **Defense-in-Depth** | Multiple layers of security controls | If one fails, others remain |
| **Zero Trust** | Never trust, always verify | Verify explicitly, least privilege, assume breach |
| **Shared Responsibility** | Cloud security shared between provider and customer | Customer always owns data & identity |

---

## 🔐 Zero Trust Pillars

Identity · Devices · Applications · Data · Infrastructure · Networks

**Three principles:** Verify explicitly | Least privilege access | Assume breach

---

## 🔑 Microsoft Entra ID at a Glance

| Feature | Description |
|---------|-------------|
| Managed Identity | Auto-managed identity for Azure resources — no passwords |
| MFA | 2+ factors: something you know/have/are |
| Passwordless | FIDO2 keys, Windows Hello, Authenticator app |
| SSPR | Users reset own passwords without IT |
| Conditional Access | Policy-based: grant/block based on user, device, location, risk |
| PIM | Just-In-Time privileged role activation with approval |
| ID Protection | ML-based sign-in and user risk detection |
| Access Reviews | Periodic reviews of who has what access |

### Entra ID Editions
| Feature | Free | P1 | P2 |
|---------|------|----|----|
| Basic SSO | ✅ | ✅ | ✅ |
| MFA | ✅ | ✅ | ✅ |
| Conditional Access | ❌ | ✅ | ✅ |
| SSPR | ❌ | ✅ | ✅ |
| PIM | ❌ | ❌ | ✅ |
| ID Protection | ❌ | ❌ | ✅ |

---

## 🔒 Azure Security Services

| Service | What It Protects | Key Feature |
|---------|-----------------|-------------|
| DDoS Protection | Public IPs / network | Auto-mitigation of volumetric attacks |
| Azure Firewall | Network traffic | Stateful, FQDN-aware, managed |
| WAF | Web applications | OWASP Top 10 protection (on App Gateway/Front Door) |
| NSG | Subnets / NICs | Rule-based inbound/outbound traffic filter |
| Azure Bastion | VM admin access | Browser-based RDP/SSH without public IP |
| Azure Key Vault | Secrets, keys, certs | Centralised credential management |

---

## 🌐 Defender XDR Suite

| Product | Protects | Technology |
|---------|---------|-----------|
| Defender for Office 365 | Email, Teams, SharePoint | Anti-phishing, Safe Links, Safe Attachments |
| Defender for Endpoint | Devices (Windows/Mac/Linux/Mobile) | EDR — endpoint detection and response |
| Defender for Cloud Apps | SaaS/cloud apps | CASB — discover and govern shadow IT |
| Defender for Identity | On-premises Active Directory | Detect lateral movement, credential attacks |
| Defender Vulnerability Mgmt | Devices and software | Prioritised vulnerability remediation |
| Defender Threat Intelligence | Threat actor data | Contextualise threats with intel |

**Unified portal:** security.microsoft.com

---

## 🏢 Defender for Cloud

| Feature | Description |
|---------|-------------|
| CSPM | Assess config, show misconfigurations, recommend fixes |
| Secure Score | 0–100% rating of security posture |
| CWP plans | Workload-specific protection (servers, DBs, storage, containers) |
| Multi-cloud | Covers Azure + AWS + GCP + on-premises |

---

## 📋 Microsoft Purview Compliance

| Tool | Purpose |
|------|---------|
| Compliance Manager | Track compliance posture; Compliance Score; 300+ regulation templates |
| Sensitivity Labels | Classify and protect content (encryption, markings, sharing controls) |
| DLP | Prevent sensitive data from leaving the org |
| Retention Policies | Retain or delete content at a location level |
| Retention Labels | Retain/delete individual items; can mark as records |
| Records Management | Immutable records for regulatory requirements |
| Insider Risk Mgmt | Detect risky internal user behaviour |
| eDiscovery | Legal hold, search, and export content for litigation |
| Audit | Log and search user/admin activity (90 days standard, 365 days premium) |
| Content Explorer | See WHERE classified/labeled content is |
| Activity Explorer | See WHAT actions were taken on labeled content |

---

## 🔍 Microsoft Sentinel (SIEM + SOAR)

| Feature | Description |
|---------|-------------|
| Data collection | Connectors for 100+ sources (Microsoft, Azure, third-party) |
| Analytics rules | Detect threat patterns; trigger alerts/incidents |
| Workbooks | Visual security dashboards |
| Hunting | Proactive KQL-based threat hunting |
| Playbooks | Automated response using Azure Logic Apps |
| Fusion | ML-based multi-stage attack detection |

---

## ⭐ Top Exam Topics

| Topic | Key Point |
|-------|----------|
| Zero Trust | Never trust, always verify; assume breach; least privilege |
| Defense-in-Depth | Layered controls; no single point of failure |
| Shared Responsibility | Customer always owns data and identity |
| AuthN vs AuthZ | AuthN = who are you? AuthZ = what can you do? |
| Managed Identity | No credentials needed; Azure manages the lifecycle |
| MFA | 99.9% reduction in account compromise risk |
| FIDO2 | Most phishing-resistant authentication method |
| Conditional Access | Requires P1 or P2 licence |
| PIM | JIT privileged access; activation with MFA + approval |
| ID Protection | Risk-based detection — sign-in risk and user risk |
| Azure Bastion | Secure browser-based RDP/SSH; no public IP needed |
| Key Vault | Secrets, keys, and certs — not a general credential store |
| Azure Firewall | FQDN-aware stateful firewall; NSG cannot filter by FQDN |
| WAF | Protects web apps from OWASP Top 10 (on App Gateway or Front Door) |
| Defender for Cloud | CSPM + CWP; Secure Score; multi-cloud |
| Sentinel | SIEM + SOAR; playbooks automate response |
| Defender for Cloud Apps | CASB — controls SaaS/cloud app access |
| Defender for Identity | Protects ON-PREMISES Active Directory |
| Compliance Manager | Compliance Score; improvement actions; 300+ regulations |
| Sensitivity Labels | Classify + protect (encrypt, restrict sharing, watermarks) |
| DLP | Prevent sensitive data leaving org (block/warn/audit) |
| Retention Label | Wins over Retention Policy if both apply |
| PIM vs Access Reviews | PIM = activate roles JIT; Access Reviews = periodic review of existing access |
| Service Trust Portal | servicetrust.microsoft.com — audit reports and compliance docs |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
