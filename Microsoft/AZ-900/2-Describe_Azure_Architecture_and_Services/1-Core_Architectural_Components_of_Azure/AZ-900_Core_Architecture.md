# AZ-900 Notes -- Core Architectural Components of Azure

------------------------------------------------------------------------

## What is Microsoft Azure

**Definition:**\
Microsoft Azure is a cloud computing platform providing on-demand
services such as compute, storage, networking, databases, analytics, and
AI over the internet.

**Key Characteristics:** - Pay-as-you-go pricing - Global
infrastructure - High availability & scalability - Supports IaaS, PaaS,
SaaS

**Core Benefits:** - No upfront hardware cost - Elastic scaling -
Built-in security & compliance

------------------------------------------------------------------------

## Get Started with Azure Accounts

To use Azure services, you need an **Azure account**.

**Important Concepts:**

**1. Azure Account** - Identity used to access Azure services - Linked
to billing & subscriptions

**2. Subscription** - Logical container for resources - Defines billing
& limits - Multiple subscriptions allowed

**3. Billing** - Charges based on usage - Can apply budgets & cost
controls

**Memory Tip:**\
Account → Subscription → Resources

------------------------------------------------------------------------

## Interacting with Azure

You can manage Azure using multiple tools:

**1. Azure Portal** - Web-based GUI - Easy for beginners & admins

**2. Azure CLI** - Command-line interface - Good for automation &
scripting

**3. Azure PowerShell** - PowerShell-based management

**4. ARM Templates / Bicep** - Infrastructure as Code (IaC)

**Exam Tip:**\
Portal = GUI \| CLI/PowerShell = Automation

------------------------------------------------------------------------

## Azure Physical Infrastructure

Azure operates through a global network of datacenters.

**Key Components:**

### Region (Azure Region)

A **region** is a geographical area containing one or more datacenters.

Examples:

-   Central India
-   East US
-   West Europe

Characteristics:

-   Physically separate from other regions
-   Connected via Microsoft's global network
-   Independent deployment boundary

**Key Point:** A region defines **where your resources are deployed**.

------------------------------------------------------------------------

### Availability Zone (AZ)

An **Availability Zone** is a physically separate datacenter within a
single Azure region.

Each zone has:

-   Independent power
-   Independent cooling
-   Independent networking

Purpose:

-   Protects against **datacenter-level failures**

Example:

A region may contain:

-   Zone 1
-   Zone 2
-   Zone 3

If one zone fails, resources in other zones remain available.

**Key Point:** Availability Zones provide **high availability within the
same region**.

------------------------------------------------------------------------

### Region Pair

A **region pair** consists of two Azure regions within the same
geography.

Purpose:

-   Disaster recovery
-   Data replication
-   Update sequencing

Benefits:

-   One region prioritized during outages
-   Updates rolled out one region at a time
-   Some services replicate automatically

Example Pairs:

-   West Europe ↔ North Europe
-   East US ↔ West US

**Key Point:** Region pairs protect against **entire region failures**.

------------------------------------------------------------------------

#### Quick Comparison

  Concept             Protects Against     Scope
  ------------------- -------------------- ---------------------
  Region              None by itself       Geographic location
  Availability Zone   Datacenter failure   Within one region
  Region Pair         Region failure       Between two regions

------------------------------------------------------------------------

#### Memory Analogy

Think of:

-   **Region** = City\
-   **Availability Zones** = Buildings in the city\
-   **Region Pair** = Backup city

Failure scenarios:

-   Building failure → Other building works (AZ)\
-   City-wide failure → Backup city works (Region Pair)

------------------------------------------------------------------------

#### Exam-Oriented Summary

-   **Region** → Deployment location\
-   **Availability Zone** → High availability inside region\
-   **Region Pair** → Disaster recovery across regions


------------------------------------------------------------------------

## Azure Management Infrastructure

Azure resources are organized using a hierarchy.

**Hierarchy Structure:**

**1. Management Groups** - Organize multiple subscriptions - Apply
policies at scale

**2. Subscriptions** - Billing & resource boundary

**3. Resource Groups** - Logical grouping of resources - Share lifecycle
& permissions

**4. Resources** - Actual services (VMs, Storage, DB)

**Memory Trick:**\
Management Group → Subscription → Resource Group → Resource

------------------------------------------------------------------------

## Creating Azure Resources

Basic deployment flow:

1.  Choose **Subscription**
2.  Select/Create **Resource Group**
3.  Pick **Region**
4.  Configure **Resource Settings**
5.  Deploy

**Why Resource Groups Matter:** - Access control (RBAC) - Cost
management - Lifecycle management

------------------------------------------------------------------------

## Exam Key Points

-   Regions improve performance & compliance
-   Availability Zones increase resiliency
-   Subscriptions control billing
-   Resource Groups organize resources
-   Multiple management tools available

------------------------------------------------------------------------

## Quick Revision Summary

-   Azure = Cloud platform
-   Account required to access services
-   Subscription = Billing container
-   Resource Group = Logical organization
-   Region & Zones = Availability & redundancy
