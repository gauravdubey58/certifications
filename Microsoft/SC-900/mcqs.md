# 📝 SC-900 Practice MCQs — 50 Questions with Answers

> ⬅️ [Back to SC-900 Index](./index.md)

Aim for ≥ 80% (40/50) to feel exam-ready.

---

## Domain 1 – SCI Concepts (Q1–10)

**Q1.** What does the Zero Trust security model assume about network traffic from inside the corporate network?
- A) It is always safe because it is internal
- B) It should be trusted after the first authentication
- C) It should never be automatically trusted and must always be verified
- D) It only requires verification when coming from personal devices

> ✅ **Answer: C**
> Zero Trust's core principle is "never trust, always verify" — no traffic is trusted automatically, even from inside the corporate network.

---

**Q2.** Which security concept uses multiple layers of controls so that if one fails, others still protect the asset?
- A) Zero Trust
- B) Shared Responsibility Model
- C) Defense-in-Depth
- D) Least Privilege

> ✅ **Answer: C — Defense-in-Depth**
> Defense-in-Depth layers security controls (physical, network, compute, application, data) so that no single failure exposes all assets.

---

**Q3.** In the Shared Responsibility Model, which responsibility always belongs to the customer regardless of the cloud service model?
- A) Physical datacenter security
- B) OS patching on VMs
- C) Network infrastructure management
- D) Data and identity management

> ✅ **Answer: D**
> Customers are always responsible for their own data and managing user identities/accounts, in all cloud service models (IaaS, PaaS, SaaS).

---

**Q4.** What is the key difference between authentication and authorization?
- A) Authentication is done once; authorization is done every request
- B) Authentication verifies who you are; authorization determines what you can do
- C) Authentication uses passwords only; authorization uses tokens
- D) They are the same thing with different names

> ✅ **Answer: B**
> Authentication (AuthN) = identity verification ("who are you?"). Authorization (AuthZ) = access decision ("what are you allowed to do?").

---

**Q5.** Which encryption approach uses the same key to both encrypt and decrypt data?
- A) Asymmetric encryption
- B) Public key encryption
- C) Symmetric encryption
- D) Hashing

> ✅ **Answer: C — Symmetric encryption**
> Symmetric encryption (e.g., AES) uses one shared key for both encryption and decryption. It's faster but the key must be securely shared.

---

**Q6.** A company stores user passwords as hashed values rather than plain text. What property of hashing makes this more secure?
- A) Hashes can be decrypted with a key
- B) Hashing is reversible with the right algorithm
- C) Hashing is a one-way function — the original value cannot be recovered from the hash
- D) Hashed passwords are shorter and easier to store

> ✅ **Answer: C**
> Hashing is a one-way transformation. Even if attackers steal the hashed passwords, they cannot reverse them to get the original passwords.

---

**Q7.** Which Zero Trust principle recommends giving users only the minimum access required to do their job?
- A) Assume breach
- B) Verify explicitly
- C) Use least privilege access
- D) Segment everything

> ✅ **Answer: C — Least privilege access**
> The principle of least privilege limits access to only what is needed — reducing the potential damage if an account is compromised.

---

**Q8.** What allows a user from Company A to sign in to Company B's application using Company A's identity provider, without creating a new account?
- A) Multi-Factor Authentication
- B) Federation
- C) Conditional Access
- D) Managed Identity

> ✅ **Answer: B — Federation**
> Federation establishes a trust relationship between identity providers, allowing users to authenticate with their own IdP and access partner resources.

---

**Q9.** In defense-in-depth, which layer is considered the MOST valuable and is placed at the center?
- A) Physical security
- B) Network
- C) Perimeter
- D) Data

> ✅ **Answer: D — Data**
> Data is the ultimate asset being protected. All other layers exist to protect the data layer at the core.

---

**Q10.** Encryption that protects data while it is stored on a hard drive or database is called:
- A) Encryption in transit
- B) Asymmetric encryption
- C) Encryption at rest
- D) End-to-end encryption

> ✅ **Answer: C — Encryption at rest**
> Encryption at rest protects stored data (on disks, databases, backups). Encryption in transit protects data moving over networks.

---

## Domain 2 – Microsoft Entra (Q11–25)

**Q11.** What was Microsoft Entra ID previously known as?
- A) Microsoft Identity Platform
- B) Azure Active Directory (Azure AD)
- C) Active Directory Domain Services (AD DS)
- D) Azure Identity Management

> ✅ **Answer: B — Azure Active Directory (Azure AD)**
> Microsoft Entra ID was renamed from Azure Active Directory (Azure AD) in 2023.

---

**Q12.** A developer wants their Azure web app to access Azure Key Vault without storing any credentials in the code. What should they use?
- A) Service Principal with client secret
- B) Managed Identity
- C) Guest user account
- D) FIDO2 key

> ✅ **Answer: B — Managed Identity**
> Managed Identities allow Azure resources to authenticate to other Azure services without any credentials stored in code or config.

---

**Q13.** Which authentication method is considered the MOST phishing-resistant?
- A) Password + SMS code
- B) Microsoft Authenticator push notification
- C) FIDO2 security key
- D) Password alone

> ✅ **Answer: C — FIDO2 security key**
> FIDO2 hardware keys are bound to the specific website and cannot be phished — they are the most phishing-resistant authentication method.

---

**Q14.** What does Self-Service Password Reset (SSPR) allow users to do?
- A) Reset their manager's password
- B) Create new user accounts
- C) Reset their own password without contacting IT helpdesk
- D) Bypass MFA requirements

> ✅ **Answer: C**
> SSPR lets users reset their own passwords after verifying their identity — reducing helpdesk load and user downtime.

---

**Q15.** A company wants to require MFA only when employees sign in from outside the corporate office. Which Entra ID feature enables this?
- A) Password Protection
- B) SSPR
- C) Conditional Access with Named Location condition
- D) Privileged Identity Management

> ✅ **Answer: C**
> Conditional Access can use Named Locations (IP ranges) as a condition — requiring MFA when the sign-in originates outside trusted IP ranges.

---

**Q16.** Which Entra ID feature detects when a user account's credentials have been leaked on the dark web?
- A) Conditional Access
- B) Microsoft Entra ID Protection (user risk)
- C) Privileged Identity Management
- D) Access Reviews

> ✅ **Answer: B — Microsoft Entra ID Protection (user risk)**
> ID Protection detects user risk — including leaked/compromised credentials — using Microsoft's threat intelligence signals.

---

**Q17.** What is the primary benefit of Microsoft Entra Privileged Identity Management (PIM)?
- A) It automatically resets admin passwords every 30 days
- B) It provides Just-In-Time access to privileged roles, reducing standing admin access
- C) It prevents admins from signing in outside office hours
- D) It logs all admin activities in a spreadsheet

> ✅ **Answer: B**
> PIM reduces security risk by ensuring admins only have elevated privileges when they actively need them — not permanently.

---

**Q18.** An organization uses Microsoft Entra Connect. What does this tool do?
- A) Connects Azure to AWS
- B) Synchronises on-premises Active Directory users to Microsoft Entra ID for hybrid identity
- C) Creates VPN connections between offices
- D) Manages Azure RBAC roles

> ✅ **Answer: B**
> Entra Connect (formerly Azure AD Connect) synchronises identities from on-premises AD DS to Entra ID, enabling hybrid identity scenarios.

---

**Q19.** Which Entra ID licence tier is required to use Conditional Access policies?
- A) Free
- B) Microsoft Entra ID P1 or P2
- C) Microsoft 365 Basic
- D) Any Azure subscription

> ✅ **Answer: B — P1 or P2**
> Conditional Access requires Microsoft Entra ID P1 (included in Microsoft 365 Business Premium, E3, E5) or P2.

---

**Q20.** What is the purpose of Access Reviews in Microsoft Entra ID Governance?
- A) Monitor failed login attempts
- B) Periodically verify that users still need their current access rights
- C) Automatically provision new user accounts
- D) Reset passwords on a schedule

> ✅ **Answer: B**
> Access Reviews periodically ask managers, users, or resource owners to confirm whether existing access assignments are still appropriate.

---

**Q21.** What is a "system-assigned managed identity"?
- A) An identity shared across multiple Azure resources
- B) An identity tied to a specific Azure resource that is automatically deleted when the resource is deleted
- C) A human admin account with elevated privileges
- D) An identity used by on-premises servers

> ✅ **Answer: B**
> System-assigned managed identities are automatically created with the Azure resource and share its lifecycle — deleted when the resource is deleted.

---

**Q22.** A Global Administrator wants to temporarily activate Owner permissions on a subscription for 2 hours to perform emergency maintenance. Which tool enables this with an approval workflow?
- A) Conditional Access
- B) Access Reviews
- C) Privileged Identity Management (PIM)
- D) SSPR

> ✅ **Answer: C — PIM**
> PIM enables Just-In-Time role activation, supports time limits (e.g., 2 hours), and can require MFA and manager approval.

---

**Q23.** Which of the following is stored in Microsoft Entra ID but NOT in on-premises Active Directory Domain Services?
- A) Users
- B) Groups
- C) Service Principals (app registrations)
- D) Computer objects

> ✅ **Answer: C — Service Principals**
> Service Principals are cloud-native Entra ID constructs representing app identities. On-premises AD DS doesn't have an equivalent concept for cloud apps.

---

**Q24.** An employee is signing in from an unusual country and at an unusual time. Microsoft Entra ID Protection flags this as high-risk. What can automatically happen if Conditional Access is configured?
- A) Nothing — someone must manually review it
- B) The user's account is permanently deleted
- C) Access is blocked or the user is required to complete MFA
- D) The IT department receives a weekly digest email

> ✅ **Answer: C**
> Conditional Access integrated with ID Protection can automatically require MFA or block access based on the sign-in risk level — no manual intervention needed.

---

**Q25.** Which feature in Microsoft Entra ID allows customers to prevent users from setting commonly breached passwords?
- A) SSPR
- B) Conditional Access
- C) Password Protection (global and custom banned password lists)
- D) PIM

> ✅ **Answer: C — Password Protection**
> Entra ID Password Protection maintains a global banned list of weak/breached passwords and allows organizations to add their own custom banned terms.

---

## Domain 3 – Security Solutions (Q26–40)

**Q26.** A developer connects to a production Linux VM via SSH in their browser without opening any ports or using a VPN. Which Azure service enables this?
- A) Azure Firewall
- B) Azure VPN Gateway
- C) Azure Bastion
- D) NSG

> ✅ **Answer: C — Azure Bastion**
> Azure Bastion provides browser-based RDP and SSH access to VMs over HTTPS — no public IP, no VPN client, no exposed ports needed.

---

**Q27.** Which Azure service protects web applications from attacks like SQL injection and cross-site scripting (XSS)?
- A) Azure Firewall
- B) NSG
- C) Azure DDoS Protection
- D) Web Application Firewall (WAF)

> ✅ **Answer: D — WAF**
> WAF (deployed on Application Gateway or Azure Front Door) specifically protects web applications from OWASP Top 10 vulnerabilities.

---

**Q28.** What is the key difference between Azure Firewall and a Network Security Group (NSG)?
- A) NSGs are more expensive than Azure Firewall
- B) Azure Firewall is FQDN-aware and stateful; NSGs filter by IP/port only
- C) NSGs protect the entire VNet; Azure Firewall only protects one subnet
- D) They are identical in functionality

> ✅ **Answer: B**
> Azure Firewall can filter by fully qualified domain name (FQDN) and is centrally managed. NSGs work at Layer 3/4 only (IP and port).

---

**Q29.** Where would you store an application's database connection string securely in Azure?
- A) In the application's appsettings.json file
- B) As a GitHub secret
- C) In Azure Key Vault
- D) In an Azure Blob Storage container

> ✅ **Answer: C — Azure Key Vault**
> Key Vault is the designated service for securely storing secrets like connection strings, with access logging and role-based access control.

---

**Q30.** What does Microsoft Defender for Cloud's "Secure Score" represent?
- A) The number of active threats detected
- B) The speed of security incident response
- C) A numeric measure of your overall security posture — higher is better
- D) The cost of implementing all security recommendations

> ✅ **Answer: C**
> Secure Score is a 0–100% metric in Defender for Cloud indicating how well your environment aligns with security best practices. Implementing recommendations increases the score.

---

**Q31.** Microsoft Defender for Cloud provides protection for resources across which environments?
- A) Azure only
- B) Azure and Microsoft 365 only
- C) Azure, AWS, GCP, and on-premises (multi-cloud and hybrid)
- D) On-premises datacenters only

> ✅ **Answer: C**
> Defender for Cloud supports multi-cloud and hybrid scenarios — it can protect resources in Azure, AWS, GCP, and on-premises servers.

---

**Q32.** What does CSPM stand for in the context of Microsoft Defender for Cloud?
- A) Cloud Service Protection Management
- B) Cloud Security Posture Management
- C) Compliance Standards and Policy Monitoring
- D) Centralised Security Platform Manager

> ✅ **Answer: B — Cloud Security Posture Management**
> CSPM focuses on assessing your security configuration and providing recommendations to improve your posture.

---

**Q33.** Which Microsoft security product is classified as a CASB (Cloud Access Security Broker)?
- A) Microsoft Defender for Endpoint
- B) Microsoft Defender for Identity
- C) Microsoft Defender for Cloud Apps
- D) Microsoft Sentinel

> ✅ **Answer: C — Microsoft Defender for Cloud Apps**
> Defender for Cloud Apps is Microsoft's CASB — it discovers shadow IT, monitors SaaS app usage, and governs cloud application access.

---

**Q34.** Which Defender XDR product specifically protects on-premises Active Directory from credential-based attacks and lateral movement?
- A) Defender for Endpoint
- B) Defender for Identity
- C) Defender for Office 365
- D) Defender for Cloud

> ✅ **Answer: B — Defender for Identity**
> Defender for Identity monitors on-premises AD for suspicious activity — including pass-the-hash attacks, lateral movement, and privilege escalation.

---

**Q35.** What is Microsoft Sentinel?
- A) A firewall service for Azure networks
- B) A cloud-native SIEM and SOAR platform for detecting and responding to security threats
- C) A compliance management tool
- D) An endpoint antivirus product

> ✅ **Answer: B**
> Microsoft Sentinel is Azure's cloud-native SIEM (Security Information and Event Management) and SOAR (Security Orchestration, Automated Response) platform.

---

**Q36.** In Microsoft Sentinel, what are "playbooks" used for?
- A) Manual security investigation procedures
- B) Automated response actions triggered by security incidents (built on Azure Logic Apps)
- C) Documentation of security policies
- D) Training materials for security teams

> ✅ **Answer: B**
> Playbooks are SOAR automation workflows in Sentinel. They use Azure Logic Apps to automatically respond to alerts — e.g., block a user, send a Teams message, open a ticket.

---

**Q37.** Which Defender for Office 365 feature scans email attachments in a sandbox environment before delivering them to users?
- A) Safe Links
- B) Anti-spam filtering
- C) Safe Attachments
- D) Mail flow rules

> ✅ **Answer: C — Safe Attachments**
> Safe Attachments detonates email attachments in a virtual sandbox to detect malware before the email is delivered to the recipient.

---

**Q38.** A company wants to discover which unsanctioned cloud apps their employees are using. Which service provides this capability?
- A) Microsoft Defender for Endpoint
- B) Microsoft Defender for Cloud Apps (CASB)
- C) Microsoft Sentinel
- D) Azure Policy

> ✅ **Answer: B — Microsoft Defender for Cloud Apps**
> Defender for Cloud Apps (CASB) discovers shadow IT by analysing network traffic and identifying all cloud apps being accessed by employees.

---

**Q39.** Which Defender XDR product monitors and responds to threats on Windows, Mac, Linux, iOS, and Android devices?
- A) Defender for Identity
- B) Defender for Office 365
- C) Defender for Cloud Apps
- D) Defender for Endpoint

> ✅ **Answer: D — Defender for Endpoint**
> Defender for Endpoint is the EDR (Endpoint Detection and Response) solution that protects devices across multiple platforms.

---

**Q40.** An organization receives a volumetric DDoS attack against their public-facing website. Which Azure service automatically mitigates this?
- A) Azure Firewall
- B) NSG
- C) Azure DDoS Protection
- D) Azure Bastion

> ✅ **Answer: C — Azure DDoS Protection**
> Azure DDoS Protection automatically detects and mitigates volumetric DDoS attacks against Azure public IP resources.

---

## Domain 4 – Compliance Solutions (Q41–50)

**Q41.** Where can organizations find third-party audit reports and compliance certifications for Microsoft cloud services?
- A) Microsoft Learn
- B) The Azure Portal
- C) The Microsoft Service Trust Portal (servicetrust.microsoft.com)
- D) Microsoft Defender for Cloud

> ✅ **Answer: C — Service Trust Portal**
> The Service Trust Portal (servicetrust.microsoft.com) hosts audit reports, compliance documentation, and trust information for Microsoft cloud services.

---

**Q42.** What does the Compliance Score in Microsoft Purview Compliance Manager measure?
- A) The security posture of Azure resources
- B) How well the organization has implemented recommended compliance controls against specific regulations
- C) The number of data breaches in the past 12 months
- D) The cost of achieving regulatory compliance

> ✅ **Answer: B**
> The Compliance Score measures your progress in implementing recommended controls mapped to specific compliance frameworks (GDPR, HIPAA, ISO 27001, etc.).

---

**Q43.** A legal team needs to preserve all emails from a specific employee for an ongoing litigation case, even if the employee deletes them. Which Microsoft Purview feature enables this?
- A) Sensitivity labels
- B) DLP policy
- C) Legal Hold (eDiscovery)
- D) Retention label

> ✅ **Answer: C — Legal Hold (eDiscovery)**
> Legal Holds in eDiscovery preserve content that would otherwise be deleted, ensuring evidence is available for litigation or regulatory investigations.

---

**Q44.** Which tool in Microsoft Purview prevents users from accidentally emailing documents containing credit card numbers to external recipients?
- A) Sensitivity labels
- B) Data Loss Prevention (DLP)
- C) Retention policies
- D) Compliance Manager

> ✅ **Answer: B — Data Loss Prevention (DLP)**
> DLP policies detect sensitive information in content and can block, warn, or audit when it's shared outside the organization.

---

**Q45.** An admin applies both a Retention Policy and a Retention Label to the same document. Which one takes priority?
- A) The Retention Policy always wins
- B) They average out their settings
- C) The Retention Label takes priority over the Retention Policy
- D) Neither applies — they conflict and cancel out

> ✅ **Answer: C**
> Retention Labels take priority over Retention Policies when both apply to the same content. The most specific setting wins.

---

**Q46.** Which Microsoft Purview feature is designed to detect when a departing employee is copying sensitive files to a personal USB drive?
- A) eDiscovery
- B) Audit
- C) Insider Risk Management
- D) DLP

> ✅ **Answer: C — Insider Risk Management**
> Insider Risk Management uses signals from Microsoft 365 activity (file access, copying, HR data) to detect risky behaviours by internal users such as departing employees.

---

**Q47.** What does a sensitivity label that is configured with encryption do when applied to a document?
- A) It permanently deletes the document after 90 days
- B) It restricts who can open or edit the document based on the label's settings, even if the file is shared outside the org
- C) It adds a visible watermark only
- D) It prevents the document from being saved to OneDrive

> ✅ **Answer: B**
> Sensitivity labels with encryption travel with the document — the encryption settings are enforced regardless of where the file is stored or shared.

---

**Q48.** A security administrator wants to see a timeline of all the actions taken on files labelled as "Highly Confidential" over the last 30 days. Which Purview tool provides this?
- A) Content Explorer
- B) Activity Explorer
- C) Compliance Manager
- D) Audit (Standard)

> ✅ **Answer: B — Activity Explorer**
> Activity Explorer shows a timeline of labelling and protection activities on content — what happened to sensitive files and when.

---

**Q49.** What makes a record in Microsoft Purview Records Management different from regular retained content?
- A) Records are stored in Azure Blob Storage
- B) Records cannot be modified or deleted while the record status is active
- C) Records are automatically shared with regulators
- D) Records expire after 7 years

> ✅ **Answer: B**
> When content is marked as a record, it becomes immutable — it cannot be edited or deleted until the record period expires. This ensures regulatory compliance.

---

**Q50.** Which Microsoft Purview solution helps organisations respond to individual data subject requests (e.g., "delete all data you have about me") under privacy regulations like GDPR?
- A) Compliance Manager
- B) eDiscovery
- C) Microsoft Priva — Subject Rights Requests
- D) Insider Risk Management

> ✅ **Answer: C — Microsoft Priva Subject Rights Requests**
> Priva's Subject Rights Requests feature automates and streamlines the process of finding, reviewing, and responding to GDPR/CCPA data subject requests.

---

## 📊 Score Tracker

| Domain | Questions | Your Score |
|--------|-----------|-----------|
| Domain 1 – SCI Concepts | Q1–Q10 (10 Qs) | /10 |
| Domain 2 – Microsoft Entra | Q11–Q25 (15 Qs) | /15 |
| Domain 3 – Security Solutions | Q26–Q40 (15 Qs) | /15 |
| Domain 4 – Compliance Solutions | Q41–Q50 (10 Qs) | /10 |
| **Total** | **50 questions** | **/50** |

**Scoring guide:** 45–50 ✅ Ready | 38–44 🟡 Review weak areas | <38 🔴 Keep studying

---

> ⬅️ [Back to SC-900 Index](./index.md) | ⚡ [Quick Reference](./quick-reference.md)
