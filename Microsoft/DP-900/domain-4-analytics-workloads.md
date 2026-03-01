# Domain 4 – Analytics Workloads on Azure `25–30%`

> ⬅️ [Back to DP-900 Index](./index.md)

---

## 4.1 Data Warehousing Concepts

### What is a Data Warehouse?
- A **centralized repository of integrated data** from multiple sources
- Optimized for **analytical queries** (OLAP), not transactional
- Data is **historical and read-heavy**
- Data goes through: Source → ETL/ELT → Data Warehouse → BI/Reports

### Schema Designs

#### Star Schema
- **Central fact table** surrounded by **dimension tables**
- Dimension tables are denormalized (data repeated)
- Fast query performance (fewer joins)

```
         DimDate
            |
DimProduct - FactSales - DimCustomer
            |
         DimStore
```

#### Snowflake Schema
- Like star, but **dimension tables are normalized** (split into sub-tables)
- More joins required, but less data redundancy
- More complex queries, less storage

### Key Concepts

| Term | Definition |
|------|-----------|
| **Fact table** | Contains measurable, quantitative data (e.g., sales amounts, quantities) |
| **Dimension table** | Contains descriptive attributes (e.g., product name, customer city, date) |
| **Grain** | The level of detail in the fact table (e.g., one row per transaction) |
| **Measure** | A numeric value in the fact table (e.g., revenue, quantity) |
| **Slowly Changing Dimension (SCD)** | Handles dimension data that changes over time |

---

## 4.2 Azure Synapse Analytics

Azure Synapse is an **enterprise analytics service** that unifies data warehousing and big data analytics.

### Key Components

| Component | Description |
|-----------|-------------|
| **Synapse SQL Pool (Dedicated)** | Cloud data warehouse; massive parallel processing (MPP); billed per DWU |
| **Synapse SQL Pool (Serverless)** | Query data lake files with SQL on-demand; pay per query (TB scanned) |
| **Synapse Spark Pool** | Apache Spark for big data processing; Python, Scala, R, .NET |
| **Synapse Pipelines** | Built-in data integration (like Azure Data Factory) |
| **Synapse Studio** | Unified web IDE for all Synapse services |
| **Synapse Link** | HTAP — query Cosmos DB or SQL DB without ETL |

### Dedicated SQL Pool Architecture
- Uses **MPP (Massively Parallel Processing)** — splits queries across compute nodes
- Data stored in **Azure Data Lake Storage** (not inside Synapse)
- **Distributions** — data is split across 60 distributions for parallelism
- Distribution strategies: **Hash**, **Round-Robin**, **Replicated**
- Scale up/down by changing **Data Warehouse Units (DWUs)**

### Distribution Types
| Type | How | Best For |
|------|-----|----------|
| **Hash** | Rows distributed by hash of a column | Large fact tables |
| **Round-Robin** | Rows distributed evenly, randomly | Staging tables, small tables |
| **Replicated** | Full copy on every compute node | Small dimension tables |

> ⭐ **Exam Tip:** Serverless SQL Pool = query files in data lake without provisioning anything. Dedicated SQL Pool = provisioned data warehouse, pay by DWU. Both are part of Azure Synapse Analytics.

---

## 4.3 Azure Databricks

- **Analytics platform** based on **Apache Spark**
- Collaborative environment with shared **notebooks** (Python, SQL, R, Scala)
- Integrates with: Azure Data Lake, Azure Synapse, Power BI, MLflow
- Used for: **big data processing**, **machine learning**, **streaming analytics**, **data engineering**
- Supports **Delta Lake** — adds ACID transactions and versioning to data lake files

### When to Use Databricks vs Synapse

| Databricks | Synapse |
|------------|---------|
| Advanced ML/AI workloads | Traditional data warehousing |
| Multi-language notebooks (Python/R) | SQL-first analytics |
| Streaming + batch in one platform | BI reporting with Power BI |
| Third-party cloud (runs on Azure) | Native Azure integration |

---

## 4.4 Microsoft Fabric

Microsoft Fabric is an **all-in-one analytics platform** that integrates:

| Experience | Description |
|-----------|-------------|
| **Data Factory** | Data integration and pipelines |
| **Synapse Data Engineering** | Spark-based data engineering |
| **Synapse Data Warehouse** | Cloud data warehousing |
| **Synapse Data Science** | ML model training and deployment |
| **Synapse Real-Time Analytics** | Streaming analytics (KQL) |
| **Power BI** | Business intelligence and reporting |
| **Data Activator** | Automated alerts and actions on data |

### OneLake
- **OneLake** is the single unified data lake at the heart of Microsoft Fabric
- All Fabric experiences share data in OneLake — no data movement needed
- Based on ADLS Gen2 with Delta Parquet format

---

## 4.5 Azure Data Factory (ADF)

- **Cloud-based data integration service** (ETL/ELT)
- Orchestrates data movement and transformation at scale
- No-code/low-code visual pipeline designer

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Pipeline** | Logical grouping of activities |
| **Activity** | A single step (Copy Data, Execute Databricks notebook, etc.) |
| **Dataset** | Reference to data in a linked service |
| **Linked Service** | Connection string to a data source |
| **Trigger** | Defines when a pipeline runs (schedule, tumbling window, event-based) |
| **Integration Runtime (IR)** | Compute infrastructure for data movement |

### Integration Runtime Types
- **Azure IR** — managed, serverless, for cloud-to-cloud
- **Self-hosted IR** — installed on-premises for on-prem data sources
- **Azure-SSIS IR** — runs SSIS packages in the cloud

---

## 4.6 Real-Time Analytics

### Azure Stream Analytics
- **Real-time data stream processing** service
- Uses **SQL-like query language** to process streams
- Reads from: **Event Hubs**, **IoT Hub**, Blob Storage
- Writes to: Power BI, Azure SQL, Blob Storage, Cosmos DB, Event Hubs

```sql
-- Example Stream Analytics Query
SELECT
    DeviceID,
    AVG(Temperature) AS AvgTemp,
    System.Timestamp AS WindowEnd
FROM IoTInput TIMESTAMP BY EventTime
GROUP BY DeviceID, TumblingWindow(minute, 5)
```

### Azure Event Hubs
- **Big data streaming platform** and event ingestion service
- Millions of events per second
- Retention: 1–90 days
- Used as the **input source** for Stream Analytics and Spark

### Azure IoT Hub
- **Bidirectional communication** between Azure and IoT devices
- Device management, device twins, direct methods
- Can feed data into Event Hubs or Stream Analytics

### Windowing Functions in Stream Analytics
| Window | Description | Use Case |
|--------|-------------|----------|
| **Tumbling** | Fixed, non-overlapping time windows | Count events per hour |
| **Hopping** | Fixed windows that overlap | Rolling 5-min avg, updated every 1 min |
| **Sliding** | Fires when an event occurs within window | Detect events within last 5 mins |
| **Session** | Groups events with no gap longer than X | User session analytics |

---

## 4.7 Data Visualization with Power BI

### Power BI Components

| Component | Description |
|-----------|-------------|
| **Power BI Desktop** | Free Windows app for building reports |
| **Power BI Service** | Cloud platform (app.powerbi.com) for sharing and collaboration |
| **Power BI Mobile** | iOS/Android app for consuming reports |
| **Power BI Report Server** | On-premises report server |
| **Power BI Embedded** | Embed reports in custom apps |

### Data Connectivity Modes

| Mode | How | Best For |
|------|-----|----------|
| **Import** | Data copied into Power BI | Fast queries, offline capability |
| **DirectQuery** | Queries sent live to source | Always-fresh data, large datasets |
| **Composite** | Mix of Import and DirectQuery | Flexibility |
| **Live Connection** | Connect to Analysis Services / Synapse | Enterprise semantic models |

### Key Power BI Concepts
- **Report** — collection of visualizations (charts, tables, maps)
- **Dashboard** — single page of pinned visuals from multiple reports
- **Dataset** — the data model underlying reports
- **Workspace** — collaborative area for sharing content
- **Gateway** — connects Power BI Service to on-premises data sources

> ⭐ **Exam Tip:** Know that **dashboards are in the Power BI Service** (cloud only), not in Desktop. Reports are created in Desktop and published to the Service.

### DAX (Data Analysis Expressions)
- Formula language for calculated columns and measures in Power BI
- Similar to Excel functions
```
TotalSales = SUM(Sales[Amount])
SalesGrowth = DIVIDE([CurrentSales] - [PreviousSales], [PreviousSales])
```

---

## 4.8 Azure HDInsight

- **Managed open-source analytics** cluster service
- Supports: **Hadoop**, **Spark**, **Hive**, **HBase**, **Kafka**, **Storm**
- Use when you need specific open-source frameworks not available in Databricks/Synapse
- Increasingly replaced by Azure Databricks and Synapse for most use cases

---

> ⬅️ [← Domain 3](./domain-3-non-relational-data.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
