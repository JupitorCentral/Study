- [ ] Role of PCB (process control block) and example scenario ➕ 2025-06-16 

A **Process Control Block (PCB)** is a data structure within the operating system's kernel that stores all the information the OS needs to manage a specific process. Think of it as an ID card or a file folder for each running program.

---

### What's Inside a PCB? 🗂️

The PCB holds all the vital details about a process, including:

- **Process State:** The current status of the process (e.g., _new, ready, running, waiting, terminated_).
- **Program Counter:** The address of the next instruction to be executed.
- **CPU Registers:** A snapshot of the process's register values. This, along with the program counter, is crucial for pausing and resuming the process correctly.
- **CPU Scheduling Information:** Details like the process's priority and pointers to scheduling queues.
- **Memory-Management Information:** Data on the memory allocated to the process, such as page tables or base/limit registers.
- **Accounting Information:** CPU time used, time limits, process ID, etc.
- **I/O Status Information:** A list of I/O devices allocated to the process and a list of its open files.

---

### What is the PCB's Role?

The PCB is essential for enabling key operating system functions, most notably multitasking.

- **Context Switching:** When the CPU switches from executing one process to another, the OS performs a **context switch**. It saves the current process's state (program counter, CPU registers) into its PCB and loads the state of the next process from its PCB. This allows processes to be paused and resumed seamlessly without losing their progress.
- **Process Scheduling:** The OS uses PCBs to manage process execution. It links the PCBs into various queues (e.g., a **ready queue** for processes waiting for the CPU or a **device queue** for processes waiting for I/O). The scheduler selects a process from the ready queue based on the information in its PCB.
- **State Management:** As a process changes state (e.g., from _running_ to _waiting_ for a file to load), the OS updates the "Process State" field in its PCB and moves the PCB to the appropriate queue.
- **Resource Management:** The PCB tracks all the resources allocated to a process, such as memory, open files, and I/O devices. When a process terminates, the OS uses this information to deallocate all its resources.

---

### Example Scenario: A Simple Context Switch

Imagine a computer with one CPU running two processes, **Process A** and **Process B**.

1. **Process A is Running:** The CPU scheduler selects Process A. Its context (CPU registers, program counter) is loaded from its PCB, its state is set to _Running_, and it begins to execute.
    
2. **Interrupt Occurs:** Process A's allotted time slice (quantum) expires, triggering an interrupt. The OS takes control.
    
3. **Save Context for Process A:** The OS saves the current values of the CPU registers and the program counter into **Process A's PCB**. It then updates Process A's state from _Running_ to _Ready_ and moves its PCB to the back of the ready queue.
    
4. **Load Context for Process B:** The scheduler selects Process B next. The OS loads the CPU registers and program counter using the values stored in **Process B's PCB**.
    
5. **Process B Resumes:** The OS updates Process B's state to _Running_ and transfers control to it. Process B now executes exactly where it left off.