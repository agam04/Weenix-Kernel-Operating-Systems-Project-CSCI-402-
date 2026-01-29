# Weenix Kernel — Operating Systems Project (CSCI 402)

⚠️ **Source Code Notice**
The kernel source code is **not publicly available** due to course academic integrity and distribution policies. As a result, this repository contains **architecture explanations, screenshots, recordings, and technical write-ups** demonstrating the functionality and design of the implemented kernel components rather than the code itself.

The project was completed over the course of a full semester in a **team of two**, and involved designing and implementing large portions of a working OS kernel in C.


## Project Structure

The kernel was implemented in **three major phases**, each focusing on a core operating system subsystem:

1. **Processes, Threads, Scheduling, and Synchronization**
2. **Virtual File System (VFS) and System V–style File System**
3. **Virtual Memory, Demand Paging, and Copy-on-Write Fork**

Each phase built directly on the previous one, resulting in a fully functional multitasking kernel capable of running user programs under heavy stress.



## Features Implemented

### 1. Processes, Threads & Scheduling

* Full **process lifecycle management**, including creation, termination, parent–child relationships, and zombie cleanup
* Kernel **thread subsystem** supporting thread creation, cancellation, exit, and destruction
* **Preemptive round-robin scheduler** with runqueue management and an idle process
* Thread state transitions (RUNNING, RUNNABLE, SLEEPING, ZOMBIE) with race-free synchronization
* `waitpid` support for parent–child process synchronization and exit status propagation

### Synchronization Primitives

* Mutex locks for mutual exclusion in critical sections
* Wait queues and sleep/wakeup mechanisms for thread coordination
* Deadlock-free lock ordering and correct handling of race conditions under contention


### 2. Virtual File System (VFS)

* Abstract **VFS layer** decoupling system calls from filesystem implementations
* Reference-counted **vnode and file object caching** to prevent use-after-free bugs
* Full support for core POSIX-style file operations:

  * `open`, `close`, `read`, `write`, `lseek`, `stat`
  * Directory operations including creation, removal, linking, and unlinking
* Per-process **file descriptor tables** with correct sharing semantics across `fork`
* Support for **character and block device files**

### System V File System (S5FS)

* Inode allocation, deallocation, and in-memory caching
* Directory entry management and pathname resolution
* Free-block bitmap–based disk block allocation
* Demand-paged disk I/O via page frames


### 3. Virtual Memory & Copy-on-Write Fork

* Layered VM architecture separating:

  * Address spaces
  * Memory regions
  * Backing memory objects
* Two-level **x86 page table** management with TLB synchronization
* User/kernel address space separation with strict permission enforcement
* `mmap` and `munmap` system calls supporting private, shared, anonymous, and fixed mappings
* **Demand paging** with a comprehensive page fault handler
* **Copy-on-write fork implementation**:

  * Shadow memory objects for private mappings
  * Shared bottom objects to avoid redundant copying
  * Zero-copy fork semantics until first write
* Correct duplication of file descriptors, address spaces, and thread contexts during `fork`

## Tools & Technologies

* **Language:** C (kernel-level, no standard library)
* **Architecture:** x86 (paging, privilege levels, TLBs)
* **Debugging:** GDB with QEMU, kernel assertions, structured debug tracing
* **Concepts:**

  * Scheduling & synchronization
  * Virtual memory & paging
  * Copy-on-write
  * File systems & VFS design
  * Low-level concurrency and race condition debugging


## Why This Repository Exists

While the source code cannot be shared, this repository exists to:

* Demonstrate **systems-level design and implementation experience**
* Provide architectural insight into a non-trivial OS kernel
* Serve as a reference for discussions in interviews or technical evaluations

If you’re interested in **how** specific components were designed or debugged, feel free to explore the documentation or reach out.
