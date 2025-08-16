- [ ] Relational Data Model Fundamentals ➕ 2025-08-05 

### ||Q|| : Interviewer introduces the topic and asks for the definition of a Relation (Table)

Hello, I'm the interviewer for this backend development role. Today, we'll dive into Relational Data Model Fundamentals as part of assessing your knowledge. This is a key area for backend work, especially when designing databases and ensuring data integrity.

To start, let's go through some basics step by step. I'll ask questions based on the points you provided, and we can chain from there. If your answers need clarification or improvement, I'll guide you—like suggesting better ways to phrase things or correcting directions to help you learn.

**First question: Can you clearly define what a `Relation (Table)` is in the relational data model? Please explain it in your own words, and feel free to give an example if it helps.**

---

### ||A|| : Answering about the definition of a Relation

Relation means some connection between entity. For example, There's student entity, and class entity, and for a record inside a class entity (table), multiple records of a student entity is included.

---

### ||Q|| : Interviewer corrects the definition of Relation and asks about Tuple (Row)

Thank you for your response. I appreciate you trying to explain it with an example, but I think there might be some confusion here. Your answer seems to focus on _relationships between entities_ (like how a class might connect to multiple students), which is more about how tables link via keys in a database schema. That's an important concept, but it's not quite what I asked for.

#### Clarification on 'Relation'

What I'm specifically asking about is the definition of a **Relation** in the relational data model—essentially, what it means at its core. To clarify, a **Relation** is not about connections between entities; it's the formal term for what we commonly call a **table** in a database. It's a collection of data organized into rows and columns, where each row represents a unique record, and each column represents a specific attribute or property.

For example, imagine a "Students" relation (table) with columns like `StudentID`, `Name`, and `Age`. Each row would be a specific student's data, and the whole thing is a set of such rows following the same structure.

A better way to phrase your answer next time could be something like: "A **Relation** is a table in the relational model, consisting of a set of tuples (rows) that share the same attributes (columns), with each tuple representing a unique instance of data."

#### Expression Check

That said, your English is clear, but the example felt a bit ambiguous—did you mean that a class record includes multiple student records directly in the same table? If so, that might describe a denormalized structure, but let's confirm: can you rephrase your example to make sure I understand what you meant?

**Moving on to build on this, let's chain to the next fundamental: Can you define what a `Tuple (Row)` is in the context of a relation? Explain it in your own words, and maybe tie it back to the Students table example I just gave.**

---

### ||A|| : Answering about the definition of a Tuple

Tuple means a record, which constitutes a Relation ( table ). tuples inside a Relation share a same attributes.

---

### ||Q|| : Interviewer refines the definition of Tuple and asks about Attribute (Column)

Thank you for your response. That's a solid start—you're on the right track by noting that a tuple is a record that makes up a relation and that tuples share the same attributes.

#### Feedback on 'Tuple' Definition

However, there are a couple of small issues: it's a bit concise, and there's a minor grammar tweak ("a same attributes" should be "the same attributes"). To make it even stronger for a future interview, you could expand it like this: "A **tuple** is a single row or record in a relation (table), representing a specific instance of data. All tuples within the same relation share the exact same set of attributes (columns), ensuring consistency across the table."

#### Expression Check

Your English is clear overall, but just to confirm I understood correctly: when you say "tuples inside a Relation share a same attributes," do you mean that every tuple has values for the exact same columns defined in the table? If that's not what you intended, could you rephrase it for me?

**Great, let's keep chaining forward. Now, building on relations and tuples, can you define what an `Attribute (Column)` is in the relational data model? Explain in your own words, and perhaps give an example using the Students table we discussed earlier (with columns like `StudentID`, `Name`, and `Age`).**

---

### ||A|| : Answering about the definition of an Attribute

Attributes are values that constitutes a record (instance of a Relation). For example, Student can have attributes like name, class, grades.

---

### ||Q|| : Interviewer corrects the definition of Attribute and asks about Domain

Thank you for your response. I see where you're coming from with that definition, but it's heading in the wrong direction—it's not quite what I asked for. You seem to be describing the _values_ that fill in a record (like "John Doe" for name or "A+" for grades), but an **attribute** isn't the value itself. Instead, it's the named column or field that defines what kind of data can be stored in that position across all tuples in the relation.

#### Correction on 'Attribute' Definition

To explain what I'm looking for: An **attribute** is a column in a relation (table), representing a specific property or characteristic that applies to every tuple (row). It has a name and a domain of possible values. For example, in a Students table, **Name** is an attribute (column) that holds string values like "Alice" or "Bob" for each student row; **Class** could be another attribute holding values like "Math 101"; and **Grades** might hold numeric or letter values like 95 or "A".

A better way to answer this next time could be: "An **attribute** is a column in a relation, defining a particular property of the data, such as 'Name' or 'Age' in a Students table. Each attribute has a domain specifying the allowable values, and every tuple in the relation provides a value (or NULL) for each attribute."

#### Expression Check

Your English is mostly clear, but the phrasing "Attributes are values that constitutes a record" felt a bit ambiguous and grammatically off ("constitutes" should be "constitute" since "values" is plural). Did you mean that attributes are the individual data points making up a row, like the specific name or grade for one student? If so, that's closer to describing cell values, not the attribute itself. Could you rephrase your answer to confirm?

**Alright, let's chain to the next fundamental to keep building your understanding. Can you define what a `Domain` is in the context of an attribute? Explain in your own words, and tie it to one of the attributes from the Students table example (e.g., what might the domain be for the "Age" attribute?).**

---

### ||Q|| & ||A|| : Re-attempting the definition of an Attribute

I'd like to repeat the answer about what the attributes is before go beyound.

so, Attributes are columns of tuples which defines characteristics of tuples and domain of value that column can have.

---

### ||Q|| : Interviewer provides feedback on the revised Attribute definition and re-asks about Domain

Thank you for circling back and revising your answer on attributes—that shows great initiative in an interview setting, as it demonstrates you're reflecting and improving in real-time. Let's evaluate this new definition you provided: "Attributes are columns of tuples which defines characteristics of tuples and domain of value that column can have."

#### Feedback on Revised 'Attribute' Definition

This is a good improvement from your previous response, as you're now correctly identifying attributes as columns and linking them to characteristics and domains. However, it's still slightly off-track and could be clearer. Attributes aren't exactly "columns of tuples"—they're the named columns defined at the level of the entire relation (table), which then apply to every tuple (row) in it.

A stronger way to phrase this for next time could be: "An **attribute** is a column in a relation (table) that defines a specific characteristic or property shared by all tuples. Each attribute has a name and a domain, which specifies the set of allowable values for that column across all rows."

#### Expression Check

Also, some grammar tweaks: "defines" should be "define" (since "columns" is plural), and "domain of value that column can have" would be smoother as "the domain of values that the column can hold." The mention of domain is spot-on, though, and we'll dive into that next.

Your English is understandable, but the phrasing felt a bit ambiguous—did you mean that attributes are parts of individual tuples, or that they define the structure for the whole table? If it's the former, remember that attributes are structural (like blueprint fields), while the values in a specific tuple are instances of those attributes. Could you confirm or rephrase to make sure I got it right?

**Now that we've solidified attributes, let's proceed as you requested. Can you define what a `Domain` is in the context of an attribute? Explain in your own words, and tie it to one of the attributes from the Students table example (e.g., what might the domain be for the "Age" attribute?).**

---

### ||A|| : Answering about the difference between Superkey and Candidate Key

either {studentID} and {studentID, name} can be supey key, but candiate key is {StudentID} because even if we exclude name from set of column, we can stil distinguish tuples uniquely.

---

### ||Q|| : Interviewer confirms understanding of Superkey/Candidate key and asks about {Email}

Excellent reasoning! You've correctly identified that `{StudentID}` is a candidate key because it's minimal—removing any attribute (well, there's only one) would break the uniqueness property. And you're absolutely right that `{StudentID, Name}` is a superkey but NOT a candidate key because it contains a redundant attribute (Name) that can be removed while still maintaining uniqueness.

#### Feedback on Superkey vs. Candidate Key

However, I noticed you missed something from my example. I also mentioned that `{Email}` can uniquely identify students. Given your understanding of minimality, what would you say about `{Email}`?

#### Expression Check

Also, small grammar note for future interviews: "supey key" should be "superkey," and "candiate key" should be "candidate key." Your logic is spot-on though!

Your English is clear, and I understand your reasoning perfectly. Just to confirm: when you say "even if we exclude name from set of column, we can stil distinguish tuples uniquely," you mean that StudentID alone is sufficient for uniqueness, making Name redundant in that combination, correct?

**Would `{Email}` also be a candidate key? Now let's chain to the next concept: `Primary Key`. Can you define what a primary key is and explain how it relates to candidate keys? In our Students example, if we have candidate keys `{StudentID}` and `{Email}`, how do we decide which one becomes the primary key?**

---

### ||Q|| : Questioning if an Email can be a candidate key due to potential duplicates

I don't think Email can be candidate key - Email can be duplicated, while studentid is not. I think.

---

### ||Q|| : Interviewer clarifies the theoretical constraints and re-asks about Email as a Candidate Key

That's a great observation about real-world considerations! You're absolutely right to think critically about whether email addresses could be duplicated in practice. However, let me clarify the scenario I presented.

#### Clarification on Theoretical Constraints

In my example, I specifically stated that `{Email}` can uniquely identify students as a given constraint for this theoretical table design. This means that in this particular `Students` table, we've designed it so that email addresses are unique and cannot be duplicated—perhaps enforced by a unique constraint in the database.

#### Real-world vs. Theoretical Design

In real-world scenarios, you're correct that emails could potentially be duplicated (maybe students share family emails, or there are data entry errors), but for this exercise, we're assuming the database design ensures email uniqueness.

Your critical thinking about practical constraints is excellent for a backend developer—that's exactly the kind of real-world consideration that matters when designing systems. But for now, let's work with the theoretical constraint I provided.

**So given that assumption—that both `{StudentID}` and `{Email}` can uniquely identify students without duplication—would `{Email}` also be a candidate key? And then we can move on to discussing primary keys.**

---

### ||A|| : Confirming {Email} can be a candidate key if uniqueness is guaranteed

{StudentId, Email} is redundant because {studentid} and {email}alon can be super key. so I think {email} can be candidate key if it can define each tuples uniquely.

---

### ||Q|| : Interviewer confirms the answer and asks to define Primary Key

Perfect! You've got it exactly right. You correctly identified that:

- `{Email}` is indeed a candidate key because it can uniquely identify each tuple by itself (minimal superkey)
    
- `{StudentID}` is also a candidate key for the same reason
    
- `{StudentID, Email}` would be redundant because either attribute alone is sufficient for uniqueness
    

#### Confirmation of Understanding

Your reasoning is spot-on—you understand the concept of minimality perfectly.

#### Expression Check

Small grammar note for future interviews: "email alon" should be "email alone," and "define each tuples uniquely" should be "identify each tuple uniquely." But your logic is crystal clear!

**Great! Now we have two candidate keys: `{StudentID}` and `{Email}`. This brings us perfectly to the next concept: `Primary Key` - Can you define what a primary key is and explain how it relates to candidate keys? In our Students example where we have candidate keys `{StudentID}` and `{Email}`, how do we decide which one becomes the primary key? What factors might influence that decision? Think about this: if a relation can have multiple candidate keys, why do we need to choose one as the primary key?**

---

### ||A|| : Expressing confusion about the difference between a Primary Key and a Candidate Key

Because there is no need for multiple candidate key, which is redundant, and it can cause some confusion - like when we trying to distinguish tulpes, we can choose both candidate key (student id, and email). but ,,, um, tbh, I can't find accurate reason why we should choose which one as primary key. since I heard that primary key is a set of column that can distinguish tuples, just like candidate key. I can't find any difference between primary key and candidate key.

---

### ||Q|| : Interviewer explains the distinction between Primary and Candidate Keys and asks about Foreign Keys

Thank you for your honest response—that's a strong quality in an interview, as it shows self-awareness and openness to learning. You're correct that both primary keys and candidate keys serve to uniquely identify tuples, and there's no strict "need" for multiple candidate keys in a purely theoretical sense. However, your answer veered a bit off-track by suggesting multiple candidate keys are inherently redundant or confusing; actually, having multiple candidate keys is common and useful (e.g., for alternative unique identifiers like `StudentID` or `Email`), and the primary key is simply the one we _choose_ from them for specific database purposes like enforcing integrity.

#### Clarifying the Difference: Primary vs. Candidate Key

To clarify the key difference (which you mentioned struggling with): A **candidate key** is any minimal superkey that could potentially serve as the unique identifier—there can be multiple per table, and their attributes can sometimes allow NULL values (depending on the design). A **primary key**, on the other hand, is the _selected_ candidate key that we designate as the official unique identifier for the table—there's only one per table, it cannot contain NULL values, and it's used for things like linking to foreign keys in other tables. The reason we choose one is for consistency: it provides a standard way to reference records across the database, avoids ambiguity in relationships, and ensures data integrity (e.g., no duplicates or NULLs in that key).

A better way to phrase this next time could be: "A primary key is one chosen candidate key that uniquely identifies each tuple in a relation, with no NULL values allowed. It relates to candidate keys as the 'official' selection from among them, ensuring a single, reliable identifier for referential integrity and database operations. We pick it to standardize how we reference and link data, reducing potential confusion in larger schemas."

#### Expression Check

Your English is mostly clear, but some parts felt a bit ambiguous—like when you said "when we trying to distinguish tulpes, we can choose both candidate key (student id, and email)." Did you mean that having multiple options could lead to indecision on which to use for queries? If so, that's a fair point in practice, but the primary key resolves that by being the designated one. Could you rephrase that part to confirm?

#### Application to the Example

Now, applying this to our Students example (with candidate keys `{StudentID}` and `{Email}`): We might choose `{StudentID}` as the primary key because it's likely more stable (emails can change, but IDs are assigned permanently), simpler (a single integer vs. a longer string), and better for performance in joins. Factors influencing the decision often include:

- **Stability**: Does the value change over time?
    
- **Simplicity**: Is it concise and easy to index?
    
- **Uniqueness guarantee**: How reliably unique is it in the real world?
    
- **NULL handling**: Primary keys forbid NULLs, so it must always have a value.
    

Does that make sense? **What would you choose as the primary key in this case, and why? Let's chain to the next key concept: `Foreign Key`. Can you define what a foreign key is and explain how it relates to primary keys? For example, if we have a `Courses` table with a foreign key referencing the `Students` table's primary key, what does that enforce?**

---

### ||Q|| : Asking for clarification on whether non-primary candidate keys can have NULL values

ok, so primary key is selected for clarification of a tuple, and for the reference from other tables, we designated it to not have null value, while candidate key - not primary key can have null values. am I right ?

#### Refined Question

Based on your explanation, am I correct in understanding that a primary key is chosen for clarity and referencing, and must not be NULL, while other candidate keys are allowed to have NULL values?

---

### ||Q|| : Interviewer corrects the misconception about NULL values in Candidate Keys and re-introduces the Foreign Key topic

Good effort at summarizing the key concepts! You're on the right track with most of it, but there's one important clarification needed.

You're correct that:

- ✅ Primary key is _selected_ from candidate keys for clarification and reference from other tables
    
- ✅ Primary key _cannot have NULL values_
    

#### Correction: Candidate Keys Cannot Have NULLs

However, there's a misconception in your statement: **Candidate keys also cannot have NULL values.** This is a common point of confusion. The sources confirm that candidate keys must be unique and cannot contain NULL values, just like primary keys.

#### Key Differences Summarized

- **Quantity**: A table can have multiple candidate keys, but only one primary key.
    
- **Selection**: The primary key is the _chosen_ candidate key that becomes the official identifier.
    
- **Purpose**: The primary key is used for referential integrity, indexing by default, and foreign key references.
    
- **Status**: All primary keys are candidate keys, but not all candidate keys become the primary key.
    

So a better way to phrase it would be: "A primary key is the selected candidate key used as the official unique identifier for a table, ensuring no NULL values and serving as the reference point for foreign keys from other tables. Both primary keys and candidate keys must be unique and cannot contain NULLs."

#### Expression Check

Your English is clear, and I understand your reasoning. Just to confirm: when you said "candidate key - not primary key can have null values," did you mean attributes that are _not_ candidate keys can have NULLs? That would be correct. Could you clarify that distinction?

**Now let's move forward to `Foreign Keys` as we planned. Can you define what a foreign key is and explain how it works? For example, if we have an `Enrollments` table that references our `Students` table, what would the foreign key look like and what does it enforce?**

---

### ||Q|| : Asking about scenarios where no single column can be a primary key

Then, if a relation can't distinguish a tuple only with one column, it can't have a primary key, only it can have candidate key ? or we should add some column which will acts as primary key ?

#### Refined Question

If a relation cannot be uniquely identified by a single column, does that mean it cannot have a primary key and can only have a candidate key? Or should we add a new column to act as a primary key?

---

### ||Q|| : Interviewer explains Composite and Surrogate Keys and re-asks about Foreign Keys

That's an excellent question that shows you're thinking deeply about the practical implications! Let me clarify this important point.

You have a slight misconception here. **A primary key doesn't have to be a single column—it can be a _composite primary key_ made up of multiple columns.** So if no single column can uniquely identify tuples, you can create a primary key using a combination of columns.

#### Clarification on Composite and Surrogate Keys

Here are the options when no single column provides uniqueness:

1. **Composite Primary Key**: Combine multiple existing columns to form a primary key.
    
    - **Example**: If we have a `CourseEnrollments` table where neither `StudentID` nor `CourseID` alone is unique (since students can take multiple courses and courses can have multiple students), we can use `{StudentID, CourseID}` together as a composite primary key.
        
2. **Add a Surrogate Key**: Create a new column (often an auto-incrementing integer) to serve as the primary key.
    
    - **Example**: Add an `EnrollmentID` column that auto-increments for each new enrollment record.
        

#### Necessity of a Primary Key

Every relation should have a primary key—it's a fundamental requirement of the relational model. The sources confirm that tables without primary keys can lead to data integrity issues and make it difficult to reference records reliably.

#### Expression Check

Your English is clear, and I understand your question perfectly. Just to confirm: when you asked "if a relation can't distinguish a tuple only with one column," did you mean situations where no single column provides uniqueness? If so, that's exactly the scenario where composite keys or surrogate keys come into play.

**Now let's move forward to `Foreign Keys`. Can you define what a foreign key is and explain how it creates relationships between tables? For example, if our `CourseEnrollments` table has a foreign key referencing the `Students` table, what does that enforce and why is it important?**

---

### ||A|| : Answering about the definition and purpose of a Foreign Key

Foreign key is the key that refers / points to column to another relation, and by that Foreign key creates Relationship between to Relation. in your exmaple, a column 'studentid' should refer to 'studentId' column in Student table, and this enforce the column 'studentID' in courseEnroolement table should exsist on studentid column in Student table. It's important because purpose of foreign key is to follow values of a referenced column. (?). I can't find proper expression for it.

---

### ||Q|| : Interviewer refines the Foreign Key definition and asks about referential integrity actions

Excellent work! You've captured the essential concept of foreign keys very well. Your understanding is solid—you correctly identified that a foreign key creates relationships between tables by referencing another table's column, and that it enforces referential integrity.

Let me help you find that proper expression you were looking for. The term you're seeking is **"referential integrity"**.

#### Refining the Definition of 'Foreign Key'

Here's how to phrase it more precisely:

"A **foreign key** is a column (or set of columns) in one table that references the primary key of another table, creating a relationship between the two tables. It enforces **referential integrity**, meaning that any value in the foreign key column must exist in the referenced table's primary key column. This prevents orphaned records and maintains data consistency."

Your example is perfect: the `StudentID` column in the `CourseEnrollments` table must reference valid `StudentID` values that exist in the `Students` table.

#### The Importance of Referential Integrity

**Why it's important** (the expression you were looking for): Foreign keys maintain referential integrity by ensuring that relationships between tables remain consistent and prevent "orphaned records"—rows that reference non-existent data.

#### Expression Check

Small grammar note: "courseEnroolement" should be "CourseEnrollment," and "exsist" should be "exist." Your logic is crystal clear though!

Your English is mostly clear. When you said "purpose of foreign key is to follow values of a referenced column," did you mean that foreign keys ensure the values stay synchronized with the referenced table? That's exactly right—they maintain referential integrity.

**Now let's explore what happens when we try to violate these constraints. Can you explain what would happen if we tried to:**

- **Insert a `StudentID` in `CourseEnrollments` that doesn't exist in the `Students` table?**
    
- **Delete a student from the `Students` table who has enrollments in `CourseEnrollments`?**
    
- **What are some ways databases can handle these situations?**
    

---

### ||Q|| : Asking if a foreign key must always reference a primary key

But, didn't referenced column of foreign key have to primary key ? I saw some of it when I worked at SI company. There was a foreign key which is not a primary key of that table, and referenced key wasn't the primary key of its own table neither. Is this my imagination or confusion ?

#### Refined Question

Doesn't the column referenced by a foreign key have to be a primary key? I recall from a previous job seeing a foreign key that referenced a column that was not the primary key in its table. Was I mistaken?

---

### ||Q|| : Interviewer clarifies that Foreign Keys can reference any UNIQUE constraint, not just Primary Keys

Thank you for sharing your real-world experience from the SI company—that's valuable context and shows you're drawing from practical knowledge, which is great in a backend interview. No, it's not your imagination or confusion; you're recalling something that's technically possible in many database systems. Let me clarify this accurately based on standard relational database principles.

#### Can Foreign Keys Reference Non-Primary Keys?

Yes, a foreign key does **not** have to reference a primary key. Instead, it can reference **any column (or set of columns) that has a UNIQUE constraint** in the parent table. This ensures the referenced values are unique, maintaining referential integrity without needing the full status of a primary key.

Why this works: The `UNIQUE` constraint guarantees no duplicates in the referenced column, similar to a primary key but without the additional requirements (like being the "official" identifier or forbidding NULLs entirely—though primary keys always forbid NULLs).

Example: Suppose you have a `Users` table with a `Username` column that's `UNIQUE` but not the primary key (maybe the primary key is an auto-incrementing `UserID`). Another table like `Orders` can have a foreign key on its `OrderedByUsername` column referencing `Users.Username`, as long as `Username` is `UNIQUE`.

#### Caveats and Best Practices

Not all databases enforce this strictly (e.g., some allow it only if explicitly supported), and it's generally best practice to reference primary keys for simplicity and performance. Referencing non-primary keys can lead to more complex designs and potential issues if the `UNIQUE` column allows NULLs (which might not be permitted in some systems).

In your SI company example, if the referenced column had a `UNIQUE` constraint (even if not primary), that foreign key setup would be valid. If it didn't have `UNIQUE`, it might have been a non-standard or unenforced relationship, which could risk data inconsistencies.

#### Expression Check

A better way to phrase your question next time could be: "I've seen foreign keys referencing non-primary keys in practice—does the referenced column just need to be `UNIQUE`, or must it always be a primary key?"

Your English is clear overall, but "referenced key wasn't the primary key of its own table neither" felt a bit ambiguous—did you mean the foreign key itself wasn't primary in its table, and the referenced column wasn't primary in the parent table? If that's not it, could you rephrase?

---

### Questions

1. In your own words, what is the difference between a `Relation` and a relationship between tables?
    
2. Explain the hierarchy of keys: Superkey, Candidate Key, and Primary Key. Why is `{StudentID, Name}` a superkey but not a candidate key for a `Students` table where `{StudentID}` is unique?
    
3. A table has two candidate keys: `{EmployeeID}` (an integer) and `{SocialSecurityNumber}` (a string). Which would you choose as the primary key, and what are three factors that would influence your decision?
    
4. Can a primary key consist of multiple columns? If so, what is this called and when would you use it?
    
5. What is "referential integrity" and how do foreign keys enforce it?
    
6. Is it possible for a foreign key to reference a column that is not a primary key? If so, what condition must that referenced column meet?



----



### ||A|| : Explaining Referential Integrity and Data Consistency

#### Referential Integrity in Databases

Referential integrity ensures that relationships between tables in a relational database remain valid and consistent. It requires that every foreign key value in one table must either match an existing primary key value in the referenced table or be null (if the foreign key column allows null values). This core principle prevents the creation of "orphaned" records, such as an order that references a customer who does not exist in the customer table.

For example, if a user attempts to delete a customer record, the database's referential integrity constraints will dictate the outcome. The database might be configured to:

- **Cascade the delete:** Automatically delete all orders associated with that customer.
    
- **Block the delete:** Prevent the deletion of the customer as long as related orders exist.
    

Ultimately, referential integrity maintains data accuracy by enforcing these foreign key constraints.

#### Data Consistency in Databases

Data consistency means that data remains accurate, valid, and uniform across the entire database, adhering to all predefined rules, constraints, and transactions. Within the context of ACID properties, the 'C' (Consistency) guarantees that any transaction will bring the database from one valid state to another, preserving its integrity.

For instance, consider a bank transfer. If the system fails midway through the transaction, the consistency property ensures that any partial changes are rolled back. This prevents data mismatches, such as money being deducted from one account without being credited to another. By preventing logical errors like duplicate entries or contradictory information, data consistency supports reliable queries and trustworthy decision-making.

---

### Questions

1. In your own words, what is the primary purpose of referential integrity?
    
2. What is an "orphaned record"? Provide an example.
    
3. Explain the relationship between a foreign key and a primary key in the context of referential integrity.
    
4. What is data consistency, and how does it relate to the ACID properties of a transaction?
    
5. Using the bank transfer example, explain how a database ensures consistency if a transaction fails.
    
6. Can you have data consistency without referential integrity? Why or why not?