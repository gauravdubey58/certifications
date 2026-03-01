# Domain 3 – Non-Relational Data on Azure `15–20%`

> ⬅️ [Back to DP-900 Index](./index.md)

---

## 3.1 Non-Relational (NoSQL) Concepts

### What is NoSQL?
- "Not Only SQL" — databases that **don't use fixed tabular schemas**
- Designed for **scale, flexibility, and specific access patterns**
- Trade ACID compliance for **performance and horizontal scalability**
- Schema can be **dynamic** — different documents can have different fields

### NoSQL Data Models

| Model | Description | Example Use Case | Azure Service |
|-------|-------------|-----------------|---------------|
| **Key-Value** | Data stored as key → value pairs. Extremely fast lookups by key. | Session storage, caching, shopping carts | Azure Cosmos DB (Table API), Azure Cache for Redis |
| **Document** | Data stored as JSON/BSON documents. Rich querying within documents. | Product catalogs, user profiles, content management | Azure Cosmos DB (NoSQL API) |
| **Column-Family** | Data stored in column families. Great for wide sparse tables. | IoT time-series, logging, analytics at scale | Azure Cosmos DB (Cassandra API) |
| **Graph** | Data stored as nodes (entities) and edges (relationships). | Social networks, fraud detection, recommendation engines | Azure Cosmos DB (Gremlin API) |

### NoSQL vs Relational

| | Relational (SQL) | Non-Relational (NoSQL) |
|--|-----------------|----------------------|
| Schema | Fixed, predefined | Flexible, dynamic |
| Scaling | Vertical (bigger server) | Horizontal (more servers) |
| Transactions | Full ACID | Often eventual consistency |
| Query language | SQL (standardized) | Varies by database |
| Best for | Structured data, complex joins | Large-scale, flexible, fast reads |

---

## 3.2 Azure Cosmos DB

Azure Cosmos DB is Microsoft's **fully managed, globally distributed, multi-model NoSQL database**.

### Key Features
- **Global distribution** — replicate data to any Azure region with one click
- **Multiple APIs** — choose the API that matches your use case or existing code
- **Elastic scale** — scale throughput (RU/s) and storage independently
- **SLA guarantees** — 99.999% availability, <10ms reads, <15ms writes at 99th percentile
- **Multiple consistency levels** — tune consistency vs performance

### Cosmos DB APIs

| API | Data Model | Use When |
|-----|------------|----------|
| **NoSQL (Core/SQL)** | Document (JSON) | New apps, rich JSON querying |
| **MongoDB** | Document (BSON) | Migrating MongoDB apps |
| **Cassandra** | Column-family | Migrating Cassandra apps, IoT |
| **Gremlin** | Graph (nodes + edges) | Graph traversals, relationships |
| **Table** | Key-value | Migrating Azure Table Storage |
| **PostgreSQL** | Relational | Distributed PostgreSQL (Citus) |

### Consistency Levels (Strongest → Weakest)

| Level | Description | Trade-off |
|-------|-------------|-----------|
| **Strong** | All reads reflect latest write. Globally consistent. | Highest latency, lowest availability |
| **Bounded Staleness** | Reads lag behind writes by X operations or T time | Predictable lag |
| **Session** | Consistent within a single session (client) | Default; great for user-specific data |
| **Consistent Prefix** | Reads never see out-of-order writes | Good balance |
| **Eventual** | Replicas eventually converge; reads may be stale | Lowest latency, highest throughput |

> ⭐ **Exam Tip:** **Session consistency** is the default in Cosmos DB. Understand the trade-off: stronger consistency = higher latency = lower throughput.

### Throughput: Request Units (RU/s)
- Cosmos DB charges based on **Request Units per second (RU/s)**
- Reading a 1 KB document = **1 RU**
- Writes, queries, and indexing cost more RUs
- Can be provisioned per container or shared at database level
- **Autoscale** automatically scales between min and max RU/s

---

## 3.3 Azure Blob Storage

Azure Blob Storage is **object storage** for massive amounts of unstructured data.

### Blob Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Block Blob** | Stored as blocks; can upload large files in parallel | Files, documents, images, video, backups |
| **Append Blob** | Optimized for append operations | Log files, streaming data |
| **Page Blob** | Random read/write in 512-byte pages | Azure VM disks (VHDs) |

### Access Tiers

| Tier | Storage Cost | Access Cost | Latency | Best For |
|------|-------------|-------------|---------|----------|
| **Hot** | Highest | Lowest | Instant | Frequently accessed data |
| **Cool** | Lower | Higher | Instant | Infrequently accessed (≥30 days) |
| **Cold** | Lower | Higher | Instant | Rarely accessed (≥90 days) |
| **Archive** | Lowest | Highest | Hours (rehydrate first) | Long-term backups, compliance |

> ⭐ **Exam Tip:** Archive tier data must be **rehydrated** before it can be accessed. This can take hours. It cannot be read directly.

### Blob Storage Organization
```
Storage Account
└── Container (like a bucket/folder)
    └── Blob (the actual file)
```

---

## 3.4 Azure Data Lake Storage Gen2 (ADLS Gen2)

- **Combines Azure Blob Storage with a hierarchical file system**
- Designed for **big data analytics** workloads
- Supports **POSIX permissions** at file/folder level (unlike regular Blob)
- Natively integrates with: Azure Synapse, Azure Databricks, HDInsight

### Key Differences: Blob Storage vs ADLS Gen2

| Feature | Blob Storage | ADLS Gen2 |
|---------|-------------|-----------|
| Hierarchy | Flat namespace (virtual folders) | True hierarchical namespace |
| Permissions | Container-level (ACLs limited) | POSIX ACLs at file/folder level |
| Performance | Good | Optimized for analytics |
| Use case | General object storage | Analytics data lake |

> ⭐ **Exam Tip:** ADLS Gen2 is **built on top of Blob Storage** — it's a feature you enable on a storage account, not a separate service.

---

## 3.5 Azure Table Storage

- **Key-value store** — simple, cheap, scalable
- Data stored in tables with **PartitionKey** and **RowKey** as the composite key
- No joins, no foreign keys, no stored procedures
- Good for: structured data that doesn't need complex queries
- **Azure Cosmos DB Table API** is the upgraded, globally distributed version

### Table Storage Structure
```
Table
└── Entity (row)
    ├── PartitionKey (logical grouping for scale)
    ├── RowKey (unique within partition)
    └── Columns... (flexible, up to 252 properties)
```

---

## 3.6 Azure Files and Azure Queue Storage

### Azure Files
- Fully managed **cloud file shares** accessible via SMB (Windows) and NFS (Linux)
- Can be mounted as a drive on VMs or on-premises machines
- Use for: shared application settings, lift-and-shift of file servers

### Azure Queue Storage
- **Message queue** for async communication between application components
- Each message can be up to **64 KB**
- Messages kept for up to **7 days**
- Use for: decoupling microservices, task scheduling, buffering workloads

---

> ⬅️ [← Domain 2](./domain-2-relational-data.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-analytics-workloads.md)
