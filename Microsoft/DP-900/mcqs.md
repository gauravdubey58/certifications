# 📝 DP-900 Practice MCQs — 50 Questions with Answers

> ⬅️ [Back to DP-900 Index](./index.md)

**Instructions:** Try to answer each question before revealing the answer. Aim for ≥ 80% to be exam-ready.

---

## Domain 1 – Core Data Concepts (Questions 1–15)

**Q1.** Which type of data is stored in a fixed schema with rows and columns?
- A) Unstructured
- B) Semi-structured
- C) Structured
- D) Raw

> ✅ **Answer: C — Structured**
> Structured data has a predefined schema organized in tables with rows and columns, typical of relational databases.

---

**Q2.** A company stores customer feedback as free-form text and images. What type of data is this?
- A) Structured
- B) Semi-structured
- C) Unstructured
- D) Relational

> ✅ **Answer: C — Unstructured**
> Free-form text and images have no predefined schema or format, making them unstructured data.

---

**Q3.** JSON and XML files are examples of which data type?
- A) Structured
- B) Semi-structured
- C) Unstructured
- D) Binary

> ✅ **Answer: B — Semi-structured**
> JSON and XML have some structure (tags and key-value pairs) but don't conform to a rigid relational schema.

---

**Q4.** Which file format is COLUMNAR and best suited for analytical workloads?
- A) CSV
- B) JSON
- C) Parquet
- D) XML

> ✅ **Answer: C — Parquet**
> Parquet is a columnar binary format that allows reading only the required columns, making it highly efficient for analytical queries.

---

**Q5.** A retail company runs its nightly sales report by processing the entire day's transactions at once. Which processing type is this?
- A) Streaming
- B) Batch
- C) Real-time
- D) Micro-batch

> ✅ **Answer: B — Batch**
> Batch processing collects data over a period and processes it all at once. Nightly processing is a classic batch pattern.

---

**Q6.** Which Azure service is best suited for processing a continuous stream of IoT sensor data in real time?
- A) Azure Data Factory
- B) Azure Synapse Analytics
- C) Azure Stream Analytics
- D) Azure Blob Storage

> ✅ **Answer: C — Azure Stream Analytics**
> Stream Analytics is designed to process continuous data streams using SQL-like queries in real time.

---

**Q7.** Who is primarily responsible for building and maintaining data pipelines in an organization?
- A) Data Analyst
- B) Data Scientist
- C) Data Engineer
- D) Database Administrator

> ✅ **Answer: C — Data Engineer**
> Data Engineers build and maintain the infrastructure and pipelines that move and transform data.

---

**Q8.** Which role typically creates Power BI reports and dashboards for business users?
- A) Data Engineer
- B) Data Analyst
- C) Data Scientist
- D) Database Administrator

> ✅ **Answer: B — Data Analyst**
> Data Analysts use tools like Power BI to query data and create reports and dashboards for business decision-making.

---

**Q9.** An e-commerce website needs to record every customer order immediately with full data integrity. Which workload type is appropriate?
- A) OLAP
- B) Batch
- C) OLTP
- D) Streaming

> ✅ **Answer: C — OLTP**
> Online Transaction Processing (OLTP) handles frequent, small operations like order recording with ACID guarantees.

---

**Q10.** Which schema is typically used in data warehouses, consisting of a central fact table and surrounding dimension tables?
- A) Normalized schema
- B) Star schema
- C) Entity-Relationship schema
- D) Flat schema

> ✅ **Answer: B — Star schema**
> A star schema has a central fact table (with measurements) connected to dimension tables (descriptive attributes).

---

**Q11.** What does the "A" stand for in ACID properties?
- A) Authentication
- B) Atomicity
- C) Authorization
- D) Availability

> ✅ **Answer: B — Atomicity**
> Atomicity means a transaction is all-or-nothing — either all operations complete or none do.

---

**Q12.** Which process loads raw data into a destination first and then transforms it?
- A) ETL
- B) ELT
- C) ETL-T
- D) Data migration

> ✅ **Answer: B — ELT (Extract, Load, Transform)**
> ELT loads raw data first into the target system (like a data lake or cloud warehouse), then transforms it using the destination's compute power.

---

**Q13.** A data warehouse stores historical data for trend analysis. Which workload type is this?
- A) OLTP
- B) OLAP
- C) Real-time processing
- D) Transactional processing

> ✅ **Answer: B — OLAP**
> Online Analytical Processing (OLAP) supports complex analytical queries on large volumes of historical data.

---

**Q14.** Which role is primarily responsible for training and deploying machine learning models?
- A) Data Analyst
- B) Data Engineer
- C) Database Administrator
- D) Data Scientist

> ✅ **Answer: D — Data Scientist**
> Data Scientists use statistical techniques and ML frameworks to build predictive models.

---

**Q15.** Which of the following is NOT an ACID property?
- A) Atomicity
- B) Concurrency
- C) Isolation
- D) Durability

> ✅ **Answer: B — Concurrency**
> ACID stands for Atomicity, Consistency, Isolation, Durability. Concurrency is not part of ACID.

---

## Domain 2 – Relational Data on Azure (Questions 16–28)

**Q16.** You need to migrate an existing SQL Server application to Azure with minimal code changes and full SQL Server feature compatibility. Which service should you use?
- A) Azure SQL Database
- B) Azure SQL Managed Instance
- C) SQL Server on Azure VM
- D) Azure Database for PostgreSQL

> ✅ **Answer: B — Azure SQL Managed Instance**
> SQL Managed Instance provides near 100% SQL Server compatibility, making it ideal for migrations with minimal changes.

---

**Q17.** Which Azure SQL service gives you the most control including OS-level access?
- A) Azure SQL Database
- B) Azure SQL Managed Instance
- C) SQL Server on Azure Virtual Machine
- D) Azure Database for MySQL

> ✅ **Answer: C — SQL Server on Azure Virtual Machine**
> SQL on VM is IaaS — you manage the OS, SQL Server installation, and configuration.

---

**Q18.** What type of JOIN returns only rows where there is a match in BOTH tables?
- A) LEFT JOIN
- B) RIGHT JOIN
- C) FULL OUTER JOIN
- D) INNER JOIN

> ✅ **Answer: D — INNER JOIN**
> An INNER JOIN returns only the rows with matching values in both tables.

---

**Q19.** Which SQL command is used to remove all rows from a table while keeping its structure?
- A) DROP TABLE
- B) DELETE FROM table
- C) TRUNCATE TABLE
- D) REMOVE TABLE

> ✅ **Answer: C — TRUNCATE TABLE** (also acceptable: B)
> TRUNCATE removes all rows efficiently. DELETE without WHERE also removes all rows but is logged row-by-row. DROP removes the table entirely.

---

**Q20.** A column that uniquely identifies each row in a table is called a:
- A) Foreign Key
- B) Index
- C) Primary Key
- D) Unique Constraint

> ✅ **Answer: C — Primary Key**
> A primary key uniquely identifies each row and cannot be NULL.

---

**Q21.** Which SQL clause is used to filter results AFTER a GROUP BY aggregation?
- A) WHERE
- B) HAVING
- C) FILTER
- D) ORDER BY

> ✅ **Answer: B — HAVING**
> HAVING filters aggregated results. WHERE filters rows before aggregation. Example: `HAVING COUNT(*) > 5`.

---

**Q22.** What is the purpose of a VIEW in a relational database?
- A) Store a physical copy of query results
- B) Create a virtual table based on a SELECT query
- C) Improve write performance
- D) Automatically index a table

> ✅ **Answer: B — Create a virtual table based on a SELECT query**
> A view is a stored query. It doesn't store data itself; it presents data from underlying tables.

---

**Q23.** Which Azure service provides a fully managed MySQL database?
- A) Azure SQL Database
- B) Azure SQL Managed Instance
- C) Azure Database for MySQL
- D) Azure Database for MariaDB

> ✅ **Answer: C — Azure Database for MySQL**
> Azure Database for MySQL is the managed offering for the MySQL open-source database engine.

---

**Q24.** Which type of index determines the physical sort order of data in a table?
- A) Non-clustered index
- B) Clustered index
- C) Composite index
- D) Unique index

> ✅ **Answer: B — Clustered index**
> A clustered index determines the physical storage order of table rows. Each table can have only one.

---

**Q25.** The process of organizing database tables to reduce redundancy is called:
- A) Indexing
- B) Normalization
- C) Partitioning
- D) Sharding

> ✅ **Answer: B — Normalization**
> Normalization organizes tables to minimize data duplication and dependency through normal forms (1NF, 2NF, 3NF).

---

**Q26.** Which DDL command is used to modify the structure of an existing table?
- A) UPDATE
- B) MODIFY
- C) ALTER
- D) CHANGE

> ✅ **Answer: C — ALTER**
> `ALTER TABLE` is the DDL command for modifying table structure (add/remove/modify columns).

---

**Q27.** A company wants a managed SQL database for a brand-new cloud application. The cheapest and simplest option is:
- A) SQL Server on Azure VM
- B) Azure SQL Managed Instance
- C) Azure SQL Database
- D) Azure Database for MariaDB

> ✅ **Answer: C — Azure SQL Database**
> Azure SQL Database is the simplest, most cost-effective fully managed option for new cloud applications.

---

**Q28.** Which of the following is NOT a feature of Azure SQL Database?
- A) Automatic backups
- B) High availability
- C) Full OS access
- D) Geo-replication

> ✅ **Answer: C — Full OS access**
> Azure SQL Database is PaaS — you don't have OS-level access. OS management is handled by Microsoft.

---

## Domain 3 – Non-Relational Data on Azure (Questions 29–38)

**Q29.** Which Cosmos DB API should you use if you are migrating an existing MongoDB application?
- A) NoSQL API
- B) Cassandra API
- C) MongoDB API
- D) Gremlin API

> ✅ **Answer: C — MongoDB API**
> The Cosmos DB for MongoDB API is wire-compatible with MongoDB, allowing existing applications to work with minimal changes.

---

**Q30.** Which Cosmos DB consistency level is the default?
- A) Strong
- B) Eventual
- C) Session
- D) Bounded Staleness

> ✅ **Answer: C — Session**
> Session consistency is the default in Cosmos DB. It guarantees consistency within a single client session.

---

**Q31.** You need the strongest consistency in Cosmos DB, where all reads reflect the latest write globally. Which level should you choose?
- A) Eventual
- B) Session
- C) Consistent Prefix
- D) Strong

> ✅ **Answer: D — Strong**
> Strong consistency ensures all reads see the most recent committed write, globally, but has the highest latency.

---

**Q32.** Which Cosmos DB API is best for social network applications that need to traverse complex relationships?
- A) NoSQL API
- B) Cassandra API
- C) Table API
- D) Gremlin API

> ✅ **Answer: D — Gremlin API**
> The Gremlin API is Cosmos DB's graph database offering, ideal for relationship-heavy data like social networks.

---

**Q33.** An application requires long-term backup storage that is accessed very rarely and needs the cheapest possible storage cost. Which Azure Blob tier should be used?
- A) Hot
- B) Cool
- C) Cold
- D) Archive

> ✅ **Answer: D — Archive**
> Archive tier has the lowest storage cost but requires rehydration (which can take hours) before data can be accessed.

---

**Q34.** Which Azure Blob Storage type is optimized for virtual machine disk storage (VHDs)?
- A) Block Blob
- B) Append Blob
- C) Page Blob
- D) File Blob

> ✅ **Answer: C — Page Blob**
> Page Blobs support random read/write in 512-byte pages, making them ideal for VM disk storage.

---

**Q35.** What is the key difference between Azure Blob Storage and Azure Data Lake Storage Gen2?
- A) ADLS Gen2 is cheaper than Blob Storage
- B) ADLS Gen2 adds a hierarchical namespace and POSIX ACLs on top of Blob Storage
- C) Blob Storage supports more file types than ADLS Gen2
- D) ADLS Gen2 is a completely separate service

> ✅ **Answer: B**
> ADLS Gen2 is built on Blob Storage with an added hierarchical namespace (true folders) and POSIX-compliant access control lists.

---

**Q36.** Which Azure service would you use to store application log files that are continuously appended?
- A) Azure Blob Storage (Block Blob)
- B) Azure Blob Storage (Append Blob)
- C) Azure Table Storage
- D) Azure Files

> ✅ **Answer: B — Append Blob**
> Append Blobs are optimized for append-only operations, making them perfect for log files.

---

**Q37.** How does Cosmos DB charge for database operations?
- A) Per GB of data stored only
- B) Per query executed
- C) In Request Units per second (RU/s)
- D) Per connection

> ✅ **Answer: C — Request Units per second (RU/s)**
> Cosmos DB measures all database operations (reads, writes, queries) in Request Units. You provision RU/s capacity.

---

**Q38.** Which Azure service provides managed cloud file shares accessible via SMB protocol?
- A) Azure Blob Storage
- B) Azure Table Storage
- C) Azure Files
- D) Azure Queue Storage

> ✅ **Answer: C — Azure Files**
> Azure Files provides fully managed cloud file shares accessible via SMB (Windows) and NFS (Linux).

---

## Domain 4 – Analytics Workloads (Questions 39–50)

**Q39.** Which Azure Synapse Analytics option allows you to query files stored in a data lake using SQL without provisioning dedicated resources?
- A) Dedicated SQL Pool
- B) Spark Pool
- C) Serverless SQL Pool
- D) SQL on VM

> ✅ **Answer: C — Serverless SQL Pool**
> Serverless SQL Pool is on-demand and queries data lake files directly. You pay per TB of data scanned.

---

**Q40.** A company needs to build a traditional enterprise data warehouse with predictable performance. Which service is most appropriate?
- A) Azure Databricks
- B) Azure Synapse Analytics Dedicated SQL Pool
- C) Azure Stream Analytics
- D) Azure HDInsight

> ✅ **Answer: B — Azure Synapse Analytics Dedicated SQL Pool**
> Dedicated SQL Pool is a provisioned, MPP-based cloud data warehouse for enterprise analytics workloads.

---

**Q41.** What does Azure Data Factory primarily do?
- A) Stores analytical data at scale
- B) Visualizes data in dashboards
- C) Orchestrates data movement and transformation pipelines
- D) Processes real-time streaming data

> ✅ **Answer: C — Orchestrates data movement and transformation pipelines**
> ADF is an ETL/ELT orchestration tool that moves data between sources and destinations with transformation capabilities.

---

**Q42.** Which Power BI connectivity mode imports data into Power BI for fast performance?
- A) DirectQuery
- B) Live Connection
- C) Import
- D) Cached Query

> ✅ **Answer: C — Import**
> Import mode copies data into Power BI's internal engine (VertiPaq), providing fast query performance but requiring data refreshes.

---

**Q43.** A dashboard in Power BI can ONLY be created in:
- A) Power BI Desktop
- B) Power BI Mobile
- C) Power BI Report Server
- D) Power BI Service

> ✅ **Answer: D — Power BI Service**
> Dashboards are a cloud-only feature of the Power BI Service (app.powerbi.com). They are not available in Desktop.

---

**Q44.** Which component of Microsoft Fabric serves as the unified data lake underlying all Fabric workloads?
- A) Fabric Warehouse
- B) OneLake
- C) Fabric Pipeline
- D) Power BI Dataset

> ✅ **Answer: B — OneLake**
> OneLake is a single, unified data lake across all Microsoft Fabric experiences, eliminating the need to copy data between services.

---

**Q45.** Azure Databricks is primarily based on which open-source framework?
- A) Hadoop
- B) Kafka
- C) Apache Spark
- D) Flink

> ✅ **Answer: C — Apache Spark**
> Azure Databricks is a managed Apache Spark platform for data engineering, analytics, and machine learning.

---

**Q46.** Which windowing function in Azure Stream Analytics creates fixed, non-overlapping time windows?
- A) Hopping Window
- B) Sliding Window
- C) Tumbling Window
- D) Session Window

> ✅ **Answer: C — Tumbling Window**
> Tumbling windows are fixed-size, non-overlapping. Each event belongs to exactly one window.

---

**Q47.** What does a "fact table" in a star schema contain?
- A) Descriptive attributes like product names and customer cities
- B) Measurable, quantitative business metrics like sales amounts
- C) Configuration settings for the data warehouse
- D) Lookup values for dimension keys

> ✅ **Answer: B — Measurable, quantitative business metrics**
> Fact tables contain numeric measurements (revenue, quantity) and foreign keys to dimension tables.

---

**Q48.** Which Azure service ingests high-volume event streams at millions of events per second?
- A) Azure Service Bus
- B) Azure Event Hubs
- C) Azure Queue Storage
- D) Azure Notification Hubs

> ✅ **Answer: B — Azure Event Hubs**
> Azure Event Hubs is a big data streaming platform capable of ingesting millions of events per second with low latency.

---

**Q49.** A Power BI report must always show live data from a large Azure SQL Database without importing. Which connectivity mode should you use?
- A) Import
- B) Export
- C) DirectQuery
- D) Live Connection

> ✅ **Answer: C — DirectQuery**
> DirectQuery sends queries live to the source database each time a visual is loaded, ensuring data is always current.

---

**Q50.** Which Azure service would you choose to handle bidirectional communication with millions of IoT devices?
- A) Azure Event Hubs
- B) Azure IoT Hub
- C) Azure Service Bus
- D) Azure Stream Analytics

> ✅ **Answer: B — Azure IoT Hub**
> Azure IoT Hub provides bidirectional communication (device-to-cloud AND cloud-to-device) plus device management features.

---

## 📊 Score Tracker

| Domain | Questions | Your Score |
|--------|-----------|-----------|
| Domain 1 – Core Data Concepts | Q1–Q15 (15 questions) | /15 |
| Domain 2 – Relational Data | Q16–Q28 (13 questions) | /13 |
| Domain 3 – Non-Relational Data | Q29–Q38 (10 questions) | /10 |
| Domain 4 – Analytics Workloads | Q39–Q50 (12 questions) | /12 |
| **Total** | **50 questions** | **/50** |

**Scoring guide:**
- 45–50 ✅ Excellent — You're ready!
- 38–44 🟡 Good — Review weak areas
- Below 38 🔴 Keep studying — revisit the notes

---

> ⬅️ [Back to DP-900 Index](./index.md) | ⚡ [Quick Reference](./quick-reference.md)
