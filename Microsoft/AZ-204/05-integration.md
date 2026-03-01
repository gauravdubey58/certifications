# 🟣 Module 5: Connect to and Consume Azure Services (20–25%)

📘 [← Back to AZ-204 Index](./index.md)

---

## 5.1 Implement Azure API Management (APIM)

### 🌐 Core Concepts

Azure API Management provides a façade (gateway) over your backend APIs, adding security, throttling, caching, transformation, and auto-generated documentation.

**Architecture:**

| Layer | Description |
|-------|-------------|
| **API Gateway** | Processes incoming requests, applies policies, routes to backends |
| **Developer Portal** | Auto-generated, customisable documentation site for API consumers |
| **Management Plane** | Azure Portal / REST API / CLI for APIM configuration |
| **Backend** | The actual service (App Service, Function, AKS, on-premises, etc.) |

**Pricing Tiers:**

| Tier | SLA | VNET | Multi-Region | Use For |
|------|-----|------|-------------|---------|
| Developer | ❌ | External only | ❌ | Non-production, testing |
| Basic | ✅ | ❌ | ❌ | Small workloads |
| Standard | ✅ | External | ❌ | Production |
| Premium | ✅ | Internal & External | ✅ | Enterprise, high availability |
| Consumption | ✅ | ❌ | ❌ | Serverless, per-call billing |

---

### 📜 APIM Policies

Policies are XML-based rules applied to requests/responses. They're the most tested APIM topic on AZ-204.

**Policy Scopes (applied in order — outer to inner):**
```
Global → Workspace → Product → API → Operation
```

**Policy Document Structure:**
```xml
<policies>
    <inbound>     <!-- Applied to request BEFORE reaching backend -->
        <base />  <!-- Inherit policies from outer scope -->
        <!-- Custom inbound policies here -->
    </inbound>
    <backend>     <!-- Controls how request is forwarded to backend -->
        <base />
    </backend>
    <outbound>    <!-- Applied to response BEFORE returning to caller -->
        <base />
        <!-- Custom outbound policies here -->
    </outbound>
    <on-error>    <!-- Error handling -->
        <base />
    </on-error>
</policies>
```

**Essential Policies to Know:**

| Policy | Section | Description |
|--------|---------|-------------|
| `rate-limit` | Inbound | Limit calls per subscription per time window |
| `rate-limit-by-key` | Inbound | Limit by custom key (IP, header value, JWT claim) |
| `quota` | Inbound | Enforce volume/bandwidth quota per subscription (resets on schedule) |
| `ip-filter` | Inbound | Allow or deny by IP address or CIDR range |
| `validate-jwt` | Inbound | Validate a Bearer JWT — checks signature, claims, expiry |
| `cache-lookup` | Inbound | Return cached response if available |
| `cache-store` | Outbound | Store response in cache |
| `rewrite-uri` | Inbound | Rewrite request URL before sending to backend |
| `set-backend-service` | Inbound | Dynamically set which backend to call |
| `mock-response` | Inbound | Return hardcoded mock — bypasses backend |
| `cors` | Inbound | Add CORS headers |
| `set-header` | Inbound/Outbound | Add, replace, or delete a header |
| `set-query-parameter` | Inbound | Add/modify query string parameters |
| `transform-xml` | Outbound | XSLT transform on XML payload |
| `json-to-xml` / `xml-to-json` | Inbound/Outbound | Convert payload format |
| `return-response` | Any | Return a custom response immediately |
| `send-request` | Any | Make an outbound HTTP call from within a policy |
| `retry` | Backend | Retry failed backend calls with configurable conditions |
| `log-to-eventhub` | Any | Log request/response data to Event Hubs |
| `choose` | Any | Conditional policy execution (if/else logic) |

**Policy Examples:**

```xml
<!-- Rate limit: 10 calls per 60 seconds per subscription -->
<rate-limit calls="10" renewal-period="60" />

<!-- Rate limit by IP address -->
<rate-limit-by-key calls="5" renewal-period="10"
    counter-key="@(context.Request.IpAddress)" />

<!-- Validate a JWT (Entra ID) -->
<validate-jwt header-name="Authorization" failed-validation-httpcode="401"
              failed-validation-error-message="Unauthorized">
    <openid-config url="https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>my-api-audience</value>
        </claim>
    </required-claims>
</validate-jwt>

<!-- Add API key to backend request -->
<set-header name="X-Api-Key" exists-action="override">
    <value>{{backend-api-key}}</value>  <!-- Named value (secret) -->
</set-header>

<!-- Conditional logic -->
<choose>
    <when condition="@(context.Request.Headers.ContainsKey("X-Feature-Flag"))">
        <set-backend-service base-url="https://new-backend.example.com" />
    </when>
    <otherwise>
        <set-backend-service base-url="https://stable-backend.example.com" />
    </otherwise>
</choose>

<!-- Cache responses for 60 seconds, vary by query param -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
    <vary-by-query-parameter>category</vary-by-query-parameter>
</cache-lookup>
<!-- In outbound: -->
<cache-store duration="60" />
```

---

### 🔐 APIM Access Control

| Method | Header/Location | Notes |
|--------|----------------|-------|
| **Subscription Key** | `Ocp-Apim-Subscription-Key` header or `?subscription-key=` | Default access control |
| **OAuth 2.0 + Entra ID** | `Authorization: Bearer <JWT>` | Use `validate-jwt` policy |
| **Client Certificates** | Mutual TLS | `context.Request.Certificate` in policies |
| **IP Filtering** | `ip-filter` policy | Allow/deny CIDR ranges |

**Products:** Bundle APIs under a product with a single subscription key. A product can require approval before granting access.

**Named Values:** Key-value pairs (plain or secret/Key Vault-backed) referenced in policies as `{{named-value-key}}`.

---

## 5.2 Develop Event-Based Solutions

### 📢 Azure Event Grid

**Purpose:** Reactive, publish-subscribe event routing. Delivers discrete events from event sources to event handlers.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Event Source / Publisher** | Emits events — Azure services or custom apps |
| **System Topic** | Built-in topic for Azure services (Storage, ACR, App Service, etc.) |
| **Custom Topic** | Endpoint you create for your own app events |
| **Event Subscription** | Routes events from a topic to a handler, with optional filtering |
| **Event Handler** | Receives events — Functions, Logic Apps, Event Hubs, Service Bus, Webhooks |

**Event Schema:**
```json
{
  "id": "abc-123",
  "source": "/subscriptions/.../resourceGroups/myRG/providers/Microsoft.Storage/storageAccounts/mystorage",
  "subject": "/blobServices/default/containers/uploads/blobs/photo.jpg",
  "type": "Microsoft.Storage.BlobCreated",
  "time": "2026-03-01T10:30:00Z",
  "data": {
    "api": "PutBlob",
    "url": "https://mystorage.blob.core.windows.net/uploads/photo.jpg",
    "contentType": "image/jpeg",
    "contentLength": 524288
  },
  "dataVersion": "1.0",
  "specversion": "1.0"
}
```

**Event Subscriptions — Filtering:**
```bash
az eventgrid event-subscription create \
  --name mySubscription \
  --source-resource-id /subscriptions/.../storageAccounts/mystorage \
  --endpoint https://myfunction.azurewebsites.net/api/EventGridTrigger \
  --included-event-types Microsoft.Storage.BlobCreated \
  --subject-begins-with /blobServices/default/containers/images/
```

**Delivery Guarantees:**
- At-least-once delivery
- Retry with exponential back-off for up to **24 hours**
- Configurable **dead-letter queue** (Blob Storage) for undeliverable events

**Event Grid vs Event Hubs vs Service Bus:**

| | Event Grid | Event Hubs | Service Bus |
|--|-----------|-----------|------------|
| Pattern | Pub/Sub (reactive) | Streaming (time-series) | Message queuing |
| Throughput | Low–Medium | Very high (millions/sec) | Medium |
| Ordering | No | Per-partition | With sessions |
| Retention | No | 1–90 days | Up to 14 days |
| Dead-Letter | ✅ | ✅ | ✅ |
| Best For | Azure service events | IoT, logs, telemetry | Workflows, transactions |

---

### 🌊 Azure Event Hubs

**Purpose:** High-throughput, real-time event streaming. Ingests millions of events per second. Think big data pipeline / Kafka.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Event Hub** | The streaming endpoint (analogous to a Kafka topic) |
| **Partition** | Ordered log within an Event Hub; same partition = ordered delivery |
| **Consumer Group** | Independent view of the stream; one per processing application |
| **Event Retention** | Configurable 1–90 days (Standard default: 1 day; Premium: 90 days) |
| **Capture** | Auto-archive events to Azure Blob Storage or Azure Data Lake |
| **Offset** | Position in the partition stream (used for replay / checkpointing) |

> 💡 Multiple consumer groups allow the same stream to be independently processed by different applications (e.g., analytics, storage, alerting) simultaneously.

**SDK — Produce Events:**
```csharp
await using var producer = new EventHubProducerClient(connectionString, eventHubName);

// Batch for efficiency
using EventDataBatch batch = await producer.CreateBatchAsync();
batch.TryAdd(new EventData(Encoding.UTF8.GetBytes(JsonSerializer.Serialize(myEvent))));
batch.TryAdd(new EventData(Encoding.UTF8.GetBytes(JsonSerializer.Serialize(myEvent2))));

await producer.SendAsync(batch);
```

**SDK — Consume with EventProcessorClient (Recommended):**
```csharp
// BlobContainerClient stores checkpoints (progress tracking)
var storageClient = new BlobContainerClient(storageConnectionString, "checkpoints");

var processor = new EventProcessorClient(
    checkpointStore: storageClient,
    consumerGroup: "$Default",
    connectionString: eventHubConnectionString,
    eventHubName: eventHubName);

processor.ProcessEventAsync += async args =>
{
    string body = Encoding.UTF8.GetString(args.Data.Body.ToArray());
    Console.WriteLine($"Partition: {args.Partition.PartitionId} | Event: {body}");

    // Checkpoint — save progress so we don't reprocess on restart
    await args.UpdateCheckpointAsync(args.CancellationToken);
};

processor.ProcessErrorAsync += args =>
{
    Console.Error.WriteLine($"Error: {args.Exception.Message}");
    return Task.CompletedTask;
};

await processor.StartProcessingAsync();
await Task.Delay(TimeSpan.FromMinutes(5));
await processor.StopProcessingAsync();
```

---

## 5.3 Develop Message-Based Solutions

### 📨 Azure Service Bus

**Purpose:** Enterprise message broker with full reliability guarantees — delivery, ordering, transactions, and dead-lettering.

**Entities:**

| Entity | Pattern | Description |
|--------|---------|-------------|
| **Queue** | Point-to-Point | Single sender → single receiver; FIFO |
| **Topic** | Pub/Sub | One sender → multiple independent subscribers |
| **Subscription** | (used with Topic) | Each subscriber gets their own copy of messages |

**Key Features:**

| Feature | Description |
|---------|-------------|
| **Peek-lock** | Receiver locks message; must complete or abandon (default — safe) |
| **Receive-and-delete** | Message removed on receipt (faster, risk of message loss) |
| **Dead-Letter Queue (DLQ)** | Failed messages land here for investigation |
| **Sessions** | Groups related messages — guarantees FIFO within a session |
| **Scheduled messages** | Delay delivery until a future time |
| **Message deferral** | Set aside a message for later explicit retrieval |
| **Duplicate detection** | Idempotency — discard duplicate message IDs |
| **Transactions** | Atomic operations across multiple entities |

**Message Settlement:**

| Action | Method | Effect |
|--------|--------|--------|
| Done | `CompleteMessageAsync()` | Removes message from queue |
| Retry | `AbandonMessageAsync()` | Releases lock; delivery count++ |
| Dead-letter | `DeadLetterMessageAsync()` | Moves to DLQ |
| Defer | `DeferMessageAsync()` | Removes from active delivery; retrieve by sequence number |

**SDK Example:**
```csharp
await using var client = new ServiceBusClient(connectionString);

// ─── SEND ───────────────────────────────────────────────────────
var sender = client.CreateSender("my-queue");
var message = new ServiceBusMessage(JsonSerializer.Serialize(order))
{
    ContentType = "application/json",
    Subject = "NewOrder",
    MessageId = order.Id,                         // For duplicate detection
    ScheduledEnqueueTime = DateTimeOffset.UtcNow  // Or a future time for scheduled
        .AddMinutes(30)
};
await sender.SendMessageAsync(message);

// ─── RECEIVE (peek-lock — recommended) ──────────────────────────
var receiver = client.CreateReceiver("my-queue");
var received = await receiver.ReceiveMessageAsync(maxWaitTime: TimeSpan.FromSeconds(30));

if (received != null)
{
    try
    {
        var order = JsonSerializer.Deserialize<Order>(received.Body);
        await ProcessOrderAsync(order);
        await receiver.CompleteMessageAsync(received);       // ✅ Success
    }
    catch (TransientException)
    {
        await receiver.AbandonMessageAsync(received);        // 🔄 Retry
    }
    catch (PoisonMessageException)
    {
        await receiver.DeadLetterMessageAsync(received,      // ☠️ DLQ
            "PoisonMessage", "Cannot process this message type");
    }
}

// ─── TOPIC / SUBSCRIPTION ────────────────────────────────────────
var topicSender = client.CreateSender("orders-topic");
await topicSender.SendMessageAsync(new ServiceBusMessage("New Order"));

var subReceiver = client.CreateReceiver("orders-topic", "inventory-subscription");
var subMsg = await subReceiver.ReceiveMessageAsync();
await subReceiver.CompleteMessageAsync(subMsg);
```

**Subscription Filters:**

| Filter Type | Description | Performance |
|------------|-------------|-------------|
| **SQL Filter** | SQL-like expression: `color = 'red' AND quantity > 10` | Slower |
| **Correlation Filter** | Match exact property values | Faster (recommended) |
| **True Filter** | All messages (default subscription filter) | N/A |

```bash
# Add a correlation filter to a subscription
az servicebus topic subscription rule create \
  --resource-group myRG \
  --namespace-name myNamespace \
  --topic-name orders-topic \
  --subscription-name inventory-subscription \
  --name highValueOrders \
  --filter-sql-expression "OrderValue > 1000"
```

---

### 📥 Azure Queue Storage

**Purpose:** Simple, highly scalable, low-cost message queue for decoupling application components. Part of Azure Storage.

**Characteristics:**

| Feature | Value |
|---------|-------|
| Max message size | **64 KB** |
| Max queue size | Unlimited (bounded by storage account capacity) |
| Default message TTL | 7 days |
| Max message TTL | Configurable up to unlimited (`-1`) |
| Visibility timeout | Time message is hidden while being processed (default: 30 sec, max: 7 days) |

**SDK Example:**
```csharp
var queueClient = new QueueClient(connectionString, "my-queue",
    new QueueClientOptions { MessageEncoding = QueueMessageEncoding.Base64 });

await queueClient.CreateIfNotExistsAsync();

// ─── SEND ────────────────────────────────────────────────────────
await queueClient.SendMessageAsync(JsonSerializer.Serialize(myMessage));

// ─── PEEK (non-destructive look) ─────────────────────────────────
var peeked = await queueClient.PeekMessagesAsync(maxMessages: 5);

// ─── RECEIVE ─────────────────────────────────────────────────────
QueueMessage[] messages = await queueClient.ReceiveMessagesAsync(
    maxMessages: 5,
    visibilityTimeout: TimeSpan.FromSeconds(60)); // Hidden from others for 60s

foreach (var msg in messages)
{
    var item = JsonSerializer.Deserialize<MyMessage>(msg.MessageText);
    await ProcessAsync(item);
    await queueClient.DeleteMessageAsync(msg.MessageId, msg.PopReceipt); // Remove
}

// ─── UPDATE (extend visibility or change content) ─────────────────
await queueClient.UpdateMessageAsync(
    messageId: messages[0].MessageId,
    popReceipt: messages[0].PopReceipt,
    message: "updated content",
    visibilityTimeout: TimeSpan.FromSeconds(30));
```

---

## 📝 Domain 5 — Quick Comparison Summary

### Messaging Services

| Feature | Storage Queue | Service Bus Queue | Service Bus Topic |
|---------|--------------|------------------|------------------|
| Pattern | Point-to-Point | Point-to-Point | Pub/Sub |
| Max message size | 64 KB | 256 KB (Std) / 100 MB (Prem) | 256 KB (Std) / 100 MB (Prem) |
| Max retention | 7 days | 14 days | 14 days |
| Ordering | ❌ | With sessions | With sessions |
| Dead-letter queue | Manual | ✅ Built-in | ✅ Built-in |
| Transactions | ❌ | ✅ | ✅ |
| Duplicate detection | ❌ | ✅ | ✅ |
| Filters | ❌ | ❌ | ✅ SQL / Correlation |
| Cost | Very low 💰 | Medium 💰💰 | Medium 💰💰 |

### Event Services

| Feature | Event Grid | Event Hubs |
|---------|-----------|-----------|
| Pattern | Reactive Pub/Sub | Streaming |
| Max event size | 1 MB | 1 MB |
| Throughput | Low–Medium | Millions/sec |
| Ordering | No | Per-partition |
| Retention | No (transient) | 1–90 days |
| Replay | ❌ | ✅ (within retention) |
| Use case | Service events, webhooks | IoT, logs, analytics |

---

📘 [← Back to AZ-204 Index](./index.md)
