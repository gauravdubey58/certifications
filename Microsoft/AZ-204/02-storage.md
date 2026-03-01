# 🟢 Module 2: Develop for Azure Storage (15–20%)

📘 [← Back to AZ-204 Index](./index.md)

---

## 2.1 Develop Solutions that Use Azure Cosmos DB

### 🌍 Core Concepts

Azure Cosmos DB is a globally distributed, multi-model NoSQL database with single-digit millisecond latency and a 99.999% SLA.

**Account Hierarchy:**
```
Cosmos DB Account
 └── Database
      └── Container (Collection / Table / Graph / Keyspace)
           └── Item (Document / Row / Edge / Entity)
```

**Supported APIs:**

| API | Best For |
|-----|----------|
| **NoSQL (Core SQL)** | JSON documents with SQL-like query language — default, most features |
| MongoDB | Lift-and-shift of existing MongoDB workloads |
| Cassandra | Wide-column data, Cassandra Query Language (CQL) |
| Gremlin | Graph data — nodes and edges |
| Table | Migrating from Azure Table Storage |

> 💡 The **NoSQL API** has the most Azure-native features (RBAC, change feed, multi-master, etc.). Prefer it for new apps.

---

### 💰 Request Units (RU)

A **Request Unit (RU)** is a normalised, currency-like measure of the compute resources (CPU, IOPS, memory) consumed per operation.

| Operation | Approximate RU Cost |
|-----------|-------------------|
| 1 KB point read | **1 RU** |
| 1 KB write | ~5 RUs |
| Query (varies) | 1 RU to 1000s depending on complexity & result size |

**Throughput Modes:**

| Mode | Description | Best For |
|------|-------------|----------|
| Provisioned (Manual) | Set fixed RU/s on container or database | Predictable, high-traffic workloads |
| Provisioned (Autoscale) | Scales between 10% and 100% of max RU/s | Variable traffic |
| Serverless | Pay per operation, no pre-provisioning | Intermittent or unpredictable workloads |

---

### 🔑 Partition Key

The **partition key** is the most critical design decision in Cosmos DB.

| Rule | Detail |
|------|--------|
| High cardinality | More unique values = better distribution |
| Evenly distributed | Avoid "hot" partitions — one key shouldn't get all traffic |
| Included in queries | Queries with the partition key hit one partition (in-partition = cheap) |
| Immutable | Cannot be changed after container creation |

> 💡 **Synthetic partition keys:** Concatenate multiple fields to increase cardinality (e.g., `userId_productId`).

---

### ⚖️ Consistency Levels

Cosmos DB offers 5 levels — a trade-off between consistency, availability, and performance:

| Level | Description | Use When |
|-------|-------------|----------|
| **Strong** | Always reads the latest write. Linearisable. | Financial, inventory (critical accuracy) |
| **Bounded Staleness** | Reads lag by at most K versions or T seconds | Near-real-time with defined staleness |
| **Session** *(default)* | Reads your own writes within a session | Most web/mobile apps — best balance |
| **Consistent Prefix** | No out-of-order reads; may lag behind | Ordered event feeds |
| **Eventual** | No ordering guarantee; lowest latency & cost | Social media likes, counters |

> 💡 **Session consistency** is the **default** and covers most use cases. It guarantees your own writes are always readable in the same session.

---

### 🔄 Change Feed

Cosmos DB automatically tracks **inserts and updates** to a container in order (per partition key).

**Key Facts:**
- ✅ Captures: inserts, updates
- ❌ Does NOT capture: deletes (by default — use soft-delete pattern with TTL)
- Events are ordered within a partition key
- Useful for: event sourcing, cache invalidation, real-time analytics, search indexing

**Change Feed Processor (SDK):**

Uses a **lease container** to track progress and distribute work across multiple workers.

```csharp
var processor = container
    .GetChangeFeedProcessorBuilder<MyItem>(
        processorName: "orderProcessor",
        onChangesDelegate: async (changes, cancellationToken) =>
        {
            foreach (var item in changes)
                Console.WriteLine($"Changed: {item.Id}");
        })
    .WithInstanceName("instance-1")
    .WithLeaseContainer(leaseContainer)
    .Build();

await processor.StartAsync();
```

---

### 💻 SDK Operations (NoSQL API)

```csharp
var client = new CosmosClient(connectionString);
var container = client.GetContainer("MyDatabase", "MyContainer");

// CREATE
await container.CreateItemAsync(item, new PartitionKey(item.Category));

// READ (point read — cheapest operation, 1 RU for 1 KB)
var response = await container.ReadItemAsync<MyItem>(
    id: "item-id",
    partitionKey: new PartitionKey("electronics"));
MyItem item = response.Resource;

// UPSERT (create or replace)
await container.UpsertItemAsync(item, new PartitionKey(item.Category));

// UPDATE (replace)
await container.ReplaceItemAsync(item, item.Id, new PartitionKey(item.Category));

// DELETE
await container.DeleteItemAsync<MyItem>(
    id: "item-id",
    partitionKey: new PartitionKey("electronics"));

// QUERY
var query = new QueryDefinition(
    "SELECT * FROM c WHERE c.category = @cat AND c.price < @max")
    .WithParameter("@cat", "electronics")
    .WithParameter("@max", 500);

var iterator = container.GetItemQueryIterator<MyItem>(query);
while (iterator.HasMoreResults)
{
    var page = await iterator.ReadNextAsync();
    foreach (var item in page)
        Console.WriteLine(item.Name);
}
```

---

## 2.2 Develop Solutions that Use Azure Blob Storage

### 🏗️ Storage Account Types

| Type | Supports | Use Case |
|------|----------|----------|
| Standard general-purpose v2 | Blob, File, Queue, Table | **Recommended default** |
| Premium Block Blob | Block blobs only | Low-latency blob workloads |
| Premium File Shares | Azure Files only | High-performance SMB/NFS |
| Premium Page Blobs | Page blobs only | Azure VM unmanaged disks |

**Redundancy Options (increasing durability & cost):**

| Option | Copies | Regions | Zone-Redundant |
|--------|--------|---------|---------------|
| LRS | 3 | 1 | ❌ |
| ZRS | 3 | 1 (3 zones) | ✅ |
| GRS | 6 | 2 | ❌ |
| GZRS | 6 | 2 (3 zones primary) | ✅ |

> 💡 For production, prefer **ZRS** (same region, zone-resilient) or **GZRS** (max durability).

---

### 📂 Blob Types

| Type | Best For | Notes |
|------|----------|-------|
| **Block Blob** | Text and binary files, uploads, streaming | Most common — use for all general data |
| **Append Blob** | Log files | Optimised for appending; cannot update existing blocks |
| **Page Blob** | Random-read/write access | Used for Azure VM disks; 512-byte pages |

---

### 🌡️ Access Tiers (Block Blobs)

| Tier | Storage Cost | Access Cost | Min Duration | Best For |
|------|-------------|-------------|-------------|----------|
| **Hot** | High | Low | None | Frequently accessed data |
| **Cool** | Lower | Higher | 30 days | Infrequent access |
| **Cold** | Lower | Higher | 90 days | Rarely accessed |
| **Archive** | Lowest | High + rehydration | 180 days | Long-term archival |

> ⚠️ **Archive blobs cannot be read directly.** They must be **rehydrated** to Hot, Cool, or Cold first.  
> - **Standard rehydration:** Up to 15 hours  
> - **High priority rehydration:** Under 1 hour (for objects < 10 GB) — higher cost

---

### 🏷️ Properties, Metadata, and Tags

| Feature | Set By | Queryable Across Account | Max Size |
|---------|--------|--------------------------|----------|
| System Properties | Azure | No | N/A |
| User-Defined Metadata | Developer | No | 8 KB |
| Blob Index Tags | Developer | ✅ Yes | 10 tags, 256 chars each |

```csharp
var blobClient = containerClient.GetBlobClient("myfile.txt");

// Upload with options
await blobClient.UploadAsync(stream, new BlobUploadOptions
{
    HttpHeaders = new BlobHttpHeaders
    {
        ContentType = "application/json",
        ContentEncoding = "utf-8"
    },
    Metadata = new Dictionary<string, string>
    {
        { "author", "gaurav" },
        { "version", "1.0" }
    },
    Tags = new Dictionary<string, string>
    {
        { "project", "az-204" },
        { "environment", "prod" }
    }
});

// Get system properties
BlobProperties props = await blobClient.GetPropertiesAsync();
Console.WriteLine($"Content-Type: {props.ContentType}");
Console.WriteLine($"Last-Modified: {props.LastModified}");
Console.WriteLine($"ETag: {props.ETag}");

// Update metadata (replaces all existing metadata)
await blobClient.SetMetadataAsync(new Dictionary<string, string>
{
    { "status", "processed" }
});

// Search blobs by index tags (account-wide)
string tagQuery = @"project = 'az-204' AND environment = 'prod'";
await foreach (var item in serviceClient.FindBlobsByTagsAsync(tagQuery))
    Console.WriteLine(item.BlobName);
```

---

### 🔒 Access Control

| Method | Description | Recommendation |
|--------|-------------|---------------|
| Account Keys | Full admin access to storage account | Avoid — use for emergency/admin only |
| Shared Access Signatures (SAS) | Time-limited, scoped delegated access | Use User Delegation SAS |
| Azure RBAC | Entra ID identities with role assignments | Preferred for service-to-service |
| Anonymous / Public Access | Container or blob-level read-only | Disabled by default — use only if needed |

---

### 🔑 Shared Access Signatures (SAS)

A SAS is a URI that grants limited, time-bound access to Azure Storage resources without exposing account keys.

**SAS Types:**

| Type | Signed By | Scope | Revocable? |
|------|-----------|-------|-----------|
| **Account SAS** | Account key | Multiple services | Only by rotating key |
| **Service SAS** | Account key | Single service | Via Stored Access Policy |
| **User Delegation SAS** | Entra ID token | Blob service only | Via revoking Entra ID permission |

> 💡 **Always prefer User Delegation SAS** — it's tied to an Entra ID identity, not a static key.

**SAS Query Parameters:**

| Parameter | Meaning |
|-----------|---------|
| `sv` | Storage service version |
| `st` | Start time |
| `se` | Expiry time |
| `sr` | Resource type (`b`=blob, `c`=container, `s`=service, `f`=file) |
| `sp` | Permissions (`r`=read, `w`=write, `d`=delete, `l`=list, `a`=add, `c`=create) |
| `sig` | HMAC-SHA256 signature |

**Stored Access Policy:** Attach a SAS to a named policy on a container. Revoke access by deleting the policy — without rotating the key.

```csharp
// Create user delegation key
var delegationKey = await serviceClient.GetUserDelegationKeyAsync(
    startsOn: DateTimeOffset.UtcNow,
    expiresOn: DateTimeOffset.UtcNow.AddHours(1));

// Build SAS URI
var sasBuilder = new BlobSasBuilder
{
    BlobContainerName = "mycontainer",
    BlobName = "myfile.txt",
    Resource = "b",
    StartsOn = DateTimeOffset.UtcNow,
    ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
};
sasBuilder.SetPermissions(BlobSasPermissions.Read);

var sasUri = blobClient.GenerateSasUri(sasBuilder);
```

---

### ⏱️ Lifecycle Management Policies

Automate blob tier transitions and deletions based on last-modified or last-accessed time.

```json
{
  "rules": [
    {
      "name": "manageLogs",
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["logs/", "archive/"]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToCold": { "daysAfterModificationGreaterThan": 60 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 365 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

**Filters available:**
- `blobTypes`: `blockBlob`, `appendBlob`
- `prefixMatch`: array of path prefixes
- `blobIndexMatch`: filter by index tags

---

📘 [← Back to AZ-204 Index](./index.md)
