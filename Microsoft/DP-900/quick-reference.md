# ⚡ DP-900 Quick Reference Cheat Sheet

> ⬅️ [Back to DP-900 Index](./index.md)

---

## 🗺️ Azure Data Services Decision Tree

```
What do you need?
│
├── Store & query STRUCTURED data with SQL?
│   ├── New cloud app → Azure SQL Database
│   ├── Migrate SQL Server → Azure SQL Managed Instance
│   └── Full control / IaaS → SQL Server on Azure VM
│
├── Store FLEXIBLE / NoSQL data?
│   ├── Documents (JSON) → Cosmos DB (NoSQL API)
│   ├── Key-Value pairs → Cosmos DB (Table API) or Azure Cache for Redis
│   ├── Graph data → Cosmos DB (Gremlin API)
│   ├── Cassandra workloads → Cosmos DB (Cassandra API)
│   └── Simple cheap key-value → Azure Table Storage
│
├── Store UNSTRUCTURED files / objects?
│   ├── General files → Azure Blob Storage
│   ├── Analytics data lake → Azure Data Lake Storage Gen2
│   └── Cloud file shares (SMB/NFS) → Azure Files
│
├── ANALYTICS workloads?
│   ├── Enterprise data warehouse → Azure Synapse Analytics (Dedicated Pool)
│   ├── Query data lake with SQL → Synapse Serverless SQL Pool
│   ├── Big data + ML + Spark → Azure Databricks
│   ├── All-in-one analytics → Microsoft Fabric
│   └── Open-source Hadoop/Kafka → Azure HDInsight
│
├── INGEST & MOVE data?
│   ├── ETL/ELT pipelines → Azure Data Factory
│   └── Event streaming ingestion → Azure Event Hubs / Azure IoT Hub
│
├── REAL-TIME stream processing?
│   └── Azure Stream Analytics
│
└── VISUALIZE & report data?
    └── Power BI (Desktop → Service → Mobile)
```

---

## 📊 Key Comparisons

### OLTP vs OLAP

| | OLTP | OLAP |
|--|------|------|
| Purpose | Daily transactions | Analytics & reporting |
| Operations | INSERT, UPDATE, DELETE | SELECT, aggregations |
| Schema | Normalized (3NF) | Star/Snowflake |
| Volume | Small per operation | Very large datasets |
| Azure service | Azure SQL Database | Azure Synapse Analytics |
| Example | E-commerce order system | Sales trend dashboard |

### Batch vs Streaming

| | Batch | Streaming |
|--|-------|-----------|
| Timing | Process historical data at once | Process data as it arrives |
| Latency | High (minutes–hours) | Low (milliseconds–seconds) |
| Azure service | Azure Data Factory | Azure Stream Analytics |
| Example | Nightly payroll | Real-time fraud detection |

### Storage Tiers

| Tier | Cost | Access | Use For |
|------|------|--------|---------|
| Hot | $$$ | Instant | Daily accessed files |
| Cool | $$ | Instant | Monthly accessed files |
| Cold | $ | Instant | Quarterly accessed files |
| Archive | ¢ | Hours (rehydrate) | Years-old backups |

---

## 🏷️ Every Azure Data Service at a Glance

| Service | Category | Key Point |
|---------|----------|-----------|
| Azure SQL Database | Relational PaaS | New cloud apps, fully managed |
| Azure SQL Managed Instance | Relational PaaS | Migration, near-100% SQL Server compat |
| SQL Server on VM | Relational IaaS | Full control, OS access |
| Azure DB for PostgreSQL | Relational PaaS | Open-source PostgreSQL |
| Azure DB for MySQL | Relational PaaS | Open-source MySQL |
| Azure Cosmos DB | NoSQL | Multi-model, globally distributed |
| Azure Blob Storage | Object store | Unstructured files |
| ADLS Gen2 | Data lake | Analytics-optimized blob with hierarchy |
| Azure Table Storage | Key-value | Simple, cheap NoSQL |
| Azure Files | File share | SMB/NFS cloud file shares |
| Azure Queue Storage | Messaging | Async message queue (64 KB, 7 days) |
| Azure Synapse Analytics | Analytics | Unified DW + big data |
| Azure Databricks | Big data / ML | Spark-based collaborative platform |
| Microsoft Fabric | All-in-one | Full analytics ecosystem |
| Azure Data Factory | Orchestration | ETL/ELT pipeline tool |
| Azure Stream Analytics | Real-time | SQL-based stream processing |
| Azure Event Hubs | Ingestion | High-volume event streaming |
| Azure IoT Hub | IoT | Device-to-cloud bidirectional |
| Power BI | Visualization | Reports, dashboards, insights |
| Azure HDInsight | Open-source | Hadoop, Spark, Kafka clusters |

---

## 🧠 Top Exam Topics

| Topic | Remember This |
|-------|--------------|
| Data types | Structured = tables; Semi-structured = JSON/XML; Unstructured = files/media |
| Parquet | Columnar format, compressed, best for analytics |
| ACID | Atomicity, Consistency, Isolation, Durability — OLTP requirement |
| ETL vs ELT | ETL transforms before loading; ELT loads raw then transforms |
| Azure SQL DB vs MI | DB = new apps; MI = migrate SQL Server apps |
| Cosmos DB consistency | Strong → Bounded Staleness → Session (default) → Consistent Prefix → Eventual |
| Cosmos DB APIs | NoSQL, MongoDB, Cassandra, Gremlin, Table, PostgreSQL |
| Blob tiers | Hot/Cool/Cold = instant access; Archive = rehydrate needed (hours) |
| ADLS Gen2 | Blob + hierarchical namespace + POSIX ACLs |
| Star schema | Fact table (numbers) + Dimension tables (descriptions) |
| Synapse Dedicated | Provisioned DW, pay by DWU, MPP architecture |
| Synapse Serverless | Query data lake files on-demand, pay per TB scanned |
| ADF | Pipeline orchestration for ETL/ELT |
| Stream Analytics | SQL-like queries on real-time data streams |
| Power BI Dashboard | Cloud only (Service), not Desktop |
| Power BI Import | Data copied in = fast queries |
| Power BI DirectQuery | Live queries to source = always fresh |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
