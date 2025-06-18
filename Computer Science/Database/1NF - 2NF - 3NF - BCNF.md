#dev


- [ ] 1NF - 2NF - 3NF - BCNF ➕ 2025-06-14 

Database normalization is the process of organizing columns and tables in a relational database to minimize data redundancy and improve data integrity. Each normal form represents a set of rules that builds upon the last.

### The Progressive Example: A University Class Roster

Imagine we start with a single, unnormalized table to track which students are in which course, who teaches it, and the professor's details.

**Unnormalized Table**

| Course_ID | Course_Name     | Instructor_ID | Instructor_Name  | Student_Roster |
| :-------- | :-------------- | :------------ | :--------------- | :------------- |
| CS101     | Intro to Python | P205          | Dr. Ada Lovelace | S1, S2         |
| MA203     | Calculus II     | P110          | Dr. Alan Turing  | S2, S3, S4     |
| CS201     | Data Structures | P205          | Dr. Ada Lovelace | S1, S3         |
|           |                 |               |                  |                |
|           |                 |               |                  |                |


---

### 1st Normal Form (1NF): Ensure Atomic Values

**Rule:** Each cell in the table must hold a single, indivisible (atomic) value. There can be no repeating groups or lists in any cell.

**Problem:** The `Student_Roster` column violates 1NF because it contains a list of multiple students.

**Action:** To fix this, we create a separate row for each student in each course.

**Result (Now in 1NF):**

|Course_ID|Course_Name|Instructor_ID|Instructor_Name|Student_ID|
|:--|:--|:--|:--|:--|
|CS101|Intro to Python|P205|Dr. Ada Lovelace|S1|
|CS101|Intro to Python|P205|Dr. Ada Lovelace|S2|
|MA203|Calculus II|P110|Dr. Alan Turing|S2|
|MA203|Calculus II|P110|Dr. Alan Turing|S3|
|MA203|Calculus II|P110|Dr. Alan Turing|S4|
|CS201|Data Structures|P205|Dr. Ada Lovelace|S1|
|CS201|Data Structures|P205|Dr. Ada Lovelace|S3|


---

### 2nd Normal Form (2NF): Eliminate Partial Dependencies

**Rule:** The table must be in 1NF, and all non-key attributes must depend on the **entire** primary key. This rule is only relevant for tables with a **composite primary key** (a key made of two or more columns).

**Problem:** In our 1NF table, the primary key is `{Course_ID, Student_ID}`. However, some columns only depend on a _part_ of this key:

- `Course_Name`, `Instructor_ID`, and `Instructor_Name` depend only on `Course_ID`. This is a **partial dependency** and it causes redundancy. For example, "Intro to Python" and "Dr. Ada Lovelace" are repeated for every student in that course.

**Action:** We decompose the table. We pull out the columns that are partially dependent into a new table.

**Result (Now in 2NF):**

**Table 1: Enrollments**

|Course_ID (FK)|Student_ID (FK)|
|:--|:--|
|CS101|S1|
|CS101|S2|
|MA203|S2|
|MA203|S3|
|MA203|S4|
|CS201|S1|
|CS201|S3|


**Table 2: Courses**

|Course_ID (PK)|Course_Name|Instructor_ID|Instructor_Name|
|:--|:--|:--|:--|
|CS101|Intro to Python|P205|Dr. Ada Lovelace|
|MA203|Calculus II|P110|Dr. Alan Turing|
|CS201|Data Structures|P205|Dr. Ada Lovelace|


---

### 3rd Normal Form (3NF): Eliminate Transitive Dependencies

**Rule:** The table must be in 2NF, and every non-key attribute must depend _only_ on the primary key, not on other non-key attributes.

**Problem:** In the `Courses` table, the primary key is `Course_ID`.

- `Instructor_ID` depends on `Course_ID`.
- `Instructor_Name` depends on `Instructor_ID`.
    

Therefore, `Instructor_Name` has an indirect, or **transitive dependency**, on `Course_ID` via `Instructor_ID`. This is a problem because if Dr. Lovelace's name changes, we might have to update it in multiple course records.

**Action:** We decompose again, moving the transitively dependent attributes into their own table.

**Result (Now in 3NF):**

**Table 1: Enrollments** (No change)

|Course_ID (FK)|Student_ID (FK)|
|:--|:--|
|...|...|

Export to Sheets

**Table 2: Courses_New**

|Course_ID (PK)|Course_Name|Instructor_ID (FK)|
|:--|:--|:--|
|CS101|Intro to Python|P205|
|MA203|Calculus II|P110|
|CS201|Data Structures|P205|


**Table 3: Instructors**

|Instructor_ID (PK)|Instructor_Name|
|:--|:--|
|P205|Dr. Ada Lovelace|
|P110|Dr. Alan Turing|


---

### Boyce-Codd Normal Form (BCNF): A Stricter 3NF

**Rule:** For any non-trivial functional dependency `A → B`, `A` must be a **superkey**. (A superkey is any set of columns that can uniquely identify a row).

Most tables in 3NF are also in BCNF. BCNF addresses very specific anomalies where a table has multiple overlapping candidate keys.

**Problem Scenario:** Let's add a new constraint. An instructor can teach multiple courses, but they can only belong to **one** department, and each department appoints one "Head Instructor" for each course.

Consider this table, which is in 3NF:

|Instructor_ID (PK)|Course_ID (PK)|Department|
|:--|:--|:--|
|P205|CS101|Computer Science|
|P205|CS201|Computer Science|
|P110|MA203|Mathematics|
|P300|CS101|Computer Science|

Export to Sheets

Here, the primary key is `{Instructor_ID, Course_ID}`. The table has two important dependencies:

1. `{Instructor_ID, Course_ID} → Department` (This follows the rules).
2. `Instructor_ID → Department` (An instructor is in only one department).

The second dependency violates BCNF. Its determinant, `Instructor_ID`, determines `Department`, but `Instructor_ID` is **not a superkey** on its own (it doesn't uniquely identify a row).

**Action:** We decompose to ensure every determinant is a superkey.

**Result (Now in BCNF):**

**Table 1: Course_Assignments**

|Instructor_ID (FK)|Course_ID (FK)|
|:--|:--|
|P205|CS101|
|P205|CS201|
|P110|MA203|
|P300|CS101|


**Table 2: Instructor_Departments**

|Instructor_ID (PK)|Department|
|:--|:--|
|P205|Computer Science|
|P110|Mathematics|
|P300|Computer Science|

### Summary Comparison

|Normal Form|Main Objective|Requirement|Eliminates|
|:--|:--|:--|:--|
|**1NF**|Ensure columns hold single values.|All values must be atomic.|Repeating groups and multi-valued columns.|
|**2NF**|Make non-key attributes dependent on the whole key.|Must be in 1NF.|Partial dependencies on a composite key.|
|**3NF**|Ensure non-key attributes depend only on the key.|Must be in 2NF.|Transitive dependencies (non-key depending on non-key).|
|**BCNF**|Ensure every determinant is a key.|For any dependency `A → B`, `A` must be a superkey.|Remaining anomalies where a non-key determines a key attribute.|