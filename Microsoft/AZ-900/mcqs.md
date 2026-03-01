# 📝 AZ-900 Practice MCQs — 50 Questions with Answers

> ⬅️ [Back to AZ-900 Index](./index.md)

**Instructions:** Attempt each question before reading the answer. Aim for ≥ 80% to feel exam-ready.

---

## Domain 1 – Cloud Concepts (Questions 1–18)

**Q1.** What is cloud computing?
- A) Software installed on personal computers
- B) The delivery of computing services over the internet
- C) A type of physical server room
- D) An operating system made by Microsoft

> ✅ **Answer: B**
> Cloud computing is the delivery of computing services — including servers, storage, databases, networking, and software — over the internet.

---

**Q2.** A company notices its web app slows down every Black Friday. They want it to automatically handle the spike and scale back down after. Which cloud benefit is this?
- A) High Availability
- B) Geo-distribution
- C) Elasticity
- D) Disaster Recovery

> ✅ **Answer: C — Elasticity**
> Elasticity is the ability to automatically scale resources up and down based on real-time demand.

---

**Q3.** Which cloud service model gives you the MOST control over the underlying infrastructure?
- A) SaaS
- B) PaaS
- C) IaaS
- D) FaaS

> ✅ **Answer: C — IaaS**
> IaaS gives you control over the OS, applications, and data. You manage the most while the cloud provider manages the physical hardware.

---

**Q4.** Microsoft 365 (formerly Office 365) is an example of which service model?
- A) IaaS
- B) PaaS
- C) SaaS
- D) CaaS

> ✅ **Answer: C — SaaS**
> Microsoft 365 is ready-to-use software delivered over the internet. Microsoft manages everything; you just use it.

---

**Q5.** A development team wants to deploy a web application without managing the underlying OS or servers. Which service model fits best?
- A) IaaS
- B) PaaS
- C) SaaS
- D) On-premises

> ✅ **Answer: B — PaaS**
> PaaS provides a managed platform for developers. They focus on code and data; the cloud manages OS, runtime, and infrastructure.

---

**Q6.** What is the key difference between a Public Cloud and a Private Cloud?
- A) Public cloud is only for government use
- B) Private cloud is hosted on the internet; public cloud is not
- C) Public cloud infrastructure is shared and owned by a provider; private cloud is dedicated to one organization
- D) Public cloud is always on-premises

> ✅ **Answer: C**
> Public cloud = shared, provider-owned infrastructure over the internet. Private cloud = dedicated infrastructure for one organization.

---

**Q7.** A company wants to keep sensitive customer data on-premises while using Azure for its public-facing web tier. Which deployment model is this?
- A) Public cloud
- B) Private cloud
- C) Hybrid cloud
- D) Multi-cloud

> ✅ **Answer: C — Hybrid cloud**
> Hybrid cloud combines on-premises private infrastructure with public cloud services, allowing data to move between them.

---

**Q8.** Converting from CapEx to OpEx means moving from:
- A) On-premises servers to rented office space
- B) Upfront purchases of hardware to a pay-as-you-go cloud model
- C) Monthly billing to annual billing
- D) Free software to licensed software

> ✅ **Answer: B**
> CapEx = buying hardware upfront. OpEx = paying for cloud services as you use them. Cloud computing converts CapEx to OpEx.

---

**Q9.** In Azure's consumption-based model, you pay for:
- A) Everything provisioned, whether used or not
- B) Only resources you actually use
- C) A fixed monthly fee regardless of usage
- D) Resources based on the number of users in your company

> ✅ **Answer: B**
> Azure's consumption model means you only pay for what you use. Unused resources don't incur charges (if de-provisioned).

---

**Q10.** Under the Shared Responsibility Model, which responsibility ALWAYS belongs to the customer regardless of service model?
- A) Physical datacenter security
- B) OS patching
- C) Network infrastructure
- D) Data and identity management

> ✅ **Answer: D — Data and identity management**
> Customers are always responsible for their data and managing user identities/access, regardless of whether they use IaaS, PaaS, or SaaS.

---

**Q11.** Who is responsible for physical datacenter security in all cloud service models?
- A) The customer
- B) A third-party auditor
- C) The cloud provider (Microsoft)
- D) Shared between customer and provider

> ✅ **Answer: C — The cloud provider (Microsoft)**
> Microsoft is always responsible for the physical security of Azure datacenters.

---

**Q12.** Which cloud benefit ensures an application remains accessible even if one Azure datacenter fails?
- A) Elasticity
- B) High Availability
- C) Agility
- D) Scalability

> ✅ **Answer: B — High Availability**
> High Availability means the system remains accessible despite failures. Achieved through redundancy and Availability Zones.

---

**Q13.** What does "economies of scale" mean in the context of cloud computing?
- A) You can only use cloud if you're a large company
- B) Larger companies pay more for cloud services
- C) Cloud providers pass cost savings to customers because they operate at massive scale
- D) Cloud resources automatically scale to any size

> ✅ **Answer: C**
> Because providers like Microsoft serve millions of customers, they can operate at lower cost per unit and pass those savings to customers.

---

**Q14.** A startup wants to launch a product quickly without buying servers. Which cloud characteristic supports this?
- A) Disaster Recovery
- B) High Availability
- C) Agility
- D) Geo-distribution

> ✅ **Answer: C — Agility**
> Agility refers to the ability to rapidly provision and deploy cloud resources — from weeks/months to minutes.

---

**Q15.** Which cloud model uses MULTIPLE public cloud providers, such as Azure AND AWS?
- A) Hybrid
- B) Private
- C) Community
- D) Multi-cloud

> ✅ **Answer: D — Multi-cloud**
> Multi-cloud involves using services from more than one public cloud provider simultaneously.

---

**Q16.** An organization buys new servers every 5 years for their data center. This is an example of:
- A) OpEx
- B) CapEx
- C) Consumption-based spending
- D) Reserved pricing

> ✅ **Answer: B — CapEx**
> Buying physical servers is a capital expenditure — a large upfront investment in hardware assets.

---

**Q17.** In a SaaS model, which of the following is the customer's responsibility?
- A) OS patching
- B) Network infrastructure
- C) Managing user access and data
- D) Middleware updates

> ✅ **Answer: C — Managing user access and data**
> In SaaS, the customer manages who has access to the app and their own data. Everything else is managed by the provider.

---

**Q18.** What is the PRIMARY benefit of geo-distribution in cloud computing?
- A) Reduced cost of compute
- B) Deploying apps closer to users for lower latency
- C) Automatic failover between services
- D) Centralizing all data in one location

> ✅ **Answer: B**
> Geo-distribution allows apps to be deployed in multiple regions worldwide, reducing latency for users by serving them from nearby locations.

---

## Domain 2 – Azure Architecture and Services (Questions 19–38)

**Q19.** What is an Azure Availability Zone?
- A) A type of virtual machine
- B) A separate physical datacenter within a region with its own power, cooling, and networking
- C) A global network of CDN nodes
- D) A backup region for disaster recovery

> ✅ **Answer: B**
> Availability Zones are physically separated datacenters within one Azure region, protecting against datacenter-level failures.

---

**Q20.** A company needs to run a legacy Windows application on Azure with full control over the OS and software. Which service should they use?
- A) Azure App Service
- B) Azure Functions
- C) Azure Virtual Machine
- D) Azure Container Instances

> ✅ **Answer: C — Azure Virtual Machine**
> VMs provide IaaS — full control over OS, software, and configuration. Ideal for legacy apps and lift-and-shift migrations.

---

**Q21.** Which Azure compute service automatically scales the number of identical VM instances based on demand?
- A) Azure App Service
- B) Azure VM Scale Sets
- C) Azure Functions
- D) Azure Kubernetes Service

> ✅ **Answer: B — Azure VM Scale Sets**
> VM Scale Sets automatically increase or decrease the number of identical VM instances based on demand or a schedule.

---

**Q22.** A developer wants to run code that triggers whenever a file is uploaded to Azure Blob Storage. Which service is most appropriate?
- A) Azure Virtual Machine
- B) Azure App Service
- C) Azure Functions
- D) Azure Kubernetes Service

> ✅ **Answer: C — Azure Functions**
> Azure Functions is serverless compute triggered by events — including blob storage uploads — with no server management needed.

---

**Q23.** Which Azure networking service connects an Azure VNet to an on-premises network over the public internet using an encrypted tunnel?
- A) ExpressRoute
- B) Azure CDN
- C) VPN Gateway
- D) Azure Firewall

> ✅ **Answer: C — VPN Gateway**
> VPN Gateway creates an encrypted IPsec/IKE tunnel over the internet connecting Azure to on-premises.

---

**Q24.** A company requires a dedicated private circuit from their headquarters to Azure with guaranteed bandwidth and no internet traffic. Which service should they use?
- A) VPN Gateway
- B) Azure CDN
- C) ExpressRoute
- D) Azure DNS

> ✅ **Answer: C — ExpressRoute**
> ExpressRoute provides a private, dedicated connection that bypasses the internet entirely — more reliable and higher bandwidth than VPN.

---

**Q25.** Which Azure storage redundancy option replicates data across three availability zones within a single region?
- A) LRS
- B) GRS
- C) RA-GRS
- D) ZRS

> ✅ **Answer: D — ZRS (Zone-Redundant Storage)**
> ZRS replicates 3 copies across 3 separate availability zones in the same region, protecting against datacenter failure.

---

**Q26.** Which Azure storage tier has the lowest storage cost but requires hours to rehydrate before data can be accessed?
- A) Hot
- B) Cool
- C) Cold
- D) Archive

> ✅ **Answer: D — Archive**
> Archive tier is the cheapest for storage but data must be rehydrated (moved to Hot or Cool) before it can be read, which can take hours.

---

**Q27.** What is Microsoft Entra ID?
- A) On-premises Active Directory Domain Services
- B) Azure's cloud-based identity and access management service
- C) A firewall service for Azure networks
- D) A database for storing user credentials

> ✅ **Answer: B**
> Microsoft Entra ID (formerly Azure AD) is Microsoft's cloud identity service for managing users, apps, and access.

---

**Q28.** Which RBAC role can create and manage Azure resources but CANNOT assign roles to other users?
- A) Owner
- B) Reader
- C) Contributor
- D) User Access Administrator

> ✅ **Answer: C — Contributor**
> Contributors can create/manage resources but cannot manage access (assign roles). That's what separates them from Owners.

---

**Q29.** Which Azure security service provides a centralized "Secure Score" and recommendations to improve your security posture?
- A) Azure Key Vault
- B) Microsoft Defender for Cloud
- C) Azure Firewall
- D) Microsoft Sentinel

> ✅ **Answer: B — Microsoft Defender for Cloud**
> Defender for Cloud provides the Secure Score — a metric measuring your overall security posture with actionable recommendations.

---

**Q30.** Where should you store application secrets, encryption keys, and SSL certificates in Azure?
- A) Azure Blob Storage
- B) App Service configuration
- C) Azure Key Vault
- D) Microsoft Entra ID

> ✅ **Answer: C — Azure Key Vault**
> Azure Key Vault is specifically designed to securely store and manage secrets, keys, and certificates with access logging and policy controls.

---

**Q31.** What does MFA (Multi-Factor Authentication) require?
- A) A password at least 12 characters long
- B) Two or more verification factors (e.g., password + phone)
- C) Biometric authentication only
- D) Logging in from a trusted device

> ✅ **Answer: B**
> MFA requires two or more factors: something you know (password), something you have (phone/app), or something you are (biometric).

---

**Q32.** Which Azure service allows you to run Docker containers quickly without managing VMs or orchestration?
- A) Azure Kubernetes Service (AKS)
- B) Azure Virtual Machine
- C) Azure Container Instances (ACI)
- D) Azure App Service

> ✅ **Answer: C — Azure Container Instances (ACI)**
> ACI is the simplest way to run containers in Azure without any orchestration or VM management overhead.

---

**Q33.** An NSG (Network Security Group) operates at which network level?
- A) Application layer (Layer 7)
- B) Transport/Network layer (Layer 3/4)
- C) Physical layer
- D) DNS layer

> ✅ **Answer: B — Layer 3/4**
> NSGs filter traffic based on IP addresses, ports, and protocols (Layer 3/4). Application Gateway handles Layer 7.

---

**Q34.** Which Azure service distributes traffic across multiple VMs at the transport layer (Layer 4)?
- A) Azure Application Gateway
- B) Azure Front Door
- C) Azure Load Balancer
- D) Azure CDN

> ✅ **Answer: C — Azure Load Balancer**
> Azure Load Balancer operates at Layer 4 (TCP/UDP). Application Gateway operates at Layer 7 (HTTP/HTTPS).

---

**Q35.** Which Azure storage service is best for storing virtual machine disks?
- A) Azure Blob Storage
- B) Azure Files
- C) Azure Table Storage
- D) Azure Disk Storage (Managed Disks)

> ✅ **Answer: D — Azure Disk Storage (Managed Disks)**
> Managed Disks are persistent block storage designed specifically for Azure VMs.

---

**Q36.** What is GRS (Geo-Redundant Storage)?
- A) Storage replicated across 3 datacenters in the same region
- B) Storage replicated to 3 zones in one region
- C) Storage replicated to a secondary paired region asynchronously
- D) Storage replicated across 5 regions simultaneously

> ✅ **Answer: C**
> GRS replicates data to a paired Azure region asynchronously, protecting against regional disasters with 6 total copies.

---

**Q37.** Which Azure service is a cloud-native SIEM and SOAR solution for detecting and responding to security threats?
- A) Microsoft Defender for Cloud
- B) Azure Key Vault
- C) Microsoft Sentinel
- D) Azure Firewall

> ✅ **Answer: C — Microsoft Sentinel**
> Microsoft Sentinel is Azure's cloud-native SIEM (Security Information and Event Management) and SOAR (Security Orchestration, Automation, Response) platform.

---

**Q38.** Which Conditional Access condition could require MFA only when a user signs in from outside the corporate office?
- A) User group membership
- B) Sign-in risk level
- C) Named location (IP-based)
- D) Device compliance

> ✅ **Answer: C — Named location**
> Conditional Access can use named locations (IP address ranges) to determine if a sign-in is from a trusted location (e.g., office) and apply policies accordingly.

---

## Domain 3 – Management and Governance (Questions 39–50)

**Q39.** Which tool should you use to ESTIMATE the cost of Azure resources BEFORE deploying them?
- A) Azure Cost Management
- B) Azure TCO Calculator
- C) Azure Pricing Calculator
- D) Azure Advisor

> ✅ **Answer: C — Azure Pricing Calculator**
> The Pricing Calculator lets you configure expected resources and get a monthly cost estimate before any deployment.

---

**Q40.** A CIO wants to know how much money the company could save by migrating on-premises servers to Azure. Which tool helps?
- A) Azure Pricing Calculator
- B) Azure TCO Calculator
- C) Azure Cost Management
- D) Azure Monitor

> ✅ **Answer: B — Azure TCO Calculator**
> The Total Cost of Ownership (TCO) Calculator compares on-premises infrastructure costs vs Azure costs to show potential savings.

---

**Q41.** A company wants to prevent any Azure resource from being deleted accidentally across all subscriptions. What should they use?
- A) Azure Policy (Audit effect)
- B) RBAC Reader role
- C) Resource Locks (CanNotDelete)
- D) Azure Blueprints

> ✅ **Answer: C — Resource Locks (CanNotDelete)**
> CanNotDelete locks prevent deletion even for users with Owner role. Must be removed before deletion is possible.

---

**Q42.** An Owner of an Azure subscription tries to delete a resource but gets an access denied error. What is the most likely reason?
- A) The Owner role doesn't have delete permission
- B) The resource has a Resource Lock applied
- C) The subscription is expired
- D) MFA was not completed

> ✅ **Answer: B — Resource Lock applied**
> Resource Locks override RBAC — even Owners cannot delete a locked resource without first removing the lock.

---

**Q43.** Which Azure governance tool can automatically DENY the creation of resources in unapproved regions?
- A) Azure Advisor
- B) Azure Resource Locks
- C) Azure Policy
- D) Management Groups

> ✅ **Answer: C — Azure Policy**
> Azure Policy with the Deny effect prevents resource creation that violates policy rules, such as deploying to unapproved regions.

---

**Q44.** What is the correct order of Azure resource hierarchy from highest to lowest?
- A) Subscription → Management Group → Resource Group → Resource
- B) Management Group → Subscription → Resource Group → Resource
- C) Resource Group → Subscription → Management Group → Resource
- D) Management Group → Resource Group → Subscription → Resource

> ✅ **Answer: B**
> The correct hierarchy is: Management Group → Subscription → Resource Group → Resource.

---

**Q45.** Which Azure tool provides FREE personalized recommendations for improving reliability, security, performance, and cost?
- A) Azure Monitor
- B) Azure Service Health
- C) Azure Advisor
- D) Microsoft Defender for Cloud

> ✅ **Answer: C — Azure Advisor**
> Azure Advisor is a free service that analyzes your Azure usage and provides prioritized recommendations across 5 categories.

---

**Q46.** A company wants to monitor the health of Azure services in the regions they use and receive notifications of outages. Which tool should they use?
- A) Azure Monitor
- B) Azure Service Health
- C) Azure Advisor
- D) Application Insights

> ✅ **Answer: B — Azure Service Health**
> Azure Service Health provides personalized alerts about Azure service issues, planned maintenance, and health advisories affecting your regions and services.

---

**Q47.** Which infrastructure-as-code option uses a simplified domain-specific language that compiles into ARM templates?
- A) Azure CLI
- B) ARM Template (JSON)
- C) Bicep
- D) Terraform

> ✅ **Answer: C — Bicep**
> Bicep is Microsoft's modern DSL for Azure IaC. It's more readable than JSON ARM templates and compiles to ARM JSON.

---

**Q48.** What is the SLA implication of using two services with 99.9% SLA each in the same solution?
- A) The combined SLA is still 99.9%
- B) The combined SLA improves to 99.99%
- C) The combined SLA decreases to approximately 99.8%
- D) SLAs don't apply when combining services

> ✅ **Answer: C — approximately 99.8%**
> Composite SLA = 99.9% × 99.9% = 99.8001%. Combining services multiplies (doesn't average) the SLAs, resulting in a lower combined guarantee.

---

**Q49.** Which Azure service lifecycle phase means a feature is openly available but NOT yet covered by an SLA and should NOT be used in production?
- A) General Availability (GA)
- B) Public Preview
- C) Private Preview
- D) Limited Release

> ✅ **Answer: B — Public Preview**
> Public Preview features are available to everyone but carry no SLA guarantees and are not suitable for production workloads.

---

**Q50.** A company has 12 Azure subscriptions across different departments. They want to apply a single compliance policy across ALL subscriptions. What should they use?
- A) Apply the policy to each subscription individually
- B) Use a Management Group and apply the policy there
- C) Use Azure Blueprints in each subscription
- D) Create a Resource Group containing all subscriptions

> ✅ **Answer: B — Management Group**
> Management Groups contain multiple subscriptions. Policies applied to a Management Group inherit down to all subscriptions and resources within it.

---

## 📊 Score Tracker

| Domain | Questions | Your Score |
|--------|-----------|-----------|
| Domain 1 – Cloud Concepts | Q1–Q18 (18 questions) | /18 |
| Domain 2 – Azure Architecture & Services | Q19–Q38 (20 questions) | /20 |
| Domain 3 – Management & Governance | Q39–Q50 (12 questions) | /12 |
| **Total** | **50 questions** | **/50** |

**Scoring guide:**
- 45–50 ✅ Excellent — You're ready!
- 38–44 🟡 Good — Review weak areas
- Below 38 🔴 Keep studying — revisit the notes

---

> ⬅️ [Back to AZ-900 Index](./index.md) | ⚡ [Quick Reference](./quick-reference.md)
