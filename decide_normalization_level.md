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


