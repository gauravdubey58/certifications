# ☁️ AZ-900: Benefits of Using Cloud Services

------------------------------------------------------------------------

## 1️⃣ High Availability & Scalability

### High Availability (HA)

**Definition:** Ability of a system to remain operational with minimal
downtime.

Cloud providers achieve HA using: - Multiple datacenters - Availability
Zones - Redundancy - Automatic failover

**Exam Insight:** Hardware failure should not cause service outage.

------------------------------------------------------------------------

### Scalability

**Definition:** Ability to increase or decrease resources based on
demand.

**Types of Scaling:**

  Type                 Description
  -------------------- --------------------------------------
  Vertical Scaling     Increase CPU/RAM of existing machine
  Horizontal Scaling   Add or remove machines

**Example:** Increased website traffic → Add more VMs

------------------------------------------------------------------------

### Elasticity

**Definition:** Automatic scaling of resources based on workload.

**Example:** - Peak hours → Scale up - Low usage → Scale down

**Exam Tip:**\
Scalability = Planned adjustment\
Elasticity = Automatic adjustment

------------------------------------------------------------------------

## 2️⃣ Reliability & Predictability

### Reliability

**Definition:** Ability of a system to recover from failures and
continue functioning.

Cloud ensures reliability via: - Redundancy - Backup systems - Fault
tolerance - Region pairs

**Example:** Server failure → Workload moves elsewhere

------------------------------------------------------------------------

### Predictability

Cloud enables predictable: - Performance - Costs - Resource behavior

Azure tools supporting predictability: - Pricing Calculator - Cost
Management - Service Level Agreements (SLAs)

**Exam Tip:**\
Reliability = Recovery from failure\
Predictability = Stable cost & performance

------------------------------------------------------------------------

## 3️⃣ Security & Governance

### Security Benefits

Cloud providers offer strong security including: - Physical datacenter
security - Network protection - Encryption - DDoS protection - Identity
management

**Key Idea:** Cloud often provides stronger security than typical
on‑prem setups.

------------------------------------------------------------------------

### Governance

**Definition:** Establishing rules and controls over cloud resources.

Governance tools include: - Role-Based Access Control (RBAC) - Azure
Policy - Resource Locks - Management Groups

Governance helps with: - Compliance - Cost control - Standardization

**Exam Tip:**\
Security = Protection\
Governance = Control & compliance

------------------------------------------------------------------------

## 4️⃣ Manageability in the Cloud

### Manageability

**Definition:** Ease of managing cloud resources and services.

Cloud management options: - Azure Portal (Web UI) - Azure CLI -
PowerShell - Automation & Templates

------------------------------------------------------------------------

### Types of Manageability

#### Management of the Cloud

How you manage resources: - Deploy services - Configure settings -
Monitor resources

Tools: - Azure Portal - Azure CLI - ARM Templates / Bicep

------------------------------------------------------------------------

#### Management in the Cloud

Cloud provider manages: - Hardware maintenance - Updates & patches
(PaaS/SaaS) - Failure handling

**Exam Tip:** Cloud reduces operational overhead.

------------------------------------------------------------------------

## 🚀 Quick Revision Table

  Concept             Key Point
  ------------------- ---------------------------
  High Availability   Minimal downtime
  Scalability         Adjust resources
  Elasticity          Automatic scaling
  Reliability         Recovery from failure
  Predictability      Stable cost & performance
  Security            Threat protection
  Governance          Policies & controls
  Manageability       Easy administration
