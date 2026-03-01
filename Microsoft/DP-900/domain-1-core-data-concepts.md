# Domain 1 – Describe Core Data Concepts `25–30%`

> ⬅️ [Back to DP-900 Index](./index.md)

This domain covers the **fundamental building blocks** of all data work — understand these well as they underpin everything else.

---

## 1.1 Types of Data

### Structured Data
- Organized in a **fixed schema** (rows and columns)
- Stored in relational databases (tables)
- Easy to query with SQL
- Examples: customer records, product catalogs, financial transactions

### Semi-Structured Data
- Has some structure but is **flexible/schema-less**
- Uses tags, keys, or markers to separate elements
- Examples: **JSON**, **XML**, **YAML**, **CSV**, email headers
- Stored in NoSQL databases, document stores

### Unstructured Data
- **No predefined format** or schema
- Requires processing/AI to extract meaning
- Examples: images, videos, audio files, Word documents, PDFs, social media posts
- Stored in blob/object storage

| Type | Schema | Examples | Azure Storage |
|------|--------|----------|---------------|
| Structured | Fixed | Tables, spreadsheets | Azure SQL, Azure Tables |
| Semi-structured | Flexible | JSON, XML, CSV | Azure Cosmos DB, Blob |
| Unstructured | None | Images, video, audio | Azure Blob Storage |

---

## 1.2 Data Storage Formats

### File Formats

| Format | Description | Best For |
|--------|-------------|----------|
| **CSV** | Comma-separated values, text-based | Simple tabular data export/import |
| **JSON** | JavaScript Object Notation, hierarchical | APIs, document storage, NoSQL |
| **XML** | Extensible Markup Language, tag-based | Legacy systems, configuration |
| **Parquet** | Columnar binary format, compressed | Big data analytics, data lakes |
| **Avro** | Row-based binary format with schema | Streaming, Kafka |
| **ORC** | Optimized Row Columnar, Hadoop | Hive, big data |
| **Delta** | Parquet + transaction log | Data lakehouses (Databricks, Fabric) |

> ⭐ **Exam Tip:** **Parquet** is the most commonly tested file format. It is columnar (great for analytics — reads only needed columns), compressed (smaller file size), and widely used in Azure Synapse and Databricks.

### Databases vs File Storage
- **Databases** — optimized for querying, ACID transactions, structured data
- **Data Lakes** — raw storage of any format, massive scale, cheaper per GB
- **Data Warehouses** — optimized for analytical queries on structured data

---

## 1.3 Batch vs Streaming Data

### Batch Processing
- Collects data over a period of time and **processes it all at once**
- Higher latency (results available after batch completes)
- Cost-effective for large volumes
- Examples: nightly payroll processing, monthly billing, daily sales reports

### Streaming (Real-time) Processing
- Processes data **continuously as it arrives**
- Very low latency (near real-time results)
- More complex and expensive
- Examples: fraud detection, live dashboards, IoT sensor monitoring, stock tickers

| Characteristic | Batch | Streaming |
|---------------|-------|-----------|
| Data timing | Historical, bounded | Continuous, unbounded |
| Latency | Minutes to hours | Milliseconds to seconds |
| Processing | All at once | Event by event |
| Complexity | Lower | Higher |
| Cost | Lower | Higher |
| Azure service | Azure Data Factory | Azure Stream Analytics, Azure Event Hubs |

---

## 1.4 Data Roles and Responsibilities

### Key Data Roles

| Role | Responsibilities | Common Tools |
|------|-----------------|--------------|
| **Data Engineer** | Build and maintain data pipelines, ETL/ELT processes, data infrastructure | Azure Data Factory, Azure Synapse, Databricks |
| **Data Analyst** | Query data, create reports and dashboards, interpret results | Power BI, SQL, Excel |
| **Data Scientist** | Build ML models, statistical analysis, feature engineering | Azure ML, Python, R, Notebooks |
| **Database Administrator (DBA)** | Manage database performance, security, backups, availability | Azure SQL, SSMS |
| **Business Intelligence (BI) Developer** | Design data warehouses, create semantic models | Power BI, Azure Synapse, SSAS |

> ⭐ **Exam Tip:** Know the difference between **Data Engineer** (builds pipelines/infrastructure) vs **Data Analyst** (uses data to create reports) vs **Data Scientist** (builds predictive models).

---

## 1.5 OLTP vs OLAP

### OLTP — Online Transaction Processing
- Handles **day-to-day operational transactions**
- Many small, frequent read/write operations
- Optimized for **write performance** (INSERT, UPDATE, DELETE)
- Requires **ACID** compliance
- Examples: e-commerce orders, bank transfers, booking systems

### OLAP — Online Analytical Processing
- Handles **analytical queries** on large datasets
- Few complex read-heavy queries
- Optimized for **read performance** (aggregations, GROUP BY)
- Uses **denormalized** schema (star/snowflake)
- Examples: sales trend analysis, financial reporting, BI dashboards

| Characteristic | OLTP | OLAP |
|---------------|------|------|
| Purpose | Daily transactions | Business analytics |
| Data volume | Small per operation | Very large |
| Query type | Simple INSERT/UPDATE/SELECT | Complex aggregations |
| Schema | Normalized (3NF) | Denormalized (star/snowflake) |
| Users | Many concurrent users | Few analysts |
| Response time | Milliseconds | Seconds to minutes |
| Azure service | Azure SQL Database | Azure Synapse Analytics |

### ACID Properties (OLTP)
- **A**tomicity — transaction completes fully or not at all
- **C**onsistency — database always moves from one valid state to another
- **I**solation — concurrent transactions don't interfere with each other
- **D**urability — committed transactions are permanently saved

---

## 1.6 Data Ingestion and Processing

### ETL vs ELT

| | ETL (Extract, Transform, Load) | ELT (Extract, Load, Transform) |
|--|-------------------------------|-------------------------------|
| Order | Transform before loading | Load raw first, transform in destination |
| Where transform? | Intermediate/staging server | In the data warehouse/lake |
| Best for | Structured targets, legacy DWs | Cloud data lakes, modern DWs |
| Azure tool | Azure Data Factory | Azure Synapse, Databricks |

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-relational-data.md)
