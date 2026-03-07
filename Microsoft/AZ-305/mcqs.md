# 📝 AZ-305: Designing Microsoft Azure Infrastructure Solutions — 50 Practice MCQs

> Mapped to official exam domains (updated January 2026):
> - **Domain 1:** Design Identity, Governance, and Monitoring Solutions — 25–30%
> - **Domain 2:** Design Data Storage Solutions — 25–30%
> - **Domain 3:** Design Business Continuity Solutions — 10–15%
> - **Domain 4:** Design Infrastructure Solutions — 25–30%

📘 [← Back to AZ-305 Index](./index.md)

> ⚠️ AZ-305 questions are **scenario-based and design-focused**. Questions ask *which service*, *why*, and *which configuration* — not implementation code.

---

## 🔵 Domain 1: Identity, Governance, and Monitoring (Q1–Q14)

---

**Q1.** A company needs to ensure that their Global Administrator accounts are only activated when needed, require approval from a second administrator, and automatically expire after 4 hours. Which Microsoft Entra feature should be implemented?

- A) Conditional Access with MFA requirement
- B) Privileged Identity Management (PIM) with eligible role assignments and approval workflow
- C) Role-Based Access Control with time-limited assignments
- D) Entra ID Identity Protection with risk-based access

✅ **Answer: B**  
*PIM provides just-in-time access — users are "eligible" for roles and must activate them with justification, optional approval, and a time limit (1–8 hours configurable). Conditional Access enforces access conditions but cannot make roles time-limited or require approval for activation.*

---

**Q2.** A multinational organisation has subsidiaries in the EU and must ensure that diagnostic logs from EU resources never leave the EU region. However, the security operations team needs centralised access to all logs. What is the recommended Log Analytics Workspace architecture?

- A) A single global workspace in West US
- B) Regional workspaces per geography (EU workspace, US workspace) with security team cross-workspace queries
- C) One workspace per Azure subscription
- D) No Log Analytics — use Azure Storage blobs per region

✅ **Answer: B**  
*When data residency regulations require logs to stay within a geography, per-region (or per-geography) workspaces satisfy compliance. The security team can run cross-workspace KQL queries across multiple workspaces. A single global workspace would violate GDPR for EU data.*

---

**Q3.** An organisation wants to automatically enable diagnostic settings on every newly created Azure SQL Database, routing logs to a central Log Analytics Workspace — without requiring manual intervention for each resource. What is the most scalable approach?

- A) Create a runbook that checks for new SQL Databases every hour
- B) Train operations staff to enable diagnostics after each deployment
- C) Assign an Azure Policy with `DeployIfNotExists` effect at subscription or management group scope
- D) Use ARM template deployment scripts to enable diagnostics during provisioning

✅ **Answer: C**  
*`DeployIfNotExists` policies automatically deploy a related resource (diagnostic settings) if it doesn't exist on the target resource. Applied at management group scope, this enforces diagnostics on all current and future SQL Databases without manual action.*

---

**Q4.** A team manages 20 web applications each running in Azure App Service. The team wants a single identity that can be assigned to all 20 apps so they share access to the same Key Vault secrets. Which Managed Identity type should be used?

- A) System-assigned Managed Identity — create one per App Service
- B) User-assigned Managed Identity — create one and assign it to all 20 App Services
- C) Service Principal with a client secret stored in the apps
- D) Certificate-based authentication with a shared certificate

✅ **Answer: B**  
*User-assigned Managed Identities are independent Azure resources that can be assigned to multiple services. One identity with the `Key Vault Secrets User` role can be shared across all 20 App Services — simpler than managing 20 system-assigned identities each needing separate role assignments.*

---

**Q5.** A company needs to block the creation of any Azure resource outside the UK South and UK West regions across all subscriptions. The policy must apply to all current and future subscriptions. At which scope should the Azure Policy be assigned?

- A) At each individual subscription
- B) At the Resource Group level
- C) At the Management Group level (root or parent group)
- D) At the Azure tenant level using a Conditional Access policy

✅ **Answer: C**  
*Azure Policy assigned at a Management Group scope applies to all current and future subscriptions beneath it via inheritance. Assigning at each subscription individually doesn't scale and won't automatically cover future subscriptions.*

---

**Q6.** A security audit flags that the company's Azure Key Vault containing production secrets is accessible via its public endpoint from the internet. Secrets are only accessed by applications running in Azure. What is the recommended design change?

- A) Enable the Key Vault firewall and whitelist all Azure datacentre IP ranges
- B) Rotate all secrets in the vault and update application configurations
- C) Create a Private Endpoint for the Key Vault and disable public network access
- D) Move secrets to Azure App Configuration instead

✅ **Answer: C**  
*Private Endpoints create a private IP address in your VNet for the Key Vault. Disabling public network access means the vault is only reachable from within your private network, eliminating internet-based attack surface. Whitelisting Azure IPs (A) is not a security improvement as it allows access from all Azure customers.*

---

**Q7.** A user is reporting they can access an Azure subscription's resources from an unmanaged personal device, which violates the company's security policy. The company wants to block access from non-Entra-ID-joined or non-compliant devices. What should be configured?

- A) Remove the user's Azure RBAC role assignments
- B) Create a Conditional Access policy requiring a compliant or Entra ID-joined device for Azure management
- C) Enable MFA for the user
- D) Enable Identity Protection risky sign-in policy

✅ **Answer: B**  
*Conditional Access can enforce device compliance as a condition for accessing Azure management (`management.azure.com`). Non-compliant and unmanaged personal devices will be blocked. MFA (C) still allows personal devices — it only adds a second factor, not a device state check.*

---

**Q8.** Application Insights is showing a high failure rate for a specific API call in a microservices application. A developer needs to trace exactly which downstream services were called, in what order, and how long each took. Which Application Insights feature provides this?

- A) Live Metrics stream
- B) Application Map
- C) Distributed Tracing (Transaction Search / End-to-end transaction details)
- D) Availability Tests

✅ **Answer: C**  
*Distributed Tracing captures the full chain of calls across microservices for a single user request, showing the sequence, duration, and failure point of each downstream call. Application Map (B) shows aggregate dependency health — not individual request traces.*

---

**Q9.** A company has three Azure subscriptions managed by different teams. Finance wants to see costs broken down by department, but all resources are under a shared subscription. What is the most appropriate cost management design?

- A) Move all resources to separate subscriptions, one per department
- B) Apply mandatory cost-centre tags to all resources using Azure Policy, then use Cost Management's grouping by tag
- C) Create a budget alert for each department in the shared subscription
- D) Use Azure Advisor recommendations to identify cost savings per department

✅ **Answer: B**  
*Tagging resources with a `CostCentre` or `Department` tag (enforced by an Azure Policy `Deny` effect to require the tag) allows Cost Management to filter and group costs by tag value — providing per-department visibility without restructuring subscriptions.*

---

**Q10.** An enterprise architect is designing an Azure environment for a new organisation. They need to enforce company-wide security baselines, manage subscriptions at scale, and isolate platform resources (identity, management, connectivity) from workload resources. What architectural pattern should they follow?

- A) All resources in a single subscription with RBAC isolation
- B) Azure Landing Zone (Enterprise Scale) with Management Groups, platform subscriptions, and landing zone subscriptions
- C) Separate Azure tenants per business unit
- D) VNet peering to isolate workloads within a flat subscription structure

✅ **Answer: B**  
*The Azure Landing Zone (Enterprise Scale) architecture is Microsoft's recommended pattern for enterprise Azure adoption. It uses Management Groups for hierarchical policy enforcement, separates platform services (identity, networking, management) from application landing zones, and scales to hundreds of subscriptions.*

---

**Q11.** An organisation wants to ensure that all Azure VM deployments automatically include a specific tag set that cannot be removed by the VM owner. Which combination of Azure Policy and resource lock achieves this?

- A) Policy with `Audit` effect + ReadOnly lock on all VMs
- B) Policy with `Modify` effect to append tags + Policy with `Deny` effect to prevent tag removal
- C) Azure Blueprints with a tag policy assignment and `DoNotDelete` lock
- D) ARM template parameter defaults and a manual tagging process

✅ **Answer: B**  
*A `Modify` policy appends required tags automatically. A separate `Deny` policy can block tag modification or removal for specific tag keys. This combination enforces tags without relying on VM owners to apply them. Blueprints (C) could also work but is more heavyweight.*

---

**Q12.** A new developer has joined the team and needs read-only access to a specific Azure SQL Database — not the entire subscription or resource group. At what scope should the RBAC role be assigned?

- A) Subscription level with the Reader role
- B) Resource Group level with the Reader role
- C) Directly on the Azure SQL Database resource with the Reader role
- D) Management Group level with a custom role

✅ **Answer: C**  
*RBAC follows the principle of least privilege. Assigning the role directly on the specific SQL Database resource grants access only to that resource. Assigning at higher scopes (subscription, resource group) gives read access to more resources than needed.*

---

**Q13.** A company uses Azure Monitor and wants to be notified when the error rate in their App Service exceeds 5% over any 15-minute window. Which alert type should be configured?

- A) Metric alert on the `Http5xx` metric with a static threshold
- B) Log alert using a KQL query on the `requests` table in Application Insights, evaluated every 15 minutes
- C) Activity Log alert for App Service deployment events
- D) Smart Detection alert (automatic — no configuration needed)

✅ **Answer: B**  
*A Log Alert queries Application Insights' `requests` table using KQL — calculating the percentage of failed requests over a 15-minute window. This provides flexible, threshold-based alerting on derived metrics. Metric alerts (A) can also work for HTTP 5xx counts, but Log Alerts enable more sophisticated calculations like percentages.*

---

**Q14.** Which Azure Conditional Access configuration would prevent all sign-ins from countries where the organisation has no business presence, while still allowing access from authorised company locations worldwide?

- A) Identity Protection — block high-risk users
- B) Conditional Access — Named Locations (allowed countries list) → "All users" → "Block" for any location not in the Named Location
- C) Entra ID B2B access restrictions — block external collaborators
- D) MFA for all users outside the corporate network

✅ **Answer: B**  
*Named Locations in Conditional Access define trusted countries or IP ranges. A policy targeting "All Cloud Apps" with a location condition excluding the Named Location set to "Block" prevents all sign-ins from unlisted countries.*

---

## 🟢 Domain 2: Design Data Storage Solutions (Q15–Q27)

---

**Q15.** An application requires a relational database that supports SQL Server-specific features including SQL Server Agent, cross-database queries, and linked server connections. The team wants a fully managed PaaS solution. Which Azure service should be selected?

- A) Azure SQL Database
- B) Azure SQL Managed Instance
- C) SQL Server on Azure VM
- D) Azure Synapse Analytics Dedicated SQL Pool

✅ **Answer: B**  
*Azure SQL Managed Instance provides near 100% SQL Server compatibility on a fully managed PaaS platform — including SQL Server Agent, cross-database queries, CLR, linked servers, and service broker. Azure SQL Database doesn't support these features.*

---

**Q16.** A SaaS company hosts 500 customer databases, each small but with unpredictable peak usage patterns throughout the day. They want to minimise per-database cost while sharing resources during peaks. Which Azure SQL deployment option is most appropriate?

- A) 500 Azure SQL Database instances with Serverless compute
- B) Azure SQL Elastic Pool with all 500 databases sharing a pool of eDTUs or vCores
- C) A single Azure SQL Database with schema-per-tenant
- D) Azure SQL Managed Instance with multiple databases

✅ **Answer: B**  
*Azure SQL Elastic Pool allows multiple databases to share a pool of compute and storage resources. When one database spikes, it uses more from the pool; when idle, others can use those resources. This is ideal for multi-tenant SaaS with variable, unpredictable per-DB load.*

---

**Q17.** A financial services application requires that the database remains readable in a secondary Azure region at all times, and failover to that region happens automatically within 30 seconds if the primary becomes unavailable. Which Azure SQL feature should be implemented?

- A) Active Geo-Replication with a manual failover script
- B) Zone Redundant Database within the primary region
- C) Auto-failover Group with automatic failover policy enabled
- D) Azure Backup with cross-region restore

✅ **Answer: C**  
*Auto-failover Groups support automatic failover to a secondary region with a configurable grace period. The failover group provides a stable listener endpoint — applications reconnect automatically without configuration changes. Active Geo-Replication (A) requires manual failover intervention.*

---

**Q18.** A development team is building a globally distributed e-commerce platform where each customer's shopping cart data must always reflect their own latest updates, but slight delays in propagating cart data to other regions are acceptable. Which Cosmos DB consistency level is most appropriate?

- A) Strong
- B) Bounded Staleness
- C) Session
- D) Eventual

✅ **Answer: C**  
*Session consistency guarantees that a user always reads their own writes within a session — ideal for shopping cart scenarios where a customer must see their own updates immediately. It avoids the overhead of Strong consistency while preventing the confusion of reading stale data from your own session.*

---

**Q19.** An analytics team needs to query petabytes of raw data stored in Azure Data Lake Storage Gen2 using T-SQL without loading the data into a dedicated data warehouse. Which service enables this?

- A) Azure SQL Database — Hyperscale tier
- B) Azure Synapse Analytics Serverless SQL Pool
- C) Azure Cosmos DB analytical store
- D) Azure Data Factory with a SQL transformation activity

✅ **Answer: B**  
*Synapse Analytics Serverless SQL Pool can query data directly in ADLS Gen2 (Parquet, CSV, Delta) using T-SQL with no data loading — paying only for data scanned. It's ideal for ad-hoc analytics and data exploration at scale without provisioning a dedicated SQL pool.*

---

**Q20.** A company stores regulatory documents in Azure Blob Storage and must retain them for 7 years without the possibility of modification or deletion — including by administrators. Which Azure Blob Storage feature satisfies this?

- A) Azure Backup policy with a 7-year retention rule
- B) Blob soft-delete with 7-year retention
- C) Immutable Blob Storage with a time-based retention policy locked via `LockPolicy`
- D) Azure Policy denying blob delete operations

✅ **Answer: C**  
*Immutable Blob Storage with a locked time-based retention policy (WORM — Write Once Read Many) prevents modification or deletion for the defined period — even by subscription owners or administrators. This is required for compliance with SEC 17a-4, FINRA, and similar regulations.*

---

**Q21.** A company migrates an on-premises file server that hosts 10 TB of shared files. Users in the London office access files over SMB. The company wants to reduce on-premises storage while keeping frequently accessed files local for performance. Which design achieves this?

- A) Azure Files with direct SMB mount from user workstations over the internet
- B) Azure File Sync with cloud tiering enabled — keeps hot files on-premises, tiers cold files to Azure Files
- C) Azure Blob Storage with a custom SMB wrapper application
- D) Azure NetApp Files with global file caching

✅ **Answer: B**  
*Azure File Sync replicates on-premises file shares to Azure Files. With cloud tiering enabled, files not recently accessed are replaced by a lightweight pointer — freeing local storage. Users access all files transparently; cold files are recalled from Azure on demand.*

---

**Q22.** An e-commerce application uses Azure Cache for Redis for session state. The company requires that cached data survives a Redis restart without data loss. Which Redis tier and feature enables this?

- A) Standard tier — it persists data automatically
- B) Premium tier with Redis persistence (RDB or AOF) enabled
- C) Basic tier with storage account backup
- D) Enterprise tier only — persistence is not available below Enterprise

✅ **Answer: B**  
*Redis persistence (RDB snapshots or AOF log) is available on the Premium tier. RDB saves periodic snapshots; AOF logs every write operation. Both enable data recovery after a restart. Standard tier does not support persistence.*

---

**Q23.** A new application needs a globally distributed database that stores social media posts as JSON documents. Posts are identified by `userId` and `postId`. Queries always filter by `userId`. What should be the partition key?

- A) `postId` — maximises uniqueness
- B) `userId` — queries always filter by this field; data is distributed by user
- C) A composite of `userId_postId` — maximises cardinality
- D) `timestamp` — enables chronological queries

✅ **Answer: B**  
*When queries always include `userId`, partitioning by `userId` makes all queries in-partition (efficient and cheap). `postId` alone would scatter related posts across partitions, making user-level queries expensive fan-outs. The composite key (C) would still result in cross-partition user queries.*

---

**Q24.** A data platform team needs to orchestrate an ETL pipeline that extracts data from an on-premises SQL Server, transforms it, and loads it into Azure Synapse Analytics. Which Azure service and component handles the on-premises extraction?

- A) Azure Synapse Analytics pipeline with an Azure Integration Runtime
- B) Azure Data Factory pipeline with a Self-hosted Integration Runtime installed on-premises
- C) Azure Logic Apps with a SQL Server connector
- D) Azure Stream Analytics with an on-premises input adapter

✅ **Answer: B**  
*A Self-hosted Integration Runtime (SHIR) installed on an on-premises machine acts as a bridge between the on-premises SQL Server and Azure Data Factory. Azure IR (cloud-only) cannot reach on-premises systems without a SHIR or VPN/ExpressRoute connection.*

---

**Q25.** An application needs to store time-series telemetry from 1 million IoT sensors, with queries that aggregate data over time windows (e.g., average temperature per device per hour). Which service is most appropriate?

- A) Azure SQL Database with a time-series table
- B) Azure Cosmos DB with a time-series partition key
- C) Azure Data Explorer (ADX)
- D) Azure Table Storage

✅ **Answer: C**  
*Azure Data Explorer (ADX) is purpose-built for time-series analytics — ingesting millions of events per second and providing ultra-fast analytical queries over time-stamped data. It uses the Kusto Query Language (KQL) and is optimised for IoT telemetry, logs, and time-series data at scale.*

---

**Q26.** An organisation uses Azure SQL Database with Transparent Data Encryption (TDE). A compliance team requires that the encryption key must be managed by the organisation (not Microsoft) and rotated on a set schedule. What should be implemented?

- A) Enable Always Encrypted on sensitive columns
- B) Configure TDE with a Customer-Managed Key (CMK) stored in Azure Key Vault
- C) Disable TDE and implement application-level encryption instead
- D) Enable Azure Defender for SQL to manage key rotation

✅ **Answer: B**  
*TDE with Customer-Managed Keys (Bring Your Own Key / BYOK) stores the TDE protector key in Azure Key Vault, giving the organisation full control over the key lifecycle — creation, rotation, and revocation. Microsoft never has access to the plaintext key.*

---

**Q27.** A solution requires connecting an on-premises Oracle database to Azure and transforming the data before loading it into Azure Cosmos DB. Which services should be used?

- A) Azure Database Migration Service (DMS) directly to Cosmos DB
- B) Azure Data Factory with a Self-hosted Integration Runtime, Oracle Linked Service, and Cosmos DB Linked Service
- C) Azure Synapse Analytics pipeline with serverless SQL pool
- D) Logic Apps with Oracle and Cosmos DB connectors

✅ **Answer: B**  
*Azure Data Factory with a Self-hosted IR supports Oracle as a source (via JDBC driver on the SHIR machine) and Cosmos DB as a sink. Transformation can be applied using data flows. DMS (A) migrates databases but doesn't orchestrate ongoing ETL pipelines to Cosmos DB.*

---

## 🟡 Domain 3: Design Business Continuity Solutions (Q28–Q36)

---

**Q28.** A company runs critical Azure VMs in UK South and must be able to recover them in UK West if UK South becomes unavailable. The recovery time objective is 30 minutes and the recovery point objective is 5 minutes. Which service meets these requirements?

- A) Azure Backup with GRS storage — restore to UK West
- B) Azure Site Recovery replicating VMs from UK South to UK West
- C) VM snapshots copied to UK West every 5 minutes via automation
- D) Zone Redundant VM deployment in UK South

✅ **Answer: B**  
*Azure Site Recovery provides continuous replication with RPO typically under 1 minute and RTO of minutes (VMs are pre-replicated and ready to start). Azure Backup (A) has RPO of hours (daily backup) and RTO of hours (restore from backup) — not suitable for these requirements.*

---

**Q29.** A data science team accidentally deletes a production Azure SQL Database. The database needs to be restored to its state from 2 hours before deletion. Which feature enables this?

- A) Azure Backup point-in-time restore
- B) Azure SQL Database built-in automated backups with point-in-time restore (PITR)
- C) Azure SQL geo-restore from a geo-redundant backup
- D) Active Geo-Replication failover

✅ **Answer: B**  
*Azure SQL Database automatically takes full, differential, and transaction log backups. PITR allows restoring to any point within the retention period (7–35 days). This is built-in with no additional configuration. Azure Backup (A) is an alternative for additional long-term retention but PITR is the native, simpler answer here.*

---

**Q30.** A Recovery Services Vault is configured with GRS redundancy. A disaster occurs in the primary Azure region. How can administrators restore a VM backup to the secondary (paired) region?

- A) GRS automatically fails over — no action needed
- B) Enable Cross-Region Restore on the vault, then restore to the secondary region from the GRS copy
- C) Manually copy the backup data from primary to secondary using AzCopy
- D) Cross-region restore is only available with GZRS vaults

✅ **Answer: B**  
*Cross-Region Restore (CRR) must be explicitly enabled on the Recovery Services Vault. Once enabled, GRS backups are accessible in the secondary region, and administrators can restore VMs there during a regional outage. It's not automatic — it requires conscious opt-in and manual restore initiation.*

---

**Q31.** An organisation needs to protect against ransomware that could encrypt or delete Azure Backup data. Which two Azure Backup features specifically defend against this threat?

- A) LRS storage and daily backups
- B) Soft delete (retains deleted backup data for 14 days) and Multi-User Authorisation (MUA) for destructive operations
- C) Immutable vault and GRS storage
- D) Azure Defender for Storage and Azure Policy backup compliance

✅ **Answer: B**  
*Soft delete prevents immediate data loss when backups are deleted — retaining data for 14 extra days so ransomware-triggered deletions can be reversed. Multi-User Authorisation (MUA) requires a second administrator to approve destructive operations (disable backup, delete data), preventing a compromised single account from destroying backups.*

---

**Q32.** A web application requires 99.99% availability. The current design uses a single App Service in one region with 99.95% SLA. What is the minimum architectural change to meet the 99.99% requirement?

- A) Upgrade the App Service Plan to Isolated (ASE) tier for a higher SLA
- B) Deploy App Service instances in two Azure regions with Traffic Manager for automatic DNS failover
- C) Enable zone redundancy for the App Service within the same region
- D) Add a second App Service in a different region and configure manual failover

✅ **Answer: C**  
*Zone-redundant App Service (Premium v2/v3 with availability zones) provides a 99.99% SLA within a single region. Traffic Manager (B) with multi-region is also valid but more complex. The simplest correct answer is zone redundancy, which is explicitly required for the 99.99% SLA.*

---

**Q33.** A company performs Azure Site Recovery test failovers monthly. What is the key requirement for a test failover to be non-disruptive to production?

- A) Test failover requires shutting down the production VM temporarily
- B) Test failover must use an isolated test VNet that has no connectivity to production networks
- C) Test failover can only be performed during a maintenance window
- D) Test failover replaces the production VM with the replicated copy

✅ **Answer: B**  
*ASR test failovers bring up the replicated VM in a specified test VNet — completely isolated from production. This allows DR drills without impacting live workloads. Production VMs continue running unaffected. If the test VNet could reach production, it could cause IP conflicts or disrupt services.*

---

**Q34.** An application's RTO requirement is 4 hours and RPO is 1 hour for a supporting database. The database is backed up hourly. The primary region goes down. What is the approximate time to recover?

- A) 1 hour (restore time from backup)
- B) 5 hours (wait 4 hours RTO + 1 hour RPO)
- C) Up to 5 hours total — up to 1 hour data loss (RPO) + up to 4 hours restore time (RTO)
- D) Immediate — hourly backups mean the database is always ready

✅ **Answer: C**  
*RPO defines maximum data loss — up to the last 1-hour backup. RTO defines maximum restore time — up to 4 hours. Total worst-case scenario: up to 1 hour of lost data and up to 4 hours of downtime. These are independent metrics that both apply.*

---

**Q35.** An organisation uses Traffic Manager with Priority routing across two regions. Primary is West Europe; secondary is North Europe. After testing, the team realises requests from Asian users are routed to West Europe (higher latency) even when both regions are healthy. What routing method change would improve performance for all global users?

- A) Switch to Weighted routing (50/50 split)
- B) Switch to Performance routing — users routed to lowest-latency endpoint
- C) Switch to Geographic routing — Asian users to a third Asia region
- D) Switch to Multivalue routing — return both endpoints

✅ **Answer: B**  
*Performance routing directs users to the Traffic Manager endpoint with the lowest latency for their location. This would route Asian users to an Asian or lower-latency endpoint if available, rather than always routing to the Priority=1 West Europe endpoint.*

---

**Q36.** An organisation operates in a single Azure region with no DR requirements. Their business continuity requirement is to survive the failure of a single datacentre within that region. What VM deployment configuration achieves this?

- A) Availability Set — groups VMs across update domains and fault domains within a datacentre
- B) Availability Zones — spreads VMs across physically separate datacentres within the region
- C) VM Scale Set in Uniform mode
- D) Azure Backup with daily snapshots

✅ **Answer: B**  
*Availability Zones span multiple physically separate datacentres (zones) within a region. If one zone's datacentre fails, VMs in other zones continue running. Availability Sets (A) protect against rack and update failures within a single datacentre — not entire datacentre failures.*

---

## 🔴 Domain 4: Design Infrastructure Solutions (Q37–Q50)

---

**Q37.** A company needs to lift-and-shift 300 on-premises VMware VMs to Azure as quickly as possible with minimal application changes. Which service and strategy should be used?

- A) Azure Migrate with the Rehost (lift-and-shift) strategy using agentless VMware replication
- B) Azure Site Recovery to migrate all VMs to Azure VMs
- C) Rebuild all applications as containerised microservices in AKS
- D) Export VMware VM templates and import them as Azure Managed Disks

✅ **Answer: A**  
*Azure Migrate Discovery and Assessment tool discovers VMware VMs, and the Server Migration tool performs agentless replication — the standard approach for large-scale VMware lift-and-shift. ASR (B) is technically possible but Azure Migrate is specifically designed and optimised for migration at scale.*

---

**Q38.** A company runs an on-premises .NET web application with a SQL Server backend. They want to migrate to Azure with minimal changes and no infrastructure management. Which is the most appropriate target for each tier?

- A) Azure VMs for both web and database
- B) Azure App Service for the web tier; Azure SQL Managed Instance for the database (SQL Server feature compatibility)
- C) Azure Container Apps for the web tier; Azure SQL Database for the database
- D) Azure Kubernetes Service for the web tier; Azure Synapse Analytics for the database

✅ **Answer: B**  
*App Service requires minimal code changes for .NET web apps (replatform). If the SQL Server uses features only in Managed Instance (SQL Agent, cross-DB queries), SQL MI is the right PaaS target. This replatforms both tiers to PaaS without requiring containerisation or refactoring.*

---

**Q39.** A startup is deploying a new API that will receive completely unpredictable traffic — sometimes thousands of requests per second, sometimes zero. They want zero infrastructure management and billing only for actual execution time. Which compute option is most appropriate?

- A) Azure VM Scale Sets with aggressive autoscaling
- B) Azure App Service on Basic plan
- C) Azure Functions on Consumption plan
- D) Azure Container Apps with KEDA scaling

✅ **Answer: C**  
*Azure Functions Consumption plan scales from zero to massive concurrency automatically and bills only per execution — no charges when idle. This perfectly matches unpredictable, bursty traffic patterns with zero infrastructure management.*

---

**Q40.** An organisation wants to deploy a hub-and-spoke network topology. All spoke VNet traffic — including inter-spoke traffic — must be inspected by a centralised firewall in the hub VNet. What configuration enables spoke-to-spoke traffic inspection?

- A) VNet peering between all spoke VNets directly
- B) User Defined Routes (UDRs) on spoke subnets pointing to Azure Firewall in the hub as the next hop for all traffic
- C) Network Security Groups on spoke subnets with rules allowing inter-spoke traffic
- D) Azure Virtual WAN with standard hub

✅ **Answer: B**  
*VNet peering is not transitive — spoke A cannot reach spoke B through the hub by default. UDRs on each spoke subnet force all outbound traffic (including inter-spoke) through the Azure Firewall in the hub, which can inspect and control it. NSGs (C) can filter but cannot route traffic through a firewall.*

---

**Q41.** A global e-commerce application serves users in Europe, Asia, and North America. It requires SSL offload, a Web Application Firewall, and routing users to the nearest healthy backend server. Which Azure service should be the internet-facing entry point?

- A) Azure Application Gateway with WAF (one per region)
- B) Azure Traffic Manager with performance routing
- C) Azure Front Door with WAF policy enabled
- D) Azure Load Balancer Standard with health probes

✅ **Answer: C**  
*Azure Front Door operates at the global edge — providing HTTP/HTTPS load balancing, SSL offload, WAF, and routing users to the nearest healthy origin (backend). Application Gateway (A) is regional; Traffic Manager (B) is DNS-based without SSL offload or WAF.*

---

**Q42.** A company needs to connect their on-premises data centre to Azure with guaranteed bandwidth, consistent low latency, and a private connection (no data over the public internet). What is the appropriate connectivity solution?

- A) Site-to-Site VPN Gateway
- B) Point-to-Site VPN Gateway
- C) Azure ExpressRoute via a connectivity provider
- D) Azure Virtual WAN with internet connectivity

✅ **Answer: C**  
*ExpressRoute provides a private, dedicated connection to Azure through a connectivity provider (MSP/colocation provider). It never traverses the public internet, providing guaranteed bandwidth, consistent latency, and a private path. VPN (A, B) is encrypted but travels over the public internet.*

---

**Q43.** Operations team members need to RDP and SSH into Azure VMs for maintenance. The security team has required that VMs must have no public IP addresses and no RDP/SSH ports open to the internet. What enables secure VM access under these constraints?

- A) Just-in-time (JIT) VM access via Microsoft Defender for Cloud
- B) Azure Bastion deployed in the hub VNet
- C) A jump server VM with a public IP and NSG rule restricting access to the ops team's IP
- D) Both A and B are valid solutions

✅ **Answer: D**  
*Both are valid: JIT VM access (A) temporarily opens RDP/SSH ports for specific IPs on-demand, then auto-closes them. Azure Bastion (B) provides browser-based RDP/SSH without any port exposure. Both eliminate standing public access. The question asks which enables secure access — both qualify.*

---

**Q44.** An application is being refactored from a monolith into microservices. Services need to communicate asynchronously, and if a service is down, messages should be retained and retried without loss. Which messaging service should be used for inter-service communication?

- A) Azure Event Grid
- B) Azure Service Bus Queue with dead-letter queue enabled
- C) Azure Event Hubs
- D) Azure Storage Queue

✅ **Answer: B**  
*Service Bus provides reliable, guaranteed delivery with at-least-once semantics, peek-lock (so messages aren't lost if the consumer fails), and a dead-letter queue for undeliverable messages. Event Grid (A) is for reactive pub/sub with no message retention. Storage Queue (D) lacks DLQ, ordering, and enterprise features.*

---

**Q45.** A company wants to expose a set of internal microservices as a managed API to external partners. The API needs authentication with API keys, rate limiting per partner, and request transformation. Which service should sit between partners and the backend microservices?

- A) Azure Application Gateway
- B) Azure API Management (APIM)
- C) Azure Front Door
- D) Azure Load Balancer

✅ **Answer: B**  
*APIM is specifically designed as an API gateway — providing subscription key management (one key per partner), rate limiting via `rate-limit` policy, request/response transformation, developer portal for documentation, and security. App Gateway (A) is a load balancer with WAF, not a full API management solution.*

---

**Q46.** A company migrates from on-premises to Azure and needs Azure VMs to resolve DNS names from their existing on-premises Active Directory DNS. Azure VMs also need to resolve Azure Private DNS zones. What design enables both?

- A) Use Azure-provided DNS only (168.63.129.16) — it resolves everything automatically
- B) Deploy Azure DNS Private Resolver with forwarding rules — forward Azure queries to Azure private DNS and on-premises queries to on-premises DNS servers
- C) Set custom DNS on the VNet to point only to on-premises DNS servers
- D) Use separate VNets — one for Azure resources, one for hybrid resources

✅ **Answer: B**  
*Azure DNS Private Resolver can conditionally forward DNS queries based on domain suffix — sending Azure private zone queries to Azure DNS and on-premises AD domain queries to on-premises DNS servers. Pointing VNet DNS to on-premises only (C) would break Azure private DNS resolution.*

---

**Q47.** A containerised application uses multiple microservices that need to scale independently based on HTTP traffic volume. One service should scale to zero when unused. The team wants Dapr for inter-service communication without managing Kubernetes directly. Which service is most appropriate?

- A) Azure Kubernetes Service (AKS)
- B) Azure Container Instances
- C) Azure Container Apps
- D) Azure App Service with Docker containers

✅ **Answer: C**  
*Azure Container Apps is built on Kubernetes with KEDA and natively supports Dapr, scale-to-zero, HTTP-based KEDA scaling, and independent per-service scaling — without exposing Kubernetes complexity. AKS (A) offers more control but requires managing Kubernetes directly.*

---

**Q48.** An architect is designing the network for a new application that consists of a web tier (public-facing), an application tier (internal APIs), and a data tier (Azure SQL Database). How should the network be segmented?

- A) All tiers in the same subnet with NSGs differentiating traffic
- B) Each tier in a separate subnet with NSGs controlling traffic between tiers; SQL Database accessed via Private Endpoint in the data subnet
- C) Each tier in a separate VNet with VNet peering
- D) Public subnet for all tiers with Azure Firewall blocking external attacks

✅ **Answer: B**  
*Network segmentation best practice: separate subnets per tier, NSGs controlling inter-subnet traffic (e.g., only app subnet can reach data subnet on port 1433), and Azure SQL accessed via Private Endpoint eliminating public exposure. Separate VNets (C) add peering complexity without additional security benefit for a single application.*

---

**Q49.** A large batch processing job needs to process 10,000 video files in parallel. Each file takes 5 minutes to process. The job runs once per week. Which compute service minimises cost while completing within a reasonable time?

- A) 10,000 Azure Functions running simultaneously
- B) A large Azure VM with many CPU cores
- C) Azure Batch with a pool of VMs that auto-scales during the job and scales to zero after
- D) An Azure Container Apps job triggered by a queue

✅ **Answer: C**  
*Azure Batch is specifically designed for large-scale parallel compute jobs. It manages a pool of VMs, distributes tasks across them, handles retries, and scales the pool down (to zero cost) when the job completes. Running 10,000 simultaneous Functions (A) is technically possible but Azure Batch provides better job management and task retry logic for HPC scenarios.*

---

**Q50.** A solutions architect needs to design an architecture for a web application that must meet the following requirements: global users with lowest possible latency, protection against OWASP threats, SSL termination at the edge, and automatic failover between Azure regions. Which combination of services fulfils all four requirements?

- A) Azure Load Balancer + Application Gateway with WAF + Traffic Manager
- B) Azure Front Door with WAF policy + zone-redundant backend App Service instances in two regions
- C) Azure CDN + Azure Application Gateway with WAF in each region
- D) Azure Traffic Manager (performance routing) + Application Gateway with WAF in each region

✅ **Answer: B**  
*Azure Front Door provides: global anycast routing (lowest latency for global users), integrated WAF policy (OWASP protection), SSL termination at edge PoPs, and automatic health-probe-based failover between regional backends. Zone-redundant App Service instances ensure backend HA within each region. This single service satisfies all four requirements without complex multi-service orchestration.*

---

## 📊 Summary by Domain

| Domain | Questions | Exam Weight |
|--------|-----------|------------|
| 1 – Identity, Governance & Monitoring | Q1–Q14 | 25–30% |
| 2 – Data Storage | Q15–Q27 | 25–30% |
| 3 – Business Continuity | Q28–Q36 | 10–15% |
| 4 – Infrastructure | Q37–Q50 | 25–30% |
| **Total** | **50** | **100%** |

---

## 🔗 Study Resources

| Resource | Link |
|----------|------|
| Official Study Guide | [AZ-305 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-305) |
| Free Practice Assessment | [Practice Questions](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-305/practice/assessment?assessment-type=practice&assessmentId=15) |
| Azure Architecture Centre | [Azure Architecture Centre](https://learn.microsoft.com/en-us/azure/architecture/) |
| Azure Well-Architected Framework | [WAF Documentation](https://learn.microsoft.com/en-us/azure/well-architected/) |
| Azure Landing Zones | [Enterprise Scale Architecture](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/) |

---

📘 [← Back to AZ-305 Index](./index.md)

*Good luck with AZ-305! 🚀 Remember — this exam tests design decisions, not syntax. For every scenario, ask: "Which service? Why? What configuration?"*
