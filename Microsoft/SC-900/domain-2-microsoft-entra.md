# Domain 2 – Capabilities of Microsoft Entra `25–30%`

> ⬅️ [Back to SC-900 Index](./index.md)

---

## 2.1 Microsoft Entra ID

Formerly known as **Azure Active Directory (Azure AD)** — Microsoft's cloud-based **Identity and Access Management (IAM)** service.

### What Entra ID Does
- Manages **users, groups, apps, and devices**
- Provides **Single Sign-On (SSO)** — one login for multiple apps
- Enables **MFA**, **Conditional Access**, and **RBAC**
- Integrates with: Microsoft 365, Azure, third-party SaaS apps

### Entra ID vs On-Premises Active Directory

| Feature | On-Premises AD DS | Microsoft Entra ID |
|---------|------------------|-------------------|
| Protocol | Kerberos, LDAP, NTLM | HTTPS, SAML, OAuth, OpenID Connect |
| Location | Your datacenter | Microsoft cloud |
| Used for | Domain-joined PCs, internal apps | Cloud apps, SaaS, M365 |
| Object types | Users, computers, GPOs | Users, groups, service principals |
| Management | Group Policy | Conditional Access, Intune |

### Types of Identities in Entra ID

| Identity Type | Description | Example |
|--------------|-------------|---------|
| **User** | A human employee or external user | john@contoso.com |
| **Service Principal** | An app's identity in Entra ID | A web app authenticating to Azure SQL |
| **Managed Identity** | Auto-managed identity for Azure resources — no credentials needed | An Azure Function accessing Key Vault |
| **Device** | A registered or joined device | A company laptop enrolled in Intune |
| **External User (Guest)** | B2B collaboration — user from another org | vendor@partner.com |

### Managed Identity Types

| Type | Description |
|------|-------------|
| **System-assigned** | Created and managed automatically with the Azure resource; deleted when resource is deleted |
| **User-assigned** | Standalone identity created separately; can be shared across multiple resources |

> ⭐ **Exam Tip:** Managed Identities eliminate the need to store credentials in code or config files. The Azure platform manages the credential lifecycle automatically.

### Hybrid Identity
- **Microsoft Entra Connect** — syncs on-premises AD users/groups to Entra ID
- Allows users to sign in to cloud apps with their on-premises credentials
- **Password Hash Sync (PHS)** — hash of password synced to cloud
- **Pass-through Authentication (PTA)** — password verified on-premises in real time
- **Federation (AD FS)** — redirect authentication to on-premises federation service

---

## 2.2 Authentication Capabilities

### Authentication Methods in Entra ID

| Method | Type | Notes |
|--------|------|-------|
| **Password** | Something you know | Traditional; weakest alone |
| **Microsoft Authenticator app** | Something you have | Push notification or TOTP code |
| **FIDO2 security key** | Something you have | Hardware token; most phishing-resistant |
| **Windows Hello for Business** | Something you are/have | PIN or biometric bound to device |
| **SMS / Voice call** | Something you have | Less secure; susceptible to SIM swap |
| **Certificate-based** | Something you have | Smart card or device certificate |

### Multi-Factor Authentication (MFA)
- Requires **two or more** verification factors from different categories:
  - Something you **know** (password, PIN)
  - Something you **have** (phone, hardware token)
  - Something you **are** (fingerprint, face)
- Dramatically reduces account compromise risk (99.9% reduction according to Microsoft)
- Configured in: Entra ID → Security → MFA or via Conditional Access policies

### Passwordless Authentication
- Eliminates the password entirely — uses stronger factors only
- Methods: **Windows Hello for Business**, **FIDO2 keys**, **Microsoft Authenticator (passwordless)**
- Benefits: phishing-resistant, more secure, better user experience

### Self-Service Password Reset (SSPR)
- Users can **reset their own passwords** without contacting IT
- Requires: MFA verification before reset allowed
- Reduces helpdesk costs and user downtime
- Admin can configure: number of methods required, available methods

### Password Protection in Entra ID
- **Global banned password list** — Microsoft maintains a list of commonly breached passwords that are blocked globally
- **Custom banned password list** — organizations add their own terms (company name, product names, etc.)
- Works for: cloud accounts AND can be extended to on-premises AD via Entra ID Password Protection agent

---

## 2.3 Access Management

### Conditional Access
- **Policy-based access control** — define conditions and grant/block access accordingly
- Available in: **Microsoft Entra ID P1 and P2**

#### How It Works
```
User attempts to sign in
        ↓
Signal collected: Who? What device? What location? What risk level?
        ↓
Policy evaluated: Does any Conditional Access policy apply?
        ↓
Decision: Grant / Block / Grant with conditions (MFA, compliant device, etc.)
```

#### Common Conditions (Signals)
| Signal | Examples |
|--------|---------|
| **User/Group** | All users, specific groups, external users |
| **Location** | Named IP ranges, countries |
| **Device platform** | Windows, iOS, Android |
| **Device compliance** | Is device managed by Intune? Is it compliant? |
| **Application** | Which app is being accessed? |
| **Sign-in risk** | Low, medium, high (from Identity Protection) |
| **User risk** | Based on compromised credential signals |

#### Common Access Controls (Actions)
- **Block access** — deny entirely
- **Grant access** — allow with or without conditions
- Require: MFA, compliant device, hybrid Azure AD joined device, approved client app

### Microsoft Entra Roles and RBAC

| Role | Scope | Permissions |
|------|-------|------------|
| **Global Administrator** | Entire tenant | Full control — most powerful |
| **User Administrator** | Users and groups | Create/manage users and groups |
| **Security Administrator** | Security features | Manage security policies and settings |
| **Application Administrator** | App registrations | Manage enterprise apps |
| **Billing Administrator** | Billing | Manage subscriptions and billing |

- Roles are assigned to users, groups, or service principals
- **Principle of least privilege** — assign only the roles needed
- **Azure RBAC** (for Azure resources) is separate from **Entra ID roles** (for directory management)

---

## 2.4 Identity Protection and Governance

### Microsoft Entra ID Governance
- Tools to manage the **identity lifecycle** — ensure right people have right access at the right time
- Components:
  - **Entitlement Management** — govern access to resources via access packages
  - **Access Reviews** — periodic reviews of who has access
  - **Lifecycle Workflows** — automate onboarding/offboarding tasks
  - **Terms of Use** — require users to accept policies before access

### Access Reviews
- Periodically ask: "Should this person still have this access?"
- Reviewers can be: the user themselves, their manager, or a resource owner
- Automatic actions: remove access if no response, apply decision
- Use case: Quarterly review of admin role assignments; contractor access expiry

### Microsoft Entra Privileged Identity Management (PIM)
- Provides **Just-In-Time (JIT)** privileged access
- Users are eligible for admin roles but must **activate** them on-demand
- Activation requires: MFA, business justification, approval
- Time-limited: access expires after a set period
- Full audit trail of who activated what privilege and when
- Benefits: reduces standing privileged access; detects unnecessary permissions

> ⭐ **Exam Tip:** PIM is about managing **admin/privileged role access** with JIT activation, time limits, and approval workflows — it's a key control to reduce insider threat and privileged account misuse.

### Microsoft Entra ID Protection
- Uses **machine learning and signals** to detect risky sign-ins and users
- **Risk levels:** Low, Medium, High

| Risk Type | What It Detects |
|-----------|----------------|
| **Sign-in risk** | Probability that this sign-in is not from the legitimate user (e.g., impossible travel, unfamiliar location) |
| **User risk** | Probability that the user's account is compromised (e.g., leaked credentials on dark web) |

- Can be integrated with **Conditional Access** to require MFA or block access based on risk level
- Provides reports: risky users, risky sign-ins, risk detections

---

> ⬅️ [← Domain 1](./domain-1-sci-concepts.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-security-solutions.md)
