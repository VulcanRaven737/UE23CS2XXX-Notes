# Cloud Computing — Unit 2: Complete Study Notes

### Course: Cloud Computing | Department of Computer Science and Engineering

---

> **How to use these notes:** Every major topic ends with an exhaustive Q&A section. Read the theory first, then test yourself with the Q&A. Cover the answers and try to recall — that is the fastest path to acing the exam.

---

# Table of Contents

1. What is Virtualization?
2. Compute Virtualization and its Relationship to Cloud Computing
3. Types of Hypervisors (Type 1 and Type 2)
4. Physical Machine Context — Privilege Rings
5. Paravirtualization and Full (Transparent) Virtualization
6. Virtualization Software Techniques — Trap & Emulate and Binary Translation
7. Hardware Virtualization (Intel VT-x / AMD-v)
8. Memory Virtualization — Shadow Page Tables and Extended Page Tables (EPT)
9. I/O Virtualization
10. Popek and Goldberg Principles
11. VM Migration (Cold, Pre-copy, Post-copy)
12. Lightweight Virtualization — Containers, Namespaces, cgroups
13. Docker — Architecture, Images, Containers, Registries, UnionFS
14. DevOps — Principles, Pipeline, CI/CD
15. Container Orchestration and Kubernetes

---

---

# 1. What is Virtualization?

Virtualization is a **technique of abstracting physical resources** — compute (CPU), memory, storage, and networking — into a logical view. Each application believes it has exclusive access to its own logical resource, independent of the underlying physical reality.

## Key Benefits

- **Increased utilization and capability** of IT infrastructure — multiple workloads share the same physical hardware.
- **Simplified resource management** through pooling and sharing of resources.
- **Reduced downtime** — both planned (maintenance) and unplanned (failures).
- **Improved performance** of IT infrastructure overall.

## Four Pillars of Virtualization

|Virtual Resource|What the Application Sees|
|---|---|
|Virtual Memory|Its own logical memory, independent of physical memory|
|Virtual Network|Its own logical network, independent of physical network|
|Virtual Server|Its own logical server, independent of physical servers|
|Virtual Storage|Its own logical storage, independent of physical storage|

---

## Q&A — Virtualization Basics

**Q1: What is virtualization in the context of cloud computing?** A: Virtualization is a technique that abstracts physical hardware resources (compute, memory, storage, networking) into a logical view, creating isolated virtual environments that appear as real machines to applications. It is the foundational technology enabling cloud computing because it allows a single physical server to host multiple isolated virtual machines, each serving different users or workloads.

**Q2: What are the four types of physical resources that can be virtualized?** A: Compute/CPU (virtual servers), Memory (virtual memory), Storage (virtual storage), and Networking (virtual networks).

**Q3: How does virtualization reduce downtime?** A: Virtualization enables live migration — moving running VMs from one physical host to another without interruption — so hardware can be maintained without shutting down services. It also provides rapid VM restart on a healthy host if a physical machine fails, reducing unplanned downtime.

**Q4: What is the difference between virtual memory and virtual storage?** A: Virtual memory is an abstraction of RAM — it gives each process/VM the illusion of having its own private memory address space. Virtual storage is an abstraction of disk/block storage — it gives each VM the illusion of having its own dedicated disk, independent of how physical disks are partitioned or shared.

---

---

# 2. Compute Virtualization and Relationship to Cloud Computing

## What is Compute Virtualization?

Compute virtualization (also called Server Virtualization) is a **framework or methodology of dividing the resources of a computer/server into multiple execution environments**. It is achieved using:

- Hardware and software partitioning
- Time-sharing
- Partial or complete machine simulation
- Emulation
- Quality of Service (QoS)
- Other techniques

The key idea is to present and partition computing resources in a **logical way rather than according to physical reality**.

## The Virtual Machine (VM)

- A **VM is the coarse-granular view of Virtual Compute** at the next abstract level.
- A VM is **logically identical to a physical machine** with the ability to run an OS.
- VMs enable sharing of the same physical resources across different users.
- Virtualization is a **key technology for Cloud Computing** because VMs are the primary way compute resources are provisioned in a cloud environment.

## Before vs. After Server Virtualization

### Before Server Virtualization (Bare Metal)

- Single operating system image per machine.
- Software and hardware are tightly coupled.
- OS handles bare hardware (CPU, memory, I/O) directly.
- Running multiple applications on the same machine can create conflicts.
- Resources are underutilized.

### After Server Virtualization

- Virtual Machines break the dependency between OS and hardware.
- OS and application are managed as a single unit, encapsulated into VMs.
- Strong fault and security isolation between VMs.
- Hardware-independent — VMs can be moved across different physical servers.

## Why Use VMs? (Key Reasons)

|Reason|Explanation|
|---|---|
|Server consolidation|Run multiple workloads in isolation on the same physical server, increasing hardware utilization|
|Workload mobility|Move applications from one server to another|
|Development and test|Easily provision virtual resources for testing|
|Business continuity|Live-migrate VMs to other physical servers for DR|
|Management|Consolidate and simplify management|
|OS diversity|Run both Linux and Windows on same hardware|
|Security/Isolation|Hypervisor separates and isolates VMs from hardware and each other|
|High availability / Load balancing|Migrate VMs to balance loads|
|Rapid provisioning|On-demand provisioning of hardware resources|
|Encapsulation|Application execution environment is self-contained in the VM|
|Lower costs|Increased efficiencies, flexibility, and responsiveness|

---

## Q&A — Compute Virtualization

**Q1: What is compute virtualization and how does it relate to cloud computing?** A: Compute virtualization divides a single physical server's resources into multiple isolated execution environments (VMs). Cloud computing relies on this because it allows providers to multiplex many tenants on shared hardware, offer on-demand self-service, elasticity, and rapid provisioning — all fundamental cloud properties.

**Q2: What are the main technologies used to achieve compute virtualization?** A: Hardware and software partitioning, time-sharing, partial/complete machine simulation, emulation, and Quality of Service mechanisms.

**Q3: Why is encapsulation important in virtualization?** A: Encapsulation bundles the entire execution environment of an application (OS + app + configuration) into a VM image. This makes VMs portable — they can be moved, copied, or restored as a single unit regardless of the underlying physical hardware.

**Q4: How does virtualization support OS diversity?** A: Since the hypervisor abstracts hardware, different VMs can run completely different operating systems (e.g., Linux and Windows) on the same physical server simultaneously.

---

---

# 3. Types of Hypervisors (VMM)

The layer of software that provides the illusion of a "real" machine to multiple VM instances is called the **Virtual Machine Monitor (VMM)** or **Hypervisor**.

## Three Requirements for a VMM (Popek & Goldberg Foundation)

1. VMM should provide an environment for programs essentially **identical to the original machine**.
2. Programs running in this environment should show, at worst, only **minor reduction in performance**.
3. VMM should be in **complete control of the system resources**.

## Type 1 Hypervisor — Bare Metal Hypervisor

```
+-------------+  +-------------+  +-------------+
|  Guest VM   |  |  Guest VM   |  |  Guest VM   |
+-------------+  +-------------+  +-------------+
+---------------------------------------------------+
|                     VMM (Type 1)                   |
+---------------------------------------------------+
|                     Hardware                       |
+---------------------------------------------------+
```

- **Definition:** The VMM is inserted **directly between real hardware and the Guest OSes** — it runs directly on the hardware with no host OS beneath it.
- Also called: **Bare Metal VMM** or **Native Hypervisor**.
- It **directly interacts with the hardware** and manages hardware resources.
- Each time a guest program accesses hardware, the VMM captures and mediates the process.
- **Examples:** Xen, VMware ESX Server, IBM CP/CMS, Windows Virtualization (2008).

## Type 2 Hypervisor — Hosted Hypervisor

```
+-------------+  +-------------+  +-------------+
|  Guest VM   |  |  Guest VM   |  |  Guest VM   |
+-------------+  +-------------+  +-------------+
+---------------------------------------------------+
|                     VMM (Type 2)                   |
+---------------------------------------------------+
|                     Host OS                        |
+---------------------------------------------------+
|                     Hardware                       |
+---------------------------------------------------+
```

- **Definition:** The VMM is implemented **on top of a host OS** as an application.
- Also called: **Hosted VMM** or **Hosted Hypervisor**.
- The VMM maintains a **software-level representation of physical hardware** and interposes on operations, redirecting them per policies.
- **Examples:** VMware Workstation, Oracle VirtualBox.
- **Special case:** KVM (Kernel-Based Virtual Machine) for Linux is implemented **as part of the OS itself** — the Linux kernel is extended to become a hypervisor.

## Hybrid Hypervisor

```
+------+  +---------+  +---------+
| Host |  |  Guest  |  |  Guest  |
|  VM  |  |   VM    |  |   VM    |
+------+  +---------+  +---------+
+----------+  +---------------------+
|  VMM     |  |       VMM           |
+----------+  +---------------------+
              |       Host OS        |
              +---------------------+
+-------------------------------------------+
|                  Hardware                  |
+-------------------------------------------+
```

- Supports **both bare-metal and hosted** operation simultaneously.
- **Examples:** MS Virtual Server, MS Virtual PC.

## Type 1 vs. Type 2 Comparison

|Feature|Type 1 (Bare Metal)|Type 2 (Hosted)|
|---|---|---|
|Runs on|Directly on hardware|On top of Host OS|
|Performance|Higher (closer to metal)|Lower (extra OS layer)|
|Security|Better isolation|Depends on Host OS security|
|Setup|Complex|Simpler|
|Use case|Data centers, production|Development, testing, desktop|
|Examples|VMware ESXi, Xen, Hyper-V|VMware Workstation, VirtualBox, KVM|

---

## Q&A — Hypervisors

**Q1: What is a hypervisor and what are its three core requirements?** A: A hypervisor (VMM) is a software layer that creates and manages virtual machines. The three requirements are: (1) Equivalence — VMs must behave identically to bare-metal hardware; (2) Efficiency — most instructions must run natively without VMM intervention; (3) Resource Control — only the VMM can manage and control hardware resources.

**Q2: What is the fundamental difference between Type 1 and Type 2 hypervisors?** A: A Type 1 hypervisor runs directly on hardware (bare metal) with no host OS. A Type 2 hypervisor runs as an application on top of a host OS. Type 1 has better performance and is used in production; Type 2 is easier to install and is used for development/testing.

**Q3: What is KVM and why is it special?** A: KVM (Kernel-Based Virtual Machine) is a Linux para-virtualization hypervisor that is integrated as part of the Linux kernel (since version 2.6.20). It is special because it leverages existing Linux kernel mechanisms for memory management and scheduling, making it simpler than standalone hypervisors. It uses hardware-assisted virtualization (VT-x/AMD-v) and supports unmodified guest OSes.

**Q4: Name two examples each for Type 1 and Type 2 hypervisors.** A: Type 1: VMware ESX Server, Xen. Type 2: VMware Workstation, Oracle VirtualBox.

**Q5: What is a Hybrid Hypervisor? Give examples.** A: A Hybrid Hypervisor supports both bare-metal and hosted-application operation simultaneously. Examples are MS Virtual Server and MS Virtual PC.

---

---

# 4. Physical Machine Context — Privilege Rings

To understand hypervisors, one must understand how a physical machine manages privilege levels.

## Privilege Rings in x86

Physical machines have real hardware resources (CPU, memory, I/O) managed by the OS through hardware interfaces defined by the ISA (Instruction Set Architecture, e.g., x86).

The x86 processor provides **four privilege rings** (Ring 0 to Ring 3):

```
         Outermost (least privileged)
+------------------------------------------+
|  Ring 3 — User Applications              |
|  Ring 2 — (unused in most OSes)          |
|  Ring 1 — (unused in most OSes)          |
|  Ring 0 — OS Kernel (most privileged)    |
+------------------------------------------+
         Innermost (most privileged)
```

- **Ring 0 (Kernel Mode / Privileged Mode):** Full access to hardware. Can execute privileged instructions like I/O instructions, Halt, Turn off all Interrupts, Set the Timer, Context Switching, Clear Memory, Remove a process from memory.
- **Ring 3 (User Mode):** Only safe instructions (Load, Store, Add, Subtract, etc.). Cannot directly access hardware.

## System Calls

User-mode programs access hardware through **system calls**:

1. When a system call is made, the mode bit is set to **0 (supervisory mode)**.
2. The privileged action is executed.
3. The mode bit is set back to **1 (user mode)**.
4. Control returns to the user process.

This ensures no user process ever gets supervisory control permanently.

---

---

# 5. Paravirtualization and Full (Transparent) Virtualization

## Why Different Virtualization Types Exist

Applications execute both ordinary and **privileged instructions** that modify machine resources. User processes cannot be trusted with privileged instructions. The OS uses four rings to enforce privilege. In a virtualized environment, the hypervisor must manage these interactions for multiple VMs.

**The core problem:** Not all architectures make all privileged instructions "trap-able" (the original x86 had 17 such problematic instructions), leading to different virtualization approaches.

---

## Full (Transparent) Virtualization

- **Definition:** Provides **complete simulation of the underlying hardware**.
- The guest OS **runs without modification** — it thinks it's running on real hardware.
- The hypervisor **intercepts and emulates** privileged and sensitive instructions at **runtime**.
- Technique used: **Binary Translation** (discussed in detail in Section 6).
- **Examples:** Oracle VirtualBox, VMware (early versions), KVM.

---

## Paravirtualization

- **Definition:** A virtualization technique that presents a **software interface to VMs that is similar but not identical** to the underlying hardware.
- The guest OS must be **explicitly modified (ported)** to run on top of the VMM/Hypervisor.
- The guest OS kernel is compiled with changes, **replacing privileged instructions with hypercalls** to the hypervisor.
- **Hypercalls** are like system calls but from a guest OS to the VMM — special I/O APIs that do not emulate hardware I/O.
- Para-virtualization handles privileged/sensitive instructions at **compile time** (not runtime).
- Useful when the **source code of the OS is modifiable**.
- **Examples:** Xen (uses paravirtualization), IBM: MVS/VM, Linux, Microsoft Windows (as guest in Xen).

### Why Paravirtualization?

- **Performance degradation** is the critical issue of virtualized systems.
- Para-virtualization attempts to **reduce virtualization overhead** by modifying the guest OS kernel.
- There is **no requirement to modify user applications** — only the guest OS kernel changes.

---

## Guest OS Compiler Support in Paravirtualization

- The guest OS running in a guest domain runs at **Ring 1** instead of Ring 0.
- This means it **cannot execute some privileged/sensitive instructions**.
- The privileged instructions are replaced with **hypercalls** to the hypervisor.
- After replacing instructions, the modified guest OS emulates the behavior of the original.
- On UNIX, a system call involves an interrupt/service routine; hypercalls use a **dedicated service routine**.
- **Xen** assumes this para-virtualization architecture.

---

## Paravirtualization — Memory Handling Example (Xen Writable Page Tables)

This is an important detailed example. Understand the flow:

### The Problem (Pure Trap-and-Emulate)

- Suppose Guest OS wants to start Oracle, which needs **1 GB of virtual memory**.
- With 4 KB page size: 1 GB / 4 KB = **250,000 page table entries**.
- Under pure trap-and-emulate: **250,000 traps** to the VMM — one per entry. Extremely slow.

### Xen's Solution (Paravirtualized Writable Page Tables)

```
GuestOS  →  Guest Page Table  (Read Access initially)
         →  Tries Write Access  →  Generates TRAP
         →  Xen makes page temporarily writable
         →  Removes page from physical page table
         →  Guest restarts and modifies page table entries
         →  On completion → generates trap → Xen notified
         →  Xen updates Physical Page Table (playback)
```

**Calculation:**

- The 1 GB page table itself requires 4 MB of storage.
- 4 MB / 4 KB = **1,000 pages** of page table storage.
- With Xen: Only **1,000 traps** (one per page of the page table) instead of 250,000 traps.
- **Performance improvement: 250x reduction in traps.**

---

## Paravirtualization Issues and Advantages

### Issues

1. **Compatibility and portability**: Must also support unmodified OSes in some scenarios.
2. **High maintenance cost**: Deep OS kernel modifications are required.
3. **Variable performance advantage**: Varies greatly due to workload variations.

### Advantages

- Compared to full virtualization, paravirtualization is **relatively easier and more practical**.
- Full virtualization's main bottleneck is **low performance in binary translation**. Paravirtualization avoids this.
- **Xen, KVM, and VMware ESX** are good examples of systems that use para-virtualization principles.

---

## VMware ESX Server Architecture (Para-Virtualization)

- ESX is a VMM/Hypervisor for **bare-metal x86 SMP servers**.
- Accesses hardware resources (I/O) directly with **complete resource management control**.
- **VM kernel interacts directly with hardware** without involving a host OS.
- ESX-enabled server has **four components**:
    1. **Virtualization Layer** — virtualizes physical hardware (CPU, memory, network, disk). Every VM has its own set of virtual hardware resources.
    2. **Resource Manager** — allocates CPU, memory, disk, and network bandwidth, mapping them to each VM's virtual hardware.
    3. **Hardware Interface Components** — device drivers and VMware ESX Server File System.
    4. **Service Console** — boots the system, initiates the VMM and Resource Manager, and relinquishes control to those layers. Also facilitates system administrators.

---

## Summary: Full vs. Paravirtualization

|Feature|Full (Transparent) Virtualization|Paravirtualization|
|---|---|---|
|Guest OS modification|Not required (runs unmodified)|Required (must be ported)|
|Instruction handling|At runtime (binary translation/emulation)|At compile time (hypercalls replace privileged instructions)|
|Performance|Lower (translation overhead)|Higher (less overhead)|
|Compatibility|Works with any OS|Only OSes whose source can be modified|
|Technique|Binary translation, trap-and-emulate|Hypercalls|
|Examples|Oracle VirtualBox, VMware Workstation|Xen, KVM, VMware ESX|

---

## Q&A — Paravirtualization and Full Virtualization

**Q1: What is the fundamental difference between full virtualization and paravirtualization?** A: In full virtualization, the guest OS runs completely unmodified and the hypervisor emulates hardware at runtime. In paravirtualization, the guest OS kernel is modified to replace privileged instructions with hypercalls to the hypervisor, improving performance but requiring OS source code changes.

**Q2: What is a hypercall? How is it different from a system call?** A: A hypercall is a call from the guest OS directly to the hypervisor/VMM, analogous to how a system call is a call from a user application to the OS. System calls use interrupt/service routines; hypercalls use dedicated service routines. Hypercalls bypass hardware I/O emulation and thus are faster.

**Q3: In the Xen page table example, how does paravirtualization improve performance vs. pure trap-and-emulate?** A: For a 1 GB Oracle workload with 4 KB pages, pure trap-and-emulate generates 250,000 traps. With Xen's writable page table support, only 1,000 traps are generated (one per page of the page table, i.e., 4 MB / 4 KB). This is a 250x reduction.

**Q4: Why must the guest OS run at Ring 1 in Xen's paravirtualization?** A: Ring 1 is less privileged than Ring 0. If the guest OS ran at Ring 0 alongside the hypervisor, it could directly execute privileged instructions without the hypervisor's mediation, breaking isolation and control. By running at Ring 1, any attempt to execute privileged instructions causes a trap to the hypervisor at Ring 0, maintaining control.

**Q5: What are the three main issues with paravirtualization?** A: (1) Compatibility and portability — must also support unmodified OSes. (2) High maintenance cost — deep OS kernel modifications needed. (3) Variable performance advantage — performance gain depends heavily on workload.

**Q6: What are the four components of the VMware ESX Server architecture?** A: Virtualization Layer, Resource Manager, Hardware Interface Components, and Service Console.

---

---

# 6. Virtualization Software Techniques — Trap & Emulate and Binary Translation

## Direct Execution (Recap)

**Idea:** Run most VM instructions directly on hardware.

- **Advantage:** Performance close to native execution.
- **Challenge:** If all instructions execute directly, the VMM has no control — isolation is broken.

The original intent for a VMM is to run an unmodified guest OS as if it were executing directly on hardware.

---

## Trap and Emulate

### Concept

**Idea:**

1. **Trap to the hypervisor** when the VM tries to execute an instruction that could change system state (i.e., impact safety/isolation).
2. **Emulate** the execution of these instructions inside the hypervisor.
3. **Directly execute** any other innocuous/safe instructions on hardware.

### Two Categories of Instructions

**User instructions (safe):**

- Compute instructions: `add`, `mult`, `ld`, `store`, `jmp`

**System instructions (privileged):**

- System management: `iret`, `invlpg`, `hlt`, `in`, `out`
- `invlpg` — privileged instruction to invalidate TLB entries.

### Two Modes of CPU Operation

- **User mode** (x86-64: Ring 3) — only safe instructions.
- **Privileged mode** (x86-64: Ring 0) — all instructions including system instructions.

Attempting to execute system instructions in user mode generates a **trap / General Protection Fault (GPF)**.

### Ring Assignment in Trap-and-Emulate

|Component|Ring|
|---|---|
|VMM / Hypervisor|Ring 0 (most privileged)|
|Guest OS|Ring 1 (needs privileges but not as much as VMM)|
|Guest Applications|Ring 3 (least privileged)|

- Guest OS runs at Ring 1 — it traps to VMM (Ring 0) when it tries privileged operations.
- Guest applications run at Ring 3 — they remain protected from the Guest OS.

### Trap Flow (System Call from Guest App)

```
1. Guest app executes syscall/interrupt (int n)  →  Trap to VMM
2. VMM doesn't know how to handle  →  Jumps to Guest OS trap handler
3. Trap handled by Guest OS normally
4. Guest OS performs return-from-trap (iret)  →  Privileged instr  →  Trap to VMM
5. VMM jumps to corresponding user process
```

Any privileged action by Guest OS (set IDT, set CR3, access hardware) → traps to VMM → VMM emulates → returns to Guest OS.

---

## Problems with Trap-and-Emulate

### Problem 1: Performance Overhead

- Trapping privileged instructions is expensive (context switching to VMM and back).
- High trap costs for frequent privileged operations.

### Problem 2: x86 Architecture Was Not Fully Virtualizable

The original x86 ISA did NOT meet the Popek & Goldberg requirements. There were **17 sensitive but non-privileged instructions** in x86:

`SGDT, SIDT, SLDT, SMSW, PUSHF, POPF, LAR, LSL, VERR, VERW, POP, PUSH, CALL, JMP, INT, RET, STR, MOV`

**Why are these problematic?**

- Some x86 instructions that **change hardware state (sensitive) run in BOTH privileged and unprivileged modes**.
- They **behave differently** depending on whether the guest OS is in Ring 0 vs. Ring 1.
- **OS behaves incorrectly in Ring 1** — the instruction does not trap to VMM.

**Classic Example — `popf` (pop flags):**

- In Ring 0: `popf` can change the system state flag (e.g., interrupt delivery flag).
- In Ring 3: CPU **silently ignores** `popf` when it tries to alter the interrupt flag — no trap!
- This is a **behavior-sensitive instruction that is NOT privileged** → it does NOT generate a trap.

**Classic Example — `pushf`:**

- Simply reveals that the kernel is running in user mode (guest OS can detect it is being virtualized).

**Result:** x86 ISA (pre-2005) does NOT meet the Popek & Goldberg requirements for virtualization.

### Problem 3: Guest OS May Detect It Is Virtualized

- Some registers (like the code segment register `%cs`) in x86 reflect CPU privilege level.
- If the guest OS is in Ring 1, it can read `%cs` and discover it is not actually in Ring 0, detecting the hypervisor.

### Problem 4: Memory Protection

- Physical memory is a shared resource.
- May need to prevent Guest OS from directly accessing physical memory.
- Requires two levels of virtual address translation:
    - Virtual page → fake physical page (guest physical address).
    - Fake physical page → real machine frame/page.

---

## Trap and Emulate Exercise: Page Table Walkthrough

**Setup:**

- Guest OS is trying to define a new page `gvp` for a process.
- Finds an unused page `gpp` in VM memory (guest physical memory).
- Tries to load `(gvp, gpp)` into the page table.
- `hpp` is a free physical page in real memory.

**Execution Steps:**

1. Guest OS tries to modify page tables so `gvp` points to `gpp`.
2. Hypervisor **traps this privileged instruction**.
3. Hypervisor maps (if not already done) `gpp` to a host physical page `hpp`.
4. Loads the Guest page table so `gvp` points to `hpp` by copying from VMM/Host page table (to enhance usability).
5. Other page table operations (e.g., read) must also be trapped.

> The new page table created by the VMM is also called a **Shadow Page Table**.

---

## Strictly Virtualizable

A processor or mode is **strictly virtualizable** if, when executed in a lesser-privileged mode:

- All instructions that access privileged state **should trap**.
- All instructions either **trap or execute identically**.

This is the test for whether trap-and-emulate will work on a given architecture.

---

## Binary Translation

### What Is It?

Binary Translation is a specific approach to implementing **full virtualization** that does **not require hardware virtualization features** (like VT-x instructions for handling the 17 non-virtualizable instructions).

### Why Binary Translation?

- Not having VT-x support limits VMs to not being further virtualizable by users (e.g., an EC2 instance cannot be nested).
- Binary translation handles the 17 problematic x86 instructions without hardware help.

### How It Works

1. The monitor (VMM) **inspects the next sequence of instructions** (typically a basic block — all instructions up to the next branch/control transfer).
2. Each instruction is **translated** and the translation is copied into a **translation cache**.
3. Three types of translation:
    - **Ident (identity):** Instructions that pose no problems are copied as-is.
    - **Inline:** Simple dangerous instructions are translated into a short emulation code sequence and put directly into the translation cache. Example: modification of the Interrupt Enable flag.
    - **Call-out:** Complex dangerous instructions require calls to emulation code in the VMM. Example: changing the page table base register.

### Binary Translation Complexities

- **Control flow changes:** Translation can change the addresses of instructions, so branches must be re-translated.
- **Keeping track of branch addresses** is necessary.
- Each translator invocation consumes one **Translation Unit (TU)** and produces one **Compiled Code Fragment (CCF)**.

### Key Properties

- **Supports x86 architecture** virtualization (handles the 17 non-virtualizable instructions).
- VMware used this technique for full virtualization.
- **Conceptually simple example:** Replacing the `popf` instruction with a trap to kernel and emulation.

---

## Q&A — Trap & Emulate and Binary Translation

**Q1: Explain the Trap-and-Emulate technique in detail.** A: The hypervisor runs at Ring 0. The Guest OS is pushed to Ring 1. Guest applications run at Ring 3. When the Guest OS attempts to execute a privileged instruction (system instruction), the hardware generates a trap (General Protection Fault) at Ring 1, transferring control to the VMM at Ring 0. The VMM then emulates the effect of that instruction safely. Safe/non-sensitive instructions execute directly on hardware without VMM intervention, preserving performance.

**Q2: What are the 17 non-virtualizable x86 instructions, and why are they problematic?** A: They are: SGDT, SIDT, SLDT, SMSW, PUSHF, POPF, LAR, LSL, VERR, VERW, POP, PUSH, CALL, JMP, INT, RET, STR, MOV. These are sensitive instructions (they access/modify system state) but they are NOT privileged — meaning they run in both Ring 0 and Ring 3 but behave differently in each. In user mode (Ring 3), they silently fail or behave differently instead of generating a trap to the VMM. Since no trap is generated, the VMM cannot intercept and control them.

**Q3: Why does `popf` break Trap-and-Emulate?** A: `popf` is used in Ring 0 to change the interrupt enable flag (system state). However, when executed in Ring 3 (user mode), the CPU silently ignores it — no trap is generated. This means the VMM never knows the guest OS attempted to change the interrupt flag, breaking the VMM's ability to control interrupt delivery for the guest.

**Q4: What is Binary Translation and how does it solve the x86 virtualization problem?** A: Binary Translation has the VMM dynamically inspect and rewrite guest machine code before execution. It scans basic blocks of code, identifies "unsafe" sensitive but unprivileged instructions, and replaces them with safe equivalents or calls to VMM emulation routines. Since there are no sensitive-but-unprivileged instructions left in the translated code, no trap needs to be generated — the VMM has already handled them proactively.

**Q5: What is a "strictly virtualizable" processor?** A: A processor is strictly virtualizable if: (1) all instructions that access privileged state trap when executed at a lower privilege level, and (2) all instructions either trap or execute identically. This guarantees that trap-and-emulate will work correctly.

**Q6: Describe the three types of instruction translations in Binary Translation.** A: (1) Ident — safe instructions copied unchanged. (2) Inline — simple dangerous instructions replaced with inline emulation code directly in the translation cache. (3) Call-out — complex dangerous instructions that require calls to the full VMM emulation code.

**Q7: What is a Translation Unit (TU) and Compiled Code Fragment (CCF)?** A: Each invocation of the binary translator consumes one Translation Unit (a basic block of guest code) and produces one Compiled Code Fragment (the translated, safe version of that code placed in the translation cache).

---

---

# 7. Hardware Virtualization (Intel VT-x / AMD-v)

## Why Hardware Assistance Was Needed

Three techniques existed to virtualize x86:

1. **Paravirtualization (Xen):** Rewrite guest OS code; makes hypercalls instead of privileged ops. Needs OS source code — cannot work with unmodified OS.
2. **Full virtualization / Binary Translation (VMware Workstation):** Translate sensitive instructions dynamically. Higher overhead than paravirtualization.
3. **Hardware-assisted virtualization (KVM in Linux):** CPU has a special VMX mode.

## Workaround for x86-32: Paravirtualization Trick

- Block all 17 problematic sensitive-but-non-privileged instructions.
- Modify the guest OS to `#undef` them (remove their use).
- Provide **hypercalls** from guest OS to VMM as replacements.
- Advantage: Simplicity. Disadvantage: Needs modified guest OS.

## Two Challenges of Virtualizing x86-64

1. How to **hide system/privileged state** from the VM?
2. How to ensure a VM **cannot directly change system state** (e.g., interrupt flags) of the processor?

## Hardware-Assisted Virtualization Solution (Intel VT-x / AMD-v)

### Core Idea: Two New Modes of Operation

Intel introduced **VMX (Virtual Machine eXtensions)** with two new modes:

|Mode|Who Runs Here|
|---|---|
|**VMX Root Mode**|Hypervisor/VMM|
|**VMX Non-Root Mode**|Guest VMs (each with full rings 0-3)|

- **Each mode has a complete set of execution rings (0-3).**
- Hardware state is **duplicated for each operation mode**.
- Hypervisor runs in **root mode**; VMs run in **non-root mode**.

### New VMX Instructions

- `vmlaunch` — launch a VM.
- `vmresume` — resume a VM.
- `vmexit` — exit from the VM back to VMM (triggered by sensitive instructions).
- `vmread` / `vmwrite` — read/write the VMCS.

### VMCS (Virtual Machine Control Structure)

- A data structure in memory that controls VMX transitions.
- Stores: guest state, host state, and VM execution control fields.
- The VMM initializes/modifies the VMCS.

### Ring Layout with Hardware Virtualization

```
     Non-Root Mode              Root Mode
   +------------------+        +-----------+
VM 1: Ring 3: Guest app        Ring 3
      Ring 2                   Ring 2
      Ring 1                   Ring 1
      Ring 0: Guest OS         Ring 0: Hypervisor/VMM

VM 0: Ring 3: Guest app
      Ring 2
      Ring 1
      Ring 0: Guest OS
```

### How KVM Uses VT-x

1. **VMM initializes/modifies VMCS** with the desired VM configuration.
2. **VMM executes `vmlaunch`/`vmresume`** to enter the VM.
3. **VM runs** (in VMX Non-Root mode) until it hits a `vmexit` condition.
4. **VMM analyzes the `vmexit` cause** and takes the appropriate action.
5. Cycle repeats.

When any "sensitive" instruction is executed in non-root mode, it either:

- Is executed by the processor on duplicated state (transparent to VM), OR
- Causes a `vmexit` trap to the hypervisor.

### What Causes a VM Exit?

- Sensitive instructions that need VMM attention.
- `VMCALL` instruction (used for hypercalls from guest).
- Certain I/O operations.
- Memory access violations.

---

## KVM Architecture

- KVM **extends the Linux kernel** to add hypervisor functionality.
- **Leverages Linux** code for scheduling, paging, etc.
- **Uses VT-x and EPT** (Extended Page Tables).
- Each VM appears as a **Linux process** to the host OS.
- This makes KVM simpler — the Linux kernel handles all the complex parts, and KVM just does virtualization.

## Xen Architecture

- **Paravirtualization hypervisor**.
- VMs are called **domains**.
- **Dom0 (Domain 0):** Special privileged domain based on Linux; handles all I/O; uses filter drivers for Windows.
- **Xen Hypervisor:** Handles all functions other than I/O: trap-and-emulate, binary translation, replacing sensitive instructions with hypervisor calls.
- Even Xen now uses hardware virtualization assistance.

## Current Status of Hardware Assist

- Almost **all VMMs today use hardware assistance**.
- KVM is integrated into Linux and makes heavy use of it.
- Even Xen and VMware Workstation use it.

---

## Q&A — Hardware Virtualization

**Q1: What are the two new modes introduced by Intel VT-x and what runs in each?** A: VMX Root Mode (where the Hypervisor/VMM runs) and VMX Non-Root Mode (where Guest VMs run). Each mode has a complete set of execution rings (0-3), so the guest OS can run at Ring 0 within Non-Root mode, which it previously could not do safely in software-only approaches.

**Q2: What is VMCS and what is its purpose?** A: VMCS (Virtual Machine Control Structure) is a data structure stored in memory that controls VMX transitions between root and non-root modes. It stores the guest state, host state, and VM execution control information. The VMM uses vmread/vmwrite instructions to configure it.

**Q3: What is a `vmexit` and what triggers it?** A: A `vmexit` is a hardware-triggered transfer of control from the VM (running in VMX Non-Root mode) back to the VMM (in VMX Root mode). It is triggered when a VM executes a sensitive instruction that requires VMM intervention, issues a VMCALL, performs certain I/O, or causes certain memory access violations.

**Q4: How is KVM different from a standalone hypervisor like Xen?** A: KVM extends the Linux kernel itself to become a hypervisor rather than being a separate software layer. This means it reuses all Linux subsystems (scheduler, memory management, device drivers) rather than reimplementing them. Each VM appears as a regular Linux process. KVM heavily uses Intel VT-x/AMD-v for hardware-assisted virtualization.

**Q5: What is Dom0 in Xen and what does it do?** A: Dom0 (Domain 0) is a special, privileged virtual machine in the Xen architecture that is based on Linux. It has direct hardware access and handles all I/O operations on behalf of other VMs (domains). It also performs management tasks like creating, terminating, or migrating other VMs.

---

---

# 8. Memory Virtualization

## Recap: Un-Virtualized Address Translation

In a normal (non-virtualized) system:

- Each process has a virtual address space.
- The OS manages page tables.
- **VA → PA** translation is done via TLB (hit) or Page Table Walk (miss).
- The **CR3 register** points to the root of the page table.
- Hardware page walker (PTW) generates load-like instructions to access page table entries.
- Recent translations are cached in the **TLB (Translation Lookaside Buffer)**.

**Address translation formula:**

```
Virtual Address (VA) → TLB → Hit: Physical Address (PA) directly
                            → Miss: Page Table Walk (PTW) using CR3
                                   → Page Table → Physical Address (PA)
```

---

## Requirements for Memory Virtualization

1. **VMs / Guest OSes should NOT have direct access to physical memory.**
2. **Only the Hypervisor/VMM should manage Host Physical Memory (sPA).**
3. **Guest OS should be made to believe it is accessing physical memory** (it sees Guest Physical Addresses, gPA — which are actually fake/imaginary).

---

## Two Levels of Address Translation in a VM

```
Application → Guest Virtual Address (gVA)
           → [Guest Page Table (gPT)] → Guest Physical Address (gPA)
           → [Host/System Page Table (sPA)] → System Physical Address (sPA)
```

There are **two levels of address translation on each memory access** by an application running inside a VM:

**Level 1:** `gVA → gPA` (managed by Guest OS, via Guest Page Table, gPT) **Level 2:** `gPA → sPA` (managed by Hypervisor, via Host/System Page Table)

---

## Two Implementations of Two-Level Translation

### 1. Shadow Page Tables (Software-Based)

#### Concept

- The VMM creates a **Shadow Page Table (sPT)** for each Guest Page Table (gPT).
- The Shadow Page Table maps **gVA directly to host Physical Address (sPA)** — combining both levels into one.
- The hypervisor makes **CR3 point to the shadow page table**, not the guest page table.
- The hardware page walker (PTW) walks the shadow page table.

```
Guest Page Table (gPT): gVA → gPA (Guest sees this)
Shadow Page Table (sPT): gVA → sPA (CPU actually uses this)
```

Shadow page tables were developed as a **software-only technique** before hardware support existed.

#### How Shadow Page Tables Are Maintained

1. **Guest OS page tables are marked read-only** by the VMM.
2. Any write to a guest page table by the guest OS generates a **page fault → trap to VMM**.
3. VMM intercepts the modification, validates it, and **updates the shadow page table** accordingly (syncing gPT changes to sPT, replacing gPA with sPA).
4. The shadow page table is **not visible to the guest**.

#### Shadow Page Table Challenges

- **Write-protect overhead:** Every guest page table write causes a page fault → trap to hypervisor.
- **Per-process shadow page tables:** One shadow page table per guest application process.
- **Context switch overhead:** Every context switch in the guest requires a trap to update CR3 to point to the new shadow page table.
- **TLB flush overhead:** Loss of performance due to TLB flush on every "world-switch" (switching between VMs).
- **Memory overhead:** Shadow copies of all guest page tables consume extra memory.
- **Consistency overhead:** Maintaining consistency between guest PTs and shadow PTs adds significant VMM trap overhead.

---

### 2. Nested / Extended Page Tables (Hardware-Assisted)

#### Concept

Nested/Extended Page Tables were introduced to **avoid the software overheads of shadow paging**.

**Key Idea:** Make hardware aware of TWO levels of address translation simultaneously.

```
gVA → [gPT, managed by Guest OS] → gPA → [nPT/EPT, managed by Hypervisor] → sPA
```

- **Guest OS controls its own page tables (gPT):** Maps gVA → gPA. Guest OS has full control.
- **Hypervisor controls the Extended/Nested Page Table (EPT/nPT):** Maps gPA → sPA.
- The hardware page walker performs a **two-dimensional walk**: walking both gPT and nPT to resolve a virtual address.
- **Two CR3 registers:**
    - `gCR3` → points to gPT (guest page table root)
    - `nCR3` → points to nPT (nested page table root)

#### Advantages of Nested/Extended Page Tables

- **Eliminates shadow page tables** entirely.
- **No VM exits** due to guest page faults, INVLPG, or CR3 changes — the guest has full control over its own page tables.
- **No need to write-protect guest page tables** or intercept guest page table modifications.
- **Eliminates TLB flushes** on context switches between guest processes.
- Overall **improved memory virtualization performance** (EPT improves performance significantly).

#### EPT — Extended Page Tables (Intel)

- EPT is a new page-table structure under the control of the VMM.
- Defines the mapping between **guest physical addresses and host physical addresses**.
- EPT base pointer (EPTP) is a new VMCS field pointing to the EPT page tables.
- EPT is activated on VM entry and deactivated on VM exit.
- No VM exits required for INVLPG operations — the hypervisor intercepts INVLPG to remove the corresponding shadow translation.

---

## Q&A — Memory Virtualization

**Q1: Why can't a Guest OS directly access host physical memory?** A: Because physical memory is a shared resource managed exclusively by the hypervisor. If a guest OS could directly address host physical memory, it could access or corrupt memory belonging to other VMs or the hypervisor itself, breaking isolation.

**Q2: What is a Shadow Page Table and how does it work?** A: A Shadow Page Table is a VMM-managed page table that directly maps Guest Virtual Addresses (gVA) to System Physical Addresses (sPA), bypassing the need for two separate table walks at runtime. The VMM creates one shadow PT for each guest PT, marks the guest's PT as read-only, intercepts any write to the guest PT (via a page fault trap), and updates the shadow PT accordingly. The hardware CR3 is pointed to the shadow PT so the CPU uses it directly.

**Q3: What are the main drawbacks of Shadow Page Tables?** A: (1) High overhead from trapping every guest page table write (write-protect generates page faults). (2) One shadow PT per guest process → high memory usage. (3) TLB flushed on every VM context switch (world switch). (4) Complex consistency maintenance between guest PTs and shadow PTs.

**Q4: How do Extended/Nested Page Tables solve the Shadow Page Table problem?** A: EPT/NPT inform the hardware about both levels of translation (gVA→gPA via gPT, and gPA→sPA via EPT). The hardware page walker performs a two-dimensional walk, handling both levels in hardware without VMM intervention. This eliminates the need to write-protect guest page tables, removes most VM exits caused by page table operations, and eliminates the shadow PT memory overhead.

**Q5: What is the "two-dimensional page walk" in nested paging?** A: To resolve a gVA to sPA, the hardware page walker walks the guest page table (gCR3 → gPT) to get gPA, then for each gPA encountered during the walk, it also walks the nested page table (nCR3 → nPT) to get the actual sPA. This is called a two-dimensional walk because it involves walking two sets of page tables simultaneously.

**Q6: What does INVLPG do, and how is it handled with EPT vs. shadow paging?** A: INVLPG is a privileged instruction used by the OS to invalidate a specific TLB entry (remove a translation). With shadow paging, the VMM intercepts INVLPG, removes the corresponding translation from the shadow PT, and executes INVLPG for the removed translation. With EPT, the guest can execute INVLPG for its own translations without triggering a VM exit, since guest PT management is fully guest-controlled.

---

---

# 9. I/O Virtualization

## What is I/O Virtualization?

I/O Virtualization (IOV) is technology that uses software to **abstract upper-layer protocols from physical connections/transports**. It involves managing the routing of I/O requests between virtual devices and shared physical hardware.

## Three Ways to Implement I/O Virtualization

### 1. Full Device Emulation

- **Emulates well-known, real-world devices** entirely in software.
- All functions of a device/bus infrastructure (device enumeration, identification, interrupts, DMA) are **replicated in software in the VMM**.
- The VMM acts as a virtual device; I/O access requests from the guest OS are **trapped in the VMM**, which then interacts with the real I/O device.
- **Drawback:** High overhead due to full software emulation.
- Implementation: A middle layer maps real I/O devices to virtual devices for the guest device driver.

### 2. Para-I/O Virtualization (Split Driver / Hosted I/O Virtualization)

- Also known as the **split driver model** — consists of a **frontend driver** and a **backend driver**.
- **Frontend driver** runs in Domain U (User/Guest domain) and manages I/O requests of the guest OS.
- **Backend driver** runs in Domain 0 and manages the real I/O devices, multiplexing I/O data of different VMs.
- They interact via a **block of shared memory**.
- Achieves **better device performance than full device emulation** but with **higher CPU overhead**.
- Typically used in **Xen**.

```
Guest VM (Domain U)
   → Frontend Driver
   → [Shared Memory Block]
   → Backend Driver
   → Domain 0
   → Real I/O Device
```

### 3. Direct I/O (Passthrough) I/O Virtualization

- Lets the VM **access devices directly** — bypassing the VMM's I/O emulation layer.
- Achieves **close-to-native performance** with **low CPU cost**.
- Enables a device to **directly DMA to/from host memory**.
- **Intel VT-x** allows a VM to have direct access to a physical address (if configured by VMM).
- **VT-d extensions** add virtualization support to Intel chipsets, enabling assignment of specific I/O devices to specific VMs.
- **Main concern:** Limited scalability — a physical device can only be assigned to **one VM**.

### Comparison of I/O Virtualization Methods

|Method|Performance|CPU Overhead|VM Exclusivity|Examples|
|---|---|---|---|---|
|Full Device Emulation|Lowest|Highest|Shared|QEMU|
|Para-I/O (Split Driver)|Medium|Medium-High|Shared|Xen|
|Direct I/O (Passthrough)|Near-native|Lowest|Exclusive (1 VM)|VT-d, SR-IOV|

## I/O Virtualization Advantages

- **Flexibility:** Hardware independence, better utilization, faster provisioning.
- **Cost minimization:** Fewer cables, cards, and switch ports.
- **Increased density:** More connections in a given space.
- **Minimizing cables:** Reduces multiple cables connecting servers to storage and networks.

## Xen I/O Virtualization Specifically

- Xen does **NOT emulate hardware devices**.
- Exposes device **abstractions** for simplicity and performance.
- I/O data transferred to/from guest via Xen using **shared-memory buffers**.
- **Virtualized interrupts:** Lightweight event delivery mechanism from Xen to guest.
    - Updates a bitmap in shared memory.
    - Optional callback handlers registered by the guest OS.

---

## Q&A — I/O Virtualization

**Q1: What are the three methods of I/O virtualization and briefly describe each?** A: (1) Full Device Emulation — the VMM software emulates a complete virtual device; all I/O operations are trapped and handled by the VMM. High overhead but works with any unmodified guest. (2) Para-I/O / Split Driver — uses a frontend driver in the guest and a backend driver in Dom0 that communicate via shared memory; better performance than emulation but requires modified drivers. (3) Direct I/O / Passthrough — the VM accesses the physical device directly via VT-d; near-native performance but the device is dedicated to one VM only.

**Q2: What is the main limitation of Direct I/O virtualization?** A: Scalability. A physical device assigned via direct passthrough can only be assigned to one VM at a time. This is impractical in environments where many VMs need access to the same type of device.

**Q3: How does Xen implement I/O virtualization?** A: Xen uses a split driver (para-I/O) model with a frontend driver in the guest domain (Domain U) and a backend driver in Dom0. Data is exchanged via shared-memory buffers. Xen also uses lightweight event delivery (virtualized interrupts) by updating a bitmap in shared memory rather than real hardware interrupts.

**Q4: What is VT-d and how does it relate to I/O virtualization?** A: VT-d (Virtualization Technology for Directed I/O) is an Intel chipset technology that adds hardware support for direct I/O virtualization. It enables specific physical I/O devices to be directly assigned to specific VMs, providing address translation support for DMA operations from the device, ensuring memory isolation and protection.

---

---

# 10. Popek and Goldberg Virtualization Principles

## Background

In 1974, **Gerald Popek and Robert Goldberg** formally defined the requirements for a computer architecture to efficiently support virtualization.

These principles apply to **"third-generation" machines** — systems that have:

- At least two operating modes (user and supervisor).
- Some instructions not available in user mode.
- Addressing via a relocation register.
- Ability to perform table lookups.

---

## Terminologies (Defined by Popek & Goldberg)

**Virtual Machine:** A complete compute environment with isolated processing, memory, and communication channels as created by the VMM.

- A VM is an **efficient, isolated duplicate of the physical machine**.

**VMM (Hypervisor):** System software that creates and manages VMs, characterized as:

- **Efficient:** All safe guest instructions run directly on hardware (no VMM intervention).
- **Omnipotent:** Only the VMM can manipulate sensitive state.
- **Undetectable:** A guest cannot determine it is running atop a VMM.

---

## Essential Characteristics of a VMM

1. **Equivalence:** A program running on a VM must behave essentially **identically** to the same program running directly on the physical hardware. (Isolation from a protection perspective.)
    
2. **Resource Control:** The VMM must be in **total control of virtualized resources**. Programs under the VMM cannot access resources not explicitly allocated. The VMM must be able to **regain control** of allocated resources.
    
3. **Efficiency:** The **great majority of machine instructions must execute without VMM intervention** (without trap). Performance should show, at worst, only minor decreases.
    

---

## Classification of Instructions

Popek & Goldberg bucketed all instructions into three types:

### Privileged Instructions

- **Definition:** Instructions that cause a **trap if the processor is not in the privileged mode** (Ring 0).
- Example: Interrupt instructions, I/O port access in Ring 0.

### Sensitive Instructions (Two subtypes)

These access low-level machine states (page tables, I/O devices, privilege bits) that need to be managed by the OS or VMM.

#### a) Behavior-Sensitive Instructions

- Instructions whose **behavior/result depends on the hardware mode or configuration**.
- If a guest OS executes these at lower privilege, the **result will be wrong**.
- x86 examples: `POP, PUSH, CALL, JMP, INT n, RET, LAR, LSL, VERR, VERW, MOV`

#### b) Control-Sensitive Instructions

- Instructions that **attempt to change the configuration of system resources** (modify system registers).
- x86 examples: `PUSHF, POPF, SGDT, SIDT, SLDT, SMSW`

### Safe Instructions

- Instructions that are **not sensitive** — they can be executed at any privilege level without affecting system state.

---

## Popek & Goldberg Theorems

### Theorem 1 (Sufficient Condition for Virtualizability)

> **For any conventional third-generation computer, a VMM may be constructed if the set of sensitive instructions for that computer is a subset of the set of privileged instructions.**

**Meaning:**

- If every sensitive instruction is also a privileged instruction (i.e., it traps when executed in non-privileged mode), then a VMM can be built.
- All sensitive instructions will trap, allowing the VMM to intercept and emulate them.
- Non-privileged (safe) instructions execute natively (efficiently).

**Implication for x86:** The original x86 FAILS this theorem because it has 17 sensitive-but-non-privileged instructions (they are sensitive but do NOT trap in Ring 3).

```
For VMM to exist: Sensitive Instructions ⊆ Privileged Instructions
x86 pre-2005: NOT the case → x86 is NOT directly virtualizable
```

### Theorem 2 (Recursive Virtualizability)

> **A conventional third-generation computer is recursively virtualizable if it is: (a) virtualizable, AND (b) a VMM without any timing dependencies can be constructed for it.**

**Meaning:**

- **Recursive virtualization** = ability to install a VM within a VM (nested virtualization).
- If the architecture cannot be virtualized classically, **binary translation** can be used — replacing sensitive instructions that do not generate traps.
- Recursive virtualization allows running a VMM inside a VMM.

### Theorem 3 (Hybrid VMM)

> **A hybrid virtual machine monitor may be constructed for any conventional third-generation machine in which the set of user-sensitive instructions are a subset of the set of privileged instructions.**

**Meaning:**

- Even if the full set of sensitive instructions is not privileged, as long as the user-mode sensitive instructions are a subset of privileged instructions, a hybrid VMM can be built.

---

## x86 Non-Conformance to Popek & Goldberg

### Summary

- x86 ISA **does NOT meet the Popek & Goldberg requirements** for virtualization.
- x86 had **17+ sensitive but unprivileged instructions** that do not trap in Ring 3.

### The `push %cs` Problem

- `push` pushes a register value onto the stack.
- The `%cs` register contains (among other things) **2 bits representing the current privilege level**.
- A guest OS running in Ring 3 could `push %cs` and read those bits, seeing that the privilege level is Ring 3 (not Ring 0), **detecting the hypervisor**.
- To be virtualizable, `push %cs` in Ring 3 should trap so the VMM can push a **fake %cs** indicating Ring 0.

### The `pushf`/`popf` Problem

- `pushf`/`popf` read/write the `%eflags` register.
- Bit 9 of `%eflags` enables interrupts.
- In Ring 0, `popf` can set bit 9.
- In Ring 3, the CPU **silently ignores `popf`** — no trap is generated.
- This should trap so the VMM can detect when the guest wants to change its interrupt level.

### Solution

1. **Workarounds** (paravirtualization or binary translation).
2. **Hardware enhancements** (Intel VT-x / AMD-v) that introduced VMX Root and Non-Root modes, making all sensitive operations cause proper VM exits.

---

## Q&A — Popek & Goldberg

**Q1: State Popek & Goldberg Theorem 1 and explain its significance.** A: Theorem 1 states that a VMM can be constructed for any third-generation computer if the set of sensitive instructions is a subset of the set of privileged instructions. This means that if every instruction that could affect correct VMM functioning (sensitive) always traps (because it's also privileged), the VMM can always intercept and control these instructions. Safe/non-sensitive instructions run natively. The significance is that it gives a formal, sufficient condition for an architecture to be virtualizable.

**Q2: Why does x86 (pre-2005) fail Theorem 1?** A: Because x86 has 17 sensitive but non-privileged instructions (e.g., PUSHF, POPF, SGDT). These instructions can affect system state or behave differently based on privilege level, but they do NOT generate a trap when executed in Ring 3 (user/non-privileged mode). Since they don't trap, the VMM cannot intercept them, violating the requirement that all sensitive instructions trap.

**Q3: What is the difference between behavior-sensitive and control-sensitive instructions?** A: Behavior-sensitive instructions are those whose behavior or result depends on the current hardware mode — executing them at lower privilege produces wrong results (e.g., `push %cs` reveals the actual privilege level). Control-sensitive instructions attempt to change system resource configuration (e.g., `popf` modifies the interrupt flag in `%eflags`).

**Q4: What is recursive virtualization (Theorem 2)?** A: Recursive virtualization is the ability to run a VM inside a VM — a VMM can be instantiated on top of itself. Theorem 2 says an architecture is recursively virtualizable if it is (a) virtualizable and (b) a VMM with no timing dependencies can be built for it. Binary translation can substitute for cases where classical virtualization fails.

**Q5: What are the three essential characteristics of a VMM according to Popek & Goldberg?** A: Equivalence (VMs behave identically to the real machine), Resource Control (only VMM controls resources; programs cannot access unallocated resources), and Efficiency (vast majority of instructions execute natively without VMM intervention).

**Q6: What properties make a VMM "undetectable"?** A: An undetectable VMM must ensure: (1) the guest cannot read any state that reveals it is not running on real hardware (e.g., fake privilege level registers), (2) timing behaviors are indistinguishable from bare-metal execution, and (3) instruction execution results are identical to bare-metal. In practice, this is impossible to guarantee perfectly, but the theoretical model requires it.

---

---

# 11. VM Migration

## What is VM Migration?

VM migration is the process of **moving a virtual machine from one host to another**.

## Types of Migration

### Cold Migration (Offline Migration)

- Migrates a **powered-off VM**.
- Migrations can be across different CPU families.

### Non-Live (Offline) VM Migration

- The VM at the source is **paused**.
- **All state** is transferred to the target host.
- VM execution is **resumed at the target**.
- **Drawback:** Larger downtime.

### Live (Hot) Migration

- Migrates a **powered-on VM with no disruption to service**.
- Includes memory content and all VM-defining information (BIOS, devices, MAC addresses, etc.).
- **No dropped network connections.**
- **Applications continue to run uninterrupted.**
- End users are unaware of the migration.
- Example: **vSphere vMotion**.

---

## Why Migrate?

|Reason|Explanation|
|---|---|
|Hardware Maintenance|Migrate VMs to allow physical server maintenance|
|Workload Balancing|Dynamically adjust loads among cluster nodes|
|Scaling|Move to a physical server with required resources|
|Server Consolidation|Optimize utilization for cost|
|Workload Mobility|Compliance requirements may mandate specific locations|
|Performance|Map VMs to nodes with the best resources|
|Energy Efficiency|Reduce data center energy consumption|
|Business Continuity|Support availability requirements due to failures|

---

## Non-Live (Cold) Migration — Details

- **Simplest** migration technique.
- VM's current execution is **suspended before migration**, resumed after.
- Service is stopped during migration.
- State of VM does not resume on destination until migration is complete.
- **Memory pages** need to be migrated only once.
- Migration time is **short and predictable**.
- **Drawback:** Application performance is degraded during the migration window.

---

## Live Migration — Overview

Live migration is the current standard for production environments where availability is critical. The goal is to migrate without the user application even noticing.

**Key Considerations:**

- Cost/overhead of migration.
- Total time taken for migration.
- VM migration is **resource-intensive** — demands CPU cycles, cache memory, memory capacity, and network bandwidth.
- **Degrades performance** of running applications.
- Must meet **Service Level Agreements (SLAs)**.

**Objectives of Live Migration:**

- Efficiently use bandwidth to optimize application performance.
- **Minimize application downtime** (the window where the VM is stopped).
- Reduce overall transfer time.
- Migrate without significantly impacting the user application.

### Two Techniques of Live Migration

1. **Pre-copy technique**
2. **Post-copy technique**

---

## Pre-Copy Technique

### Six Stages of Pre-Copy Migration

**Stage 1: Selecting the Destination Host**

- Determine the migrating VM and destination host.
- Can be manual or automatic (e.g., load balancing, server consolidation policies).

**Stage 2: Reservation of Resources**

- Reserve CPU, memory, and network resources on the destination host.

**Stage 3: Iterative Pre-Copying Rounds (Key Stage)**

- VM continues running at source while memory is transferred.
- **Round 1:** All memory is transferred to the destination.
- **Subsequent rounds:** Only **dirty pages** (pages modified since the last round) are re-copied.
- Iteration continues until:
    - A fixed threshold is reached, OR
    - The dirty portion of memory is small enough for a final copy.
- **Dirty bitmap:** Tracks which pages were modified in each round.
- VM execution is NOT obviously interrupted during pre-copying.

**Stage 4: Stop and Transfer VM State**

- Suspend the VM and copy the **last dirty pages** (the remaining dirty data after iterations).
- Copy all **non-memory state** (CPU registers, network state) as well.
- This is the **"downtime"** — the period when the VM is stopped and service is unavailable. This must be as short as possible.

**Stage 5: Commitment**

- Commit the new host — the destination host is confirmed as the new home.

**Stage 6: VM Activation at Destination**

- VM reloads all state at the destination and resumes execution.
- **Network connection is redirected** to the new VM.
- Dependency on the source host is cleared.
- Original VM is removed from the source host.

### Pre-Copy Advantages and Disadvantages

||Pre-Copy|
|---|---|
|Advantage|Low VM downtime (only the final dirty pages need to be copied while VM is stopped)|
|Advantage|Good when few memory transfers occur (dirty rate is low)|
|Disadvantage|Repeated copying of dirty pages can increase total migration time|
|Disadvantage|Greater transfer time and downtime if memory is frequently modified (high dirty rate)|

**Users:** KVM, Xen, and VMware hypervisors use the pre-copy technique.

---

## Post-Copy Technique

### Concept

- **Processor state is transferred to the destination FIRST**, before memory content.
- The VM **resumes immediately at the destination** after processor state transfer.
- Memory pages are copied from source to destination **while the VM is running on the destination**.
- **Memory faults (page faults)** are generated on the destination for pages that haven't been fetched yet — these missing pages are fetched from the source on demand.

### Sub-techniques to Reduce Page Faults

**1. Demand Paging:**

- When VM resumes on the destination, page faults are serviced by re-transmitting the missing page from the source server.
- Simple but the slowest option.
- Degrades application performance.

**2. Active Push:**

- Pro-actively pushes VM pages to destination even while the VM is running there.
- If a page fault occurs, demand paging fills the gap.
- Pages are sent only once — either via active push or demand paging.

**3. Memory Prepaging:**

- Predicts which pages will be needed (based on access patterns) and pushes them proactively.
- More efficient than pure active push — reduces memory faults.

### Post-Copy Disadvantages

- **Page fault overhead:** VM is suspended at destination each time it accesses a page not yet transferred, waiting for retransmission from source.
- **Network traffic overhead:** Need to minimize data transfer (memory pages and CPU state) over the network.
- **Repetitive page fault detection:** Main drawback — should be optimized to reduce cycles of memory re-transmission.

---

## Live Migration Comparison

|Feature|Pre-Copy|Post-Copy|
|---|---|---|
|When memory is sent|Before VM stops (iteratively)|After VM resumes at destination (on demand)|
|VM downtime|Short (only final dirty pages)|Very short (only processor state stop)|
|Total migration time|Can be long if dirty rate is high|Can be long if many page faults occur|
|Risk|None — source VM still runs until last step|Higher — VM runs on dest with missing pages|
|Examples|KVM, Xen, VMware|Research implementations|

---

## Issues in VM Migration: Memory, File System, Network

### Memory Migration

- Memory in modern systems ranges from hundreds of MB to several GB.
- **ISR (Internet Suspend-Resume) technique:** Exploits temporal locality — suspended and resumed VM memory states differ only by the work done since last suspension.
- Uses a **tree of small subfiles** representation for efficient delta-transfer (only changed parts are sent).

### File System Migration

- VMs must have a **consistent, location-independent view of the filesystem** available on all hosts.
- **Option 1:** Provide each VM with its own **virtual disk** and transport it with the VM.
- **Option 2:** Use a **global distributed filesystem** accessible from all hosts — no file copying needed.

### Network Migration

- The migrating VM must maintain **all open network connections** without relying on forwarding mechanisms from the original host.
- Each VM is assigned a **virtual IP address** known to other entities (distinct from the host machine's IP).
- Each VM also has its own **virtual MAC address**.
- The VMM maintains a mapping of virtual IP/MAC to their corresponding VMs.
- The migrating VM carries its IP address with it.

---

## Xen Live Migration: Implementation Details

- **Domain 0 (Dom0)** performs tasks to create, terminate, or migrate VMs using a **send/recv model**.
- **Migration daemons** in management VMs perform migration.
- **CBC (Characteristic Based Compression) algorithm** compresses memory pages adaptively.
- **Shadow page tables** in VMM trace modifications (dirty pages) during precopy phase → flags set in dirty bitmap.
- At the start of each precopy round: bitmap is sent to migration daemon, then cleared and shadow page tables are destroyed/re-created for the next round.
- Memory pages denoted by the bitmap are extracted, **compressed**, and sent to the destination.
- Destination **decompresses** the received data.

---

## Q&A — VM Migration

**Q1: What is the difference between live migration and non-live migration?** A: In non-live (cold/offline) migration, the VM is paused at the source, all its state is copied to the destination, and then it is resumed there — causing a service outage (downtime) for the entire migration duration. In live migration, the VM continues running at the source while memory is being transferred; only a brief final "downtime" occurs when the last dirty pages and CPU state are copied. Live migration is transparent to users.

**Q2: Describe the pre-copy live migration technique step by step.** A: (1) Select destination host and reserve resources. (2) Copy all memory to the destination (Round 1). (3) Iteratively re-copy only dirty pages (pages modified during the previous round). (4) Once dirty pages are few enough, stop the VM and copy the final dirty pages plus CPU/network state (this is the "downtime"). (5) Commit the new host. (6) VM activates at the destination and network connections are redirected.

**Q3: What is the key difference between pre-copy and post-copy migration?** A: In pre-copy, memory is transferred to the destination before the VM stops (iteratively). The VM only stops briefly at the end to copy the remaining dirty pages. In post-copy, the processor state is transferred first, the VM resumes immediately at the destination, and memory is transferred on demand (as page faults occur). Pre-copy has lower risk; post-copy has potentially shorter downtime but more page fault overhead.

**Q4: What are dirty pages and why do they matter in pre-copy migration?** A: Dirty pages are memory pages that have been modified since the last copying round. They matter in pre-copy because they must be re-copied in every subsequent round. If the VM has a high "dirty rate" (many pages being modified), repeated rounds may be needed and the total migration time increases. The goal is to reduce the dirty set to a small size so the final stop-and-copy phase is brief.

**Q5: How is network continuity maintained during VM migration?** A: Each VM has its own virtual IP address and virtual MAC address. During migration, the VM carries its virtual IP address to the new host. The VMM maintains the mapping of virtual IP/MAC to the actual host. Network connections are thus preserved — external entities continue communicating with the same virtual IP, which the new host now responds to.

**Q6: What is the role of CBC in Xen's live migration?** A: CBC (Characteristic Based Compression) is an adaptive compression algorithm used in Xen's live migration to compress memory pages before sending them to the destination. Adaptive compression changes its algorithm based on the type of data being compressed, improving bandwidth efficiency and reducing migration time.

---

---

# 12. Lightweight Virtualization — Containers, Namespaces, cgroups

## Motivation for Containers

While VMs provide strong isolation, this isolation is **expensive**:

- Each VM needs its own full OS installation.
- VMs take minutes to boot and consume gigabytes.
- Traditional OSes support multiple processes but share everything (disk, filesystem, network) with only access-control-based separation.

**Containers** provide lightweight virtualization that extends the isolation already provided by the OS.

---

## What Are Containers?

**Linux Containers (LXC):** An **operating-system-level virtualization** method for running multiple isolated Linux systems (containers) on a control host running a **single Linux OS instance**.

- LXC provides a virtual environment with its own **process and network space**.
- **Does NOT create a full-fledged virtual machine.**
- Uses **Linux kernel cgroups and namespace isolation** to achieve this.

> Note: `lxc` (tools) is a user-space toolset for creating and managing Linux Containers — different from LXC Linux containers.

## Container Characteristics

- Containers sit on top of a physical server and its **host OS**.
- Each container **shares the host OS kernel** and usually binaries/libraries (as **read-only**).
- Reduces management overhead: only **one OS** needs to be maintained for patches.
- **Exceptionally light** — only **megabytes** in size, start in **seconds** (vs. gigabytes and minutes for VMs).
- Container creation is similar to **process creation** — speed, agility, portability.
- Higher **provisioning performance** than VMs.

---

## Containers vs. VMs

|Feature|VMs|Containers|
|---|---|---|
|OS|Each VM has its own full OS image|Share the host OS kernel|
|Size|Gigabytes|Megabytes|
|Boot time|Minutes|Seconds|
|Performance|Lower (I/O overhead, boot overhead)|Higher (process-like creation)|
|Flexibility|Run multiple OS variants|Must run same OS family as host|
|Isolation|Strong (hardware-level)|Moderate (OS-level)|
|Resource usage|More|Less|

---

## Linux Namespaces

### What Is a Namespace?

- If you can't name an object, you can't access it (name → access).
- A **namespace wraps a global system resource** in an abstraction that makes it appear to processes within the namespace that they have their own isolated instance of that resource.
- A namespace is a **feature of the Linux kernel** that partitions kernel resources so that different sets of processes see different sets of resources.
- A physical computer can have multiple namespaces.

### Namespaces Used by Docker

|Namespace|What It Isolates|
|---|---|
|PID namespace|Process isolation (each container has its own PID 1)|
|UTS namespace|Hostname and domain name (each container can have its own hostname/FQDN)|
|MNT namespace|Mount points / filesystem access and structure (similar to chroot)|
|IPC namespace|Inter-process communication via shared memory|
|NET namespace|Network interfaces, routing tables, IP addresses, ports|
|USR namespace|User name-to-UID mapping|
|chroot syscall|Controls the location of the filesystem root|
|cgroups|Resource limiting and accounting|
|CAP drop|OS feature restrictions|
|Security modules|Mandatory access controls|

### MNT Namespace Example

```
Container 1                Container 2
 /                          /
 ├── mnt (exclusive)        ├── mnt2 (exclusive)
 └── usr/ (shared)          └── usr/ (shared)
      bin/
```

- Container 1 can access `/usr` (shared) and `/mnt` (non-shared).
- Container 1 CANNOT access `/mnt2`.
- Container 2 can access `/usr` (shared) and `/mnt2` (non-shared).
- Container 2 CANNOT access `/mnt`.

### Namespace Operations (System Calls)

```c
// Create new namespace (clone with flag):
pid = clone(childFunc, stackTop, CLONE_NEWNS | SIGCHLD, argv[1]);
// CLONE_NEWNS = new Mount Namespace

// Join existing namespace:
int setns(int fd, int nstype);

// Create new namespace for calling process:
int unshare(int flags);
// CLONE_NEWNS specifies mount namespace
```

### UTS (Unix Time Sharing) Namespace

- Per namespace: **Hostname** and **NIS domain name**.
- Processes can change UTS values — only reflected in the child namespace.
- Allows containers to have their own **FQDN (Fully Qualified Domain Name)**.
- Example: Global namespace has `globalhost/rootns.com`; green namespace has `greenhost/greenns.org`.
- Named after the data structure used by the `uname` system call.

### NET Namespace

- A virtual network barrier encapsulating a process to isolate its **network connectivity and resources** from the Linux core and other processes.
- Per-namespace network objects:
    - Network devices (Ethernet interfaces)
    - Bridges
    - Routing tables
    - IP addresses
    - Ports
- **VETH (Virtual Ethernet):** A pair of virtual Ethernet devices created as a tunnel between two namespaces. Packets transmitted on one end are immediately received on the other.

---

## chroot Namespace

- The OS filesystem starts at root `/` with `/bin`, `/usr`, etc.
- **`chroot`** changes the root directory for the current process and its children:
    - Mount new `/bin` somewhere (e.g., `/mnt`)
    - Issue `chroot /mnt`
    - Process now sees `/mnt` as its root `/`
- Technique to give each process/container its own isolated filesystem.
- **`pivot_root`** is often used instead — it detaches the old root and attaches the new root:
    - `chroot` points the process's filesystem root to a new directory.
    - `pivot_root` detaches the new root and attaches it to the process's root directory.
    - `chroot` can be escaped with proper capabilities; `pivot_root` is more secure.

---

## cgroups (Control Groups)

### What Are cgroups?

cgroups (Control Groups) is a **Linux kernel feature** to **limit, police, and account** the resource usage for a set of processes.

**Docker uses cgroups to limit system resources.**

### cgroups Capabilities

|Feature|Description|
|---|---|
|Access|Control which devices can be used per cgroup|
|Resource Limiting|Limit memory, CPU, block I/O, device accessibility|
|Prioritization|Who gets more CPU, memory, etc.|
|Accounting|Track resource usage per cgroup|
|Control|Freezing and checkpointing processes|
|Injection|Packet tagging|

### Resources That Can Be Limited

- **Memory**
- **CPU**
- **Block IO**
- **Devices** (which devices can be used and whether new ones can be created)
- **Network**

### cgroups Structure

- cgroups are **hierarchically structured** — groups are created for a resource with a number.
- **Tasks are assigned to cgroups**.
- Each cgroup has a **resource limitation**.
- There is a **hierarchy for each resource**.
- When a process creates a child process, the **child stays in the same cgroup** (inherits parent's limitations).
    - Good for servers like NFS: receive request → fork child → child terminates when done → all children respect parent's resource limit.

### Docker cgroup Commands Example

```bash
# Limit CPU to 50%
docker run -d --name mycontainer --cpus="0.5" ubuntu sleep 1000

# Limit memory to 100MB
docker run -d --name mycontainer --memory=100m ubuntu sleep 1000

# Inspect cgroup values inside container
docker exec mycontainer cat /sys/fs/cgroup/cpu/cpu.cfs_quota_us
docker exec mycontainer cat /sys/fs/cgroup/memory/memory.limit_in_bytes

# Monitor stats
docker stats mycontainer
```

---

## Q&A — Containers, Namespaces, cgroups

**Q1: What is a Linux Container (LXC) and how does it differ from a VM?** A: LXC is an OS-level virtualization method that runs multiple isolated Linux systems on a single host sharing one Linux kernel. Unlike VMs, which virtualize hardware and require each VM to have its own full OS, containers share the host OS kernel (read-only) and only differ in their namespace and cgroup configurations. Containers are much lighter (megabytes vs. gigabytes) and start in seconds vs. minutes.

**Q2: What is a Linux namespace and what problem does it solve?** A: A Linux namespace wraps a global system resource in an abstraction so that processes within the namespace have their own isolated instance of that resource. It solves the problem of process isolation — different containers can have different views of system resources (process IDs, hostnames, filesystems, network interfaces) without interfering with each other.

**Q3: Explain the PID namespace.** A: The PID namespace isolates the process ID number space. Processes inside a container have their own PID 1 (init process) and are unaware of processes outside their namespace. Two containers can each have a process with PID 1 without conflict. From outside the container (on the host), these processes have different PIDs.

**Q4: What is the difference between MNT namespace and chroot?** A: The MNT (mount) namespace isolates the set of mount points — processes in different namespaces cannot see each other's files. It is similar to chroot but more comprehensive and secure. `chroot` only changes the root directory for the process, and can potentially be escaped. MNT namespace completely isolates the mount point tree.

**Q5: What are cgroups and what resources can they limit?** A: cgroups (Control Groups) is a Linux feature that limits, polices, and accounts for resource usage of process groups. They can limit: Memory, CPU, Block I/O, Devices (which can be used and whether new ones can be created), and Network bandwidth.

**Q6: How do namespaces and cgroups complement each other in container isolation?** A: Namespaces provide **visibility isolation** — they control what a process can SEE (which files, which network interfaces, which other processes, which hostname). cgroups provide **resource isolation** — they control how much a process can USE (CPU time, memory, I/O bandwidth). Together, they provide the complete isolation needed for containers: a container cannot see resources outside its namespace and cannot consume more than its cgroup-assigned resources.

**Q7: What is the relationship between child processes and cgroups?** A: When a process creates a child process, the child automatically stays in the same cgroup as its parent, inheriting the same resource limitations. This is useful for server-type applications (like NFS) that fork children to handle requests — all children are constrained by the same resource limits as the parent.

---

---

# 13. Docker — Architecture, Images, Containers, Registries, UnionFS

## What is Docker?

Docker is an **open platform tool** that makes it easier to **create, test, ship, deploy, and execute applications** using containers.

- Separates applications from infrastructure, enabling **faster deployment**.
- Packages and runs an application in a **loosely isolated environment** called a container.
- PaaS product using **OS-level virtualization** to deliver software packages.
- Provides isolation and security to run many containers on a single server/VM.
- Typical load: **8 to 18 containers** simultaneously on a single server/VM.

## Docker Benefits

- **Portability:** Build once, run anywhere (Linux, macOS, Windows, cloud).
- **Collaboration:** Share containers ensuring identical setups.
- **Resource efficiency:** Lighter than traditional VMs.
- **Consistency:** Eliminates "Works on my machine!" problems.

---

## Docker Architecture

Docker uses a **client-server architecture**:

```
Docker Client ─── REST API ──→ Docker Daemon (dockerd)
                                    │
                         ┌──────────┼──────────┐
                     Images    Containers   Networks/Volumes
                         │
                    Docker Registry (Docker Hub)
```

### Docker Daemon (`dockerd`)

- Listens for **Docker API requests**.
- Manages Docker objects: images, containers, networks, volumes.
- Can communicate with other daemons to manage Docker services.

### Docker Client (`docker`)

- Primary way users interact with Docker.
- Commands like `docker run` send instructions to `dockerd` via the Docker API.
- Can communicate with more than one daemon.
- **Docker Compose** is another Docker client — manages multi-container applications.

### Docker Host

- Has the Docker Daemon running.
- Can host a private registry OR connect to a public registry (Docker Hub).
- Responsible for Docker Objects: Images, Containers, Networks, Volumes, Plugins.

---

## Docker Objects

### Images

- A **read-only template** with instructions for creating a Docker container.
- Can be based on another image with additional customizations.
- Created using a **Dockerfile** — a script defining steps to build the image.
- Each instruction in a Dockerfile creates a **layer** in the image.
- Dockerfiles are distributed with software for users to build images themselves.

### Dockerfile Instructions (Key)

|Instruction|Purpose|
|---|---|
|`FROM`|Specifies the base image (e.g., `FROM nginx:alpine`)|
|`WORKDIR`|Sets the working directory inside the container|
|`RUN`|Executes commands during image build (installs packages, copies files)|
|`COPY`|Copies files from host to container|
|`EXPOSE`|Declares the port the container listens on|
|`ENV`|Sets environment variables|
|`CMD`|Defines the default command to run when the container starts|

### Image Layers

- Each Dockerfile instruction creates a **layer**.
- A layer is a **set of files and file metadata** packaged as an atomic unit.
- Docker treats each layer like an image ("intermediate image").
- Layers can be promoted to a full image by tagging.
- Most layers build upon a parent layer by applying **filesystem changes**.
- **Only incremental changes** are rebuilt when the Dockerfile changes — lightweight, small, fast.
- Layers are shared across images — efficiency through reuse.

### Docker Registries

- A **Docker registry** stores Docker images.
- **Docker Hub:** The default public registry — anyone can use and share images.
- Private registries can also be hosted.
- `docker pull` / `docker run` → images pulled from configured registry.
- `docker push` → image pushed to registry.

### Containers

- **Running instances of images** — the complete package needed to run the application.
- Also: a runnable instance of an image, or an execution environment (sandbox).
- Operations: create, start, stop, move, delete via Docker API or CLI.
- Can be connected to networks, have storage attached.
- **Well-isolated** from other containers and the host.
- Container processes cannot access non-shared objects of other containers.
- Container access is restricted to a **subset of host objects** using namespaces.
- When a container is removed, **unsaved state disappears** (unless stored in persistent storage/volumes).

---

## Docker and UnionFS

### Container Filesystem

- Programs inside containers **know nothing about image layers**.
- From inside the container, the filesystem appears as an exclusive copy of all files.
- This is made possible by **Union Filesystem (UFS)**.
- UFS works in conjunction with MNT namespaces and chroot.

### Union Filesystem

- A filesystem type that **creates an illusion of merging contents of several directories into one** (without modifying original sources).
- Operates by **creating layers** — lightweight and fast.
- Docker Engine uses UnionFS variants: **AUFS, overlay2, btrfs, vfs, DeviceMapper**.

### UnionFS Layering Example

```
/Fruits: Apple, Tomatoes
/Vegetables: Carrots, Tomatoes

mount -t unionfs -o dirs=/Fruits:/Vegetables none /mnt/healthy

/mnt/healthy contains: Apple, Tomatoes (from /Fruits — higher priority), Carrots
```

- As if `/Fruits` is layered on top of `/Vegetables`.
- **Copy-on-Write (CoW):** `-o cow` option on mount enables CoW.
    - If a change is made to a file → original is NOT modified.
    - New file is created in a hidden location.
    - If `/Fruits` is read-only (ro), changes are recorded in a temporary layer.

### Docker Layers and UnionFS

```
Top layer (R/W): Container
    ↑
Read-only layer: Image (e.g., App + config)
    ↑
Read-only layer: Base OS libraries
    ↑
First layer: Base image (ubuntu / busybox / scratch)
```

- First layer = **base image**.
- Read-only layers = **image layers** (each instruction in Dockerfile).
- The top **read-write layer = container**.
- Multiple containers can share the same read-only image layers — each gets its own R/W layer.

### Weaknesses of UnionFS

1. Different filesystems have different rules for file attributes, sizes, names — **translation issues**.
2. **Copy-on-write** makes implementing **memory-mapped files (mmap)** difficult.
3. Not appropriate for **long-lived data** or **sharing data between containers or container and host** — use Volumes instead.
4. Most write issues can be solved with **Docker Volumes**.

---

## Docker Networking

- Docker provides networking to let containers communicate with each other and the host.
- **Docker Network (`docker network create`)** — a software-defined network; containers communicate using service names (not IPs).
- `localhost` inside a container refers to the container's own network namespace, NOT the host machine.
- Use **container names** or **network DNS** to communicate between containers in the same Docker network.

---

## Docker Volumes

- Containers are **ephemeral** — data is lost when a container is stopped/removed.
- **Volumes** store data outside the container so it persists.
- Created with `docker volume create <name>`.
- Mounted into containers with `-v <volume-name>:/path/in/container`.

---

## Container Software Ecosystem

1. Docker
2. AWS Fargate
3. Google Kubernetes Engine (GKE)
4. Amazon ECS (Elastic Container Service)
5. LXC
6. Microsoft Azure Container Instances
7. Google Cloud Platform
8. CoreOS

---

## Q&A — Docker

**Q1: What is Docker and why is it useful?** A: Docker is an open platform for building, shipping, and running applications in containers. It is useful because containers package everything an application needs (code, runtime, libraries, configs) into a portable, isolated unit. This eliminates the "works on my machine" problem, speeds up deployment, reduces resource usage compared to VMs, and ensures consistency across development, testing, and production environments.

**Q2: Explain the Docker client-server architecture.** A: The Docker Client (`docker` command) communicates with the Docker Daemon (`dockerd`) via REST API over UNIX sockets or a network interface. The daemon performs all the heavy lifting — building images, running containers, managing networks and volumes. Docker Compose is another client that manages multi-container applications.

**Q3: What is the relationship between Dockerfile, Image, and Container?** A: A Dockerfile is a text-based blueprint of instructions to build an image. An Image is the built, read-only template created from the Dockerfile — it contains all application code, dependencies, and configuration in layered form. A Container is a running instance of an image — it adds a read-write layer on top of the image layers and executes the application.

**Q4: What is a Docker image layer and why are layers important?** A: Each instruction in a Dockerfile creates a separate layer in the image — a set of filesystem changes (files added, modified, or deleted). Layers are important because they are shared across images (reducing storage), only changed layers are rebuilt when the Dockerfile changes (speeding up builds), and they enable the copy-on-write behavior that keeps containers lightweight.

**Q5: What is a Union Filesystem (UnionFS) and how does Docker use it?** A: A UnionFS merges the contents of multiple directories (branches) into a single view without modifying the originals. Docker uses UnionFS (via overlay2, AUFS, etc.) to stack image layers. The base image is the lowest layer (read-only). Each Dockerfile instruction adds another read-only layer. The running container gets a thin read-write layer on top. The container sees all layers merged as a single coherent filesystem.

**Q6: What is Copy-on-Write in Docker?** A: In Copy-on-Write, when a process in a container modifies a file that exists in a read-only layer, a copy of that file is made into the container's read-write layer. The original in the read-only layer is never modified. The container then reads from its own modified copy. This allows many containers to share the same image layers while each having their own independent modifications.

**Q7: Why is `localhost` unreliable for inter-container communication?** A: `localhost` inside a Docker container refers to the container's own network namespace loopback interface, not the host machine or other containers. If Container A tries to connect to `localhost:3306` to reach Container B's MySQL, it will fail because there is no MySQL in Container A's network namespace. Instead, containers should be placed on the same Docker network and communicate using container names, which Docker's internal DNS resolves to the correct container IP.

**Q8: What is the purpose of Docker Volumes?** A: Containers are ephemeral — when a container is deleted, all data in its read-write layer is lost. Docker Volumes provide persistent storage outside the container. A volume is a managed directory on the host that is mounted into the container. When the container is deleted and recreated, data in the volume persists.

---

---

# 14. DevOps — Principles, Pipeline, CI/CD

## What is DevOps?

**DevOps** is a set of practices that **combines software development (Dev) and IT operations (Ops)** — two traditionally separate specialist functions with often conflicting goals.

DevOps aims to:

- **Shorten the System Development Life Cycle (SDLC)**.
- **Increase organizational ability to deliver, deploy, and support** applications and services at higher quality and speed.
- Automate processes between development and IT teams.

DevOps is **complementary with Agile** software development — many aspects came from Agile.

**DevOps follows the cycle: Plan → Build → Deploy → Operate.**

**Key tools:** Docker, Kubernetes, Git, Jenkins, Chef, Puppet.

---

## The Dev vs. Ops Conflict

|Development|Operations|
|---|---|
|"Put the current release live NOW!"|"What are the dependencies?"|
|"It works on my machine!"|"No machines available..."|
|"We need this yesterday"|"Which DB? High Availability? Scalability?"|

Cloud and automation bridge this gap. In the pre-cloud era, procuring machines required manual intervention. With cloud, automation is possible.

---

## DevOps Advantages

- **Faster Time to Market:** More frequent check-ins → more frequent builds and tests.
- **Better Code Quality/Release:** Frequent testing improves quality.
- **Integration Often:** Continuous integration.
- **Deploy Often:** Changes reach customers faster.
- **Automated:** Repeatable and faster processes.

---

## Principles of Continuous Delivery

1. **Principle 1:** Every build is a potential release.
2. **Principle 2:** Eliminate manual bottlenecks.
3. **Principle 3:** Automate wherever possible.
4. **Principle 4:** Have automated tests you can trust.

---

## Typical DevOps Pipeline for Containerized Applications

1. Developer **pushes code changes to Git**.
2. **Build system automatically builds** the current version and runs sanity tests.
3. If tests pass, **container image is published to the central container registry**.
4. Newly built container is **automatically deployed to a staging environment**.
5. Staging environment undergoes **automated acceptance tests**.
6. Verified container image is **deployed to production**.

---

## Continuous Development (CI)

**Continuous Integration (CI):** The automatic merging of code, requiring every module to compile (build) correctly and pass sanity tests against the mainline branch.

- Developers distributed across geographies need a **common view of code**.
- CI ensures merge conflicts and broken builds are detected immediately.
- **Tools:** Git, ClearCase, Maven, ANT, Jenkins, Travis CI, Drone.

---

## Continuous Testing

The process of executing **predominantly automated tests** as part of the delivery pipeline:

- Copy binaries to staging environment.
- Configure correctly, ensure dependencies are installed.
- Execute integration tests, functionality tests, performance tests, acceptance tests.
    - **Tools:** JUnit, Selenium.
- Generate reports.

---

## Continuous Delivery / Deployment (CD)

**Continuous Delivery:** Frequent shipping of sanitized builds to a given environment (test or production).

- **Jenkins** is a widely used CD tool.

**Continuous Deployment:** The automatic deployment of successful builds to production.

- Developers deploy by: pushing a button, merging a merge request, or pushing a Git release tag.

### Deployment Setup Tools

- **Chef/Puppet:** Specify infrastructure type, machine configuration.
- **Immutable Infrastructure:** Infrastructure that does not change after deployment — enables easier automation.
- **Docker:** Create images with all dependencies for each service.
- **Kubernetes:** Orchestrate containers, set scaling policies, setup load balancers.
- **Jenkins:** Workflow automation server.

---

## Jenkins

- **Most widely adopted CD tool**.
- Self-contained, open-source **automation server** for building, testing, and delivering software.
- Can automate build pipelines entirely.
- Can be installed via native packages, Docker, or standalone (requires JRE).
- **JenkinsX** is a dedicated project for running Jenkins in Kubernetes clusters.

---

## Q&A — DevOps

**Q1: What is DevOps and what problem does it solve?** A: DevOps is a set of practices combining software development and IT operations that traditionally had conflicting goals — developers wanted to ship features fast while operators wanted stability. DevOps solves this by automating the build, test, and deployment pipeline, enabling frequent, reliable releases. It shortens the SDLC and improves both the speed and quality of software delivery.

**Q2: What are the 4 Principles of Continuous Delivery?** A: (1) Every build is a potential release. (2) Eliminate manual bottlenecks. (3) Automate wherever possible. (4) Have automated tests you can trust.

**Q3: Describe a typical DevOps pipeline for a containerized application.** A: Developer pushes code to Git → Build system builds and runs sanity tests → If tests pass, container image is published to registry → Image is auto-deployed to staging → Automated acceptance tests run → Verified image is deployed to production.

**Q4: What is the difference between Continuous Delivery and Continuous Deployment?** A: Continuous Delivery ensures every build is potentially releasable and can be deployed to any environment on demand (often triggered manually). Continuous Deployment goes one step further — every successful build is automatically deployed to production without human intervention.

**Q5: What is Jenkins and what is its role in DevOps?** A: Jenkins is a self-contained, open-source automation server used to automate the CI/CD pipeline — building, testing, and deploying software. It serves as the workflow automation engine connecting all stages of the DevOps pipeline. JenkinsX extends it for Kubernetes environments.

---

---

# 15. Container Orchestration and Kubernetes

## What is Container Orchestration?

**Container orchestration** automates the **deployment, management, scaling, and networking** of containers across a compute cluster of multiple nodes.

A **container orchestrator** is centralized management software that allocates and schedules containers to run on a **pool of servers** (cluster).

**Orchestration vs. Scheduling:**

- **Orchestration:** Coordinating and sequencing different activities toward a common goal (like an orchestra conductor).
- **Scheduling:** Managing available resources and assigning/deploying workloads to where they can most efficiently run.

### Key Activities of Container Orchestration

- Provisioning and deployment
- Configuration and scheduling
- Resource allocation between containers and managing networking between them
- Container availability and redundancy management
- Scaling or removing containers based on workload
- Load balancing and traffic routing
- Monitoring container and host health
- Configuring applications based on the container they will run in
- Keeping container interactions secure
- Migrating containers between hosts for failure handling/resource management

---

## Immutable Infrastructure

A paradigm where **servers are never modified after deployment**. If something needs updating, **new servers built from a common image** with the appropriate changes replace the old ones.

**Benefits:**

- More consistency and reliability.
- Simpler, more predictable deployment process.
- Prevents configuration drift.
- Enables comprehensive deployment automation.
- Fast server provisioning in cloud.

---

## Kubernetes (K8s)

Kubernetes is the **most pervasive container orchestration platform**.

- Open-source container management (orchestration) tool.
- Originally developed by Google, donated to CNCF (Cloud Native Computing Foundation).
- Handles: container deployment, scaling/descaling, load balancing.
- Manages complex application deployments quickly, predictably, and reliably.
- Extends containerized application scaling to support truly immutable infrastructure.

---

## Kubernetes Building Blocks — Objects

### Basic K8s Objects

|Object|Description|
|---|---|
|**Pod**|A group of one or more containers|
|**ReplicaSet (RS)**|Ensures the desired number of pods is running|
|**Service**|Abstraction defining a logical set of pods + access policy + endpoint|
|**Deployment**|Declarative updates for pods in a ReplicaSet|
|**Volume**|Abstraction to persist data (containers are ephemeral)|
|**Namespace**|Segment of the cluster for a team or project (virtual sub-cluster)|
|**StatefulSet**|Manages stateful applications (e.g., databases)|
|**DaemonSet**|Ensures all/some worker nodes run a copy of a pod (e.g., Fluentd logging)|
|**Job**|Creates pods, runs a task to completion, then deletes the pods|

---

## Pod — In Depth

- A **Pod** is a single or group of containers that share:
    - Storage
    - Linux namespaces
    - cgroups
    - IP address and port space
    - Can communicate over `localhost` within the pod.
- Pods represent **microservices** that communicate with each other.
- Each pod is assigned an **IP address** accessible by other pods in the cluster.
- Applications within a pod have access to **shared volumes**.
- Containers in a pod are **co-located** and always **scheduled together**.
- Pods are **NOT intended to live long** — they are created, destroyed, and re-created on demand.

### Pod Lifecycle

```
Pending → Running → Succeeded / Failed
```

- **Pending:** Pod is accepted but container(s) not yet started.
    
- **Running:** At least one container is running.
    
- **Succeeded:** All containers terminated successfully.
    
- **Failed:** At least one container terminated with failure.
    
- A Pod is **scheduled once** and runs on that Node until it stops or is terminated.
    
- The kubelet can restart containers within a pod to handle faults.
    
- **Pods do NOT self-heal** — if a pod's node fails, the pod is deleted (the controller, e.g., Deployment, recreates it).
    

---

## Service — In Depth

- Pods in Kubernetes are **constantly created and destroyed**, causing their IP addresses to change.
- This creates **discoverability issues** — how does the frontend know which pod's IP to use?
- **Service:** An abstraction over a number of pods with a **stable Virtual IP address (VIP)**.
    - Other services communicate via the VIP.
    - The service tracks IP address changes of pods.
    - Provides **load balancing** across pods.
- A Kubernetes Service is a **logical collection of pods** in a cluster.

---

## Kubernetes Architecture

### Cluster Structure

```
         Master Node (Control Plane)
+---------------------------------------------+
| kube-apiserver | etcd | kube-scheduler      |
| kube-controller-manager                     |
+---------------------------------------------+
         ↕ (API calls)
+----------+  +----------+  +----------+
| Worker   |  | Worker   |  | Worker   |
| Node 1   |  | Node 2   |  | Node 3   |
+----------+  +----------+  +----------+
| Kubelet  |  | Kubelet  |  | Kubelet  |
| kube-proxy| | kube-proxy| | kube-proxy|
| Container|  | Container|  | Container|
| Runtime  |  | Runtime  |  | Runtime  |
+----------+  +----------+  +----------+
```

### Master Node Components

|Component|Role|
|---|---|
|**kube-apiserver**|Exposes the Kubernetes API — entry point for all REST commands to control the cluster|
|**etcd**|Key-value store for all cluster data (jobs, pods, services, namespaces, replication info)|
|**kube-scheduler**|Knows available resources; schedules new pods on worker nodes|
|**kube-controller-manager**|Runs controllers that watch cluster state via apiserver and make corrective changes|

**Example Controller:** The Replication Controller maintains the desired number of pods in the system.

### Worker Node Components

|Component|Role|
|---|---|
|**Kubelet**|Agent that gets pod configuration from apiserver and ensures described containers are running|
|**kube-proxy**|Network proxy and load balancer on each worker node; handles TCP/UDP routing|
|**Container Runtime**|Executes containers (e.g., Docker, containerd)|

---

## Kubernetes Functioning

- You describe the desired application configuration using a **JSON or YAML file**.
- The file specifies: container images, network configuration, log storage.
- When deploying, Kubernetes **automatically schedules** the container to the right host based on defined requirements and constraints.
- The orchestration tool manages the **container lifecycle** based on the compose file specifications.

---

## Kubernetes Benefits

|Benefit|Description|
|---|---|
|Horizontal scaling|Scale up/down from command line or UI|
|Automated rollouts and rollbacks|Roll out changes while monitoring health; auto-rollback on failure|
|Service discovery and load balancing|Expose containers via DNS or IP; balance traffic|
|Storage orchestration|Automatically mount local, cloud, or network storage|
|Secret and configuration management|Store passwords, tokens, SSH keys without rebuilding images|
|Self-healing|Restart failed containers, replace/reschedule when nodes die, health checks|
|Batch execution|Manage batch and CI workloads, replace failed containers|
|Automatic binpacking|Bin-pack containers into hosts based on CPU/memory requirements|

---

## Container Orchestration Tools

|Tool|Description|
|---|---|
|**Docker Swarm**|Native Docker clustering — turns a group of Docker engines into a single virtual Docker engine|
|**Google Container Engine (GKE)**|Built on Kubernetes; runs Docker containers on Google Cloud|
|**Kubernetes**|Open-source orchestration system for Docker containers; handles scheduling and workloads|
|**Amazon ECS**|Supports Docker; runs applications on managed clusters of Amazon EC2 instances|

---

## Where Orchestration Fits in the System Stack

Container Orchestration mediates between apps/services and container runtimes:

- **Service Management:** Labels (key-value pairs for pod identification), groups, namespaces, dependencies, load balancing, readiness checks.
- **Scheduling:** Allocation, replication, container resurrection, rescheduling, rolling deployment, upgrades/downgrades.
- **Resource Management:** Memory, CPU, GPU, volumes, ports, IPs.

---

## Q&A — Kubernetes and Orchestration

**Q1: What is container orchestration and why is it needed?** A: Container orchestration is the automated deployment, management, scaling, and networking of containers across a cluster of machines. It is needed because manually managing many containers across many hosts is complex and error-prone. Orchestration handles service discovery, load balancing, health monitoring, auto-scaling, rolling updates, and node failure recovery automatically.

**Q2: What is a Pod in Kubernetes?** A: A Pod is the smallest deployable unit in Kubernetes — a group of one or more containers that share the same network namespace (IP, ports), storage (volumes), and Linux namespaces. Containers in a pod communicate via localhost. Pods represent tightly coupled microservices that always run together.

**Q3: What is the role of a Service in Kubernetes?** A: A Service provides a stable, virtual IP address (VIP) in front of a dynamic set of pods. Since pods are ephemeral and their IPs change, the Service gives a consistent endpoint. It performs load balancing across all healthy pods that match its selector. Other services and external clients communicate with the Service's VIP, not directly with individual pod IPs.

**Q4: What are the components of the Kubernetes Master Node?** A: kube-apiserver (exposes the K8s API, entry point for all cluster control), etcd (distributed key-value store for all cluster state data), kube-scheduler (schedules pods to worker nodes based on resource availability), and kube-controller-manager (runs controllers like the Replication Controller to maintain desired cluster state).

**Q5: What is the difference between a ReplicaSet and a Deployment in Kubernetes?** A: A ReplicaSet ensures a specified number of pod replicas are running at any given time. A Deployment is a higher-level abstraction that manages ReplicaSets, providing declarative updates (rolling updates, rollbacks). You typically use Deployments, which manage ReplicaSets internally.

**Q6: What is etcd and why is it critical?** A: etcd is a distributed, reliable key-value store used by Kubernetes to store all cluster state data — job information, pod/service details and state, namespaces, replication information. It is critical because it is the single source of truth for the entire cluster. If etcd is lost without backup, the entire cluster state is lost.

**Q7: What does "self-healing" mean in Kubernetes?** A: Self-healing in Kubernetes means the platform automatically recovers from failures: it restarts failed containers, replaces and reschedules containers when nodes fail, kills containers that don't respond to user-defined health checks, and does not advertise containers to clients until they are ready. The desired state (defined in manifests) is continuously reconciled against actual state.

**Q8: What is Docker Swarm and how does it compare to Kubernetes?** A: Docker Swarm is Docker's native clustering solution — it joins multiple Docker hosts into a single virtual Docker engine, allowing containers to be deployed across a cluster. It is simpler to set up and use than Kubernetes but less feature-rich. Kubernetes is more complex but offers advanced scheduling, self-healing, auto-scaling, and a richer ecosystem.

---

---

# Quick Reference Summary Tables

## Virtualization Techniques Comparison

|Technique|Guest OS Modification|Instruction Handling|Performance|Architecture Support|Examples|
|---|---|---|---|---|---|
|Trap & Emulate|No|At runtime, hardware trap|Medium|Architectures where sensitive ⊆ privileged|Classic VMMs|
|Binary Translation|No|At runtime, proactive rewrite|Medium (translation overhead)|x86 (workaround for 17 instructions)|VMware Workstation|
|Paravirtualization|Yes (hypercalls)|At compile time|Higher|x86 (with modified OS)|Xen, VMware ESX|
|Hardware-Assisted (VT-x)|No|Hardware VMX modes|High|x86-64 (modern, post-2005)|KVM, Xen (modern)|

## VM Migration Comparison

|Type|VM State During Migration|Downtime|Memory Transfer|Risk|
|---|---|---|---|---|
|Cold|Powered off|Entire migration|Once|Low|
|Non-live|Paused|Entire migration|Once|Low|
|Pre-copy Live|Running|Short (final dirty pages)|Iteratively|Low|
|Post-copy Live|Running at destination|Very short (CPU state only)|On-demand|Higher (page faults)|

## Namespace Types in Docker

|Namespace|Isolates|Key Feature|
|---|---|---|
|PID|Process IDs|Each container has its own PID 1|
|UTS|Hostname, NIS domain|Each container has its own hostname/FQDN|
|MNT|Mount points, filesystem|Each container sees a subset of the filesystem|
|IPC|Shared memory|Isolated inter-process communication|
|NET|Network interfaces, IPs, ports|Each container has its own network stack|
|USR|User IDs and names|UID isolation|

## Kubernetes Master vs. Worker

|Master Node|Worker Node|
|---|---|
|kube-apiserver|Kubelet|
|etcd|kube-proxy|
|kube-scheduler|Container Runtime (Docker)|
|kube-controller-manager||

---

---

> **Final Exam Tip:** Focus heavily on: (1) Trap & Emulate vs. Binary Translation vs. Paravirtualization differences; (2) Shadow Page Tables vs. EPT; (3) Pre-copy vs. Post-copy live migration; (4) Namespaces vs. cgroups; (5) Popek & Goldberg theorems; (6) Kubernetes architecture. These are the most commonly examined conceptual topics.

---

_Notes prepared from CC_U2_Merged.pdf | Cloud Computing Unit 2 | Dr. Prafullata Kiran Auradkar, Department of Computer Science and Engineering_