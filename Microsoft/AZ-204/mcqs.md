# 📝 AZ-204: Developing Solutions for Microsoft Azure — 50 Practice MCQs

> Mapped to official exam domains (updated January 2026):
> - **Domain 1:** Develop Azure Compute Solutions — 25–30%
> - **Domain 2:** Develop for Azure Storage — 15–20%
> - **Domain 3:** Implement Azure Security — 15–20%
> - **Domain 4:** Monitor and Troubleshoot Azure Solutions — 5–10%
> - **Domain 5:** Connect to and Consume Azure Services — 20–25%

📘 [← Back to AZ-204 Index](./index.md)

---

## 🔵 Domain 1: Develop Azure Compute Solutions (Q1–Q15)

---

**Q1.** You need to build and push a Docker image to Azure Container Registry without installing Docker on your local machine. Which command achieves this?

- A) `docker build -t myregistry.azurecr.io/myapp:v1 . && docker push`
- B) `az acr build --registry myRegistry --image myapp:v1 .`
- C) `az acr import --name myRegistry --source myapp:v1`
- D) `az container create --registry myRegistry --image myapp:v1`

✅ **Answer: B**  
*`az acr build` submits the build context to ACR Tasks, which builds and pushes the image entirely in the cloud. No local Docker daemon is needed. `az acr import` copies an existing image from another registry — it doesn't build.*

---

**Q2.** You are deploying a multi-container solution to Azure Container Instances. Container A runs the web frontend and Container B runs a logging sidecar. They must share a network namespace. What is the correct resource to use?

- A) Two separate Container Instance resources in the same Resource Group
- B) A Container Group containing both containers
- C) An Azure Container App with two replicas
- D) Two containers in the same App Service Plan

✅ **Answer: B**  
*A Container Group is the top-level ACI resource. Containers within the same group share the same IP address, network namespace, and lifecycle. They communicate with each other via `localhost`.*

---

**Q3.** An Azure Container App currently runs a single active revision. You need to gradually shift traffic from the current version to a new version — starting at 10% new, 90% old. Which revision mode must be configured first?

- A) Single revision mode with traffic rules
- B) Multiple revision mode
- C) Canary mode under the ingress settings
- D) Blue-green mode in the Container App environment

✅ **Answer: B**  
*Traffic splitting across revisions requires **Multiple revision mode**. In Single revision mode, each new deployment immediately replaces the active revision with no traffic splitting capability.*

---

**Q4.** A web application on Azure App Service needs to read a value that differs between its production and staging slots — for example, a different database connection string. How should this be configured?

- A) Store the connection string in `appsettings.json` in each deployment branch
- B) Mark the connection string as a **slot setting** (sticky) in App Service configuration
- C) Use a different App Service Plan for each slot
- D) Use deployment scripts to update the setting during each swap

✅ **Answer: B**  
*Slot settings (sticky settings) are not swapped when slots are exchanged. Each slot retains its own value for sticky app settings and connection strings, making them ideal for environment-specific configuration.*

---

**Q5.** An App Service web app experiences high traffic at 09:00 every Monday. You want the app to proactively scale out to 5 instances at 08:45 every Monday and scale back to 2 instances at 18:00. Which autoscale feature should you configure?

- A) Metric-based autoscale rule on CPU percentage
- B) Schedule-based autoscale rule (time-based scaling)
- C) A deployment slot swap at 08:45
- D) App Service automatic scaling with pre-warm instances

✅ **Answer: B**  
*Schedule-based (time-based) autoscale rules allow you to set specific instance counts at defined times. This proactively prepares for expected load, unlike metric-based rules which are reactive.*

---

**Q6.** You have an Azure Function that processes messages from an Azure Service Bus queue. It should automatically retry up to 3 times with a 30-second delay before dead-lettering the message. Where should this retry configuration be defined?

- A) In the function code using a `try/catch` loop
- B) In `host.json` under the Service Bus extension configuration
- C) In the Service Bus namespace settings
- D) In the function's binding attributes in `function.json`

✅ **Answer: B**  
*The Service Bus extension retry policy is configured in `host.json`. This controls how the Functions runtime handles message retries before dead-lettering, without requiring retry logic in application code.*

---

**Q7.** A Durable Function orchestrator needs to run two activity functions — `GetInventory` and `GetPricing` — at the same time, then combine their results. Which Durable Functions pattern should be used?

- A) Function Chaining
- B) Async HTTP API
- C) Fan-out / Fan-in
- D) Monitor

✅ **Answer: C**  
*Fan-out/Fan-in launches multiple activity functions in parallel using `Task.WhenAll()` and waits for all to complete before processing the aggregated results. Function Chaining runs activities sequentially.*

---

**Q8.** A Durable Function orchestrator is not producing the expected output. During debugging you discover it calls `DateTime.Now` inside the orchestrator function body. What is the problem?

- A) `DateTime.Now` is not available in Azure Functions
- B) Orchestrators replay from history, so `DateTime.Now` returns different values on each replay, breaking determinism
- C) `DateTime.Now` requires additional NuGet packages in Azure Functions
- D) This is only a problem if the function runs on the Consumption plan

✅ **Answer: B**  
*Orchestrators must be deterministic — they replay their execution from the stored history on every checkpoint. `DateTime.Now` returns different values on replay, causing inconsistent behaviour. Use `context.CurrentUtcDateTime` instead.*

---

**Q9.** Which Azure Functions hosting plan provides pre-warmed instances to eliminate cold starts while still supporting automatic scaling and VNET integration?

- A) Consumption plan
- B) Flex Consumption plan
- C) Premium plan
- D) Dedicated (App Service) plan

✅ **Answer: C**  
*The **Premium plan** maintains pre-warmed instances (eliminating cold starts), supports VNET integration, and scales automatically. The Consumption plan has cold starts; the Dedicated plan doesn't auto-scale by default.*

---

**Q10.** You want to deploy a containerised Azure Function to Azure Container Apps. Which hosting plan enables this?

- A) Consumption plan
- B) Premium plan
- C) Dedicated plan
- D) Container Apps hosting plan

✅ **Answer: D**  
*The **Container Apps hosting plan** allows deploying Azure Functions as containers, running on the Azure Container Apps infrastructure with KEDA-based scaling.*

---

**Q11.** An App Service Web App is in the **Standard** pricing tier. How many deployment slots can it have (including production)?

- A) 2
- B) 5
- C) 10
- D) 20

✅ **Answer: B**  
*The Standard tier supports up to **5 deployment slots** (including production). Premium supports up to 20. Basic and Free/Shared have no slots.*

---

**Q12.** You need to configure an HTTP-triggered Azure Function so it can only be called by requests that include a valid function-specific key. Which authorization level should be set?

- A) `Anonymous`
- B) `Function`
- C) `Admin`
- D) `System`

✅ **Answer: B**  
*`Function` authorization requires a function-specific key passed via `?code=<key>` querystring or `x-functions-key` header. `Anonymous` requires no key; `Admin` requires the host master key.*

---

**Q13.** An Azure Container App is configured with `min-replicas: 0` and a Service Bus queue-based KEDA scaling rule. What happens when there are no messages in the queue?

- A) The app maintains at least 1 replica to avoid cold starts
- B) The app scales down to zero replicas, incurring no compute cost
- C) Azure automatically sets min-replicas to 1 when KEDA is used
- D) The app throws an error because KEDA requires at least 1 replica

✅ **Answer: B**  
*With `min-replicas: 0`, Container Apps can scale to zero when there's no work. This is the serverless behaviour of Container Apps — no messages means no replicas, and therefore no compute charges.*

---

**Q14.** You are reviewing an App Service app's configuration and notice the `WEBSITE_RUN_FROM_PACKAGE` setting is set to `1`. What does this mean?

- A) The app runs from a mounted NFS share
- B) The app runs directly from a ZIP package, and the file system is read-only
- C) The app is configured to run from a Docker container
- D) Kudu is disabled for this app

✅ **Answer: B**  
*`WEBSITE_RUN_FROM_PACKAGE=1` (Run From Package) mounts the deployment ZIP as a read-only filesystem. This improves cold start performance and prevents file-level deployments from partially succeeding.*

---

**Q15.** A Container Instance needs to run a batch job once and then stop. Which restart policy should be configured?

- A) `Always`
- B) `OnFailure`
- C) `Never`
- D) `OnSuccess`

✅ **Answer: C**  
*`Never` means the container is not restarted after it exits, regardless of the exit code. This is ideal for one-shot batch jobs. `OnFailure` restarts only if the exit code is non-zero. `Always` always restarts.*

---

## 🟢 Domain 2: Develop for Azure Storage (Q16–Q25)

---

**Q16.** A Cosmos DB container uses `category` as the partition key. A query filters only on `itemName` without specifying `category`. What type of query is this, and what is its impact?

- A) In-partition query — efficient and cheap
- B) Cross-partition (fan-out) query — scans all partitions, higher RU cost
- C) Point read — lowest possible RU cost
- D) An invalid query that will return an error

✅ **Answer: B**  
*Without the partition key in the filter, Cosmos DB must scan all partitions (fan-out), which is more expensive in RUs and slower. Always include the partition key in queries where possible.*

---

**Q17.** You need the cheapest possible read operation in Azure Cosmos DB. You have both the item `id` and its `partitionKey` value. Which SDK method should you use?

- A) `container.GetItemQueryIterator<T>(query)`
- B) `container.ReadItemAsync<T>(id, partitionKey)`
- C) `container.UpsertItemAsync<T>(item, partitionKey)`
- D) `container.GetItemLinqQueryable<T>().Where(...)`

✅ **Answer: B**  
*A point read using `ReadItemAsync` with both `id` and `partitionKey` costs exactly **1 RU** for a 1 KB item and is the most efficient operation in Cosmos DB. Queries always cost more.*

---

**Q18.** Your Cosmos DB application performs financial transactions where clients must always read the most recently committed data, even across replicas. Which consistency level should be configured?

- A) Session
- B) Consistent Prefix
- C) Bounded Staleness
- D) Strong

✅ **Answer: D**  
*Strong consistency guarantees linearisable reads — every read reflects the most recent write. This is appropriate for financial applications where stale data is not acceptable, though it has the highest latency and RU cost.*

---

**Q19.** You want to trigger an Azure Function every time a new item is inserted or updated in a Cosmos DB container. Which binding should you use?

- A) Cosmos DB Input binding
- B) Cosmos DB Output binding
- C) Cosmos DB Trigger (Change Feed)
- D) Event Grid trigger pointed at Cosmos DB

✅ **Answer: C**  
*The Cosmos DB Trigger uses the **Change Feed** to detect inserts and updates. The Input binding reads data on demand; the Output binding writes data. The Change Feed trigger is the correct mechanism for reactive processing.*

---

**Q20.** A blob has been moved to the **Archive** access tier. A developer calls `DownloadAsync()` on the blob and receives an error. What must be done before the blob can be read?

- A) Copy the blob to a new container
- B) Delete and re-upload the blob to the Hot tier
- C) Rehydrate the blob by changing its tier to Hot, Cool, or Cold
- D) Generate a SAS token with Archive read permission

✅ **Answer: C**  
*Archive blobs are offline and cannot be read directly. They must be **rehydrated** — their access tier changed to Hot, Cool, or Cold — before they become readable. Standard rehydration takes up to 15 hours.*

---

**Q21.** You need to store custom searchable metadata on blobs that can be filtered and queried across an entire storage account without listing all blobs. Which feature should you use?

- A) User-defined metadata
- B) Blob system properties
- C) Blob index tags
- D) Azure Search integration

✅ **Answer: C**  
*Blob **index tags** are key-value pairs that are indexed by the storage service, enabling cross-account/cross-container queries using the `FindBlobsByTagsAsync` method. User-defined metadata is not queryable.*

---

**Q22.** A developer creates a SAS token using the storage account key with a 24-hour expiry. After the token is issued, the security team needs to revoke it immediately without rotating the storage account key. What should have been set up beforehand to allow this?

- A) A shorter expiry time on the SAS
- B) A **Stored Access Policy** linked to the SAS
- C) A **User Delegation SAS** instead of an account-key SAS
- D) Both B and C would allow immediate revocation

✅ **Answer: D**  
*A Stored Access Policy can be deleted to immediately revoke all SAS tokens linked to it. A User Delegation SAS can be revoked by revoking the Entra ID permission of the identity that issued it. Both are valid approaches.*

---

**Q23.** A storage lifecycle management policy should automatically delete blobs with the prefix `temp/` that haven't been modified for more than 7 days. Which action and condition achieves this?

- A) `tierToArchive` with `daysAfterModificationGreaterThan: 7`
- B) `delete` with `daysAfterModificationGreaterThan: 7` and `prefixMatch: ["temp/"]`
- C) `tierToCool` with `daysAfterLastAccessTimeGreaterThan: 7`
- D) `delete` with `daysAfterCreationGreaterThan: 7`

✅ **Answer: B**  
*The `delete` action with `daysAfterModificationGreaterThan: 7` removes blobs inactive for 7 days. The `prefixMatch` filter scopes the rule to only `temp/` blobs. `daysAfterCreationGreaterThan` ignores modification time.*

---

**Q24.** A Cosmos DB container is configured with **serverless** throughput mode. Which statement correctly describes a limitation of serverless mode?

- A) Serverless containers cannot use the change feed
- B) Serverless containers cannot be geo-replicated to multiple regions
- C) Serverless containers have a maximum throughput of 100 RU/s
- D) Serverless containers do not support the NoSQL API

✅ **Answer: B**  
*Serverless Cosmos DB accounts are single-region only — they cannot be configured with multi-region writes or reads. For globally distributed workloads, provisioned throughput (with autoscale) is required.*

---

**Q25.** You need to implement soft-delete capability for a Cosmos DB container so that deleted items can still be tracked via the change feed. Which approach should you use?

- A) Enable the built-in Cosmos DB delete capture feature
- B) Use a soft-delete pattern — add a `deleted: true` property and use TTL to physically remove the item later
- C) Enable geo-redundancy so deleted items are retained in secondary regions
- D) Configure the container's conflict resolution policy to retain deleted items

✅ **Answer: B**  
*Cosmos DB change feed does not capture deletes by default. The standard pattern is to mark items with a `deleted: true` flag (an update — which IS captured) and use TTL to expire them later. The change feed then picks up the "soft delete" as an update event.*

---

## 🔴 Domain 3: Implement Azure Security (Q26–Q35)

---

**Q26.** A .NET web application needs to call the Microsoft Graph API on behalf of a signed-in user. The app runs server-side. Which OAuth 2.0 flow is most appropriate?

- A) Client Credentials flow
- B) Device Code flow
- C) Authorization Code flow
- D) Implicit flow

✅ **Answer: C**  
*The **Authorization Code** flow is designed for server-side web apps acting on behalf of a signed-in user. It exchanges an authorisation code for tokens securely on the server. The Implicit flow is deprecated. Client Credentials has no user context.*

---

**Q27.** A background service (daemon) needs to access Azure resources with no user involved. Which MSAL client type and flow should be used?

- A) Public client — Authorization Code + PKCE
- B) Confidential client — Client Credentials
- C) Public client — Device Code
- D) Confidential client — On-Behalf-Of

✅ **Answer: B**  
*Daemons and services use a **confidential client** with the **Client Credentials** flow. The app authenticates as itself (using a client secret or certificate) with no user interaction. Public clients cannot securely store secrets.*

---

**Q28.** An App Service application uses a System-assigned Managed Identity to read secrets from Azure Key Vault. After deleting and recreating the App Service, the Key Vault access stops working. Why?

- A) System-assigned Managed Identities expire after 24 hours
- B) The new App Service has a different Object ID — the old RBAC assignment pointed to the deleted identity
- C) Managed Identities don't support App Service
- D) Key Vault access policies must be re-applied every 30 days

✅ **Answer: B**  
*A system-assigned Managed Identity is deleted when its resource is deleted. The new App Service gets a new identity with a different Object ID. The previous RBAC/access policy assignment referenced the old (now deleted) identity and must be recreated.*

---

**Q29.** A developer writes `var credential = new DefaultAzureCredential()` in their code. When running locally, which credential source is tried BEFORE Managed Identity?

- A) Managed Identity is always tried first regardless of environment
- B) Environment variables (`AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`, `AZURE_TENANT_ID`)
- C) Interactive browser prompt
- D) Visual Studio Code credential

✅ **Answer: B**  
*`DefaultAzureCredential` tries sources in a fixed order. **Environment variables** (priority 1) are tried before Workload Identity (2), then Managed Identity (3), then developer tool credentials (Visual Studio, VS Code, Azure CLI, etc.).*

---

**Q30.** You want to reference an Azure Key Vault secret in an App Service app setting without writing any custom code. The App Service has a System-assigned Managed Identity. Which syntax should be used for the app setting value?

- A) `{{KeyVault:MySecret}}`
- B) `$(KeyVault.MySecret)`
- C) `@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/MySecret/)`
- D) `#{KeyVault.MySecret}#`

✅ **Answer: C**  
*App Service and Functions support Key Vault references using the `@Microsoft.KeyVault(...)` syntax in app settings. The runtime resolves the value at startup using the resource's Managed Identity — no code required.*

---

**Q31.** Which Shared Access Signature type is signed with an Azure Entra ID credential rather than a storage account key, and is therefore revocable by removing the user's permission?

- A) Account SAS
- B) Service SAS
- C) User Delegation SAS
- D) Stored Access Policy SAS

✅ **Answer: C**  
*A **User Delegation SAS** is signed using an Azure AD user delegation key obtained via the `GetUserDelegationKey` API. It can be revoked by removing the signer's permissions in Entra ID, without needing to rotate storage account keys.*

---

**Q32.** An Azure Function must write to a Cosmos DB container using a Managed Identity. The function's identity has been granted the `Cosmos DB Built-in Data Contributor` role. Which code correctly initialises the Cosmos DB client without a connection string or key?

- A) `new CosmosClient(connectionString)`
- B) `new CosmosClient(accountEndpoint, new DefaultAzureCredential())`
- C) `new CosmosClient(accountEndpoint, accountKey)`
- D) `CosmosClient.CreateFromManagedIdentity(accountEndpoint)`

✅ **Answer: B**  
*When using Managed Identity (or any Entra ID credential), pass the account endpoint URI and a `TokenCredential` (such as `DefaultAzureCredential`) to the `CosmosClient` constructor — no key or connection string needed.*

---

**Q33.** An Azure App Configuration store contains feature flag `NewCheckout`. You want to enable it for only 20% of users. Which built-in feature filter should you use?

- A) `TimeWindow`
- B) `Targeting`
- C) `Percentage`
- D) `Experimental`

✅ **Answer: C**  
*The built-in **Percentage** filter enables a feature flag for a randomly selected percentage of requests. The **Targeting** filter enables it for specific users or groups. **TimeWindow** enables it during a time range.*

---

**Q34.** A security review flags that your application stores its Azure AD client secret in `appsettings.json` committed to a Git repository. What is the recommended remediation?

- A) Encrypt `appsettings.json` using AES-256 before committing
- B) Move the secret to Azure Key Vault and reference it via a Key Vault reference or Managed Identity
- C) Store the secret in an environment variable on the developer's machine
- D) Use a shorter secret rotation period of 7 days

✅ **Answer: B**  
*Secrets should never be stored in source code. The recommended approach is to store them in **Azure Key Vault** and access them via Key Vault references (for App Service) or using `DefaultAzureCredential` + Key Vault SDK.*

---

**Q35.** Microsoft Graph API requires your application to read all users in an organisation's directory — not on behalf of a signed-in user. Which permission type and consent is required?

- A) Delegated permission — user consent
- B) Delegated permission — admin consent
- C) Application permission — admin consent
- D) Application permission — user consent

✅ **Answer: C**  
*Reading all users requires `User.Read.All` as an **Application permission** (no user context — daemon scenario). Application permissions always require **admin consent** from a Global Administrator or Privileged Role Administrator.*

---

## 🟡 Domain 4: Monitor and Troubleshoot (Q36–Q40)

---

**Q36.** An Application Insights query needs to show the 95th percentile response time for all HTTP requests, grouped in 5-minute buckets, for the last 6 hours. Which KQL query is correct?

- A)
```kusto
requests | where timestamp > ago(6h)
| summarize avg(duration) by bin(timestamp, 5m)
```
- B)
```kusto
requests | where timestamp > ago(6h)
| summarize percentile(duration, 95) by bin(timestamp, 5m)
| render timechart
```
- C)
```kusto
requests | where timestamp > ago(6h)
| summarize max(duration) by bin(timestamp, 5m)
```
- D)
```kusto
requests | top 95 by duration desc
| summarize count() by bin(timestamp, 5m)
```

✅ **Answer: B**  
*`percentile(duration, 95)` calculates the P95 latency. `bin(timestamp, 5m)` groups results into 5-minute windows. `render timechart` visualises the result as a time-series chart. Option A calculates the average (not P95).*

---

**Q37.** You need to track a custom business event each time a user completes a purchase in your web application. Which Application Insights method should be used?

- A) `_telemetry.TrackRequest("Purchase", ...)`
- B) `_telemetry.TrackTrace("PurchaseCompleted")`
- C) `_telemetry.TrackEvent("PurchaseCompleted", properties, metrics)`
- D) `_telemetry.TrackDependency("Purchase", ...)`

✅ **Answer: C**  
*`TrackEvent` is designed for business/application events with optional custom properties (string key-value pairs) and measurements (numeric). `TrackTrace` is for log messages; `TrackRequest` is for incoming HTTP requests; `TrackDependency` is for outbound calls.*

---

**Q38.** An alert should fire when more than 50 exceptions occur within any 10-minute window. Which alert type should be created in Azure Monitor?

- A) Metric alert on the `exceptions/count` metric
- B) Log alert using a KQL query on the `exceptions` table
- C) Activity log alert for exception events
- D) Smart Detection alert

✅ **Answer: B**  
*A **log alert** queries Application Insights' `exceptions` table using KQL (e.g., `exceptions | where timestamp > ago(10m) | summarize count() > 50`) on a recurring evaluation schedule. Metric alerts on `exceptions/count` are also valid but log alerts offer more flexibility.*

---

**Q39.** Application Insights is not capturing any telemetry from your ASP.NET Core app running on App Service. The NuGet package is installed and `AddApplicationInsightsTelemetry()` is called. What is the most likely missing configuration?

- A) The `APPINSIGHTS_INSTRUMENTATIONKEY` or `APPLICATIONINSIGHTS_CONNECTION_STRING` app setting is not set
- B) Application Insights requires a Premium App Service plan
- C) `AddApplicationInsightsTelemetry()` must be called after `app.UseRouting()`
- D) Application Insights only works with .NET Framework apps

✅ **Answer: A**  
*Application Insights requires the connection string (or instrumentation key) to know which resource to send telemetry to. Without `APPLICATIONINSIGHTS_CONNECTION_STRING` in the app settings, the SDK has no endpoint to send data to.*

---

**Q40.** An Azure Monitor alert triggers, but the on-call engineer receives no notification. The alert is configured and the action group contains the engineer's email. What should be checked first?

- A) Whether the alert severity is set to Sev 0
- B) Whether the Action Group is linked to the alert rule
- C) Whether the engineer's email is confirmed and the action group is not suppressed (alert processing rules)
- D) Whether Azure Monitor is enabled for the subscription

✅ **Answer: C**  
*The most common causes of missing notifications are: unconfirmed email addresses in the action group, or **alert processing rules** (formerly suppression rules) that mute notifications during certain time windows. These should be checked before investigating infrastructure.*

---

## 🟣 Domain 5: Connect to Azure Services (Q41–Q50)

---

**Q41.** An APIM policy needs to limit each subscriber to 100 API calls per minute. Exceeding this limit should return HTTP 429. Which policy achieves this?

- A) `<quota calls="100" renewal-period="60" />`
- B) `<rate-limit calls="100" renewal-period="60" />`
- C) `<throttle calls="100" per="minute" />`
- D) `<ip-filter calls="100" per="60" />`

✅ **Answer: B**  
*`rate-limit` enforces a maximum number of calls per time window per subscription, returning HTTP 429 when exceeded. `quota` enforces a total volume limit over a longer period (e.g., monthly). `throttle` is not a valid APIM policy name.*

---

**Q42.** You need an APIM policy that validates the `Authorization: Bearer <token>` header against your Microsoft Entra ID tenant, and rejects requests with an invalid or expired token. Which policy should be used?

- A) `<authentication-basic />`
- B) `<check-header name="Authorization" />`
- C) `<validate-jwt header-name="Authorization">`
- D) `<set-header name="Authorization" />`

✅ **Answer: C**  
*The `validate-jwt` policy verifies JWT tokens — checking signature (via OpenID Connect discovery), expiry, issuer, audience, and required claims. It returns 401 for invalid tokens before the request reaches the backend.*

---

**Q43.** An APIM backend service is temporarily unavailable. You want APIM to retry the backend request up to 3 times with a 2-second wait between attempts. Which policy handles this?

- A) `<wait duration="2" />` inside a `<for-each>` loop
- B) `<retry condition="..." count="3" interval="2">` around the forward-request policy
- C) `<set-backend-service retry="3" />`
- D) `<on-error>` with a redirect to a fallback backend

✅ **Answer: B**  
*The `<retry>` policy wraps `<forward-request />` and retries the backend call based on a condition (e.g., HTTP 5xx responses), up to the specified count, with a configurable interval.*

---

**Q44.** A blob is uploaded to an Azure Storage container. You want an Azure Function to be triggered automatically and process the blob. A colleague suggests using either an Event Grid trigger or a Blob Storage trigger. What is the key advantage of using an **Event Grid trigger** over the standard Blob Storage trigger for this scenario?

- A) Event Grid triggers support more programming languages
- B) Event Grid triggers react in near real-time rather than polling, and scale better for large numbers of blobs
- C) Event Grid triggers can access the blob content directly without additional binding
- D) Blob Storage triggers cannot be used with Azure Functions

✅ **Answer: B**  
*The standard Blob Storage trigger uses polling, which can introduce delays (especially in Consumption plan). The Event Grid trigger reacts to `BlobCreated` events in near real-time via push notification and is more scalable for high-throughput scenarios.*

---

**Q45.** You are publishing events from a custom application to Azure Event Grid. The event payload must conform to the Event Grid schema. Which field is **required** in every event?

- A) `data`, `type`, `time`, `id`, `subject`, `source`
- B) `eventType`, `data`, `dataVersion`
- C) `id`, `eventType`, `subject`, `eventTime`, `dataVersion`, `data`
- D) `source`, `specversion`, `type`, `id`

✅ **Answer: A**  
*For the Event Grid schema, required fields are: `id`, `source` (or `topic` for legacy schema), `subject`, `type` (or `eventType`), `time` (or `eventTime`), and `data`. All must be present for the event to be accepted.*

---

**Q46.** Multiple applications need to independently process the same stream of telemetry events from Azure Event Hubs — one for real-time dashboards, one for archiving, and one for anomaly detection. How should this be achieved?

- A) Create three separate Event Hubs and publish each event three times
- B) Create three separate **Consumer Groups** on the same Event Hub
- C) Use three separate partitions and assign one consumer per partition
- D) Create one consumer and fan out the events using Azure Functions

✅ **Answer: B**  
*Each Consumer Group provides an independent cursor (offset) into the Event Hub stream. Multiple consumer groups can read the same events independently and at their own pace, without interfering with each other.*

---

**Q47.** An Event Hubs consumer crashes mid-processing. When it restarts, it should resume from where it left off rather than reprocessing all events from the beginning. Which mechanism handles this?

- A) Event Hubs automatically remembers the last processed event per consumer
- B) The `EventProcessorClient` uses **checkpointing** backed by Azure Blob Storage
- C) The consumer must store the last offset in a database manually
- D) Event Hubs replay is not possible — events are deleted after processing

✅ **Answer: B**  
*`EventProcessorClient` checkpoints the last processed offset (via `UpdateCheckpointAsync`) to an Azure Blob Storage container. On restart, it reads the checkpoint and resumes from that position, avoiding reprocessing.*

---

**Q48.** A Service Bus queue contains 500 messages. A consumer reads a message, processes it for 45 seconds, but the message lock timeout is set to 30 seconds. What happens?

- A) The message is automatically completed after 30 seconds
- B) The lock expires and the message becomes visible to other consumers, potentially causing duplicate processing
- C) The consumer's connection to Service Bus is terminated
- D) The message is automatically moved to the Dead-Letter Queue

✅ **Answer: B**  
*When a peek-lock message's lock expires before it is completed, the lock is released and the message becomes visible to other consumers again. The delivery count increments. If the max delivery count is reached, it is dead-lettered.*

---

**Q49.** A Service Bus Topic has three subscriptions: `inventory`, `shipping`, and `analytics`. A message is sent with `OrderType = "Express"`. You want only the `shipping` subscription to receive messages where `OrderType = "Express"`. Which filter type on the shipping subscription is most performant for this use case?

- A) SQL filter: `OrderType = 'Express'`
- B) Correlation filter matching `OrderType = 'Express'`
- C) True filter on all subscriptions, with client-side filtering in the consumer
- D) A separate Topic per order type

✅ **Answer: B**  
*Correlation filters match specific property values and are evaluated by the Service Bus broker using an optimised path — they are significantly faster than SQL filters for exact-match scenarios. Client-side filtering wastes bandwidth by receiving all messages.*

---

**Q50.** An application uses Azure Queue Storage. A message is received but processing fails. The developer calls no settlement method. What happens to the message after the visibility timeout expires?

- A) The message is permanently deleted
- B) The message is moved to a Dead-Letter Queue
- C) The message becomes visible again in the queue and its delivery count increments
- D) The message remains invisible indefinitely

✅ **Answer: C**  
*Azure Queue Storage has no `AbandonMessage` method. If the consumer does not delete the message before the visibility timeout expires, the message automatically reappears in the queue. Unlike Service Bus, Queue Storage has no built-in DLQ — dead-lettering must be implemented manually.*

---

## 📊 Summary by Domain

| Domain | Questions | Exam Weight |
|--------|-----------|-------------|
| 1 – Develop Azure Compute Solutions | Q1–Q15 | 25–30% |
| 2 – Develop for Azure Storage | Q16–Q25 | 15–20% |
| 3 – Implement Azure Security | Q26–Q35 | 15–20% |
| 4 – Monitor and Troubleshoot | Q36–Q40 | 5–10% |
| 5 – Connect to Azure Services | Q41–Q50 | 20–25% |
| **Total** | **50** | **100%** |

---

## 🔗 Study Resources

| Resource | Link |
|----------|------|
| Official Study Guide | [AZ-204 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-204) |
| Free Practice Assessment | [Microsoft Practice Questions](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-204/practice/assessment?assessment-type=practice&assessmentId=35) |
| Microsoft Learn Path | [Azure Developer Learning Path](https://learn.microsoft.com/en-us/credentials/certifications/azure-developer/) |
| Exam Sandbox | [aka.ms/examdemo](https://aka.ms/examdemo) |

---

📘 [← Back to AZ-204 Index](./index.md)

*Good luck with AZ-204! 🚀 Remember — the exam is scenario-based, so focus on understanding **why** not just **what**.*
