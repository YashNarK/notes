# Operating System Fundamentals — Deep Dive

---

## Table of Contents

- [What is an Operating System?](#what-is-an-operating-system)
- [Process Management](#process-management)
- [Threads vs Processes](#threads-vs-processes)
- [CPU Scheduling Algorithms](#cpu-scheduling-algorithms)
- [Concurrency and Parallelism](#concurrency-and-parallelism)
- [Deadlocks](#deadlocks)
- [Memory Management](#memory-management)
- [Virtual Memory and Paging](#virtual-memory-and-paging)
- [File Systems](#file-systems)
- [I/O Management](#io-management)
- [Inter-Process Communication (IPC)](#inter-process-communication-ipc)
- [System Calls](#system-calls)
- [User Space vs Kernel Space](#user-space-vs-kernel-space)
- [Linux Essentials](#linux-essentials)
- [Permissions and Ownership (Linux)](#permissions-and-ownership-linux)
- [Signals](#signals)
- [Boot Process](#boot-process)
- [Containers vs VMs](#containers-vs-vms)

---

# What is an Operating System?

An operating system (OS) is the **software layer** between hardware and applications. It manages hardware resources and provides services for programs to run.

## Core Responsibilities

| Responsibility | Description |
|---|---|
| **Process Management** | Creating, scheduling, and terminating processes |
| **Memory Management** | Allocating and tracking RAM usage |
| **File System Management** | Reading, writing, organizing files on disk |
| **I/O Management** | Interfacing with devices (disk, network, keyboard, display) |
| **Security & Access Control** | User authentication, file permissions, process isolation |
| **Networking** | TCP/IP stack, sockets, network interfaces |

## Types of Operating Systems

| Type | Description | Examples |
|---|---|---|
| **Batch OS** | Executes jobs in batches without user interaction | Early mainframes |
| **Time-Sharing OS** | Shares CPU among multiple users/processes | Unix, Linux |
| **Real-Time OS (RTOS)** | Guarantees response within strict time constraints | VxWorks, FreeRTOS |
| **Distributed OS** | Manages a group of networked machines as one system | Google's Borg |
| **Embedded OS** | Designed for embedded systems with limited resources | Android Things, Zephyr |

---

# Process Management

## What is a Process?

A process is a **program in execution**. It includes the program code, current activity (program counter, registers), stack, heap, and data section.

## Process States

```
                ┌─── Admitted ──────┐
                │                   ▼
            ┌───────┐          ┌─────────┐     Scheduler      ┌─────────┐
  Start ──► │  New  │ ───────► │  Ready  │ ──────────────────► │ Running │
            └───────┘          └─────────┘                     └─────────┘
                                    ▲                            │    │
                                    │                            │    │
                               I/O Complete                  Interrupt │
                                    │                            │    │
                                    │     ┌──────────┐           │    │ Exit
                                    └──── │ Waiting  │ ◄─────────┘    │
                                          │(Blocked) │                │
                                          └──────────┘            ┌───▼──────┐
                                                                  │Terminated│
                                                                  └──────────┘
```

| State | Description |
|---|---|
| **New** | Process is being created |
| **Ready** | Waiting to be assigned to the CPU |
| **Running** | Currently executing on the CPU |
| **Waiting (Blocked)** | Waiting for I/O operation or event |
| **Terminated** | Finished execution |

## Process Control Block (PCB)

Each process has a PCB in the OS kernel containing:
- Process ID (PID)
- Process state
- Program counter (next instruction address)
- CPU registers
- Memory allocation info
- Open files list
- Scheduling priority

---

# Threads vs Processes

## What is a Thread?

A thread is a **lightweight unit of execution** within a process. A process can have multiple threads that share the same memory space.

| Aspect | Process | Thread |
|---|---|---|
| **Memory** | Separate address space | Shared address space within process |
| **Creation Cost** | Heavy (needs new address space) | Lightweight (shares parent's memory) |
| **Communication** | IPC needed (pipes, sockets, shared memory) | Direct access to shared variables |
| **Isolation** | Crash in one doesn't affect others | Crash in one can crash the whole process |
| **Context Switch** | Expensive (save/load full address space) | Cheaper (only save/load registers and stack) |
| **Example** | Two separate Chrome windows | Multiple tabs within one Chrome process |

## Multi-threading Models

| Model | Description |
|---|---|
| **Many-to-One** | Many user threads mapped to one kernel thread. If one blocks, all block. |
| **One-to-One** | Each user thread maps to a kernel thread. More concurrency, more overhead. (Linux, Windows) |
| **Many-to-Many** | Multiple user threads mapped to multiple kernel threads. Best flexibility. |

### Relevance to Node.js

Node.js is **single-threaded for JavaScript execution** (event loop) but uses a **thread pool** (libuv, 4 threads by default) for blocking I/O operations (file system, DNS, crypto). `Worker Threads` can be used for CPU-intensive tasks.

---

# CPU Scheduling Algorithms

The CPU scheduler decides **which process/thread runs next** when the CPU is free.

## Key Metrics

| Metric | Description |
|---|---|
| **Throughput** | Number of processes completed per unit time |
| **Turnaround Time** | Total time from arrival to completion |
| **Waiting Time** | Time spent in the ready queue |
| **Response Time** | Time from request to first response |
| **CPU Utilization** | Percentage of time CPU is busy |

## Common Scheduling Algorithms

### 1. First-Come, First-Served (FCFS)

- Processes execute in **order of arrival**.
- **Non-preemptive** — once started, runs to completion.
- **Problem**: Convoy effect — short jobs stuck behind a long one.

### 2. Shortest Job First (SJF)

- Process with the **shortest burst time** runs next.
- Optimal for minimizing average waiting time.
- **Problem**: Starvation — long processes may never run. Requires knowing burst time in advance.

### 3. Round Robin (RR)

- Each process gets a fixed **time quantum** (e.g., 10ms).
- After the quantum, the process is moved to the back of the ready queue.
- **Preemptive** — fair to all processes.
- **Trade-off**: Small quantum = more context switches; large quantum = approaches FCFS.

### 4. Priority Scheduling

- Each process has a **priority**. Higher priority runs first.
- Can be preemptive or non-preemptive.
- **Problem**: Starvation of low-priority processes. Solved by **aging** (gradually increasing priority).

### 5. Multilevel Queue

- Ready queue split into **multiple queues** (e.g., foreground/interactive vs background/batch).
- Each queue has its own scheduling algorithm.
- Processes don't move between queues.

### 6. Multilevel Feedback Queue

- Like multilevel queue, but processes **can move between queues** based on behavior.
- CPU-bound processes sink to lower-priority queues; I/O-bound processes rise.
- Most general and flexible — used by modern OS kernels (Linux CFS is similar in spirit).

---

# Concurrency and Parallelism

| Concept | Description |
|---|---|
| **Concurrency** | Multiple tasks **make progress** during overlapping time periods (may not run simultaneously). Interleaving on a single core. |
| **Parallelism** | Multiple tasks **execute simultaneously** on multiple CPU cores. |

```
Concurrency (single core):
  Task A: ████░░░░████░░░░
  Task B: ░░░░████░░░░████

Parallelism (multi-core):
  Core 1: ████████████████  ← Task A
  Core 2: ████████████████  ← Task B
```

## Race Conditions

A **race condition** occurs when multiple threads/processes access shared data concurrently and the outcome depends on **timing**.

```
Thread A: read balance (100) → balance + 50 → write (150)
Thread B: read balance (100) → balance - 30 → write (70)
// Expected: 120, but Thread B overwrites Thread A's update
```

## Synchronization Primitives

| Primitive | Description |
|---|---|
| **Mutex** | Mutual exclusion lock — only one thread can hold it at a time |
| **Semaphore** | Counter-based — allows N threads to access a resource simultaneously |
| **Monitor** | Higher-level — combines mutex + condition variables (used in Java's `synchronized`) |
| **Spinlock** | Busy-waiting lock — thread loops checking if lock is free (used in kernel) |
| **Read-Write Lock** | Multiple readers OR one writer at a time |

---

# Deadlocks

A **deadlock** occurs when two or more processes are waiting for each other to release resources, and none can proceed.

## Four Necessary Conditions (All must hold)

| Condition | Description |
|---|---|
| **Mutual Exclusion** | At least one resource is held in a non-sharable mode |
| **Hold and Wait** | A process holds resources while waiting for others |
| **No Preemption** | Resources cannot be forcibly taken away from a process |
| **Circular Wait** | A circular chain of processes, each waiting for a resource held by the next |

## Deadlock Example

```
Process A: holds Resource 1, waiting for Resource 2
Process B: holds Resource 2, waiting for Resource 1
→ Neither can proceed = DEADLOCK
```

## Handling Deadlocks

| Strategy | Description |
|---|---|
| **Prevention** | Eliminate one of the four conditions (e.g., enforce ordering of resource requests) |
| **Avoidance** | Dynamically check if granting a request could lead to deadlock (Banker's Algorithm) |
| **Detection** | Allow deadlocks to occur, detect them (resource allocation graph), then recover |
| **Ignorance** | Assume deadlocks are rare and just restart if they happen (Linux, Windows approach for most cases) |

---

# Memory Management

## Memory Hierarchy

```
Fastest ─────────────────────────────────── Slowest
Registers → L1 Cache → L2 Cache → L3 Cache → RAM → SSD → HDD
Smallest ─────────────────────────────────── Largest
Most Expensive ──────────────────────── Least Expensive
```

| Level | Size | Speed | Latency |
|---|---|---|---|
| CPU Registers | ~1 KB | ~0.3 ns | Immediate |
| L1 Cache | 32-64 KB | ~1 ns | ~1 cycle |
| L2 Cache | 256 KB – 1 MB | ~4 ns | ~10 cycles |
| L3 Cache | 4 – 64 MB | ~12 ns | ~40 cycles |
| RAM | 8 – 128 GB | ~100 ns | ~200 cycles |
| SSD | 256 GB – 4 TB | ~100 µs | ~100,000 cycles |
| HDD | 1 – 20 TB | ~10 ms | ~10,000,000 cycles |

## Memory Allocation

### Contiguous Allocation

- Each process gets a **single continuous block** of memory.
- Simple but leads to **external fragmentation** (free memory scattered in small chunks).

### Non-Contiguous Allocation

- Process memory can be spread across non-adjacent blocks.
- Used by **paging** and **segmentation**.

## Fragmentation

| Type | Description | Solution |
|---|---|---|
| **External** | Free memory exists but is scattered in small chunks | Compaction, paging |
| **Internal** | Allocated block is larger than needed — unused space inside | Smaller allocation units |

---

# Virtual Memory and Paging

## Virtual Memory

Virtual memory gives each process its own **virtual address space**, mapped to physical RAM by the OS. Processes think they have the entire memory to themselves.

**Benefits**:
- Process isolation (can't access another process's memory)
- Allows running programs larger than physical RAM
- Simplifies memory management for programmers

## Paging

Paging divides memory into fixed-size blocks:
- **Pages**: Fixed-size blocks of virtual memory (typically 4 KB)
- **Frames**: Fixed-size blocks of physical memory (same size as pages)
- **Page Table**: Maps virtual pages to physical frames

```
Virtual Address Space          Physical Memory (RAM)
┌──────────┐                   ┌──────────┐
│  Page 0  │ ──── maps to ──► │ Frame 5  │
│  Page 1  │ ──── maps to ──► │ Frame 2  │
│  Page 2  │ ──── on disk ──► │  (swap)  │
│  Page 3  │ ──── maps to ──► │ Frame 7  │
└──────────┘                   └──────────┘
```

## Page Fault

When a process accesses a page that's **not currently in RAM** (it's on disk/swap):

1. CPU raises a **page fault** interrupt.
2. OS finds the page on disk.
3. OS loads it into a free frame (may need to evict another page).
4. OS updates the page table.
5. The interrupted instruction is restarted.

## Page Replacement Algorithms

| Algorithm | Description |
|---|---|
| **FIFO** | Replace the oldest page. Simple but can cause Belady's anomaly. |
| **LRU (Least Recently Used)** | Replace the page that hasn't been used for the longest time. Good performance. |
| **Optimal** | Replace the page that won't be used for the longest time in the future. Theoretical best (impossible to implement in practice). |
| **Clock (Second Chance)** | FIFO with a reference bit — gives pages a second chance before eviction. |

## Thrashing

When the system spends more time paging (swapping) than executing — too many processes competing for too little RAM. The CPU utilization drops despite high activity.

**Solution**: Reduce degree of multiprogramming or add more RAM.

---

# File Systems

A file system organizes and manages how data is **stored and retrieved** on disk.

## Common File Systems

| File System | OS | Features |
|---|---|---|
| **ext4** | Linux | Journaling, supports up to 1 EB, most common Linux FS |
| **NTFS** | Windows | Journaling, permissions, encryption, compression |
| **FAT32** | Cross-platform | Simple, 4 GB file size limit, no permissions |
| **exFAT** | Cross-platform | Like FAT32 but no 4 GB limit — good for USB drives |
| **APFS** | macOS | Optimized for SSD, snapshots, encryption |
| **ZFS** | Linux/BSD | Data integrity, snapshots, RAID-like features |
| **Btrfs** | Linux | Copy-on-write, snapshots, self-healing |

## File System Concepts

| Concept | Description |
|---|---|
| **Inode** | Data structure storing file metadata (permissions, size, timestamps, block pointers). Does NOT store the filename. |
| **Directory** | A special file that maps filenames to inode numbers. |
| **Hard Link** | Multiple filenames pointing to the same inode. Deleting one doesn't delete the file. |
| **Soft Link (Symlink)** | A pointer to another filename (like a shortcut). Can break if target is deleted. |
| **Journaling** | Logs changes before writing to disk — prevents corruption on crash. |

---

# I/O Management

## I/O Methods

| Method | Description |
|---|---|
| **Programmed I/O** | CPU actively waits for I/O device — wastes CPU cycles (polling). |
| **Interrupt-Driven I/O** | Device sends an interrupt when ready — CPU does other work while waiting. |
| **DMA (Direct Memory Access)** | I/O device transfers data directly to/from memory without CPU involvement. CPU only involved at start and end. |

## Blocking vs Non-Blocking I/O

| Type | Description | Example |
|---|---|---|
| **Blocking** | Thread waits until I/O completes | `fs.readFileSync()` in Node.js |
| **Non-Blocking** | Thread continues; gets notified when I/O is done | `fs.readFile()` (callback/async) |
| **Async I/O** | I/O happens in background; result delivered via callback/promise | Node.js event loop model |

### Relevance to Node.js

Node.js uses **non-blocking, async I/O** via the event loop (libuv). This is why a single-threaded Node.js server can handle thousands of concurrent connections — it never blocks waiting for I/O.

---

# Inter-Process Communication (IPC)

IPC mechanisms allow processes to exchange data and coordinate actions.

| Mechanism | Description | Use Case |
|---|---|---|
| **Pipes** | One-way byte stream between parent-child processes | `ls \| grep txt` |
| **Named Pipes (FIFO)** | Like pipes but with a filesystem name — any process can use | Unrelated process communication |
| **Message Queues** | Processes send/receive discrete messages through a kernel queue | Structured data exchange |
| **Shared Memory** | Multiple processes map the same region of memory | Fastest IPC — no kernel involvement for reads/writes |
| **Sockets** | Communication over network (or locally via Unix sockets) | Client-server communication |
| **Signals** | Asynchronous notifications sent to a process | `SIGTERM`, `SIGKILL`, `SIGHUP` |
| **Semaphores** | Synchronization primitive — controls access to shared resources | Preventing race conditions |

---

# System Calls

A system call is the **interface between user programs and the kernel**. When a program needs OS services (file access, network, memory), it makes a system call.

## Common System Calls

| Category | Examples | Description |
|---|---|---|
| **Process** | `fork()`, `exec()`, `wait()`, `exit()` | Create/manage processes |
| **File** | `open()`, `read()`, `write()`, `close()` | File operations |
| **Memory** | `mmap()`, `brk()`, `sbrk()` | Memory allocation |
| **Network** | `socket()`, `bind()`, `listen()`, `accept()` | Network I/O |
| **Device** | `ioctl()`, `read()`, `write()` | Device control |

## How a System Call Works

```
User Program → System Call (trap/interrupt) → Kernel Mode → Kernel executes → Return to User Mode
```

1. Program calls a library function (e.g., `write()`).
2. Library triggers a **trap** (software interrupt), switching to kernel mode.
3. Kernel handles the request.
4. Result returned, CPU switches back to user mode.

---

# User Space vs Kernel Space

| Aspect | User Space | Kernel Space |
|---|---|---|
| **Privilege** | Restricted — can't access hardware directly | Full access to hardware and memory |
| **Runs** | Applications, libraries, shell | Kernel, device drivers, core OS services |
| **Memory** | Each process has isolated virtual memory | Shared across all kernel operations |
| **Crash Impact** | Only the process crashes | Can crash the entire system (kernel panic) |
| **Access** | Via system calls only | Direct access to everything |

```
┌─────────────────────────────────────┐
│           User Space                │
│  ┌──────┐ ┌──────┐ ┌──────┐       │
│  │ App1 │ │ App2 │ │ App3 │       │
│  └──┬───┘ └──┬───┘ └──┬───┘       │
│     │        │        │            │
│     ▼        ▼        ▼            │
│  ┌──────────────────────────┐      │
│  │   System Call Interface   │      │
├──┴──────────────────────────┴──────┤
│           Kernel Space              │
│  ┌──────────────────────────────┐  │
│  │  Process  │ Memory  │  File  │  │
│  │  Mgmt     │ Mgmt    │  Sys   │  │
│  ├──────────────────────────────┤  │
│  │       Device Drivers         │  │
│  ├──────────────────────────────┤  │
│  │         Hardware              │  │
│  └──────────────────────────────┘  │
└─────────────────────────────────────┘
```

---

# Linux Essentials

## Process Management Commands

| Command | Purpose | Example |
|---|---|---|
| `ps` | List processes | `ps aux` (all processes) |
| `top` / `htop` | Real-time process monitor | `htop` |
| `kill` | Send signal to process | `kill -9 1234` (force kill PID 1234) |
| `bg` / `fg` | Background/foreground a job | `bg %1` |
| `nohup` | Run command immune to hangups | `nohup ./server &` |
| `nice` / `renice` | Set/change process priority | `nice -n 10 ./heavytask` |

## Memory & Disk Commands

| Command | Purpose | Example |
|---|---|---|
| `free` | Show memory usage | `free -h` |
| `df` | Show disk space usage | `df -h` |
| `du` | Show directory size | `du -sh /var/log` |
| `vmstat` | Virtual memory stats | `vmstat 1 5` |
| `iostat` | I/O statistics | `iostat -x 1` |

## File & Directory Commands

| Command | Purpose | Example |
|---|---|---|
| `find` | Search for files | `find / -name "*.log" -mtime -7` |
| `grep` | Search file contents | `grep -rn "error" /var/log/` |
| `tar` | Archive files | `tar -czf backup.tar.gz /data` |
| `ln` | Create links | `ln -s /path/to/file symlink` |
| `chmod` | Change permissions | `chmod 755 script.sh` |
| `chown` | Change ownership | `chown user:group file` |

## Networking Commands

| Command | Purpose | Example |
|---|---|---|
| `ss` / `netstat` | Show open ports/connections | `ss -tlnp` |
| `curl` | Make HTTP requests | `curl -v https://api.example.com` |
| `ip addr` | Show network interfaces | `ip addr show` |
| `iptables` | Configure firewall rules | `iptables -L` |
| `systemctl` | Manage services | `systemctl restart nginx` |

---

# Permissions and Ownership (Linux)

## File Permissions

Every file/directory has permissions for **Owner**, **Group**, and **Others**.

```
-rwxr-xr-- 1 john devs 4096 Mar 24 10:00 script.sh
│├──┤├──┤├──┤
│ │   │   └── Others: read only (r--)
│ │   └────── Group: read + execute (r-x)
│ └────────── Owner: read + write + execute (rwx)
└──────────── File type: - (regular file), d (directory), l (symlink)
```

## Numeric (Octal) Permissions

| Permission | Value |
|---|---|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

| Notation | Meaning |
|---|---|
| `777` | rwxrwxrwx — everyone can do everything (dangerous!) |
| `755` | rwxr-xr-x — owner full, others read + execute |
| `644` | rw-r--r-- — owner read/write, others read only |
| `600` | rw------- — owner read/write only (private files) |
| `700` | rwx------ — owner only (private executables/dirs) |

## Special Permissions

| Permission | Numeric | Effect |
|---|---|---|
| **SUID** (Set User ID) | 4xxx | File executes as the file owner, not the user running it. `chmod 4755 file` |
| **SGID** (Set Group ID) | 2xxx | File executes as the file group. On directories, new files inherit the group. |
| **Sticky Bit** | 1xxx | On directories, only the file owner can delete their files. Used on `/tmp`. |

---

# Signals

Signals are **asynchronous notifications** sent to processes by the kernel or other processes.

| Signal | Number | Default Action | Description |
|---|---|---|---|
| `SIGHUP` | 1 | Terminate | Hangup — terminal closed. Often used to reload config. |
| `SIGINT` | 2 | Terminate | Interrupt — sent by `Ctrl+C` |
| `SIGQUIT` | 3 | Core dump | Quit — sent by `Ctrl+\` |
| `SIGKILL` | 9 | Terminate | Force kill — **cannot be caught or ignored** |
| `SIGTERM` | 15 | Terminate | Graceful termination request (default for `kill`) |
| `SIGSTOP` | 19 | Stop | Pause process — **cannot be caught or ignored** |
| `SIGCONT` | 18 | Continue | Resume a stopped process |
| `SIGCHLD` | 17 | Ignore | Child process stopped or terminated |
| `SIGUSR1` | 10 | Terminate | User-defined signal 1 |
| `SIGUSR2` | 12 | Terminate | User-defined signal 2 |

### Handling Signals in Node.js

```javascript
process.on('SIGTERM', () => {
  console.log('Received SIGTERM — shutting down gracefully');
  server.close(() => {
    process.exit(0);
  });
});

process.on('SIGINT', () => {
  console.log('Received SIGINT (Ctrl+C)');
  process.exit(0);
});
```

---

# Boot Process

## Linux Boot Sequence

```
1. BIOS/UEFI → 2. Bootloader (GRUB) → 3. Kernel → 4. init/systemd → 5. User Space
```

| Step | Description |
|---|---|
| **1. BIOS/UEFI** | Hardware initialization, POST (Power On Self Test), find bootable device |
| **2. Bootloader (GRUB)** | Loads the kernel into memory, passes boot parameters |
| **3. Kernel** | Initializes hardware drivers, mounts root filesystem, starts first process |
| **4. init / systemd** | First user-space process (PID 1). Starts services based on targets/runlevels |
| **5. User Space** | Login prompt, desktop environment, services (nginx, docker, etc.) |

### systemd Targets (Runlevels)

| Target | Old Runlevel | Description |
|---|---|---|
| `poweroff.target` | 0 | Shut down |
| `rescue.target` | 1 | Single-user mode (recovery) |
| `multi-user.target` | 3 | Multi-user, no GUI |
| `graphical.target` | 5 | Multi-user with GUI |
| `reboot.target` | 6 | Reboot |

---

# Containers vs VMs

## Virtual Machines (VMs)

- Run a **full guest OS** on top of a hypervisor.
- Each VM has its own kernel, libraries, and binaries.
- **Heavy**: Gigabytes of disk, slow to start (minutes).

## Containers

- Share the **host OS kernel** — no guest OS needed.
- Only package the application + its dependencies.
- **Lightweight**: Megabytes of disk, start in seconds.

```
Virtual Machines:                    Containers:
┌──────┐ ┌──────┐ ┌──────┐         ┌──────┐ ┌──────┐ ┌──────┐
│ App1 │ │ App2 │ │ App3 │         │ App1 │ │ App2 │ │ App3 │
├──────┤ ├──────┤ ├──────┤         ├──────┤ ├──────┤ ├──────┤
│ Libs │ │ Libs │ │ Libs │         │ Libs │ │ Libs │ │ Libs │
├──────┤ ├──────┤ ├──────┤         └──────┴─┴──────┴─┴──────┘
│Guest │ │Guest │ │Guest │         ┌─────────────────────────┐
│  OS  │ │  OS  │ │  OS  │         │    Container Engine      │
├──────┴─┴──────┴─┴──────┤         │      (Docker)            │
│      Hypervisor         │         ├─────────────────────────┤
├─────────────────────────┤         │       Host OS            │
│       Host OS           │         ├─────────────────────────┤
├─────────────────────────┤         │      Hardware            │
│      Hardware           │         └─────────────────────────┘
└─────────────────────────┘
```

## Comparison

| Aspect | VM | Container |
|---|---|---|
| **Isolation** | Full (separate kernel) | Process-level (shared kernel) |
| **Startup Time** | Minutes | Seconds |
| **Size** | Gigabytes | Megabytes |
| **Performance** | Near-native (slight overhead) | Near-native (minimal overhead) |
| **Use Case** | Running different OSes, strong isolation | Microservices, CI/CD, dev environments |
| **Security** | Stronger (kernel isolation) | Weaker (shared kernel — escape possible) |
| **Examples** | VMware, VirtualBox, Hyper-V | Docker, Podman, containerd |

## Linux Features That Enable Containers

| Feature | Purpose |
|---|---|
| **Namespaces** | Isolate what a process can **see** (PID, network, mount, user) |
| **cgroups** | Limit what a process can **use** (CPU, memory, I/O) |
| **Union Filesystems** | Layer filesystem images (OverlayFS) for efficient image storage |
