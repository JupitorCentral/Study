#database #normalization


- [x] 1NF - 2NF ➕ 2025-06-14 ✅ 2025-06-19

### 1st Normal Form (1NF)

1NF is the most basic level of normalization. It sets the minimum requirements for a table to be considered a relational database table.

**The Rules for 1NF:**

1. **Each column must contain atomic values:** This is the most important rule. "Atomic" means that the values in a column are indivisible. You cannot have lists, arrays, or repeating groups of values within a single cell. 
2. **Each row must be unique:** This is typically achieved by having a primary key.
3. All values in a column must be of the same data type.

#### Example: From Non-1NF to 1NF

Let's imagine a simple table for tracking customer orders that is **not** in 1NF.

**Before 1NF (Violates Atomicity)**

| OrderID | CustomerName | Products                    |
| :------ | :----------- | :-------------------------- |
| 101     | Alice        | `Laptop, Mouse`             |
| 102     | Bob          | `Keyboard`                  |
| 103     | Charlie      | `Monitor, Webcam, Keyboard` |

Export to Sheets

**The Problem:** The `Products` column is not atomic. It contains a comma-separated list of items in a single cell, which is difficult to query and manage. For example, how would you easily find everyone who bought a "Keyboard"?

**To achieve 1NF**, we must ensure every cell holds only one value. We do this by giving each product in an order its own row.

**After 1NF**

|OrderID|CustomerName|Product|
|:--|:--|:--|
|101|Alice|Laptop|
|101|Alice|Mouse|
|102|Bob|Keyboard|
|103|Charlie|Monitor|
|103|Charlie|Webcam|
|103|Charlie|Keyboard|


Now the table is in 1NF. Every cell contains a single, atomic value.

---

### 2nd Normal Form (2NF)

2NF builds directly on 1NF. It is concerned with eliminating a specific type of redundancy called a "partial dependency."

**The Rules for 2NF:**

1. The table must already be in **1NF**.
2. There must be **no partial dependencies**.

**What is a Partial Dependency?** A partial dependency occurs when a non-key attribute depends on **only a part** of a composite primary key.

_This means 2NF is only a concern for tables with a **composite primary key** (a primary key made of two or more columns)._ If a table is in 1NF and has a single-column primary key, it is automatically in 2NF.

#### Example: From 1NF to 2NF

Let's continue with our order example. To uniquely identify each row in our 1NF table, we need a composite primary key: `{OrderID, Product}`.

**Before 2NF (In 1NF, but with Partial Dependencies)**

| OrderID (PK) | Product (PK) | CustomerName | OrderDate  | ProductName |
| :----------- | :----------- | :----------- | :--------- | :---------- |
| 101          | P01          | Alice        | 2025-06-10 | Laptop      |
| 101          | P02          | Alice        | 2025-06-10 | Mouse       |
| 102          | P03          | Bob          | 2025-06-11 | Keyboard    |
| 103          | P04          | Charlie      | 2025-06-12 | Monitor     |
| 103          | P05          | Charlie      | 2025-06-12 | Webcam      |
| 103          | P03          | Charlie      | 2025-06-12 | Keyboard    |


**The Problem:**

- Our Primary Key is `{OrderID, Product}`.
- `CustomerName` and `OrderDate` depend only on `OrderID`, not the whole key. This is a **partial dependency**.
    
- `ProductName` depends only on `Product` (assuming we use a Product ID), not the whole key. This is also a **partial dependency**.
    

This causes redundancy. "Alice" and her order date are repeated for every item in her order. If Alice's name needs to be updated, it has to be changed in multiple places.

**To achieve 2NF**, we must remove these partial dependencies by splitting the table. We create new tables for the attributes that are partially dependent.

**After 2NF (Decomposed into Three Tables)**

**1. Orders Table** (Information that depends only on `OrderID`)

|OrderID (PK)|CustomerName|OrderDate|
|:--|:--|:--|
|101|Alice|2025-06-10|
|102|Bob|2025-06-11|
|103|Charlie|2025-06-12|


**2. Products Table** (Information that depends only on `ProductID`)

|ProductID (PK)|ProductName|
|:--|:--|
|P01|Laptop|
|P02|Mouse|
|P03|Keyboard|
|P04|Monitor|
|P05|Webcam|


**3. Order_Details Table** (The remaining information, linking Orders and Products)

|OrderID (FK)|ProductID (FK)|
|:--|:--|
|101|P01|
|101|P02|
|102|P03|
|103|P04|
|103|P05|
|103|P03|


Now, all partial dependencies are removed. Each table contains information about one specific entity (an order, a product, or the link between them), which significantly reduces redundancy and improves data integrity.

### Summary: 1NF vs. 2NF

|Aspect|1st Normal Form (1NF)|2nd Normal Form (2NF)|
|:--|:--|:--|
|**Main Rule**|All columns must have atomic (indivisible) values.|Must be in 1NF and have no partial dependencies.|
|**Key Requirement**|Single values in each cell.|Non-key columns must depend on the _entire_ composite primary key.|
|**Problem It Solves**|Eliminates repeating groups and multi-valued columns.|Eliminates redundancy by separating data that only depends on _part_ of a composite key.|



