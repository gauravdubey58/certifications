# AZ-900 Notes -- Azure Storage Services

------------------------------------------------------------------------

## Azure Storage Overview

Azure Storage provides **scalable, durable, secure cloud storage** for
different data types.

**Key Benefits:** - Highly available & durable - Massive scalability -
Secure by default - Pay only for usage

------------------------------------------------------------------------

## Azure Storage Accounts

**Definition:**\
A storage account is a **logical container** that holds Azure storage
services.

**Important Points:** - Required before creating storage services -
Globally unique name - Defines performance & redundancy options

**Types (Exam-Level):** - General-purpose v2 (most common) - Supports
blobs, files, queues, tables

**Memory Tip:**\
Storage Account = Parent container

------------------------------------------------------------------------

## Azure Storage Redundancy (Very Important for Exam)

Redundancy protects against data loss.

**1. LRS -- Locally Redundant Storage** - Copies within single
datacenter - Lowest cost

**2. ZRS -- Zone Redundant Storage** - Copies across availability
zones - Protects zone failures

**3. GRS -- Geo-Redundant Storage** - Replicates to paired region -
Disaster recovery protection

**4. GZRS -- Geo-Zone Redundant Storage** - Combines ZRS + GRS - Highest
resiliency

**5. RA-GRS -- Read-Access Geo-Redundant** - Read from secondary region

**Quick Exam Trick:** More letters = More protection = Higher cost

------------------------------------------------------------------------

## Azure Storage Services

Azure supports multiple storage types:

### Blob Storage

**Use:** Unstructured data (images, videos, backups)

### File Storage

**Use:** Managed file shares (SMB protocol)

### Queue Storage

**Use:** Message storage for applications

### Table Storage

**Use:** NoSQL key-value storage

### Disk Storage

**Use:** VM persistent disks

**Memory Tip:** Blob = Objects \| File = Shares \| Queue = Messages \|
Table = NoSQL

------------------------------------------------------------------------

## Create a Storage Blob (Conceptual Flow)

Typical steps:

1.  Create Storage Account
2.  Create Blob Container
3.  Upload Blob (files/data)

------------------------------------------------------------------------

## Azure Data Migration Options

Used when moving data into Azure.

**Common Options:**

### Azure Migrate

-   Assess & migrate workloads

### Azure Data Box

-   Physical device for large data transfer
-   Useful for TB/PB scale data

### Storage Migration Service

-   Server migrations

**Exam Tip:**\
Data Box = Offline large data transfer

------------------------------------------------------------------------

## Azure File Movement Options

Used for copying/syncing files.

### AzCopy

-   Command-line tool
-   Fast data transfer

### Azure Storage Explorer

-   GUI-based management tool

### Azure File Sync

-   Sync on-premises & Azure file shares

**Exam Tip:** AzCopy = Scripted transfer \| Storage Explorer = GUI

------------------------------------------------------------------------

## Exam Key Takeaways

-   Storage Account required first
-   Redundancy = Durability vs Cost
-   Blob storage for unstructured data
-   Data Box for huge datasets
-   AzCopy & Storage Explorer for transfers
