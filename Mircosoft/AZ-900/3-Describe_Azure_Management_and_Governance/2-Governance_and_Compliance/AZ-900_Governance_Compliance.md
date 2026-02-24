# AZ-900 Notes -- Azure Governance & Compliance Tools

## 1. Microsoft Purview

### Purpose

Microsoft Purview is a **data governance, risk, and compliance**
solution.

### Key Functions

-   Data discovery and classification
-   Data cataloging and mapping
-   Data loss prevention (DLP)
-   Compliance and risk insights

### Why It Matters for AZ-900

-   Helps organizations understand **where sensitive data lives**
-   Supports regulatory and compliance requirements
-   Provides visibility across on-premises, multi-cloud, and SaaS data

### Exam Tip

If the question mentions **data governance**, **compliance**, or **data
classification**, think **Purview**.

------------------------------------------------------------------------

## 2. Azure Policy

### Purpose

Azure Policy enforces **organizational standards** and assesses
compliance.

### What It Can Do

-   Restrict resource creation (e.g., allowed regions)
-   Enforce tagging rules
-   Audit resources
-   Ensure compliance with company rules

### Important Concepts

-   **Policy Definition** → The rule
-   **Policy Assignment** → Where rule is applied
-   **Initiative** → Group of policies

### Common Use Cases

-   Only allow specific VM sizes
-   Force resource tags
-   Prevent public IP creation

### Exam Tip

If the question involves **enforcing rules** or **compliance controls**,
answer is usually **Azure Policy**.

------------------------------------------------------------------------

## 3. Resource Locks

### Purpose

Resource locks prevent **accidental deletion or modification**.

### Lock Types

1.  **CanNotDelete (Delete Lock)**
    -   Users can modify but NOT delete
2.  **ReadOnly Lock**
    -   Users cannot modify or delete

### Why Locks Are Important

-   Protect critical production resources
-   Avoid human errors

### Exam Tip

If the question mentions **prevent accidental deletion**, think
**Resource Locks**.

------------------------------------------------------------------------

## 4. Service Trust Portal

### Purpose

The Service Trust Portal provides **compliance, security, and regulatory
information** about Microsoft services.

### What You Find There

-   Audit reports
-   Compliance certifications (ISO, SOC, etc.)
-   Security documentation
-   Privacy information

### When It Is Used

-   Compliance validation
-   Security assessments
-   Regulatory checks

### Exam Tip

If the question asks **where to find compliance documents or audit
reports**, answer is **Service Trust Portal**.

------------------------------------------------------------------------

# Quick Memory Cheatsheet

  Need                               Tool
  ---------------------------------- ----------------------
  Data governance & classification   Microsoft Purview
  Enforce rules & standards          Azure Policy
  Prevent deletion/modification      Resource Locks
  Compliance & audit reports         Service Trust Portal

------------------------------------------------------------------------

# Exam Strategy Tip

AZ-900 questions are **scenario-based**.

Ask yourself:

-   Is this about **data**? → Purview\
-   Is this about **rules/restrictions**? → Azure Policy\
-   Is this about **protection from deletion**? → Locks\
-   Is this about **compliance documents**? → Service Trust Portal
