# AZ-900 Notes -- Cloud Service Types

## Cloud Service Models

Azure provides three main cloud service models:

1.  **IaaS -- Infrastructure as a Service**
2.  **PaaS -- Platform as a Service**
3.  **SaaS -- Software as a Service**

Key idea: **Who manages what?**

------------------------------------------------------------------------

## IaaS (Infrastructure as a Service)

**Definition:**\
Cloud provider supplies infrastructure. You manage OS and above.

**Provider Manages:** - Datacenter - Networking - Storage - Servers /
Virtualization

**You Manage:** - Operating System - Runtime & Middleware -
Applications - Data

**Examples:** - Virtual Machines - Virtual Networks - Load Balancers

**Use Cases:** - Lift-and-shift migrations - Full OS control - Legacy
applications

**Memory Tip:**\
IaaS = Maximum control, Maximum responsibility

------------------------------------------------------------------------

## PaaS (Platform as a Service)

**Definition:**\
Provider manages infrastructure + OS + runtime. You focus on apps.

**Provider Manages:** - Infrastructure - Operating System - Runtime /
Middleware - Scaling & Availability

**You Manage:** - Application Code - Data - Configuration

**Examples:** - Azure App Service - Azure SQL Database - Azure Functions

**Use Cases:** - Web apps & APIs - Faster development - No server
management

**Memory Tip:**\
PaaS = Focus on code, Not servers

------------------------------------------------------------------------

## SaaS (Software as a Service)

**Definition:**\
Complete software delivered over the internet.

**Provider Manages:** - Infrastructure - Platform - Application -
Updates & Maintenance

**You Manage:** - Usage & Settings - Your Data

**Examples:** - Microsoft 365 - Outlook Online - OneDrive

**Use Cases:** - Ready-to-use software - No IT management -
Subscription-based tools

**Memory Tip:**\
SaaS = Least control, Least responsibility

------------------------------------------------------------------------

## Responsibility Comparison

  Layer         IaaS       PaaS       SaaS
  ------------- ---------- ---------- ----------
  Datacenter    Provider   Provider   Provider
  Servers       Provider   Provider   Provider
  OS            You        Provider   Provider
  Runtime       You        Provider   Provider
  Application   You        You        Provider
  Data          You        You        You

------------------------------------------------------------------------

## Exam Key Points

-   Moving IaaS → SaaS = Less management
-   Moving SaaS → IaaS = More control
-   PaaS is common for modern apps
-   SaaS requires no deployment or patching

------------------------------------------------------------------------

## Quick Memory Trick -- Pizza Analogy

-   On-Premises → Make pizza at home
-   IaaS → Take-and-bake pizza
-   PaaS → Pizza delivered
-   SaaS → Eat at restaurant
