# Domain 1 – Security, Compliance & Identity Concepts `10–15%`

> ⬅️ [Back to SC-900 Index](./index.md)

---

## 1.1 Security Concepts

### Shared Responsibility Model
Security is a **shared responsibility** between Microsoft and the customer. Who owns what depends on the service model:

| Responsibility | On-Premises | IaaS | PaaS | SaaS |
|---------------|-------------|------|------|------|
| Physical security | Customer | **Microsoft** | **Microsoft** | **Microsoft** |
| Network & firewall | Customer | Customer | **Shared** | **Microsoft** |
| OS & runtime | Customer | Customer | **Microsoft** | **Microsoft** |
| Applications | Customer | Customer | Customer | **Microsoft** |
| **Data & identity** | Customer | **Customer** | **Customer** | **Customer** |

> ⭐ **Exam Tip:** **Customers are ALWAYS responsible for their data, accounts, and identities** regardless of the cloud service model.

---

### Defense-in-Depth
A **layered security strategy** where multiple controls protect assets — if one layer fails, others remain. No single control is relied upon.

#### The 7 Layers (inside-out)

```
[Data] ← most critical
[Application]
[Compute / VM]
[Network]
[Perimeter]
[Physical]
[Policies, Procedures, Awareness] ← outer layer
```

| Layer | Description | Example Controls |
|-------|-------------|-----------------|
| **Physical** | Protect hardware and facilities | Datacenter locks, cameras, guards |
| **Perimeter** | Protect network boundary from large-scale attacks | DDoS protection, firewalls |
| **Network** | Limit communication between resources | NSGs, VNets, segmentation |
| **Compute** | Secure VMs and endpoints | Antimalware, patch management |
| **Application** | Secure apps from vulnerabilities | Secure coding, WAF, SAST |
| **Data** | Protect data at rest and in transit | Encryption, access control |
| **Identity** | Control access via verified identities | MFA, Conditional Access, RBAC |

---

### Zero Trust Model
"**Never trust, always verify**" — assume breach is possible from anywhere, including inside the network.

#### Zero Trust Principles

| Principle | Meaning |
|-----------|---------|
| **Verify explicitly** | Always authenticate and authorize based on all available data points (identity, location, device, service) |
| **Use least privilege access** | Limit access to only what's needed — Just-In-Time (JIT) and Just-Enough-Access (JEA) |
| **Assume breach** | Minimise blast radius; segment access; encrypt everything; monitor continuously |

#### Zero Trust Pillars
- **Identities** — verify with strong authentication
- **Devices** — ensure compliance before granting access
- **Applications** — discover shadow IT; control permissions
- **Data** — classify, label, and encrypt
- **Infrastructure** — monitor for anomalies
- **Networks** — micro-segment; encrypt traffic

> ⭐ **Exam Tip:** Zero Trust replaces the old "trust but verify" perimeter model. The old model trusted anyone inside the corporate network. Zero Trust trusts no one by default — even inside the network.

---

### Encryption and Hashing

#### Encryption
Transforms readable data (**plaintext**) into unreadable data (**ciphertext**) using an algorithm and key.

| Type | Description | Key characteristic |
|------|-------------|-------------------|
| **Symmetric encryption** | Same key encrypts and decrypts (e.g., AES) | Faster; key must be shared securely |
| **Asymmetric encryption** | Public key encrypts; private key decrypts (e.g., RSA) | More secure for key exchange; slower |

**Encryption at rest** — data stored on disk is encrypted (e.g., Azure Storage Service Encryption)
**Encryption in transit** — data moving over a network is encrypted (e.g., TLS/HTTPS)

#### Hashing
- A **one-way function** that converts data into a fixed-length hash value
- Cannot be reversed to recover original data
- Used for: password storage, data integrity verification
- Examples: SHA-256, bcrypt
- **Salting** — adding a random value before hashing to prevent rainbow table attacks

| | Encryption | Hashing |
|--|------------|---------|
| Reversible? | ✅ Yes (with key) | ❌ No |
| Use case | Protecting data confidentiality | Password storage, integrity checks |
| Key required? | ✅ Yes | ❌ No |

---

### Governance, Risk, and Compliance (GRC)

| Term | Definition |
|------|-----------|
| **Governance** | System of rules, practices, and processes for directing an organization |
| **Risk** | The potential for something bad to happen; managing likelihood and impact |
| **Compliance** | Adhering to laws, regulations, and standards (GDPR, HIPAA, ISO 27001) |

---

## 1.2 Identity Concepts

### Identity as the Primary Security Perimeter
- The traditional **network perimeter** (firewall) is no longer sufficient
- Users access resources from anywhere, on any device
- **Identity** has become the new security boundary — controlling access based on who you are

### Authentication (AuthN) vs Authorization (AuthZ)

| Concept | Question It Answers | Example |
|---------|--------------------|---------| 
| **Authentication** | Who are you? | Entering your password |
| **Authorization** | What can you do? | Having read-only access to a file share |

### Identity Providers (IdPs)
- A service that **creates, manages, and verifies** digital identities
- Issues tokens after successful authentication
- Examples: Microsoft Entra ID, Google, Okta, Active Directory Federation Services

### Directory Services and Active Directory
- **Active Directory Domain Services (AD DS)** — on-premises directory service for Windows networks
- **Microsoft Entra ID** — cloud-based identity service (successor/cloud equivalent)
- Stores: users, groups, computers, policies
- Entra ID Connect syncs on-premises AD with Entra ID for **hybrid identity**

### Federation
- Allows organizations to establish **trust relationships** with external identity providers
- Users authenticate with their own organization's IdP and get access to partner resources
- Example: "Sign in with Google" or company A's employees accessing company B's apps using their own credentials
- Based on standards: **SAML**, **OAuth 2.0**, **OpenID Connect (OIDC)**

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-microsoft-entra.md)
