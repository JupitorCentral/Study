#database #nomarlization


- [ ] 3NF - BCNF ➕ 2025-06-14 

## 3NF

| EnrollmentID (PK) | StudentID | StudentName | CourseID | CourseName      |
| :---------------- | :-------- | :---------- | :------- | :-------------- |
| 101               | S1        | Alice       | C10      | Intro to SQL    |
| 102               | S2        | Bob         | C20      | Adv. Databases  |
| 103               | S3        | Charlie     | C10      | Intro to SQL    |
| 104               | S4        | David       | C30      | Web Development |
| 105               | S5        | Eve         | C10      | Intro to SQL    |
|                   |           |             |          |                 |


A non-key attribute (`CourseName`) depends on another non-key attribute (`CourseID`). This design leads to potential data anomalies:

- **Update Anomaly:** If the name of the "Intro to SQL" course changes, you would need to find and update all three rows where `CourseID` is 'C10'. Missing even one would lead to inconsistent data.

- **Insertion Anomaly:** You cannot add a new course (e.g., 'C40', 'Data Science') to the system until at least one student enrolls in it, because you need an `EnrollmentID` to create a record.

- **Deletion Anomaly:** If you delete the enrollment record for the only student in "Web Development" (EnrollmentID 104), you lose all information about that course, including its ID and name.



---

#### What is a 'Non-Key' Attribute?

In simple terms, a **non-key attribute is any column in a table that is _not_ part of the primary key.**

The **Primary Key (PK)** is a special column (or set of columns) designated to uniquely identify each single record in a table. Think of it as a Social Security Number for each row—no two can be the same.

Everything else is a non-key attribute. These columns hold the descriptive details about the record.

**Let's look at our example:**

In the `Courses` table we created:

|CourseID (PK)|CourseName|
|:--|:--|
|**C10**|**Intro to SQL**|
|**C20**|**Adv. Databases**|
|**C30**|**Web Development**|

Export to Sheets

- **`CourseID` is the Primary Key**. It uniquely identifies each course. 'C10' will only ever appear once in this column.
- **`CourseName` is a Non-Key Attribute**. It's a piece of information that _describes_ the record identified by the primary key. It tells us the name of the course with the ID 'C10'.

In the original, unnormalized table:

|EnrollmentID (PK)|StudentID|StudentName|CourseID|CourseName|
|:--|:--|:--|:--|:--|
|**101**|**S1**|**Alice**|**C10**|**Intro to SQL**|

- **`EnrollmentID` is the Primary Key**.
- **`StudentID`**, **`StudentName`**, **`CourseID`**, and **`CourseName`** are all **Non-Key Attributes**. They provide details about that specific enrollment record.

The goal of normalization (like 3NF) is to make sure that all the non-key attributes in a table are dependent _only_ on the primary key, and not on other non-key attributes.



#### What is 'Enrollment'?

In this context, an **enrollment is the official record that links one specific student to one specific course.**

Think of it as an event or a relationship.

- A **Student** is a person (e.g., Alice).
- A **Course** is a subject being taught (e.g., "Intro to SQL").
- An **Enrollment** is the action or record of Alice signing up for "Intro to SQL".


---

### After 3NF (Transitive Dependency Removed)

To achieve 3NF, we must remove the transitive dependency. We do this by splitting the original table into two separate tables. Non-key attributes must depend only on the primary key.

**1. Enrollments Table**

This table now only contains information directly related to the enrollment itself. We keep `CourseID` as a foreign key to link to the course information.

|EnrollmentID (PK)|StudentID|StudentName|CourseID (FK)|
|:--|:--|:--|:--|
|101|S1|Alice|C10|
|102|S2|Bob|C20|
|103|S3|Charlie|C10|
|104|S4|David|C30|
|105|S5|Eve|C10|



**2. Courses Table**

This new table holds the course details. `CourseName` now depends directly on the table's primary key, `CourseID`.

| CourseID (PK) | CourseName      |
| :------------ | :-------------- |
| C10           | Intro to SQL    |
| C20           | Adv. Databases  |
| C30           | Web Development |


---

### Transitive Dependency

In simple terms, a **transitive dependency** is an indirect dependency where a non-key attribute depends on another non-key attribute, instead of depending directly on the primary key.

Think of it like a "friend of a friend" relationship in your data.

- **You → Your Friend** (This is a direct dependency).
- **Your Friend → Their Friend** (This is another direct dependency).
- **You → Their Friend** (This is an indirect, or transitive, dependency).

### The Database Rule

A table has a transitive dependency if it has the following structure:

1. `A → B` (B depends on A)
2. `B → C` (C depends on B)

Where:

- `A` is the Primary Key.
- `B` and `C` are non-key attributes.
    

The chain looks like this: **Primary Key → Non-Key Attribute 1 → Non-Key Attribute 2**. The problem is that Non-Key Attribute 2 should depend _only_ on the Primary Key.


---

### Why is it called 'transitive' ?


That's an excellent question that gets to the heart of the term. It's called "transitive" because the concept is borrowed directly from the **Transitive Property** in mathematics and logic.

The word itself comes from the Latin _transire_, meaning "to go across" or "to pass through."

### The Transitive Property in Math

In its most basic form, the transitive property states:

If A is related to B, and B is related to C, then A is related to C.

The relationship "passes through" the middle element (B). The classic example is with numbers:

- If `a > b`
- And `b > c`
- Then it must be true that `a > c`

The property of "greater than" is transferred from `a` to `c` _through_ `b`.

### How This Applies to Databases

A transitive dependency is the exact same pattern applied to database attributes.

Let's look at the dependency chain again:

`Primary Key (A) → Non-Key Attribute (B) → Non-Key Attribute (C)`

Here's how it mirrors the mathematical property:

1. **A is related to B**: The Primary Key determines the value of a non-key attribute.
    
    - `Course_ID → Instructor_ID`
2. **B is related to C**: That non-key attribute determines the value of another non-key attribute.
    
    - `Instructor_ID → Instructor_Name`
3. **Therefore, A is related to C**: The Primary Key indirectly determines the final non-key attribute.
    
    - `Course_ID → Instructor_Name`

The functional dependency of `Instructor_Name` on `Course_ID` isn't direct. It has to **pass through** the intermediate attribute, `Instructor_ID`.

So, it's called **transitive** because the dependency is transferred across an intermediary attribute, just like the transitive property in logic.


---


### Transitive


The word **"transitive"** fundamentally describes a relationship or action where something is **transferred or passed across from one thing to another, often through an intermediary.**

Its meaning becomes clearer when looking at its two most common contexts: grammar and mathematics.

### 1. In Grammar (Transitive Verbs)

This is the most common use of the word. A verb is **transitive** if the action of the verb passes across to a direct object. The direct object is the thing that receives the action.

**You can ask "What?" or "Whom?" after the verb to see if it's transitive.**

**Examples:**

- **She reads the book.**
    
    - _Verb:_ reads
    - _Action:_ Reading
    - The action of reading is transferred directly to the **book**. (She reads _what_? The book.)
- **The dog chased the cat.**
    
    - _Verb:_ chased
    - The action of chasing passes across to the **cat**. (The dog chased _what_? The cat.)
- **He wrote a letter.**
    
    - _Verb:_ wrote
    - The action of writing is received by the **letter**. (He wrote _what_? A letter.)

The opposite is an **intransitive verb**, where the action does not pass to an object.

- **He sleeps.** (You can't ask "He sleeps what?")
- **The sun rises.**
- **She arrived late.**

---

### 2. In Mathematics and Logic (Transitive Property)

This context is the origin for the term "transitive dependency" that we discussed. The **transitive property** states that if a relationship holds between a first and second element, and also between that second and a third element, then it must hold between the first and third.

The relationship "passes through" the middle element.

**Examples:**

- **Equality (`=`):** If `A = B` and `B = C`, then you know that `A = C`.
    
- **Inequality (`>` or `<`):** If `Sarah is taller than John`, and `John is taller than Mike`, then it must be true that `Sarah is taller than Mike`.
    
- **Family Ancestry:** If you are a descendant of your mother, and your mother is a descendant of your grandmother, then you are a descendant of your grandmother.
    

---

### 3. In Databases (Transitive Dependency)

As we discussed, this is a direct application of the logical property. A dependency is transitive if it's indirect and passes through another attribute.

**Example:**

- If `Course_ID → Instructor_ID`
- And `Instructor_ID → Instructor_Name`
- Then the dependency of `Instructor_Name` on `Course_ID` is **transitive** because it passes through the `Instructor_ID`.

In all these cases, the core idea remains the same: a property, action, or relationship is transferred across an intermediary point.


---


### Transferred

- past participle of 'transfer'

In the context we've been discussing, **"transferred"** means that an action, quality, or effect is **moved or passed on** from a source to a destination.

Think of it as something being conveyed from one point to another.

Here are a few examples to illustrate the meaning:

**1. Physical Transfer:**

- You **transfer** money from your savings account to your checking account. (The money moves from one place to another).
    
- He **transferred** the file from his computer to a USB drive. (The data is moved from one location to another).

**2. Abstract Transfer (like in our examples):**

- **In Grammar:** In the sentence "He kicked the ball," the action of "kicking" is **transferred** from the subject ("He") to the object ("the ball"). The ball is what receives the action.
    
- **In Logic:** In the statement "If A > B and B > C," the property of being "greater than" is **transferred** from A to C. The quality doesn't just apply to A and B; it's passed on to create a relationship between A and C.
    

So, when we say a dependency is "transferred," it means the relationship or effect moves across from one attribute to another through an intermediary.



---








---


## BCNF


### What is BCNF?

A table is in **Boyce-Codd Normal Form (BCNF)** if, for every one of its non-trivial functional dependencies, `X → Y`, **`X` must be a superkey**.

Let's quickly break that down:

- **Functional Dependency (`X → Y`)**: This means that a value or set of values `X` determines the value of `Y`. For example, `StudentID → StudentName`.
- **Superkey**: A set of one or more columns that can uniquely identify every row in a table. A candidate key is a minimal superkey.
    

The rule for BCNF is simpler and stricter than 3NF. While 3NF allows for some exceptions (specifically, when the right side of the dependency is a prime attribute), BCNF insists that the left side **must always** be a superkey. For this reason, it's sometimes called "3.5NF".

---



A **non-trivial functional dependency** is a dependency that actually tells you something new and meaningful about the relationships in your data.

To understand it, it's easiest to first define its opposite:

### Trivial Functional Dependency

A functional dependency `A → B` (meaning "A determines B") is **trivial** if B is a subset of A.

In simple terms, it's an obvious dependency where you are just stating that a set of attributes determines itself or a part of itself.

**Examples:**

- `{StudentID} → {StudentID}`
- `{StudentID, StudentName} → {StudentName}`

These are trivial because they don't provide any new information. Of course, if you know a student's ID and name, you know their name. They are always true by definition and are ignored during normalization because they can't cause data anomalies.

### Non-Trivial Functional Dependency

A functional dependency `A → B` is **non-trivial** if B is **not** a subset of A.

These are the dependencies that matter for database design because they represent a real business rule or constraint that can lead to redundancy if not handled correctly.

**Examples:**

- `{StudentID} → {StudentName}` _This is non-trivial because `StudentName` is not part of `StudentID`. This dependency tells us a meaningful rule: a specific ID is assigned to a specific person._
    
- `{CourseID} → {InstructorID}` _This is non-trivial because `InstructorID` is not part of `CourseID`. It tells us a specific course is taught by a specific instructor._
    

**Why does this distinction matter?**

When you see a rule like BCNF stating, "for every **non-trivial functional dependency** `A → B`, `A` must be a superkey," it's specifically telling you to ignore the obvious, trivial ones and focus only on the meaningful dependencies that actually structure your data and could potentially cause problems.




---
### Example: A Table in 3NF but Not BCNF

The classic example involves students, the subjects they take, and the professors who teach them.

Let's establish a couple of rules for our scenario:

1. A student can enroll in multiple subjects.
2. For any given subject, a student is taught by only one professor.
3. Each professor teaches only one subject.
4. Multiple professors can teach the same subject (e.g., Prof. Smith and Prof. Jones both teach 'History').

Here's our table structure:

**Student_Professor_Subject Table**

|StudentID|ProfessorID|Subject|
|:--|:--|:--|
|S101|P20|History|
|S101|P21|Math|
|S102|P20|History|
|S103|P22|Chemistry|
|S104|P21|Math|

#### Analyzing the Dependencies and Keys

In this table, we have two **candidate keys** that can uniquely identify any row:

- `{StudentID, Subject}`
- `{StudentID, ProfessorID}`

The table also has another important functional dependency:

- `ProfessorID → Subject` (Because each professor teaches only one subject).

This table **is in 3NF**. It doesn't have transitive dependencies on non-prime attributes.

However, it **is not in BCNF**. The dependency `ProfessorID → Subject` violates the BCNF rule because its left side, `ProfessorID`, is **not a superkey**. It cannot, on its own, uniquely identify a row in the table (e.g., Professor P20 is listed for both student S101 and S102).

This violation can lead to anomalies. For instance, if Professor P22 leaves and we delete their record, we lose the information that 'Chemistry' is a subject.

### Decomposing to BCNF

To fix this, we decompose the table into two separate tables that both satisfy BCNF.

**1. Student_Professor Table** This table links each student to their professor for a course. The candidate key is `{StudentID, ProfessorID}`.

|StudentID|ProfessorID|
|:--|:--|
|S101|P20|
|S101|P21|
|S102|P20|
|S103|P22|
|S104|P21|


**2. Professor_Subject Table** This table stores which subject each professor teaches. The primary key is `ProfessorID`.

|ProfessorID|Subject|
|:--|:--|
|P20|History|
|P21|Math|
|P22|Chemistry|


Now, both tables are in BCNF. In the `Professor_Subject` table, the dependency is `ProfessorID → Subject`, and `ProfessorID` is the primary key (and therefore a superkey).

---

### BCNF vs. 3NF: The Key Difference

|Aspect|3rd Normal Form (3NF)|Boyce-Codd Normal Form (BCNF)|
|:--|:--|:--|
|**Main Rule**|For a dependency `X → Y`, either **`X` is a superkey**, OR **`Y` is part of a candidate key** (a prime attribute).|For a dependency `X → Y`, **`X` must be a superkey**. No exceptions.|
|**Strictness**|Less strict.|More strict. Every table in BCNF is also in 3NF.|
|**Anomalies**|Eliminates most data anomalies.|Eliminates all anomalies related to functional dependencies.|
|**Use Case**|Commonly used as it always preserves all dependencies.|The ideal form, but achieving it might sometimes mean losing a functional dependency from the design.|
