### QUE: what are the normal forms generally implemented in a project using ADF and Databricks , while designing the tables ? How to model a schema and how to decide the normalization we should follow ? Give me an example table design for Bronze, Silver, and Gold layers in a Databricks + ADF project (customer orders use case).

### ANS:

### ✅ Normal Forms in ADF + Databricks Projects

Typically, the following **normalization levels** are used:

* **Bronze Layer**: *Raw ingested data* — usually **unnormalized or lightly normalized** (1NF).
* **Silver Layer**: *Cleaned and transformed* — typically **3NF**, with joins and surrogate keys.
* **Gold Layer**: *Business-level aggregations* — **denormalized**, optimized for reporting.

---

### ✅ Schema Modeling Guidelines

* **Bronze**: Model directly from source, minimal changes.
* **Silver**: Apply normalization up to 3NF. Create dimension and fact-like structures.
* **Gold**: Design for performance and user consumption. Denormalize with joins/aggregations.

---

### 📦 Example: Customer Orders Use Case

#### 🔶 Bronze Layer (Raw from source – JSON/CSV/etc.)

| customer\_id | full\_name  | address       | order\_id | product | amount | order\_date |
| ------------ | ----------- | ------------- | --------- | ------- | ------ | ----------- |
| C001         | John Doe    | "123 St, NY"  | O1001     | Laptop  | 55000  | 2024-01-12  |
| C002         | Alice Smith | "456 Ave, LA" | O1002     | Mouse   | 1000   | 2024-01-13  |

*No keys, no constraints. Stored as-is in Delta.*

---

#### 🔷 Silver Layer (Normalized: 3NF)

**Customer Table**

| customer\_id | full\_name | address    |
| ------------ | ---------- | ---------- |
| C001         | John Doe   | 123 St, NY |

**Order Table**

| order\_id | customer\_id | order\_date |
| --------- | ------------ | ----------- |
| O1001     | C001         | 2024-01-12  |

**Order\_Items Table**

| order\_id | product | amount |
| --------- | ------- | ------ |
| O1001     | Laptop  | 55000  |

*Surrogate keys and FK relationships applied.*

---

#### 🟡 Gold Layer (Aggregated for dashboard/report)

| customer\_name | total\_orders | total\_amount | last\_order\_date |
| -------------- | ------------- | ------------- | ----------------- |
| John Doe       | 3             | 125000        | 2024-03-15        |
| Alice Smith    | 2             | 67000         | 2024-02-20        |

*Business-friendly, denormalized, materialized as views or tables.*

---

### QUE: Explain what is meant by normalization and normal forms 1,2,3 means ? use examples with sample tables. 
### ANS:

### ✅ What is Normalization?

**Normalization** is the process of organizing data in a database to:

* Reduce **data redundancy**
* Improve **data integrity**
* Make updates/inserts/deletes more efficient

---

### ✅ Normal Forms Explained with Examples

---

### 🔹 **1NF (First Normal Form)**

**Rule**:

* Atomic values only (no repeating groups or arrays)
* Each row must be unique

**❌ Not in 1NF**:

| customer\_id | name | orders        |
| ------------ | ---- | ------------- |
| C001         | John | Laptop, Mouse |

**✅ 1NF (atomic values)**:

| customer\_id | name | order  |
| ------------ | ---- | ------ |
| C001         | John | Laptop |
| C001         | John | Mouse  |

---

### 🔹 **2NF (Second Normal Form)**

**Rule**:

* Must be in 1NF
* No **partial dependency** (non-key columns should depend on the **whole** primary key)

**❌ Not in 2NF (partial dependency on part of composite key)**:

| order\_id | product\_id | customer\_name |
| --------- | ----------- | -------------- |
| O1        | P1          | John           |

Here, `customer_name` depends only on `order_id` (part of composite key), not full key.

**✅ 2NF (split to remove partial dependency)**:

**Orders Table**

| order\_id | customer\_id |
| --------- | ------------ |
| O1        | C001         |

**Order\_Items Table**

| order\_id | product\_id |
| --------- | ----------- |
| O1        | P1          |

**Customers Table**

| customer\_id | name |
| ------------ | ---- |
| C001         | John |

---

### 🔹 **3NF (Third Normal Form)**

**Rule**:

* Must be in 2NF
* No **transitive dependency** (non-key column depends on another non-key column)

**❌ Not in 3NF (city depends on pincode, not on primary key)**:

| customer\_id | name | pincode | city      |
| ------------ | ---- | ------- | --------- |
| C001         | John | 110001  | New Delhi |

**✅ 3NF (remove transitive dependency)**:

**Customers Table**

| customer\_id | name | pincode |
| ------------ | ---- | ------- |
| C001         | John | 110001  |

**Pincode Table**

| pincode | city      |
| ------- | --------- |
| 110001  | New Delhi |

---


