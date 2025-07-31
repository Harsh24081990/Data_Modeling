#### QUE: How to model a schema and how to decide the normalization level (in a ADF - Databricks project we should follow) ? how to decide which schema we should use (star / snowflake) ? Star and snowflake are in which normal form ?

### ANS:

In an ADF–Databricks data pipeline project, schema modeling and normalization level depend on the **layer (Bronze/Silver/Gold)** and **business needs**.

---

### 🔹 How to decide **normalization level**:

| Layer      | Normalization Level                     | Reason                                                       |
| ---------- | --------------------------------------- | ------------------------------------------------------------ |
| **Bronze** | *Raw / Minimal normalization (0NF/1NF)* | Fast ingestion, retain source format                         |
| **Silver** | *Moderate normalization (2NF/3NF)*      | Clean, deduplicated, optimized for joins and transformations |
| **Gold**   | *Denormalized (0NF/1NF)*                | Optimized for analytics, reporting, and BI tools             |

---

### 🔹 Star vs Snowflake Schema

| Criteria    | **Star Schema**                      | **Snowflake Schema**               |
| ----------- | ------------------------------------ | ---------------------------------- |
| Structure   | Fact table + denormalized dimensions | Fact table + normalized dimensions |
| Joins       | Fewer joins                          | More joins                         |
| Performance | Better for queries                   | Slightly slower due to joins       |
| Storage     | More space used                      | Space-efficient                    |
| Use Case    | BI/reporting (Gold Layer)            | Complex querying (Silver Layer)    |

---

### 🔹 Normal Forms of Star and Snowflake

* **Star Schema**: 1NF or sometimes unnormalized (due to denormalized dimension tables)
* **Snowflake Schema**: Usually in **3NF** (dimension tables are normalized)

---

### ✅ Decision Guidelines:

1. **Use Snowflake schema** in Silver layer if:

   * You need modular, reusable dimension tables.
   * Query performance is not a big concern.
   * Data size is large and needs normalization.

2. **Use Star schema** in Gold layer if:

   * You need fast BI/dashboard access.
   * Tools like Power BI/Tableau are used.
   * Simpler, flat structures are preferred.

