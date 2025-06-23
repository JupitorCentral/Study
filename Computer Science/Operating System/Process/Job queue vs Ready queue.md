- [x] Job queue vs Ready queue ➕ 2025-06-17 ✅ 2025-06-23


A **job queue** consists of all processes in the system, including those spooled to a mass-storage device like a disk, waiting for execution. In contrast, a **ready queue** is a list of all processes that are residing in main memory and are ready and waiting to be executed by the CPU.

### Key Distinctions:

- **Location:** The job queue includes all processes, even those on disk awaiting memory allocation, while the ready queue contains only processes that are already in main memory.
    
- **Scheduler:** The **long-term scheduler** (or job scheduler) selects processes from the job queue and loads them into memory. The **short-term scheduler** (or CPU scheduler) selects a process from the ready queue to allocate the CPU to it.
    
- **Frequency of Use:** The long-term scheduler, which manages the job queue, executes much less frequently than the short-term scheduler, which manages the ready queue. The short-term scheduler must run often, sometimes as frequently as every 100 milliseconds, because a process may only execute for a very short time before needing to wait for an I/O request.