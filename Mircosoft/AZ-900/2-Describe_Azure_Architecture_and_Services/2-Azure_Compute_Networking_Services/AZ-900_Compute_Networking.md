# AZ-900 Notes -- Azure Compute & Networking Services

------------------------------------------------------------------------

## Azure Virtual Machines (VMs)

**Definition:**\
Virtual Machines provide on-demand, scalable computing resources (IaaS).

**Key Points:** - Full OS control (Windows / Linux) - Pay for compute
time - Supports custom software & legacy apps

**Use Cases:** - Lift-and-shift migrations - Custom server
configurations - Full administrative access required

**Exam Tip:**\
VM = Highest flexibility, more management

------------------------------------------------------------------------

## Azure Virtual Desktop (AVD)

**Definition:**\
Desktop and app virtualization service running in Azure.

**Key Points:** - Access desktops remotely - Centralized management -
Secure & scalable

**Use Cases:** - Remote workforce - Centralized desktop environments

------------------------------------------------------------------------

## Azure Containers

**Definition:**\
Lightweight virtualization running applications without managing full
OS.

**Benefits:** - Fast startup - Portable & scalable - Efficient resource
usage

**Azure Services:** - Azure Container Instances (ACI) - Azure Kubernetes
Service (AKS)

**Exam Tip:**\
Containers ≠ VMs (lighter & faster)

------------------------------------------------------------------------

## Azure Functions (Serverless)

**Definition:**\
Event-driven compute service where Azure manages infrastructure.

**Key Points:** - No server management - Pay per execution -
Automatically scales

**Use Cases:** - Microservices - Event processing - Background tasks

**Memory Tip:**\
Functions = Run code without servers

------------------------------------------------------------------------

## Application Hosting Options

Azure offers multiple ways to host applications:

**1. Virtual Machines** → Full control\
**2. App Service (PaaS)** → Managed web apps\
**3. Containers** → Portable workloads\
**4. Functions** → Serverless

**Exam Tip:**\
Choose based on control vs management effort

------------------------------------------------------------------------

## Azure Virtual Networking

**Definition:**\
Allows Azure resources to communicate securely.

**Core Capabilities:** - Network isolation - IP addressing - Secure
connectivity

------------------------------------------------------------------------

## Virtual Network (VNet)

**Definition:**\
Logical network boundary in Azure.

**Key Features:** - Subnets for segmentation - Private IP
communication - Connect to on-premises networks

**Memory Tip:**\
VNet = Your private network in Azure

------------------------------------------------------------------------

## Connectivity Options

**1. VPN Gateway** - Encrypted connection over internet - Site-to-site /
Point-to-site

**2. ExpressRoute** - Private dedicated connection - Higher reliability
& speed - Does not use public internet

**Exam Tip:**\
ExpressRoute = Private & predictable performance

------------------------------------------------------------------------

## Azure DNS

**Definition:**\
Domain hosting and name resolution service.

**Key Points:** - Translates domain names → IP addresses - Highly
available & scalable

------------------------------------------------------------------------

## Exam Key Takeaways

-   VMs = IaaS & full control
-   Containers = Lightweight & fast
-   Functions = Serverless compute
-   VNet = Network isolation
-   VPN = Secure internet-based connection
-   ExpressRoute = Private connection
-   DNS = Name resolution
