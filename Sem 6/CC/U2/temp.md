# 📘 Cloud Computing: Comprehensive Study Notes

> **Course**: Cloud Computing — Virtualization, Containers & Docker  
> **Instructor**: Dr. Prafullata Kiran Auradkar  
> **Dept**: Computer Science and Engineering

---

# Table of Contents

1. [Popek & Goldberg Virtualization Principles](#1-popek--goldberg-virtualization-principles)
2. [Memory Virtualization](#2-memory-virtualization)
3. [I/O Virtualization](#3-io-virtualization)
4. [VM Migration](#4-vm-migration)
5. [Lightweight Virtualization – Containers](#5-lightweight-virtualization--containers)
6. [Docker](#6-docker)
7. [Linux Namespaces](#7-linux-namespaces)
8. [Control Groups (cgroups)](#8-control-groups-cgroups)
9. [Union Filesystem (UnionFS)](#9-union-filesystem-unionfs)

---

# 1. Popek & Goldberg Virtualization Principles

## 1.1 Background

Virtualization divides the resources of a physical computer into multiple **execution environments** using partial or complete machine simulation/emulation. It allows the resources to be shared between different tenants.

- The typical unit of compute division is a **Virtual Machine (VM)**.
- A software layer called the **Virtual Machine Monitor (VMM)** or **Hypervisor** provides the illusion of a "real" machine to multiple VM instances.
- The VMM uses two core techniques: **Trap and Emulate** and **Binary Translation**.

## 1.2 Key Terminologies (Popek & Goldberg, 1974)

| Term | Definition |
|---|---|
| **Virtual Machine (VM)** | A complete, isolated compute environment with its own processing, memory, and communication channels. It is an *efficient, isolated duplicate* of the physical machine. |
| **VMM/Hypervisor** | System software that creates and manages VMs. It must be: **Efficient** (safe instructions run directly on hardware), **Omnipotent** (only VMM manipulates sensitive state), and **Undetectable** (guest cannot tell it runs atop a VMM). |
| **Third Generation Machine** | A system with at least two operating modes (user and supervisor), where some instructions are unavailable in user mode, and addressing uses a relocation register. |

## 1.3 Essential Characteristics of a VMM

1. **Equivalence (Isolation/Protection)**: The VMM provides an environment essentially identical to the original machine. A program running on a VM should behave the same as one executing directly on hardware.

2. **Resource Control**: The VMM must be in *complete control* of virtualized resources. Programs must not access resources not explicitly allocated to them, and the VMM must be able to reclaim those resources.

3. **Efficiency**: The vast majority of machine instructions must execute *without VMM intervention* (i.e., without trapping). At worst, only minor speed decreases are acceptable.

## 1.4 Instruction Classification

| Type | Description | Examples (x86) |
|---|---|---|
| **Privileged Instructions** | Cause a **trap** if the processor is NOT in privileged (Ring 0) mode. | `HLT`, `LGDT`, `LIDT` |
| **Sensitive Instructions** | Access or modify low-level machine state (page tables, I/O devices, privilege bits). Split into two sub-types below. | — |
| ↳ *Behavior Sensitive* | Behavior/result depends on the mode or configuration of the hardware. If run at lower privilege, the result will be **wrong**. | `POP`, `PUSH`, `CALL`, `JMP`, `INT n`, `RET`, `LAR`, `LSL`, `VERR`, `VERW`, `MOV` |
| ↳ *Control Sensitive* | Attempt to **change** the configuration of system resources (modify system registers). | `PUSHF`, `POPF`, `SGDT`, `SIDT`, `SLDT`, `SMSW` |
| **Safe (Innocuous) Instructions** | Not sensitive — no impact on system state. Can run freely in any mode. | Arithmetic, data movement, etc. |

## 1.5 The Three Theorems

### Theorem 1 (The Fundamental Theorem)
> *"For any conventional third generation computer, a VMM may be constructed if the **set of sensitive instructions is a subset of the set of privileged instructions**."*

- If every sensitive instruction causes a trap when run outside Ring 0, the VMM can intercept and emulate it.
- Non-privileged (safe) instructions run natively for efficiency.

### Theorem 2 (Recursive Virtualization)
> *"A conventional third generation computer is **recursively virtualizable** if it is (a) virtualizable, and (b) a VMM without timing dependencies can be constructed for it."*

- Recursive virtualization = running a VM inside a VM.
- If an architecture *cannot* be classically virtualized, **binary translation** can replace the offending sensitive instructions.

### Theorem 3 (Hybrid VMM)
> *"A **hybrid VMM** may be constructed if the set of **user-sensitive** instructions is a subset of privileged instructions."*

- Relaxes Theorem 1 slightly: only user-mode sensitive instructions must be privileged.

## 1.6 The x86 Problem

The original **x86 architecture violated Theorem 1**. It had **17 sensitive, unprivileged instructions**:

```
SGDT, SIDT, SLDT, SMSW, PUSHF, POPF, LAR, LSL, VERR, VERW, POP, PUSH, CALL, JMP, INT, RET, STR, MOV
```

### Example: `PUSH %cs`
- `%cs` register contains the **Current Privilege Level (CPL)** (2 bits).
- A guest OS running in Ring 3 could `PUSH %cs` and see that the privilege level **is NOT Ring 0** — thus detecting it is running in a VM.
- To be virtualizable, `PUSH` should have trapped so the VMM could push a **faked** `%cs` value.

### Example: `PUSHF / POPF`
- `PUSHF`/`POPF` read/write the `%eflags` register.
- Bit 9 enables interrupts. In Ring 0, `POPF` can set bit 9, but in **Ring 3, the CPU silently ignores it** — no trap occurs.

### Resolution
- **Binary Translation** (used by VMware): rewrite sensitive instructions before execution.
- **Hardware extensions**: Intel **VT-x** and AMD **AMD-V** introduced a new execution mode for guests, making all sensitive instructions trap properly.

---

## 📝 Q&A: Popek & Goldberg Principles

**Q1: What are the three essential characteristics a VMM must satisfy?**  
**A:** Equivalence (programs behave identically on VM and real hardware), Resource Control (VMM has total control over all resources), and Efficiency (most instructions execute without VMM intervention).

**Q2: State Popek & Goldberg's Theorem 1 and explain its significance.**  
**A:** A VMM can be constructed for any third-generation computer if the set of sensitive instructions is a subset of privileged instructions. This means every instruction that could affect VMM correctness must cause a trap when run in user mode, allowing the VMM to intercept and emulate it.

**Q3: Why did the original x86 architecture fail to meet the Popek-Goldberg requirements?**  
**A:** x86 had 17 sensitive instructions that were NOT privileged — they executed silently in user mode without trapping. For example, `PUSHF` would silently ignore interrupt flag changes in Ring 3 instead of trapping, and `PUSH %cs` would reveal the true privilege level.

**Q4: What is the difference between behavior-sensitive and control-sensitive instructions?**  
**A:** Behavior-sensitive instructions produce different results depending on the hardware mode (e.g., revealing the privilege level). Control-sensitive instructions attempt to change system configuration (e.g., modifying control registers).

**Q5: How was the x86 virtualization problem solved?**  
**A:** Two approaches: (1) **Binary Translation** — VMware dynamically rewrites sensitive instructions before execution. (2) **Hardware extensions** (Intel VT-x, AMD-V) — introduced a new Ring -1 for the hypervisor, making all sensitive instructions trap properly.

**Q6: What is recursive virtualization, and what does Theorem 2 require for it?**  
**A:** Recursive virtualization is the ability to run a VM inside another VM. Theorem 2 states it requires (a) the architecture to be virtualizable, and (b) the VMM to have no timing dependencies.

**Q7: What are x86 protection rings, and how do they relate to virtualization?**  
**A:** x86 defines 4 privilege rings (0–3). Ring 0 is most privileged (kernel), Ring 3 is least (user apps). In virtualization, the OS runs in Ring 3 (or Ring 1), while the VMM runs in Ring 0. The problem is some sensitive instructions don't trap when running outside Ring 0, violating Theorem 1.

---

# 2. Memory Virtualization

## 2.1 Background: Address Translation in Unvirtualized Systems

In a normal (non-virtualized) system:
- Each process has its own **Virtual Address Space**.
- The OS maintains a **Page Table** that maps **Virtual Page Numbers (VPN)** → **Physical Page Frame Numbers (PFN)**.
- The **CR3 register** points to the root of the current process's page table.
- A **Page Table Walker (PTW)** traverses the page table to resolve virtual addresses.
- Recent translations are cached in the **Translation Lookaside Buffer (TLB)** for speed.

### Page Walking
| Type | Description | Example |
|---|---|---|
| **Software Page Walker** | OS handler that traverses the page table. Slower due to address translation overhead. | SPARC (Sun/Oracle) |
| **Hardware Page Walker** | Hardware generates load-like "instructions" to directly access page table entries. | x86, ARM |

## 2.2 Requirements for Memory Virtualization

In a virtualized environment, three key requirements must be met:

1. **VMs/Guest OSes must NOT have direct access to physical memory.**
2. **Only the Hypervisor/VMM should manage host physical memory.**
3. **The Guest OS should *believe* it is accessing physical memory** (it is "fooled").

This creates **two levels of address translation**:
- **Guest Virtual Address (gVA)** → **Guest Physical Address (gPA)** — managed by the Guest OS.
- **Guest Physical Address (gPA)** → **System/Host Physical Address (sPA/hPA)** — managed by the Hypervisor.

## 2.3 Shadow Page Tables (Software-Based)

### Concept
The hypervisor creates a **Shadow Page Table (sPT)** that directly maps `gVA → sPA`, bypassing the two-step translation.

### How It Works
1. The Guest OS maintains its own page table: `gVA → gPA`.
2. The Hypervisor maintains a system page table: `gPA → sPA`.
3. The Hypervisor **combines** these two into a **Shadow Page Table**: `gVA → sPA`.
4. The Hypervisor modifies the **CR3 register** to point to the shadow page table instead of the guest's page table.
5. The hardware PTW walks the shadow page table directly.

### Synchronization Mechanism
- The Guest OS's page tables are marked as **read-only**.
- Any modification to the guest page table → **page fault** → **trap to VMM**.
- The VMM then updates the corresponding shadow page table entry.
- The shadow page table is **not visible** to the guest.

### Challenges of Shadow Page Tables

| Challenge | Impact |
|---|---|
| **Frequent page faults** | Every guest page table modification triggers a trap. Memory-intensive apps suffer. |
| **Per-application overhead** | Each guest process needs its own shadow page table. |
| **Context switch cost** | Every guest context switch → trap to VMM to update CR3 to point to the correct shadow PT. |
| **TLB flush** | Every "world switch" (VM entry/exit) requires flushing the TLB → performance loss. |
| **Memory overhead** | Shadow copies of all guest page tables consume additional memory. |

## 2.4 Nested/Extended Page Tables (Hardware-Assisted)

### Concept
Hardware-assisted memory virtualization introduces an **additional page table** managed by the VMM, avoiding the software overhead of shadow paging entirely.

### How It Works
1. **Guest Page Table (gPT)**: Managed by the Guest OS. Maps `gVA → gPA`. The Guest OS has **full control** (uses its own CR3, called `gCR3`).
2. **Nested Page Table (nPT) / Extended Page Table (EPT)**: Managed by the Hypervisor. Maps `gPA → sPA`. The Hypervisor uses a second CR3 register called `nCR3` (or **EPT Base Pointer, EPTP**).
3. On a TLB miss, the hardware page walker performs a **Two-Dimensional Page Walk**: it walks both the gPT and the nPT simultaneously to resolve `gVA → sPA`.

### Key Advantages Over Shadow Paging

| Feature | Shadow Paging | Nested/Extended Page Tables |
|---|---|---|
| **Page table control** | VMM must intercept ALL guest PT modifications | Guest has full control over its own PTs |
| **VM exits** | Frequent (on every PT modification, CR3 change, page fault) | Rare (no exits for guest page faults, INVLPG, or CR3 changes) |
| **Complexity** | High software complexity | Hardware handles translation |
| **Performance** | Degrades with memory-intensive workloads | Consistently better for most workloads |

### Intel EPT Details
- **EPTP (EPT Base Pointer)**: A new **VMCS** (Virtual Machine Control Structure) field that points to the EPT page tables.
- EPT is **activated on VM entry** and **deactivated on VM exit**.
- Guest has **full control** over its IA-32 page tables — no VM exits for `INVLPG` or CR3 changes.

---

## 📝 Q&A: Memory Virtualization

**Q1: Why is memory virtualization needed in cloud computing?**  
**A:** Guest OSes must not have direct access to physical memory. The VMM needs to control physical memory allocation while making each Guest OS believe it has its own physical memory. This enables isolation between VMs and efficient memory sharing.

**Q2: Explain the two levels of address translation in a virtualized environment.**  
**A:** Level 1: Guest Virtual Address (gVA) → Guest Physical Address (gPA), managed by the Guest OS's page table. Level 2: Guest Physical Address (gPA) → System Physical Address (sPA), managed by the Hypervisor's page table. Both must be resolved for every memory access.

**Q3: What is a Shadow Page Table and how does it work?**  
**A:** A shadow page table is a combined page table created by the VMM that directly maps gVA → sPA. The VMM combines the guest's page table (gVA→gPA) with the system page table (gPA→sPA). The CR3 register is set to point to the shadow PT so the hardware walker can use it directly.

**Q4: Why are shadow page tables considered expensive?**  
**A:** Because (1) every guest PT modification triggers a page fault and VMM trap, (2) each guest process needs its own shadow PT, (3) context switches require VMM intervention to update CR3, (4) TLB flushes on world switches cause performance loss, and (5) maintaining shadow copies consumes additional memory.

**Q5: How do Extended Page Tables (EPT) solve the problems of shadow page tables?**  
**A:** EPT uses hardware support to manage a second-level page table (gPA→sPA). The guest has full control over its own page tables, so the VMM doesn't need to intercept page table modifications. The hardware walker performs a two-dimensional walk on TLB misses, eliminating most VM exits related to memory management.

**Q6: What is the role of the CR3 register in virtualization?**  
**A:** In non-virtualized systems, CR3 points to the page table root. In shadow paging, the VMM modifies CR3 to point to the shadow page table. In EPT, there are effectively two CR3s: gCR3 (pointing to the guest page table) and nCR3/EPTP (pointing to the extended page table).

**Q7: What is a Two-Dimensional Page Walk?**  
**A:** When EPT is enabled and a TLB miss occurs, the hardware page walker traverses BOTH the guest page table (gPT) and the nested page table (nPT) simultaneously. Each level of the guest walk requires a full nPT walk to translate the gPA encountered to an sPA. This is more memory accesses per walk but eliminates all VM exit overhead.

**Q8: Compare Shadow Paging vs EPT in a table.**  
**A:**

| Aspect | Shadow Paging | EPT/NPT |
|---|---|---|
| Maintained by | VMM software | Hardware |
| Guest PT modifications | Trapped by VMM | No trap needed |
| VM exits | Very frequent | Rare |
| TLB flush | On every world switch | Less frequent |
| Walk cost per TLB miss | Single walk of shadow PT | Two-dimensional walk (more accesses) |
| Overall performance | Worse for memory-intensive workloads | Better overall |

---

# 3. I/O Virtualization

## 3.1 What is I/O Virtualization?

**I/O Virtualization (IOV)** uses software to abstract upper-layer protocols from physical connections/transports. It involves managing the routing of I/O requests between **virtual devices** and the **shared physical hardware**.

## 3.2 Three Methods of I/O Virtualization

### 3.2.1 Full Device Emulation

- The VMM fully **emulates** well-known, real-world devices in software.
- All device functions (enumeration, identification, interrupts, DMA) are **replicated in software** within the VMM.
- The VMM acts as a **virtual device**; guest OS I/O requests are **trapped** in the VMM.
- The VMM then interacts with the real I/O devices on behalf of the guest.

**Pros**: Maximum compatibility (guest uses standard drivers).  
**Cons**: High overhead due to full software emulation.

### 3.2.2 Para-I/O-Virtualization (Split Driver Model)

Also known as the **Hosted** or **Split Driver** model:

- A **frontend driver** runs in **Domain U** (guest/user VM).
- A **backend driver** runs in **Domain 0** (privileged management VM).
- They communicate via a **shared memory block**.
- The frontend manages guest I/O requests; the backend manages real I/O devices and multiplexes I/O for different VMs.

**Pros**: Better performance than full emulation.  
**Cons**: Higher CPU overhead; requires modified guest OS drivers. Typically used in **Xen**.

### 3.2.3 Direct I/O (Passthrough)

- The VM accesses physical devices **directly**, bypassing the VMM's I/O emulation layer.
- Achieves **close-to-native** performance with minimal CPU cost.
- Enables direct **DMA** between the device and host memory.
- Uses Intel **VT-d** extensions to assign specific I/O devices to specific VMs.

**Pros**: Best performance (near-native).  
**Cons**: **Limited scalability** — a physical device can only be assigned to ONE VM at a time.

## 3.3 Comparison Table

| Feature | Full Emulation | Para-Virtualization | Direct I/O |
|---|---|---|---|
| **Performance** | Low | Medium | High (near-native) |
| **CPU Overhead** | High | Medium-High | Low |
| **Guest OS Modification** | None needed | Required (frontend driver) | None needed |
| **Scalability** | Good (shared) | Good (shared) | Poor (1 device = 1 VM) |
| **Example** | QEMU | Xen Split Drivers | Intel VT-d passthrough |

## 3.4 Advantages of I/O Virtualization

- **Flexibility**: Hardware independence, faster provisioning.
- **Cost Minimization**: Fewer cables, cards, and switch ports.
- **Increased Density**: More connections in a given space.
- **Minimizing Cables**: Reduced cabling between servers, storage, and network.

## 3.5 Xen I/O Virtualization
- Xen does **not** emulate hardware devices → exposes **device abstractions** for simplicity and performance.
- I/O data is transferred via **shared-memory buffers** between Xen and the guest.
- Uses **light-weight event delivery** for virtualized interrupts (bitmaps in shared memory + optional callback handlers).

---

## 📝 Q&A: I/O Virtualization

**Q1: What are the three methods of I/O virtualization?**  
**A:** (1) Full Device Emulation — VMM fully emulates devices in software. (2) Para-I/O-Virtualization (Split Driver) — uses frontend/backend drivers communicating via shared memory. (3) Direct I/O (Passthrough) — VM accesses physical devices directly.

**Q2: Why is Direct I/O considered the highest-performing approach?**  
**A:** It bypasses the VMM's I/O emulation layer entirely, allowing VMs to DMA directly to/from host memory, achieving near-native I/O performance with minimal CPU overhead.

**Q3: What is the main limitation of Direct I/O?**  
**A:** Limited scalability — a physical device can only be assigned to one VM at a time. You cannot share a single passthrough device between multiple VMs.

**Q4: How does Xen implement I/O virtualization?**  
**A:** Xen uses the para-virtualization (split driver) model. It does not emulate hardware but exposes device abstractions. Data is transferred via shared-memory buffers, and interrupts are virtualized using a lightweight bitmap-based event delivery mechanism.

**Q5: What role does Intel VT-d play in I/O virtualization?**  
**A:** Intel VT-d provides hardware support for direct device assignment. It enables DMA remapping and interrupt remapping, allowing a physical I/O device to be securely assigned to a specific VM while maintaining memory isolation.

---

# 4. VM Migration

## 4.1 What is VM Migration?

VM Migration is the process of **moving a virtual machine from one physical host to another** while preserving its state.

### Types of Migration

| Type | Description |
|---|---|
| **Cold Migration** | Migrating a powered-off VM. Can cross different CPU families. |
| **Non-Live (Offline) Migration** | VM is **paused** at the source, all state is transferred, then VM resumes at the destination. Longer downtime. |
| **Live (Hot) Migration** | Migrates a **powered-on** VM with **no service disruption**. Memory, BIOS, devices, MAC addresses are all transferred. No dropped connections; users are unaware. Example: VMware vSphere vMotion. |

## 4.2 Why Migrate VMs?

| Reason | Details |
|---|---|
| **Hardware Maintenance** | Move VMs off a physical server for maintenance |
| **Load Balancing** | Distribute unbalanced workloads across cluster nodes |
| **Scaling** | Move VMs to servers with more resources when workload grows |
| **Server Consolidation** | Optimal utilization of servers for cost efficiency |
| **Workload Mobility** | Compliance requirements may require moving VMs to different locations |
| **Performance** | Migrate to less-loaded nodes for better response times |
| **Energy Efficiency** | Reduce energy consumption by consolidating VMs |
| **Availability / Business Continuity** | Respond to equipment or environment failures |

## 4.3 Non-Live (Cold) Migration

The simplest migration technique:
1. VM is **suspended** (service is paused).
2. All state is transferred to the destination host.
3. VM **resumes** at the destination.
- Memory pages are migrated only **once**.
- Migration time is short and **predictable**.
- **Drawback**: Application performance is degraded during the migration pause.

## 4.4 Live Migration — Pre-Copy Technique

Pre-copy is the most widely used live migration technique (used by **KVM, Xen, VMware**).

### The 6 Stages

| Stage | Description |
|---|---|
| **1. Select Destination Host** | Determine where the VM will migrate (manual or automatic — e.g., by load balancer). |
| **2. Reserve Resources** | Prepare the destination host (CPU, memory, network). |
| **3. Iterative Pre-Copying** | **Round 1**: Transfer ALL memory pages. **Round 2+**: Re-copy only the **dirty pages** (pages modified since the last round). Iterate until the dirty portion is small enough for a final fast copy. |
| **4. Stop & Transfer** | **Suspend** the VM. Copy the last dirty pages + CPU state + network state. This is the **downtime** period — should be minimized. |
| **5. Commitment** | Confirm migration is complete. |
| **6. Activate at Destination** | Resume the VM at the destination. Network connections are redirected. Original VM is removed from the source. |

### Advantages
- **Low VM downtime** (only for the final dirty page copy).
- Works well when memory is not heavily modified.

### Disadvantages
- **Repeated copying of dirty pages** can increase total migration time.
- Memory-write-intensive workloads → more dirty pages → longer migration.

## 4.5 Live Migration — Post-Copy Technique

### How It Works
1. **CPU/processor state** is transferred FIRST to the destination server.
2. The VM **immediately resumes** at the destination (before all memory is there).
3. Memory pages are copied in the background while the VM runs on the destination.
4. When the VM accesses a page that hasn't arrived yet → **page fault** → the page is fetched from the source.

### Sub-techniques for Page Transfer

| Technique | Description |
|---|---|
| **Demand Paging** | Pages are fetched from the source only when a page fault occurs. Simple but slow — each fault stalls the VM. |
| **Active Push** | Source proactively pushes pages to the destination, even while the VM runs. If a fault occurs, falls back to demand paging. Pages are sent only once. |
| **Memory Prepaging** | Predicts which pages the VM will need next based on access patterns. Pushes high-probability pages first, reducing faults. |

### Disadvantages
- **Page faults** at the destination cause VM stalls while waiting for data from the source.
- Network overhead from transferring memory pages during execution.
- Repetitive page fault handling can degrade application performance.

## 4.6 Migration Considerations

### Memory Migration
- Memory can be hundreds of MB to several GB.
- **Internet Suspend-Resume (ISR)** exploits **temporal locality** — memory states have considerable overlap between suspended and resumed instances.
- Uses tree-of-subfiles caching — only changed subfiles are transmitted.

### File System Migration
- Each VM needs a **consistent, location-independent** view of the file system.
- **Option 1**: Each VM has a virtual disk; its contents are transported with the VM state.
- **Option 2**: A **global file system** (e.g., NFS, distributed FS) accessible from all hosts — no file copying needed.

### Network Migration
- Each VM is assigned a **virtual IP** and **virtual MAC address**.
- The VMM maintains mappings of virtual IP/MAC to VMs.
- The migrating VM carries its IP address with it and maintains all open network connections.

---

## 📝 Q&A: VM Migration

**Q1: What are the three types of VM migration?**  
**A:** (1) Cold migration — VM is powered off. (2) Non-live/Offline migration — VM is paused, state transferred, VM resumed. (3) Live/Hot migration — VM stays powered on with no service disruption.

**Q2: List at least five reasons for VM migration.**  
**A:** Hardware maintenance, load balancing, scaling to changed workloads, server consolidation for cost, workload mobility for compliance, performance optimization, energy efficiency, and availability/business continuity.

**Q3: Explain the pre-copy live migration technique step by step.**  
**A:** (1) Select destination host. (2) Reserve resources. (3) Iteratively pre-copy memory pages — first copy all pages, then re-copy only dirty pages in each round until the dirty set is small. (4) Stop the VM and transfer the final dirty pages + CPU/network state (downtime). (5) Commit. (6) Activate at destination and redirect network.

**Q4: What is the key difference between pre-copy and post-copy migration?**  
**A:** Pre-copy transfers memory BEFORE stopping the VM (iterative dirty page copying). Post-copy transfers CPU state first, immediately resumes the VM at the destination, and transfers memory AFTER (on demand). Pre-copy has multiple memory transfers; post-copy has page faults.

**Q5: What is the "downtime" in live migration, and how is it minimized?**  
**A:** Downtime is the "service unavailable" period when the VM is stopped for the final state transfer. In pre-copy, it is minimized by iteratively reducing the dirty page set before the final stop. In post-copy, it is minimized because the VM resumes almost immediately.

**Q6: What are the three sub-techniques of post-copy migration?**  
**A:** (1) Demand Paging — fetch pages from source only on page fault (simple, slow). (2) Active Push — source proactively pushes pages while VM runs at destination. (3) Memory Prepaging — predict which pages will be needed and push them preemptively.

**Q7: How is the network maintained during live migration?**  
**A:** Each VM has a virtual IP and virtual MAC address. The VMM maintains mappings. During migration, the VM carries its IP address, and the network connection is redirected to the new host. All open connections are preserved.

**Q8: What is the ISR technique for memory migration?**  
**A:** Internet Suspend-Resume (ISR) exploits temporal locality — the memory state at suspension and resumption is largely the same. It uses a tree of small subfiles cached at both ends, sending only the changed ones, thus reducing transfer size.

---

# 5. Lightweight Virtualization – Containers

## 5.1 What are Containers?

**Linux Containers (LXC)** provide **OS-level virtualization** for running multiple isolated Linux systems on a single host kernel. They do NOT create full-fledged virtual machines.

- LXC provides a virtual environment with its own **process space** and **network space**.
- Uses **Linux kernel cgroups** and **namespace isolation** to achieve this.

## 5.2 Motivation

| Challenge with VMs | How Containers Address It |
|---|---|
| VM isolation is **expensive** (each VM has full OS) | Containers share the host OS kernel — very lightweight |
| VMs are **large** (GBs) and **slow to start** (minutes) | Containers are **small** (MBs) and start in **seconds** |
| Each VM requires its own **OS maintenance** | Single OS to maintain (patches, bug fixes, etc.) |
| VMs consume **more resources** | Containers consume **fewer resources** |

## 5.3 Container Characteristics

- Sit on top of a physical server and its host OS.
- Share the host **OS kernel** and usually binaries/libraries (read-only).
- Exceptionally **lightweight**: megabytes in size, seconds to start.
- Container creation is similar to **process creation** → speed, agility, portability.
- **Higher provisioning performance** than VMs.
- Programs running inside a container can access **only their own memory and resources** as scoped by the container.

## 5.4 Containers vs VMs — Detailed Comparison

| Aspect | Virtual Machines | Containers |
|---|---|---|
| **OS** | Each VM has its own OS image | Share host OS kernel |
| **Size** | GBs | MBs |
| **Startup** | Minutes (full boot) | Seconds (process creation) |
| **Overhead** | High (separate OS, drivers, etc.) | Low (shared kernel) |
| **Isolation** | Full hardware-level isolation | Process-level isolation (namespaces/cgroups) |
| **Flexibility** | Can run different OS types | Must match host OS type |
| **Resource Usage** | High | Low |
| **Management** | Each OS needs separate maintenance | Single OS to maintain |

---

## 📝 Q&A: Containers

**Q1: What is a Linux Container (LXC)?**  
**A:** LXC is an OS-level virtualization method that runs multiple isolated Linux systems (containers) on a single host OS instance. It uses Linux kernel cgroups and namespace isolation to create virtual environments without full virtual machines.

**Q2: How are containers fundamentally different from VMs?**  
**A:** VMs virtualize hardware and run a full OS per instance. Containers virtualize at the OS level, sharing the host kernel. This makes containers much smaller, faster to start, and more resource-efficient, but less flexible (must match host OS type).

**Q3: What kernel features enable container isolation?**  
**A:** Two primary Linux kernel features: (1) **Namespaces** — isolate resources (PID, network, mount, etc.) so containers have their own view. (2) **Cgroups** — limit, prioritize, and account resource usage (CPU, memory, I/O) for container processes.

**Q4: Why can't a Linux container run a Windows application?**  
**A:** Containers share the host kernel. Since they don't have their own OS, a Linux container uses the Linux kernel and can only run Linux-compatible applications. A Windows app would require a Windows kernel.

**Q5: When would you choose VMs over containers?**  
**A:** When you need: (1) Full OS isolation (different OS types per instance), (2) Stronger security boundaries, (3) Legacy applications requiring specific OS versions, (4) Workloads that need hardware-level isolation.

---

# 6. Docker

## 6.1 What is Docker?

Docker is an **open platform tool** for creating, testing, shipping, deploying, and executing applications using containers. It uses **OS-level virtualization** to deliver software in packages called containers.

- Separates applications from infrastructure → faster software delivery.
- Provides isolation and security for many containers on a single server/VM.
- Typically **8–18 containers** run simultaneously on a single server/VM.
- Can be considered a **PaaS product**.

## 6.2 Docker Architecture (Client-Server)

```
┌─────────────┐       REST API        ┌─────────────────────────────┐
│  Docker CLI  │ ───────────────────── │     Docker Daemon (dockerd)  │
│  (Client)    │  (UNIX socket or TCP) │                              │
└─────────────┘                        │  ┌──────┐  ┌──────┐         │
                                       │  │Image │  │Image │  ...    │
                                       │  └──┬───┘  └──┬───┘         │
                                       │     │         │              │
                                       │  ┌──▼───┐  ┌──▼───┐         │
                                       │  │Cont. │  │Cont. │  ...    │
                                       │  └──────┘  └──────┘         │
                                       │                              │
                                       │     Docker Host              │
                                       └──────────────┬──────────────┘
                                                      │
                                              ┌───────▼───────┐
                                              │ Docker Registry│
                                              │ (Docker Hub)   │
                                              └───────────────┘
```

### Key Components

| Component | Description |
|---|---|
| **Docker Daemon (`dockerd`)** | Listens for Docker API requests. Manages Docker objects (images, containers, networks, volumes). |
| **Docker Client (`docker`)** | CLI tool users interact with. Sends commands to the daemon via the Docker API. |
| **Docker Host** | Machine running the Docker daemon. Hosts images and containers. |
| **Docker Registry** | Stores Docker images. **Docker Hub** is the default public registry. You can also run a **private registry**. |

## 6.3 Docker Objects

### Images
- **Read-only templates** with instructions for creating a container.
- Can be based on other images with additional customizations.
- Defined using a **Dockerfile** — a script that specifies the steps to build the image.
- Each Dockerfile instruction creates a **layer** in the image.
- Only **incremental changes** are rebuilt → lightweight.

### Layers
- A layer is a set of files and metadata packaged as an **atomic unit**.
- Layers are treated as intermediate images.
- Build upon parent layers by applying filesystem changes.
- Only the **changed layers** need to be rebuilt.

### Containers
- **Running instances** of images — execution environments (sandboxes).
- Can be created, started, stopped, moved, deleted via Docker API/CLI.
- Isolated from other containers and the host.
- Uses **namespaces** to restrict what processes can see.
- When removed, non-persistent state disappears.

### Docker Registries
- **Docker Hub**: Default public registry.
- `docker pull` / `docker run` → pulls images from configured registry.
- `docker push` → pushes images to configured registry.

## 6.4 Essential Docker Commands

```bash
# Pull an image
docker pull ubuntu:latest

# Run a container
docker run -d --name myapp ubuntu sleep 1000

# List running containers
docker ps

# Stop a container
docker stop myapp

# Remove a container
docker rm myapp

# Build an image from a Dockerfile
docker build -t myimage:latest .

# Push image to registry
docker push myimage:latest

# Remove an image
docker rmi myimage:latest
```

---

## 📝 Q&A: Docker

**Q1: What is Docker, and how does it differ from a VM?**  
**A:** Docker is a platform for running applications in containers using OS-level virtualization. Unlike VMs which virtualize hardware and run full OSes, Docker containers share the host kernel and are much lighter (MBs vs GBs, seconds vs minutes to start).

**Q2: Describe the Docker architecture.**  
**A:** Docker uses a client-server architecture. The Docker Client (CLI) communicates via REST API with the Docker Daemon (dockerd), which manages Docker objects (images, containers, networks, volumes). The Docker Host runs the daemon and connects to a Docker Registry (like Docker Hub) for image storage.

**Q3: What is a Dockerfile and what are layers?**  
**A:** A Dockerfile is a script that defines the steps to build a Docker image. Each instruction in a Dockerfile creates a **layer** — a set of files packaged as an atomic unit. Layers are stacked and cached, so rebuilding only reprocesses changed layers, making builds fast and images lightweight.

**Q4: What is the difference between a Docker Image and a Container?**  
**A:** An image is a read-only template defining the filesystem and configuration. A container is a running instance of an image — it adds a writable layer on top of the image's read-only layers. Multiple containers can share the same image.

**Q5: What is Docker Hub?**  
**A:** Docker Hub is the default public registry for Docker images. Anyone can push and pull images. You can also run a private registry for internal use.

---

# 7. Linux Namespaces

## 7.1 What are Namespaces?

Namespaces are a Linux kernel feature that **isolate** system resources for groups of processes. Each namespace provides processes with their own **isolated instance** of a global resource.

**Core idea**: *If you can't name an object, you can't access it.* Namespaces hide resources from processes that shouldn't see them.

## 7.2 Types of Namespaces Used by Docker

Docker uses **10 major system features** for containerization. The key namespaces include:

| Namespace | Flag | Isolates |
|---|---|---|
| **PID** | `CLONE_NEWPID` | Process IDs — container processes can't see processes outside their namespace. |
| **UTS** | `CLONE_NEWUTS` | Hostname and NIS domain name — each container can have its own FQDN. |
| **MNT** | `CLONE_NEWNS` | Mount points — isolates filesystem views so containers can't see each other's files. |
| **IPC** | `CLONE_NEWIPC` | Inter-process communication — isolates shared memory and message queues. |
| **NET** | `CLONE_NEWNET` | Network interfaces, IP addresses, routing tables, ports — full network isolation. |
| **USR** | `CLONE_NEWUSER` | User/group IDs — UID mapping isolates user identity. |

Additional features:
- **chroot** / **pivot_root**: Controls filesystem root location.
- **cgroups**: Resource accounting and limiting (see next section).
- **CAP drop**: Drops specific OS capabilities.
- **Security modules**: Mandatory access controls (e.g., AppArmor, SELinux).

## 7.3 MNT Namespace Illustration

```
Container 1                Container 2
┌─────────────────┐       ┌─────────────────┐
│  /usr (shared)  │       │  /usr (shared)  │
│  /mnt (private) │       │  /mnt2 (private)│
│  ✗ /mnt2        │       │  ✗ /mnt         │
└─────────────────┘       └─────────────────┘
```
- Both containers share `/usr` (read-only).
- Container 1 can access `/mnt` but NOT `/mnt2`.
- Container 2 can access `/mnt2` but NOT `/mnt`.

## 7.4 Namespace Operations

```c
// Creating a new namespace (child process with new MNT namespace)
pid = clone(childFunc, stackTop, CLONE_NEWNS | SIGCHLD, argv[1]);

// Joining an existing namespace
int setns(int fd, int nstype);

// Creating a new namespace for the calling process
int unshare(int flags);  // e.g., CLONE_NEWNS for mount namespace
```

- Check which namespace a process is in: `ls -l /proc/<pid>/ns/`

## 7.5 NET Namespace & VETH

- Each NET namespace can have its own network devices, bridges, routing tables, IP addresses, and ports.
- **VETH (Virtual Ethernet)** pairs act as a pipe between two namespaces:
  - Created in pairs.
  - Packets sent on one device are immediately received on the other.
  - Used to connect container namespaces to the host namespace or to each other.

## 7.6 chroot vs pivot_root

| Feature | chroot | pivot_root |
|---|---|---|
| Changes | Root directory for running process | Detaches old root, attaches new root |
| Security | Can be escaped with proper capabilities | More secure — old root is fully detached |
| Usage | Building system images | Production container isolation |

---

## 📝 Q&A: Linux Namespaces

**Q1: What are Linux namespaces and why are they important for containers?**  
**A:** Namespaces are a kernel feature that isolates system resources. Each namespace gives processes their own view of a global resource. They are essential for containers because they provide isolation of PIDs, networks, filesystems, users, and IPC without needing separate OS instances.

**Q2: List and briefly explain the six key namespace types.**  
**A:** (1) PID — isolates process IDs. (2) UTS — isolates hostname/domain. (3) MNT — isolates mount points/filesystem. (4) IPC — isolates shared memory/message queues. (5) NET — isolates network interfaces/IPs/ports. (6) USR — isolates user/group IDs.

**Q3: What is a VETH pair, and how is it used in container networking?**  
**A:** A VETH (Virtual Ethernet) pair is a virtual network cable connecting two namespaces. Devices are created in pairs — packets sent on one end are immediately received on the other. Docker uses VETH pairs to connect container network namespaces to the host's network namespace.

**Q4: What is the difference between chroot and pivot_root?**  
**A:** `chroot` changes the root directory for a process but can be escaped with proper capabilities. `pivot_root` fully detaches the old root and attaches the new root, making it more secure and preferred for production containers.

**Q5: How do you check which namespace a process belongs to?**  
**A:** Run `ls -l /proc/<pid>/ns/`. This shows symbolic links to namespace IDs for each namespace type (mnt, pid, net, ipc, uts, user, etc.).

---

# 8. Control Groups (cgroups)

## 8.1 What are cgroups?

**cgroups (Control Groups)** are a Linux kernel feature to **limit, police, and account** resource usage for groups of processes. Docker uses cgroups to enforce resource limits on containers.

## 8.2 cgroup Functionality

| Function | Description |
|---|---|
| **Access** | Control which devices can be used per cgroup |
| **Resource Limiting** | Limit memory, CPU, block I/O, device accessibility |
| **Prioritization** | Assign relative priority for CPU, memory, etc. |
| **Accounting** | Track resource usage per cgroup |
| **Control** | Freeze/checkpoint processes |
| **Injection** | Packet tagging for network traffic |

## 8.3 What Can Be Limited?

- **Memory** (RAM)
- **CPU** (time, cores)
- **Block I/O** (disk bandwidth)
- **Devices** (access control, device creation)
- **Network** (bandwidth)

## 8.4 cgroup Hierarchy

- cgroups are **hierarchically structured** — each resource type has its own hierarchy.
- Tasks (processes) are assigned to cgroups.
- **Child processes inherit** the parent's cgroup — maintaining resource limits.

## 8.5 Practical Docker cgroup Commands

```bash
# Limit a container to 50% of one CPU
docker run -d --name mycontainer --cpus="0.5" ubuntu sleep 1000

# Limit a container to 100MB RAM
docker run -d --name mycontainer --memory=100m ubuntu sleep 1000

# Check CPU quota
docker exec mycontainer cat /sys/fs/cgroup/cpu/cpu.cfs_quota_us

# Check memory limit
docker exec mycontainer cat /sys/fs/cgroup/memory/memory.limit_in_bytes

# Monitor resource stats
docker stats mycontainer

# Stress test (will be killed by OOM killer if exceeds limit)
docker run -d --name mem_test --memory=100m ubuntu sleep 1000
docker exec -it mem_test stress --vm 1 --vm-bytes 150m --timeout 30s
# Container exits with code 137 (OOM killed)
```

---

## 📝 Q&A: cgroups

**Q1: What are cgroups and what problem do they solve?**  
**A:** cgroups (Control Groups) are a Linux kernel feature that limits, monitors, and accounts resource usage for groups of processes. They solve the problem of ensuring fair resource allocation and preventing one container from monopolizing CPU, memory, or I/O on a shared system.

**Q2: What resources can cgroups limit?**  
**A:** Memory, CPU time, block I/O (disk bandwidth), device access, and network bandwidth.

**Q3: What happens when a Docker container exceeds its memory limit?**  
**A:** The Linux Out-of-Memory (OOM) killer terminates the container process. The container exits with code 137, indicating it was killed due to excessive memory usage.

**Q4: How do child processes behave with respect to cgroups?**  
**A:** Child processes inherit the parent's cgroup. This means all forked/spawned processes within a container share the same resource limits as the container's main process.

**Q5: How do namespaces and cgroups work together for containers?**  
**A:** Namespaces provide **resource isolation** (what a process can see). cgroups provide **resource limitation** (how much of a resource a process can use). Together, they form the foundation of container technology: namespaces isolate the view, cgroups limit the usage.

---

# 9. Union Filesystem (UnionFS)

## 9.1 What is UnionFS?

A **Union Filesystem** creates the illusion of merging contents of several directories (organized in layers) into a single, unified view — **without modifying the original sources**.

## 9.2 How It Works

```
Layer 3: Container R/W layer    ← Writable (changes go here)
Layer 2: App files (e.g., nginx) ← Read-only
Layer 1: Libraries               ← Read-only
Layer 0: Base OS (e.g., Ubuntu)  ← Read-only
─────────────────────────────────
         Merged View → /
```

- Operates by creating **layers**, making it lightweight and fast.
- Docker Engine uses UnionFS to provide building blocks for containers.
- Variants: **AUFS, overlay2, btrfs, vfs, DeviceMapper**.

## 9.3 Layering Example

```bash
# /Fruits contains: Apple, Tomato
# /Vegetables contains: Carrots, Tomato

mount -t unionfs -o dirs=/Fruits:/Vegetables none /mnt/healthy

# /mnt/healthy now contains: Apple, Tomato, Carrots
# Tomato comes from /Fruits (first in dirs option — higher layer wins)
```

## 9.4 Copy-on-Write (CoW)

When the `-o cow` option is used:
- If a change is made to a file, the **original is NOT modified**.
- A **new file is created in a hidden location** (the writable layer).
- If a layer is mounted read-only, changes are recorded in a **temporary layer**.

### In Docker
- All **image layers are read-only**.
- Each container adds a **writable layer on top**.
- Multiple containers can share the same image layers with their own writable layers.
- **Only changed data** is stored in the writable layer.

## 9.5 Weaknesses of UnionFS

| Weakness | Details |
|---|---|
| **Filesystem rule conflicts** | Different filesystems have different rules for attributes, sizes, names. Translation can be imperfect. |
| **mmap difficulties** | Copy-on-write makes memory-mapped files (`mmap` syscall) hard to implement. |
| **Not for long-lived data** | UnionFS is not appropriate for persistent data or data sharing between containers. Use **Docker Volumes** instead. |

---

## 📝 Q&A: Union Filesystem

**Q1: What is a Union Filesystem and why does Docker use it?**  
**A:** A UnionFS creates a single merged view from multiple directory layers without modifying originals. Docker uses it to stack read-only image layers with a writable container layer, enabling efficient image reuse and fast container creation.

**Q2: Explain Copy-on-Write (CoW) in the context of Docker.**  
**A:** CoW means the original read-only image layer files are never modified. When a container process modifies a file, a copy is made in the container's writable layer, and modifications are applied there. This allows multiple containers to share the same base image efficiently.

**Q3: What are Docker layers?**  
**A:** Each instruction in a Dockerfile creates a layer — a read-only set of files. Layers are stacked to form an image. The bottom layers are the base OS, middle layers add libraries/applications, and the top writable layer is the container. Only incremental changes between layers are stored.

**Q4: Why is UnionFS not suitable for persistent data?**  
**A:** UnionFS layers are temporary — the writable container layer is deleted when the container is removed. For persistent data, Docker Volumes should be used, which exist outside the UnionFS and persist across container lifecycles.

**Q5: What happens when two layers have a file with the same name?**  
**A:** The file from the **higher layer** (specified first in the mount order) takes precedence. The lower layer's version is effectively hidden in the merged view.

---

# 🎯 Summary Table

| Topic | Key Concept | Software-Based | Hardware-Based |
|---|---|---|---|
| **Popek-Goldberg** | All sensitive instructions must be privileged | Binary Translation | VT-x, AMD-V |
| **Memory Virtualization** | Two-level address translation (gVA→gPA→sPA) | Shadow Page Tables | EPT/NPT |
| **I/O Virtualization** | Abstract physical devices for VMs | Full Emulation, Para-Virtualization | Direct I/O (VT-d) |
| **VM Migration** | Move VMs between hosts | Pre-copy, Post-copy | — |
| **Containers** | OS-level virtualization, lightweight isolation | Docker, LXC | — |
| **Isolation** | Control what processes can see | Namespaces | — |
| **Resource Limits** | Control how much resources processes use | cgroups | — |
| **Container FS** | Layered, merged filesystem with CoW | UnionFS (overlay2) | — |
