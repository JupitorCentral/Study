- [ ] OS - What to Learn ➕ 2025-06-17 

## Must know for Interview

### 1. Processes and Threads

- **Process Concept**: What a process is, the different process states (new, ready, running, waiting, terminated), and the structure of a **Process Control Block (PCB)**.
- **Threads**: The core concept of a thread, the distinction between what threads share (code, data, files) versus what is unique to each thread (stack, registers, program counter).
- **Processes vs. Threads**: Be able to clearly articulate the differences, especially regarding memory space and the overhead of creation and context switching.
- **Multithreading Models**: Understand the relationship between user threads and kernel threads (**Many-to-One**, **One-to-One**, **Many-to-Many**).

### 2. CPU Scheduling

- **Scheduling Concepts**: The roles of the short-term and long-term schedulers.
- **Preemptive vs. Non-preemptive Scheduling**: The key difference and its implications.
- **Scheduling Criteria**: How to measure an algorithm's performance (CPU utilization, throughput, turnaround time, waiting time).
- **Scheduling Algorithms**: Be prepared to explain and compare these common algorithms:
    - First-Come, First-Served (FCFS)
    - Shortest-Job-First (SJF)
    - Priority Scheduling
    - **Round Robin (RR)** (especially important for interactive systems)

### 3. Process Synchronization

- **The Critical-Section Problem**: The fundamental challenge of ensuring data integrity with concurrent access.
- **Race Conditions**: What they are and how to identify them.
- **Mutexes**: How they work to provide **mutual exclusion**.
- **Semaphores**: Their function as a signaling mechanism (both counting and binary semaphores) and how they differ from mutexes.
- **Classic Problems**: Being familiar with the **Bounded-Buffer** or **Readers-Writers** problem is useful to demonstrate your understanding.

### 4. Deadlocks

- **Deadlock Characterization**: The **four necessary conditions** (Mutual Exclusion, Hold and Wait, No Preemption, and Circular Wait). You should be able to list and explain these.
- **Methods for Handling Deadlocks**: Know the difference between **Deadlock Prevention**, **Deadlock Avoidance** (and the concept of a **safe state**), and **Deadlock Detection**.

### 5. Memory Management

- **Logical vs. Physical Address Space**: The fundamental concept separating the programmer's view of memory from the hardware's reality.
- **Paging**: How it translates logical to physical addresses using **page tables** and **frames**, and how it solves **external fragmentation**.
- **Virtual Memory**: The purpose and benefits (running programs larger than physical memory).
- **Demand Paging**: What a **page fault** is and the high-level steps an OS takes to handle it.
- **Page Replacement Algorithms**: The purpose of these algorithms and the basic idea behind **FIFO** and **LRU**.

### 6. File Systems & Storage

- **File Concept**: Basic file operations (create, read, write, seek) and attributes.
- **Allocation Methods**: The trade-offs between **Contiguous**, **Linked**, and **Indexed** allocation for storing files on disk.
- **Disk Scheduling**: The purpose of disk scheduling and the basic idea behind algorithms like **FCFS** and **SCAN/SSTF**.
- **RAID**: A high-level understanding of what RAID is and the difference between **RAID 0** (striping), **RAID 1** (mirroring), and **RAID 5** (striping with parity).



## Lists of Topics

### Part One - Overview

#### Introduction

- What Operating Systems Do
- Computer-System Organization
- Computer-System Architecture
- Operating-System Structure
- Operating-System Operations
- Process Management
- Memory1 Management
    
- Storage Management
- Protection and Security
- Distributed Systems
- Special-Purpose Systems
- Computing Environments
- Open-Source Operating Systems2
    

#### Operating System Structures

- Operating-System Services
- User Operating-System Interface
- System Calls
- Types of System Calls
- System Programs3
    
- Operating-System Design and Implementation
- Operating-System Structure
- Virtual Machines
- Operating-System Debugging
- Operating-System Generation
- System4 Boot
    

---

### Part Two - Process Management

#### Processes

- Process Concept
- Process Scheduling
- Operations on Processes
- Interprocess Communication
- Examples of IPC Systems
- Communication in Client-Server5 Systems
    

#### Threads

- Overview
- Multithreading Models
- Thread Libraries
- Threading Issues
- Operating-System Examples

#### CPU Scheduling

- Basic Concepts
- Scheduling Criteria
- Scheduling Algorithms
- Thread Scheduling
- Multiple-Processor Scheduling6
    
- Operating System Examples
- Algorithm Evaluation7
    

#### Process Synchronization

- Background
- The Critical-Section Problem
- Peterson’s Solution
- Synchronization Hardware
- Semaphores
- Classic Problems of Synchronization
- Monitors
- Synchronization Examples
- Atomic Transactions8
    

#### Deadlocks

- System Model
- Deadlock Characterization
- Methods for Handling Deadlocks
- Deadlock Prevention
- Deadlock Avoidance
- Deadlock Detection
- Recovery from Deadlock

---

### Part Three - Memory Management

#### Main Memory

- Background
- Swapping
- Contiguous Memory Allocation
- Paging
- Structure of the Page Table
- Segmentation9
    
- Example: The Intel Pentium

#### Virtual Memory

- Background
- Demand Paging
- Copy-on-Write
- Page Replacement
- Allocation of Frames
- Thrashing10
    
- Memory-Mapped Files
- Allocating Kernel Memory
- Other Considerations
- Operating-System Examples11
    

---

### Part Four - Storage Management

#### File-System Interface

- File Concept
- Access Methods
- Directory and Disk Structure
- File-System Mounting
- File Sharing
- Protection

#### File-System12 Implementation

- File-System Structure
- File-System Implementation
- Directory Implementation
- Allocation Methods
- Free-Space Management
- Efficiency and Performance
- Recovery
- NFS13
    
- Example: The WAFL File System14
    

#### Mass-Storage Structure

- Overview of Mass-Storage Structure
- Disk Structure
- Disk Attachment
- Disk Scheduling
- Disk Management
- Swap-Space Management
- RAID Structure15
    
- Stable-Storage Implementation16
    
- Tertiary-Storage Structure

#### I/O Systems

- Overview
- I/O Hardware
- Application I/O Interface
- Kernel I/O Subsystem
- Transforming I/O Requests to Hardware Operations
- STREAMS17
    
- Performance

---

### Part Five - Protection and Security

#### Protection

- Goals of Protection
- Principles of Protection
- Domain of Protection
- Access Matrix
- Implementation of Access18 Matrix
    
- Access Control
- Revocation of Access Rights
- Capability-Based Systems
- Language-Based Protection19
    

#### Security

- The Security Problem
- Program Threats
- System and Network Threats
- Cryptography as a Security Tool
- User Authentication
- Implementing Security Defenses
- Firewalling20 to Protect Systems and Networks
    
- Computer-Security Classifications21
    
- An Example: Windows XP

---

### Part Six - Distributed Systems

#### Distributed System Structures

- Motivation
- Types of Network-based Operating Systems
- Network Structure
- Network Topology
- Communication Structure
- Communication Protocols
- Robustness
- Design Issues
- An Example: Networking22
    

#### Distributed File Systems

- Background
- Naming and Transparency
- Remote File Access
- Stateful Versus Stateless Service
- File Replication23
    
- An Example: AFS24
    

#### Distributed Coordination

- Event Ordering
- Mutual Exclusion
- Atomicity
- Concurrency Control
- Deadlock Handling
- Election Algorithms
- Reaching25 Agreement
    

---

### Part Seven - Special-Purpose Systems

#### Real-Time Systems

- Overview
- System Characteristics
- Features of Real-Time Kernels
- Implementing Real-Time Operating Systems
- Real-Time CPU Scheduling26
    
- An Example: VxWorks 5.x

#### Multimedia Systems

- What Is Multimedia?
- Compression
- Requirements of Multimedia Kernels
- CPU Scheduling
- Disk Scheduling
- Network Management
- An Example: CineBlitz27
    

---

### Part Eight - Case Studies

#### The Linux System

- Linux History
- Design Principles
- Kernel Modules
- Process Management
- Scheduling
- Memory Management
- File Systems28
    
- Input and Output
- Interprocess Communication
- Network Structure29
    
- Security

#### Windows XP

- History
- Design Principles
- System Components
- Environmental Subsystems
- File System
- Networking
- Programmer Interface30
    

#### Influential Operating Systems

- Feature Migration
- Early Systems
- Atlas
- XDS-940
- THE
- RC 4000
- CTSS
- MULTICS
- IBM OS/360
- TOPS-20
- CP/M and MS/DOS
- Macintosh Operating System and Windows
- Mach
- Other Systems