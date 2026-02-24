# AZ-900 Notes -- Monitoring Tools in Azure

## 1. Azure Advisor

### Purpose

Azure Advisor provides **personalized best practice recommendations** to
improve your Azure environment.

### Recommendation Categories

-   Cost optimization
-   Performance
-   Reliability
-   Security
-   Operational excellence

### What It Does

-   Identifies underutilized resources
-   Suggests resizing or shutting down VMs
-   Recommends security improvements
-   Improves high availability setup

### Exam Tip

If the question mentions **recommendations to improve cost, security, or
performance**, think **Azure Advisor**.

------------------------------------------------------------------------

## 2. Azure Service Health

### Purpose

Azure Service Health provides information about **Azure service issues
and outages**.

### What It Includes

-   Service issues (real outages)
-   Planned maintenance
-   Health advisories
-   Regional impact information

### Key Benefit

Shows how Azure platform problems affect **your specific resources**.

### Exam Tip

If the question mentions **service outage**, **regional issue**, or
**Microsoft maintenance**, answer is **Azure Service Health**.

------------------------------------------------------------------------

## 3. Azure Monitor

### Purpose

Azure Monitor collects, analyzes, and responds to monitoring data from
Azure and on-premises environments.

### What It Collects

-   Metrics (numerical performance data)
-   Logs (detailed diagnostic data)
-   Telemetry from applications

### Core Components

#### Azure Monitor Metrics

-   Real-time performance data
-   CPU, memory, disk usage
-   Lightweight and fast

#### Azure Log Analytics

-   Query and analyze log data
-   Uses Kusto Query Language (KQL)
-   Supports troubleshooting and auditing

#### Azure Monitor Alerts

-   Trigger notifications based on metrics or logs
-   Email, SMS, webhook notifications
-   Automated actions

#### Application Insights

-   Monitors application performance
-   Tracks request rates, response times, failures
-   Useful for developers

------------------------------------------------------------------------

# Key Differences (Very Important for Exam)

  Tool                   Main Purpose
  ---------------------- ----------------------------------------
  Azure Advisor          Gives improvement recommendations
  Azure Service Health   Reports Azure service outages
  Azure Monitor          Collects and analyzes performance data
  Log Analytics          Query and analyze logs
  Application Insights   Monitor application performance

------------------------------------------------------------------------

# Quick Memory Trick

-   Improve environment → Advisor\
-   Platform outage → Service Health\
-   Performance monitoring → Azure Monitor\
-   App-level monitoring → Application Insights

------------------------------------------------------------------------

# Exam Strategy Tip

Ask yourself:

-   Is Azure suggesting improvements? → Advisor\
-   Is Microsoft reporting an outage? → Service Health\
-   Is it performance data or logs? → Azure Monitor\
-   Is it application telemetry? → Application Insights
