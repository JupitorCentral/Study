
## Algorithm
---


Here is a list of topics from the sources, designed for a quick overview:

### Data Structures

#### Linked Lists

A fundamental linear data structure where elements are linked by pointers.

- **Singly Linked Lists**: Elements point only to the next element.
- **Doubly Linked Lists**: Elements point to both the next and previous elements.
- **Mergeable Heaps using Linked Lists**: Implementing priority queues that can be combined.
- **Compact Lists**: Representing linked lists using arrays.

#### Representing Rooted Trees

Different methods for structuring and storing trees.

- **Binary Trees**: Each node has at most two children.
- **Left-Child, Right-Sibling Representation**: A flexible way to represent trees with arbitrary branching.

#### Hash Tables

Data structures that map keys to values for efficient lookups.

- **Direct-Address Tables**: Simple array-based hashing for direct mapping.
- **Hash Functions**: Methods for computing hash values from keys.
- **Collision Resolution**: Techniques like chaining and open addressing to handle hash collisions.
- **Free List**: A linked list used to manage unused slots within a hash table's storage.

#### Binary Search Trees

Tree-based data structures where nodes are ordered, allowing for efficient queries and modifications.

- **Definition and Properties**: Basic rules and characteristics of BSTs.
- **Querying**: Operations like searching, finding minimum/maximum, and finding successors/predecessors.
- **Insertion and Deletion**: Procedures for adding and removing nodes.

#### Red-Black Trees

Self-balancing binary search trees that guarantee logarithmic time complexity for most operations.

- **Properties**: Rules governing node colors and structure to maintain balance.
- **Rotations**: Operations to rebalance the tree after insertions or deletions.
- **Insertion and Deletion**: Algorithms for modifying red-black trees while preserving their properties.
- **Persistent Dynamic Sets**: Maintaining past versions of a dynamic set implemented with binary search trees.

#### Augmenting Data Structures

Adding extra information to nodes in existing data structures to support new operations efficiently.

- **Dynamic Order Statistics**: Data structures that can quickly find the i-th smallest element.
- **Interval Trees**: Specialized trees for storing and querying collections of intervals.

#### B-Trees

Self-balancing tree data structures optimized for storage systems like disks, where disk accesses are costly.

- **Definition**: Properties that keep the tree height low.
- **Basic Operations**: Searching for keys and inserting new keys.
- **Deleting Keys**: Procedures for removing keys while maintaining B-tree properties.
- **CPU Time and Disk Accesses**: Performance analysis focusing on these two distinct costs.
- **2-3-4 Trees**: A specific type of B-tree with a minimum degree of 2.

#### Data Structures for Disjoint Sets

Maintaining a collection of sets where no two sets share common elements, supporting union and find operations.

- **Disjoint-Set Operations**: MAKE-SET (create a new set), FIND-SET (find the representative of an element's set), UNION (merge two sets).
- **Linked-List Representation**: Implementing disjoint sets using linked lists.
- **Disjoint-Set Forests**: A tree-based representation for improved performance.
- **Union by Rank and Path Compression**: Heuristics that significantly speed up disjoint-set operations, leading to nearly linear time.

### Design and Analysis Techniques

#### Dynamic Programming

A method for solving complex problems by breaking them into simpler overlapping subproblems and storing the solutions to avoid recomputation.

- **Rod Cutting**: Maximizing the total value by cutting a rod into pieces.
- **Matrix-Chain Multiplication**: Minimizing the number of scalar multiplications when multiplying a chain of matrices.
- **Elements of Dynamic Programming**: Key characteristics like optimal substructure and overlapping subproblems.
- **Longest Common Subsequence (LCS)**: Finding the longest sequence that is a subsequence of two or more sequences.
- **Optimal Binary Search Trees**: Constructing a binary search tree that minimizes the expected search cost, given key access probabilities.
- **Longest Simple Path in a Directed Acyclic Graph**: Finding the longest path in a DAG.
- **Printing Neatly**: Optimizing text formatting to minimize 'badness'.
- **Edit Distance**: Computing the minimum number of operations to transform one string into another.
- **Viterbi Algorithm**: Application in speech recognition using dynamic programming on graphs.

#### Greedy Algorithms

A technique that makes the locally optimal choice at each stage with the hope of finding a globally optimal solution.

- **Activity Selection Problem**: Scheduling the maximum number of compatible activities.
- **Elements of the Greedy Strategy**: Properties that ensure a greedy choice leads to an optimal solution.
- **Huffman Codes**: A greedy algorithm for designing optimal data compression codes.
- **Offline Caching**: A greedy strategy for cache eviction, where the sequence of requests is known in advance.
- **Fractional Knapsack Problem**: Selecting items for maximum value where fractions of items are allowed.
- **0-1 Knapsack Problem**: A problem similar to fractional knapsack but does not generally have a greedy solution (often solved with dynamic programming).

#### Amortized Analysis

A method for analyzing the running time of a sequence of operations, where the average cost per operation is considered over the entire sequence.

- **Aggregate Analysis**: Calculating the total cost for a sequence of operations and averaging it.
- **Accounting Method**: Assigning 'credits' to operations to cover future expensive operations.
- **Potential Method**: Using a potential function to smooth out costs over time.
- **Dynamic Tables**: Analyzing the cost of operations on data structures that grow and shrink dynamically.

### Algorithms

#### Sorting

Arranging elements in a specific order.

- **Insertion Sort**: A simple, incremental sorting algorithm.
- **Merge Sort**: An efficient, stable, divide-and-conquer sorting algorithm.
- **Heapsort**: An in-place comparison-based sorting algorithm using a heap.
- **Quicksort**: An efficient comparison-based sorting algorithm, often randomized.
- **Radix Sort**: A non-comparison sorting algorithm that sorts elements by processing individual digits.
- **Bucket Sort**: A non-comparison sorting algorithm that distributes elements into buckets.

#### Selection and Order Statistics

Algorithms for finding the k-th smallest (or largest) element in a set.

- **Minimum and Maximum**: Finding the smallest or largest element.
- **SELECT Algorithm**: A worst-case linear-time algorithm for finding the i-th order statistic.
- **RANDOMIZED-SELECT**: An algorithm for finding the i-th order statistic with expected linear time.
- **Weighted Median**: Finding an element that divides the sum of weights in half.

#### Graph Algorithms

Algorithms for problems on graphs.

- **Representations of Graphs**: How graphs are stored (adjacency lists, adjacency matrices).
- **Breadth-First Search (BFS)**: Traversal algorithm for finding shortest paths in unweighted graphs.
- **Depth-First Search (DFS)**: Traversal algorithm for exploring graph structure, classifying edges.
- **Topological Sort**: Ordering vertices of a directed acyclic graph (DAG) such that for every directed edge (u, v), u comes before v.
- **Strongly Connected Components (SCCs)**: Decomposing a directed graph into maximal subgraphs where every pair of vertices is reachable from each other.
- **Minimum Spanning Trees (MSTs)**: Finding a tree within a connected, edge-weighted undirected graph that connects all the vertices with the minimum total edge weight.
    - **Kruskal's Algorithm**: A greedy algorithm for MSTs.
    - **Prim's Algorithm**: Another greedy algorithm for MSTs.
- **Single-Source Shortest Paths**: Finding the shortest paths from a given source vertex to all other vertices.
    - **Dijkstra's Algorithm**: For graphs with non-negative edge weights.
- **All-Pairs Shortest Paths**: Finding the shortest paths between every pair of vertices in a graph.
    - **Floyd-Warshall Algorithm**: A dynamic programming approach.
    - **Johnson's Algorithm**: An algorithm suitable for sparse graphs.
- **Maximum Flow**: Determining the maximum possible flow from a source to a sink in a flow network.
    - **Ford-Fulkerson Method**: A general framework for solving maximum flow problems.
- **Matchings in Bipartite Graphs**: Finding a subset of edges in a bipartite graph such that no two edges share a common vertex.
    - **Maximum Bipartite Matching**: Finding a matching with the largest possible number of edges (an application of max flow).
    - **Stable-Marriage Problem**: Finding a stable matching between two sets of equal size.
    - **Hungarian Algorithm for the Assignment Problem**: An algorithm for finding maximum weight matchings in bipartite graphs.

### Selected Topics

#### Parallel Algorithms

Algorithms designed to run on multiple processors simultaneously to reduce execution time.

#### Online Algorithms

Algorithms that must process input as it arrives, without knowing future inputs.

- **Online Caching**: Decision-making for cache eviction without prior knowledge of future requests.

#### Linear Programming

A mathematical technique for optimizing a linear objective function subject to linear constraints. Used for modeling various problems.

#### Number-Theoretic Algorithms

Algorithms dealing with properties of integers.

- **Primality Testing**: Determining whether a given integer is a prime number.
- **Integer Arithmetic**: Efficient algorithms for operations on large integers.

#### String Matching

Algorithms for finding occurrences of a pattern within a larger text.

- **Naive String Matcher**: A straightforward but often inefficient approach.
- **Rabin-Karp Method**: Uses hashing to efficiently find pattern occurrences.
- **Knuth-Morris-Pratt Algorithm**: Optimizes pattern matching by preprocessing the pattern.
- **Suffix Arrays**: A data structure for efficient string searching and other string operations.

#### Machine Learning

Algorithms that enable systems to learn from data.

- **Clustering**: Grouping similar data points together.
- **Weighted-Majority Algorithms**: Decision-making algorithms often used in online learning.
- **Gradient Descent**: An optimization algorithm used to find the minimum of a function.

#### NP-Complete Problems

A class of computational problems for which no polynomial-time algorithm is known.

- **Examples**: Includes problems like Hamiltonian cycle, satisfiability, subset sum, and the traveling salesperson problem.

#### Approximation Algorithms

Algorithms that find approximate solutions to computationally hard problems, especially NP-complete problems, typically with a guaranteed bound on how far the solution is from optimal.





## Operating System
---


To help you skim the whole content and see the overall structure, here is a list of topics with brief descriptions, formatted for your .md file:

### Part One - Overview

#### CHAPTER 1 - Introduction

- **What Operating Systems Do**: Explains the fundamental role of an OS as an intermediary between users/applications and hardware, managing resources efficiently and conveniently.
- **Computer-System Organization**: Describes the basic structural components of a computer system, including CPU, memory, and I/O devices.
- **Computer-System Architecture**: Discusses different computer system designs, such as single-processor, multiprocessor (SMP), and clustered systems.
- **Operating-System Structure**: Outlines general approaches to OS organization, including concepts like multiprogramming and time-sharing.
- **Operating-System Operations**: Covers the various services and functions an OS provides, like handling interrupts and managing system calls.
- **Process Management**: Introduces the concept of a process (a program in execution) and the OS responsibilities in managing them.
- **Memory Management**: Briefly touches on how the OS manages main memory for efficient CPU utilization and response time.
- **Storage Management**: Describes how the OS handles file systems, mass storage, and I/O devices.
- **Protection and Security**: Introduces mechanisms for protecting system resources and data from unauthorized access or damage.
- **Distributed Systems**: Explores systems composed of multiple interconnected processors that do not share memory or a clock.
- **Special-Purpose Systems**: Covers operating systems designed for specific uses, such as real-time embedded systems and multimedia systems.

### Part Two - Process Management

#### CHAPTER 3 - Processes

- **Process Concept**: Defines a process as an active program in execution, distinct from a passive program code.
- **Process Scheduling**: Focuses on how the operating system selects which processes from the ready queue will be allocated the CPU.
- **Operations on Processes**: Describes actions such as creating, terminating, suspending, and resuming processes.
- **Interprocess Communication**: Mechanisms that allow cooperating processes to exchange data and synchronize actions (e.g., shared memory, message passing).
- **Examples of IPC Systems**: Illustrates concrete implementations of interprocess communication.
- **Communication in Client-Server Systems**: Discusses communication models used in client-server architectures.

#### CHAPTER 4 - Threads

- **Overview**: Introduces threads as lighter-weight units of CPU utilization within a process, enabling multiple tasks concurrently.
- **Multithreading Models**: Explains different mappings between user-level threads and kernel-level threads.
- **Thread Libraries**: Describes application programming interfaces (APIs) for creating and managing threads, such as Pthreads, Win32, and Java threads.
- **Threading Issues**: Covers challenges in multithreaded programming, like the semantics of `fork()` and `exec()` system calls.
- **Operating-System Examples**: Provides real-world examples of thread implementations in various operating systems.

#### CHAPTER 5 - CPU Scheduling

- **Basic Concepts**: Fundamental ideas behind CPU scheduling, including maximizing CPU utilization in multiprogrammed systems.
- **Scheduling Criteria**: Measures used to evaluate and compare CPU-scheduling algorithms, such as CPU utilization, response time, and throughput.
- **Scheduling Algorithms**: Details various strategies for allocating CPU time to processes, including First-Come, First-Served (FCFS), Shortest-Job-First (SJF), Priority, Round-Robin (RR), and Multilevel Queue Scheduling.
- **Thread Scheduling**: Focuses on how kernel-level threads are scheduled for execution.
- **Multiple-Processor Scheduling**: Addresses scheduling challenges in systems with multiple CPUs, like symmetric multiprocessing (SMP) and multicore processors.
- **Operating System Examples**: Illustrates CPU scheduling in specific operating systems like Solaris, Windows XP, and Linux.
- **Algorithm Evaluation**: Methods for determining the performance of scheduling algorithms, including deterministic modeling, queueing models, and simulations.

#### CHAPTER 6 - Process Synchronization

- **Background**: Introduces the need for synchronization in systems with cooperating processes sharing data.
- **The Critical-Section Problem**: Defining a protocol to ensure that only one process executes in its critical section (shared code segment) at any given time.
- **Peterson’s Solution**: A classic software-based solution to the critical-section problem for two processes.
- **Synchronization Hardware**: Special CPU instructions that facilitate atomic operations for synchronization.
- **Semaphores**: A synchronization tool used to control access to shared resources.
- **Classic Problems of Synchronization**: Examples like the **Bounded-Buffer Problem**, **Readers-Writers Problem**, and **Dining-Philosophers Problem** that are used to test synchronization schemes.
- **Monitors**: A high-level language construct that simplifies the implementation of mutual exclusion and synchronization.
- **Synchronization Examples**: Real-world implementations of synchronization mechanisms in operating systems (e.g., Solaris, Windows XP, Linux).
- **Atomic Transactions**: Ensures that a sequence of operations is treated as a single, indivisible unit, either fully completed or not at all.

### Part Three - Memory Management

#### CHAPTER 8 - Main Memory

- **Background**: Fundamental concepts of memory, including how the CPU interacts with main memory and the distinction between logical and physical addresses.
- **Swapping**: A memory management technique that involves temporarily moving processes (or parts of them) between main memory and secondary storage.
- **Contiguous Memory Allocation**: Allocating a single, unbroken block of memory to each process, leading to issues like **external fragmentation**.
- **Paging**: A memory management scheme that allows a process's logical address space to be noncontiguous, dividing it into fixed-size pages mapped to physical memory frames.
- **Structure of the Page Table**: Different ways to organize page tables for large logical address spaces, such as multi-level paging and inverted page tables, often using a **Translation Look-Aside Buffer (TLB)** for speed.
- **Segmentation**: A memory management scheme that supports the user's view of memory as a collection of variable-sized segments, each with a name and length.
- **Example: The Intel Pentium**: Illustrates how a real-world architecture combines segmentation and paging for memory management.

#### CHAPTER 9 - Virtual Memory

- **Background**: The concept of allowing processes to execute even if they are not entirely in physical memory, enabling programs larger than physical memory and increased multiprogramming.
- **Demand Paging**: A technique where pages are loaded into memory only when they are needed during program execution.
- **Copy-on-Write**: An efficient technique for process creation where parent and child processes initially share pages, and copies are made only upon modification.
- **Page Replacement**: Algorithms used to select a page to be removed from memory when a new page needs to be loaded and no free frames are available (e.g., **FIFO**, **LRU**, Optimal).
- **Allocation of Frames**: Strategies for distributing the fixed amount of available physical memory frames among multiple processes.
- **Thrashing**: A severe performance degradation that occurs when a process does not have enough frames allocated to it, leading to a high page-fault rate; concepts like **working-set model** and **page-fault frequency** are used to address this.
- **Memory-Mapped Files**: A mechanism that allows file I/O to be treated as routine memory accesses by logically associating a portion of virtual address space with a file.
- **Allocating Kernel Memory**: Discusses specific techniques for memory allocation within the operating system kernel, such as the buddy system and slab allocators.
- **Other Considerations**: Additional factors affecting paging system design, including prepaging, page size, TLB reach, and program structure.
- **Operating-System Examples**: Shows how Windows XP and Solaris implement virtual memory.

### Part Four - Storage Management

#### CHAPTER 10 - File -System Interface

- **File Concept**: Defines a file as a logical storage unit, characterized by attributes like name, type, location, and size.
- **Access Methods**: Describes how information within a file can be accessed, including **sequential access**, **direct access**, and indexed access.
- **Directory and Disk Structure**: Discusses the organization of files into directories and how storage devices are partitioned into volumes.
- **File-System Mounting**: The process of attaching a file system (from a device or volume) to a specific location within the overall directory structure, making it available for use.
- **File Sharing**: Addresses mechanisms and challenges related to allowing multiple users or processes to access the same files, including consistency semantics.
- **Protection**: Mechanisms to safeguard files from physical damage and improper access, often involving access-control lists or permission schemes (owner, group, universe).

#### CHAPTER 11 - File -System Implementation

- **File-System Structure**: Describes the layered architecture of a file system, from low-level I/O control to the logical file system.
- **File-System Implementation**: Details the in-memory data structures (like the open-file table and file-control blocks) and processes involved in file operations, including Virtual File Systems (VFS).
- **Directory Implementation**: Explores different methods for organizing directory entries, such as linear lists and hash tables.
- **Allocation Methods**: Explains how disk space is allocated to files: **contiguous allocation**, **linked allocation**, and **indexed allocation**.
- **Free-Space Management**: Techniques used by the system to track and manage unallocated disk blocks, enabling reuse for new files (e.g., bit vector, linked list, counting).
- **Efficiency and Performance**: Discusses strategies to optimize disk use and I/O performance, including caching (unified buffer cache) and synchronous/asynchronous writes.
- **Recovery**: Methods for maintaining file system consistency and recovering from failures, such as consistency checking and **log-structured file systems** (journaling).
- **NFS**: Describes the Network File System, a client-server protocol for accessing files over a network.
- **Example: The WAFL File System**: A case study of a file system optimized for random writes, featuring efficient snapshots and clones.

#### CHAPTER 12 - Mass -Storage Structure

- **Overview of Mass-Storage Structure**: Provides a general description of physical secondary and tertiary storage devices, primarily magnetic disks and tapes.
- **Disk Structure**: Details how modern disk drives are logically organized as one-dimensional arrays of blocks and mapped to physical sectors and cylinders.
- **Disk Attachment**: Explores how mass storage is connected to computer systems, including host-attached, network-attached, and storage-area networks (SANs).
- **Disk Scheduling**: Algorithms used to reorder disk I/O requests to minimize head movement and improve performance (**FCFS**, **SSTF**, **SCAN**, **C-SCAN**, **LOOK**, **C-LOOK**).
- **Disk Management**: Covers tasks such as disk partitioning, logical formatting (creating a file system), and handling bad blocks.
- **Swap-Space Management**: Describes how disk space is managed to function as an extension of main memory for processes.
- **RAID Structure**: Introduces Redundant Arrays of Inexpensive Disks, which combine multiple physical disks for improved data reliability and performance.
- **Stable-Storage Implementation**: Techniques to ensure that data written to storage persists even in the event of system failures.
- **Tertiary-Storage Structure**: Discusses large-capacity, slower, and lower-cost storage devices, such as magnetic tapes and optical disks, used primarily for archiving and backup.




## Computer Network
---


### **Foundational Concepts**

#### **Internet Basics**

- **Internet:** A global network of interconnected computers and networks.
- **Protocol:** Defines format, order of messages, and actions taken for communication.
- **Host (End System):** Devices running applications, like laptops, smartphones, servers.
- **Client/Server Architecture:** A common application model where clients request services from servers.
- **Distributed Application:** Applications involving multiple end systems exchanging data.

#### **Network Architecture & Layers**

- **Layered Architecture:** Organizing complex network functions into distinct layers.
- **Protocol Stack:** The set of protocols across different layers (e.g., Internet's five layers).
- **Internet Protocol Stack (Layers):**
    - **Application Layer:** Supports network applications (e.g., Web, email).
    - **Transport Layer:** Provides process-to-process data transfer.
    - **Network Layer:** Handles host-to-host packet routing.
    - **Link Layer:** Moves data across individual communication links.
    - **Physical Layer:** Deals with the physical transmission of bits.
- **Encapsulation:** Adding header information at each layer as data moves down the stack.
- **De-encapsulation:** Removing header information at each layer as data moves up the stack.

#### **Network Components**

- **Communication Link:** Physical media connecting network devices.
- **Packet Switch:** A general device that transfers packets from input to output links.
- **Router:** A network-layer (layer 3) packet switch, forwarding based on IP addresses.
- **Link-Layer Switch:** A layer-2 packet switch, forwarding based on MAC addresses.
- **Modem:** Device to convert signals for network access (e.g., DSL modem, cable modem).
- **Data Center:** Large facilities housing many servers for cloud applications.
- **Middlebox:** Network equipment performing functions beyond basic forwarding, like firewalls or load balancers.

#### **Performance Metrics**

- **Delay:** Time taken for data to travel through the network.
    - **Processing Delay:** Time to examine packet header and direct it.
    - **Queuing Delay:** Time spent waiting in a buffer for transmission.
    - **Transmission Delay:** Time to push all packet bits onto a link.
    - **Propagation Delay:** Time for a bit to travel across the physical medium.
- **Packet Loss:** Packets dropped due to buffer overflow or errors.
- **Throughput:** Rate at which data is transferred between end systems.
- **Round-Trip Time (RTT):** Time from sending a segment until its acknowledgment is received.

### **Network Edge**

#### **Access Networks**

- **Digital Subscriber Line (DSL):** Internet access over existing telephone lines.
- **Cable Internet Access (HFC):** Internet access over hybrid fiber-coaxial cable infrastructure.
- **Fiber to the Home (FTTH):** Internet access directly over optical fiber.
- **Wireless Access Networks:**
    - **WiFi (IEEE 802.11):** Wireless LAN technology for local areas.
    - **4G/5G Cellular Networks:** Wide-area wireless access for mobile devices.
- **Physical Media:**
    - **Guided Media:** Waves guided along a solid medium (e.g., copper wire, fiber-optic cable).
    - **Unguided Media:** Waves propagate in atmosphere/space (e.g., radio spectrum).

### **Network Core**

#### **Data Transfer Approaches**

- **Packet Switching:** Resources are not reserved; messages use resources on demand.
    - **Store-and-Forward Transmission:** A packet switch receives the entire packet before transmitting it.
- **Circuit Switching:** Resources along a path are reserved for the duration of communication.
    - **Time-Division Multiplexing (TDM):** Dividing time into slots, assigning slots to nodes.
    - **Frequency-Division Multiplexing (FDM):** Dividing channel bandwidth into frequency bands, assigning bands to nodes.

#### **Router Internals**

- **Input Port:** Terminates incoming link, performs link-layer functions, and lookup.
- **Forwarding Table:** Table used by a router to determine the output link for a packet.
- **Switching Fabric:** Moves packets from input ports to output ports within a router.
- **Output Port:** Buffers packets before transmission onto an outgoing link.
- **Queuing:** Packets waiting in buffers.
    - **Drop-Tail:** Policy of dropping arriving packets when buffers are full.
    - **Active Queue Management (AQM):** Proactive packet-dropping/marking before buffers are full to signal congestion.
    - **Head-of-the-Line (HOL) Blocking:** A queued packet is blocked by another packet at the head of the line, even if its output port is free.
- **Packet Scheduling:** Determining the order of queued packets for transmission.
    - **First-Come-First-Served (FCFS):** Packets transmitted in order of arrival.
    - **Priority Queuing:** Higher priority packets are transmitted before lower priority packets.
    - **Round Robin (RR):** Serving different classes of customers in turn.
    - **Weighted Fair Queuing (WFQ):** Each class receives a guaranteed fraction of service based on weights.

### **Internet Protocol (IP)**

#### **IPv4**

- **IPv4 Datagram Format:** Structure of an IPv4 packet header including source/destination IP addresses, TTL, etc.
- **IPv4 Addressing:** 32-bit addresses assigned to interfaces.
    - **Interface:** Boundary between a host/router and a physical link.
    - **Subnet:** An isolated network segment with devices sharing a common IP address part.
    - **Classless Interdomain Routing (CIDR):** Address assignment strategy generalizing subnetting using prefixes.
    - **Prefix:** The network portion of an IP address (a.b.c.d/x).
    - **Network Address Translation (NAT):** Mechanism to map multiple private IP addresses to a single public IP address.

#### **IPv6**

- **IPv6:** New version of IP with streamlined 40-byte header, larger addresses, no fragmentation in base header.
- **Flow Labeling:** Field in IPv6 for special handling of particular packet flows.
- **Traffic Class:** Field in IPv6 for prioritizing datagrams.

#### **Generalized Forwarding & SDN**

- **Generalized Forwarding:** Forwarding decisions based on multiple header fields from different layers, not just destination IP.
- **Software-Defined Networking (SDN):** Separates data plane from control plane, with a logically centralized controller.
- **OpenFlow:** A standard that pioneered match-plus-action forwarding and SDN.
- **Flow Table:** An entry in a match-plus-action forwarding table, containing match conditions and actions.
- **Match-Plus-Action Paradigm:** A powerful abstraction where network devices match packets based on header values and perform specified actions.
- **P4 (Programming Protocol-independent Packet Processors):** A programming language for specifying packet processing behavior at line rate.

### **Routing & Control Plane**

#### **Routing Algorithms**

- **Routing:** Network-wide process to determine end-to-end paths for packets.
- **Routing Algorithm:** Computes paths (routes), typically least-cost.
- **Graph (in routing):** Nodes represent routers, edges represent links.
- **Centralized Routing Algorithm:** Runs in one location with complete network information.
- **Decentralized Routing Algorithm:** Runs in each node, exchanging information with neighbors.
- **Static Routing:** Paths are configured manually and change slowly.
- **Dynamic Routing:** Paths change automatically in response to network conditions.
- **Least-Cost Path:** The path with the lowest sum of edge costs.

#### **Intra-AS Routing (within an Autonomous System)**

- **Autonomous System (AS):** A group of routers under single administrative control.
- **Link-State (LS) Routing Algorithm:** All nodes know the network topology and link costs (e.g., OSPF).
    - **Dijkstra's Algorithm:** Used in LS to compute least-cost paths.
- **Distance-Vector (DV) Routing Algorithm:** Nodes exchange distance vectors (cost estimates to all destinations) with neighbors.
    - **Count-to-Infinity Problem:** A routing loop problem in DV algorithms.
    - **Poisoned Reverse:** A technique to prevent two-node routing loops in DV.
- **Open Shortest Path First (OSPF):** A widely deployed Internet intra-AS routing protocol based on link-state.
    - **Area (OSPF):** A hierarchical subdivision within an AS to manage routing complexity.
    - **Backbone Area (OSPF):** Central area for inter-area routing in OSPF.

#### **Inter-AS Routing (between Autonomous Systems)**

- **Border Gateway Protocol (BGP):** The de facto standard for inter-AS routing, "glues" the Internet together.
- **BGP Attributes:** Information carried with BGP routes (e.g., AS-PATH, NEXT-HOP).
- **Hot Potato Routing:** A greedy routing policy where an AS tries to get packets out of its own AS as quickly as possible.
- **IP-Anycast:** Using BGP to direct users to the "nearest" server with replicated content.
- **Routing Policy:** Rules defining how an AS chooses to route traffic, often based on commercial relationships.
- **Peering Agreements:** Confidential agreements between ISPs governing traffic exchange.

#### **SDN Control Plane**

- **Logically Centralized Control:** Control plane functions are separate and remote from forwarding devices.
- **SDN Controller:** Computes, manages, and installs flow table entries in SDN-enabled switches.
- **OpenDaylight:** An open-source SDN controller framework.
- **Network Control Applications:** Software running on the SDN controller to implement network services (e.g., routing, firewalling).
- **OpenFlow Protocol:** Communication protocol between the SDN controller and SDN-controlled devices.

#### **Network Management**

- **Internet Control Message Protocol (ICMP):** Used for error reporting and network diagnostics (e.g., Traceroute).
- **Network Management Framework:** Architecture, protocols, and data for network administration.
    - **Managed Device:** Network equipment with manageable components.
    - **Managing Server:** Software that monitors and controls managed devices.
    - **Management Information Base (MIB):** Database of managed objects and their state on a device.
- **Simple Network Management Protocol (SNMP):** A protocol for network management using PDUs (Protocol Data Units) to query/set MIB objects.
- **NETCONF/YANG:** Newer protocols for network configuration management, using XML-based messages.

### **Link Layer & LANs**

#### **Link Layer Fundamentals**

- **Framing:** Encapsulating network-layer datagrams into link-layer frames for transmission.
- **Error Detection/Correction:** Adding bits to frames to detect/correct transmission errors.
    - **Parity Check:** Simple error detection (single bit).
    - **Checksumming:** Summing data to detect errors.
    - **Cyclic Redundancy Check (CRC):** A robust error detection technique.
- **Multiple Access Links:** Shared broadcast channels with multiple sending/receiving nodes.
- **Multiple Access Protocols:** Rules coordinating access to a shared broadcast channel.
    - **Channel Partitioning Protocols:** Divide channel bandwidth (e.g., TDM, FDM, CDMA).
    - **Random Access Protocols:** Nodes transmit at random; collisions can occur (e.g., ALOHA, CSMA).
    - **Carrier Sense Multiple Access (CSMA):** Listen before speaking.
    - **CSMA/CD (Collision Detection):** Stop transmitting if a collision is detected (wired Ethernet).
    - **CSMA/CA (Collision Avoidance):** Mechanisms to reduce collisions (802.11 WiFi RTS/CTS).
    - **Taking-Turns Protocols:** Nodes take turns transmitting (e.g., polling, token passing).

#### **LAN Technologies**

- **Local Area Network (LAN):** Network geographically concentrated in a single building or campus.
- **MAC Address (Link-Layer Address):** A unique hardware identifier for network interfaces.
- **Address Resolution Protocol (ARP):** Translates IP addresses to MAC addresses.
- **Ethernet (IEEE 802.3):** Most prevalent wired LAN technology, includes physical and link-layer specifications.
    - **Ethernet Frame Format:** Structure of an Ethernet frame.
- **Link-Layer Switch:** Connects multiple LAN segments, localizes traffic, learns MAC addresses.
    - **Switch Table:** Stores MAC address to interface mappings for forwarding.
    - **Self-Learning (Switch):** Switches learn MAC addresses by observing incoming frames.
- **Virtual Local Area Network (VLAN):** Allows multiple virtual LANs over a single physical LAN infrastructure.
    - **Port-Based VLAN:** Switch ports grouped into VLANs, forming broadcast domains.
    - **VLAN Trunking:** Interconnecting VLAN switches using a special trunk port and 802.1Q tags.
    - **802.1Q Tag:** A four-byte tag added to Ethernet frames to carry VLAN identity.
- **Multiprotocol Label Switching (MPLS):** A packet-switched, virtual-circuit network for interconnecting IP devices.
    - **Label-Switched Router:** Forwards MPLS frames by looking up MPLS labels.
    - **MPLS Header:** Added between link-layer and network-layer headers, includes a label.
    - **Traffic Engineering (MPLS):** Ability to forward packets along routes not possible with standard IP routing.

#### **Data Center Networks**

- **Data Center Network (DCN):** Complex networks interconnecting internal hosts within a data center.
- **Hierarchical Topology:** Common DCN structure (e.g., core, aggregate, access layers).
- **Load Balancer:** Distributes client requests to available hosts, often performs NAT-like functions.
- **Fat-Tree Topology:** A highly interconnected DCN topology to minimize bottlenecks.

### **Wireless & Mobile Networking**

#### **Wireless Communication Challenges**

- **Wireless Link:** Communication channel without physical wiring.
- **Path Loss:** Signal attenuation due to distance.
- **Multipath Propagation:** Signal reflections causing interference.
- **Interference:** Signals from other sources disrupting communication.
- **Signal-to-Noise Ratio (SNR):** Measure of signal strength relative to noise.
- **Code Division Multiple Access (CDMA):** Channel partitioning using unique codes for simultaneous transmissions.

#### **Wireless LAN Standards**

- **IEEE 802.11 (WiFi):**
    - **Infrastructure Mode:** Wireless hosts connect to an Access Point (AP).
    - **Ad Hoc Mode:** Wireless hosts communicate directly without an AP.
    - **Beacon Frame:** APs periodically send these to announce their presence and network info.
    - **Power Management (802.11):** Nodes alternate between sleep and wake states to conserve energy.
    - **Rate Adaptation:** Transmitting at different rates based on channel conditions.
- **Bluetooth:** Personal Area Network (PAN) technology for short-range communication.
    - **Piconet:** A small ad hoc network of up to eight active Bluetooth devices (master/clients).
    - **Frequency-Hopping Spread Spectrum (FHSS):** Rapidly changing transmission frequency to avoid interference.

#### **Cellular Networks**

- **4G/5G Cellular Networks:** Mobile cellular data networks.
- **eNodeB (LTE):** Base station in 4G LTE networks.
- **Mobility Management Entity (MME):** Key control-plane element in 4G/5G for signaling and state management.
- **Serving Gateway (S-GW):** Handles mobile data path in 4G/5G.
- **Packet Data Network Gateway (P-GW):** Connects cellular network to external IP networks.
- **Home Subscriber Server (HSS):** Stores subscriber information in the home network.
- **Opportunistic Scheduling (LTE):** Base station schedules transmissions to mobile devices based on channel conditions.

#### **Mobility Management**

- **Device Mobility:** A device changing its point of attachment to the network.
- **Home Network:** The network to which a mobile device originally belongs.
- **Visited Network:** A foreign network where a mobile device is currently attached.
- **Care-of-Address (CoA):** A temporary address assigned to a mobile device in a visited network.
- **Home Agent:** A router in the home network that tunnels datagrams to the mobile device's CoA.
- **Foreign Agent:** A router in the visited network that helps register the mobile device.
- **Mobile IP:** Standards for supporting IPv4 mobility, using home and foreign agents for routing.

### **Network Security**

#### **Security Principles**

- **Confidentiality:** Keeping communication contents secret from eavesdroppers.
- **Message Integrity:** Ensuring communication content is not altered in transit.
- **End-Point Authentication:** Confirming the identity of communicating parties.
- **Nonrepudiation:** Preventing a sender from denying a message.

#### **Cryptography**

- **Plaintext (Cleartext):** Original message.
- **Ciphertext:** Encrypted message.
- **Encryption Algorithm:** Transforms plaintext to ciphertext.
- **Decryption Algorithm:** Transforms ciphertext back to plaintext.
- **Key:** Secret information used in encryption/decryption.
- **Symmetric Key Cryptography:** Sender and receiver share the same secret key.
    - **Block Ciphers:** Encrypt data in fixed-size blocks (e.g., DES, 3DES, AES).
    - **Cipher-Block Chaining (CBC):** Method to link encrypted blocks, making identical plaintext blocks produce different ciphertext.
    - **Initialization Vector (IV):** A random value used with CBC to add randomness.
- **Public Key Cryptography:** Uses a pair of keys: a public key for encryption and a private key for decryption.
    - **RSA:** A popular public key encryption algorithm.
    - **Diffie-Hellman:** A public key algorithm for establishing shared secret keys.

#### **Message Integrity & Authentication**

- **Cryptographic Hash Function:** Takes input message, produces a fixed-size digest (fingerprint); computationally infeasible to reverse or find collisions.
- **Message Authentication Code (MAC):** A cryptographic checksum based on a shared secret key, ensuring integrity and authenticity.
- **Digital Signature:** A cryptographic mechanism using public key cryptography to bind a sender to a message, ensuring integrity and nonrepudiation.
    - **Digital Certificate:** Binds a public key to an entity's identity, verified by a trusted Certificate Authority.
- **Nonce:** A "number used once" to defend against replay attacks in authentication protocols.
- **IP Spoofing:** Creating IP datagrams with a false source IP address.
- **Replay Attack:** An attacker re-sends legitimate messages to achieve an unauthorized effect.

#### **Secure Networking Protocols**

- **Pretty Good Privacy (PGP):** Email encryption standard using symmetric and public key cryptography.
- **Transport Layer Security (TLS) / Secure Sockets Layer (SSL):** Provides security services for TCP connections (e.g., Web HTTPS).
    - **TLS Handshake:** Phase for client/server to agree on algorithms, exchange nonces, and establish session keys.
    - **TLS Record:** Data unit in TLS, includes type, version, length, data, and HMAC fields.
- **IPsec (IP security protocol):** Provides security at the network layer for IP datagrams.
    - **Virtual Private Network (VPN):** Uses IPsec to create a secure private network over a public Internet.
    - **Security Association (SA):** State information (encryption/authentication keys, algorithms) maintained for IPsec communication.
    - **Authentication Header (AH):** IPsec protocol for integrity and authentication.
    - **Encapsulating Security Payload (ESP):** IPsec protocol for encryption, integrity, and authentication.
    - **Tunnel Mode (IPsec):** Encapsulates entire IP datagram for VPNs.
    - **Internet Key Exchange (IKE):** Protocol for setting up IPsec Security Associations.
- **Wireless Security (e.g., WEP, WPA3):** Mechanisms to secure wireless networks.
    - **Wired Equivalent Privacy (WEP):** Original, flawed 802.11 security standard.
    - **Four-Way Handshake (802.11):** Used to derive and install session keys for secure wireless communication.
- **Operational Security:** Protecting organizational networks.
    - **Firewall:** Device that filters traffic based on rules (e.g., packet filtering, stateful packet filters).
    - **Access Control List (ACL):** Rules defining what traffic is allowed or blocked.
    - **Deep Packet Inspection (DPI):** Examining application data within packets.
    - **Intrusion Detection System (IDS):** Detects potentially malicious traffic and generates alerts.
    - **Intrusion Prevention System (IPS):** Filters out suspicious traffic.
- **Distributed Denial-of-Service (DDoS) Attack:** Attack where multiple compromised hosts (botnet) overwhelm a target.
- **Packet Sniffing:** Passively recording copies of packets transmitted over a channel.





## Software Engineering
---


### Introduction to Software Engineering

#### Introduction

- **Software Engineering**: Professional software development, its importance and challenges.
- **Professional Software Development**: Definition of software, attributes of good software (maintainability, dependability, usability, efficiency).
- **Software Engineering Ethics**: Moral and professional responsibilities of software engineers.
- **Case Studies**: Examples of different system types (insulin pump, Mentcare, wilderness weather station, digital learning environment).

#### Software Processes

- **Software Process Models**: Simplified representations of software development (waterfall, incremental, reuse-oriented).
- **Process Activities**: Fundamental steps in software development: specification, development, validation, evolution.
- **Coping with Change**: Techniques to manage evolving requirements (prototyping, incremental delivery).
- **Process Improvement**: Enhancing development processes (maturity models, agile approaches).

#### Agile Software Development

- **Agile Methods**: Principles for rapid, flexible software delivery, contrasting with plan-driven.
- **Agile Development Techniques**: Practices like Extreme Programming (XP), user stories, test-driven development (TDD), refactoring, pair programming.
- **Agile Project Management**: Frameworks for organizing agile projects, notably Scrum (product backlog, sprints, daily Scrum).

#### Requirements Engineering

- **Functional Requirements**: Services the system should provide, specific actions or behaviors.
- **Non-functional Requirements**: Constraints on system operation or development (performance, security, usability).
- **Requirements Engineering Processes**: Activities involved in defining requirements: elicitation, analysis, validation, documentation.
- **Requirements Elicitation**: Discovering needs from stakeholders (interviews, scenarios, ethnography, user stories).
- **Requirements Specification**: Documenting requirements in various formats (natural language, structured language, use cases, SRS).

#### System Modeling

- **Context Models**: Defining system boundaries and environmental dependencies.
- **Interaction Models**: How users and systems interact (use case diagrams, sequence diagrams).
- **Structural Models**: Organization of system classes/objects and their relationships (class diagrams, generalization, aggregation).
- **Behavioral Models**: How the system responds to events and changes state (state diagrams).
- **Model-driven Engineering (MDE)**: Generating system implementations directly from abstract models.

#### Architectural Design

- **Software Architecture**: High-level organization of a software system.
- **Architectural Decisions**: Choices influencing system properties like performance and security.
- **Architectural Views**: Different perspectives for documenting architecture (conceptual, logical, process, development, physical).
- **Architectural Patterns**: Reusable structural organizations (layered, repository, client-server, pipe and filter).
- **Application Architectures**: Common structures for specific application types (data processing, transaction processing, language processing).

#### Design and Implementation

- **Object-oriented Design using UML**: Process of designing software using object classes and their interactions.
- **Design Patterns**: Reusable solutions to common design problems.
- **Implementation Issues**: Practical concerns in coding (software reuse, configuration management, host-target development).
- **Open-source Development**: Licenses, benefits, and challenges of using open-source software.

#### Software Testing

- **Development Testing**: Testing during the development process (unit, component, system testing).
- **Test-driven Development (TDD)**: Writing tests before code to guide implementation.
- **Release Testing**: Testing a complete system before release (requirements-based, scenario, performance testing).
- **User Testing**: Testing by potential users (alpha, beta testing).

#### Software Evolution

- **Evolution Processes**: Activities involved in changing software after deployment.
- **Legacy Systems**: Managing and evolving older, critical software systems.
- **Software Maintenance**: Types of changes (fault repair, environmental adaptation, functionality addition).
- **Maintenance Prediction**: Assessing future changes and costs.
- **Software Reengineering**: Improving system structure and understandability without changing functionality.
- **Refactoring**: Improving code structure and readability.

### System Dependability and Security

#### Dependable Systems

- **Dependability Properties**: Key attributes of trustworthy systems: availability, reliability, safety, security, resilience.
- **Sociotechnical Systems**: Systems encompassing technical components, people, and organizational processes.
- **Redundancy and Diversity**: Strategies to prevent system failure from individual component failures.
- **Dependable Processes**: Software development processes designed to build trustworthy systems.
- **Formal Methods and Dependability**: Mathematical approaches for rigorous system specification and verification.

#### Reliability Engineering

- **Availability and Reliability**: Measures of system uptime and consistent operation.
- **Reliability Requirements**: Specifying required system reliability (functional and non-functional).
- **Fault-tolerant Architectures**: Designs to handle failures (N-version programming, protection systems).
- **Programming for Reliability**: Good coding practices to reduce faults (information hiding, input validation).
- **Reliability Measurement**: Quantifying reliability through statistical testing and operational profiles.

#### Safety Engineering

- **Safety-critical Systems**: Systems whose failure can cause harm or damage.
- **Safety Requirements**: Defining "shall not" behaviors and risk reduction measures.
- **Hazard Analysis**: Identifying potential dangers and their causes (fault tree analysis).
- **Safety Reviews**: Formal checks to ensure safety measures are met.
- **Static Analysis**: Automated code analysis for potential safety flaws.
- **Safety Cases**: Documented evidence and arguments for a system's safety.

#### Security Engineering

- **Security and Dependability**: Relationship between protecting against attacks and overall system trustworthiness.
- **Security Threats**: Circumstances that can cause harm (interception, interruption, modification, fabrication).
- **Security Risk Assessment**: Identifying and analyzing potential security risks (attack trees).
- **Security Requirements**: Specifications for protecting system assets.
- **Design for Security**: Principles and guidelines (defense in depth, fail securely, compartmentalization).
- **Security Testing**: Validating system security (vulnerability scanning, penetration testing, static analysis).

#### Resilience Engineering

- **Cybersecurity**: Broad protection of IT infrastructure from threats.
- **Sociotechnical Resilience**: How human and technical systems work together to withstand and recover from adverse events.
- **Resilient Systems Design**: Strategies (resistance, recognition, recovery) to ensure continuous critical service delivery during and after attacks or failures.

### Advanced Software Engineering

#### Software Reuse

- **Benefits and Problems of Reuse**: Advantages (cost, speed) and challenges (compatibility, maintenance).
- **Application Frameworks**: Collections of reusable abstract/concrete classes for specific domains.
- **Software Product Lines**: Developing similar systems from a common core architecture.
- **Application System Reuse**: Adapting and configuring existing off-the-shelf software products.

#### Component-based Software Engineering (CBSE)

- **Components and Component Models**: Definition of reusable software units and their interfaces.
- **CBSE Processes**: Stages for developing components for reuse and building systems by reusing components.
- **Component Composition**: Integrating components to create larger systems, addressing interface compatibility.

#### Distributed Software Engineering

- **Distributed Systems**: Systems where components are distributed across multiple computers.
- **Client-Server Computing**: Fundamental distributed architecture model (two-tier, multi-tier, peer-to-peer).
- **Architectural Patterns for Distributed Systems**: Specific designs like master-slave, distributed component, peer-to-peer.
- **Software as a Service (SaaS)**: Delivering software functionality over the Internet, emphasizing configurability, multi-tenancy, and scalability.

#### Service-oriented Software Engineering

- **Service-oriented Architecture (SOA)**: Building systems using loosely coupled, reusable services.
- **RESTful Services**: A simpler architectural style for web services based on resource transfer.
- **Service Engineering**: Processes for identifying, designing, implementing, and deploying services.
- **Service Composition**: Combining multiple services to create new applications or workflows.

#### Systems Engineering

- **Sociotechnical Systems**: Complex systems involving hardware, software, people, and processes.
- **Conceptual Design**: Initial high-level vision and constraints for a system.
- **System Procurement**: Acquiring systems from external suppliers.
- **System Development**: Stages of building a system, including integration.
- **System Operation and Evolution**: Managing systems throughout their lifecycle.

#### Systems of Systems (SoS)

- **System Complexity**: Technical, managerial, and governance challenges in large integrated systems.
- **SoS Classification**: Categories based on governance (organizational, federated, system coalitions).
- **Reductionism and Complex Systems**: Limitations of breaking down and understanding complex systems in parts.
- **SoS Engineering**: Processes for building and evolving systems of independently managed systems.
- **SoS Architecture**: Principles and patterns for designing complex integrated systems.

#### Real-time Software Engineering

- **Embedded Systems Design**: Designing software for dedicated hardware with specific constraints.
- **Architectural Patterns for Real-time Software**: Designs for responsive and time-critical systems (producer-consumer, observer).
- **Timing Analysis**: Evaluating the execution time of critical operations to meet deadlines.
- **Real-time Operating Systems (RTOS)**: Specialized operating systems for managing real-time tasks and resources.

### Software Management

#### Project Management

- **Project Manager Tasks**: Core responsibilities: planning, risk management, people management, reporting.
- **Risk Management**: Identifying, analyzing, and planning responses to project risks.
- **Managing People**: Motivating and organizing development teams.
- **Teamwork**: Factors influencing team performance (cohesion, communication, organization).

#### Project Planning

- **Software Pricing**: Factors influencing the cost and price of software projects.
- **Plan-driven Development**: Detailed upfront planning (project plans, work breakdown, milestones, deliverables).
- **Project Scheduling**: Organizing tasks over time (bar charts, dependencies, resource allocation).
- **Agile Planning**: Flexible, incremental planning (planning game, user stories, release and iteration planning).
- **Estimation Techniques**: Methods for predicting project effort and cost (experience-based, algorithmic models like COCOMO II).

#### Quality Management

- **Software Quality**: Attributes defining good software (maintainability, efficiency, security).
- **Software Standards**: Guidelines and norms for development processes and products.
- **Reviews and Inspections**: Formal quality assurance activities to find defects in software and documentation.
- **Quality Management and Agile Development**: Adapting QM practices for agile environments.
- **Software Measurement**: Using metrics to assess and improve software quality.

#### Configuration Management

- **Version Management**: Controlling and tracking different versions of software components (baselines, codelines, branching, merging).
- **System Building**: Automating the compilation and linking of software components into executable systems.
- **Change Management**: Formal process for assessing, approving, and implementing changes to requirements and code.
- **Release Management**: Preparing, distributing, and documenting software releases.



## Database
---



### **Fundamentals of Database Systems**

#### **Book Overview**

- **Database Systems**: Introduces core concepts for designing, using, and implementing database systems and applications.
- **Ramez Elmasri & Shamkant B. Navathe**: Authors of the textbook.
- **Sixth Edition**: The specific edition of the book.
- **Database Modeling & Design**: Fundamental concepts stressed in the book.
- **Database Management Systems (DBMS)**: Focus on languages, models, and implementation techniques.
- **Target Audience**: Junior, senior, or graduate level courses, and as a reference.

#### **Core Concepts**

- **Database**: A collection of related data, representing a miniworld, used for specific purposes.
- **Database Management System (DBMS)**: Collection of programs enabling users to create and maintain a database.
- **Database System**: The database and DBMS software combined.
- **Meta-data**: Descriptive information about the database structure and constraints, stored in the DBMS catalog.
- **Miniworld**: The aspect of the real world that the database represents.

#### **Key Characteristics of the Database Approach**

- **Self-describing Nature**: DBMS catalog stores database definition (meta-data).
- **Program-Data Independence**: Insulation between application programs and data details; changes to data storage don't require program changes.
- **Data Abstraction**: Hiding low-level data details from users, achieved via data models.
- **Multiple Views of Data**: Different users can have tailored perspectives (subsets or derived data) of the database.
- **Sharing & Multiuser Transaction Processing**: Concurrent access and updates for multiple users.
- **Controlling Redundancy**: Aim to store each logical data item once (normalization), or control necessary duplication (denormalization).
- **Indexes & Buffering**: Specialized data structures (indexes) and main memory buffers to speed up disk access.

#### **Database Design Process**

- **Requirements Collection & Analysis**: Understanding and documenting user data and functional requirements.
- **Conceptual Database Design**: High-level design using conceptual models like **ER/EER Model**.
- **Logical Database Design (Data Model Mapping)**: Translating conceptual design to a data model implementable in a commercial DBMS (e.g., Relational Data Model).
- **Physical Database Design**: Specifying storage structures and access paths for performance.
- **Database Implementation & Tuning**: Creating database files, implementing applications, and optimizing performance.
- **Information System Life Cycle**: Broader context for database system development.

#### **Data Models**

- **Data Model**: Collection of concepts to describe database structure (data types, relationships, constraints).
- **High-level/Conceptual Data Models**: Concepts close to user perception (e.g., **Entity-Relationship (ER) Model**, **Enhanced-ER (EER) Model**).
    - **Entity**: Real-world object or concept.
    - **Attribute**: Property describing an entity or relationship.
    - **Key Attribute**: Uniquely identifies an entity.
    - **Composite Attribute**: Attribute composed of several components.
    - **Multivalued Attribute**: Attribute with multiple values for a single entity.
    - **Derived Attribute**: Value computed from other attributes.
    - **Relationship Type**: Association among entity types.
    - **Cardinality Ratio**: Maximum number of relationship instances an entity can participate in (e.g., 1:1, 1:N, M:N).
    - **Participation Constraint**: Whether an entity's participation in a relationship is total or partial.
    - **Specialization**: Defining subclasses from a superclass (top-down).
    - **Generalization**: Identifying common properties of multiple entity types into a superclass (bottom-up).
    - **Category (Union Type)**: Represents a subset of the union of entities from different types.
    - **UML Class Diagrams**: Notation introduced for conceptual design, similar to ER/EER.
- **Representational/Implementation Data Models**: Concepts understood by end users, but closer to storage (e.g., **Relational Data Model**).
- **Low-level/Physical Data Models**: Concepts describing data storage details (e.g., record formats, access paths).

#### **Relational Model Concepts**

- **Relation/Table**: A set of tuples/rows with named attributes/columns.
- **Tuple/Row**: A single record in a relation.
- **Attribute/Column**: A named property in a relation.
- **Domain**: Set of allowed values for an attribute.
- **Relation Schema**: Name of the relation and its attributes.
- **Relational Database Schema**: Set of relation schemas and integrity constraints.
- **Superkey**: Set of attributes that uniquely identifies tuples.
- **Candidate Key**: Minimal superkey.
- **Primary Key**: A chosen candidate key, underlined in schema diagrams.
- **Foreign Key**: Attribute(s) in one relation referencing the primary key of another relation, enforcing referential integrity.
- **NULL Values**: Represents unknown, inapplicable, or missing attribute values.

#### **SQL (Structured Query Language)**

- **Data Definition Language (DDL)**: Used to define database schemas (e.g., **CREATE TABLE**, **ALTER TABLE**, **DROP TABLE**).
- **Data Manipulation Language (DML)**: Used for retrieving and updating data.
    - **High-level (Nonprocedural/Declarative/Set-at-a-time)**: Specifies _what_ to retrieve (e.g., **SELECT-FROM-WHERE**).
    - **Low-level (Procedural/Record-at-a-time)**: Specifies _how_ to retrieve (e.g., older DMLs).
- **Basic Retrieval (SELECT Statement)**: Fundamental query command.
    - **FROM Clause**: Specifies tables involved.
    - **WHERE Clause**: Specifies conditions for selection.
    - **JOIN Conditions**: Links records across tables.
    - **GROUP BY Clause**: Groups tuples based on attribute values.
    - **HAVING Clause**: Filters groups.
    - **ORDER BY Clause**: Sorts query results.
- **Aggregate Functions**: **COUNT**, **SUM**, **AVG**, **MIN**, **MAX**.
- **Nested Queries**: Queries embedded within other queries.
- **Views (Virtual Tables)**: Derived tables that are not explicitly stored.
- **Triggers**: Database procedures executed automatically on specified events.
- **Assertions**: General constraints that must hold true for the database.
- **Data Types**: Numeric, character string, Boolean, DATE, TIME, etc.

#### **Formal Relational Languages**

- **Relational Algebra**: Procedural query language; queries are sequences of operations.
    - **SELECT (σ)**: Filters tuples based on a condition (horizontal partitioning).
    - **PROJECT (π)**: Selects attribute columns (vertical partitioning).
    - **UNION (∪), INTERSECTION (∩), SET DIFFERENCE (−)**: Set operations on compatible relations.
    - **CARTESIAN PRODUCT (×)**: Combines all tuples from two relations.
    - **JOIN (⋈)**: Combines related tuples from two relations based on a condition (**THETA JOIN**, **EQUIJOIN**, **NATURAL JOIN**).
    - **DIVISION (÷)**: Used for queries like "find X that relate to all Y."
    - **RENAME (ρ)**: Renames attributes or relations.
    - **Aggregate Function Operation (𝓕)**: Performs aggregate calculations, often with grouping.
    - **Outer Joins**: Preserves all tuples from one or both relations, even if no match exists (**LEFT OUTER JOIN**, **RIGHT OUTER JOIN**, **FULL OUTER JOIN**).
- **Relational Calculus**: Declarative query language; specifies _what_ results are desired.
    - **Tuple Relational Calculus**: Uses tuple variables.
    - **Domain Relational Calculus**: Uses domain variables.
    - **Quantifiers**: **Existential Quantifier (∃)**, **Universal Quantifier (∀)**.

#### **Database Programming**

- **Embedded SQL**: SQL statements embedded in a host programming language.
    - **Cursor**: Mechanism to process query results one tuple at a time.
- **Dynamic SQL**: Queries constructed and executed at runtime.
- **SQL/CLI (Call Level Interface)**: Library of functions (API) for database access.
- **ODBC (Open DataBase Connectivity)**: Predecessor to SQL/CLI, a common API.
- **JDBC (Java Database Connectivity)**: Call function interface for Java.
- **SQLJ**: Technique for embedding SQL in Java programs.
- **SQL/PSM (Persistent Stored Modules)**: Allows procedures and functions to be stored in the DBMS.
- **Impedance Mismatch**: Discrepancy between set-at-a-time database languages and record-at-a-time programming languages.
- **PHP**: Scripting language used for Web database programming.

#### **Normalization Theory**

- **Functional Dependency (FD)**: A constraint where one set of attributes determines another (X → Y).
- **Normalization Process**: Decomposing relation schemas to minimize redundancy and anomalies.
- **Normal Forms**: Criteria for evaluating relation schema quality.
    - **First Normal Form (1NF)**: No multivalued or composite attributes (no nested relations).
    - **Second Normal Form (2NF)**: In 1NF and every nonprime attribute is fully functionally dependent on every candidate key.
    - **Third Normal Form (3NF)**: In 2NF and no nonprime attribute is transitively dependent on any candidate key.
    - **Boyce-Codd Normal Form (BCNF)**: Stricter than 3NF; for every nontrivial FD (X → Y), X must be a superkey.
    - **Multivalued Dependency (MVD)**: A constraint where one attribute independently determines multiple sets of values (X →→ Y).
    - **Fourth Normal Form (4NF)**: In BCNF and no nontrivial MVDs exist unless the determinant is a superkey.
    - **Join Dependency (JD)**: A constraint indicating a relation can be reconstructed by joining its projections.
    - **Fifth Normal Form (5NF/PJNF)**: Based on join dependencies.
    - **Domain-Key Normal Form (DKNF)**: Most general normal form, where all constraints are logical consequences of domain constraints and key constraints.
- **Minimal Cover**: A minimal set of FDs equivalent to a given set.
- **Dependency Preservation**: A decomposition property ensuring all original FDs can be enforced by checking constraints within decomposed relations.
- **Nonadditive (Lossless) Join Property**: A decomposition property ensuring no spurious tuples are generated when relations are rejoined.
- **Spurious Tuples**: Incorrect extra tuples produced by joining relations that don't satisfy the lossless join property.
- **Dangling Tuples**: Records lost from results when joining relations if some records do not have matching tuples due to decomposition.

#### **File Structures and Indexing**

- **Storage Hierarchy**: Primary (main memory, cache), Secondary (magnetic disks), Tertiary (optical disks, tapes).
- **Magnetic Disks**: Primary medium for large databases.
    - **Track**: Concentric circles on disk surfaces.
    - **Cylinder**: Tracks with the same diameter across multiple surfaces.
    - **Block/Sector**: Smallest unit of data transferred from disk.
    - **Seek Time**: Time to move read/write head to correct track.
    - **Rotational Delay**: Time for desired block to rotate under head.
    - **Block Transfer Time**: Time to transfer data block.
- **Double Buffering**: Using multiple memory buffers to overlap I/O and CPU processing.
- **File Organizations**: How records are physically placed on disk.
    - **Heap File (Unordered File)**: Records inserted at the end.
    - **Sorted File (Sequential File)**: Records ordered by a key field.
    - **Hash File (Static Hashing)**: Records placed based on a hash function, leading to buckets and overflow.
    - **Dynamic Hashing**: Adapts number of buckets as file grows (**Extendible Hashing**, **Linear Hashing**).
    - **Files of Mixed Records**: Stores records of different types together.
- **Indexes**: Auxiliary access structures to speed up retrieval.
    - **Indexing Field**: Attribute(s) an index is built on.
    - **Primary Index**: On the ordering key field of a physically ordered file (nondense).
    - **Clustering Index**: On a nonkey ordering field (nondense).
    - **Secondary Index**: On any nonordering field, can be dense or nondense with indirection.
    - **Multilevel Indexes**: Tree-structured indexes on top of other indexes to reduce search time (e.g., **ISAM**).
    - **B-trees & B+-trees**: Dynamic, balanced tree data structures commonly used for indexes.
        - **B-tree**: Data pointers in internal and leaf nodes.
        - **B+-tree**: Data pointers only in leaf nodes, leaf nodes are linked (preferred for databases).
    - **Indexes on Multiple Keys (Composite Keys)**: Indexes on a combination of attributes.
        - **Lexicographic Ordering**: Ordering based on sequence of attribute values.
        - **Partitioned Hashing**: Extends hashing for multiple keys.
        - **Grid Files**: Multi-dimensional structures for multi-key access, good for range queries.
    - **Hash Indexes**: Secondary indexes using hashing.
    - **Bitmap Indexes**: Uses bitvectors to indicate presence of attribute values, efficient for low-cardinality attributes.
    - **Logical vs. Physical Indexes**: Logical uses logical record identifiers, physical uses physical disk addresses.
- **RAID (Redundant Arrays of Inexpensive/Independent Disks)**: Enhances storage reliability and performance (various levels: 0, 1, 3, 5).
- **Storage Area Networks (SANs)**: Dedicated networks for storage devices.
- **Network-Attached Storage (NAS)**: File-level data storage over a network.
- **iSCSI**: IP-based storage networking standard.
- **Column-Based Storage**: Alternative to row-based storage, optimized for read-only analytical queries (e.g., data warehouses).

#### **Query Processing and Optimization**

- **Query Processing**: Steps involved in executing a high-level query.
- **Query Optimization**: Process of choosing an efficient execution strategy.
- **Query Tree/Query Graph**: Internal representations of a query.
- **Query Blocks**: Basic units of an SQL query for optimization.
- **External Sorting (Sort-Merge Strategy)**: Sorting large files that don't fit in main memory.
- **SELECT Operation Algorithms (File Scans/Index Scans)**: Methods to retrieve records (linear search, binary search, index-based methods).
- **JOIN Operation Algorithms**: Methods to combine records from multiple files.
    - **Nested-Loop Join**: Iterates through one file, then the other.
    - **Single-Loop Join**: Uses an access structure (index/hash) for the inner loop.
    - **Sort-Merge Join**: Sorts files by join attributes and then merges.
    - **Partition-Hash Join**: Partitions files using hashing, then joins corresponding partitions.
- **Selectivity**: Estimates percentage of records satisfying a condition.
- **Histograms**: Data distributions used for more accurate selectivity estimates.
- **Heuristic Optimization**: Uses rules to reorder operations for efficiency.
- **Cost Estimation**: Systematically estimates the cost of different execution plans.
- **Semantic Query Optimization**: Uses known constraints to improve query plans.





