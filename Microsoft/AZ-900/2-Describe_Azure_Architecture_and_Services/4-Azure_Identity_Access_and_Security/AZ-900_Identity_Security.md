# AZ-900 Notes -- Azure Identity, Access, and Security

------------------------------------------------------------------------

## Azure Directory Services (Microsoft Entra ID)

**Definition:**\
Microsoft Entra ID (formerly Azure AD) is Azure's cloud-based identity
and access management service.

**Key Functions:** - User & group identities - Authentication &
authorization - Single Sign-On (SSO) - Integration with cloud &
on-premises apps

**Exam Tip:**\
Entra ID = Identity provider for Azure

------------------------------------------------------------------------

## Azure Authentication Methods

Authentication = Verifying identity.

**Common Methods:**

**1. Username & Password** - Basic but weakest alone

**2. Multi-Factor Authentication (MFA)** - Adds extra verification
layer - Very important for exam

**3. Passwordless Authentication** - Biometrics / device-based login

**4. Single Sign-On (SSO)** - One login for multiple apps

**Memory Tip:**\
MFA = Strong security default

------------------------------------------------------------------------

## Azure External Identities

Allows users outside your organization to access resources.

**Examples:** - Business partners - Customers - Vendors

**Purpose:** - B2B & B2C collaboration - Secure external access

------------------------------------------------------------------------

## Azure Conditional Access

**Definition:**\
Controls access decisions based on conditions.

**Conditions May Include:** - User location - Device compliance - Risk
level - Application being accessed

**Example:** - Require MFA when outside office network

**Exam Tip:**\
Conditional Access = If/Then access rules

------------------------------------------------------------------------

## Role-Based Access Control (RBAC)

Authorization = What actions are allowed.

**RBAC Uses:** - Roles (Reader, Contributor, Owner) - Principle of least
privilege

**Benefits:** - Granular permissions - Improved security

**Memory Tip:**\
RBAC = Who can do what

------------------------------------------------------------------------

## Zero Trust Model

**Core Principle:**\
Never trust, always verify.

**Key Ideas:** - Verify identity explicitly - Least privilege access -
Assume breach

**Exam Shortcut:**\
Trust nothing by default

------------------------------------------------------------------------

## Defense-in-Depth

Security strategy using multiple protection layers.

**Typical Layers:** 1. Physical Security 2. Identity & Access 3.
Perimeter (Network) 4. Compute 5. Application 6. Data

**Exam Tip:**\
Multiple layers = Better protection

------------------------------------------------------------------------

## Microsoft Defender for Cloud

**Definition:**\
Cloud security posture management & threat protection tool.

**Capabilities:** - Security recommendations - Vulnerability
assessment - Threat detection - Regulatory compliance insights

**Memory Tip:**\
Defender for Cloud = Security monitoring & guidance

------------------------------------------------------------------------

## Exam Key Takeaways

-   Entra ID = Identity service
-   Authentication ≠ Authorization
-   MFA strengthens security
-   Conditional Access enforces policies
-   RBAC controls permissions
-   Zero Trust = Verify everything
-   Defense-in-Depth = Layered security
-   Defender for Cloud = Security insights
