# CS Topics 

## Database
---

### Core Relational Model and SQL Mastery

#### 1. Relational Data Model Fundamentals

- [ ] **Definitions**: Be able to clearly define:
  - [ ] **Relation (Table)**: A collection of data organized into rows and columns.
  - [ ] **Tuple (Row)**: A single record in a table.
  - [ ] **Attribute (Column)**: A named column representing a property.
  - [ ] **Domain**: The set of allowed values for an attribute.

- [ ] **Keys (Critical)**: Understand and differentiate:
  - [ ] **Superkey**: Uniquely identifies a tuple.
  - [ ] **Candidate Key**: A minimal superkey. A relation can have multiple candidate keys.
  - [ ] **Primary Key**: The chosen candidate key to uniquely identify tuples, often underlined. Every relation schema should have one.
  - [ ] **Foreign Key**: Attribute(s) in one table that refer to the primary key of another table, linking them and enforcing referential integrity. Be aware of `ON DELETE` and `ON UPDATE` actions like `CASCADE` and `SET NULL`.
  - [ ] **Prime vs. Nonprime Attributes**: An attribute is prime if it's part of _any_ candidate key.

- [ ] **NULL Values**: Understand their meanings (unknown, not applicable, value withheld) and that SQL does not distinguish between them.

#### 2. SQL (Structured Query Language)

- [ ] **DDL (Data Definition Language)**:
  - [ ] `CREATE TABLE`: Know how to define tables with attribute names, data types (`INT`, `VARCHAR`, `DATE`, `TIME`), and constraints: `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, and `CHECK`.
  - [ ] `ALTER TABLE` (add/drop columns).
  - [ ] `DROP TABLE`.

- [ ] **DML (Data Manipulation Language)**: This requires **hands-on practice**.
  - [ ] **`SELECT` (Basic Retrieval)**: Master the `SELECT-FROM-WHERE` structure. Understand projection (what columns to show) and selection (what rows to filter).
  - [ ] **Operators**: Basic comparison (`=`, `<`, `<=`, `>`, `>=`, `<>`), `LIKE` (with `%`, `_`, `ESCAPE`), `BETWEEN`.
  - [ ] **Aggregate Functions**: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`. Understand `DISTINCT` within `COUNT`.
  - [ ] **`GROUP BY` and `HAVING`**: Use `GROUP BY` to partition tuples and `HAVING` to filter these groups based on aggregate conditions.
  - [ ] **Joins**: Focus on `INNER JOIN` logic, either with `WHERE` conditions or the `JOIN` keyword. Understand the need for aliases when joining a table to itself.
  - [ ] **Nested Queries**: Understand `IN`, `EXISTS`, and `NOT EXISTS` and their use with subqueries, especially correlated ones.
  - [ ] `INSERT`, `UPDATE`, `DELETE`: General understanding of their purpose.

- [ ] **Views**: Know their basic purpose (simplified access, security).
- [ ] **Prioritize**: Writing and understanding `SELECT` statements with various clauses. Practice joins and subqueries.

### Database Design Principles & Performance Basics

#### 3. Database Design Process & ER Model (Conceptual Design)

- [ ] **Design Process Overview**: Know the phases: requirements, conceptual design, logical design, physical design.

- [ ] **ER Model (Conceptual Modeling Tool)**:
  - [ ] **Purpose**: Used for high-level conceptual design, independent of a specific DBMS.
  - [ ] **Key Concepts**:
    - [ ] **Entities, Attributes, Relationships**: How real-world concepts are mapped.
    - [ ] **Attribute Types**: Simple, composite, multivalued (how they differ).
    - [ ] **Relationship Types**: Binary vs. Ternary (higher degree).
    - [ ] **Structural Constraints**: Crucially, **Cardinality Ratios** (1:1, 1:N, M:N) and **Participation Constraints** (total/existence dependency vs. partial). Be able to draw and interpret their common notation (1, M, N, single/double lines).
    - [ ] **Weak Entity Types**: Identify a weak entity, its owner, identifying relationship, and partial key.

#### 4. Normalization Theory (Relational Schema Quality)

- [ ] **Informal Design Guidelines**: Understand the goals: clear semantics, reducing redundancy, minimizing NULL values, and avoiding spurious tuples.
- [ ] **Update Anomalies**: Be able to explain insertion, deletion, and modification anomalies and _why_ they are problematic. This is a direct consequence of poor normalization.
- [ ] **Functional Dependencies (FDs)**: Understand X → Y means X uniquely determines Y. FDs are properties of the schema, not just a specific data state.

- [ ] **Normal Forms (Conceptual Understanding)**:
  - [ ] **1NF**: Deals with atomic values (no multivalued attributes).
  - [ ] **2NF**: Deals with partial dependencies of nonprime attributes on a primary key.
  - [ ] **3NF**: Deals with transitive dependencies of nonprime attributes on a primary key.
  - [ ] **Boyce-Codd Normal Form (BCNF)**: A stricter form of 3NF, generally desirable. Understand that it aims to eliminate all FDs where the left-hand side is not a superkey.

- [ ] **Key takeaway**: Normalization reduces redundancy and anomalies, leading to a better database design.

#### 5. Physical Storage and Indexing (High-Level)

- [ ] **Importance of Disk I/O**: Understand that disk access is orders of magnitude slower than CPU processing and is a major bottleneck.

- [ ] **Indexes**:
  - [ ] **Purpose**: Speed up data retrieval. Think of a book index.
  - [ ] **Types (conceptual)**: Primary, Clustering, Secondary. Know they exist and serve different purposes based on file organization and uniqueness of the indexed field.
  - [ ] **B+-trees**: Focus on understanding that they are the most common multilevel index. Know their key characteristics: **balanced, dynamic, efficient for both equality and range queries**. Understand why they are generally preferred over B-trees (more entries in internal nodes, fewer levels for the same data size).

#### 6. Query Processing and Optimization (High-Level)

- [ ] **Goal**: The DBMS aims to find a _reasonably efficient_ execution plan for a query, not necessarily the absolute optimal one (too complex/time-consuming).
- [ ] **Conceptual Execution Order**: `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY`. This is crucial for understanding how SQL queries are processed.
- [ ] **Selectivity**: Understand this as the fraction of records satisfying a condition; optimizers use it to estimate costs and choose the best access method.
- [ ] **Basic Join Algorithms (high-level)**: Be aware of nested-loop join and sort-merge join as common methods, and that the choice of outer loop file can impact performance.

### Database Fundamentals and Concepts

- [ ] **Database Management Systems (DBMS)**: Understand what a DBMS is and its primary role in managing data.
- [ ] **Database Systems**: Know that this comprises the database itself and the DBMS software.
- [ ] **Data and Metadata (Database Catalog)**: Distinguish between raw data and metadata, which describes the database structure.
- [ ] **Database Schema and Database State (Instance)**: Differentiate between the defined structure of the database (schema) and the actual data at any given moment (state).
- [ ] **Internal, Conceptual, and External Schemas**: Learn the three levels of data abstraction in a DBMS architecture.
- [ ] **Data Independence (Program-Data Independence)**: Understand how the DBMS insulates programs from changes in physical data storage or logical schema.
- [ ] **Database Administrator (DBA)**: Know the responsibilities for database design, security, and maintenance.
- [ ] **Database Designers**: Understand their role in identifying data and structuring it.
- [ ] **End Users (Casual, Naive/Parametric, Canned Transactions)**: Be familiar with different categories of database users and their typical interactions.
- [ ] **Database Characteristics**: Grasp key properties like self-describing nature, insulation, support for multiple views, and multi-user transaction processing.

### Data Models and Languages

- [ ] **Relational Data Model**: Learn fundamental concepts: relation (table), tuple (row), attribute (column), domain, and various types of keys (superkey, candidate key, primary key, foreign key).
- [ ] **NULL Values**: Understand their purpose and implications (unknown, not applicable).

- [ ] **SQL (Structured Query Language)**: Master this standard language.
  - [ ] **DDL (Data Definition Language)**: Commands like CREATE, ALTER, DROP for defining and modifying database schemas.
  - [ ] **DML (Data Manipulation Language)**: Commands like SELECT, INSERT, DELETE, UPDATE for manipulating data.
  - [ ] **Basic Retrieval Queries**: Learn the SELECT-FROM-WHERE structure.
  - [ ] **Complex SQL Queries**: Study nested queries (subqueries, correlated queries), set operations (UNION, INTERSECT, EXCEPT), and aggregate functions (COUNT, SUM, AVG, MIN, MAX).
  - [ ] **SQL Constraints**: Understand how to specify and enforce integrity rules (key, referential integrity, assertions, triggers).
  - [ ] **Views (Virtual Tables)**: Know how to define and use virtual tables for customized data presentation.
  - [ ] **SQL Data Types**: Be familiar with common data types like numeric, character, Boolean, date, time, CLOB, BLOB.

- [ ] **Relational Algebra**: Learn this formal, procedural query language and its operations (SELECT, PROJECT, JOIN, UNION, INTERSECTION, SET DIFFERENCE, CARTESIAN PRODUCT, Outer Joins, Aggregate Functions).
- [ ] **Relational Calculus (Tuple Relational Calculus, Domain Relational Calculus)**: Understand these formal, declarative query languages and the use of quantifiers.
- [ ] **Entity-Relationship (ER) Model**: Learn to design conceptual schemas using entities, attributes (composite, multivalued, derived), relationship types, and ER Diagrams (notation, structural constraints like cardinality ratios, weak entity types).
- [ ] **Enhanced Entity-Relationship (EER) Model**: Understand advanced conceptual modeling concepts like specialization, generalization, inheritance, subclasses, superclasses, hierarchies, lattices, and categories (union types).
- [ ] **Object-Oriented Databases (OODB)**: Be familiar with concepts such as objects, object identifiers, classes, methods, encapsulation, polymorphism, inheritance, and the basics of ODMG Standard, Object Definition Language (ODL), and Object Query Language (OQL).
- [ ] **XML (Extensible Markup Language)**: Understand the differences between structured, semistructured, and unstructured data; learn about XML documents, elements, attributes, tags, DTD, XML Schema, XPath, and XQuery.
- [ ] **UML Diagrams (Class Diagrams)**: Be familiar with using class diagrams for database modeling.

### Database Design

- [ ] **Database Design Process**: Understand the systematic phases involved in designing a database (requirements, conceptual, logical, physical, implementation).
- [ ] **Informal Design Guidelines**: Learn principles for designing good relation schemas (clear semantics, minimal redundancy, few NULLs, no spurious tuples).

- [ ] **Normalization Theory**: Learn to evaluate and improve relational schemas.
  - [ ] **Functional Dependencies (FD)**: Understand FDs as formal constraints describing attribute relationships.
  - [ ] **Normal Forms**: Master 1NF, 2NF, 3NF, Boyce-Codd Normal Form (BCNF), Fourth Normal Form (4NF) (Multivalued Dependencies - MVD), and Fifth Normal Form (5NF) (Join Dependencies - JD) to reduce redundancy and anomalies.
  - [ ] **Desirable Properties of Decomposition (Lossless Join Property, Dependency Preservation Property)**: Understand these crucial properties for valid database decomposition.
  - [ ] **Minimal Cover of FDs**: Learn how to find a minimal set of functional dependencies.
  - [ ] **Normalization Algorithms**: Be aware of algorithms used for synthesis and decomposition into normal forms.

- [ ] **Automated Database Design Tools (CASE tools)**: Understand their function in generating database schemas.

### Database Programming and Access

- [ ] **Database Programming Techniques Overview**: Understand different approaches for programs to interact with databases.
- [ ] **Embedded SQL**: Learn how SQL statements are embedded directly into host programming languages (e.g., C, Java/SQLJ) for static queries and basic dynamic queries.
- [ ] **Function Call Interfaces (SQL/CLI, JDBC)**: Understand these APIs for dynamic database access from languages like C and Java.
- [ ] **SQL/PSM (Persistent Stored Modules / Stored Procedures)**: Learn about storing and executing procedures directly within the DBMS.
- [ ] **Impedance Mismatch**: Be aware of the conceptual differences between programming language data structures and database data models.
- [ ] **Web Database Programming (PHP)**: Understand the general concepts of building dynamic web pages that interact with databases.

### Physical Database Storage and Access

- [ ] **Storage Hierarchy**: Understand the different levels of computer storage (primary, secondary) and their characteristics.
- [ ] **Magnetic Disks**: Learn about disk components (tracks, sectors, cylinders, blocks) and performance factors (seek time, rotational delay, transfer time).
- [ ] **Buffering**: Understand how buffering (e.g., double buffering) can optimize disk I/O.
- [ ] **File Organizations**: Learn different ways records are physically stored: Heap Files (unordered), Sorted Files (sequential), and Hash Files (static, extendible, linear hashing).

- [ ] **Indexing Structures**:
  - [ ] **Single-Level Indexes**: Understand Primary, Clustering, and Secondary indexes, and the difference between dense and sparse indexes.
  - [ ] **Multilevel Indexes**: Learn how they improve search efficiency by creating multiple levels of index entries.
  - [ ] **B-trees and B+-trees**: Master these dynamic, balanced tree structures used for efficient indexing, including their properties and the general idea behind search, insertion, and deletion operations. B+-trees are typically preferred.
  - [ ] **Indexes on Multiple Keys (Composite Keys, Grid Files, Partitioned Hashing)**: Learn techniques for indexing on combinations of attributes.
  - [ ] **Hash Indexes**: Understand using hashing for secondary access paths.
  - [ ] **Bitmap Indexes**: Learn their application for specific fields with low distinct values and how they enable efficient query processing using bitwise operations.

- [ ] **RAID (Redundant Array of Inexpensive/Independent Disks)**: Understand its purpose for data reliability and performance, and know common levels (0, 1, 5).
- [ ] **Advanced Storage Systems (Storage Area Networks (SAN), Network-Attached Storage (NAS), iSCSI)**: Be aware of these modern storage architectures.

### Query Processing and Optimization

- [ ] **Query Processing Steps**: Understand the stages from query input to execution (scanning, parsing, validation, internal representation, optimization).
- [ ] **Query Optimization**: Grasp the process of selecting an efficient execution plan for a query.
- [ ] **Heuristic Optimization**: Learn about rule-based strategies for reordering operations.
- [ ] **Cost Estimation**: Understand how the optimizer estimates the cost of different execution plans, often using selectivity estimates.

- [ ] **Algorithms for Relational Operations**:
  - [ ] **SELECT Operation Algorithms**: Learn various search methods (linear scan, binary search, index scans) for selection conditions.
  - [ ] **JOIN Operation Algorithms**: Understand common join strategies (nested-loop join, single-loop join, sort-merge join, partition-hash join) for combining data from multiple tables.

- [ ] **Semantic Query Optimization**: Be aware of using database constraints to refine query execution.

## Computer Network
---

### Network Fundamentals

- [ ] **Computer Networking Fundamentals**: Understand the basic architecture, including layers, protocols, and how components like hosts, routers, and links interact.
- [ ] **Protocols**: Learn that protocols define the **format and order of messages** exchanged between communicating entities, and the **actions taken** upon transmission or receipt.
- [ ] **Internet Architecture**: Differentiate between the **nuts-and-bolts description** (hardware and software components) and the **services description** (infrastructure for distributed applications).

### Network Components

- [ ] **Network Edge**: Know about **end systems (hosts)** which run applications, categorized into **clients** and **servers**, often residing in large **data centers**.
- [ ] **Network Core**: Understand the two basic approaches for data transport: **packet switching** (data segmented into packets, store-and-forward) and **circuit switching** (dedicated end-to-end connection, potentially wasteful).
- [ ] **Communication Links**: Be familiar with different **physical media** (coaxial cable, copper wire, optical fiber, radio spectrum) and **transmission rates** (bits/second).

### Network Performance

- [ ] **Delay in Packet-Switched Networks**: Learn about the four components of nodal delay: **processing delay**, **queuing delay**, **transmission delay**, and **propagation delay**.
- [ ] **Queuing Delay**: Understand that it **varies from packet to packet** and is influenced by **traffic intensity** (design systems so traffic intensity is no greater than 1).
- [ ] **Packet Loss**: Know that packets can be lost due to **full buffers** at routers.
- [ ] **Throughput**: Define it as the **amount of data per second** transferred between end systems, distinguishing between instantaneous and average throughput.

### Protocol Architecture

- [ ] **Protocol Layers**: Grasp the concept of a **protocol stack** (Internet's five layers: **application, transport, network, link, physical**) and **encapsulation** (adding header information from upper layers to lower layers).
- [ ] **Network Security Overview**: Be aware of common threats (**malware, botnets, distributed denial-of-service (DDoS) attacks, packet sniffing, IP spoofing**) and the core security services (**confidentiality, message integrity, end-point authentication**).

### Application Layer

- [ ] **Application Layer Principles**: Understand **network application architectures** (client-server, peer-to-peer), **processes communicating** (not programs), and the use of **sockets** as the interface to the transport layer.

### Transport Layer

- [ ] **Transport Services**: Differentiate between **TCP** (**connection-oriented service, reliable data transfer, flow control, congestion control**) and **UDP** (**connectionless, unreliable data transfer, provides multiplexing/demultiplexing only**).
- [ ] **Port Numbers**: Learn their role in **multiplexing and demultiplexing** to deliver segments to the correct application process. **Well-known port numbers** are reserved for specific applications.
- [ ] **Reliable Data Transfer (RDT)**: Study the **fundamental principles** (checksums for error detection, acknowledgments (ACKs/NAKs), sequence numbers for retransmissions and ordering, timers for handling loss) and common protocols (**Go-Back-N (GBN)** and **Selective Repeat (SR)**) for pipelining.

#### TCP Specifics

- [ ] **TCP Segment Structure**: Know the key fields including **sequence numbers** (byte-stream number of the first byte) and **acknowledgment numbers** (next byte expected).
- [ ] **Round-Trip Time (RTT) Estimation**: Understand how TCP estimates RTT and calculates the **timeout interval** using a **weighted average** of SampleRTTs.
- [ ] **TCP Connection Management**: Familiarize yourself with the **three-way handshake** for connection establishment (SYN, SYNACK, ACK) and the process for **connection teardown**.
- [ ] **TCP Congestion Control**: Comprehend the **congestion window (cwnd)** and its adjustment, the **slow start**, **congestion avoidance**, **fast retransmit**, and **fast recovery** phases, and the **Additive Increase Multiplicative Decrease (AIMD)** principle.

### Network Layer

- [ ] **Network Layer Roles**: Understand the separation into **data plane** (per-router forwarding) and **control plane** (network-wide routing logic).
- [ ] **Forwarding vs. Routing**: Distinguish **forwarding** (router-local action, short timescales, hardware) from **routing** (network-wide process, longer timescales, software, determines end-to-end paths).
- [ ] **Forwarding Table**: Understand its role in routers for determining the **outgoing link interface** based on packet header values, often using the **longest prefix matching rule**.

#### Router Architecture

- [ ] **Router Internal Architecture**: Learn about **input ports**, **output ports**, and the **switching fabric** (memory, bus, interconnection network) that moves packets between them.
- [ ] **Packet Queuing and Scheduling**: Understand **queuing delay** at input/output ports and various **packet scheduling disciplines** (**First-in-First-Out (FIFO)**, **Priority queuing**, **Round Robin (RR)**, **Weighted Fair Queuing (WFQ)**).

#### Internet Protocol (IP)

- [ ] **Internet Protocol (IP)**: Know the **IPv4 datagram format** (key fields like **Time-to-Live (TTL)**, **Protocol field**), **IPv4 addressing** (IP addresses associated with interfaces, **subnets**, **Classless Interdomain Routing (CIDR)**, **Network Address Translation (NAT)**), and the purpose of **IPv6** (larger address space, streamlined header).

#### Advanced Network Technologies

- [ ] **Software-Defined Networking (SDN)**: Understand the **"match-plus-action" paradigm** as a generalization of forwarding, and **OpenFlow** as a standard for programming packet switches with **flow tables** by a **logically centralized controller**.
- [ ] **Middleboxes and Network Function Virtualization (NFV)**: Know that **middleboxes** perform functions beyond routing (e.g., firewalling, load balancing) and that **NFV** aims to implement these services on commodity hardware.

### Link Layer

- [ ] **Link Layer Services**: Learn about **framing** (encapsulating network-layer datagrams into link-layer frames) and **error detection and correction techniques** (**parity checks, checksumming, Cyclic Redundancy Check (CRC)**).
- [ ] **Multiple Access Protocols**: Understand how nodes share a broadcast channel using **channel partitioning protocols** (Time Division Multiplexing (TDM), Frequency Division Multiplexing (FDM), Code Division Multiple Access (CDMA)), **random access protocols** (ALOHA, Carrier Sense Multiple Access with Collision Detection (CSMA/CD), binary exponential backoff), and **taking-turns protocols** (polling, token passing).

#### Ethernet and Switching

- [ ] **Ethernet**: Understand **MAC addresses** (link-layer addresses), the **Ethernet frame structure**, and different **Ethernet technologies**.
- [ ] **Link-Layer Switches**: Know their function in **filtering** and **forwarding** frames based on MAC addresses, and how they **self-learn** switch tables.
- [ ] **Virtual LANs (VLANs)**: Understand how **VLANs** logically segment a single physical LAN infrastructure into multiple broadcast domains, using standards like **802.1Q for VLAN trunking**.

#### MPLS

- [ ] **Multiprotocol Label Switching (MPLS)**: Learn that it's a **packet-switched, virtual-circuit network** that can interconnect IP devices, enabling **traffic engineering** by forwarding packets based on labels.

### Wireless and Mobile Networks

- [ ] **Wireless and Mobile Networks**: Understand **wireless link characteristics** (**path loss, multipath propagation, interference, signal-to-noise ratio (SNR), bit error rate (BER)**) and the distinction between issues posed by **wireless links** and by **mobility**.
- [ ] **802.11 (WiFi)**: Learn its **architecture** (Base Service Set (BSS), Access Point (AP)), **MAC protocol** (Carrier Sense Multiple Access with Collision Avoidance (CSMA/CA), RTS/CTS frames), and mechanisms for **mobility** within a subnet.
- [ ] **Cellular Networks (4G and 5G)**: Understand the **all-IP core network architecture**, **mobility management** (e.g., home and visited networks, tunneling, handover process), and key aspects of **5G** (eMBB, mMTC, URLLC, millimeter wave frequencies).

### Network Security

#### Cryptography

- [ ] **Cryptography**: Define **plaintext**, **ciphertext**, and the role of **keys**. Differentiate between **symmetric key cryptography** (same key for encryption/decryption, e.g., DES, AES) and **public key cryptography** (public key for encryption, private key for decryption, e.g., RSA, Diffie-Hellman). Be aware of common **attacks** (ciphertext-only, known-plaintext, chosen-plaintext).

#### Message Security

- [ ] **Message Integrity**: Understand how **cryptographic hash functions** are used to create fixed-size hashes of messages, and how these are applied in **Message Authentication Codes (MACs)** and **digital signatures** to verify message origin and ensure it hasn't been altered.
- [ ] **End-Point Authentication**: Learn how entities prove their identity to each other over a network, often using **nonces** (once-in-a-lifetime values) to prevent **playback attacks**.

#### Security Protocols

- [ ] **Transport Layer Security (TLS)**: Understand its multi-step **handshake protocol** (client/server negotiation, key exchange, certificate verification) and **record protocol** (encrypts application data, applies HMAC, uses sequence numbers to prevent reordering/replaying).
- [ ] **IP Security (IPsec)**: Know that it provides **security at the network layer**, commonly used for **Virtual Private Networks (VPNs)**. Key concepts include **Security Associations (SAs)** (state information for secure communication), **tunnel mode**, and the **Internet Key Exchange (IKE)** protocol for automated SA creation.

#### Security Infrastructure

- [ ] **Firewalls**: Differentiate between **traditional (stateless) packet filters** (filter based on header fields in isolation), **stateful packet filters** (track TCP connections), and **application gateways** (make decisions based on application data).
- [ ] **Intrusion Detection/Prevention Systems (IDS/IPS)**: Understand their role in **detecting suspicious traffic** (IDS alerts) and **filtering it out** (IPS).

## Operating System
---

### Core Fundamentals

#### Process Management

- [ ] **Process Concept**: Understand that a process is "a program in execution" and a distinct active entity, unlike a passive program.
- [ ] **Process States**: Be able to describe the five basic states: **New, Ready, Running, Waiting, and Terminated**, and how a process transitions between them (e.g., waiting for I/O, being interrupted, terminating).
- [ ] **Process Control Block (PCB)**: Know its purpose as a data structure that stores all process-specific information (e.g., CPU-scheduling information, memory-management information, I/O status information, accounting information).
- [ ] **Schedulers**: Differentiate between **long-term (job) scheduler** (controls degree of multiprogramming) and **short-term (CPU) scheduler** (selects processes from ready queue). Understand the role of the **medium-term scheduler** in swapping processes in/out of memory to reduce the degree of multiprogramming.

#### CPU Scheduling

- [ ] **Basic Concepts**: Understand the objective of multiprogramming (maximizing CPU utilization) and the **CPU-I/O burst cycle** (processes alternate between CPU execution and I/O wait). Know the role of the **dispatcher** (context switching, dispatch latency).
- [ ] **Scheduling Criteria**: Familiarize yourself with the key metrics used to evaluate scheduling algorithms: **CPU utilization, throughput, turnaround time, waiting time, and response time**. Understand whether they should be maximized or minimized.

- [ ] **Key Algorithms**: Understand the mechanism, general advantages, and disadvantages of:
  - [ ] **First-Come, First-Served (FCFS)**: Simplest, nonpreemptive, can lead to the **convoy effect**.
  - [ ] **Shortest-Job-First (SJF)**: Provably optimal for minimum average waiting time but difficult to implement as it requires knowing the next CPU burst length. Mention **preemptive SJF** (shortest-remaining-time-first).
  - [ ] **Priority Scheduling**: CPU allocated to the highest-priority process. Be aware of **starvation** and its solution, **aging**.
  - [ ] **Round Robin (RR)**: Designed for time-sharing systems, uses a **time quantum**. It is preemptive. Understand the impact of time quantum size on performance (too large: becomes FCFS; too small: excessive context switching overhead).
  - [ ] **Multilevel Queue and Multilevel Feedback Queue**: Understand how processes are assigned to different queues, potentially with different scheduling algorithms, and how processes can move between queues in feedback queues (to prevent starvation and separate I/O-bound from CPU-bound tasks).

#### Memory Management & Virtual Memory Basics

- [ ] **Main Memory**: Recognize its role as a central, directly accessible storage for the CPU.
- [ ] **Address Binding**: Understand the different times addresses can be bound (compile, load, execution time) and why **execution-time binding** provides flexibility (allowing processes to move in memory).
- [ ] **Paging**: Understand its purpose (solving external fragmentation) and how it works by dividing logical memory into **pages** and physical memory into **frames**. Be aware of **internal fragmentation**.
- [ ] **Translation Look-aside Buffer (TLB)**: What it is (a special hardware cache for page table entries) and its purpose (to speed up address translation). Understand the **hit ratio** and its impact on effective access time.
- [ ] **Demand Paging**: Understand the concept of loading pages only "as they are needed" (lazy swapper/pager). Know the general steps involved in handling a **page fault** (trap to OS, read page from disk, update page table, restart process). Understand that a high page-fault rate significantly degrades performance.
- [ ] **Page Replacement Algorithms (Concepts)**: Understand the _need_ for page replacement when no free frames are available. Know the role of the **modify bit (dirty bit)** in reducing I/O operations by indicating if a page needs to be written back to disk. Briefly know the ideas behind **FIFO** (Belady's Anomaly), **Optimal** (future knowledge, not implementable), and **Least-Recently-Used (LRU)** (approximates optimal by using past use).

### Concurrency, Deadlocks, and High-Level System Components

#### Process Synchronization

- [ ] **Background**: Understand the problem of **race conditions** when multiple processes or threads concurrently access shared data.
- [ ] **Critical-Section Problem**: Define this problem and its three requirements for a solution: **mutual exclusion, progress, and bounded waiting**.

- [ ] **Synchronization Tools**:
  - [ ] **Semaphores**: Understand them as integer variables with atomic `wait()` (P) and `signal()` (V) operations. Differentiate between **counting semaphores** (resource counting) and **binary semaphores** (mutex locks for mutual exclusion). Understand **busy waiting (spinlocks)** versus blocking semaphores and their trade-offs (spinlocks are good for short critical sections on multiprocessor systems).
  - [ ] **Monitors**: Know that they are a high-level language construct for synchronization and their use of **condition variables** (`wait()`, `signal()`).

- [ ] **Classic Synchronization Problems (Conceptual)**: Be familiar with the general idea behind **Bounded-Buffer** (Producer-Consumer), **Readers-Writers**, and **Dining-Philosophers** problems as examples of concurrency challenges.

#### Deadlocks

- [ ] **Characterization**: Understand the **four necessary conditions** for a deadlock to occur: **mutual exclusion, hold and wait, no preemption, and circular wait**.
- [ ] **Handling Strategies**: Be aware of the three general approaches: **prevention, avoidance, detection, and ignoring** (the latter is used by most OSes like UNIX and Windows).
- [ ] **Prevention (High-Level)**: Briefly know that prevention involves ensuring at least one of the four necessary conditions cannot hold (e.g., resource ordering for circular wait).
- [ ] **Avoidance (High-Level)**: Understand the concept of a **safe state** (system can allocate resources to processes without deadlocking) and that the **Banker's Algorithm** is an example of an avoidance algorithm.
- [ ] **Detection & Recovery (High-Level)**: Know that detection involves algorithms to identify deadlocks (like wait-for graphs for single instances), and recovery involves process termination or resource preemption.

#### Interprocess Communication (IPC)

- [ ] Understand the two common models: **shared memory** (processes share a region of memory) and **message passing** (processes exchange messages). Briefly know that communication can be direct or indirect, and synchronous or asynchronous.
- [ ] **Sockets and Remote Procedure Calls (RPC)**: Have a high-level understanding of their use for network communication.

#### File Systems (High-Level Overview)

- [ ] **Purpose**: Providing persistent storage for programs and data.
- [ ] **Basic Concepts**: File attributes and common operations (create, read, write, delete).
- [ ] **Access Methods**: Differentiate between **sequential access** and **direct (random) access**.
- [ ] **Directory Structures**: Briefly know **single-level, two-level, and tree-structured directories**. Understand concepts of **path names** (absolute vs. relative) and file sharing via **links**.
- [ ] **File Protection**: How access control (e.g., permission bits, ACLs) is used.

#### Disk Management (High-Level Overview)

- [ ] **Disk Performance**: Key metrics are **seek time** (moving the arm) and **rotational latency** (waiting for sector to rotate).
- [ ] **Disk Scheduling Algorithms**: Be familiar with the basic goals and names of algorithms like **FCFS, SSTF, SCAN, C-SCAN, LOOK, and C-LOOK**.
- [ ] **Swap-Space Management**: Understand that swap space (typically on disk) is an extension of main memory, used for entire process images or pages pushed out of main memory.

### Extended OS Concepts

#### System Organization and Architecture

- [ ] **Operating System Definition and Role**: Understand what an OS is and its fundamental functions as an **intermediary between hardware and user**, a **resource allocator**, and a **control program**.
- [ ] **Computer-System Organization**: Learn about the basic components like **CPU, memory, I/O devices, device controllers, and the system bus**, and how they interact.
- [ ] **Storage Hierarchy**: Know the different levels of storage (registers, cache, main memory, secondary storage, tertiary storage) and their trade-offs in terms of **speed, cost, size, and volatility**.
- [ ] **Interrupts and System Calls**: Understand how **hardware or software signals events (interrupts)** and how user programs request OS services via **system calls** (traps).
- [ ] **Multiprogramming and Time Sharing**: Grasp the concepts of **running multiple programs concurrently** to maximize CPU utilization (multiprogramming) and enabling **user interaction** by rapidly switching between jobs (time sharing/multitasking).

#### Advanced Process Concepts

- [ ] **Process Types (I/O-bound vs. CPU-bound)**: Understand these classifications and their importance for scheduler decisions.
- [ ] **Message Passing (send/receive)**: Understand operations, communication links (direct/indirect), and buffering (zero/bounded capacity).
- [ ] **Threads**: Understand threads as the **basic unit of CPU utilization**, sharing resources within a process, and the benefits of **multithreaded programming** (responsiveness, resource sharing, scalability, multicore utilization).
- [ ] **User-level vs. Kernel-level Threads**: Distinguish between how threads are managed by the application vs. the OS kernel, and concepts like **LWPs and scheduler activations**.

#### Advanced CPU Scheduling

- [ ] **Multiple-Processor Scheduling**: Understand approaches like **symmetric (SMP)** and **asymmetric multiprocessing**, **processor affinity**, **load balancing**, and scheduling challenges in **multicore processors** (memory stalls, coarse-grained vs. fine-grained multithreading).

#### Advanced Synchronization

- [ ] **Synchronization Hardware**: Learn about **atomic instructions** like TestAndSet() and Swap() for low-level synchronization.
- [ ] **Atomic Transactions**: Understand transactions as **single logical units of work** that must be performed entirely or not at all, and concepts like **log-based recovery (journaling)** and **serializability** in concurrent transactions.
- [ ] **Transactional Memory**: Know this as an **alternative strategy for thread-safe concurrent applications**, where memory operations are atomic.

#### Advanced Memory Management

- [ ] **Virtual Memory Benefits**: Understand how it allows programs **larger than physical memory**, increases **multiprogramming degree**, and enables **shared libraries** and **efficient process creation**.
- [ ] **Page Fault Handling**: Know the **steps taken by the OS** when a referenced page is not in memory.
- [ ] **Copy-on-Write (COW)**: Understand this technique for **efficient process creation** where parent and child share pages until one modifies them.
- [ ] **Page Replacement Algorithms**: Study algorithms to decide which page to remove when memory is full: **FIFO**, **Optimal (OPT)**, **Least-Recently-Used (LRU)**, and approximations like **Second-Chance/Clock Algorithm**. Understand **Belady's anomaly**.
- [ ] **Frame Allocation**: Know how to **distribute available physical memory frames** among processes (equal, proportional allocation) and the concept of a **minimum number of frames** per process.
- [ ] **Thrashing**: Understand this phenomenon where a process spends **more time paging than executing**, and related concepts like **locality model** and **working-set model** (and page-fault frequency strategy).
- [ ] **Memory-Mapped Files**: Learn how to treat **file I/O as routine memory access** by mapping files into virtual address space.
- [ ] **Kernel Memory Allocation**: Understand specialized techniques like the **buddy system** and **slab allocation** for efficient kernel memory management.
- [ ] **Memory Protection (Paging)**: Understand how **protection bits (read-write, read-only) and valid-invalid bits** in the page table enforce memory access control.
- [ ] **Shared Pages**: Recognize how paging enables **sharing of reentrant code** (e.g., system libraries) among multiple processes.
- [ ] **Page Size**: Understand the **trade-offs** in choosing a page size (fragmentation, page table size, I/O efficiency).

#### Advanced File Systems

- [ ] **File Concept**: Understand a file as a **logical storage unit** with **attributes** (name, identifier, type, location, size, protection, time/date) and basic **operations** (create, delete, open, close, read, write, seek, truncate).
- [ ] **File Types and Structure**: How OS may use **file extensions** for type recognition and how files are internally structured (streams of bytes vs. records).
- [ ] **Access Methods**: Differentiate between **sequential access, direct access, and indexed access** for reading/writing file data.
- [ ] **Directory Structure**: Learn about different directory organizations like **single-level, two-level, and tree-structured directories** (including concepts like current directory, absolute/relative path names).
- [ ] **File System Mounting**: Understand how **separate file systems on different devices are attached** to a unified directory structure.
- [ ] **File Sharing**: Concepts of **multiple users accessing the same file** and associated **consistency semantics** (e.g., UNIX, session, immutable-shared files).
- [ ] **File Protection**: Mechanisms to control access to files and directories, including **Access Control Lists (ACLs)** and the **owner, group, universe** permission model.

#### File System Implementation

- [ ] **File-System Implementation Layers**: Understand the typical **layered architecture** (I/O control, basic file system, file-organization module, logical file system, VFS).
- [ ] **File Allocation Methods**: How file data blocks are allocated on disk: **contiguous allocation** (and its fragmentation issues), **linked allocation** (including FAT), and **indexed allocation** (single/multi-level index blocks).
- [ ] **Free-Space Management**: Methods to **track and allocate free disk blocks** (bit vector, linked list, grouping, counting, space maps).

#### Advanced Disk Management

- [ ] **Disk Structure**: Learn about **disk platters, tracks, sectors, cylinders**, and how logical blocks are mapped onto them.
- [ ] **Disk Performance (Seek time, Rotational Latency)**: Understand these components of disk access time.
- [ ] **Disk Management**: Concepts like **low-level formatting, logical formatting**, and **bad-block recovery** (sector sparing).
- [ ] **RAID Structure**: Understand RAID as a technology for **data redundancy and performance improvement** using multiple disks.
- [ ] **File System Recovery (Journaling)**: How log-structured file systems ensure **consistency of metadata updates** after crashes.

#### Modern OS Topics

- [ ] **Open-Source Operating Systems**: Understand the **benefits** (community, security, learning) and common licenses (GPL).
- [ ] **Virtual Machines**: Learn about the concept of running **multiple operating systems concurrently** on a single physical machine.
- [ ] **Operating System Debugging**: Tools and techniques for **analyzing system behavior** (e.g., DTrace).

## Software Engineering
---

### Software Engineering Fundamentals

#### Software Engineering Basics

- [ ] **Software Engineering**: Learn that it's an **engineering discipline concerned with all aspects of software production**, from initial specification to ongoing maintenance, balancing quality, schedule, and budget.
- [ ] **Software**: Understand it's not just programs, but also **associated documentation, libraries, and configuration data** necessary for functionality.
- [ ] **Attributes of Good Software**: Focus on key qualities like **maintainability, dependability (reliability, safety, security, resilience), efficiency, and acceptability**.
- [ ] **Software Engineering Diversity**: Realize that **different types of software systems (e.g., embedded, information, web-based) require specialized engineering techniques**.
- [ ] **Ethical and Professional Issues**: Understand the importance of **confidentiality, competence, and intellectual property rights** for software engineers.

### Software Process Models

#### Process Model Types

- [ ] **Software Process Models**: Learn about **simplified representations of how software is developed**, such as Waterfall, Incremental, and Spiral models.
- [ ] **Plan-Driven Processes**: Understand that activities are **planned extensively upfront**, with progress measured against this detailed plan.
- [ ] **Agile Processes**: Know that planning is **incremental and continuous**, allowing for quick adaptation to changing requirements.

#### Core Process Activities

- [ ] **Fundamental Process Activities**: Be aware of the core stages: **specification, development, validation, and evolution**.
- [ ] **Reuse-Based Development**: Learn this involves **identifying and integrating existing components or systems** to build new software.
- [ ] **Prototyping**: Understand it's about creating **early, experimental versions of a system** to explore concepts, design choices, and validate requirements.
- [ ] **Incremental Delivery**: Know that **software is delivered in successive, functional subsets** to the customer, prioritizing high-value features.

### Agile Methods

#### Agile Fundamentals

- [ ] **Agile Methods Rationale**: Understand the drive for **rapid software development and delivery** in dynamic business environments.
- [ ] **Agile Manifesto**: Know the **core principles emphasizing individuals, working software, customer collaboration, and responding to change**.

#### Agile Practices

- [ ] **Extreme Programming (XP)**: Be familiar with practices like **user stories, test-first development, refactoring, pair programming, and continuous integration**.
- [ ] **User Stories**: Learn they are **short, narrative descriptions of desired functionality from a user's perspective**, used for planning and requirements elicitation.
- [ ] **Refactoring**: Understand it's about **improving the internal structure and readability of code without changing its external behavior**.
- [ ] **Test-First Development (TDD)**: Know that **automated tests are written before the code** they are meant to validate, guiding development and ensuring quality.

#### Scrum Framework

- [ ] **Scrum**: Understand this **agile project management framework** using product backlogs, fixed-length sprints, and daily stand-up meetings.
- [ ] **Product Backlog**: Learn it's a **prioritized, evolving list of features and work items** for a product.
- [ ] **Sprint**: Understand it's a **short, time-boxed period (e.g., 2-4 weeks)** during which a Scrum team aims to complete a set of work.
- [ ] **Scaling Agile Methods**: Be aware of the **challenges and necessary adaptations for applying agile principles to large, complex systems**.

### Requirements Engineering

#### Requirements Types

- [ ] **Requirements**: Understand that these define **what a system should do** and the constraints on its operation.
- [ ] **User Requirements**: Know they are **abstract statements of system services** for customers and end-users.
- [ ] **System Requirements**: Learn these are **detailed descriptions of system functions and constraints**, specifying what is to be implemented.
- [ ] **Functional Requirements**: Understand these describe **specific services or behaviors** the system must provide.
- [ ] **Non-Functional Requirements**: Know these define **quality attributes and constraints** such as performance, security, reliability, and usability. They should be measurable.

#### Requirements Process

- [ ] **Requirements Elicitation**: Learn this is the process of **gathering and understanding stakeholder needs**.
- [ ] **Requirements Specification**: Understand it's the activity of **documenting requirements in a clear, consistent format** (e.g., Software Requirements Document).
- [ ] **Requirements Validation**: Know this involves **checking requirements for completeness, consistency, realism, and verifiability**.
- [ ] **Requirements Change Management**: Understand the process for **assessing, approving, and tracking proposed modifications** to requirements.

### System Modeling

#### Modeling Fundamentals

- [ ] **System Models**: Understand they are **abstract representations of a system** used to simplify complexity, aid discussion, and document design.
- [ ] **UML (Unified Modeling Language)**: Be familiar with it as a **standard for object-oriented modeling**, including diagrams like use cases, class, sequence, and state diagrams.

#### Model Types

- [ ] **Context Models**: Learn these define the **system's boundaries and interactions with its external environment**.
- [ ] **Interaction Models**: Understand these show **how system components interact** (e.g., Sequence Diagrams) or how users interact with the system (e.g., Use Case Diagrams).
- [ ] **Structural Models**: Know these represent the **organization and composition of a system's elements** (e.g., Class Diagrams).
- [ ] **Behavioral Models**: Understand these describe the **dynamic behavior of a system**, showing how it responds to events (e.g., State Diagrams).
- [ ] **Model-Driven Engineering (MDE)**: Learn this approach uses **models as primary development artifacts**, from which code can be automatically generated.

### Software Architecture

#### Architecture Fundamentals

- [ ] **Software Architecture**: Understand it's the **fundamental organization of a system's components** and their relationships, significantly influencing non-functional attributes.
- [ ] **Architectural Patterns**: Learn these are **reusable, proven descriptions of common system organizations** (e.g., Client-Server, Layered, Repository).
- [ ] **Architectural Views**: Know that **different perspectives or diagrams** are used to represent various aspects of a system's architecture.
- [ ] **Application Architectures**: Understand these are **generic organizational models for specific types of applications** (e.g., transaction processing).

#### Common Architectural Patterns

- [ ] **Client-Server Architecture**: Be familiar with this **distributed architecture where functionality is separated into service providers (servers) and users (clients)**.
- [ ] **Layered Architecture**: Know this structures a system into **functional layers**, where each layer uses services from the one below it.
- [ ] **Repository Architecture**: Understand this centers components around a **shared data store**, communicating indirectly through it.

### Design and Development

#### Design Approaches

- [ ] **Object-Oriented Design**: Understand the process of **identifying and defining interacting objects** to structure a software system.
- [ ] **Design Patterns**: Learn these are **reusable solutions to common software design problems**, capturing best practices.
- [ ] **Software Reuse**: Emphasize its importance: **building systems using existing components or complete systems** to save time and cost.

#### Development Management

- [ ] **Configuration Management (CM)**: Understand it's the process of **managing changes to evolving software systems and their components** in a controlled manner.
- [ ] **Host-Target Development**: Know that **software is developed on one machine (host) and runs on another (target)**, common in embedded or distributed contexts.
- [ ] **Open-Source Development**: Understand its principles, different **licensing models (e.g., GPL, BSD)**, and their implications for using and contributing code.

### Software Testing and Verification

#### Testing Fundamentals

- [ ] **Verification and Validation (V&V)**: Understand **verification ("building the product right" - meeting specs)** and **validation ("building the right product" - meeting user needs)**.
- [ ] **Inspections**: Know these are **static V&V activities** where software artifacts are systematically reviewed for defects.
- [ ] **Software Testing**: Understand it's the **dynamic execution of a program** with test data to uncover defects.

#### Testing Levels

- [ ] **Development Testing**: Learn this includes **unit testing, component testing, and system testing**, typically done by the development team.
- [ ] **Unit Testing**: Understand it involves **testing individual smallest components (functions, methods)** in isolation.
- [ ] **Component Testing**: Know this focuses on **testing the interfaces and overall behavior of integrated components**.
- [ ] **System Testing**: Understand it tests the **complete, integrated system** for functional and non-functional requirements.

#### Testing Strategies

- [ ] **Test Case Design**: Learn strategies like **partition testing and guideline-based testing** for selecting effective inputs.
- [ ] **Regression Testing**: Understand it's about **re-running existing tests after code changes** to ensure no new bugs are introduced.
- [ ] **Release Testing**: Know this is a **formal testing phase** conducted by a separate team before software is released to customers.
- [ ] **Acceptance Testing**: Understand these are **customer-led tests** to decide if the system is acceptable for deployment.

### Software Evolution and Maintenance

#### Evolution Concepts

- [ ] **Software Evolution**: Understand it's the **process of changing a software system after it has been delivered and put into use**.
- [ ] **Software Maintenance**: Know it encompasses **modifying software to correct faults, improve performance, or adapt to environmental changes**.
- [ ] **Legacy Systems**: Understand these are **older, critical systems that are challenging to maintain** due to outdated technology or poor documentation.
- [ ] **Software Reengineering**: Understand it's about **restructuring or rewriting existing software to improve its maintainability** without changing functionality.

### Dependability and Security

#### Dependability Fundamentals

- [ ] **Dependability**: Understand it means a system can be **trusted to operate as expected**, encompassing availability, reliability, safety, security, and resilience.
- [ ] **Availability**: The **readiness of a system to deliver services** when requested.
- [ ] **Reliability**: The **ability of a system to deliver services correctly** as specified, without failures.
- [ ] **Safety**: The **ability of a system to operate without causing harm** to people or the environment.
- [ ] **Security**: The **ability of a system to protect itself from malicious attacks** (confidentiality, integrity, availability).
- [ ] **Resilience**: The **ability of a system to recover quickly** from failures or cyberattacks.

#### Dependability Engineering

- [ ] **Sociotechnical Systems**: Understand these are complex systems comprising **technical components, human operators, and organizational processes**.
- [ ] **Redundancy and Diversity**: Learn these are fundamental concepts for dependability; **redundancy (multiple copies)** provides backup, while **diversity (different approaches)** reduces common-mode failures.
- [ ] **Formal Methods**: Understand these are **mathematical approaches to software development** used for rigorous specification and verification, especially for critical systems.
- [ ] **Reliability Metrics**: Understand how to **quantify reliability** using measures like POFOD (Probability of Failure on Demand) and ROCOF (Rate of Occurrence of Failures).
- [ ] **Fault-Tolerant Architectures**: Explore designs that **allow a system to continue operating despite component failures**.
- [ ] **Programming for Reliability**: Learn **good coding practices** that reduce faults and improve system robustness (e.g., input validation, exception handling).

#### Safety Engineering

- [ ] **Safety-Critical Systems**: Understand these are systems where **failure can lead to severe consequences** (e.g., injury, death, environmental damage).
- [ ] **Hazard**: Know this is a **system state that can lead to an accident**.
- [ ] **Safety Requirements**: Learn these are often **"shall not" statements** defining unacceptable system behaviors.
- [ ] **Safety Cases**: Understand these are **structured arguments and evidence** used to demonstrate a system's safety.

#### Security Engineering

- [ ] **Security Engineering**: Focuses on **developing systems that can resist malicious attacks** (threats to confidentiality, integrity, availability).
- [ ] **Security Risk Assessment**: Learn the process of **identifying threats, vulnerabilities, and potential attacks** to system assets.
- [ ] **Misuse Cases**: Understand these are **use cases describing malicious interactions** with a system.
- [ ] **Secure Systems Design Principles**: Learn general guidelines for building security into systems, like **defense in depth** (multiple layers of protection).
- [ ] **Cybersecurity**: Know it's a **broader sociotechnical concern** encompassing technical, human, and organizational aspects of protecting systems from threats.
- [ ] **Four Rs of Resilience**: Learn the complementary strategies: **Recognition, Resistance, Recovery, and Reinstatement**.

### Component-Based and Distributed Systems

#### Component Engineering

- [ ] **Software Product Lines**: Understand this involves developing a **generic base system that can be configured** to create different, specialized product instances.
- [ ] **Component-Based Software Engineering (CBSE)**: Understand building systems by **composing standardized, reusable software components**.
- [ ] **Software Component**: Know it's a **self-contained, deployable, and documented service provider with well-defined interfaces**.
- [ ] **Component Composition**: Understand the process of **integrating components** to create new systems, often involving adaptors for interface compatibility.

#### Distributed Systems

- [ ] **Distributed Systems**: Understand these are systems where **components are deployed on different computers** across a network.
- [ ] **Scalability**: Know it's the ability of a system to **handle increasing workloads**, differentiating between scaling up (more resources on one machine) and scaling out (more machines).

#### Service-Oriented Architecture

- [ ] **Software as a Service (SaaS)**: Understand this model where **software is centrally hosted and licensed on subscription**, with considerations for multi-tenancy and scalability.
- [ ] **Service-Oriented Architecture (SOA)**: Understand this architectural style where **business logic is exposed as loosely coupled, reusable services**.
- [ ] **Web Services**: Know these are **standardized software modules accessible over the Internet**, typically described by WSDL or using RESTful principles.
- [ ] **RESTful Services**: Understand this **simpler approach to web services** based on transferring resource representations using HTTP methods.
- [ ] **Service Composition**: Understand how **multiple services are combined to create new applications or workflows**.

### Project Management

#### Project Management Fundamentals

- [ ] **Systems Engineering**: Understand it's concerned with **all aspects of complex system development**, integrating hardware, software, and human elements.
- [ ] **Project Management**: Understand its role in **ensuring software projects meet budget, schedule, and quality goals**.
- [ ] **Risk Management**: Know the process of **identifying, analyzing, planning for, and monitoring potential threats** to a project or product.
- [ ] **People Management**: Understand the importance of **motivating, organizing, and fostering teamwork** within a development team.

#### Planning and Estimation

- [ ] **Project Planning**: Understand it involves **breaking down work, estimating costs/effort, and scheduling tasks**, often using tools like Gantt charts.
- [ ] **Cost Estimation Techniques**: Learn about **experience-based and algorithmic approaches** (like COCOMO II) for predicting project effort and cost.

### Quality Management

#### Quality Assurance

- [ ] **Quality Management (QM)**: Understand its role in **ensuring software quality goals and standards are met**, often through independent checks and reviews.
- [ ] **Software Standards**: Learn they define **consistent processes and product attributes** to assure quality.
- [ ] **Reviews and Inspections (Quality)**: Understand these are formal activities for **systematically examining software and documentation** to find errors and ensure standards compliance.

#### Quality Measurement

- [ ] **Software Measurement/Metrics**: Know that quantitative data can be used to **assess software quality attributes** (e.g., size, complexity, defects).
- [ ] **Software Analytics**: Understand this involves **analyzing software development data to gain insights for better decision-making**.

### Configuration Management

#### Version Control

- [ ] **Configuration Item (CI)**: Know it's any **project artifact (code, docs, tests) placed under configuration control**.
- [ ] **Version Management**: Learn this involves **tracking different versions of software components** and allowing retrieval of past states.
- [ ] **Baselines**: Understand these are **defined system versions** that act as stable reference points.
- [ ] **Branching and Merging**: Understand these version control operations that enable **concurrent development** (branching) and **integrating changes** (merging).

#### Build and Release Management

- [ ] **System Building**: Understand it's the process of **compiling and linking components to create an executable system**, often automated.
- [ ] **Continuous Integration**: Know this practice involves **frequently merging code changes into a central repository** and running automated builds and tests.
- [ ] **Change Management (CM Context)**: Understand the formal process for **controlling changes to released software**, including analysis, approval, and tracking.
- [ ] **Release Management**: Learn this involves **preparing and distributing software versions to customers**.

## Algorithms
---

### Algorithm Analysis Fundamentals

#### Core Concepts

- [ ] **Algorithms Overview**: Understand what algorithms are, their role as a technology in computing, and the importance of analyzing their efficiency.
- [ ] **Pseudocode**: Learn to read and understand algorithms described in a clear, concise pseudocode format, similar to common programming languages like C, C++, Java, Python, or JavaScript.
- [ ] **RAM Model of Computation**: Grasp the basic model used for analyzing algorithms, where instructions and data accesses take constant time.

#### Running Time Analysis

- [ ] **Running Time Analysis (Asymptotic Notation)**: Master how to analyze an algorithm's running time, typically denoted as T(n), by summing the costs of executed statements, and express this using asymptotic notations (e.g., O, Ω, Θ-notation) to understand performance as input size grows.
- [ ] **Probabilistic Analysis and Randomized Algorithms**: Learn how to analyze algorithms by assuming a distribution on inputs (probabilistic analysis) or by making the algorithm's behavior random (randomized algorithms). This helps understand average-case behavior or ensures good performance for any input.

### Algorithm Design Paradigms

#### Fundamental Paradigms

- [ ] **Divide-and-Conquer**: A fundamental algorithm design technique where problems are broken into smaller subproblems, solved independently, and then combined.
- [ ] **Dynamic Programming**: A powerful algorithmic design technique used for optimization problems that exhibit **optimal substructure** and **overlapping subproblems**. Examples include rod cutting, matrix-chain multiplication, longest common subsequence, and optimal binary search trees.
- [ ] **Greedy Algorithms**: Another algorithmic design technique where locally optimal choices are made in the hope of finding a global optimum. Key properties are **greedy-choice property** and **optimal substructure**. Examples include activity selection and Huffman codes.

#### Advanced Analysis

- [ ] **Amortized Analysis**: Learn methods to analyze the average performance of a sequence of operations, rather than focusing on the worst-case for each individual operation.

### Data Structures

#### Elementary Data Structures

- [ ] **Arrays and Matrices**: Basic array-based storage.
- [ ] **Stacks**: LIFO (Last-In, First-Out) data structure, operations like PUSH and POP.
- [ ] **Queues**: FIFO (First-In, First-Out) data structure, operations like ENQUEUE and DEQUEUE.
- [ ] **Linked Lists**: Linear data structures where elements are linked using pointers (singly, doubly linked).
- [ ] **Rooted Trees**: Basic tree representation.

#### Hash Tables

- [ ] **Hash Tables**: Efficient data structures for dictionary operations (insert, delete, search) using hash functions to map keys to slots. Understand **direct-address tables**, **hash functions** (including practical considerations), and **open addressing** for collision resolution.

#### Tree Structures

- [ ] **Binary Search Trees**: Tree-based data structures where nodes are ordered, enabling efficient querying, insertion, and deletion (O(h) time, where h is height).
- [ ] **Red-Black Trees**: Self-balancing binary search trees that guarantee logarithmic time for operations by maintaining specific properties through rotations and color changes.
- [ ] **B-Trees**: Tree data structures optimized for disk-based storage, minimizing disk I/O operations.

#### Specialized Data Structures

- [ ] **Priority Queues**: Abstract data type that supports operations like inserting elements and extracting the minimum/maximum element (often implemented with heaps).
- [ ] **Augmenting Data Structures**: Learn the technique of adding extra information to existing data structures (like red-black trees) to support new operations, such as order-statistic operations (finding the i-th smallest) or interval searching.
- [ ] **Disjoint-Set Data Structures**: Manage a collection of disjoint sets, supporting operations like MAKE-SET, UNION, and FIND-SET.

### Sorting Algorithms

#### Comparison Sorts

- [ ] **Heapsort**: A comparison sort that uses a heap data structure.
- [ ] **Quicksort**: A widely used efficient comparison sort, focusing on its description, performance (including randomized versions), and analysis.
- [ ] **Merge Sort**: A comparison sort based on the divide-and-conquer paradigm.

#### Linear Time Sorting

- [ ] **Sorting in Linear Time**: Understand algorithms that can sort in O(n) time under specific conditions, such as **Counting Sort**, **Radix Sort**, and **Bucket Sort**.

#### Order Statistics

- [ ] **Medians and Order Statistics**: Learn algorithms for finding the i-th smallest element in an unsorted array, including minimum, maximum, and specialized linear-time selection algorithms like **RANDOMIZED-SELECT** and **SELECT**.

### Graph Algorithms

#### Graph Representation and Traversal

- [ ] **Graph Representations**: Understand adjacency lists (sparse graphs) and adjacency matrices (dense graphs).
- [ ] **Breadth-First Search (BFS)**: A graph traversal algorithm used to find shortest paths in unweighted graphs.
- [ ] **Depth-First Search (DFS)**: A graph traversal algorithm used for many applications, including topological sorting and finding strongly connected components.

#### Graph Properties and Structure

- [ ] **Topological Sorting**: Ordering vertices in a directed acyclic graph (DAG) such that for every directed edge u → v, u comes before v in the ordering.
- [ ] **Strongly Connected Components**: Decomposing a directed graph into its maximal strongly connected subgraphs.

#### Minimum Spanning Trees

- [ ] **Minimum Spanning Trees (MST)**: Algorithms (like Kruskal's or Prim's) to find a spanning tree of a weighted undirected graph with the minimum possible total edge weight.

#### Shortest Path Algorithms

- [ ] **Single-Source Shortest Paths**: Algorithms (like Dijkstra's or Bellman-Ford) to find the shortest paths from a single source vertex to all other vertices in a weighted, directed graph.
- [ ] **All-Pairs Shortest Paths**: Algorithms (like Floyd-Warshall or Johnson's) to find the shortest paths between all pairs of vertices in a weighted, directed graph.

#### Network Flow and Matching

- [ ] **Maximum Flow**: Problems involving flow networks, often solved using methods like Ford-Fulkerson.
- [ ] **Matchings in Bipartite Graphs**: Algorithms (like Hopcroft-Karp or Hungarian algorithm) for finding maximum cardinality or maximum weight matchings in bipartite graphs.
- [ ] **Stable-Marriage Problem**: A classic matching problem where preferences are considered to find a stable pairing.

### String Matching

#### String Matching Algorithms

- [ ] **Naive String-Matching Algorithm**: The brute-force approach.
- [ ] **Rabin-Karp Algorithm**: A randomized algorithm using hashing.
- [ ] **String Matching with Finite Automata**: Using finite automata for efficient matching.
- [ ] **Knuth-Morris-Pratt (KMP) Algorithm**: A linear-time algorithm that preprocesses the pattern to avoid unnecessary comparisons.
- [ ] **Suffix Arrays**: Data structures for advanced string operations like finding repeated substrings or common substrings between texts.

### Number-Theoretic Algorithms

#### Mathematical Algorithms

- [ ] **Greatest Common Divisor (GCD)**: Efficient algorithms for GCD.
- [ ] **Modular Arithmetic**: Operations with modular arithmetic.
- [ ] **Primality Testing**: Determining if a large number is prime.
- [ ] **RSA Public-Key Cryptosystem**: Understand the basics of this widely used cryptographic scheme.

### Advanced Topics

#### Optimization and Linear Programming

- [ ] **Linear Programming**: Learn how to **formulate optimization problems** mathematically by defining decision variables, constraints, and an objective function to be maximized or minimized. Understand the concept of duality.

#### Parallel and Online Algorithms

- [ ] **Parallel Algorithms**: Understand the basics of parallel computing models, including concepts like **fork-join parallelism** (spawn and sync keywords) and techniques for parallelizing common algorithms like merging.
- [ ] **Online Algorithms**: Algorithms that make decisions without knowing the future input. Evaluate their performance using **competitive analysis**, comparing against an optimal offline algorithm. This includes concepts like caching strategies.

#### Machine Learning Basics

- [ ] **Machine Learning (Basic Methods)**: Familiarize yourself with introductory algorithms such as **k-means clustering**, **weighted-majority algorithms**, and **gradient descent**.

### Priority Study Areas for Interview Preparation

#### Foundational Concepts (High Priority)

##### Algorithm Analysis Fundamentals

- [ ] **Algorithms and Efficiency**: Understand that algorithms are precise procedures for solving computational problems. Beyond just speed, other efficiency measures like power consumption or memory usage can be critical in real-world settings.
- [ ] **RAM Model of Computation**: Familiarize yourself with this generic, one-processor model, which assumes instructions and data accesses take a constant amount of time. This model is foundational for most algorithm analysis discussed in the sources.
- [ ] **Running Time Analysis**: Learn how to analyze an algorithm's running time by determining the number of times basic operations are executed. The aim is to predict how long an algorithm will take for a new input, independently of specific hardware or implementation details.
- [ ] **Asymptotic Notation (Big O, Omega, Theta)**: This is **crucial**. Master how to use Big O, Omega, and Theta (‚-notation) to describe the "order of growth" of an algorithm's running time, characterizing its performance as the input size (`n`) becomes large. Understand the distinction between **worst-case running time** (a guarantee that the algorithm will never take longer), **best-case running time**, and **average-case running time** (expected behavior over a distribution of inputs).

##### Algorithm Design Paradigms

- [ ] **Divide-and-Conquer**: Understand this strategy of breaking a problem into smaller, similar subproblems, solving them recursively, and then combining their solutions to solve the original problem. Analyzing its running time often involves recurrences.
- [ ] **Dynamic Programming**: Focus on its application to optimization problems that exhibit **optimal substructure** and **overlapping subproblems**. The running time of dynamic programming algorithms typically depends on the product of the total number of subproblems and the number of choices for each subproblem.
- [ ] **Greedy Algorithms**: Learn this approach where a locally optimal choice is made at each step in the hope of finding a globally optimal solution. It's important to understand _when_ greedy choices lead to optimal solutions, as not all greedy approaches are optimal.

#### Core Data Structures (High Priority)

##### Elementary Data Structures

- [ ] **Arrays**: Understand their basic structure as ordered collections.
- [ ] **Stacks**: A LIFO (Last-In, First-Out) data structure with operations like PUSH and POP.
- [ ] **Queues**: A FIFO (First-In, First-Out) data structure with operations like ENQUEUE and DEQUEUE.
- [ ] **Linked Lists**: Elements connected via pointers.

##### Hash Tables

- [ ] Understand how hash functions map keys to slots, enabling efficient dictionary operations (insert, delete, search).
- [ ] Know about **collision resolution** techniques, particularly chaining (using linked lists for elements that hash to the same slot).
- [ ] Understand that successful and unsuccessful searches often have an expected running time of O(1) or O(1 + α) depending on the load factor (α), assuming good hash functions.

##### Heaps and Priority Queues

- [ ] **Heaps**: Understand the heap property (e.g., max-heap where parent is greater than children).
- [ ] **Heapsort Algorithm**: Know that it's a comparison sort with an optimal worst-case running time of O(n log n).
- [ ] **Priority Queues**: Understand this abstract data type and how heaps efficiently implement it, supporting operations like `INSERT` and `EXTRACT-MAX` (or `MIN`) for scheduling jobs or events.

#### Important Sorting Algorithms (High Priority)

##### Comparison Sorts (General)

- [ ] Understand that any comparison sort algorithm requires at least O(n log n) comparisons in the worst case. This sets a lower bound for many common sorting methods.

##### Key Sorting Algorithms

- [ ] **Heapsort**: As mentioned above, it achieves the optimal worst-case O(n log n) for comparison sorts.
- [ ] **Quicksort**: While it has a worst-case running time of O(n^2), its **expected (average-case) running time is O(n log n)**, making it a highly practical choice, especially for large datasets. Emphasize the importance of its randomized version, which helps ensure good average performance across all inputs.
- [ ] **Merge Sort**: Offers a consistent O(n log n) running time in both worst-case and average-case scenarios. It's a classic example of divide-and-conquer.
- [ ] **Linear-Time Sorting Algorithms**: Be aware of algorithms like Counting Sort, Radix Sort, and Bucket Sort that can sort in O(n) time under specific assumptions about the input data (e.g., integers in a small range or uniformly distributed data). This demonstrates that the O(n log n) lower bound for comparison sorts can be "beaten" when these assumptions hold.

#### Other Essential Topics (Medium Priority, Time Permitting)

##### Medians and Order Statistics

- [ ] The problem of finding the _i_-th smallest element (order statistic).
- [ ] `RANDOMIZED-SELECT`: An algorithm that achieves an **expected linear running time** of O(n) for finding the _i_-th smallest element. Its worst-case is O(n^2).
- [ ] `SELECT`: A theoretically significant algorithm that guarantees **worst-case linear time** of O(n), although it is less practical than `RANDOMIZED-SELECT` due to larger constant factors.

##### Graph Algorithms

- [ ] **Graph Representations**: Understand how graphs can be represented, typically using adjacency lists or adjacency matrices.
- [ ] **Breadth-First Search (BFS)**: A graph traversal algorithm useful for finding shortest paths in unweighted graphs.
- [ ] **Depth-First Search (DFS)**: Another fundamental graph traversal, which has applications such as topological sorting.
- [ ] **Topological Sorting**: How to linearly order the vertices of a directed acyclic graph (DAG) such that for every directed edge (u, v), u comes before v in the ordering.

##### String Matching Algorithms

- [ ] **Naive String Matching**: The straightforward, brute-force approach.
- [ ] **Rabin-Karp Algorithm**: A string matching algorithm that uses hashing to efficiently find pattern occurrences, with an average-case expected matching time of O(n) if spurious hits are few.
- [ ] **Knuth-Morris-Pratt (KMP) Algorithm**: An efficient linear-time (O(n)) algorithm that preprocesses the pattern to avoid re-examining text characters during mismatches.