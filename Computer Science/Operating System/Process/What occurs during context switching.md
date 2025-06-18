- [ ] What occurs during context switching ➕ 2025-06-16 


A **context switch** is the mechanism the operating system uses to switch the CPU from one process to another. This allows multiple processes to share a single CPU, which is fundamental for modern multitasking. The switch is pure **overhead**, as no user-level work is done during the process.

Here is a step-by-step breakdown of the events.

### 1. The Trigger

A context switch is initiated when the currently running process can no longer use the CPU. This can be caused by:

- An **interrupt**, such as the process's time slice (quantum) expiring.
    
- The process making a **system call**, such as a request for I/O, which requires it to wait.
- The process voluntarily yielding the CPU.
    

### 2. Saving the Current Process's State

The OS must perfectly preserve the state of the outgoing process (let's call it **Process A**) so it can be resumed later as if nothing happened. The `dispatcher`, a core OS module, saves the current context of Process A into its **Process Control Block (PCB)**.

This saved information includes:

- **Program Counter:** The address of the next instruction to execute.
- **CPU Registers:** The current values of all processor registers (e.g., accumulators, stack pointers).
    
- **Process State:** The state is updated in the PCB (e.g., from _Running_ to _Ready_ or _Waiting_).
- **Memory-Management Info:** Information like a pointer to the process's page table. In some cases, the Translation Look-aside Buffer (TLB) may need to be flushed.
    

### 3. Loading the New Process's State

Once Process A's state is saved, the dispatcher loads the context of the incoming process (**Process B**) from its PCB into the CPU.

This involves:

- Loading Process B's saved **Program Counter**.
- Restoring the values of all **CPU Registers** from Process B's PCB.
- Updating the CPU's memory-management registers with Process B's information.
- Changing Process B's state in its PCB to _Running_.

### 4. Resuming Execution

The dispatcher completes its work by jumping to the address loaded into the program counter. **Process B** now resumes execution exactly where it last left off.


### Scenario: Switching from a Word Processor to a Web Browser

Let's imagine you are editing a large document in a **Word Processor (Process A)** and have a **Web Browser (Process B)** open in the background. Both are in memory, and their information is stored in their respective Process Control Blocks (PCBs).

**1. Process A is Actively Running**

- **State:** The Word Processor (Process A) is currently using the CPU. Its state in its PCB is set to _Running_.
- **Hardware Context:** The CPU's program counter is stepping through Process A's code. Its registers hold data specific to your document editing. The Memory Management Unit (MMU) is using Process A's page table to translate its logical memory addresses into physical RAM addresses. The Translation Look-aside Buffer (TLB) has cached several of these recent translations for speed.
    

**2. The Trigger: An I/O Request**

- You hit `Ctrl+S` to save your document.
- Process A executes a `save` instruction, which makes a **system call** to the operating system to write the file data to the hard disk.
- This system call creates an interrupt, pausing Process A and immediately transferring control to the OS kernel. The CPU is now in kernel mode.

**3. The OS Intervenes: A Switch is Necessary**

- The OS sees that Process A must now wait for the slow hard disk to complete the write operation. Letting the CPU sit idle during this time would be extremely inefficient.
- The **CPU scheduler** is invoked. It looks at the **ready queue** and sees that the Web Browser (Process B) is ready to run. It decides to switch the CPU to Process B.

**4. The Context Switch: Saving Process A (The "Switch Out")**

The **dispatcher** module now meticulously saves every part of Process A's current environment into its PCB:

- **Program Counter:** It saves the address of the instruction _immediately following_ the `save` system call. This is where Process A must resume later.
- **CPU Registers:** It saves a complete snapshot of all CPU registers.
- **State Update:** It changes Process A's state in its PCB from _Running_ to **_Waiting_**.
- **Queue Management:** It moves Process A's PCB from the "running" slot to an **I/O wait queue** associated with the hard disk.

**5. The Context Switch: Loading Process B (The "Switch In")**

Now the dispatcher prepares the CPU for Process B:

- **Load from PCB:** It accesses Process B's PCB and loads its saved program counter and CPU registers into the hardware.
- **Memory Management Update:** This is a critical step. The dispatcher updates the MMU's internal pointers to use **Process B's page table**. The CPU now knows how to access the memory belonging to the browser.
- **TLB Flush:** Because the TLB still contains cached memory translations for Process A, they are now useless and potentially harmful. The OS executes a privileged instruction to **flush the TLB**, clearing out the old entries.
- **State Update:** It changes Process B's state in its PCB to **_Running_**.

**6. Execution Resumes**

- The dispatcher's final act is to jump to the instruction address loaded from Process B's program counter.
- The Web Browser (Process B) is now executing on the CPU, rendering a webpage or running a script, completely unaware that just milliseconds ago the CPU was working on behalf of the Word Processor.

Later, when the hard disk finishes saving the file, it will send an interrupt to the OS. The OS will then move Process A from the I/O wait queue back to the ready queue, making it eligible to run again when the scheduler chooses it next.


### Translation Look-aside Buffer (TLB)


The **Translation Look-aside Buffer (TLB)** is a small, high-speed hardware cache that is used to speed up the process of translating logical addresses to physical addresses. Think of it as a "cheat sheet" for the page table. Because accessing the full page table in main memory can be slow, the TLB stores a few of the most recently used page-table entries.

Here are the key aspects of the TLB, followed by a scenario illustrating its use:

- **Function**: The TLB contains page-table entries that map page numbers to frame numbers. When a logical address is generated, the hardware first checks the TLB to see if the required page-table entry is present.
    
- **Structure**: The TLB is an associative, high-speed memory. Each entry consists of a key (or tag) and a value. The key is the page number, and the value is the corresponding frame number in physical memory.
    
- **Performance**: If the page number is found in the TLB (a "TLB hit"), the frame number is retrieved very quickly, and the physical address can be formed almost immediately. If the page number is not in the TLB (a "TLB miss"), the system must perform a slower memory access to the full page table to get the frame number. The system then adds this new page-to-frame mapping to the TLB, possibly replacing another entry if the TLB is full.
    
- **Hit Ratio**: The percentage of times that a page number is found in the TLB is called the **hit ratio**. A high hit ratio is crucial for performance. For example, with a 98-percent hit ratio, the slowdown in memory access time is only about 22 percent, whereas with an 80-percent hit ratio, the slowdown is a more significant 40 percent.
    
- **Address-Space Identifiers (ASIDs)**: Some TLBs store an address-space identifier (ASID) in each entry. The ASID uniquely identifies a process and is used to provide address-space protection. It allows the TLB to hold entries for multiple processes at the same time, preventing the need to flush the entire TLB with each context switch.
    

### Scenario: Accessing an Array Element

Let's walk through a scenario where a process accesses an array element, `array[i]`, and see how the TLB is used. Assume the following:

- The logical address for `array[i]` translates to page number **15** and offset **240**.
- The page table entry for page **15** maps to frame number **88**.

**Scenario 1: TLB Hit**

This is the ideal case where the translation for page 15 is already in the TLB.

1. **CPU Generates Logical Address**: The process executes an instruction to access `array[i]`, and the CPU generates the logical address corresponding to page 15, offset 240.
2. **Page Number Presented to TLB**: The hardware presents the page number, 15, to the TLB.
    
3. **TLB Search**: The TLB hardware simultaneously compares the page number 15 with all the keys in its entries.
    
4. **TLB Hit Occurs**: An entry in the TLB matches page 15. The TLB returns the corresponding frame number, 88.
    
5. **Physical Address Formed**: The frame number (88) is combined with the offset (240) to form the physical address.
6. **Memory Access**: The system accesses the physical memory at the calculated address.

The entire translation process is extremely fast because it avoids a main memory access to the page table. The diagram below illustrates this process.

**Scenario 2: TLB Miss**

This is what happens when the translation for page 15 is not in the TLB.

1. **CPU Generates Logical Address**: The process generates the logical address for page 15, offset 240.
2. **Page Number Presented to TLB**: The page number, 15, is sent to the TLB.
    
3. **TLB Search**: The TLB finds no entry with page number 15.
4. **TLB Miss Occurs**: The hardware signals a TLB miss.
    
5. **Page Table Access**: A memory reference to the page table (pointed to by the Page-Table Base Register, or PTBR) is initiated to find the entry for page 15.
    
6. **Frame Number Retrieved**: The system reads the page table and finds that page 15 maps to frame 88.
7. **Physical Address Formed**: The frame number (88) is combined with the offset (240) to form the physical address.
8. **Memory Access**: The physical memory is accessed.
9. **TLB Update**: The page number (15) and frame number (88) are added to the TLB. If the TLB was full, an existing entry is replaced according to a replacement policy (like LRU).
    

Now, any subsequent access to page 15 will result in a fast TLB hit.
