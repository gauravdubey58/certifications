# Domain 2 – Relational Data on Azure `20–25%`

> ⬅️ [Back to DP-900 Index](./index.md)

---

## 2.1 Relational Data Concepts

### What Makes Data Relational?
- Data organized in **tables** (relations) with rows (records) and columns (attributes)
- Tables are linked by **keys** (primary key → foreign key relationships)
- Schema is **defined in advance** (structure before data)
- Queried with **SQL** (Structured Query Language)

### Keys
| Key Type | Description |
|----------|-------------|
| **Primary Key (PK)** | Uniquely identifies each row in a table. Cannot be NULL. |
| **Foreign Key (FK)** | References the PK of another table. Enforces referential integrity. |
| **Composite Key** | PK made of two or more columns together |
| **Unique Key** | Like PK but allows one NULL |

### Normalization
Process of organizing tables to **reduce data redundancy**:

| Normal Form | Rule |
|-------------|------|
| **1NF** | Each column holds atomic (single) values; no repeating groups |
| **2NF** | 1NF + no partial dependencies (non-key columns depend on WHOLE PK) |
| **3NF** | 2NF + no transitive dependencies (non-key columns depend only on PK) |

### Indexes
- **Clustered index** — determines physical sort order of data; one per table (usually PK)
- **Non-clustered index** — separate structure pointing to rows; multiple allowed; speeds up searches

---

## 2.2 SQL Fundamentals

### Data Manipulation Language (DML)
```sql
-- SELECT
SELECT CustomerName, City FROM Customers WHERE Country = 'UK' ORDER BY CustomerName;

-- INSERT
INSERT INTO Customers (Name, City, Country) VALUES ('Acme', 'London', 'UK');

-- UPDATE
UPDATE Customers SET City = 'Manchester' WHERE CustomerID = 5;

-- DELETE
DELETE FROM Customers WHERE CustomerID = 5;
```

### Data Definition Language (DDL)
```sql
-- CREATE
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2)
);

-- ALTER
ALTER TABLE Products ADD CategoryID INT;

-- DROP
DROP TABLE Products;
```

### JOINs
```sql
-- INNER JOIN: only rows with matches in BOTH tables
SELECT o.OrderID, c.CustomerName
FROM Orders o INNER JOIN Customers c ON o.CustomerID = c.CustomerID;

-- LEFT JOIN: all rows from LEFT table + matching from right
SELECT c.CustomerName, o.OrderID
FROM Customers c LEFT JOIN Orders o ON c.CustomerID = o.CustomerID;

-- RIGHT JOIN: all rows from RIGHT table + matching from left
-- FULL OUTER JOIN: all rows from both tables
```

### Aggregation
```sql
SELECT Country, COUNT(*) AS CustomerCount, AVG(Age) AS AvgAge
FROM Customers
GROUP BY Country
HAVING COUNT(*) > 5
ORDER BY CustomerCount DESC;
```

### Views
- Virtual table based on a SELECT query
- Does not store data itself
- Simplifies complex queries and provides security

```sql
CREATE VIEW UKCustomers AS
SELECT CustomerID, Name FROM Customers WHERE Country = 'UK';
```

### Stored Procedures
- Pre-compiled SQL stored in the database
- Can accept parameters, contain logic, improve performance and security

---

## 2.3 Azure Relational Database Services

### Azure SQL Database
- **Fully managed PaaS** — Microsoft handles patching, backups, HA
- Based on **SQL Server** engine (latest stable version)
- Best for: **new cloud-native applications**
- Deployment options:
  - **Single Database** — isolated, own resources
  - **Elastic Pool** — multiple databases sharing a pool of resources (cost-efficient)
- Purchasing models: **DTU** (bundled compute+storage) or **vCore** (separate compute+storage)
- Built-in: high availability, automated backups, geo-replication

### Azure SQL Managed Instance
- **Fully managed PaaS** but with **near 100% SQL Server compatibility**
- Best for: **migrating existing on-premises SQL Server** apps with minimal changes
- Supports features not in Azure SQL Database: SQL Agent, linked servers, cross-database queries
- Deployed inside a **VNet** (virtual network) for full network isolation

### SQL Server on Azure Virtual Machines (IaaS)
- **Full SQL Server** installed on an Azure VM
- **You manage**: OS, patches, SQL Server, backups
- Best for: **maximum compatibility**, specific SQL Server versions, Windows Authentication
- Lift-and-shift migrations with zero change

### Comparison Table

| Feature | Azure SQL DB | SQL Managed Instance | SQL on VM |
|---------|-------------|----------------------|-----------|
| Management | Fully managed | Fully managed | You manage OS + SQL |
| SQL Server compatibility | High (some limits) | Near 100% | 100% |
| SQL Agent | Limited | ✅ Yes | ✅ Yes |
| Cross-database queries | ❌ No | ✅ Yes | ✅ Yes |
| VNet integration | Optional | Required | Yes (VM is in VNet) |
| Best for | New apps | Migration | Full control |

### Azure Database for Open-Source Engines

| Service | Engine | Notes |
|---------|--------|-------|
| **Azure Database for PostgreSQL** | PostgreSQL | Flexible Server (recommended), Single Server (retiring) |
| **Azure Database for MySQL** | MySQL | Flexible Server, compatible with MySQL Workbench |
| **Azure Database for MariaDB** | MariaDB | Fully managed fork of MySQL |

All provide: automatic backups, high availability, security, scaling.

> ⭐ **Exam Tip:** The difference between Azure SQL Database (new apps), SQL Managed Instance (migration with high compatibility), and SQL on VM (full control/IaaS) is a very common exam question.

---

## 2.4 Querying Azure Databases

### Tools for Querying
| Tool | Description |
|------|-------------|
| **Azure Portal Query Editor** | Browser-based SQL editor for Azure SQL Database |
| **SQL Server Management Studio (SSMS)** | Full-featured desktop IDE |
| **Azure Data Studio** | Cross-platform lightweight IDE |
| **sqlcmd** | Command-line tool |
| **Visual Studio Code** | With SQL Server extension |

---

> ⬅️ [← Domain 1](./domain-1-core-data-concepts.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-non-relational-data.md)
