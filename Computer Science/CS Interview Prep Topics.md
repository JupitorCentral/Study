# CS Topics 

## Database
---

### Core Relational Model and SQL Mastery

####  Relational Data Model Fundamentals

- [x] **Definitions**: Be able to clearly define: ✅ 2025-08-05
  - [x] **Relation (Table)**: A collection of data organized into rows and columns. ✅ 2025-08-05
  - [x] **Tuple (Row)**: A single record in a table. ✅ 2025-08-05
  - [x] **Attribute (Column)**: A named column representing a property. ✅ 2025-08-05
  - [x] **Domain**: The set of allowed values for an attribute. ✅ 2025-08-05

- [x] **Keys (Critical)**: Understand and differentiate: ✅ 2025-08-05
  - [x] **Superkey**: Uniquely identifies a tuple. ✅ 2025-08-05
  - [x] **Candidate Key**: A minimal superkey. A relation can have multiple candidate keys. ✅ 2025-08-05
  - [x] **Primary Key**: The chosen candidate key to uniquely identify tuples, often underlined. Every relation schema should have one. ✅ 2025-08-05
  - [x] **Foreign Key**: Attribute(s) in one table that refer to the primary key of another table, linking them and enforcing referential integrity. Be aware of `ON DELETE` and `ON UPDATE` actions like `CASCADE` and `SET NULL`. ✅ 2025-08-05
  - [x] **Prime vs. Nonprime Attributes**: An attribute is prime if it's part of _any_ candidate key. ✅ 2025-08-05

- [x] **NULL Values**: Understand their meanings (unknown, not applicable, value withheld) and that SQL does not distinguish between them. ✅ 2025-08-05

When preparing for a tech interview focused on database systems and practical skills, it is highly likely that interviewers will inquire about topics that directly relate to designing, implementing, and optimizing real-world database applications. Drawing from the provided textbook, here are some key areas you should focus on:

### 1. SQL Proficiency

**SQL (Standard Query Language)** is foundational and considered a major reason for the commercial success of relational databases. Your understanding of SQL will be tested extensively.

- **Data Definition Language (DDL)**: Be prepared to discuss `CREATE` statements for defining schemas, tables (relations), and domains. Understanding how to specify **constraints**, such as **key constraints** (primary keys, unique keys) and **referential integrity constraints** (foreign keys) is critical. You should also be familiar with `CREATE VIEW` for defining virtual tables and commands for schema modification.
- **Data Manipulation Language (DML)**: Expect questions on `INSERT`, `DELETE`, and `UPDATE` statements for modifying data.
- **Basic and Complex Queries**: Interviewers will likely ask about `SELECT` statements, including filtering data with `WHERE` clauses, retrieving specific attributes, and using `JOIN` operations to combine data from multiple tables. More advanced topics include:
    - **Nested Queries** (subqueries): Complete `SELECT-FROM-WHERE` blocks within the `WHERE` clause of another query. Be aware of how they can be used for complex comparisons and how attribute scope rules apply.
    - **Aggregate Functions**: Functions like `MAX`, `MIN`, `SUM`, `COUNT`, and `AVG` for summarizing data, and how `NULL` values are handled.
    - **Grouping Results**: Using `GROUP BY` and `HAVING` clauses to apply aggregate functions to subsets of tuples.
    - **String Pattern Matching**: Using the `LIKE` operator with wildcard characters like `%` and `_`.
    - **Views (Virtual Tables)**: How to define them and the challenges and considerations around their updatability, including view materialization.
- **Triggers and Assertions**: Mechanisms for specifying more general constraints and defining automated actions in response to data modifications.

### 2. Relational Database Design & Normalization

These concepts are essential for creating robust, consistent, and efficient database schemas, minimizing redundancy, and preventing update anomalies.

- **Conceptual Modeling (ER/EER Model)**: A strong understanding of the **Entity-Relationship (ER) model** and its extensions (EER) is paramount. This includes identifying **entity types**, **attributes** (simple, composite, multivalued, derived), **relationship types** (binary, n-ary, recursive), and their **structural constraints** (cardinality ratios like 1:1, 1:N, M:N, and participation constraints like total/partial). You should also be familiar with **weak entity types** and their identifying relationships.
- **Logical Design (ER/EER to Relational Mapping)**: Expect questions on how to transform a conceptual ER/EER design into a relational schema. This involves understanding the mapping rules for entity types, relationship types (including 1:1, 1:N, M:N), and specialization/generalization hierarchies. You should be able to explain how foreign key/primary key correspondences are established to link related tables.
- **Normalization Theory**: This formal framework evaluates relational schemas for design quality.
    - **Informal Design Guidelines**: Discuss the importance of clear attribute semantics, reducing redundant information and update anomalies (insertion, deletion, modification), minimizing `NULL` values, and avoiding spurious tuple generation.
    - **Functional Dependencies (FDs)**: Define what an FD is and how they are used to determine normal forms. Knowledge of inference rules and minimal cover is also part of this.
    - **Normal Forms**: Be able to define and explain the practical implications of **First Normal Form (1NF)**, **Second Normal Form (2NF)**, **Third Normal Form (3NF)**, and **Boyce-Codd Normal Form (BCNF)**. Understand how decomposition is used to achieve higher normal forms, typically aiming for **BCNF as the practical goal for commercial designs**.
    - **Multivalued Dependencies (MVDs) and Fourth Normal Form (4NF)**: Understand what MVDs are and how 4NF addresses redundancy caused by independent multivalued attributes.
    - **Desirable Properties of Decomposition**: Understanding **dependency preservation** and the **nonadditive join (lossless join) property**, which are critical for maintaining data integrity and avoiding information loss during decomposition. The lossless join property is an absolute must.

### 3. Database Programming Techniques

Interviewers will assess how you connect applications to databases.

- **Application Programming Interfaces (APIs)**: Familiarity with **JDBC** (Java Database Connectivity) and **SQL/CLI** (Call Level Interface) for dynamic database access from programming languages like Java or C.
- **Embedded SQL**: Understanding how SQL statements are embedded directly into a host language's source code (e.g., C, Java with SQLJ).
- **Impedance Mismatch**: Be prepared to discuss this challenge when combining object-oriented or procedural programming languages with the relational model.
- **Persistent Stored Modules (SQL/PSM)**: Knowledge of **stored procedures** and functions that reside within the DBMS for better performance and reusability.

### 4. File Structures, Indexing, and Physical Database Design

These topics directly impact database performance and are crucial for practical optimization.

- **Disk Storage Characteristics**: Understand the components of disk storage (tracks, cylinders, blocks) and the factors contributing to **disk access time** (seek time, rotational delay, transfer time). The goal of good file organization is to minimize block transfers.
- **Primary File Organizations**: Discuss basic methods like **unordered (heap) files**, **ordered (sequential) files**, and **static hashing**.
- **Indexing Techniques**:
    - **Types of Indexes**: Understand **primary, clustering, and secondary indexes**, and their distinct characteristics and use cases. A file can have at most one primary/clustering index but multiple secondary indexes.
    - **Multilevel Indexes**: How they improve search efficiency for large index files by reducing disk accesses.
    - **B-trees and B+-trees**: These are crucial and commonly used indexing structures in relational DBMSs. Be prepared to discuss their structure (internal and leaf nodes), how they balance the tree, and why **B+-trees are generally preferred** over B-trees for database indexing (more entries per internal node, fewer levels).
    - **Hash Indexes**: Using hashing as a secondary access method.
    - **Bitmap Indexes**: Their applicability for attributes with a small number of distinct values and their use in complex queries by bitvector operations.
    - **Physical vs. Logical Indexes**: Understand the difference and implications for record movement.
    - **RAID Technology**: Understand how Redundant Arrays of Inexpensive/Independent Disks (RAID) improve both **performance and reliability** of secondary storage.
- **Physical Database Design & Tuning**: This involves choosing appropriate file structures and access paths to meet specific performance objectives like **response time** and **transaction throughput**. Tuning is an ongoing activity throughout the database's lifecycle.

### 5. Query Processing and Optimization

Understanding how a DBMS executes queries efficiently is a highly practical skill.

- **Query Optimization Process**: SQL queries are translated into relational algebra expressions and then optimized. Understand that "optimization" aims for a **reasonably efficient strategy**, not necessarily the absolute optimal one due to complexity.
- **Algorithms for Relational Operations**: Be familiar with common algorithms for implementing `SELECT` (e.g., linear search, binary search, index scans, selectivity of conditions) and `JOIN` (e.g., **nested-loop join**, **sort-merge join**, **partition-hash join**). Understand how factors like **buffer space** and **join selection factor** influence performance.
- **Heuristics and Cost Estimation**: How query optimizers use **heuristic rules** and **cost estimates** (often based on **selectivity** of conditions) from the DBMS catalog to choose an execution plan.

### 6. Transaction Processing

This is crucial for ensuring data consistency and reliability in multi-user environments.

- **ACID Properties**: Understand **Atomicity, Consistency, Isolation, and Durability** as desirable properties of transactions.
- **Concurrency Control**: Mechanisms (e.g., two-phase locking) to manage simultaneous access by multiple transactions to prevent conflicts and ensure correct execution.
- **Database Recovery**: Techniques for restoring the database to a consistent state after system failures, including concepts like database backup.

### 7. Database System Architecture

Basic knowledge of how different components interact is valuable.

- **DBMS Components**: Understand the roles of key modules such as the DDL compiler, query optimizer, and runtime database processor, and their interaction with the system catalog (metadata).
- **Client/Server Architectures**: Familiarity with two-tier and three-tier (or n-tier) architectures for database applications.

By focusing on these areas, you will demonstrate a solid understanding of both theoretical concepts and practical application in database systems, which is highly valued in tech interviews.




## Computer Network
---

When preparing for a tech interview focused on practical skills, an interviewer will likely delve into your understanding of how networks actually work, how to implement or configure them, and how to troubleshoot common issues. Drawing from the provided sources, here's a breakdown of highly probable topics, organized for clarity:

### 1. Network Fundamentals and Architecture

#### Foundational Concepts

- **Protocols**: Understand what a protocol defines (format and order of messages, actions on transmission/receipt) and why standards are crucial for interoperability.
- **Internet Protocol Stack**: Be able to name and explain the principal responsibilities of each of the **five layers**: application, transport, network, link, and physical.
- **Encapsulation and De-encapsulation**: Explain how data is wrapped and unwrapped as it moves through the layers.
- **Top-Down Approach**: The book emphasizes learning from the application layer down. This means understanding how applications drive network requirements.

#### Network Types and Components

- **Network Edge vs. Network Core**: Differentiate between end systems (hosts, clients, servers, data centers) at the edge and packet switches (routers, link-layer switches) in the core.
- **Packet Switching vs. Circuit Switching**: Understand the fundamental differences, including how resources are allocated, and their respective advantages and disadvantages (e.g., wastefulness of idle circuits in circuit switching).
- **Delays in Packet-Switched Networks**: Be prepared to discuss and quantify:
    - **Processing delay**: Time to examine packet header and direct it.
    - **Queuing delay**: Time spent waiting in a buffer, especially due to **congestion**.
    - **Transmission delay**: Time to push all bits onto the link (packet length / transmission rate).
    - **Propagation delay**: Time for bits to travel across the link.
    - **Packet Loss**: Occurs when buffers overflow.
- **Throughput**: Define and explain average and instantaneous throughput, and how congestion affects it.

### 2. Application Layer

#### Core Application Concepts

- **Network Application Architectures**: Compare and contrast **client-server** and **peer-to-peer (P2P)** architectures, including their use cases and programming paradigms.
- **Processes and Sockets**: Understand that applications communicate via processes and that a **socket** is the interface between an application process and the transport layer. Know the role of **port numbers** (well-known vs. ephemeral).
- **Transport Services**: This is a critical practical decision. Explain the services offered by **TCP** (connection-oriented, reliable data transfer, congestion control, flow control) and **UDP** (connectionless, unreliable data transfer). Be ready to justify _why_ a particular application would choose one over the other (e.g., DNS over UDP for speed, HTTP over TCP for reliability).

#### Key Application Protocols

- **HTTP (Web)**:
    - **Non-persistent vs. Persistent Connections**: Explain the difference and their performance implications.
    - **HTTP/1.1 Pipelining**: How multiple requests can be sent without waiting for responses.
    - **Web Caching**: Explain how **conditional GET** works to optimize performance and freshness of content.
    - **HTTP/2**: Understand its advancements like **framing** (breaking messages into frames for interleaving) to mitigate **Head-of-Line (HOL) blocking** and support request/response prioritization.
    - **HTTP/3**: Know that it operates over **QUIC** (which uses UDP) and inherits features like stream multiplexing for lower latency connection establishment.
- **Email (SMTP)**: Understand the **SMTP dialogue** (commands like HELO, MAIL FROM, RCPT TO, DATA, QUIT) and its use of persistent connections.
- **DNS (Domain Name System)**:
    - Explain its **distributed, hierarchical database** structure.
    - Differentiate between **recursive and iterative queries**.
    - Know the purpose of different **resource records** (Type A for IP address, Type MX for mail server).
    - Be familiar with tools like `nslookup` for practical DNS querying.
- **P2P File Distribution (BitTorrent)**: Understand concepts like "rarest first" and how peers exchange chunks.

#### Practical Application Development

- **Socket Programming**: This is a direct practical skill. Be prepared to discuss how to write basic **TCP and UDP client-server programs**, including setting up sockets and handling data transfer. Familiarity with Python for socket programming is beneficial.

### 3. Transport Layer

#### Core Transport Concepts

- **Multiplexing and Demultiplexing**: Explain how source and destination **port numbers** enable this at the transport layer.
- **UDP**: Understand its segment structure (port numbers, length, checksum) and its "no-frills" nature. Discuss scenarios where UDP is preferred over TCP.

#### Reliable Data Transfer (RDT)

- **Principles**: Explain the fundamental mechanisms: **sequence numbers**, **acknowledgments (ACKs/NAKs)**, **timers**, and **retransmissions**.
- **Pipelining**: Differentiate between **Go-Back-N (GBN)** and **Selective Repeat (SR)** protocols in terms of how they handle lost packets and their buffering requirements. TCP uses a GBN-like approach with cumulative ACKs.

#### TCP in Detail

- **TCP Connection Management**:
    - The **three-way handshake** (SYN, SYNACK, ACK) for connection establishment. Understand why initial sequence numbers are randomized and why a three-way handshake is necessary (vs. two-way).
    - **SYN flood attack**: Explain how this denial-of-service attack exploits the three-way handshake and potential mitigations.
    - Connection teardown process (FIN).
- **TCP Segment Structure**: Know the important fields, especially **sequence number** (counts bytes, not segments), **acknowledgment number** (cumulative, next expected byte), and **receive window** (for flow control).
- **Round-Trip Time (RTT) Estimation**: Explain how TCP estimates RTT (SampleRTT, EstimatedRTT using Exponential Weighted Moving Average - EWMA) and calculates the **timeout interval**. Understand why SampleRTT is not measured for retransmitted segments.
- **Fast Retransmit**: How TCP retransmits a lost segment quickly upon receiving **three duplicate ACKs**, without waiting for a timeout.
- **Flow Control**: How TCP prevents the sender from overwhelming the receiver's buffer using the receive window.
- **Congestion Control**: This is a high-priority topic.
    - **Causes and Costs**: Why congestion is bad (packet loss, increased delay, reduced throughput).
    - **TCP's Approach (End-to-End)**: How TCP infers congestion from packet loss.
    - **Algorithms**: Understand **Slow Start**, **Congestion Avoidance** (Additive Increase Multiplicative Decrease - AIMD), and **Fast Recovery/Fast Retransmit**.
    - Familiarity with modern TCP variants like **TCP CUBIC** and **BBR** (delay-based).
    - **Fairness** in bandwidth sharing among competing TCP connections.

### 4. Network Layer: Data Plane

#### Router Internals

- **Forwarding vs. Routing**: Understand the distinction between the per-router action of forwarding (data plane) and the network-wide process of routing (control plane).
- **Router Architecture**: Be able to describe the components: input ports, switching fabric, and output ports.
- **Packet Processing**:
    - **Longest Prefix Matching**: How routers determine the output port for a destination IP address.
    - **Switching Fabric**: Mechanisms for moving packets from input to output ports.
    - **Queuing and Packet Loss**: Where queuing occurs at input/output ports. Understand **Head-of-Line (HOL) blocking** and strategies to manage queues like **Active Queue Management (AQM)** (e.g., Random Early Detection - RED, PIE, CoDel).
    - **Packet Scheduling**: Disciplines like **FIFO**, **Priority queuing**, **Round Robin (RR)**, and **Weighted Fair Queuing (WFQ)**.

#### IP Addressing and Protocols

- **IPv4 Datagram Format**: Know key fields like **Time-to-Live (TTL)** (prevents infinite loops) and the **Protocol field** (identifies upper-layer protocol like TCP/UDP).
- **IP Addressing**: Concepts like IP address structure, **subnetting**, and how a router has multiple IP addresses.
- **Network Address Translation (NAT)**: Explain how NAT works, its benefits (address space conservation, security by hiding internal topology), and its drawbacks (e.g., breaking end-to-end connectivity for P2P applications).
- **IPv6**: Understand its improvements over IPv4 (larger address space, simplified header) and the concept of **tunneling** IPv6 over IPv4.

#### Modern Network Concepts

- **Generalized Forwarding and SDN**:
    - **Match-Plus-Action Paradigm (OpenFlow)**: Explain how forwarding decisions can be made based on multiple header fields (from link, network, and transport layers) and associated actions (forward, drop, modify).
    - **Software-Defined Networking (SDN)**: The separation of the data plane and control plane, with a logically centralized controller managing forwarding behavior.
- **Middleboxes**: Devices that perform functions beyond basic routing/forwarding, such as **firewalling** and **load balancing**. Understand **Network Function Virtualization (NFV)** as an approach to implement these functions on commodity hardware.

### 5. Network Layer: Control Plane

#### Routing Algorithms and Protocols

- **Routing Algorithms**: General goal (determine good paths, typically least cost).
    - **Link-State (LS)**: Each router computes its own map of the network.
    - **Distance-Vector (DV)**: Routers exchange distance vectors with neighbors.
- **Intra-AS Routing (OSPF)**: Understand its role within a single Autonomous System (AS).
- **Inter-AS Routing (BGP)**: The "glue" that holds the Internet together. Be prepared to discuss its role in exchanging reachability information between ISPs and implementing **routing policies**.
- **IP-Anycast**: How BGP is used to direct users to the "nearest" server with replicated content (e.g., for DNS).

#### Network Management

- **ICMP (Internet Control Message Protocol)**: Its role in error reporting and network diagnostics (e.g., `ping`, `traceroute`).
- **SNMP (Simple Network Management Protocol)**: Architecture (managing server, managed device, MIBs, agents), common messages (GetRequest, SetRequest, Trap).
- **NETCONF/YANG**: Newer, more programmatic approaches to network configuration using XML-based messages and data modeling.
- **Command Line Interface (CLI)**: Direct configuration.

### 6. Link Layer and LANs

#### Core Link Layer Concepts

- **Error Detection and Correction**: Techniques like **checksumming** and **Cyclic Redundancy Check (CRC)**.
- **Multiple Access Protocols**: How nodes share a broadcast channel (e.g., LANs, wireless).
    - **Channel Partitioning**: Time-Division Multiplexing (TDM), Frequency-Division Multiplexing (FDM).
    - **Random Access**: **CSMA/CD (Ethernet's MAC protocol)**, ALOHA. Understand collision detection/avoidance and binary exponential backoff.
    - **Taking-Turns**: Polling, Token Passing.
- **Link-Layer Addressing (MAC Addresses)**: Physical addresses, distinct from IP addresses.
- **Address Resolution Protocol (ARP)**: How IP addresses are translated to MAC addresses within a subnet.

#### LAN Technologies

- **Ethernet**: Understand its evolution, different speeds, and how **switched Ethernet** operates (e.g., full-duplex, no collisions in switched environments). How switches build forwarding tables (self-learning).
- **VLANs (Virtual Local Area Networks)**: How they segment a single physical LAN into multiple logical LANs.
- **Data Center Networking**: Focus on the unique challenges and solutions in data centers, including hierarchical topologies, **load balancing**, redundancy, and specialized congestion control due to very high capacity and low delays.

### 7. Wireless and Mobile Networks

#### Wireless Concepts

- **Wireless vs. Mobility**: Understand the distinction between challenges posed by wireless links (e.g., signal strength, interference, multipath propagation, bit errors) and those by host mobility.
- **WiFi (IEEE 802.11)**:
    - **Architecture**: Access Points (APs), Basic Service Set (BSS).
    - **MAC Protocol**: **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance), and the role of acknowledgments.
    - **Beacon Frames**: Their purpose in network discovery.
    - **Mobility within a Subnet**: How stations move between BSSs.
- **Cellular Networks (4G/5G LTE)**: Understand the **all-IP core network** architecture. Concepts like **tunnels** (e.g., GTP) for user data paths.
- **Mobility Management**:
    - **Home Network vs. Visited Network**.
    - Role of entities like **Mobility Management Entity (MME)** and **Home Subscriber Service (HSS)**.
    - **Indirect vs. Direct Routing** to mobile devices.
    - **Handovers**: Seamless transition between base stations.
- **Impact on TCP**: How wireless link characteristics (high error rates, handovers) can negatively impact TCP's congestion control, and approaches to mitigate this (local recovery, TCP sender awareness).

### 8. Network Security

#### Fundamental Security Principles

- **Security Goals**: Understand confidentiality, message integrity, and end-point authentication.
- **Cryptography**:
    - **Symmetric Key Encryption**: Principles (single shared key), algorithms (DES, AES).
    - **Public Key Encryption**: Principles (public/private key pair), algorithms (RSA, Diffie-Hellman). Key distribution and Public Key Infrastructure (PKI).
- **Message Integrity**:
    - **Cryptographic Hash Functions**: Properties (fixed size, one-way, collision-resistant).
    - **Message Authentication Code (MAC)**: How it provides integrity and authentication.
    - **Digital Signatures**: How they provide verifiability and non-forgeability.
- **End-Point Authentication**: How one entity proves its identity to another. The use of **nonces** to prevent replay attacks. Be aware of vulnerabilities like **IP spoofing** and simple password-based authentication.

#### Secure Networking Protocols

- **TLS (Transport Layer Security / SSL)**:
    - How it secures TCP connections.
    - **TLS Handshake**: Key steps (client hello, server certificate, key exchange, session key derivation).
    - **Data Transfer**: How records are encrypted and integrity-protected with HMAC and sequence numbers.
- **IPsec (IP Security)**:
    - Provides security at the **network layer**.
    - Used to create **Virtual Private Networks (VPNs)** over the public Internet.
    - Understand **Security Associations (SAs)** and how they define security parameters.
- **Wireless LAN Security (802.11)**: Evolution of security (WEP flaws to WPA/WPA2/WPA3 - though WPA3 not detailed, the principle of evolution is mentioned), mutual authentication and key agreement.
- **Operational Security**:
    - **Firewalls**: Different types (traditional packet filters, stateful filters, application gateways) and their filtering capabilities (based on IP addresses, port numbers, flag bits, or even application-layer data).
    - **Intrusion Detection Systems (IDS)**: Used for deep packet inspection to detect attacks.

Interviewers often combine these topics into scenario-based questions to test your ability to apply knowledge practically. For example, "Describe what happens when you type a URL into a browser, tracing the protocols and actions across different layers." This integrates HTTP, DNS, TCP, IP, ARP, Ethernet, and potentially DHCP and NAT.







## Operating System
---


### Process Management

Understanding how applications run and interact is fundamental.

- #### **Processes and Threads**
    
    - **Process Concept**: Be ready to define what a **process** is, how it differs from a program, and its various states (new, ready, running, waiting, terminated).
    - **Multithreading**: Explain why applications use threads (e.g., for **responsiveness**, **resource sharing**, **computation speedup** on **multicore systems**). Discuss challenges in programming for multicore systems, such as dividing activities, balancing work, data splitting, and managing data dependencies. Distinguish between user-level and kernel-level threads.
- #### **Interprocess Communication (IPC)**
    
    - **Models**: Understand the two primary models: **shared memory** and **message passing**. Know their trade-offs, particularly regarding efficiency and flexibility.
    - **Mechanisms**: Be familiar with practical IPC methods:
        - **Pipes**: How ordinary pipes enable unidirectional producer-consumer communication (e.g., `ls | more` in UNIX).
        - **Sockets**: Explain their use for network communication in client-server systems.
        - **Remote Procedure Calls (RPCs)**: Understand RPC as a higher-level abstraction for inter-process communication, especially in distributed systems, and concepts like parameter marshaling and "exactly once" semantics.
- #### **Process Synchronization**
    
    - **Race Conditions and Critical Sections**: Explain what a **race condition** is and why the **critical-section problem** needs to be solved to ensure data consistency when multiple processes/threads access shared data concurrently.
    - **Synchronization Tools**: This is a critical area.
        - **Mutex Locks (Binary Semaphores)**: How they provide **mutual exclusion** to protect critical sections.
        - **Counting Semaphores**: Their general use for coordinating access to resources.
        - **Classic Problems**: Be prepared to discuss common synchronization problems like the **Bounded-Buffer Problem** and the **Readers-Writers Problem**, as they serve as paradigms for concurrency control.
        - **Monitors**: Understand monitors as a high-level language construct for synchronization.
        - **Spinlocks**: Know when they are appropriate (e.g., in multiprocessor systems for short-duration locks).
        - **Priority Inversion**: What this problem is and how it's addressed (e.g., through priority-inheritance protocols).
- #### **Deadlocks**
    
    - **Conditions**: List and explain the four necessary conditions for deadlock (mutual exclusion, hold and wait, no preemption, circular wait).
    - **Handling**: Discuss the three main approaches: **prevention**, **avoidance**, and **detection & recovery**. Be aware that many operating systems, including UNIX and Windows, often **ignore** the problem and rely on application developers to manage it.

### Memory Management

Efficient memory usage is crucial for application performance.

- #### **Virtual Memory Concepts**
    
    - **Purpose**: Explain how **virtual memory** allows programs larger than physical memory to run, increases the degree of multiprogramming, and simplifies programming.
    - **Demand Paging**: Describe the mechanism of **demand paging**, where pages are loaded only as needed, and the concept of a **page fault**.
    - **Page Replacement**: Know why **page replacement algorithms** are necessary and be able to compare them. Focus on:
        - **FIFO (First-In, First-Out)**: Simple but can suffer from **Belady's anomaly**.
        - **LRU (Least Recently Used)**: Considered optimal among practical algorithms but difficult to implement precisely without hardware support.
        - **LRU Approximations**: Algorithms like the **second-chance** or **clock algorithm** that use a **reference bit** for more practical implementation.
    - **Thrashing**: Understand what **thrashing** is (high page-fault rate leading to low CPU utilization) and strategies to prevent it, such as the **working-set model** or **page-fault frequency**.
    - **Memory-Mapped Files**: Explain how they enable efficient file I/O by treating file access as routine memory access, and how they facilitate **file sharing**.
- #### **Address Translation and Hardware Support**
    
    - **Paging and Segmentation**: Basic concepts of how logical addresses are mapped to physical addresses.
    - **TLB (Translation Look-aside Buffer)**: Explain its role as a hardware cache for page-table entries to speed up address translation, and discuss **hit ratio** and **TLB reach**.

### Storage Management (File Systems & Disks)

Practical skills often involve interacting with file systems and understanding disk performance.

- #### **File System Interface**
    
    - **Basic File Operations**: Describe common system calls like `open()`, `read()`, `write()`, `close()`, `create()`, and `delete()`.
    - **File Access Methods**: Discuss the difference between **sequential access** and **direct (random) access** and when each is appropriate.
    - **File Sharing and Protection**:
        - **File Locks**: Explain **mandatory** versus **advisory locks** and their importance for concurrent file access.
        - **Access Control**: How systems protect files using methods like **access-control lists (ACLs)** or owner/group/universe classifications.
- #### **Disk Management and Scheduling**
    
    - **Disk Structure**: Basic understanding of cylinders, tracks, and sectors.
    - **Disk Scheduling Algorithms**: Explain algorithms like **FCFS (First-Come, First-Served)**, **SSTF (Shortest-Seek-Time-First)**, **SCAN** (Elevator), and **C-SCAN** (Circular SCAN). Discuss how they aim to optimize disk I/O performance by minimizing head movement.
    - **Swap-Space Management**: How disk space is used as an extension of main memory (virtual memory) and how it's managed for performance.
- #### **File System Implementation Details**
    
    - **Caching**: How caching (e.g., page cache, buffer cache) is used to improve file system performance.
    - **Journaling/Log-Structured File Systems**: Understand how these improve file system consistency and recovery after system crashes by logging metadata changes.

### Fundamental System Concepts

- #### **System Calls**: Explain their purpose as the interface between user programs and the operating system services, and how they involve a change in execution context and privileges (user mode to kernel mode).
    
- #### **Kernel Mode vs. User Mode**: Clearly articulate the distinction and its importance for system protection and security.
    
- #### **Caching**: General principle of caching (copying information into a faster storage system temporarily) and its widespread use in computer systems to improve performance.
    






## Algorithms
---


When preparing for a tech interview, especially focusing on practical skills, interviewers commonly inquire about fundamental algorithms, data structures, and analysis techniques. These topics form the bedrock of efficient software development.

### Foundational Concepts and Analysis Techniques

A strong understanding of how to analyze and evaluate algorithms is crucial.

- **Algorithm Analysis and Asymptotic Notation:**
    - **Running Time Analysis:** Understanding how the execution time of an algorithm increases with input size, particularly for large inputs (asymptotic efficiency).
    - **Big-O, Big-Omega, Theta (O, Ω, Θ) Notation:** Standard methods for simplifying asymptotic analysis to describe upper bounds, lower bounds, and tight bounds on running times. This helps in comparing the relative performance of different algorithms.
    - **RAM Model of Computation:** The generic one-processor, random-access machine (RAM) model is typically assumed, where each instruction and data access takes a constant amount of time.
- **Recurrence Relations:**
    - **Solving Recurrences:** Mathematical tools to obtain asymptotic bounds for the running times of recursive algorithms.
    - **Substitution Method:** Guessing a bound and proving it correct.
    - **Recursion Trees:** A visual method to generate intuition for a good guess for a recurrence solution, and can be used for direct proof.
    - **Master Method:** A "cookbook" method for solving common recurrences of the form T(n) = aT(n/b) + f(n), frequently arising in divide-and-conquer algorithms.
- **Probabilistic Analysis and Randomized Algorithms:**
    - **Probabilistic Analysis:** Using probability to analyze the average-case running time of an algorithm, often by making assumptions about the input distribution (e.g., random order of candidates in the hiring problem).
    - **Randomized Algorithms:** Algorithms that make random choices, which can lead to good expected performance for any input, even if the input distribution is unknown. This is achieved by imposing a distribution (e.g., randomly permuting the input).

### Core Data Structures

Understanding various data structures and their strengths and limitations is fundamental for efficient algorithm design.

- **Arrays and Linked Lists:**
    - **Arrays and Matrices:** Basic methods for storing data.
    - **Linked Lists:** Including singly and doubly linked lists, with operations like insertion (O(1) for singly linked) and deletion.
- **Stacks and Queues:**
    - These fundamental data structures are often implemented using linked lists or arrays.
- **Hash Tables:**
    - **Concept:** Generalize arrays by computing an index from a key, highly effective and practical for dictionary operations.
    - **Operations:** Support basic dictionary operations (INSERT, DELETE, SEARCH) in **O(1) expected time**.
    - **Collision Resolution:**
        - **Chaining:** Each slot points to a linked list of keys that hash to the same index.
        - **Open Addressing:** Such as linear probing and double hashing, where all elements are stored in the hash table itself. Linear probing is efficient due to memory hierarchies.
    - **Hash Functions:** Methods for computing array indices from keys, including considerations for practical approximations to uniform hashing.
- **Trees:**
    - **Binary Search Trees (BSTs):** Basic structure for storing ordered data, with search, insertion, and deletion operations typically running in time proportional to the height of the tree.
    - **Red-Black Trees:** A type of self-balancing binary search tree that guarantees **O(lg n) time** for dictionary operations (search, insert, delete). This ensures predictable performance even in worst-case scenarios.
    - **Heaps and Priority Queues:** Used for efficient retrieval of the minimum or maximum element, with operations like INSERT, EXTRACT-MIN/MAX, and DECREASE-KEY. Often implemented using arrays for complete binary trees.

### Fundamental Algorithms

Interviewers frequently test knowledge of classic algorithms and their applications.

- **Sorting Algorithms:**
    - **Insertion Sort:** A simple incremental sorting algorithm, efficient for small input sizes and sorts in place.
    - **Merge Sort:** A divide-and-conquer algorithm with a **better asymptotic running time of O(n lg n)**, suitable for large datasets.
    - **Quicksort:** A highly practical and fast algorithm, especially its randomized version, often chosen for sorting large data sets in software libraries. It typically has an **expected running time of O(n lg n)**.
    - Understanding the trade-offs (time complexity, space complexity, stability, in-place sorting) between different sorting algorithms is important.
- **Graph Algorithms:**
    - **Representations:** Adjacency lists (preferred for sparse graphs) and adjacency matrices (for dense graphs or quick edge checks).
    - **Breadth-First Search (BFS):** For finding shortest paths in unweighted graphs and exploring graphs level by level.
    - **Depth-First Search (DFS):** For exploring paths as deeply as possible before backtracking. Applications include topological sorting and finding strongly connected components.
    - **Topological Sort:** For ordering tasks or events with dependencies in a directed acyclic graph (DAG).
    - **Shortest Path Algorithms:** Such as **Dijkstra's algorithm** for single-source shortest paths in graphs with nonnegative edge weights. Understanding its use with priority queues is key.
- **String Matching:**
    - **Rabin-Karp Algorithm:** Uses hashing for pattern searching, with **O(n+m) expected time** (where n is text length, m is pattern length).
    - **Knuth-Morris-Pratt (KMP) Algorithm:** A linear-time string matching algorithm that efficiently preprocesses the pattern to avoid re-examining characters.

### Advanced Design Paradigms

Beyond basic algorithms, interviewers may ask about common problem-solving approaches.

- **Dynamic Programming:**
    - **Core Concepts:** Identifying **optimal substructure** (optimal solutions contain optimal solutions to subproblems) and **overlapping subproblems** (subproblems are reused multiple times).
    - **Approaches:** Solving problems using either a top-down approach with memoization or a bottom-up approach (iterative table filling).
    - **Classic Problems:** Examples include rod cutting, matrix-chain multiplication, longest common subsequence, and optimal binary search trees.
- **Greedy Algorithms:**
    - **Core Concepts:** Making locally optimal choices with the hope of finding a globally optimal solution.
    - **Properties:** Possessing the **greedy-choice property** and **optimal substructure** are key indicators for applicability.
    - **Applications:** Activity-selection problem, Huffman codes (data compression), and some caching problems.
- **Amortized Analysis:**
    - **Purpose:** To analyze the average time cost of an operation over a sequence of operations, guaranteeing average performance in the worst case (unlike average-case analysis which involves probability).
    - **Techniques:** Aggregate analysis, the accounting method, and the potential method.

### Specialized Topics

Depending on the role, more specialized areas might be covered.

- **Online Algorithms:** Algorithms that receive input over time and must make decisions without knowing future inputs. Competitive analysis is used to evaluate them by comparing against an optimal algorithm that knows the future. A classic example is the caching problem.
- **Number-Theoretic Algorithms:** Essential for cryptography (e.g., RSA public-key cryptosystem) which relies on properties of large prime numbers. Topics include greatest common divisor and modular arithmetic.
- **Linear Programming:** Used for resource allocation problems, where problems are modeled mathematically to optimize a linear objective function subject to linear constraints.

---

Regarding your original query, "I'm preparing tech interview. list up topics that's highly likely interviewer ask about. interviewer will ask about the topic that's highly related to practical skills. don't show citation numbers on your texts. add some subheading like ###, #### so that I can view texts easily on .md file view."

Your sentence "list up topics that's highly likely interviewer ask about" has a grammatical issue. It would be more grammatically correct to say: "list **out** topics that **interviewers are** highly likely to ask about."

Similarly, "interviewer will ask about the topic that's highly related to practical skills" could be improved to: "**Interviewers** will ask about **topics** that **are** highly related to practical skills."



## Software Engineering
---


When preparing for a tech interview, interviewers often focus on practical skills that are directly applicable to the daily work of a software engineer. Based on the provided sources, here are topics highly likely to be discussed:

### Software Development Processes

Interviewers often want to understand how you approach software development.

- **Agile Methods**: Expect questions on the **principles of agile development**, emphasizing **rapid delivery of functionality** and **responsiveness to changing customer requirements**. Be ready to discuss specific agile methods like **Scrum** (sprints, product backlog, daily scrums, sprint backlog, product owner, scrum master) and **Extreme Programming (XP)** (user stories, refactoring, pair programming, continuous integration, test-first development). The ability of agile methods to deliver useful software earlier is a key point. You might be asked about **how agile methods minimize documentation** and use informal communication.
- **Hybrid Approaches**: In reality, industry often mixes techniques. Be prepared to discuss **integrating agility with other methods**, or how **plan-driven approaches** can incorporate agile practices, especially for large or regulated systems. The importance of balancing agility and discipline is a practical consideration.
- **Incremental Development**: This is a core concept across many processes. Understand how **early delivery and deployment of useful software** is possible through increments, even if not all functionality is included.

### Requirements Engineering

Understanding and managing requirements is foundational.

- **Functional vs. Non-Functional Requirements**: Be able to differentiate between what the system _does_ (functional) and its _qualities_ (non-functional), such as **reliability, security, performance, usability**, and **maintainability**. You should also know how to specify non-functional requirements quantitatively, making them "testable" whenever possible.
- **Requirements Elicitation**: Discuss practical techniques like **interviewing** (structured vs. unstructured, common difficulties like jargon or unspoken knowledge), **observation or ethnography** (discovering actual work practices vs. formal processes), and **user stories** (how they capture customer needs, their advantages for planning and user involvement, and potential completeness issues).
- **Requirements Specification**: Understand how requirements are documented. Practical formats include **natural language** (with guidelines for clarity, consistency, and avoiding jargon) and **structured specifications** (using templates and tables to reduce ambiguity). Know why a detailed **software requirements document (SRS)** might be essential for outsourced or multi-team projects, even if agile methods favor less formal documentation.

### Software Design and Modeling

Design is a core engineering activity.

- **Object-Oriented Design**: Be familiar with the general **object-oriented design process steps**: understanding context, designing architecture, identifying principal objects, developing design models, and specifying interfaces.
- **UML (Unified Modeling Language)**: Practical use of UML diagrams is common. Focus on diagrams like **use case models** (for user interaction), **class diagrams** (for static structure and relationships), **sequence diagrams** (for dynamic object interactions), and **state diagrams** (for system behavior in response to events). Understand that **models are abstractions** and that the level of detail depends on their purpose.
- **Design Patterns**: Be ready to discuss what **design patterns** are (well-tried solutions to common problems) and how they facilitate the **reuse of design knowledge and experience**.
- **Architectural Design**: Understand the importance of a system's architecture in meeting non-functional requirements. Be familiar with common **architectural patterns** like **layered architectures** (e.g., in the Mentcare system) and **client-server architectures**.

### Implementation Issues

Directly related to coding and code management.

- **Software Reuse**: This is a dominant paradigm. Discuss different forms of reuse, from **application system integration** (combining existing systems) to **software product lines** (configuring generic applications). Understand the **trade-offs** and challenges of reuse, such as imperfect fit to requirements or integration complexities.
- **Configuration Management (CM)**: This is crucial for collaborative development. Be prepared to discuss **version control systems** (like Git, including concepts like cloning, branching, and merging), **system building** (creating executable systems from components), and **change management** (assessing, approving, and tracking changes with change request forms).
- **Host-Target Development**: Understand the setup where software is developed on one machine (host) and executed on another (target), common in embedded systems.
- **Dependable Programming Guidelines**: These are practical coding best practices to reduce faults and improve security, such as **limiting visibility of information**, **checking all inputs for validity**, **providing handlers for all exceptions**, **minimizing error-prone constructs**, and **checking array bounds**.

### Software Testing and Validation

Quality assurance is an ongoing process.

- **Development Testing**: Explain **unit testing** (individual components/objects) and **component testing** (interfaces of composite components).
- **Test-Driven Development (TDD)**: A highly practical agile practice where **tests are written before the code**. Understand its benefits (reducing regression testing costs, acting as documentation).
- **Release Testing**: Discuss how the entire system is tested before release, including **requirements-based testing** (linking tests to specific requirements), **scenario testing** (using realistic user stories), and **performance testing** (stress testing, load testing).
- **Acceptance Testing**: Understand its role as **customer-driven testing** to ensure the system meets their needs.
- **Automated Testing**: Recognize the importance of **automated tests** for efficient and repeatable testing, especially for regression testing.

### Software Evolution and Maintenance

Software rarely stays static.

- **Software Maintenance Types**: Understand the practical differences and challenges of **fault repairs**, **adaptations to new environments**, and **implementing new requirements** (often called perfective maintenance).
- **Refactoring**: Explain refactoring as a way to **improve software structure and readability** to make future changes easier and avoid structural deterioration.

### Dependability and Security

Critical concerns for modern systems.

- **Core Concepts**: Be aware of the key attributes: **reliability, availability, safety, security, and resilience**. Understand that building dependable systems is not just a technical problem, but also involves human, social, and organizational factors.
- **Redundancy and Diversity**: Explain how these are fundamental mechanisms to create dependable systems (e.g., replicated servers, N-version programming for software fault tolerance).
- **Secure Systems Design Guidelines**: Discuss practical guidelines like **using defense in depth** (multiple security mechanisms), **failing securely** (system failures should not compromise security), **specifying input formats** (to prevent attacks like SQL poisoning), and **compartmentalizing assets** (limiting access to sensitive data).
- **Security Testing**: Be familiar with practical techniques such as **experience-based testing** (using checklists of known vulnerabilities), **penetration testing** (simulating attacks by dedicated teams), and **tool-based static analysis** (automatically identifying potential vulnerabilities in code).

### Project Management Concepts (for Engineers)

Engineers are part of a larger team and project.

- **Risk Management**: Understand how risks are identified (project, product, business risks), analyzed (probability, seriousness), and planned for (avoidance, minimization, contingency). Knowing **common project risks** and **potential indicators** is valuable.
- **Teamwork and Communication**: Recognize the importance of **group cohesion**, effective **communication channels**, and how factors like **group composition** (mix of personalities) and the **physical work environment** can impact productivity. Understand that **project managers motivate people** through interaction, recognition, and personal development opportunities.

These topics cover the lifecycle of software development from a practical, hands-on perspective, making them very relevant for a technical interview.

