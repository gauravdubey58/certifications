# ☁️ AZ-900: Describe Cloud Computing - Notes

------------------------------------------------------------------------

## 1️⃣ Introduction to Cloud Computing

### What is Cloud Computing?

Cloud computing is the **delivery of computing services over the
internet**.

These services include: - Compute (VMs, containers) - Storage -
Networking - Databases - Analytics - AI - Software

Instead of owning physical servers, you **rent resources from a cloud
provider** like Microsoft Azure.

------------------------------------------------------------------------

## 2️⃣ Key Characteristics of Cloud (Exam Important)

1.  **On-demand self-service** -- Create resources when needed.
2.  **Broad network access** -- Accessible over internet.
3.  **Resource pooling** -- Shared infrastructure (multi-tenant).
4.  **Rapid elasticity** -- Scale up/down quickly.
5.  **Measured service** -- Pay only for what you use.

------------------------------------------------------------------------

## 3️⃣ Benefits of Cloud Computing

  Benefit             Explanation
  ------------------- ---------------------------------
  High availability   Services available with SLA
  Scalability         Increase/decrease resources
  Elasticity          Auto-scale during demand spikes
  Agility             Deploy in minutes
  Fault tolerance     Failover to another region
  Disaster recovery   Backup and geo-redundancy
  Global reach        Deploy worldwide

------------------------------------------------------------------------

## 4️⃣ Shared Responsibility Model (Very Important)

### On-Premises

You manage EVERYTHING: - Physical servers - Networking - OS -
Applications - Data - Security

### IaaS (Infrastructure as a Service)

Example: Azure Virtual Machines

Microsoft manages: - Physical servers - Networking - Datacenter -
Virtualization

Customer manages: - OS - Applications - Data - Access management

### PaaS (Platform as a Service)

Example: Azure App Service, Azure SQL Database

Microsoft manages: - Infrastructure - OS - Runtime - Middleware

Customer manages: - Applications - Data - Access

### SaaS (Software as a Service)

Example: Microsoft 365

Microsoft manages: - Infrastructure - Platform - Application

Customer manages: - Data - Users - Device security

**Exam Tip:**\
More control = More responsibility\
IaaS → Most customer responsibility\
SaaS → Least customer responsibility

------------------------------------------------------------------------

## 5️⃣ Cloud Deployment Models

### Public Cloud

-   Owned by cloud provider
-   Accessible via internet
-   Shared infrastructure
-   Example: Azure

### Private Cloud

-   Used by single organization
-   On-prem or hosted
-   More control
-   More expensive

### Hybrid Cloud

-   Combination of public + private
-   Data & apps move between them
-   Example: Azure Arc, Azure VPN

------------------------------------------------------------------------

## 6️⃣ Consumption-Based Model (Pay-As-You-Go)

You pay based on: - Compute hours used - Storage used - Network usage

### Capital Expenditure (CapEx)

-   Upfront investment
-   Buy servers
-   On-prem model

### Operational Expenditure (OpEx)

-   Monthly payment
-   Cloud model
-   No upfront cost

### Benefits of Consumption Model

-   No upfront cost
-   Pay only for usage
-   Better cost prediction
-   Stop paying when resource is deleted

------------------------------------------------------------------------

## 🔥 Quick AZ-900 Revision Points

-   Cloud offers **scalability + elasticity**
-   Shared responsibility changes across IaaS, PaaS, SaaS
-   Public vs Private vs Hybrid
-   Pay-as-you-go = Consumption model
-   Azure provides global regions
-   SLA = Service Level Agreement
