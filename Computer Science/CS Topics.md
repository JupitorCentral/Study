
- [ ] CS Topics ➕ 2025-08-02 


### Database
---

\[\[Prioritized]]

##### Core Relational Model and SQL Mastery

- **1. Relational Data Model Fundamentals**
    
    - **Definitions**: Be able to clearly define:
        - **Relation (Table)**: A collection of data organized into rows and columns.
        - **Tuple (Row)**: A single record in a table.
        - **Attribute (Column)**: A named column representing a property.
        - **Domain**: The set of allowed values for an attribute.
    - **Keys (Critical)**: Understand and differentiate:
        - **Superkey**: Uniquely identifies a tuple.
        - **Candidate Key**: A minimal superkey. A relation can have multiple candidate keys.
        - **Primary Key**: The chosen candidate key to uniquely identify tuples, often underlined. Every relation schema should have one.
        - **Foreign Key**: Attribute(s) in one table that refer to the primary key of another table, linking them and enforcing referential integrity. Be aware of `ON DELETE` and `ON UPDATE` actions like `CASCADE` and `SET NULL`.
        - **Prime vs. Nonprime Attributes**: An attribute is prime if it's part of _any_ candidate key.
    - **NULL Values**: Understand their meanings (unknown, not applicable, value withheld) and that SQL does not distinguish between them.
- **2. SQL (Structured Query Language)**
    
    - **DDL (Data Definition Language)**:
        - `CREATE TABLE`: Know how to define tables with attribute names, data types (`INT`, `VARCHAR`, `DATE`, `TIME`), and constraints: `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, and `CHECK`.
        - `ALTER TABLE` (add/drop columns).
        - `DROP TABLE`.
    - **DML (Data Manipulation Language)**: This requires **hands-on practice**.
        - **`SELECT` (Basic Retrieval)**: Master the `SELECT-FROM-WHERE` structure. Understand projection (what columns to show) and selection (what rows to filter).
        - **Operators**: Basic comparison (`=`, `<`, `<=`, `>`, `>=`, `<>`), `LIKE` (with `%`, `_`, `ESCAPE`), `BETWEEN`.
        - **Aggregate Functions**: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`. Understand `DISTINCT` within `COUNT`.
        - **`GROUP BY` and `HAVING`**: Use `GROUP BY` to partition tuples and `HAVING` to filter these groups based on aggregate conditions.
        - **Joins**: Focus on `INNER JOIN` logic, either with `WHERE` conditions or the `JOIN` keyword. Understand the need for aliases when joining a table to itself.
        - **Nested Queries**: Understand `IN`, `EXISTS`, and `NOT EXISTS` and their use with subqueries, especially correlated ones.
        - `INSERT`, `UPDATE`, `DELETE`: General understanding of their purpose.
    - **Views**: Know their basic purpose (simplified access, security).
    - **Prioritize**: Writing and understanding `SELECT` statements with various clauses. Practice joins and subqueries.

##### Database Design Principles & Performance Basics

- **3. Database Design Process & ER Model (Conceptual Design)**
    
    - **Design Process Overview**: Know the phases: requirements, conceptual design, logical design, physical design.
    - **ER Model (Conceptual Modeling Tool)**:
        - **Purpose**: Used for high-level conceptual design, independent of a specific DBMS.
        - **Key Concepts**:
            - **Entities, Attributes, Relationships**: How real-world concepts are mapped.
            - **Attribute Types**: Simple, composite, multivalued (how they differ).
            - **Relationship Types**: Binary vs. Ternary (higher degree).
            - **Structural Constraints**: Crucially, **Cardinality Ratios** (1:1, 1:N, M:N) and **Participation Constraints** (total/existence dependency vs. partial). Be able to draw and interpret their common notation (1, M, N, single/double lines).
            - **Weak Entity Types**: Identify a weak entity, its owner, identifying relationship, and partial key.
- **4. Normalization Theory (Relational Schema Quality)**
    
    - **Informal Design Guidelines**: Understand the goals: clear semantics, reducing redundancy, minimizing NULL values, and avoiding spurious tuples.
    - **Update Anomalies**: Be able to explain insertion, deletion, and modification anomalies and _why_ they are problematic. This is a direct consequence of poor normalization.
    - **Functional Dependencies (FDs)**: Understand X → Y means X uniquely determines Y. FDs are properties of the schema, not just a specific data state.
    - **Normal Forms (Conceptual Understanding)**:
        - **1NF**: Deals with atomic values (no multivalued attributes).
        - **2NF**: Deals with partial dependencies of nonprime attributes on a primary key.
        - **3NF**: Deals with transitive dependencies of nonprime attributes on a primary key.
        - **Boyce-Codd Normal Form (BCNF)**: A stricter form of 3NF, generally desirable. Understand that it aims to eliminate all FDs where the left-hand side is not a superkey.
    - **Key takeaway**: Normalization reduces redundancy and anomalies, leading to a better database design.
- **5. Physical Storage and Indexing (High-Level)**
    
    - **Importance of Disk I/O**: Understand that disk access is orders of magnitude slower than CPU processing and is a major bottleneck.
    - **Indexes**:
        - **Purpose**: Speed up data retrieval. Think of a book index.
        - **Types (conceptual)**: Primary, Clustering, Secondary. Know they exist and serve different purposes based on file organization and uniqueness of the indexed field.
        - **B+-trees**: Focus on understanding that they are the most common multilevel index. Know their key characteristics: **balanced, dynamic, efficient for both equality and range queries**. Understand why they are generally preferred over B-trees (more entries in internal nodes, fewer levels for the same data size).
- **6. Query Processing and Optimization (High-Level)**
    
    - **Goal**: The DBMS aims to find a _reasonably efficient_ execution plan for a query, not necessarily the absolute optimal one (too complex/time-consuming).
    - **Conceptual Execution Order**: `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`. This is crucial for understanding how SQL queries are processed.
    - **Selectivity**: Understand this as the fraction of records satisfying a condition; optimizers use it to estimate costs and choose the best access method.
    - **Basic Join Algorithms (high-level)**: Be aware of nested-loop join and sort-merge join as common methods, and that the choice of outer loop file can impact performance.



---
**Database Fundamentals and Concepts**

- **Database Management Systems (DBMS)**: Understand what a DBMS is and its primary role in managing data.
- **Database Systems**: Know that this comprises the database itself and the DBMS software.
- **Data and Metadata (Database Catalog)**: Distinguish between raw data and metadata, which describes the database structure.
- **Database Schema and Database State (Instance)**: Differentiate between the defined structure of the database (schema) and the actual data at any given moment (state).
- **Internal, Conceptual, and External Schemas**: Learn the three levels of data abstraction in a DBMS architecture.
- **Data Independence (Program-Data Independence)**: Understand how the DBMS insulates programs from changes in physical data storage or logical schema.
- **Database Administrator (DBA)**: Know the responsibilities for database design, security, and maintenance.
- **Database Designers**: Understand their role in identifying data and structuring it.
- **End Users (Casual, Naive/Parametric, Canned Transactions)**: Be familiar with different categories of database users and their typical interactions.
- **Database Characteristics**: Grasp key properties like self-describing nature, insulation, support for multiple views, and multi-user transaction processing.

**Data Models and Languages**

- **Relational Data Model**: Learn fundamental concepts: relation (table), tuple (row), attribute (column), domain, and various types of keys (superkey, candidate key, primary key, foreign key).
- **NULL Values**: Understand their purpose and implications (unknown, not applicable).
- **SQL (Structured Query Language)**: Master this standard language.
    - **DDL (Data Definition Language)**: Commands like CREATE, ALTER, DROP for defining and modifying database schemas.
    - **DML (Data Manipulation Language)**: Commands like SELECT, INSERT, DELETE, UPDATE for manipulating data.
    - **Basic Retrieval Queries**: Learn the SELECT-FROM-WHERE structure.
    - **Complex SQL Queries**: Study nested queries (subqueries, correlated queries), set operations (UNION, INTERSECT, EXCEPT), and aggregate functions (COUNT, SUM, AVG, MIN, MAX).
    - **SQL Constraints**: Understand how to specify and enforce integrity rules (key, referential integrity, assertions, triggers).
    - **Views (Virtual Tables)**: Know how to define and use virtual tables for customized data presentation.
    - **SQL Data Types**: Be familiar with common data types like numeric, character, Boolean, date, time, CLOB, BLOB.
- **Relational Algebra**: Learn this formal, procedural query language and its operations (SELECT, PROJECT, JOIN, UNION, INTERSECTION, SET DIFFERENCE, CARTESIAN PRODUCT, Outer Joins, Aggregate Functions).
- **Relational Calculus (Tuple Relational Calculus, Domain Relational Calculus)**: Understand these formal, declarative query languages and the use of quantifiers.
- **Entity-Relationship (ER) Model**: Learn to design conceptual schemas using entities, attributes (composite, multivalued, derived), relationship types, and ER Diagrams (notation, structural constraints like cardinality ratios, weak entity types).
- **Enhanced Entity-Relationship (EER) Model**: Understand advanced conceptual modeling concepts like specialization, generalization, inheritance, subclasses, superclasses, hierarchies, lattices, and categories (union types).
- **Object-Oriented Databases (OODB)**: Be familiar with concepts such as objects, object identifiers, classes, methods, encapsulation, polymorphism, inheritance, and the basics of ODMG Standard, Object Definition Language (ODL), and Object Query Language (OQL).
- **XML (Extensible Markup Language)**: Understand the differences between structured, semistructured, and unstructured data; learn about XML documents, elements, attributes, tags, DTD, XML Schema, XPath, and XQuery.
- **UML Diagrams (Class Diagrams)**: Be familiar with using class diagrams for database modeling.

**Database Design**

- **Database Design Process**: Understand the systematic phases involved in designing a database (requirements, conceptual, logical, physical, implementation).
- **Informal Design Guidelines**: Learn principles for designing good relation schemas (clear semantics, minimal redundancy, few NULLs, no spurious tuples).
- **Normalization Theory**: Learn to evaluate and improve relational schemas.
    - **Functional Dependencies (FD)**: Understand FDs as formal constraints describing attribute relationships.
    - **Normal Forms**: Master 1NF, 2NF, 3NF, Boyce-Codd Normal Form (BCNF), Fourth Normal Form (4NF) (Multivalued Dependencies - MVD), and Fifth Normal Form (5NF) (Join Dependencies - JD) to reduce redundancy and anomalies.
    - **Desirable Properties of Decomposition (Lossless Join Property, Dependency Preservation Property)**: Understand these crucial properties for valid database decomposition.
    - **Minimal Cover of FDs**: Learn how to find a minimal set of functional dependencies.
    - **Normalization Algorithms**: Be aware of algorithms used for synthesis and decomposition into normal forms.
- **Automated Database Design Tools (CASE tools)**: Understand their function in generating database schemas.

**Database Programming and Access**

- **Database Programming Techniques Overview**: Understand different approaches for programs to interact with databases.
- **Embedded SQL**: Learn how SQL statements are embedded directly into host programming languages (e.g., C, Java/SQLJ) for static queries and basic dynamic queries.
- **Function Call Interfaces (SQL/CLI, JDBC)**: Understand these APIs for dynamic database access from languages like C and Java.
- **SQL/PSM (Persistent Stored Modules / Stored Procedures)**: Learn about storing and executing procedures directly within the DBMS.
- **Impedance Mismatch**: Be aware of the conceptual differences between programming language data structures and database data models.
- **Web Database Programming (PHP)**: Understand the general concepts of building dynamic web pages that interact with databases.

**Physical Database Storage and Access**

- **Storage Hierarchy**: Understand the different levels of computer storage (primary, secondary) and their characteristics.
- **Magnetic Disks**: Learn about disk components (tracks, sectors, cylinders, blocks) and performance factors (seek time, rotational delay, transfer time).
- **Buffering**: Understand how buffering (e.g., double buffering) can optimize disk I/O.
- **File Organizations**: Learn different ways records are physically stored: Heap Files (unordered), Sorted Files (sequential), and Hash Files (static, extendible, linear hashing).
- **Indexing Structures**:
    - **Single-Level Indexes**: Understand Primary, Clustering, and Secondary indexes, and the difference between dense and sparse indexes.
    - **Multilevel Indexes**: Learn how they improve search efficiency by creating multiple levels of index entries.
    - **B-trees and B+-trees**: Master these dynamic, balanced tree structures used for efficient indexing, including their properties and the general idea behind search, insertion, and deletion operations. B+-trees are typically preferred.
    - **Indexes on Multiple Keys (Composite Keys, Grid Files, Partitioned Hashing)**: Learn techniques for indexing on combinations of attributes.
    - **Hash Indexes**: Understand using hashing for secondary access paths.
    - **Bitmap Indexes**: Learn their application for specific fields with low distinct values and how they enable efficient query processing using bitwise operations.
- **RAID (Redundant Array of Inexpensive/Independent Disks)**: Understand its purpose for data reliability and performance, and know common levels (0, 1, 5).
- **Advanced Storage Systems (Storage Area Networks (SAN), Network-Attached Storage (NAS), iSCSI)**: Be aware of these modern storage architectures.

**Query Processing and Optimization**

- **Query Processing Steps**: Understand the stages from query input to execution (scanning, parsing, validation, internal representation, optimization).
- **Query Optimization**: Grasp the process of selecting an efficient execution plan for a query.
- **Heuristic Optimization**: Learn about rule-based strategies for reordering operations.
- **Cost Estimation**: Understand how the optimizer estimates the cost of different execution plans, often using selectivity estimates.
- **Algorithms for Relational Operations**:
    - **SELECT Operation Algorithms**: Learn various search methods (linear scan, binary search, index scans) for selection conditions.
    - **JOIN Operation Algorithms**: Understand common join strategies (nested-loop join, single-loop join, sort-merge join, partition-hash join) for combining data from multiple tables.
- **Semantic Query Optimization**: Be aware of using database constraints to refine query execution.



### Computer Network
---




### Operation System
---


Given that your interviewee has only **two days** to prepare, the focus should be on grasping the most fundamental and frequently tested operating system concepts. The aim is to build a solid conceptual understanding of the core areas, emphasizing the "what," "why," and "how" (at a high level) for each, along with key trade-offs.

Here's a highly condensed and prioritized list of topics, structured for two days of intensive study:

---

#### Core Fundamentals

This day should cover the absolute bedrock of operating systems: how processes are managed and how the CPU is allocated, followed by the basics of memory management, especially virtual memory.

- **Process Management**
    - **Process Concept**: Understand that a process is "a program in execution" and a distinct active entity, unlike a passive program.
    - **Process States**: Be able to describe the five basic states: **New, Ready, Running, Waiting, and Terminated**, and how a process transitions between them (e.g., waiting for I/O, being interrupted, terminating).
    - **Process Control Block (PCB)**: Know its purpose as a data structure that stores all process-specific information (e.g., CPU-scheduling information, memory-management information, I/O status information, accounting information).
    - **Schedulers**: Differentiate between **long-term (job) scheduler** (controls degree of multiprogramming) and **short-term (CPU) scheduler** (selects processes from ready queue). Understand the role of the **medium-term scheduler** in swapping processes in/out of memory to reduce the degree of multiprogramming.
- **CPU Scheduling**
    - **Basic Concepts**: Understand the objective of multiprogramming (maximizing CPU utilization) and the **CPU-I/O burst cycle** (processes alternate between CPU execution and I/O wait). Know the role of the **dispatcher** (context switching, dispatch latency).
    - **Scheduling Criteria**: Familiarize yourself with the key metrics used to evaluate scheduling algorithms: **CPU utilization, throughput, turnaround time, waiting time, and response time**. Understand whether they should be maximized or minimized.
    - **Key Algorithms**: Understand the mechanism, general advantages, and disadvantages of:
        - **First-Come, First-Served (FCFS)**: Simplest, nonpreemptive, can lead to the **convoy effect**.
        - **Shortest-Job-First (SJF)**: Provably optimal for minimum average waiting time but difficult to implement as it requires knowing the next CPU burst length. Mention **preemptive SJF** (shortest-remaining-time-first).
        - **Priority Scheduling**: CPU allocated to the highest-priority process. Be aware of **starvation** and its solution, **aging**.
        - **Round Robin (RR)**: Designed for time-sharing systems, uses a **time quantum**. It is preemptive. Understand the impact of time quantum size on performance (too large: becomes FCFS; too small: excessive context switching overhead).
        - **Multilevel Queue and Multilevel Feedback Queue**: Understand how processes are assigned to different queues, potentially with different scheduling algorithms, and how processes can move between queues in feedback queues (to prevent starvation and separate I/O-bound from CPU-bound tasks).
- **Memory Management & Virtual Memory Basics**
    - **Main Memory**: Recognize its role as a central, directly accessible storage for the CPU.
    - **Address Binding**: Understand the different times addresses can be bound (compile, load, execution time) and why **execution-time binding** provides flexibility (allowing processes to move in memory).
    - **Paging**: Understand its purpose (solving external fragmentation) and how it works by dividing logical memory into **pages** and physical memory into **frames**. Be aware of **internal fragmentation**.
    - **Translation Look-aside Buffer (TLB)**: What it is (a special hardware cache for page table entries) and its purpose (to speed up address translation). Understand the **hit ratio** and its impact on effective access time.
    - **Demand Paging**: Understand the concept of loading pages only "as they are needed" (lazy swapper/pager). Know the general steps involved in handling a **page fault** (trap to OS, read page from disk, update page table, restart process). Understand that a high page-fault rate significantly degrades performance.
    - **Page Replacement Algorithms (Concepts)**: Understand the _need_ for page replacement when no free frames are available. Know the role of the **modify bit (dirty bit)** in reducing I/O operations by indicating if a page needs to be written back to disk. Briefly know the ideas behind **FIFO** (Belady's Anomaly), **Optimal** (future knowledge, not implementable), and **Least-Recently-Used (LRU)** (approximates optimal by using past use).

---

#### Concurrency, Deadlocks, and High-Level System Components

This day focuses on how processes/threads interact, problems arising from concurrency, and how they are solved. It concludes with a high-level overview of IPC, File Systems, and Disk Management.

- **Process Synchronization**
    - **Background**: Understand the problem of **race conditions** when multiple processes or threads concurrently access shared data.
    - **Critical-Section Problem**: Define this problem and its three requirements for a solution: **mutual exclusion, progress, and bounded waiting**.
    - **Synchronization Tools**:
        - **Semaphores**: Understand them as integer variables with atomic `wait()` (P) and `signal()` (V) operations. Differentiate between **counting semaphores** (resource counting) and **binary semaphores** (mutex locks for mutual exclusion). Understand **busy waiting (spinlocks)** versus blocking semaphores and their trade-offs (spinlocks are good for short critical sections on multiprocessor systems).
        - **Monitors**: Know that they are a high-level language construct for synchronization and their use of **condition variables** (`wait()`, `signal()`).
    - **Classic Synchronization Problems (Conceptual)**: Be familiar with the general idea behind **Bounded-Buffer** (Producer-Consumer), **Readers-Writers**, and **Dining-Philosophers** problems as examples of concurrency challenges.
- **Deadlocks**
    - **Characterization**: Understand the **four necessary conditions** for a deadlock to occur: **mutual exclusion, hold and wait, no preemption, and circular wait**.
    - **Handling Strategies**: Be aware of the three general approaches: **prevention, avoidance, detection, and ignoring** (the latter is used by most OSes like UNIX and Windows).
    - **Prevention (High-Level)**: Briefly know that prevention involves ensuring at least one of the four necessary conditions cannot hold (e.g., resource ordering for circular wait).
    - **Avoidance (High-Level)**: Understand the concept of a **safe state** (system can allocate resources to processes without deadlocking) and that the **Banker's Algorithm** is an example of an avoidance algorithm.
    - **Detection & Recovery (High-Level)**: Know that detection involves algorithms to identify deadlocks (like wait-for graphs for single instances), and recovery involves process termination or resource preemption.
- **Interprocess Communication (IPC)**
    - Understand the two common models: **shared memory** (processes share a region of memory) and **message passing** (processes exchange messages). Briefly know that communication can be direct or indirect, and synchronous or asynchronous.
    - **Sockets and Remote Procedure Calls (RPC)**: Have a high-level understanding of their use for network communication.
- **File Systems (High-Level Overview)**
    - **Purpose**: Providing persistent storage for programs and data.
    - **Basic Concepts**: File attributes and common operations (create, read, write, delete).
    - **Access Methods**: Differentiate between **sequential access** and **direct (random) access**.
    - **Directory Structures**: Briefly know **single-level, two-level, and tree-structured directories**. Understand concepts of **path names** (absolute vs. relative) and file sharing via **links**.
    - **File Protection**: How access control (e.g., permission bits, ACLs) is used.
- **Disk Management (High-Level Overview)**
    - **Disk Performance**: Key metrics are **seek time** (moving the arm) and **rotational latency** (waiting for sector to rotate).
    - **Disk Scheduling Algorithms**: Be familiar with the basic goals and names of algorithms like **FCFS, SSTF, SCAN, C-SCAN, LOOK, and C-LOOK**.
    - **Swap-Space Management**: Understand that swap space (typically on disk) is an extension of main memory, used for entire process images or pages pushed out of main memory.

---



- **Operating System Definition and Role**: Understand what an OS is and its fundamental functions as an **intermediary between hardware and user**, a **resource allocator**, and a **control program**.
- **Computer-System Organization**: Learn about the basic components like **CPU, memory, I/O devices, device controllers, and the system bus**, and how they interact.
- **Storage Hierarchy**: Know the different levels of storage (registers, cache, main memory, secondary storage, tertiary storage) and their trade-offs in terms of **speed, cost, size, and volatility**.
- **Interrupts and System Calls**: Understand how **hardware or software signals events (interrupts)** and how user programs request OS services via **system calls** (traps).
- **Multiprogramming and Time Sharing**: Grasp the concepts of **running multiple programs concurrently** to maximize CPU utilization (multiprogramming) and enabling **user interaction** by rapidly switching between jobs (time sharing/multitasking).
- **Process Concept**: Define a **process as a program in execution**, understand its components (text, data, heap, stack), and its **lifecycle states** (new, ready, running, waiting, terminated).
- **Process Control Block (PCB)**: Know that the PCB is a **data structure holding all information** about a process.
- **Process Scheduling (Schedulers)**: Differentiate between **long-term (job), short-term (CPU), and medium-term schedulers** and their roles in managing processes.
- **Process Types (I/O-bound vs. CPU-bound)**: Understand these classifications and their importance for scheduler decisions.
- **Interprocess Communication (IPC)**: Learn mechanisms for processes to communicate, primarily **shared memory and message passing**.
- **Message Passing (send/receive)**: Understand operations, communication links (direct/indirect), and buffering (zero/bounded capacity).
- **Sockets and Remote Procedure Calls (RPC)**: Know how processes communicate across networks (sockets) and make calls to procedures on remote systems (RPC).
- **Threads**: Understand threads as the **basic unit of CPU utilization**, sharing resources within a process, and the benefits of **multithreaded programming** (responsiveness, resource sharing, scalability, multicore utilization).
- **User-level vs. Kernel-level Threads**: Distinguish between how threads are managed by the application vs. the OS kernel, and concepts like **LWPs and scheduler activations**.
- **CPU Scheduling Criteria**: Understand metrics like **turnaround time, waiting time, response time, CPU utilization, and throughput** used to evaluate scheduling algorithms.
- **CPU Scheduling Algorithms**: Be familiar with **First-Come, First-Served (FCFS)**, **Shortest-Job-First (SJF)** (including preemptive/shortest-remaining-time-first), **Priority Scheduling** (including aging for starvation prevention), **Round Robin (RR)** (with time quantum considerations), and **Multilevel Queue Scheduling**.
- **Multiple-Processor Scheduling**: Understand approaches like **symmetric (SMP)** and **asymmetric multiprocessing**, **processor affinity**, **load balancing**, and scheduling challenges in **multicore processors** (memory stalls, coarse-grained vs. fine-grained multithreading).
- **Process Synchronization Background**: Understand the need to ensure **consistency of shared data** among cooperating processes or threads.
- **Critical-Section Problem**: Define this problem and its solutions ensuring **mutual exclusion, progress, and bounded waiting**.
- **Synchronization Hardware**: Learn about **atomic instructions** like TestAndSet() and Swap() for low-level synchronization.
- **Semaphores**: Understand this **integer variable synchronization tool** with atomic wait() and signal() operations.
- **Classic Synchronization Problems**: Study the **Bounded-Buffer, Readers-Writers, and Dining-Philosophers Problems** as examples of concurrency control.
- **Monitors**: Learn about this **high-level abstract data type** for synchronization, including condition variables.
- **Atomic Transactions**: Understand transactions as **single logical units of work** that must be performed entirely or not at all, and concepts like **log-based recovery (journaling)** and **serializability** in concurrent transactions.
- **Transactional Memory**: Know this as an **alternative strategy for thread-safe concurrent applications**, where memory operations are atomic.
- **Virtual Memory Benefits**: Understand how it allows programs **larger than physical memory**, increases **multiprogramming degree**, and enables **shared libraries** and **efficient process creation**.
- **Demand Paging**: Learn the technique where **pages are loaded into memory only when they are needed** (page fault).
- **Page Fault Handling**: Know the **steps taken by the OS** when a referenced page is not in memory.
- **Copy-on-Write (COW)**: Understand this technique for **efficient process creation** where parent and child share pages until one modifies them.
- **Page Replacement Algorithms**: Study algorithms to decide which page to remove when memory is full: **FIFO**, **Optimal (OPT)**, **Least-Recently-Used (LRU)**, and approximations like **Second-Chance/Clock Algorithm**. Understand **Belady's anomaly**.
- **Frame Allocation**: Know how to **distribute available physical memory frames** among processes (equal, proportional allocation) and the concept of a **minimum number of frames** per process.
- **Thrashing**: Understand this phenomenon where a process spends **more time paging than executing**, and related concepts like **locality model** and **working-set model** (and page-fault frequency strategy).
- **Memory-Mapped Files**: Learn how to treat **file I/O as routine memory access** by mapping files into virtual address space.
- **Kernel Memory Allocation**: Understand specialized techniques like the **buddy system** and **slab allocation** for efficient kernel memory management.
- **TLB (Translation Look-aside Buffer)**: Know about this **hardware cache for page table entries** to speed up address translation, including concepts like **hit ratio** and **TLB reach**.
- **Memory Protection (Paging)**: Understand how **protection bits (read-write, read-only) and valid-invalid bits** in the page table enforce memory access control.
- **Shared Pages**: Recognize how paging enables **sharing of reentrant code** (e.g., system libraries) among multiple processes.
- **Page Size**: Understand the **trade-offs** in choosing a page size (fragmentation, page table size, I/O efficiency).
- **File Concept**: Understand a file as a **logical storage unit** with **attributes** (name, identifier, type, location, size, protection, time/date) and basic **operations** (create, delete, open, close, read, write, seek, truncate).
- **File Types and Structure**: How OS may use **file extensions** for type recognition and how files are internally structured (streams of bytes vs. records).
- **Access Methods**: Differentiate between **sequential access, direct access, and indexed access** for reading/writing file data.
- **Directory Structure**: Learn about different directory organizations like **single-level, two-level, and tree-structured directories** (including concepts like current directory, absolute/relative path names).
- **File System Mounting**: Understand how **separate file systems on different devices are attached** to a unified directory structure.
- **File Sharing**: Concepts of **multiple users accessing the same file** and associated **consistency semantics** (e.g., UNIX, session, immutable-shared files).
- **File Protection**: Mechanisms to control access to files and directories, including **Access Control Lists (ACLs)** and the **owner, group, universe** permission model.
- **File-System Implementation Layers**: Understand the typical **layered architecture** (I/O control, basic file system, file-organization module, logical file system, VFS).
- **File Allocation Methods**: How file data blocks are allocated on disk: **contiguous allocation** (and its fragmentation issues), **linked allocation** (including FAT), and **indexed allocation** (single/multi-level index blocks).
- **Free-Space Management**: Methods to **track and allocate free disk blocks** (bit vector, linked list, grouping, counting, space maps).
- **Disk Structure**: Learn about **disk platters, tracks, sectors, cylinders**, and how logical blocks are mapped onto them.
- **Disk Performance (Seek time, Rotational Latency)**: Understand these components of disk access time.
- **Disk Scheduling Algorithms**: Methods to **optimize disk arm movement** for I/O requests: **FCFS, SSTF, SCAN, C-SCAN, LOOK, C-LOOK**.
- **Disk Management**: Concepts like **low-level formatting, logical formatting**, and **bad-block recovery** (sector sparing).
- **Swap-Space Management**: How disk space is used as an **extension of main memory** for pages, and its location and management.
- **RAID Structure**: Understand RAID as a technology for **data redundancy and performance improvement** using multiple disks.
- **File System Recovery (Journaling)**: How log-structured file systems ensure **consistency of metadata updates** after crashes.
- **Open-Source Operating Systems**: Understand the **benefits** (community, security, learning) and common licenses (GPL).
- **Virtual Machines**: Learn about the concept of running **multiple operating systems concurrently** on a single physical machine.
- **Operating System Debugging**: Tools and techniques for **analyzing system behavior** (e.g., DTrace).



### Software Engineering
---




### Algorithms
---

