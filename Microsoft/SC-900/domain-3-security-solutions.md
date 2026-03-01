# Domain 3 – Capabilities of Microsoft Security Solutions `35–40%`

> ⬅️ [Back to SC-900 Index](./index.md)

This is the **largest and most important domain** — covers Azure infrastructure security, Defender for Cloud, Sentinel, and Defender XDR.

---

## 3.1 Azure Core Infrastructure Security Services

### Azure DDoS Protection
Defends against **Distributed Denial of Service** attacks that flood resources with traffic.

| Tier | Description |
|------|-------------|
| **Network Protection** | Always-on; protects Azure infrastructure globally; automatic |
| **IP Protection** | Per-public-IP protection with advanced mitigation, alerting, and telemetry |

- Automatic traffic scrubbing during an attack
- Real-time telemetry and alerting (IP Protection tier)
- No configuration needed for basic protection

### Azure Firewall
- **Managed, stateful, cloud-native network firewall** — fully managed by Microsoft
- Filters traffic using: IP address, port, protocol, FQDN (fully qualified domain names)
- **Supports FQDN filtering** — unlike NSGs, can allow/deny based on domain name
- Three tiers: **Standard**, **Premium** (IDPS, TLS inspection, URL filtering), **Basic**
- Deployed in a VNet hub; protects all spokes via hub-and-spoke topology

### Web Application Firewall (WAF)
- Protects web apps from **common web exploits** (OWASP Top 10)
- Deployed on: **Azure Application Gateway** or **Azure Front Door**
- Protects against: SQL injection, XSS, CSRF, path traversal
- **Detection mode** — monitor only, no blocking (for initial tuning)
- **Prevention mode** — actively block detected threats

### Network Segmentation with Azure Virtual Networks (VNets)
- **VNets** create isolated network boundaries for Azure resources
- Resources in different VNets cannot communicate without explicit peering or gateway
- **Subnets** further divide VNets for additional segmentation
- Limits blast radius if one segment is compromised

### Network Security Groups (NSGs)
- **Virtual firewall rules** applied to subnets or individual network interfaces (NICs)
- Filter inbound and outbound traffic by: source/destination IP, port, protocol
- Rules evaluated by **priority** (lower number = higher priority, evaluated first)
- Default rules: deny all inbound from internet; allow VNet-to-VNet; allow outbound

### Azure Bastion
- Provides **secure RDP and SSH access** to VMs **directly from the Azure Portal**
- No need to expose VMs with public IP addresses
- Traffic goes over SSL/TLS (port 443) — no VPN needed
- Eliminates RDP/SSH exposure on the public internet

> ⭐ **Exam Tip:** Azure Bastion lets admins connect to VMs without needing a public IP or VPN client. Everything goes through the browser over HTTPS.

### Azure Key Vault
- Securely stores and manages **secrets, keys, and certificates**

| Type | Examples |
|------|---------|
| **Secrets** | Passwords, connection strings, API keys |
| **Keys** | Encryption keys (software or HSM-backed) |
| **Certificates** | SSL/TLS certificates with auto-renewal |

- Access controlled via **RBAC** and **access policies**
- Full audit trail of every access
- Eliminates hard-coded credentials in code

---

## 3.2 Security Management Capabilities of Azure

### Microsoft Defender for Cloud
Unified **security management** and **threat protection** platform for Azure, on-premises, and multi-cloud (AWS, GCP).

#### Two Core Pillars

| Pillar | What It Does |
|--------|-------------|
| **Cloud Security Posture Management (CSPM)** | Assess your security configuration; show what's misconfigured; recommend fixes |
| **Cloud Workload Protection (CWP)** | Detect and respond to threats on specific workloads (VMs, containers, databases, etc.) |

#### Secure Score
- A **numeric score** (0–100%) measuring your overall security posture
- Higher score = better security
- Each recommendation has a score impact — fixing it increases your score
- Used for: tracking improvement over time, benchmarking

#### Security Policies, Standards, and Recommendations
- **Policy** — a rule about what configurations are required (backed by Azure Policy)
- **Standard** — a collection of policies mapped to a compliance framework (e.g., NIST, ISO 27001, CIS)
- **Recommendation** — a specific action to fix a misconfiguration (e.g., "Enable MFA for accounts with owner permissions")

#### Cloud Workload Protection Plans
Defender for Cloud offers protection plans for specific resource types:

| Plan | Protects |
|------|---------|
| Defender for Servers | VMs and on-premises servers |
| Defender for Databases | Azure SQL, SQL on VMs, Cosmos DB, open-source DBs |
| Defender for Storage | Azure Blob, Files, ADLS |
| Defender for Containers | AKS, container registries |
| Defender for App Service | Web apps and APIs |
| Defender for Key Vault | Key Vault access anomalies |

---

## 3.3 Microsoft Sentinel

### What is Sentinel?
Microsoft Sentinel is Azure's **cloud-native SIEM (Security Information and Event Management)** and **SOAR (Security Orchestration, Automated Response)** platform.

| Component | Description |
|-----------|-------------|
| **SIEM** | Collect, analyse, and correlate security events from across the organization |
| **SOAR** | Automate threat detection and response using **playbooks** (Logic App workflows) |

### What Sentinel Does

| Capability | Description |
|-----------|-------------|
| **Data collection** | Ingest logs from: Azure, Microsoft 365, on-premises, third-party tools |
| **Detection** | Built-in and custom analytics rules to detect threats |
| **Investigation** | Interactive investigation graph to trace attack paths |
| **Hunting** | Proactive threat hunting using KQL queries |
| **Automation** | Playbooks (Logic Apps) auto-respond to incidents |

### Data Connectors
Sentinel connects to data sources via **connectors**:
- Microsoft services: Microsoft 365 Defender, Defender for Cloud, Entra ID
- Azure services: Activity logs, Diagnostic logs
- Third-party: Cisco, Palo Alto, AWS, Okta, Syslog

### Workbooks and Analytics Rules
- **Workbooks** — visual dashboards for monitoring security data
- **Analytics rules** — define what patterns trigger an alert/incident
- **Fusion** — Microsoft's ML-based multi-stage attack detection

---

## 3.4 Microsoft Defender XDR

**Defender XDR (Extended Detection and Response)** is an integrated suite of security products that work together to detect, investigate, and respond to threats across the Microsoft security estate.

### Defender XDR Services

| Service | Protects | Key Function |
|---------|---------|-------------|
| **Defender for Office 365** | Email, Teams, SharePoint | Anti-phishing, anti-malware, Safe Attachments, Safe Links |
| **Defender for Endpoint** | Endpoints (Windows, Mac, Linux, iOS, Android) | EDR — detect and respond to endpoint threats |
| **Defender for Cloud Apps** | SaaS apps (shadow IT) | CASB — discover, monitor, and control cloud app usage |
| **Defender for Identity** | On-premises AD | Detect lateral movement, credential attacks, compromised identities |
| **Defender Vulnerability Management** | Devices and software | Discover, assess, and remediate vulnerabilities |
| **Defender Threat Intelligence (TI)** | Threat actors and TTPs | Contextualize threats with threat intel data |

### The Microsoft Defender Portal (security.microsoft.com)
- Unified portal for all Defender XDR services
- Single pane of glass for: incidents, alerts, hunting, reports
- Replaces individual product portals
- Incidents automatically correlate related alerts across all Defender products

> ⭐ **Exam Tip:** Defender for Cloud Apps is a **CASB (Cloud Access Security Broker)**. Defender for Identity protects **on-premises Active Directory**. Defender for Endpoint is an **EDR (Endpoint Detection and Response)** solution.

### Key Security Acronyms

| Acronym | Full Name | What It Does |
|---------|-----------|-------------|
| SIEM | Security Information and Event Management | Collect, analyze, and correlate security logs |
| SOAR | Security Orchestration, Automated Response | Automate response to security incidents |
| EDR | Endpoint Detection and Response | Monitor and respond to endpoint threats |
| CASB | Cloud Access Security Broker | Govern access to cloud/SaaS applications |
| XDR | Extended Detection and Response | Unified detection across multiple domains |
| IDPS | Intrusion Detection and Prevention System | Detect and block known attack patterns |

---

> ⬅️ [← Domain 2](./domain-2-microsoft-entra.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-compliance-solutions.md)
