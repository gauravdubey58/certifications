# 🔵 Module 1: Design Identity, Governance, and Monitoring Solutions (25–30%)

📘 [← Back to AZ-305 Index](./index.md)

---

## 1.1 Design a Solution for Logging and Monitoring

### 📡 Azure Monitor — Central Monitoring Platform

Azure Monitor is the unified observability platform for all Azure resources. On AZ-305, questions focus on **which service to choose** and **how to design the monitoring architecture**, not implementation code.

**Azure Monitor Data Types:**

| Data Type | Description | Storage |
|-----------|-------------|---------|
| **Metrics** | Numerical time-series; lightweight, near real-time | Azure Monitor Metrics DB (93 days) |
| **Logs** | Structured records — events, traces, diagnostics | Log Analytics Workspace (configurable) |
| **Activity Log** | Subscription-level control-plane events (who did what) | Azure Monitor (90 days, exportable) |
| **Resource Logs** | Data-plane operations within a resource | Sent to Log Analytics / Storage / Event Hub via Diagnostic Settings |
| **Traces** | Distributed request tracing across services | Application Insights |

---

### 🏗️ Designing a Monitoring Architecture

**Key Design Decisions:**

| Requirement | Design Choice |
|-------------|--------------|
| Central log repository for all subscriptions | Single **Log Analytics Workspace** at management group scope |
| Monitor application performance and dependencies | **Application Insights** (connected to same or separate workspace) |
| Alert on infrastructure metrics (CPU, disk) | **Azure Monitor Metric Alerts** |
| Alert on log query results (errors > threshold) | **Azure Monitor Log Alerts** |
| Governance audit trail (who changed what) | **Activity Log** + export to Log Analytics |
| Cost reporting and anomalies | **Microsoft Cost Management** + Budgets + Alerts |
| Operational dashboards for teams | **Azure Workbooks** or **Azure Dashboards** |
| Route alerts to multiple channels | **Action Groups** (email, SMS, webhook, Function, Logic App) |

**Workspace Design Patterns:**

| Pattern | When to Use |
|---------|-------------|
| **Single centralised workspace** | Preferred for most organisations — unified querying, simpler governance |
| **Per-region workspaces** | Data residency requirements (GDPR); reduce cross-region egress costs |
| **Per-environment workspaces** | Dev/Test/Prod isolation for billing or access control |

> 💡 **AZ-305 key principle:** Favour a **single centralised workspace** with RBAC to control which teams can query which data — rather than multiple workspaces that fragment visibility.

---

### 📊 Application Insights

**What it is:** APM service for live applications — automatic performance anomaly detection, distributed tracing, user behaviour analytics.

**Workspace-based vs Classic:**
- Always use **workspace-based** Application Insights — data is stored in a Log Analytics Workspace, enabling cross-resource queries.
- Classic (standalone) Application Insights is deprecated.

**Key Capabilities for AZ-305:**

| Capability | Purpose |
|------------|---------|
| **Application Map** | Visualises dependencies between components and failure rates |
| **Live Metrics** | Real-time telemetry stream — zero-latency debugging |
| **Availability Tests** | External health probes from Azure PoP locations |
| **Smart Detection** | ML-based anomaly detection on metrics |
| **User Flows** | Visualises how users navigate through an app |
| **Distributed Tracing** | End-to-end request tracking across microservices |

---

### 📋 Diagnostic Settings Design

Every Azure resource can be configured to send logs and metrics to:
- **Log Analytics Workspace** (for querying and alerting)
- **Azure Storage Account** (for long-term archival at low cost)
- **Azure Event Hubs** (for streaming to SIEM tools like Sentinel or Splunk)

**Design rule:** Configure diagnostic settings at scale using **Azure Policy** with a `DeployIfNotExists` effect — automatically enabling diagnostics on all resources in a scope.

---

## 1.2 Design Authentication and Authorisation Solutions

### 🆔 Microsoft Entra ID (Azure Active Directory)

Microsoft Entra ID is the cloud-based identity and access management service for Azure — the foundation of all authentication design.

**Key Authentication Concepts for AZ-305:**

| Feature | Description | When to Design For |
|---------|-------------|-------------------|
| **Multi-Factor Authentication (MFA)** | Requires a second verification factor | Always — especially for privileged access |
| **Conditional Access** | Context-aware access policies (location, device, risk) | When access must adapt to risk signals |
| **Identity Protection** | Detects risky sign-ins and compromised credentials | Enterprise scenarios with high-value data |
| **Passwordless** | FIDO2, Windows Hello, Authenticator app | Modern workplaces reducing password risk |
| **B2B Collaboration** | External partners access your resources with their own identity | Multi-org scenarios |
| **B2C** | Customer-facing apps with social/local identity providers | Consumer-facing applications |
| **External ID** | Unified external identity platform (combines B2B + B2C) | Modern replacement for B2B/B2C separation |

---

### 🔐 Conditional Access — Design Patterns

Conditional Access is the **policy engine** for Entra ID — granting, blocking, or requiring additional verification based on signals.

**Signal Sources → Policy → Controls:**

```
Signals:                    Controls:
├── User / Group            ├── Grant (require MFA)
├── Location (IP/country)   ├── Grant (require compliant device)
├── Device state            ├── Grant (require Entra ID join)
├── Application             ├── Block access
├── Sign-in risk            └── Session controls (app restrictions)
└── User risk
```

**Common Conditional Access Policies for AZ-305:**

| Scenario | Policy Design |
|----------|--------------|
| Require MFA for all admin access | Target: Directory Roles (Global Admin etc.) → Require MFA |
| Block access from outside UK | Target: All Users → Location condition (exclude UK) → Block |
| Allow only compliant devices | Target: All Users → Device filter → Require compliant device |
| Require MFA for risky sign-ins | Target: All Users → Sign-in risk = Medium/High → Require MFA |
| Block legacy authentication | Target: All Users → Client apps = Legacy → Block |

> 💡 Always create a **break-glass account** excluded from Conditional Access policies — an emergency admin account not subject to MFA or device compliance, stored securely offline.

---

### 👑 Privileged Identity Management (PIM)

PIM provides **just-in-time (JIT) privileged access** — users are eligible for roles but must explicitly activate them, reducing standing privilege.

**PIM Workflow:**
```
Admin assigns user as "eligible" for Global Admin role
    ↓
User requests activation (provides justification, duration)
    ↓
Approval (optional) by designated approver
    ↓
Role activated for 1–8 hours (configurable max)
    ↓
Access expires automatically — no persistent privilege
```

**PIM vs Direct Role Assignment:**

| | Direct Assignment | PIM (Eligible) |
|--|-----------------|----------------|
| Standing privilege | ✅ Always active | ❌ Activate on-demand |
| Audit trail | Limited | Full activation history |
| Approval required | No | Optional |
| Best for | Service accounts, automation | Human admins |

---

### 🔑 Managed Identities — Design Guidance

Managed Identities allow Azure resources to authenticate to other Azure services without credentials in code or config.

**System-assigned vs User-assigned:**

| | System-assigned | User-assigned |
|--|----------------|--------------|
| Lifecycle | Tied to resource | Independent resource |
| Shared across resources | No | Yes |
| Best for | Single-resource, unique identity | Multiple resources sharing one identity |

**Common Design Patterns:**

| Scenario | Design |
|----------|--------|
| VM needs to read Key Vault secrets | System-assigned MI + Key Vault RBAC (`Key Vault Secrets User`) |
| 10 App Services sharing one identity | User-assigned MI assigned to all 10 apps |
| Function triggers from Service Bus | System-assigned MI + Service Bus Data Receiver role |
| AKS pods access Azure Storage | Workload Identity (replaces pod-managed identity) |

---

### 🛡️ Azure Key Vault — Design

**When to use Key Vault vs other secret stores:**

| Need | Service |
|------|---------|
| Application secrets, connection strings, API keys | Azure Key Vault — Secrets |
| Encryption keys for data-at-rest | Azure Key Vault — Keys (or Managed HSM for FIPS 140-2 Level 3) |
| TLS certificates with auto-renewal | Azure Key Vault — Certificates |
| Secrets at massive scale / multi-region | Azure Key Vault + Private Endpoint per region |

**Key Vault Design Best Practices:**
- One vault per application per environment (dev/staging/prod) — avoid sharing vaults between apps
- Enable **soft-delete** and **purge protection** — prevents accidental or malicious deletion
- Use **Private Endpoints** to restrict access to your VNet
- Grant access via **RBAC** (preferred over legacy Access Policies)
- Enable **Key Vault Firewall** — allow only specific VNets and IPs

---

## 1.3 Design Governance

### 🏢 Management Hierarchy

```
Tenant Root Group (Management Group)
    ├── Platform Management Group
    │    ├── Identity Subscription
    │    ├── Management Subscription
    │    └── Connectivity Subscription
    └── Landing Zones Management Group
         ├── Corp Management Group
         │    ├── Production Subscription
         │    └── Development Subscription
         └── Online Management Group
              └── Public-facing Subscription
```

**Azure Landing Zone (Enterprise Scale):** Microsoft's recommended architecture for enterprise Azure adoption — separating platform (identity, networking, management) from application landing zones.

---

### 📜 Azure Policy — Design

**Policy Effects (in order of evaluation):**

| Effect | Behaviour |
|--------|-----------|
| `Disabled` | Policy rule is ignored |
| `Append` | Adds fields to a resource (e.g., add tags) |
| `Modify` | Modifies properties or tags on resources |
| `Audit` | Flags non-compliant resources but allows them |
| `AuditIfNotExists` | Audits if a related resource doesn't exist |
| `Deny` | Blocks resource creation/update if non-compliant |
| `DeployIfNotExists` | Automatically deploys a related resource if missing |
| `DenyAction` | Prevents specific actions (e.g., deny delete) |

**Policy Scope — inheritance (outer → inner):**
```
Management Group → Subscription → Resource Group → Resource
```

**Common Policy Scenarios for AZ-305:**

| Requirement | Policy |
|-------------|--------|
| Require a cost-centre tag on all resources | `Deny` — tag key required |
| Auto-enable diagnostics on all VMs | `DeployIfNotExists` — deploy diagnostic extension |
| Restrict VM sizes to approved SKUs | `Deny` — allowed VM SKU list |
| Enforce geo-restriction (EU only) | `Deny` — allowed locations list |
| Audit resources without encryption | `Audit` — check encryption settings |
| Auto-apply tags from resource group | `Modify` — inherit tag |

**Policy Initiatives:** Group multiple policies into a single assignable unit (e.g., "ISO 27001 compliance" initiative contains 50+ individual policies).

---

### 🏷️ Azure Blueprints vs ARM Templates vs Bicep vs Terraform

| Tool | Purpose | State Tracking |
|------|---------|---------------|
| **ARM Templates / Bicep** | Deploy Azure resources | No (idempotent redeploy) |
| **Azure Blueprints** | Deploy + lock subscriptions with governance artefacts (policies, RBACs, ARM) | Yes (assignment tracking) |
| **Terraform** | Multi-cloud IaC with state management | Yes (state file) |
| **Azure Policy** | Ongoing compliance enforcement | Yes (compliance reports) |

> 💡 **AZ-305 scenario tip:** If a question asks about enforcing a consistent environment configuration that cannot be modified by subscription owners, choose **Azure Blueprints** with `DoNotDelete` or `ReadOnly` resource locks.

---

### 💰 Cost Management Design

| Tool | Purpose |
|------|---------|
| **Azure Pricing Calculator** | Estimate costs before deployment |
| **Total Cost of Ownership (TCO) Calculator** | Compare on-prem vs Azure costs for migration justification |
| **Microsoft Cost Management** | Monitor, analyse, and optimise actual spend |
| **Budgets** | Set spending thresholds with alert notifications |
| **Cost Allocation** | Distribute shared costs across departments |
| **Azure Advisor** | Recommendations for cost, security, reliability, performance |

---

📘 [← Back to AZ-305 Index](./index.md)
