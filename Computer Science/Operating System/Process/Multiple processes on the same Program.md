- [ ] Multiple processes on the same Program ➕ 2025-06-16 


### The Fundamental Rule: Program vs. Process

First, it's essential to distinguish between a program and a process:

- **A Program** is a passive, static entity. It's an executable file stored on your disk, containing a set of instructions (e.g., `chrome.exe` or the `/bin/ls` file).
- **A Process** is an active, dynamic entity. It's a _program in execution_. When you run a program, the operating system loads it into memory and creates a process to manage its execution.

The key idea is that **one program can give rise to many processes**. Here are two primary ways this happens.

---

### Scenario 1: Multiple Independent Copies

This is the simplest case. A user runs the same application program multiple times.

- **How it works:** Every time you launch an application like a web browser, text editor, or calculator, the operating system creates a new, separate process.
- **Result:** You have multiple distinct processes running. While they all execute the code from the **same program file**, each process has its own private memory, its own variables, and its own execution state. They are independent copies.

> **Example:** You open two Google Chrome windows. You now have two separate Chrome processes running. They share the same program code (`chrome.exe`) but are independent and have their own memory for their specific tabs and data.

---

### Scenario 2: The Parent-Child Model (`fork()` and `exec()`)

This is a more powerful and fundamental mechanism used by operating systems like UNIX and Linux to create and manage processes. It allows a single program to spawn new processes that can either help the original program or go on to do entirely different things.

This happens in a sequence:

#### Step A: Duplication with `fork()`

A running process (the **parent**) can call the `fork()` system call. When it does, the operating system creates a new **child process** that is an _exact duplicate_ of the parent. At this precise moment, you have two identical processes running the same program code.

#### Step B: Divergence with the `fork()` Return Value

How can these identical processes do different things? The `fork()` call returns a different value to the parent and the child:

- In the **child process**, `fork()` returns `0`.
- In the **parent process**, `fork()` returns the unique Process ID (PID) of its new child.

A program can use simple `if/else` logic to check this return value and direct the parent and child down different execution paths within the same program.

**Pseudo-code Example:**

C

```
pid = fork();

if (pid == 0) {
    // I am the CHILD process because pid is 0.
    // I will execute this block of code to perform the child's task.
} else {
    // I am the PARENT process because pid is a non-zero number.
    // I will execute this block of code to perform the parent's task.
}
```

This is how one program can contain the code for two different functions, with the `fork()` call acting as the branch point that assigns each function to a different process.

#### Step C: Transformation with `exec()`

A very common pattern is for the child process to immediately use an `exec()` system call. The `exec()` call **replaces the child's entire memory space** with a completely new program. The child process essentially transforms itself to run a different program.

> **Example: A Command Shell**
> 
> 1. You type `ls` into your command shell and press Enter. The `shell` is the **parent process**.
> 2. The `shell` process calls **`fork()`** to create a duplicate of itself (a **child process**).
> 3. The child process (where the return value of `fork()` was 0) immediately calls **`exec()`** to load the `/bin/ls` program.
> 4. The child process is no longer running the shell program; its memory has been completely replaced with the `ls` program, which now runs.
> 5. The parent `shell` process waits for the child (`ls`) to finish and then prompts you for the next command.
