
- [ ] Process Scheduling and Context Switching Demystified ➕ 2025-06-17 


### Job Queue vs. Ready Queue

Processes initially enter the system and are placed in a **job queue**, which contains all processes in the system, typically residing on disk awaiting the allocation of main memory. However, for context switching and CPU allocation, the relevant queue is the **ready queue**. The ready queue consists of processes that are residing in main memory and are ready and waiting to execute.

### Process B's Selection

Process B is selected from this **ready queue**. The selection process is carried out by the **short-term scheduler** (also known as the CPU scheduler). Once selected, the **dispatcher** is the module that actually gives control of the CPU to Process B.

### "Front of the Queue" and Scheduling Algorithms

While it's common for a process to be at the "front" in some conceptual sense, the ready queue is not necessarily a simple First-In, First-Out (FIFO) queue. There are various CPU-scheduling algorithms that determine which process gets selected from the ready queue:

- **First-Come, First-Served (FCFS):** If the FCFS algorithm is used, then yes, the process at the head of the queue is selected.
    
- **Shortest-Job-First (SJF):** This algorithm selects the process with the smallest next CPU burst, which might not be at the very front of a FIFO queue.
    
- **Priority Scheduling:** The CPU is allocated to the process with the highest priority.
    
- **Round-Robin (RR):** The ready queue is treated as a circular queue, and the CPU scheduler allocates the CPU to each process in turn for a defined "time quantum."
    
[[CPU scheduling algorithms]]


So, while Process B must be in the ready queue and selected by the scheduler, it isn't always about being "first in line" in a strict temporal sense, but rather "first according to the scheduling algorithm's criteria."

### Process States

- **Process B's State:** Before the dispatcher gives control of the CPU to Process B, its state would be **ready**. Once the dispatcher takes action, Process B transitions to the **running** state. ( [[2025-06-17 Eng#"Transition" as a Verb]] )
- **Process A's State:** When Process A is switched out, its state changes depending on why the context switch occurred:
    - If Process A made an **I/O request** or created a new subprocess and is waiting for its termination, it moves from "running" to the **waiting** state and is placed in an appropriate I/O or waiting queue.
    - If Process A was forcibly removed from the CPU (e.g., its time quantum expired in a time-sharing system or a higher-priority process became ready due to an interrupt), it transitions from "running" back to the **ready** state and is put back into the ready queue.
        

### Summary

To rephrase the process more naturally and correctly:

"When a context switch occurs from Process A to Process B, the dispatcher saves all information related to Process A into its Process Control Block (PCB), which is managed by the kernel, is inaccessible to user processes, and is separate from Process A's memory. Then, the short-term scheduler selects Process B from the ready queue. Process B's state would be 'ready' before this selection. After selection, the dispatcher loads the registers and other context from Process B's PCB into the processor, allowing Process B to begin execution. As for Process A, its state would transition to 'waiting' if it initiated an I/O operation (or another event it needs to wait for), or 'ready' if it was preempted (e.g., by a timer interrupt or a higher-priority process) and returned to the ready queue."
