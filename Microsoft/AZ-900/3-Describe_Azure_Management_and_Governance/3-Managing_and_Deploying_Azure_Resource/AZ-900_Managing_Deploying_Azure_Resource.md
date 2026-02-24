# AZ-900 Notes -- Managing & Deploying Azure Resources

## 1. Tools for Interacting with Azure

Azure provides multiple ways to manage resources.

### Azure Portal

-   Web-based graphical interface
-   No scripting knowledge required
-   Good for learning and manual management

### Azure CLI

-   Command-line tool (cross-platform)
-   Uses commands for automation & scripting
-   Works on Windows, Linux, macOS

Example idea: Create, update, delete resources via commands

### Azure PowerShell

-   PowerShell module for Azure management
-   Preferred by Windows / PowerShell users
-   Supports automation and scripting

### Azure Cloud Shell

-   Browser-based shell environment
-   No installation needed
-   Supports **Azure CLI** and **PowerShell**
-   Temporary storage, quick access

### Other Interaction Methods

-   REST API
-   SDKs (Python, .NET, Java, etc.)

### Exam Tip

If question asks **GUI** → Azure Portal\
If question asks **commands / automation** → CLI or PowerShell\
If question asks **no local setup** → Cloud Shell

------------------------------------------------------------------------

## 2. Azure Arc

### Purpose

Azure Arc extends Azure management to:

-   On-premises servers
-   Multi-cloud environments
-   Kubernetes clusters outside Azure

### Key Idea

Manage non-Azure resources **as if they were Azure resources**.

### Benefits

-   Unified management
-   Governance & policy control
-   Consistent tools across environments

### Exam Tip

If question mentions **hybrid**, **on-prem**, or **multi-cloud
management**, think **Azure Arc**.

------------------------------------------------------------------------

## 3. Azure Resource Manager (ARM)

### Purpose

ARM is the **deployment and management layer** of Azure.

All resource creation goes through ARM.

### Key Responsibilities

-   Resource deployment
-   Access control (RBAC)
-   Policy enforcement
-   Dependency handling

### Important Concept

Azure resources are organized into **resource groups**.

### Benefits

-   Consistent management layer
-   Declarative deployments
-   Idempotent operations

------------------------------------------------------------------------

## 4. ARM Templates

### What They Are

-   JSON files defining infrastructure
-   Declarative syntax (describe desired state)

### Why They Matter

-   Infrastructure as Code (IaC)
-   Repeatable deployments
-   Automation & consistency

### Key Advantages

-   Version control
-   Reusability
-   Predictable environments

### Exam Tip

If question mentions **JSON-based deployment**, answer is **ARM
Template**.

------------------------------------------------------------------------

## 5. Bicep (Important for Exams)

### What Is Bicep

-   Higher-level language for ARM
-   Simpler syntax than JSON
-   Compiles into ARM templates

### Why It Exists

-   Easier to read & write
-   Less verbose than JSON
-   Same capabilities as ARM templates

### Exam Tip

If question compares **Bicep vs ARM JSON**, remember:

Bicep = Easier authoring\
ARM Template = Underlying deployment format

------------------------------------------------------------------------

# Quick Memory Cheatsheet

  Need                              Tool
  --------------------------------- ------------------------
  Visual management (GUI)           Azure Portal
  Command-line automation           Azure CLI / PowerShell
  Browser-based shell               Cloud Shell
  Hybrid & multi-cloud management   Azure Arc
  Azure deployment engine           ARM
  JSON IaC deployments              ARM Templates
  Simplified IaC language           Bicep

------------------------------------------------------------------------

# Exam Strategy Tip

Identify the intent of the question:

-   Manual visual work → Portal
-   Automation / scripting → CLI / PowerShell
-   Hybrid resources → Azure Arc
-   Declarative deployments → ARM / Templates / Bicep
