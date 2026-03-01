# 🔵 Module 1: Develop Azure Compute Solutions (25–30%)

📘 [← Back to AZ-204 Index](./index.md)

---

## 1.1 Implement Containerized Solutions

### 🐳 Azure Container Registry (ACR)

**What it is:** A managed, private Docker registry for storing and managing container images and related artefacts.

**SKUs:**

| SKU | Key Features |
|-----|-------------|
| Basic | Development/testing; lower storage & throughput limits |
| Standard | Production workloads; increased storage & throughput |
| Premium | Geo-replication, private endpoints, content trust, dedicated data endpoints, zone redundancy |

**Image naming format:** `<registry>.azurecr.io/<repository>:<tag>`

**Authentication Methods:**

| Method | Use Case |
|--------|----------|
| Admin User | Quick testing only — not recommended for production |
| Service Principal | Headless/automated scenarios |
| Managed Identity | Preferred for Azure-hosted workloads |
| Azure RBAC | Fine-grained access control |

**RBAC Roles:**

| Role | Permissions |
|------|------------|
| `AcrPull` | Pull images only |
| `AcrPush` | Push and pull images |
| `AcrDelete` | Delete images |
| `Contributor` / `Owner` | Full management |

**Common CLI Commands:**

```bash
# Create a registry
az acr create --name myRegistry --resource-group myRG --sku Standard

# Build and push image directly in ACR (no local Docker needed)
az acr build --registry myRegistry --image myapp:v1 .

# Login to ACR
az acr login --name myRegistry

# List images in registry
az acr repository list --name myRegistry

# Show tags for an image
az acr repository show-tags --name myRegistry --repository myapp
```

> 💡 **ACR Tasks** automate image builds triggered by base image updates, Git commits, or schedules — without needing a local Docker daemon.

---

### 📦 Azure Container Instances (ACI)

**What it is:** Serverless containers — fastest way to run a container in Azure. No orchestrator needed. Billed per second.

**Key Concepts:**

| Concept | Detail |
|---------|--------|
| Container Group | Top-level resource; containers share lifecycle, network, and storage |
| OS Support | Linux and Windows containers |
| Networking | Public IP, DNS name label, or private VNET injection |
| Restart Policy | `Always` (default), `Never`, `OnFailure` |
| Resource Limits | CPU (cores) and Memory (GB) specified per container |

**Container Groups:**
- Multiple containers in a group share the same IP address
- Communicate with each other via `localhost`
- Ports are exposed at the group level

```bash
# Deploy a container
az container create \
  --resource-group myRG \
  --name mycontainer \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --dns-name-label myapp-unique-label \
  --ports 80

# View logs
az container logs --resource-group myRG --name mycontainer

# Execute a command inside a running container
az container exec \
  --resource-group myRG --name mycontainer \
  --exec-command "/bin/sh"

# Show container details
az container show --resource-group myRG --name mycontainer
```

**Environment Variables:**
```bash
# Non-sensitive
--environment-variables KEY=value

# Sensitive (masked in portal/logs)
--secure-environment-variables SECRET=value
```

**Mounting Azure File Shares:**
```bash
az container create \
  --resource-group myRG \
  --name mycontainer \
  --image myimage \
  --azure-file-volume-account-name mystorageaccount \
  --azure-file-volume-account-key <key> \
  --azure-file-volume-share-name myshare \
  --azure-file-volume-mount-path /mnt/data
```

---

### 🌿 Azure Container Apps (ACA)

**What it is:** A fully managed serverless container platform built on Kubernetes and KEDA. Designed for microservices, APIs, and event-driven workloads — without managing Kubernetes.

**Core Concepts:**

| Concept | Detail |
|---------|--------|
| Environment | Shared boundary for a group of container apps (shared VNET, logging workspace) |
| Revision | An immutable snapshot of a container app configuration |
| Ingress | HTTP/HTTPS — internal (within environment) or external (public internet) |
| Dapr | Built-in Distributed Application Runtime support |
| Scaling | HTTP concurrency, CPU/memory, or KEDA event sources |

**Revision Modes:**

| Mode | Behaviour |
|------|-----------|
| Single | Each new deployment replaces the active revision |
| Multiple | Multiple revisions live simultaneously; supports traffic splitting |

**Traffic Splitting (Multiple Revision Mode):**
```bash
az containerapp ingress traffic set \
  --name myapp --resource-group myRG \
  --revision-weight myapp--rev1=80 myapp--rev2=20
```

**Scaling:**
```bash
az containerapp create \
  --name myapp \
  --resource-group myRG \
  --environment myEnv \
  --image myregistry.azurecr.io/myapp:v1 \
  --target-port 80 \
  --ingress external \
  --min-replicas 0 \    # Scale to zero = serverless
  --max-replicas 10
```

**KEDA-based Scaling Rules:**
- HTTP requests, CPU/memory utilisation
- Azure Service Bus queue depth
- Azure Event Hubs consumer lag
- Custom KEDA scalers

**Dapr Integration:** Enables service-to-service discovery, pub/sub, state management, and observability without code changes.

---

## 1.2 Implement Azure App Service Web Apps

### 🌐 App Service Plan Tiers

| Tier | Key Features |
|------|-------------|
| Free / Shared | Shared infrastructure, no SLA, no custom domain SSL |
| Basic | Dedicated VMs, manual scaling, custom domains/SSL |
| Standard | Auto-scaling, up to 5 staging slots, daily backups, Traffic Manager |
| Premium | Enhanced performance, up to 20 slots, VNET integration |
| Isolated | App Service Environment (ASE) — dedicated VNET, highest isolation |

> 💡 **Deployment Slots** and **Autoscaling** are available from the **Standard** tier and above.

**Create a Web App:**
```bash
az webapp create \
  --name myWebApp \
  --resource-group myRG \
  --plan myAppServicePlan \
  --runtime "DOTNET:8.0"
```

---

### ⚙️ Configuration

**App Settings (environment variables):**
```bash
az webapp config appsettings set \
  --name myWebApp --resource-group myRG \
  --settings MY_KEY=MY_VALUE ANOTHER_KEY=ANOTHER_VALUE
```

**Connection Strings:**
```bash
az webapp config connection-string set \
  --name myWebApp --resource-group myRG \
  --connection-string-type SQLAzure \
  --settings MyDB="Server=tcp:..."
```

**TLS / HTTPS:**
```bash
# Force HTTPS only
az webapp update --name myWebApp --resource-group myRG --https-only true

# Set minimum TLS version
az webapp config set --name myWebApp --resource-group myRG --min-tls-version 1.2
```

**Custom Domains:**
1. Verify domain ownership via CNAME or A record + TXT record
2. Add the custom domain to the app
3. Bind an SSL certificate (Managed = free; Custom = upload PFX; App Service Certificate = paid managed cert)

---

### 📊 Diagnostics and Logging

| Log Type | Description | Storage Destination |
|----------|-------------|-------------------|
| Application Logging | App-written messages via ILogger | Filesystem or Blob |
| Web Server Logging | Raw HTTP request logs (IIS format) | Filesystem or Blob |
| Detailed Error Messages | HTML pages for 400+ HTTP errors | Filesystem only |
| Failed Request Tracing | IIS FREB trace for failed requests | Filesystem only |
| Deployment Logging | Logs from Git/ZIP deployment | Filesystem only |

```bash
# Enable application logging to filesystem
az webapp log config \
  --name myWebApp --resource-group myRG \
  --application-logging filesystem \
  --level verbose

# Stream live logs
az webapp log tail --name myWebApp --resource-group myRG
```

> 💡 Filesystem logs are temporary — they reset on a restart. For long-term retention, configure Blob storage logging.

---

### 🚀 Deployment Methods

| Method | Description |
|--------|-------------|
| ZIP Deploy | Upload compressed build output via Kudu REST API |
| Local Git | Push to App Service's built-in Git remote |
| GitHub Actions | CI/CD via `.github/workflows/` |
| Azure DevOps | Release pipelines integration |
| Container Deploy | Pull from ACR or Docker Hub |
| Run From Package | Mount a ZIP directly — files are read-only (recommended) |

```bash
# ZIP deploy
az webapp deploy \
  --name myWebApp --resource-group myRG \
  --type zip --src-path ./publish.zip
```

---

### 🔀 Deployment Slots

Slots are live environments with their own hostnames:  
`myapp-staging.azurewebsites.net`

```bash
# Create a staging slot
az webapp deployment slot create \
  --name myWebApp --resource-group myRG --slot staging

# Deploy to staging (not production)
az webapp deploy \
  --name myWebApp --resource-group myRG \
  --slot staging --type zip --src-path ./publish.zip

# Swap staging → production (zero downtime)
az webapp deployment slot swap \
  --name myWebApp --resource-group myRG \
  --slot staging --target-slot production
```

**Sticky Settings:** App settings and connection strings can be marked slot-specific (not swapped). Use these for environment-specific values like connection strings.

**Swap Behaviour:**
1. Staging slot warms up
2. Routes a small amount of traffic to staging (auto-swap preview)
3. Swaps routing — staging becomes production, production becomes staging
4. Previous production is now in the staging slot (easy rollback)

---

### ⚡ Autoscaling

**Metric-based scaling rules:**

| Metric | Example Threshold |
|--------|-----------------|
| CPU Percentage | Scale out when CPU > 70% for 10 min |
| Memory Percentage | Scale out when memory > 80% |
| HTTP Queue Length | Scale out when queue > 100 |
| Data In/Out | Custom bandwidth-based rules |

**Key Settings:**
- **Cool-down period:** Prevents thrashing; minimum time between scale actions (default: 5 minutes)
- **Min instances:** Keep at least N instances always running
- **Max instances:** Hard upper limit on instance count
- **Scale-in / scale-out:** Horizontal (more VMs) — preferred for availability
- **Scale-up:** Move to a larger VM tier — vertical

---

## 1.3 Implement Azure Functions

### ⚙️ Hosting Plans

| Plan | Cold Start | Max Timeout | VNET | Best For |
|------|-----------|-------------|------|----------|
| Consumption | Yes | 10 min | No (outbound only) | Infrequent, bursty |
| Flex Consumption | Minimal | Unlimited | Yes | Predictable latency |
| Premium | No | Unlimited | Yes | No cold start, VNET |
| Dedicated (App Service) | No | Unlimited | Yes | Long-running, always-on |
| Container Apps | No | Unlimited | Yes | Containerised functions |

> 💡 `host.json` controls global runtime settings (concurrency, retry, logging). `local.settings.json` is for local development only — never deployed to Azure.

---

### 🔗 Triggers

Each function has exactly **one** trigger. The trigger defines what starts the function.

| Trigger | Description |
|---------|-------------|
| **HTTP** | RESTful APIs and webhooks |
| **Timer** | CRON-based scheduling |
| **Blob Storage** | Fires when a new blob is created/updated |
| **Queue Storage** | Processes messages from Azure Queue |
| **Service Bus** | Enterprise messaging trigger |
| **Event Grid** | Reacts to Event Grid events |
| **Event Hubs** | Processes streaming events |
| **Cosmos DB** | Reacts to Cosmos DB change feed |

**Timer CRON format:** `{second} {minute} {hour} {day} {month} {day-of-week}`
```
"0 0 9 * * 1"  →  Every Monday at 09:00:00
"0 */5 * * * *" →  Every 5 minutes
"0 0 0 1 * *"   →  1st of every month at midnight
```

---

### 🔁 Bindings

Bindings are declarative connections to external services — no SDK boilerplate needed.

| Binding | Direction | Notes |
|---------|-----------|-------|
| HTTP | In / Out | Response object |
| Blob Storage | In / Out / Trigger | Read or write blobs |
| Queue Storage | Out / Trigger | Enqueue or process messages |
| Service Bus | Out / Trigger | Enterprise messaging |
| Event Grid | Out / Trigger | Publish or react to events |
| Event Hubs | Out / Trigger | Stream events |
| Cosmos DB | In / Out / Trigger | Read/write documents, change feed |
| Table Storage | In / Out | Work with table entities |
| SignalR | Out | Push real-time updates to clients |

**C# Example — HTTP Trigger + Cosmos DB Output:**
```csharp
[Function("SaveOrder")]
[CosmosDBOutput("OrdersDB", "Orders", Connection = "CosmosConnection")]
public static Order Run(
    [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequest req)
{
    var order = JsonSerializer.Deserialize<Order>(req.Body);
    order.Id = Guid.NewGuid().ToString();
    return order; // Automatically written to Cosmos DB
}
```

---

### 🔐 HTTP Trigger Authorization Levels

| Level | Requires | Use Case |
|-------|----------|---------|
| `Anonymous` | Nothing | Public endpoints |
| `Function` | Function key in header/querystring | Default — protected APIs |
| `Admin` | Host master key | Admin-only operations |

Keys are stored in the Function App and passed via `?code=<key>` or `x-functions-key` header.

---

### 🔄 Durable Functions

Enables stateful, long-running workflows as code on top of Azure Functions.

**Patterns:**

| Pattern | Description | Example |
|---------|-------------|---------|
| Function Chaining | Sequential execution; output flows to next | Order processing pipeline |
| Fan-out / Fan-in | Parallel tasks; wait for all to finish | Parallel report generation |
| Async HTTP API | Start long job, poll for completion | Video encoding status |
| Monitoring | Polling loop with flexible timing | Check external API until ready |
| Human Interaction | Pause workflow for approval | Expense approval workflow |
| Aggregator | Stateful entity accumulates events | Running totals, counters |

**Roles:**

| Role | Description |
|------|-------------|
| **Orchestrator** | Coordinates the workflow. Must be deterministic — no I/O, no DateTime.Now, no random |
| **Activity** | Does the actual work — calls APIs, reads/writes data |
| **Entity** | Manages a stateful object (Durable Entities / Actor pattern) |
| **Client** | Starts, queries, and sends events to orchestrations |

```csharp
// Orchestrator function
[Function("ApprovalOrchestrator")]
public async Task RunOrchestrator([OrchestrationTrigger] TaskOrchestrationContext ctx)
{
    // Sequential: validate → approve → notify
    var data = ctx.GetInput<ExpenseReport>();
    await ctx.CallActivityAsync("ValidateExpense", data);

    // Wait for human approval (up to 48 hours)
    var approved = await ctx.WaitForExternalEvent<bool>("ApprovalReceived",
        TimeSpan.FromHours(48));

    if (approved)
        await ctx.CallActivityAsync("ProcessPayment", data);
    else
        await ctx.CallActivityAsync("NotifyRejection", data);
}
```

> ⚠️ **Orchestrator rules:** Never use `DateTime.Now` (use `ctx.CurrentUtcDateTime`), no random numbers, no async I/O — orchestrators replay from history and must be deterministic.

---

📘 [← Back to AZ-204 Index](./index.md)
