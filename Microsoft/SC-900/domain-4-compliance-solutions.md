# Domain 4 – Capabilities of Microsoft Compliance Solutions `20–25%`

> ⬅️ [Back to SC-900 Index](./index.md)

---

## 4.1 Service Trust Portal and Privacy Principles

### Microsoft Service Trust Portal (STP)
- Public portal at **https://servicetrust.microsoft.com**
- Provides: **audit reports, compliance documents, whitepapers, and trust information** about Microsoft cloud services
- Accessible to: anyone (some content requires sign-in with a Microsoft account)
- Contains:
  - Third-party audit reports (ISO 27001, SOC 1/2/3, PCI DSS, FedRAMP)
  - Compliance guides and blueprints
  - Data protection resources
  - Regional compliance information

### Microsoft's Privacy Principles

| Principle | Description |
|-----------|-------------|
| **Control** | Customers control their own data |
| **Transparency** | Microsoft is transparent about data collection and use |
| **Security** | Microsoft protects customer data with strong security |
| **Legal Protections** | Microsoft respects local privacy laws |
| **No Content-Based Targeting** | Microsoft does not use customer content to target advertising |
| **Benefits to You** | Data used only to provide the services, not for unrelated purposes |

### Microsoft Priva
- Microsoft's **privacy management** solution
- Helps organizations: discover, assess, and manage personal data risks
- Components:
  - **Privacy Risk Management** — identify and reduce privacy risks automatically
  - **Subject Rights Requests** — manage GDPR/CCPA data subject requests at scale

---

## 4.2 Microsoft Purview — Compliance Management

Microsoft Purview is the **unified data governance and compliance platform** for Microsoft 365 and beyond.

### The Microsoft Purview Compliance Portal (compliance.microsoft.com)
- Central hub for all compliance tools
- Dashboard: shows compliance score, active alerts, top recommendations
- Contains: Compliance Manager, Information Protection, DLP, Records Management, etc.

### Compliance Manager
- Helps organizations **assess, manage, and track** their compliance posture
- **Compliance Score** — a numeric score (0–100%) reflecting how well you've implemented recommended controls

#### Key Components

| Component | Description |
|-----------|-------------|
| **Assessments** | Evaluate compliance against a specific regulation (e.g., GDPR, HIPAA, ISO 27001) |
| **Controls** | Specific requirements within a regulation |
| **Improvement Actions** | Recommended steps to improve your score |
| **Regulations** | 300+ prebuilt regulatory templates |

#### Compliance Score vs Secure Score
| | Compliance Score | Secure Score |
|--|-----------------|-------------|
| What it measures | Compliance with regulations | Security posture |
| Managed by | Microsoft Purview | Microsoft Defender for Cloud |
| Focus | Data protection, privacy | Threat and vulnerability reduction |

---

## 4.3 Information Protection and Data Governance

### Data Classification in Microsoft Purview

Before protecting data, you must **understand what you have**:

| Classification Method | Description |
|----------------------|-------------|
| **Sensitive Information Types (SITs)** | Pattern-based detection (e.g., credit card numbers, SSNs, passport numbers) — 200+ built-in |
| **Trainable Classifiers** | ML models trained to recognize content categories (e.g., financial statements, source code) |
| **Exact Data Match (EDM)** | Match specific data values from a database (e.g., customer list) |

### Content Explorer and Activity Explorer

| Tool | What It Shows |
|------|--------------|
| **Content Explorer** | Snapshot of all items classified with a label or SIT — shows WHERE sensitive data is |
| **Activity Explorer** | Timeline of actions taken on labeled content — shows WHAT happened to sensitive data |

### Sensitivity Labels
- **Labels applied to content** to classify and protect it
- Applied to: emails, documents, meetings, Teams, sites, SharePoint
- What labels can do:
  - Apply **encryption** (restrict who can open/edit)
  - Apply **watermarks, headers, footers**
  - Control **sharing** (e.g., prevent external sharing)
  - Mark the content (visual marking only)
- **Sensitivity label policies** define which labels are published to which users

### Data Loss Prevention (DLP)
- Prevents **accidental or intentional sharing of sensitive data** outside the organization
- Policies detect sensitive content (credit cards, health data, etc.) and take action:
  - **Block** the action (prevent send/upload)
  - **Warn** the user and allow them to override
  - **Audit** the action silently (log only)
- Works across: Exchange email, Teams, SharePoint, OneDrive, endpoints, Defender for Cloud Apps

---

## 4.4 Data Lifecycle Management

### Records Management
- Manages **high-value content** that must be kept for regulatory or legal reasons
- Marks content as a **record** — once marked, it cannot be edited or deleted
- **Regulatory record** — even stricter; cannot be unlocked

### Retention Policies and Labels

| | Retention Policy | Retention Label |
|--|----------------|----------------|
| Applied to | Locations (mailboxes, SharePoint sites, Teams) | Individual items (emails, documents) |
| Granularity | Broad (all content in a location) | Precise (item by item) |
| Can mark as record? | ❌ No | ✅ Yes |
| Priority | Lower | Higher (label wins over policy) |

**Retention actions:**
- **Retain** — keep content for X years, even if deleted
- **Delete** — delete content after X years
- **Retain then delete** — keep for X years, then automatically delete

---

## 4.5 Insider Risk, eDiscovery, and Audit

### Insider Risk Management
- Detects **risky activities by internal users** — employees, contractors
- Uses signals from: Microsoft 365 activity, HR data, endpoint behaviour
- Example policies: Data theft by departing employees, data leaks, security violations
- Preserves **privacy**: configurable anonymization of user identities during investigation
- Workflow: Policy → Alert → Case → Investigation → Action

### eDiscovery
Used for **legal holds, investigations, and litigation** — identify, preserve, collect, and export content.

| Tool | Use Case |
|------|---------|
| **Content Search** | Search across Microsoft 365 for content matching criteria |
| **eDiscovery (Standard)** | Create cases, hold content, export search results |
| **eDiscovery (Premium)** | End-to-end workflow with custodian management, advanced analytics, review sets |

**Legal Hold** — preserve content that would otherwise be deleted by retention policies; critical for litigation.

### Audit in Microsoft Purview

| Solution | Description |
|----------|-------------|
| **Audit (Standard)** | 90-day audit log retention; search user and admin activity across Microsoft 365 |
| **Audit (Premium)** | 365-day (or 10-year with add-on) retention; high-value "crucial events"; log bandwidth |

**What is audited:** User sign-ins, file access, sharing, admin changes, policy changes, mailbox access

> ⭐ **Exam Tip:** Audit logs are for **investigating what happened** (who accessed what, when). Compliance Manager measures **how compliant** you are with regulations. They serve different purposes.

---

> ⬅️ [← Domain 3](./domain-3-security-solutions.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
