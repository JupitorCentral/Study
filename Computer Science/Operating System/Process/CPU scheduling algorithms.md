- [ ] CPU scheduling algorithms ➕ 2025-06-17 

### First-Come, First-Served (FCFS) Scheduling

This is the most straightforward scheduling algorithm.

- **How it works:** The process that requests the CPU first is the one that gets allocated the CPU first. It is managed with a simple FIFO (first-in, first-out) queue. When a process is ready, its Process Control Block (PCB) is added to the end of the queue. When the CPU is free, it is given to the process at the front of the queue, which is then removed.
    
- **Characteristics:**
    - It is a nonpreemptive algorithm, meaning once a process has the CPU, it keeps it until it either terminates or requests I/O.
        
    - The average waiting time can often be quite long, especially if long processes are at the front of the queue, causing shorter processes behind them to wait. This is known as the **convoy effect**.
        
    - This algorithm is particularly problematic for time-sharing systems where it's important for each user to get a share of the CPU at regular intervals.
        

### Shortest-Job-First (SJF) Scheduling

This algorithm is designed based on the length of the process's next CPU burst.

- **How it works:** When the CPU is available, it is assigned to the process that has the smallest next CPU burst. If there's a tie, FCFS scheduling is used to break it.
    
- **Characteristics:**
    - SJF is provably optimal because it provides the minimum average waiting time for a given set of processes.
        
    - The primary challenge is knowing the length of the next CPU request, which is difficult to do for short-term scheduling. One approach is to predict the length based on an **exponential average** of the process's previous CPU bursts.
        
    - SJF can be either **nonpreemptive** or **preemptive**. The preemptive version, sometimes called **shortest-remaining-time-first** scheduling, will preempt the currently executing process if a new process arrives with a CPU burst shorter than the remaining time of the current process.
        

### Priority Scheduling

This is a more general case of the SJF algorithm.

- **How it works:** A priority is associated with each process, and the CPU is allocated to the process with the highest priority. Processes with equal priority are scheduled in FCFS order. SJF is a priority algorithm where the priority is the inverse of the predicted next CPU burst length.
    
- **Characteristics:**
    - Priorities can be defined **internally** (using measurable quantities like time limits or memory requirements) or **externally** (based on factors like the importance of the process).
        
    - Like SJF, it can be either preemptive or nonpreemptive.
        
    - A significant problem is **indefinite blocking** or **starvation**, where low-priority processes may never get to execute. A solution to this is **aging**, which involves gradually increasing the priority of processes that wait in the system for a long time.
        

### Round-Robin (RR) Scheduling

This algorithm is specifically designed for time-sharing systems.

- **How it works:** It is similar to FCFS, but with preemption added to switch between processes. A small unit of time, called a **time quantum** or **time slice** (typically 10-100 milliseconds), is defined. The ready queue is treated as a circular queue. The CPU scheduler allocates the CPU to each process for a time interval of up to one time quantum. If the process is still running after its time quantum expires, it is preempted, and the CPU is given to the next process in the queue.
    
- **Characteristics:**
    - The performance of RR depends heavily on the size of the time quantum. A very large quantum makes RR behave like FCFS. A very small quantum (processor sharing) gives the appearance that each process has its own slower processor, but the overhead of context switching becomes high.
        
    - A rule of thumb is that 80% of CPU bursts should be shorter than the time quantum.
        

### Multilevel Queue Scheduling

This algorithm is for situations where processes can be easily classified into different groups, such as foreground (interactive) and background (batch) processes.

- **How it works:** The ready queue is partitioned into several separate queues. Processes are permanently assigned to one queue based on properties like memory size or process type. Each queue has its own scheduling algorithm (e.g., RR for the foreground queue, FCFS for the background queue).
    
- **Characteristics:**
    - Scheduling must also be done among the queues, which is commonly implemented as fixed-priority preemptive scheduling. For example, the foreground queue may have absolute priority over the background queue.
        
    - Another possibility is **time-slicing** among the queues, where each queue gets a certain portion of the CPU time to schedule among its processes.
        

### Multilevel Feedback Queue Scheduling

This is an enhancement of the multilevel queue algorithm that allows processes to move between queues.

- **How it works:** The idea is to separate processes according to their CPU burst characteristics. If a process uses too much CPU time, it is moved to a lower-priority queue. This leaves I/O-bound and interactive processes in the higher-priority queues. Conversely, a process that waits too long in a lower-priority queue may be moved to a higher-priority queue to prevent starvation.
    
- **Characteristics:**
    - It is the most general CPU-scheduling algorithm because it can be configured to match a specific system.
        
    - It is also the most complex, as it requires defining several parameters: the number of queues, the scheduling algorithm for each queue, and the methods for upgrading and demoting processes between queues.