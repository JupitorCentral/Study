- [x] Transitive dependency vs Partial dependency ➕ 2025-06-18 ✅ 2025-06-18

### The Core Distinction

The simplest way to distinguish them is by looking at what an attribute depends on:

- **Partial Dependency:** A non-key attribute depends on **only a part of the composite primary key**. This can _only_ happen if your table uses a composite primary key (a key made of two or more columns).
    
- **Transitive Dependency:** A non-key attribute depends on **another non-key attribute**, which in turn depends on the primary key. This is like an indirect dependency.

Here’s a table to summarize the key differences before we dive into examples:

|Feature|Partial Dependency|Transitive Dependency|
|:--|:--|:--|
|**Normalization Level**|Addressed by **Second Normal Form (2NF)**|Addressed by **Third Normal Form (3NF)**|
|**Key Requirement**|Requires a **Composite Primary Key**|Can occur with a simple or composite primary key|
|**Dependency Path**|Non-Key Attribute → Part of Primary Key|Non-Key Attribute → Another Non-Key Attribute → Primary Key|


---

### Partial Dependency (Violates 2NF)

A partial dependency exists when an attribute is not fully dependent on the entire composite primary key; it's only dependent on one part of it.

**Rule:** To be in 2NF, a table must first be in 1NF and have **no partial dependencies**.

#### Example:

Let's consider a table for student course registrations where the combination of `StudentID` and `CourseID` uniquely identifies each row.

**Registrations Table (Not in 2NF)**

|StudentID (PK)|CourseID (PK)|StudentName|CourseGrade|
|:--|:--|:--|:--|
|S101|C20|Alice|A|
|S101|C30|Alice|B|
|S102|C20|Bob|C|



- **Primary Key**: (`StudentID`, `CourseID`) - This is a composite primary key.
    
- **Non-Key Attributes**: `StudentName`, `CourseGrade`.

Now let's analyze the dependencies:

- `CourseGrade` depends on **both** `StudentID` and `CourseID`. You need to know which student and which course to find the grade. This is a **full dependency**.
- `StudentName` depends **only on `StudentID`**. If you know the `StudentID` is 'S101', you know the `StudentName` is 'Alice', regardless of which course she's taking.
    

This is a **partial dependency**. `StudentName` is dependent on only a part (`StudentID`) of the composite primary key.

**Problems this causes:**

- **Redundancy:** 'Alice' is stored multiple times.
- **Update Anomaly:** If Alice changes her name, you have to update it in every row she appears in.
- **Insertion Anomaly:** You cannot add a new student until they have registered for at least one course.

**Solution (Achieving 2NF):** Decompose the table to remove the partial dependency.

**Students Table**

|StudentID (PK)|StudentName|
|:--|:--|
|S101|Alice|
|S102|Bob|



**Enrollments Table**

|StudentID (PK, FK)|CourseID (PK, FK)|CourseGrade|
|:--|:--|:--|
|S101|C20|A|
|S101|C30|B|
|S102|C20|C|



Now, `StudentName` is in its own table, and every non-key attribute in the `Enrollments` table (`CourseGrade`) is fully dependent on the entire primary key (`StudentID`, `CourseID`).

---

### Transitive Dependency (Violates 3NF)

A transitive dependency is an indirect relationship. It occurs when a non-key attribute determines another non-key attribute.

**Rule:** To be in 3NF, a table must first be in 2NF and have **no transitive dependencies**.

#### Example:

Let's use a table of employees and their departments.

**Employees Table (In 2NF, but not 3NF)**

|EmployeeID (PK)|EmployeeName|DepartmentID|DepartmentName|
|:--|:--|:--|:--|
|E01|Charlie|D1|Marketing|
|E02|David|D2|Engineering|
|E03|Eve|D1|Marketing|



- **Primary Key**: `EmployeeID` (a simple key)
    
- **Non-Key Attributes**: `EmployeeName`, `DepartmentID`, `DepartmentName`

Let's analyze the dependencies using a dependency chain:

1. `EmployeeID` → `DepartmentID` (The Employee ID determines which department the employee is in).
2. `DepartmentID` → `DepartmentName` (The Department ID determines the department's name).

Because `EmployeeID` → `DepartmentID` and `DepartmentID` → `DepartmentName`, we have an indirect, or **transitive**, dependency: `EmployeeID` → `DepartmentName` through `DepartmentID`.

A non-key attribute (`DepartmentName`) is determined by another non-key attribute (`DepartmentID`).

**Problems this causes:**

- **Redundancy:** 'Marketing' is stored multiple times.
- **Update Anomaly:** If the 'Marketing' department is renamed to 'Sales & Marketing', you have to find and update all employee records in that department.
- **Insertion Anomaly:** You cannot add a new department (e.g., 'Human Resources') until you assign at least one employee to it.
    

**Solution (Achieving 3NF):** Decompose the table to remove the transitive dependency.

**Employees Table**

|EmployeeID (PK)|EmployeeName|DepartmentID (FK)|
|:--|:--|:--|
|E01|Charlie|D1|
|E02|David|D2|
|E03|Eve|D1|



**Departments Table**

|DepartmentID (PK)|DepartmentName|
|:--|:--|
|D1|Marketing|
|D2|Engineering|



Now, all non-key attributes depend directly on the primary key, and only the primary key.

### Final Summary

- Think **"Part of Key"** for **Partial Dependency**. Does a non-key field depend on just one piece of a multi-column key? This is a **2NF** issue.
- Think **"Non-Key to Non-Key"** for **Transitive Dependency**. Does a non-key field depend on another non-key field? This is a **3NF** issue.