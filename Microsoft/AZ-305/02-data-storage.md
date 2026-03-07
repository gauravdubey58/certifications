# 🟢 Module 2: Design Data Storage Solutions (25–30%)

📘 [← Back to AZ-305 Index](./index.md)

---

## 2.1 Design a Data Storage Solution for Relational Data

### 🗄️ Azure SQL Options — Choosing the Right Service

| Service | Description | Best For |
|---------|-------------|---------|
| **Azure SQL Database** | Fully managed single database | New cloud-native apps; serverless option available |
| **Azure SQL Managed Instance** | Near 100% SQL Server compatibility; full PaaS | Lift-and-shift with SQL Server Agent, CLR, linked servers |
| **SQL Server on Azure VM** | Full SQL Server on IaaS | Apps requiring OS-level access, specific SQL versions |
| **Azure SQL Elastic Pool** | Shared resources for multiple databases | SaaS apps with many DBs of variable activity |
| **Azure Synapse Analytics** | Massively parallel processing for analytics | Data warehousing, large-scale analytics |

**Decision Framework:**

```
Do you need SQL Server features not available in PaaS (SQL Agent, CLR, cross-DB queries)?
├── Yes → Azure SQL Managed Instance
└── No → Do you need OS-level access or a specific SQL Server version?
          ├── Yes → SQL Server on Azure VM
          └── No → Azure SQL Database (or Elastic Pool for multi-tenant SaaS)
```

---

### ⚡ Azure SQL Database Service Tiers

| Tier / Model | Compute | Best For |
|-------------|---------|---------|
| **DTU – Basic** | Fixed | Dev/test, small apps |
| **DTU – Standard** | Fixed | Production workloads with predictable demand |
| **DTU – Premium** | Fixed | High performance, in-memory OLTP |
| **vCore – General Purpose** | Flexible | Most production workloads |
| **vCore – Business Critical** | Local SSD, 3 replicas | Mission-critical apps needing highest IOPS, readable replicas |
| **vCore – Hyperscale** | Scales independently | Very large DBs (up to 100 TB), rapid scaling |
| **Serverless** | Auto-pause/resume | Intermittent, unpredictable workloads — bill only when active |

---

### 🌍 High Availability and Geo-Redundancy for SQL

| Feature | Description | RPO/RTO |
|---------|-------------|--------|
| **Active Geo-Replication** | Up to 4 readable secondary replicas in any region | RPO < 5 sec; manual failover |
| **Auto-failover Groups** | Managed failover of one or more DBs with a single listener endpoint | RPO < 5 sec; auto or manual failover |
| **Zone Redundant Databases** | Replicas spread across Availability Zones within a region | Survives zone failure; near-zero RPO/RTO |

> 💡 **AZ-305 key:** Use **Auto-failover Groups** when applications must automatically reconnect to the new primary using a **single, stable connection string** (the failover group listener endpoint doesn't change after failover).

---

### 🔒 Azure SQL Security Design

| Layer | Control |
|-------|---------|
| **Network** | Private Endpoint (recommended) or Service Endpoint; Server Firewall rules |
| **Authentication** | Microsoft Entra ID (preferred) or SQL authentication |
| **Authorisation** | Database roles, row-level security, column-level security |
| **Encryption at rest** | Transparent Data Encryption (TDE) — enabled by default |
| **Encryption in transit** | TLS — enforced by default |
| **Data masking** | Dynamic Data Masking — hide sensitive columns from non-privileged users |
| **Auditing** | SQL Auditing → Log Analytics / Storage / Event Hub |
| **Threat protection** | Microsoft Defender for SQL — detects SQL injection, anomalous access |
| **Encryption in use** | Always Encrypted — data encrypted client-side; server never sees plaintext |

---

### 🐘 Azure Database for PostgreSQL / MySQL / MariaDB

**Azure Database for PostgreSQL — Flexible Server:**
- Fully managed; supports extensions, custom parameters
- Zone redundant HA with standby replica; auto-failover < 60 seconds
- Point-in-time restore up to 35 days

**When to choose:**
- Greenfield app choosing open-source DB → PostgreSQL Flexible Server or MySQL Flexible Server
- Existing PostgreSQL workload → Azure Database for PostgreSQL (Flexible Server preferred over Single Server — Single Server is retired)

---

## 2.2 Design Data Storage Solutions for Semi-Structured and Unstructured Data

### 🌐 Azure Cosmos DB — Design Decisions

Cosmos DB is Azure's globally distributed, multi-model NoSQL database. AZ-305 focuses on **when to choose Cosmos DB**, **which API**, and **which consistency level**.

**Choosing the Right Cosmos DB API:**

| API | Use When |
|-----|---------|
| **NoSQL (Core)** | New greenfield NoSQL app; needs full Azure-native features |
| **MongoDB** | Existing MongoDB app migrating to Azure |
| **Cassandra** | Existing Cassandra workload; wide-column data model |
| **Gremlin** | Graph data — social networks, fraud detection, recommendation engines |
| **Table** | Simple key-value; replacing Azure Table Storage with global distribution |

**Consistency Level Selection — AZ-305 Design Guide:**

| Level | When to Choose |
|-------|---------------|
| **Strong** | Financial transactions, inventory counts — must be correct |
| **Bounded Staleness** | Multi-region reads with a known acceptable lag |
| **Session** *(default)* | Web/mobile apps — read your own writes; best balance |
| **Consistent Prefix** | Display feeds, event logs — ordering matters more than recency |
| **Eventual** | Social likes, view counters — availability over accuracy |

**Partition Key Design — AZ-305 Rules:**
- High cardinality (many unique values)
- Even distribution of reads and writes
- Included in most query filters
- Cannot be changed after container creation

---

### 📦 Azure Blob Storage — Design

**Storage Account Type Selection:**

| Type | Supports | Use When |
|------|----------|---------|
| **Standard GPv2** | Blob, File, Queue, Table | General purpose — recommended default |
| **Premium Block Blob** | Block blobs only | Low-latency workloads (analytics, AI) |
| **Premium File** | Azure Files only | High-performance SMB/NFS file shares |
| **Premium Page Blob** | Page blobs only | IaaS disks (unmanaged) |

**Access Tier Strategy:**

| Tier | Cost Profile | Use For |
|------|-------------|---------|
| **Hot** | High storage, low access | Active data, frequent reads |
| **Cool** | Lower storage, higher access | Infrequent access (monthly) |
| **Cold** | Lower still, higher access | Rarely accessed (quarterly) |
| **Archive** | Cheapest storage, retrieval delay | Long-term retention, compliance |

**Lifecycle Management:** Automatically tier blobs based on last-modified or last-access time using JSON policy rules.

---

### 📁 Azure Files and Azure NetApp Files

| Service | Protocol | Use For |
|---------|---------|---------|
| **Azure Files** | SMB 3.0, NFS 4.1 | Lift-and-shift of file server workloads; UNC path replacement |
| **Azure NetApp Files** | NFS, SMB | High-performance; SAP HANA, Oracle, HPC workloads |
| **Azure File Sync** | Agent-based | Extend on-prem Windows file servers with cloud tiering |

**Azure File Sync Tiers:**
- **Cloud tiering ON:** Files not recently accessed are automatically offloaded to Azure Files; accessed via recall
- **Cloud tiering OFF:** Full sync — all files stored locally and in Azure Files

---

### 📊 Choosing the Right Storage Service — Summary Table

| Requirement | Service |
|-------------|---------|
| Structured relational data with complex queries | Azure SQL Database / Managed Instance |
| Globally distributed JSON documents | Cosmos DB (NoSQL API) |
| Object storage — images, videos, backups | Azure Blob Storage |
| Shared file system — SMB/NFS | Azure Files |
| Time-series IoT telemetry at massive scale | Azure Data Explorer |
| Analytical data warehouse | Azure Synapse Analytics |
| Graph relationships | Cosmos DB (Gremlin API) |
| Key-value with millisecond latency cache | Azure Cache for Redis |
| Message queues with ordering + DLQ | Azure Service Bus |

---

## 2.3 Design Data Integration Solutions

### 🔄 Azure Data Factory (ADF)

ADF is Azure's cloud-native ETL/ELT and data orchestration service.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Pipeline** | Logical grouping of activities forming a workflow |
| **Activity** | A step in a pipeline — Copy, Transform, Execute, Wait |
| **Dataset** | Pointer to data in a linked service |
| **Linked Service** | Connection definition — like a connection string |
| **Integration Runtime (IR)** | Compute for data movement and transformation |
| **Trigger** | Schedules pipeline runs (Schedule, Tumbling Window, Event-based) |

**Integration Runtime Types:**

| Type | Description | Use For |
|------|-------------|---------|
| **Azure IR** | Fully managed; cloud-to-cloud | Azure service-to-service data movement |
| **Self-hosted IR** | Installed on-premises or VM | On-premises to cloud data movement |
| **Azure-SSIS IR** | Runs SSIS packages | Lift-and-shift of SSIS workloads |

---

### 🌊 Azure Synapse Analytics — Design

Azure Synapse Analytics is the unified analytics platform combining data warehousing, big data, and data integration.

**Key Components:**

| Component | Description |
|-----------|-------------|
| **Synapse SQL Pool (Dedicated)** | Massively parallel processing data warehouse — best for structured analytics |
| **Serverless SQL Pool** | Query data in Azure Data Lake using T-SQL without provisioning |
| **Apache Spark Pool** | PySpark/Scala for big data transformation and ML |
| **Synapse Pipelines** | ADF-equivalent ETL built into Synapse |
| **Synapse Link** | Zero-ETL analytical link to Cosmos DB, SQL, Dataverse |

**When to choose Synapse vs Databricks vs Data Factory:**

| Scenario | Recommended Service |
|----------|-------------------|
| Data warehouse + SQL analytics + orchestration in one platform | Azure Synapse Analytics |
| Complex Spark-based data engineering, ML, streaming | Azure Databricks |
| Simple ETL/ELT orchestration between cloud services | Azure Data Factory |
| Real-time stream processing | Azure Stream Analytics or Event Hubs + Spark |

---

### ⚡ Azure Cache for Redis — Design

**Use Cases:**
- Session state storage for web apps
- Database query result caching (reduce SQL load)
- Real-time leaderboards (sorted sets)
- Pub/Sub messaging

**Tiers:**

| Tier | Description |
|------|-------------|
| **Basic** | Single node; dev/test only; no SLA |
| **Standard** | Primary/replica pair; SLA; automatic failover |
| **Premium** | Persistence, clustering, geo-replication, VNet injection |
| **Enterprise** | Redis Enterprise, active geo-replication, RediSearch |
| **Enterprise Flash** | Redis on NVMe flash — large datasets at lower cost |

---

📘 [← Back to AZ-305 Index](./index.md)
