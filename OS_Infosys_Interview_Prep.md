# Operating System — Infosys Interview Preparation

## 🗺️ Question Roadmap

**Basics**
1. 🔴 What is an Operating System? Why is it needed?
2. 🔴 Process vs Thread
3. 🔴 Process States/Lifecycle
4. 🔴 What is a Context Switch?
5. 🟠 Multitasking vs Multiprogramming vs Multithreading  

**Concurrency**
6. 🔴 Mutex vs Semaphore
7. 🟠 Deadlock — conditions and prevention
8. 🟡 Race Condition & Critical Section

**Memory Management**
9. 🔴 Paging vs Segmentation
10. 🟠 Virtual Memory
11. 🟡 Fragmentation (Internal vs External)

**Scheduling**
12. 🟠 CPU Scheduling Algorithms (FCFS, SJF, Round Robin, Priority)
13. 🟡 Starvation & Aging

---

## A. 🔴 MUST KNOW

### Q1. What is an Operating System? Why is it needed?

**Answer:**
> "An OS is system software that acts as an interface between the user/applications and the hardware. It manages resources like CPU, memory, storage, and I/O devices, so applications don't need to talk to hardware directly."

**Interview Tip:** Mention core responsibilities: process management, memory management, file system management, I/O management, security.
**Follow-up:** Give examples of OS types (batch, real-time, distributed, multi-user).

---

### Q2. Process vs Thread

| Aspect | Process | Thread |
|---|---|---|
| Definition | An independent program in execution | A lightweight unit of execution within a process |
| Memory | Has its own separate memory space | Shares memory space with other threads in same process |
| Communication | Needs IPC (Inter-Process Communication) — slower | Shares data directly — faster |
| Creation cost | Expensive (heavyweight) | Cheap (lightweight) |
| Crash impact | One process crashing doesn't affect another | A thread crash can potentially crash the whole process |

**Example (real-world):** Opening two different applications (say, Chrome and VS Code) = two processes. Multiple tabs within Chrome, in some cases, run as threads or separate processes depending on architecture.

**Interview Tip:** Say clearly: "Threads within the same process share the code, data, and heap segment but have their own stack and registers." This level of detail impresses interviewers.
**Common mistake:** Saying threads and processes are "basically the same thing."
**Follow-up:** What is multithreading used for? What is the GIL in Python? (If Python is on your resume — Python threads don't achieve true parallelism for CPU-bound tasks due to the GIL.)

---

### Q3. Process States / Lifecycle

**Explain the concept first:** A process moves through different states during its lifetime, managed by the OS scheduler.

```
New → Ready → Running → (Waiting) → Ready → ... → Terminated
```

| State | Meaning |
|---|---|
| New | Process being created |
| Ready | Waiting for CPU time |
| Running | Currently executing on CPU |
| Waiting/Blocked | Waiting for I/O or an event |
| Terminated | Execution finished |

**Interview Tip:** Draw this verbally as a flow: "A process starts as New, moves to Ready once loaded, gets Running when the scheduler picks it, may go to Waiting if it needs I/O, then back to Ready, and finally Terminated."
**Follow-up:** What is a PCB (Process Control Block)? (A data structure the OS uses to store all info about a process — state, PC, registers, memory limits.)

---

### Q4. What is a Context Switch?

> "A context switch is the process of saving the state of a currently running process/thread and loading the state of another one, so the CPU can switch between them. It's how multitasking is achieved on a single CPU core."

**Interview Tip:** Mention it has overhead — saving/restoring registers, program counter, memory maps takes CPU time, so too many context switches hurt performance.
**Follow-up:** What triggers a context switch? (Interrupt, I/O request, time slice expiry, higher-priority process arriving.)

---

### Q5. Mutex vs Semaphore

**Explain the concept first:** Both are synchronization tools to prevent multiple threads/processes from causing race conditions when accessing shared resources.

| Aspect | Mutex | Semaphore |
|---|---|---|
| Type | Locking mechanism | Signaling mechanism |
| Value | Binary (locked/unlocked) | Can be binary or counting (integer value) |
| Ownership | Owned by the thread that locked it — only that thread can unlock | No ownership — any thread/process can signal |
| Use case | Protecting a single shared resource (critical section) | Controlling access to a resource pool (e.g., N available connections) |

**Example (conceptual, not literal code):**
```python
import threading
lock = threading.Lock()   # mutex

def critical_section():
    lock.acquire()
    try:
        # only one thread here at a time
        pass
    finally:
        lock.release()
```

**Interview Tip:** Say: "A mutex is like a single key to a room — only the person holding it can enter and only they can return it. A semaphore is like a parking lot with N slots — any car can take or release a slot." This analogy is easy to say out loud and interviewers like it.
**Follow-up:** What is a binary semaphore vs mutex then — are they the same? (Conceptually similar, but ownership rules differ.)

---

## B. 🟠 VERY IMPORTANT

### Q6. Multitasking vs Multiprogramming vs Multithreading

| Term | Meaning |
|---|---|
| Multiprogramming | Multiple programs reside in memory; CPU switches when one is idle (waiting for I/O) — maximizes CPU utilization |
| Multitasking | Multiple tasks/processes appear to run simultaneously via fast context switching — focuses on user responsiveness |
| Multithreading | Multiple threads run within a single process, sharing memory |

**Interview Tip:** Give a one-line differentiator: "Multiprogramming is about keeping the CPU busy; multitasking is about switching fast enough that it *feels* simultaneous to the user; multithreading is doing this within one process."

---

### Q7. Deadlock — Conditions and Prevention

**Explain the concept first:** A deadlock is a situation where two or more processes are stuck waiting for each other's resources, and none can proceed.

**4 Necessary Conditions (Coffman conditions) — all must hold for deadlock:**
1. **Mutual Exclusion** — resource can't be shared
2. **Hold and Wait** — process holds one resource while waiting for another
3. **No Preemption** — resource can't be forcibly taken away
4. **Circular Wait** — a cycle of processes each waiting on the next

**Prevention:** Break any one of the four conditions — e.g., request all resources at once (breaks hold-and-wait), impose resource ordering (breaks circular wait).

**Interview Tip:** Real-world analogy: "Two people trying to pass each other in a narrow hallway, each waiting for the other to step aside." Mention that Deadlock **Detection** (using resource allocation graphs) is different from Deadlock **Prevention** — know the distinction.
**Follow-up:** What is the Banker's Algorithm? (An algorithm to avoid deadlock by only granting resource requests that keep the system in a "safe state" — high level answer is enough for freshers.)

---

### Q8. Race Condition & Critical Section

> "A race condition occurs when two or more threads/processes access shared data concurrently, and the final outcome depends on the timing/order of execution — leading to unpredictable results."

**Critical Section:** The part of the code that accesses shared resources and must not be executed by more than one thread at a time.

**Example:** Two threads incrementing a shared counter `count += 1` without a lock — because `+=` isn't atomic, both could read the same old value and one increment gets lost.

**Interview Tip:** Tie this directly back to mutex/semaphore as the *solution* to race conditions — interviewers often ask these as a connected pair.

---

### Q9. Paging vs Segmentation

**Explain the concept first:** Both are memory management schemes that let a process's memory be non-contiguous in physical RAM.

| Aspect | Paging | Segmentation |
|---|---|---|
| Division basis | Fixed-size blocks (pages) | Variable-size logical blocks (segments — e.g., code, stack, heap) |
| Visibility to programmer | Invisible — pure OS/hardware mechanism | Reflects logical program structure |
| Fragmentation | Internal fragmentation possible | External fragmentation possible |
| Address | Single address (page number + offset) | Two-part address (segment number + offset) |

**Interview Tip:** Say: "Paging divides memory into fixed-size pages for uniform management, while segmentation divides a program into logical units like code, data, and stack, which can vary in size."
**Follow-up:** What is Virtual Memory and how does paging relate to it?

---

### Q10. Virtual Memory

> "Virtual memory is a technique that lets a process use more memory than what's physically available in RAM, by using disk space (swap) as an extension of RAM. It uses paging to map virtual addresses to physical addresses."

**Interview Tip:** Mention **Demand Paging** — pages are loaded into RAM only when needed, and a **Page Fault** occurs when a required page isn't in RAM and must be fetched from disk.
**Follow-up:** What is thrashing? (Excessive page swapping causing more time spent on paging than actual execution.)

---

### Q11. CPU Scheduling Algorithms

| Algorithm | Description | Pros/Cons |
|---|---|---|
| FCFS (First Come First Serve) | Executes in arrival order | Simple, but can cause "convoy effect" (short jobs stuck behind long ones) |
| SJF (Shortest Job First) | Picks shortest burst time next | Optimal average wait time, but can cause starvation of long jobs |
| Round Robin | Each process gets a fixed time slice, cycles through queue | Fair, good for time-sharing, but performance depends heavily on time-quantum size |
| Priority Scheduling | Highest priority process runs first | Can cause starvation of low-priority processes (solved via aging) |

**Interview Tip:** Round Robin is the most commonly asked in detail — be ready to explain what happens with a small example (3 processes, fixed quantum, walk through the Gantt chart verbally).
**Follow-up:** What is Starvation? What is Aging? (Aging gradually increases the priority of a waiting process to prevent starvation.)

---

## C. 🟡 GOOD TO KNOW

### Q12. Internal vs External Fragmentation

| Type | Cause | Occurs in |
|---|---|---|
| Internal | Allocated memory block is larger than needed, wasting space inside the block | Paging (fixed-size blocks) |
| External | Free memory is split into small non-contiguous chunks, unusable for larger requests despite enough total free space | Segmentation (variable-size blocks) |

**Interview Tip:** Simple memory trick: "Internal = wasted space *inside* an allocated block. External = wasted space *between* allocated blocks."

---

### Q13. Starvation & Aging

Already touched on above — reinforce as a standalone answer if asked directly: Starvation happens when a process waits indefinitely because the scheduler keeps favoring others (common in Priority and SJF scheduling). Aging fixes this by increasing priority over time.

---

## ⭐ Top Questions to Memorize

1. Process vs Thread
2. Process states/lifecycle diagram
3. Mutex vs Semaphore with the room-key/parking-lot analogy
4. Deadlock — 4 necessary conditions
5. Paging vs Segmentation
6. What is a context switch and its overhead
7. Virtual memory and demand paging
8. Round Robin scheduling — how it works
9. Race condition and critical section
10. Internal vs External fragmentation
11. Starvation vs Aging
12. Multiprogramming vs Multitasking vs Multithreading

## 🎯 Interview Preparation Checklist

- [ ] Can draw/describe the process lifecycle diagram from memory
- [ ] Can explain Mutex vs Semaphore with a real-world analogy
- [ ] Can list all 4 deadlock conditions without notes
- [ ] Can explain paging vs segmentation clearly with the fragmentation link
- [ ] Can walk through a Round Robin scheduling example with a small process set
- [ ] Comfortable explaining race condition with a concrete counter example
