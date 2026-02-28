# ☁️ Cloud Computing — Unit 2: Complete Study Notes
### *Virtualization, Containers, Docker, DevOps, and Kubernetes*
> Based on lecture slides by Dr. Prafullata Kiran Auradkar, Dept. of CSE

---

# 📚 Table of Contents

1. [What is Virtualization?](#1-what-is-virtualization)
2. [Types of Hypervisors (VMM)](#2-types-of-hypervisors-vmm)
3. [Paravirtualization vs Full (Transparent) Virtualization](#3-paravirtualization-vs-full-transparent-virtualization)
4. [Virtualization Software Techniques: Trap & Emulate and Binary Translation](#4-virtualization-software-techniques-trap--emulate-and-binary-translation)
5. [Hardware-Assisted Virtualization (Intel VT-x / AMD-V)](#5-hardware-assisted-virtualization-intel-vt-x--amd-v)
6. [Memory Virtualization (Shadow Page Tables & EPT)](#6-memory-virtualization-shadow-page-tables--ept)
7. [I/O Virtualization](#7-io-virtualization)
8. [Popek and Goldberg Principles](#8-popek-and-goldberg-principles)
9. [VM Migration](#9-vm-migration)
10. [Containers, Namespaces, and cgroups](#10-containers-namespaces-and-cgroups)
11. [Docker — Architecture, Images, Containers, Registries](#11-docker--architecture-images-containers-registries)
12. [Union Filesystem (UnionFS)](#12-union-filesystem-unionfs)
13. [DevOps and CI/CD](#13-devops-and-cicd)
14. [Container Orchestration and Kubernetes](#14-container-orchestration-and-kubernetes)
15. [Microservices Architecture](#15-microservices-architecture)

---

# 1. What is Virtualization?

## 1.1 Definition

**Virtualization** is the technique of **abstracting physical resources** — compute (CPU), memory, storage, and networking — into a **logical (virtual) view**. Each application or virtual environment sees its *own* isolated set of resources, independent of the physical hardware below it.

Think of it this way: before virtualization, one physical server runs one OS which runs applications directly on hardware. After virtualization, **one physical server can host many independent virtual machines (VMs)**, each with its own OS and set of resources. They are fully isolated from each other.

## 1.2 Why Virtualization? (The Problem it Solves)

Before virtualization, the traditional **bare-metal** setup had serious problems:

| Problem | What it Means |
|---|---|
| One OS per machine | Each server wasted most of its compute capacity |
| Hardware-software coupling | An app tightly tied to specific hardware |
| Conflicts | Multiple applications on the same machine would conflict |
| Underutilized resources | CPU/memory sitting idle most of the time |
| Poor fault isolation | One app crash could bring down the whole system |

## 1.3 Benefits of Virtualization

After virtualization, the scenario improves dramatically:

- **Server consolidation** — Run multiple applications in isolation on the same physical server, increasing hardware utilization.
- **Workload mobility** — Move applications from one server to another without downtime.
- **OS diversity** — Run both Linux and Windows on the same physical hardware simultaneously.
- **Security / Isolation** — The hypervisor separates VMs from each other and from hardware.
- **High Availability / Load Balancing** — Live-migrate a VM to another physical server instantly.
- **Rapid provisioning** — Spin up virtual resources on demand.
- **Encapsulation** — An entire application execution environment (OS + apps) is packaged inside a VM.
- **Lower costs** — Better efficiency, flexibility, and responsiveness.

## 1.4 The Virtual Machine (VM)

A **Virtual Machine (VM)** is the fundamental unit of compute virtualization. It is:

> "Logically identical to a physical machine, with the ability to run an OS."

Each VM believes it has its own CPU, memory, disk, and network card — but these are all *virtual*, backed by shared physical hardware.

## 1.5 What is a VMM / Hypervisor?

A **Virtual Machine Monitor (VMM)** — also called a **Hypervisor** — is the layer of software that:

- Creates and manages virtual machines
- Intercepts VM access to hardware
- Provides the illusion of real hardware to each VM
- Ensures strong isolation between VMs

Three key requirements for a VMM (from Goldberg's original 1974 definition):

1. **Equivalence** — Programs in the VM run essentially identically to bare metal.
2. **Performance** — Only minor slowdown at worst.
3. **Resource Control** — VMM is in complete control of system resources.

---

## ❓ Q&A: Virtualization Basics

**Q1. What is virtualization and why is it important for cloud computing?**
> Virtualization is the abstraction of physical resources (CPU, memory, storage, network) into logical, isolated environments called virtual machines. It is fundamental to cloud computing because it enables one physical server to be shared among many tenants, allows on-demand resource provisioning, improves utilization, and enables features like live migration.

**Q2. What is the difference between a VM and a physical machine?**
> A physical machine (bare metal) runs one OS that directly accesses hardware. A VM is a software-simulated machine that runs its own OS on top of a hypervisor. The hypervisor multiplexes physical hardware among multiple VMs, giving each the illusion of dedicated hardware.

**Q3. What are the three requirements for a VMM according to classical definitions?**
> (1) Equivalence: VMs behave as if they are running on real hardware. (2) Efficiency: Most instructions execute directly, with minimal overhead. (3) Resource Control: The VMM is the sole authority over physical resources.

**Q4. List five benefits of virtualization.**
> Server consolidation, workload mobility, OS diversity, security isolation, and rapid resource provisioning.

**Q5. What problem does VM encapsulation solve?**
> Encapsulation means the entire execution state of an application (OS + app + libraries + configuration) is stored within a VM. This solves the "it works on my machine" problem — the VM can be copied, moved, or restored with all its state intact.

---

# 2. Types of Hypervisors (VMM)

## 2.1 Type 1 Hypervisor — Bare Metal VMM

```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Guest VM │ │ Guest VM │ │ Guest VM │
└────┬─────┘ └────┬─────┘ └────┬─────┘
     └──────────┬──────────────┘
           ┌────┴─────┐
           │   VMM    │   ← runs directly on hardware
           └────┬─────┘
           ┌────┴─────┐
           │ Hardware │
           └──────────┘
```

**Type 1 Hypervisor** (also called a **Bare Metal Hypervisor**) runs **directly on the physical hardware** — there is no host OS in between. The VMM *is* the OS for the hardware.

- Has direct access to hardware (CPU, memory, I/O)
- Provides the strongest isolation and best performance
- Used in enterprise and cloud data centers

**Examples:** VMware ESX Server, Xen, IBM CP/CMS, Microsoft Hyper-V (2008+)

## 2.2 Type 2 Hypervisor — Hosted VMM

```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Guest VM │ │ Guest VM │ │ Guest VM │
└────┬─────┘ └────┬─────┘ └────┬─────┘
     └──────────┬──────────────┘
           ┌────┴─────┐
           │   VMM    │   ← runs as an app on top of Host OS
           └────┬─────┘
           ┌────┴─────┐
           │  Host OS │
           └────┬─────┘
           ┌────┴─────┐
           │ Hardware │
           └──────────┘
```

**Type 2 Hypervisor** (also called a **Hosted Hypervisor**) runs **on top of an existing host OS**, as a regular application. The host OS manages hardware; the VMM adds virtualization on top.

- Easier to install and use (just install an application)
- Suitable for development/testing on personal machines
- Slightly slower due to extra software layer

**Examples:** VMware Workstation, Oracle VirtualBox

## 2.3 Hybrid Hypervisor

```
Host Guest  Guest
 VM    VM    VM
 │     │     │
VMM   VMM   VMM
 └──┬──────┘
Host OS
 └──┬──┘
Hardware
```

**Hybrid Hypervisor** is a combination — it has both bare-metal and hosted components. The main VMM runs partially on the OS and partially directly on hardware.

**Examples:** Microsoft Virtual Server, Microsoft Virtual PC, KVM (Kernel-based VM integrated into Linux kernel)

## 2.4 Key Differences Summary

| Feature | Type 1 (Bare Metal) | Type 2 (Hosted) |
|---|---|---|
| Runs on | Hardware directly | Host OS |
| Performance | Higher | Slightly lower |
| Security isolation | Stronger | Weaker |
| Use case | Data centers, production | Dev/test, personal use |
| Examples | ESX, Xen, Hyper-V | VirtualBox, VMware Workstation |

---

## ❓ Q&A: Hypervisors

**Q1. What is the fundamental difference between a Type 1 and Type 2 hypervisor?**
> A Type 1 hypervisor runs directly on hardware (bare metal) with no underlying OS. A Type 2 hypervisor runs as an application on top of a host OS, which in turn runs on hardware.

**Q2. Why is a Type 1 hypervisor generally preferred in production/cloud environments?**
> Because it eliminates the overhead of a host OS, giving the hypervisor direct access to hardware. This results in better performance, stronger isolation, and more efficient resource management.

**Q3. What is KVM, and which category of hypervisor does it belong to?**
> KVM (Kernel-based Virtual Machine) is a Linux feature where hypervisor functionality is built into the Linux kernel itself. It can be considered a hybrid/Type 1 because Linux (the host OS) is the hypervisor, and memory/scheduling are handled by the kernel directly.

**Q4. Can a Type 2 hypervisor be used for enterprise cloud deployments? Why or why not?**
> While technically possible, it is not preferred because of the additional overhead from the host OS layer, weaker hardware access, and reduced isolation. Enterprise environments use Type 1 hypervisors for performance and security.

**Q5. Give two examples each of Type 1 and Type 2 hypervisors.**
> Type 1: VMware ESX Server, Xen. Type 2: Oracle VirtualBox, VMware Workstation.

---

# 3. Paravirtualization vs Full (Transparent) Virtualization

## 3.1 Background: The Privilege Ring Problem

Modern CPUs operate in **privilege rings**:

```
Ring 0 — Kernel/OS (most privileged)
Ring 1 — Device Drivers
Ring 2 — Device Drivers
Ring 3 — User Applications (least privileged)
```

When you add a hypervisor into the picture, there's a conflict:
- The hypervisor needs **Ring 0** (the most privileged).
- But the Guest OS also expects to run in Ring 0.

The solution: push the Guest OS to **Ring 1** and keep the hypervisor at Ring 0.

Now the Guest OS at Ring 1 tries to execute **privileged instructions** (instructions that can only run at Ring 0). Two strategies handle this differently:

## 3.2 Full (Transparent) Virtualization

In **Full Virtualization**, the hypervisor **fully simulates** the underlying hardware. The Guest OS runs **unmodified** — it doesn't know it's inside a VM.

- Guest OS believes it's running directly on hardware
- Hypervisor intercepts and **emulates** privileged instructions at runtime
- Technique used: **Trap-and-Emulate** + **Binary Translation** (for x86)

**Advantage:** The guest OS doesn't need any modification.
**Disadvantage:** Emulation has performance overhead; binary translation adds complexity.

**Examples:** Oracle VirtualBox, VMware Workstation (early versions)

## 3.3 Paravirtualization

In **Paravirtualization**, the **guest OS is modified** to be aware that it's running inside a VM. Instead of executing privileged instructions directly, it makes **hypercalls** to the hypervisor.

> A **hypercall** is like a system call, but instead of going from user space to OS (kernel), it goes from the Guest OS to the Hypervisor.

- Guest OS is **explicitly ported** to run on top of the hypervisor
- Requires access to the **source code** of the Guest OS
- Replaces privileged instructions with hypercalls *at compile time*

**Advantage:** Better performance, fewer traps needed.
**Disadvantage:** Guest OS must be modified; not compatible with proprietary or closed-source OSes.

**Examples:** Xen hypervisor, IBM MVS/VM, KVM (with hardware assist)

## 3.4 Paravirtualization in Detail: How Hypercalls Work

Consider the Guest OS running in Ring 1 and trying to write to page tables (a privileged action):

```
Normal: Guest OS → executes privileged instruction → TRAP → VMM handles
Paravirt: Guest OS → calls hypercall API → VMM handles directly
```

At the **compiler level**, when building the modified Guest OS for paravirtualization, the compiler replaces all privileged instructions like `invlpg`, `hlt`, `iret` with calls into the hypervisor's API.

**Xen example — Page Table handling:**
1. Guest OS wants to write to guest page tables (read-only).
2. Write attempt generates a trap.
3. Xen temporarily makes that page writable.
4. Guest modifies entries freely.
5. Guest calls a Xen API to signal completion.
6. Xen validates and updates the physical page table.

This hypercall approach drastically reduces traps:
- **Pure trap-and-emulate**: 250,000 traps for a 1GB memory allocation (4K pages).
- **Paravirtualization with hypercalls**: Only ~1,000 traps (one per page table page, not per entry).

## 3.5 Compile-Time vs Runtime Handling

| Feature | Full Virtualization | Paravirtualization |
|---|---|---|
| Privileged instruction handling | Intercepted at **runtime** by hypervisor | Replaced with hypercalls at **compile time** |
| Guest OS modification | **None required** | **Required** |
| Performance | Lower (runtime overhead) | Higher (fewer traps) |
| Technique | Trap-and-emulate + Binary Translation | Hypercalls |

## 3.6 KVM (Kernel-Based Virtual Machine)

KVM is a **Linux para-virtualization system** integrated into the Linux kernel (version 2.6.20+).

- Memory management and scheduling done by existing Linux kernel
- KVM handles the rest → simpler than a standalone hypervisor
- **Hardware-assisted**: Uses Intel VT-x / AMD-V to support even unmodified guest OSes like Windows, Solaris, etc.

## 3.7 VMware ESX Server (Para-virtualization variant)

VMware ESX uses a **para-virtualization architecture** where the VM kernel interacts **directly with hardware** without the host OS.

Its four components:
1. **Virtualization Layer** — Virtualizes CPU, memory, network, disk; each VM gets its own virtual hardware set.
2. **Resource Manager** — Allocates CPU, memory, disk, network bandwidth.
3. **Hardware Interface Components** — Device drivers and VMware ESX File System.
4. **Service Console** — Boots system, initializes VMM and resource manager, supports system admins.

---

## ❓ Q&A: Paravirtualization vs Full Virtualization

**Q1. What is the key difference between full virtualization and paravirtualization?**
> Full virtualization provides complete hardware simulation; the guest OS runs unmodified. Paravirtualization modifies the guest OS to replace privileged instructions with hypercalls, achieving better performance.

**Q2. What is a hypercall?**
> A hypercall is a privileged function call from the guest OS directly to the hypervisor, analogous to a system call from a user process to the OS. It replaces the need to trap on privileged instructions, reducing overhead.

**Q3. Why does paravirtualization require source code access to the guest OS?**
> Because the guest OS kernel must be modified to replace privileged/sensitive instructions with hypercall equivalents. Without source code, this compilation-time change cannot be made.

**Q4. Why does Xen use paravirtualization?**
> Xen uses paravirtualization to avoid the high overhead of runtime binary translation for privileged instructions. By having the guest OS modified to use hypercalls, it achieves near-native performance.

**Q5. Why is paravirtualization "not strictly compatible" with all OSes?**
> Because it requires that the guest OS source code be modified and recompiled. Proprietary operating systems (e.g., Windows in older days) or OSes whose source is unavailable cannot be para-virtualized directly.

**Q6. How many traps does full virtualization generate vs paravirtualization for a 1GB memory mapping (4K pages)?**
> Full virtualization: 250,000 traps (one per page table entry). Paravirtualization (Xen): ~1,000 traps (one per page-table page of 4MB/4K = 1000 pages).

---

# 4. Virtualization Software Techniques: Trap & Emulate and Binary Translation

## 4.1 Direct Execution (Baseline)

The most basic idea: run most guest VM instructions **directly on the hardware**. This gives near-native performance but has a problem — if all instructions run directly, the VMM has no control over the VM. A misbehaving or malicious VM could take over the system.

## 4.2 Trap and Emulate

**Idea:**
- Run the VM in **user mode (Ring 3)**, VMM in **privileged mode (Ring 0)**.
- When the VM tries to execute a **system/privileged instruction** (one that modifies hardware state, like `iret`, `hlt`, `invlpg`, or accessing control registers like CR3), the CPU **traps** — control jumps to the VMM.
- The VMM then **emulates** the intended effect of the instruction safely.

```
Ring 3 → VM executes normal instructions (add, load, store)
Ring 3 → VM tries privileged instruction → TRAP → Ring 0 (VMM)
Ring 0 → VMM emulates instruction safely → returns to Ring 3 (VM)
```

### Trap flow for system calls:

1. Guest application issues `int n` (system call trap).
2. Traps to VMM.
3. VMM doesn't understand it — jumps to Guest OS trap handler.
4. Guest OS handles the system call normally.
5. Guest OS executes `iret` (return from trap) — privileged.
6. Traps to VMM.
7. VMM jumps to the correct user process.

## 4.3 Problems with Trap and Emulate on x86

The classic x86 architecture has a **critical flaw** for virtualization:

**Not all sensitive instructions trap when executed in user mode!**

In x86, two categories of instructions:
- **Privileged instructions** — trap if executed in Ring 3 (e.g., `hlt`, `iret`, `invlpg`).
- **Sensitive instructions** — access or affect hardware state, but **do NOT always trap** in lower privilege levels.

Two subcategories of sensitive instructions:

| Type | Description | Example |
|---|---|---|
| **Control-sensitive** | Try to change the configuration of resources (system registers) | `PUSHF`, `POPF`, `SGDT`, `SIDT`, `SLDT`, `SMSW` |
| **Behavior-sensitive** | Behavior depends on current privilege mode | `POP`, `PUSH`, `CALL`, `JMP`, `INT`, `RET`, `MOV` |

**The 17 non-virtualizable x86 instructions:**

> `SGDT, SIDT, SLDT, SMSW, PUSHF, POPF, LAR, LSL, VERR, VERW, POP, PUSH, CALL, JMP, INT, RET, STR, MOV`

### Famous example: `popf` instruction

- In **Ring 0**: `popf` can set bit 9 of `%eflags` (the interrupt flag) — controls whether interrupts are enabled/disabled.
- In **Ring 3**: `popf` silently ignores the attempt to change the interrupt flag. **No trap is generated!**

This is disastrous for virtualization: the Guest OS might try to disable interrupts (a sensitive operation), but because no trap is generated, the VMM is never notified.

### Famous example: `pushf` instruction

- Pushes the `%eflags` register onto the stack, including privilege level bits.
- If the Guest OS (in Ring 1 or 3) executes `pushf`, it can **see** that its current privilege level is not Ring 0 — it can detect it's inside a VM!

### Summary of Problems:
1. **Performance Overhead** — Trap costs are high for frequent operations.
2. **Not all architectures support trap-and-emulate** — x86 pre-2005 did not meet Popek-Goldberg requirements.
3. **17 sensitive-but-unprivileged instructions** — behave incorrectly without generating a trap.
4. **Guest OS detects it's in a VM** — via privilege bits exposed in registers.
5. **Memory management complexity** — Two levels of address translation needed.

## 4.4 Binary Translation

**Binary Translation** is the solution to the "17 non-virtualizable instructions" problem on x86, developed primarily by VMware.

**How it works:**
1. The hypervisor **examines the guest's binary code** (machine instructions) before execution.
2. It finds **sensitive but unprivileged instructions** that would not trap properly.
3. It **replaces them** with safe equivalents (either a trap to the hypervisor, or safe inline code).
4. The translated code is stored in a **translation cache** for reuse.
5. Translated code is then executed, achieving correct and safe behavior.

**Three translation types:**

| Type | Description | Example |
|---|---|---|
| **Ident (identity)** | Safe instruction, copied unchanged | `add`, `mov` between general-purpose registers |
| **Inline** | Simple dangerous instruction translated into short emulation code, placed inline | Modifying interrupt enable flag (`popf`) |
| **Call-out** | Complex dangerous instruction — control passed to VMM emulation code | Changing page table base register (`CR3`) |

**Binary Translation is done on basic blocks:**
- A **basic block** is a sequence of instructions with no branches (a straight-line section).
- The monitor inspects one basic block at a time, translates it, and stores in cache.
- When a branch is encountered, the next basic block is translated.

**Advantage:** Supports full virtualization of x86 without hardware extensions. Works with **unmodified guest OSes**.

**Disadvantage:** Translation overhead; control flow changes need careful handling (branch addresses can shift after translation).

## 4.5 Strictly Virtualizable

A processor (or mode) is **strictly virtualizable** if, when executed at lower privilege:
- All instructions that access privileged state **trap**.
- All instructions either trap or **execute identically** (same behavior).

The **x86 ISA (pre-2005)** was **NOT strictly virtualizable** because of the 17 instructions that neither trapped nor behaved identically.

---

## ❓ Q&A: Trap & Emulate and Binary Translation

**Q1. Explain the trap-and-emulate technique in detail.**
> The guest VM runs in user mode (Ring 3). When it executes a sensitive/privileged instruction, the CPU generates a trap, transferring control to the VMM at Ring 0. The VMM inspects the instruction, emulates its intended effect safely (e.g., updating a virtual control register instead of the real one), and returns control to the guest VM. This allows the guest to believe its privileged instruction executed normally.

**Q2. Why is `popf` a problem for trap-and-emulate virtualization on x86?**
> In Ring 0, `popf` can modify the interrupt enable flag (bit 9 of eflags). But in Ring 3, the CPU silently ignores this modification without generating a trap. So if the Guest OS (placed in Ring 3) tries to disable interrupts using `popf`, the VMM is never notified, and the guest's interrupt state is silently wrong.

**Q3. What is the translation cache in binary translation?**
> It is a memory area where the hypervisor stores already-translated code blocks. When a basic block is translated (sensitive instructions replaced with safe ones), the result is cached. On re-execution of the same code, the hypervisor uses the cached version, avoiding repeated translation overhead.

**Q4. How many non-virtualizable instructions existed in the original x86 ISA, and what were they?**
> 17 instructions: SGDT, SIDT, SLDT, SMSW, PUSHF, POPF, LAR, LSL, VERR, VERW, POP, PUSH, CALL, JMP, INT, RET, STR, MOV. These were sensitive but not privileged — they would not trap when run in unprivileged mode.

**Q5. What does "strictly virtualizable" mean?**
> A processor is strictly virtualizable if, at any privilege level, every instruction either (a) traps when it accesses privileged state, or (b) behaves identically regardless of the current privilege level. x86 pre-2005 was NOT strictly virtualizable due to those 17 instructions.

**Q6. What advantage does binary translation have over paravirtualization?**
> Binary translation does not require modification of the guest OS. It works with unmodified OSes (including Windows), while paravirtualization requires access to the OS source code and modification of the kernel.

---

# 5. Hardware-Assisted Virtualization (Intel VT-x / AMD-V)

## 5.1 Motivation

Both trap-and-emulate (limited by non-virtualizable instructions) and binary translation (performance overhead) are software workarounds. The **ideal solution** is to fix the hardware itself.

Intel introduced **VT-x (Virtualization Technology for x86)** and AMD introduced **AMD-V** around 2005-2006 to make x86 hardware properly virtualizable.

## 5.2 The Core Idea: Two Modes of Operation

Intel VT-x introduces a completely new mode called **VMX (Virtual Machine Extensions)** with two sub-modes:

```
┌─────────────────────────────────────────┐
│              VMX Root Mode              │
│  Ring 0, 1, 2, 3 — Hypervisor lives here│
└─────────────────────────────────────────┘
┌─────────────────────────────────────────┐
│            VMX Non-Root Mode            │
│  Ring 0, 1, 2, 3 — Guest VMs live here  │
└─────────────────────────────────────────┘
```

- **VMX Root Mode**: The hypervisor (VMM) runs here. Full, unrestricted hardware access.
- **VMX Non-Root Mode**: Guest VMs run here. Even Ring 0 in non-root mode is restricted.

This is the crucial innovation: **the Guest OS now runs in Ring 0 of the non-root mode**, solving the "Guest OS needs Ring 0" problem. The Guest OS no longer needs to be demoted to Ring 1.

## 5.3 VM Entry and VM Exit

| Operation | Direction | Description |
|---|---|---|
| **VM Entry** | Root → Non-Root | Hypervisor launches or resumes a VM. Uses `vmlaunch` / `vmresume` instructions. |
| **VM Exit** | Non-Root → Root | Certain events (sensitive instructions, interrupts) automatically return control to hypervisor. |

**VM Exit triggers include:**
- Executing sensitive instructions in non-root mode
- I/O operations
- Certain memory accesses
- External interrupts
- Explicit `vmcall` instruction

## 5.4 VMCS — Virtual Machine Control Structure

The **VMCS** is a hardware data structure (a block of memory) that controls the behavior of the VM:

- Stores guest and host processor state
- Defines which events cause VM exits
- The hypervisor reads/writes the VMCS using `vmread` and `vmwrite` instructions

**KVM's operational flow using VT-x:**

```
1. VMM initializes/modifies VMCS
2. VMM executes vmlaunch/vmresume → VM enters non-root mode
3. VM runs until a VM exit condition is met
4. VM exit occurs — control returns to VMM
5. VMM analyzes exit reason, takes appropriate action
6. VMM executes vmresume → VM continues
```

## 5.5 How Hardware Assist Solves the 17 Instructions Problem

With VT-x, the Guest OS runs in **Ring 0 of the non-root mode**. Hardware is modified so that those previously non-trapping sensitive instructions NOW either:

- Execute safely on duplicated hardware state (each mode has its own copy of critical registers), OR
- Generate a **VM Exit** to the hypervisor

Either way, the VMM retains control. No software binary translation needed for these instructions.

## 5.6 Xen Architecture (revisited with hardware assist)

Xen is fundamentally paravirtualization, but modern Xen uses hardware assist too:

- **VMs called domains** — the management domain is **Dom0**, based on Linux.
- Dom0 handles all I/O.
- The Xen hypervisor handles all other functions (trap-and-emulate, binary translation historically; hardware assist now).
- Windows VMs use "filter drivers" to interface with Xen.

## 5.7 KVM Architecture

KVM extends the Linux kernel to add hypervisor functionality:

- Leverages Linux's existing scheduling, paging, memory management.
- Uses VT-x and EPT (Extended Page Tables).
- Each VM appears as a **Linux process** (with a unique PID).
- QEMU handles device emulation.

## 5.8 Paravirtualization workaround for x86-32 (Before hardware assist)

On x86-32, Xen used this trick:
- Identify all 17 sensitive-but-not-privileged instructions.
- Modify the guest OS to `#undef` (remove) those instructions.
- Replace them with **hypercalls** from guest OS to VMM.

---

## ❓ Q&A: Hardware-Assisted Virtualization

**Q1. What problem does Intel VT-x solve that software techniques (binary translation, paravirtualization) could not fully address?**
> VT-x introduces hardware-level VMX root/non-root modes so that the Guest OS can run in Ring 0 of the non-root mode. The previously problematic 17 non-virtualizable x86 instructions now either execute on duplicated hardware state or cause VM exits — without needing software binary translation or OS modification.

**Q2. What is VMX root mode and VMX non-root mode?**
> VMX root mode is where the hypervisor runs with full hardware access. VMX non-root mode is where guest VMs run — even Ring 0 here is restricted, so any sensitive operation causes a VM exit to the hypervisor.

**Q3. What is VMCS and what is its role?**
> VMCS (Virtual Machine Control Structure) is a hardware-maintained data structure that stores the complete state of a guest VM and defines the conditions under which VM exits occur. The hypervisor reads/writes VMCS using vmread/vmwrite instructions to control VM behavior.

**Q4. What is a VM Exit?**
> A VM Exit is a hardware-triggered transfer of control from the guest VM (non-root mode) to the hypervisor (root mode). It occurs when the guest executes certain sensitive instructions, generates interrupts, or accesses restricted resources. The hypervisor analyzes the exit reason and takes action.

**Q5. How does KVM use hardware virtualization?**
> KVM is a Linux kernel module that uses VT-x/AMD-V to run each VM as a Linux process. It uses the VMCS (via vmlaunch/vmresume) to run guest code in non-root mode, VT-x to handle VM exits, and EPT (Extended Page Tables) for hardware-assisted memory virtualization.

---

# 6. Memory Virtualization (Shadow Page Tables & EPT)

## 6.1 Why Memory Virtualization is Needed

In a non-virtualized system, the OS manages page tables that map **virtual addresses → physical addresses**. The CR3 register points to the root of the page table.

In a virtualized system:
- VMs must NOT have direct access to physical memory.
- The hypervisor must manage the actual physical memory.
- Each guest OS has its own page tables, but these map to **"guest physical" addresses** — not the true physical addresses.

**Three address levels in virtualized systems:**

```
gVA (Guest Virtual Address) → gPA (Guest Physical Address) → sPA (System Physical Address)
                              Guest Page Table              Host/System Page Table
```

## 6.2 Shadow Page Tables (Software Technique)

The hypervisor maintains a **shadow page table** for each guest process. The shadow page table maps guest virtual addresses **directly to system physical addresses** (combining both translation levels into one).

```
Guest Page Table:   gVA → gPA
Shadow Page Table:  gVA → sPA   (maintained by hypervisor)
```

**How it works:**
1. Guest OS creates/modifies its page tables (gVA → gPA).
2. Hypervisor intercepts these modifications (guest page tables are **write-protected**).
3. Any write to guest page table generates a **page fault → trap to hypervisor**.
4. Hypervisor updates the shadow page table accordingly (mapping gVA → actual sPA).
5. The CPU's CR3 register is pointed to the **shadow page table** (not the guest's).
6. Hardware page walker uses shadow page table for address translation.

**Challenges with shadow paging:**

| Challenge | Impact |
|---|---|
| Write-protecting guest page tables | Many page faults for write-intensive apps |
| One shadow table per guest process | Context switch requires hypervisor trap to change CR3 |
| TLB flush on world-switch | Performance degradation |
| Memory overhead | Each guest page table needs a shadow copy |

## 6.3 Nested / Extended Page Tables (Hardware-Assisted Technique)

**EPT (Intel's Extended Page Tables)** or **NPT (AMD's Nested Page Tables)** is hardware support introduced to eliminate shadow paging overhead.

**Key idea:** The hardware page walker performs a **two-dimensional page walk**:
1. Walk the **guest page table** (gPT) to translate gVA → gPA.
2. Walk the **nested page table** (nPT) to translate gPA → sPA.

```
gVA → [walk gPT] → gPA → [walk nPT] → sPA
```

Two separate CR3s:
- **gCR3** → points to guest page table (controlled by Guest OS)
- **nCR3** (or EPT Base Pointer — EPTP) → points to nested/extended page table (controlled by hypervisor)

**Benefits of EPT over Shadow Paging:**
- Guest OS freely modifies its own page tables — no trapping needed.
- Hypervisor only manages the EPT (mapping gPA → sPA) — far less frequent changes.
- No need for shadow page tables.
- Significantly reduces VM exits due to page faults, `INVLPG`, and CR3 changes.

**Performance:** EPT significantly improves memory virtualization performance.

## 6.4 Address Translation Summary

| Technique | Translates | Managed By |
|---|---|---|
| Guest Page Table | gVA → gPA | Guest OS |
| Shadow Page Table | gVA → sPA | Hypervisor |
| Nested Page Table (EPT) | gPA → sPA | Hypervisor |

---

## ❓ Q&A: Memory Virtualization

**Q1. Why can't the Guest OS directly access physical memory in a virtualized environment?**
> Because multiple VMs share the same physical memory. If any guest OS had direct physical access, it could read/write another VM's memory, destroying isolation. The hypervisor must be the sole manager of physical memory allocation.

**Q2. What is a shadow page table, and how does the hypervisor keep it synchronized?**
> A shadow page table is a hypervisor-maintained page table that maps guest virtual addresses directly to system physical addresses. The hypervisor keeps it synchronized by write-protecting the guest's page tables; any modification generates a page fault trap to the hypervisor, which updates the shadow accordingly.

**Q3. What are the two main drawbacks of shadow page table?**
> (1) High number of VM exits/page faults when the guest modifies its page tables frequently. (2) Memory overhead from maintaining shadow copies of all guest page tables.

**Q4. How does EPT (Extended Page Tables) improve on shadow paging?**
> EPT uses hardware support for a two-dimensional page walk: first the guest page table (gVA→gPA) and then the nested page table (gPA→sPA). The guest OS can modify its own page tables freely without generating traps, eliminating the overhead of shadow page table maintenance.

**Q5. What is a two-dimensional page walk?**
> When the hardware page walker encounters a TLB miss, it walks both the guest page table (controlled by Guest OS) and the nested/extended page table (controlled by hypervisor) to fully translate a guest virtual address to a system physical address. This is done entirely in hardware, without VMM intervention.

**Q6. What register controls the root of page tables, and how is it handled in virtualization?**
> CR3 controls the root of the page table. In shadow paging, the hypervisor sets CR3 to point to the shadow page table and traps any guest modification of CR3. In EPT/NPT, there are two separate CRs (gCR3 for the guest and a separate EPTP for the hypervisor), allowing each to be independently managed.

---

# 7. I/O Virtualization

## 7.1 What is I/O Virtualization?

**I/O Virtualization (IOV)** is the technology that uses software to abstract upper-layer I/O protocols from physical connections. It manages the routing of I/O requests between virtual devices and shared physical hardware.

## 7.2 Three Approaches to I/O Virtualization

### Approach 1: Full Device Emulation

The hypervisor **emulates well-known physical I/O devices** in software.

- Software in the VMM acts as a virtual device (e.g., a virtual NIC, virtual disk controller).
- Guest OS sends I/O requests to the virtual device.
- VMM intercepts these and translates them to real I/O operations.
- Guest OS uses its unmodified drivers for standard, well-known devices.

**Advantage:** Guest OS doesn't need modification.  
**Disadvantage:** High overhead — every I/O goes through emulation.

### Approach 2: Para-I/O Virtualization (Split Driver Model)

Also called the **hosted** or **split driver model**:

```
[ Guest VM (Domain U) ]     [ Management VM (Domain 0) ]
   Frontend Driver    ←──────────  Backend Driver
   (Guest OS)         Shared Mem  (Host OS / real drivers)
```

- **Frontend driver** runs inside the Guest VM (Domain U).
- **Backend driver** runs in the management/host VM (Domain 0).
- They communicate via **shared memory buffers**.
- Frontend handles guest I/O requests; Backend manages real I/O devices and multiplexes data from multiple VMs.

This is the primary I/O model used by **Xen**.

**Advantage:** Better performance than full emulation.  
**Disadvantage:** Higher CPU overhead; requires modified guest drivers.

### Approach 3: Direct I/O (Passthrough)

The VM gets **direct access to the physical I/O device**, bypassing the VMM's emulation layer.

- VM can directly DMA (Direct Memory Access) to/from host memory.
- Uses **Intel VT-d** (Virtualization Technology for Directed I/O) to assign specific I/O devices to specific VMs.
- Provides near-native I/O performance.

**Advantage:** Highest performance, minimal CPU overhead.  
**Disadvantage:** Limited scalability — one physical device can only be assigned to one VM.

## 7.3 Xen I/O Virtualization Specifics

- Xen does **not** emulate hardware devices directly.
- Instead, it exposes **device abstractions** for simplicity and performance.
- I/O data is transferred via **shared-memory buffers** between the guest (frontend) and Dom0 (backend).
- Uses **virtualized interrupts**: lightweight event delivery mechanism via bitmaps in shared memory.

## 7.4 Advantages of I/O Virtualization

| Benefit | Description |
|---|---|
| **Flexibility** | Hardware independence, utilization, faster provisioning |
| **Cost minimization** | Fewer cables, cards, and switch ports |
| **Increased density** | More connections in a given space |
| **Cable reduction** | Reduces multiple cables needed for storage and network |

---

## ❓ Q&A: I/O Virtualization

**Q1. What are the three ways to implement I/O virtualization?**
> (1) Full device emulation — hypervisor emulates devices in software. (2) Paravirtual/Split driver — frontend in Guest, backend in Dom0, communicating via shared memory. (3) Direct I/O (passthrough) — VM accesses physical device directly, bypassing VMM emulation.

**Q2. What is the split driver model in Xen?**
> Xen's para-I/O model uses a frontend driver inside the guest domain (Domain U) and a backend driver in the management domain (Domain 0). They communicate via shared memory. The frontend handles guest I/O requests; the backend manages actual hardware and multiplexes I/O across VMs.

**Q3. What is Direct I/O, and what are its limitations?**
> Direct I/O (passthrough) lets a VM access a physical device directly via Intel VT-d, achieving near-native performance. Its main limitation is that one physical device can only be assigned to one VM, making it unscalable when many VMs need the same device.

**Q4. Why does Xen not emulate standard hardware devices?**
> Xen opts for paravirtual abstractions instead of device emulation because emulation is complex and has high overhead. By having guest VMs use frontend drivers that communicate with Dom0's backend drivers via shared memory, Xen achieves better performance and simpler implementation.

---

# 8. Popek and Goldberg Principles

## 8.1 Historical Background

In **1974**, Gerald Popek and Robert Goldberg published their landmark paper: *"Formal Requirements for Virtualizable Third Generation Architectures."* This paper set the theoretical foundation for what makes an architecture virtualizable.

At the time, IBM was building VMs for its mainframes, and the question was: can you build a VMM for any arbitrary architecture?

## 8.2 Terminology

Popek and Goldberg defined:

### Virtual Machine
A **complete compute environment** with its own isolated processing, memory, and communication channels, created by the VMM. Key property: it is an **efficient, isolated duplicate** of the physical machine.

### Virtual Machine Monitor (VMM) / Hypervisor
System software that creates and manages VMs with three properties:
- **Efficient**: All safe guest instructions run directly on hardware.
- **Omnipotent**: Only the VMM can manipulate sensitive state.
- **Undetectable**: A guest cannot determine it is running inside a VMM.

### Third-Generation Machines
The paper applies to systems with at least:
- Two operating modes: user mode and supervisor mode
- Some instructions restricted to supervisor mode
- Relocation register for addressing
- Ability to perform table lookups

## 8.3 Three Essential Characteristics

| Characteristic | Description |
|---|---|
| **Equivalence** | A program running on a VM must behave essentially identically to running it directly on hardware |
| **Resource Control** | VMM must have total control of virtualized resources; programs cannot access unallocated resources |
| **Efficiency** | The great majority of instructions must execute without VMM intervention (i.e., directly on hardware) |

## 8.4 Instruction Classification

Popek and Goldberg classified all instructions into:

### Privileged Instructions
Instructions that **cause a trap** if the processor is not in privileged (supervisor) mode. These are "naturally virtualizable" — if executed in user mode, they trap to the VMM.

### Sensitive Instructions
Instructions that **access low-level machine state** (page tables, I/O devices, privilege bits). Two types:

1. **Control-Sensitive**: Attempt to change the configuration of system resources.
   - x86 examples: `PUSHF`, `POPF`, `SGDT`, `SIDT`, `SLDT`, `SMSW`

2. **Behavior-Sensitive**: Their behavior depends on the hardware's current mode/configuration.
   - x86 examples: `POP`, `PUSH`, `CALL`, `JMP`, `INT`, `RET`, `LAR`, `LSL`, `VERR`, `VERW`, `MOV`

### Safe Instructions
Instructions that are **not sensitive** — their behavior is identical regardless of privilege level, and they don't affect system state.

## 8.5 The Three Theorems

### Theorem 1 (Sufficient Condition for VMM)

> *"For any conventional third-generation computer, a VMM may be constructed if the set of sensitive instructions is a subset of the set of privileged instructions."*

**In plain English:** If every sensitive instruction also traps in user mode (i.e., every sensitive instruction is also privileged), then you can build a VMM using trap-and-emulate. The VMM gets control every time the guest tries something sensitive.

**Implication for x86:** x86 failed this theorem because those 17 sensitive instructions were NOT privileged (didn't trap in user mode).

### Theorem 2 (Recursive Virtualization)

> *"A conventional third-generation computer is recursively virtualizable if it is (a) virtualizable, and (b) a VMM without any timing dependencies can be constructed for it."*

**In plain English:** Recursive virtualization means you can run a VM **inside a VM** (nested virtualization). If Theorem 1 holds and there are no timing dependencies, you can nest VMs.

**If Theorem 1 fails (like x86):** Use binary translation to replace sensitive non-privileged instructions with safe equivalents.

### Theorem 3 (Hybrid VMM)

> *"A hybrid virtual machine monitor may be constructed for any conventional third-generation machine in which the set of user-sensitive instructions is a subset of the set of privileged instructions."*

A **hybrid VMM** can be built under slightly relaxed conditions. User-sensitive instructions must trap; supervisor-sensitive instructions don't have to.

## 8.6 Does x86 Satisfy Popek-Goldberg?

**No, the original x86 ISA did NOT satisfy Popek-Goldberg requirements** because:

- The 17 sensitive-but-unprivileged instructions exist.
- `PUSHF` simply reveals kernel is running in user mode (violates "undetectable" property).
- `POPF` silently ignores privilege flag changes in user mode (violates "equivalence" property).

**Workarounds:**
1. Paravirtualization (Xen): Modify Guest OS, replace with hypercalls.
2. Binary Translation (VMware): Translate offending instructions at runtime.
3. Hardware extensions (Intel VT-x / AMD-V): New hardware modes that make x86 properly virtualizable.

---

## ❓ Q&A: Popek and Goldberg

**Q1. State Theorem 1 of Popek and Goldberg and explain its significance.**
> Theorem 1: A VMM may be constructed for any third-generation computer if every sensitive instruction is also a privileged instruction (i.e., sensitive ⊆ privileged). This means trap-and-emulate is sufficient — every sensitive operation traps to the VMM, giving it full control.

**Q2. What is the difference between a privileged instruction and a sensitive instruction?**
> A privileged instruction traps when executed in user mode. A sensitive instruction accesses or modifies critical hardware state. Privileged ⊄ Sensitive and Sensitive ⊄ Privileged in general; for virtualizable architectures, we need Sensitive ⊆ Privileged.

**Q3. What does "recursive virtualization" mean (Theorem 2)?**
> Recursive virtualization means the ability to run a VM inside another VM — i.e., a VMM can be installed on top of itself. Theorem 2 states conditions under which this is possible.

**Q4. Why does x86 fail the Popek-Goldberg requirements?**
> Because x86 has 17 sensitive instructions that are NOT privileged — they do not trap when executed in user mode. This violates the requirement that sensitive ⊆ privileged. Without traps, the VMM cannot intercept and control these operations.

**Q5. What are the three essential characteristics (VMM properties) defined by Popek and Goldberg?**
> Equivalence (VM behaves like real machine), Resource Control (VMM has total control of resources), and Efficiency (most instructions execute directly without VMM intervention).

**Q6. What does it mean for a VMM to be "undetectable"?**
> The guest program running inside the VM should not be able to determine that it is running inside a VMM rather than directly on hardware. If a guest can detect it's virtualized (e.g., by reading a privilege-level register), it violates this property.

---

# 9. VM Migration

## 9.1 What is VM Migration?

**VM Migration** is the process of moving a virtual machine from one physical host to another. The migration includes all of the VM's state: memory contents, CPU state, device state, BIOS, MAC addresses, etc.

## 9.2 Why Migrate VMs?

| Reason | Explanation |
|---|---|
| **Hardware Maintenance** | Move VMs off a server to allow physical maintenance without downtime |
| **Load Balancing** | Redistribute workload evenly across servers in a cluster |
| **Scaling** | Move VM to a server with more resources if current server is underpowered |
| **Workload Mobility** | Compliance requirements may mandate VM location changes |
| **Performance** | Move to a server with better CPU/memory/network for faster response |
| **Energy Efficiency** | Consolidate VMs onto fewer servers during off-peak and shut down idle servers |
| **Business Continuity / HA** | Migrate away from failing hardware proactively |

## 9.3 Types of Migration

### Cold (Offline) Migration
- VM is **powered off** before migration.
- All state is transferred to the destination.
- VM starts up on the destination.
- **Drawback:** Downtime during the entire migration (can be significant).
- **Advantage:** Simple, compatible with different CPU families.

### Non-Live (Offline) Migration
- VM at source is **paused**.
- All state transferred to destination.
- VM **resumed** at destination.
- **Drawback:** Downtime is the total transfer time.
- Memory pages migrated only once.

### Live (Hot) Migration
- VM migrates **while still running** — no disruption to service.
- End users are completely unaware of migration.
- Network connections are maintained; applications continue uninterrupted.
- **Example:** VMware vSphere vMotion

## 9.4 Live Migration — Pre-Copy Technique

The **pre-copy** technique is the most widely used (KVM, Xen, VMware all use it).

**Six stages:**

1. **Select destination host** — based on load balancing, availability.

2. **Reserve resources** — allocate memory, CPU, network on destination.

3. **Iterative pre-copying rounds:**
   - Round 1: Transfer **all** memory pages to destination.
   - Subsequent rounds: Transfer only **dirty pages** (pages modified since last round).
   - Continues until one of:
     - Fixed threshold reached.
     - Dirty page set is small enough.
   - VM keeps running during all this — execution is not paused!

4. **Stop and transfer remaining state:**
   - VM is **paused** (this is the "downtime").
   - Copy remaining dirty pages + CPU state, device state.
   - This downtime should be as **short as possible** (ideally invisible to users).

5. **Commitment** — destination accepts the state.

6. **VM activation at destination:**
   - VM resumes at destination.
   - Network connections redirected to new VM.
   - Source VM deleted.

**Advantages:**
- Low downtime (only the final dirty page copy phase).
- Good for applications with low write rates (few dirty pages per round).

**Disadvantages:**
- High total migration time if memory is written heavily (pages keep getting dirtied and need re-copying).
- For write-intensive workloads, dirty pages accumulate faster than they can be transferred.

### Live Migration — Post-Copy Technique

In **post-copy**, the approach is reversed:

1. Transfer **processor state** (CPU registers, device state) to destination first.
2. **VM immediately resumes** at destination.
3. Memory pages are transferred **on-demand** as the VM needs them (via page faults).

**Page fault handling at destination:**
- **Demand Paging:** VM generates a page fault → destination fetches the missing page from source. Slowest option but simple.
- **Active Push:** Source proactively pushes pages to destination even while VM runs. If a page fault occurs, demand paging handles it. Each page sent only once.
- **Memory Prepaging:** Predict which pages will be needed (based on access locality) and proactively send them, reducing page faults.

**Advantages:**
- VM resumes almost immediately (no waiting for memory transfer).
- Very low initial downtime.

**Disadvantages:**
- Page faults cause repeated interruptions during execution.
- Each page fault suspends the VM until the page arrives from source.
- Network dependency — source must stay available throughout memory transfer.

## 9.5 Memory Migration Challenges

- **ISR (Internet Suspend-Resume):** Exploits **temporal locality** — the VM's memory state when resumed is likely very similar to when it was last suspended. Only changed pages need to be sent, using a tree-based caching structure.

## 9.6 File System Migration

- Each VM needs a consistent, location-independent view of its filesystem during migration.
- Solutions:
  - Virtual disk with VM (copy the disk along with VM state).
  - **Global/shared filesystem** (e.g., NFS, NAS) — files accessible from any host; no file copying needed.

## 9.7 Network Migration

- During migration, the VM maintains the same virtual IP and MAC address.
- The VMM maintains a mapping of virtual IP/MAC to the current physical host.
- The migrating VM carries its IP address with it, ensuring existing connections survive.

---

## ❓ Q&A: VM Migration

**Q1. What is the difference between cold migration and live migration?**
> Cold migration powers off the VM before transfer — there is downtime equal to the entire transfer time. Live migration migrates the VM while it is running — services continue uninterrupted and users are unaware.

**Q2. Explain the pre-copy migration technique in detail.**
> Pre-copy keeps the VM running while iteratively copying all memory to the destination. In round 1, all pages are sent. In subsequent rounds, only dirty (modified) pages are resent. When the dirty set becomes small enough, the VM is briefly paused, remaining pages and CPU state are transferred, the VM is committed and activated at the destination.

**Q3. What is the "downtime" in pre-copy migration?**
> Downtime is the period when the VM is paused to transfer the final dirty pages and CPU state. It is the only time users experience service interruption. The goal is to minimize this period.

**Q4. What is the post-copy technique, and how does it differ from pre-copy?**
> In post-copy, the CPU state is transferred first, the VM immediately resumes at the destination, and memory pages are fetched on-demand as page faults occur. Pre-copy keeps the VM running at the source while iteratively sending memory; post-copy resumes at destination immediately but experiences page faults.

**Q5. What is "demand paging" in the context of post-copy migration?**
> When the VM running on the destination host accesses a memory page that hasn't been transferred yet, a page fault occurs. The missing page is fetched from the source host on demand. This is called demand paging in the migration context.

**Q6. Why is live migration considered resource-intensive?**
> Because the VM continuously demands CPU cycles, cache memory, memory bandwidth, and network bandwidth during migration. Copying gigabytes of memory across a network while the VM is running requires significant I/O resources.

---

# 10. Containers, Namespaces, and cgroups

## 10.1 Why Containers? The Problem with VMs

VMs provide strong isolation — but at a **heavy cost**:
- Each VM needs its own full OS image (GBs of storage).
- VMs take **minutes** to boot.
- Each VM has significant memory overhead.

For many workloads, we want isolation but don't need a full OS for every application. Enter **containers**.

## 10.2 What is a Container?

A **Linux Container (LXC)** is an **OS-level virtualization** technique that runs multiple isolated Linux environments (containers) on a single Linux OS instance.

Key properties:
- Containers **share the host OS kernel** (no separate kernel per container).
- Containers share host binaries and libraries (as read-only).
- Containers are **megabytes in size** (vs gigabytes for VMs).
- Start in **seconds** (vs minutes for VMs).
- Container creation is similar to process creation.

## 10.3 VMs vs Containers: Full Comparison

| Feature | Virtual Machine | Container |
|---|---|---|
| Isolation level | Hardware-level (own kernel) | OS-level (shared kernel) |
| OS | Separate, full OS per VM | Shares host OS kernel |
| Size | Gigabytes | Megabytes |
| Boot time | Minutes | Seconds |
| Performance | I/O overhead from emulation | Near-native (process-like) |
| Flexibility | Can run different OS types | Must run same OS type as host |
| Resource usage | Higher | Much lower |
| Use case | Full OS isolation, mixed OS | Microservices, app packaging |
| Example | VMware, VirtualBox | Docker, LXC |

## 10.4 How Containers Achieve Isolation: Linux Namespaces

**Namespaces** are a Linux kernel feature that **partitions kernel resources** so that one set of processes sees one set of resources, while another set sees a different set. Each namespace provides an **isolated instance of a global resource**.

Core principle: *"If you can't name an object, you can't access it."*

Docker uses the following namespaces:

| Namespace | What it Isolates |
|---|---|
| **PID** | Process IDs — containers can't see processes of other containers |
| **UTS** | Hostname and domain name — each container can have its own hostname |
| **MNT** | Mount points — each container has its own filesystem view |
| **IPC** | Inter-process communication — shared memory, semaphores isolated |
| **NET** | Network interfaces, routes, IP addresses, ports |
| **USR** | User/group IDs — maps container user IDs to host user IDs |

### MNT Namespace Example

```
Container 1 can access:
  /usr (shared)
  /mnt (its own)
  Cannot access /mnt2 (belongs to Container 2)

Container 2 can access:
  /usr (shared)
  /mnt2 (its own)
  Cannot access /mnt (belongs to Container 1)
```

### chroot — Filesystem Root Isolation

`chroot` changes the apparent root directory for a process:
- Creates a "jail" — the process sees a different directory as `/`.
- `chroot /mnt` makes the process see `/mnt` as its root.
- Used for filesystem isolation in containers.
- `pivot_root` is a stronger variant that fully detaches the old root.

### NET Namespace

A NET namespace creates a **virtual network stack**:
- Each container gets its own network interfaces, routing tables, IP addresses, and ports.
- Containers in different namespaces can have the same IP addresses and port numbers without conflicts.
- **VETH (Virtual Ethernet) pairs** are used when namespaces need to communicate:
  - A VETH pair is like a pipe — packets sent on one end are received on the other.
  - One end lives in the container's NET namespace; the other lives in the host namespace.

### UTS Namespace

**UTS (Unix Time Sharing) Namespace** isolates:
- **Hostname**
- **NIS domain name**

Each container can have its own fully qualified domain name (FQDN). Changes to hostname inside a container are only reflected in that container's namespace.

## 10.5 Resource Control: cgroups (Control Groups)

**cgroups** (Control Groups) is a Linux kernel feature to **limit, police, and account** resource usage for a set of processes.

cgroups can control:

| Resource | What it Controls |
|---|---|
| **Memory** | Maximum RAM a group of processes can use |
| **CPU** | CPU time allocated (e.g., 50% of one core) |
| **Block I/O** | Disk read/write bandwidth |
| **Network** | Network bandwidth |
| **Devices** | Which devices a cgroup can access |

**cgroup functionality:**
- **Access** — which devices a cgroup can use
- **Resource limiting** — set maximums on memory, CPU, etc.
- **Prioritization** — who gets more CPU/memory when resources are contested
- **Accounting** — track resource usage per cgroup
- **Control** — freeze (pause) and checkpoint processes
- **Injection** — packet tagging

**Key property:** When a process creates a child process, the child **stays in the same cgroup**, inheriting the parent's resource limits. This is useful for servers that fork worker processes.

### Docker cgroup examples

```bash
# Limit container to 50% of one CPU
docker run -d --name mycontainer --cpus="0.5" ubuntu sleep 1000

# Limit container to 100MB of RAM
docker run -d --name mycontainer --memory=100m ubuntu sleep 1000

# Check cgroup limits from inside the container
docker exec mycontainer cat /sys/fs/cgroup/cpu/cpu.cfs_quota_us
docker exec mycontainer cat /sys/fs/cgroup/memory/memory.limit_in_bytes
```

If a container exceeds its memory limit, the **OOM (Out-of-Memory) killer** terminates it (exit code 137).

---

## ❓ Q&A: Containers, Namespaces, cgroups

**Q1. What is the fundamental difference between a container and a virtual machine?**
> A VM has its own complete OS kernel and virtualizes hardware. A container shares the host OS kernel and uses namespace isolation to create isolated environments for processes. Containers are much lighter (MB vs GB, seconds vs minutes to start).

**Q2. What are Linux namespaces and how do they enable container isolation?**
> Namespaces partition kernel resources so that each namespace has its own isolated view of those resources. Containers use namespaces for PID isolation (can't see other containers' processes), NET isolation (own network stack), MNT isolation (own filesystem view), UTS isolation (own hostname), IPC isolation, and USR isolation.

**Q3. What is the role of cgroups in containerization?**
> cgroups control and limit the physical resources that a group of processes can use — CPU, memory, block I/O, network, and devices. Docker uses cgroups to enforce resource limits on containers (e.g., max 100MB RAM), preventing any single container from monopolizing system resources.

**Q4. Explain how the MNT namespace provides filesystem isolation.**
> The MNT namespace isolates the set of mount points visible to a process. Processes in different MNT namespaces see different filesystem hierarchies. A container's MNT namespace is set up so it only sees its own filesystem, not the host's or other containers' filesystems.

**Q5. What is the difference between namespaces and cgroups?**
> Namespaces control **what a process can see** (resource visibility/isolation). cgroups control **how much of a resource a process can use** (resource limiting/accounting). Together they form the foundation of container isolation and resource management.

**Q6. What happens when a Docker container exceeds its memory limit?**
> The Linux OOM (Out-of-Memory) killer terminates the container. This results in an exit code of 137. This behavior can be observed by setting a memory limit with `--memory` and running a stress test that exceeds the limit.

**Q7. What is chroot and how does it relate to container filesystem isolation?**
> `chroot` changes the apparent root directory (`/`) for a process and its children. By using `chroot` inside a container, the container's processes see a completely different filesystem root, preventing them from accessing the host's real filesystem. Docker uses MNT namespaces combined with `chroot`/`pivot_root` for strong filesystem isolation.

---

# 11. Docker — Architecture, Images, Containers, Registries

## 11.1 What is Docker?

**Docker** is an open platform that makes it easier to create, test, ship, deploy, and execute applications using **containers**.

- It separates **applications from infrastructure**.
- Reduces time between writing code and running in production.
- Packages an application in a loosely isolated environment called a container.
- Can be considered a **PaaS product** that uses OS-level virtualization to deliver software packages.
- Typically 8–18 containers run simultaneously on a single server/VM.

Docker is available for:
- Linux (native)
- macOS (native app)
- Windows (native app, runs native Windows containers on Windows Server)

## 11.2 Docker Architecture

Docker uses a **client-server architecture**:

```
┌──────────────┐   REST API / Unix socket   ┌─────────────────────────────────┐
│ Docker Client│ ─────────────────────────► │         Docker Host             │
│  (docker CLI)│                            │  ┌──────────────┐               │
└──────────────┘                            │  │ Docker Daemon│               │
                                            │  │  (dockerd)   │               │
┌──────────────┐                            │  └──────┬───────┘               │
│Docker Compose│                            │         │                       │
│  (Client)    │                            │  ┌──────▼──────────────────┐   │
└──────────────┘                            │  │ Containers │ Images      │   │
                                            │  │ Networks   │ Volumes     │   │
                                            │  └─────────────────────────┘   │
                                            └──────────────┬──────────────────┘
                                                           │
                                                 ┌─────────▼──────────┐
                                                 │  Docker Registry    │
                                                 │  (Docker Hub etc.)  │
                                                 └────────────────────┘
```

### Docker Daemon (dockerd)
- Listens for Docker API requests.
- Manages Docker objects: images, containers, networks, volumes.
- Can communicate with other daemons to manage Docker services.

### Docker Client (docker CLI)
- The primary way users interact with Docker.
- Sends commands like `docker run`, `docker build`, `docker push` to the daemon via the Docker API.
- Can communicate with multiple daemons.

### Docker Compose
- Another client for Docker.
- Manages applications consisting of multiple containers.
- Defines multi-container apps in a single `docker-compose.yml` file.

### Docker Host
- The machine where Docker Daemon runs.
- Can host a local registry or connect to Docker Hub.

## 11.3 Docker Objects

### Images

A Docker **Image** is a **read-only template** with instructions for creating a Docker container.

- Can be based on another image with additional customizations.
- Built from a **Dockerfile** — a script with step-by-step instructions.
- Each instruction in a Dockerfile creates a **layer** in the image.
- Images are lightweight because they are stored in layers.

**Example Dockerfile:**

```dockerfile
FROM nginx:alpine                          # Base image
WORKDIR /usr/share/nginx/html             # Set working directory
RUN rm -rf ./*                            # Clean directory
RUN apk add --no-cache git && \           # Install git
    git clone <repo> /temp-repo && \      # Clone code
    cp -r /temp-repo/* . && \             # Copy files
    rm -rf /temp-repo                     # Cleanup
EXPOSE 80                                 # Expose port
CMD ["nginx", "-g", "daemon off;"]        # Start nginx
```

Each `RUN`, `COPY`, `ADD`, etc. creates a new layer. Layers are cached — rebuilding an image only rebuilds changed layers.

### Image Layers

An image layer is a **set of files and file metadata** packaged as an atomic unit.

- Each layer builds on a parent layer by applying filesystem changes.
- Docker treats each layer internally as an image ("intermediate images").
- When a Dockerfile changes and image is rebuilt, only **incremental changes** are rebuilt — making it fast.
- Multiple images can share common base layers.

### Containers

A Docker **Container** is a **running instance of an image** — an execution environment (sandbox).

- A container holds the entire package needed to run the application.
- We can create, start, stop, move, or delete containers using Docker API or CLI.
- Containers are relatively isolated from other containers and the host.
- When a container is removed, any unsaved state is lost.

**Container isolation uses:**
- **Namespaces** — restrict what processes in the container can see.
- **cgroups** — restrict how many resources the container can use.

### Docker Registries

A **Docker Registry** stores Docker images.

- **Docker Hub** is the default public registry.
- You can host your own private registry.
- `docker pull` — downloads an image from registry.
- `docker push` — uploads your image to registry.
- `docker run` — pulls image if not present locally, then runs it.

## 11.4 Docker Commands Cheat Sheet

```bash
# Build an image from Dockerfile in current directory
docker build -t myapp:latest .

# Run a container from an image
docker run -d -p 8080:80 myapp:latest

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop <container_id>

# Remove a container
docker rm <container_id>

# Remove an image
docker rmi myapp:latest

# View logs
docker logs <container_id>

# Execute command in running container
docker exec -it <container_id> bash

# Create a network
docker network create mynetwork

# Create a volume
docker volume create myvolume

# Run with network and volume
docker run -d --network mynetwork -v myvolume:/data myapp:latest

# Push to Docker Hub
docker tag myapp:latest username/myapp:latest
docker push username/myapp:latest
```

## 11.5 What Happens When You Run `docker run`?

1. Docker client sends `docker run` request to Docker daemon.
2. Daemon checks if the image exists locally.
3. If not, pulls it from the configured registry (Docker Hub by default).
4. Docker creates a new container from the image.
5. Docker allocates a read-write filesystem layer on top of the read-only image layers.
6. Docker creates a network interface and assigns an IP address.
7. Docker starts the container's main process.
8. The container runs until the main process exits.

---

## ❓ Q&A: Docker

**Q1. What is Docker and why is it used?**
> Docker is an open platform for building, shipping, and running applications in containers. It is used because it eliminates environment setup problems, enables consistent deployments across machines, separates applications from infrastructure, and is much lighter than VMs.

**Q2. What is the difference between a Docker image and a container?**
> A Docker image is a read-only template (blueprint) containing the application and its dependencies. A container is a running instance of that image — a live execution environment. Multiple containers can be created from the same image.

**Q3. What is a Dockerfile and how is it used?**
> A Dockerfile is a script containing step-by-step instructions to build a Docker image. Each instruction creates a layer. The `docker build` command processes the Dockerfile and produces an image. Common instructions include `FROM` (base image), `RUN` (execute commands), `COPY` (copy files), `EXPOSE` (declare ports), and `CMD` (default startup command).

**Q4. What is Docker Hub?**
> Docker Hub is Docker's official public image registry. It stores pre-built Docker images that anyone can use. You can `docker pull` images from Docker Hub and `docker push` your own images to share them.

**Q5. How does Docker networking solve the container communication problem?**
> Docker creates a virtual network where containers can communicate using container names instead of IP addresses. IP addresses of containers can change when containers restart, but names remain stable within a Docker network. You create a network with `docker network create`, then run containers with `--network` to add them to it.

**Q6. What are Docker volumes and why are they needed?**
> Containers are ephemeral — when a container is removed, its data is lost. Docker volumes provide persistent storage that survives container deletion. Volumes are stored outside the container's filesystem and can be shared among multiple containers.

**Q7. What is the role of `docker-compose.yml`?**
> `docker-compose.yml` defines multi-container applications — specifying services (containers), their images, ports, networks, volumes, and dependencies. Running `docker-compose up` starts all defined services together, handling networking and dependency order automatically.

---

# 12. Union Filesystem (UnionFS)

## 12.1 What is UnionFS?

A **Union Filesystem (UnionFS)** creates an **illusion of merging multiple directories into one** without modifying the original sources. The merged view appears as a single filesystem.

Docker uses UnionFS to provide the layered image filesystem. Variants include:
- **AUFS** (Advanced Union File System)
- **overlay2** (most common in modern Docker)
- **btrfs**, **vfs**, **DeviceMapper**

## 12.2 How UnionFS Works: Layering

```
/Fruits: [Apple, Tomato]
/Vegetables: [Carrots, Tomato]

mount -t unionfs -o dirs=/Fruits:/Vegetables none /mnt/healthy

/mnt/healthy: [Apple, Tomato (from /Fruits), Carrots]
```

- `/Fruits` is layered **on top** of `/Vegetables`.
- When the same file exists in both (`Tomato`), the **upper layer wins**.
- The result appears as one coherent directory.

## 12.3 Copy-on-Write (CoW)

When a container modifies a file that exists in a read-only image layer:
1. The original file in the lower layer is **not modified**.
2. A **copy of the file is made** to the container's top read-write layer.
3. Modifications are applied to the copy.

This is **Copy-on-Write**. It ensures:
- The base image layers remain unchanged and shareable.
- Each container gets its own private view of any file it modifies.
- Read operations go through to the lowest layer where the file exists.

## 12.4 Docker Layer Model

```
┌─────────────────────────────────────┐
│   Container Layer (Read-Write)       │  ← unique per container
├─────────────────────────────────────┤
│   Image Layer: App installation      │  ← read-only
├─────────────────────────────────────┤
│   Image Layer: Dependencies          │  ← read-only
├─────────────────────────────────────┤
│   Image Layer: Base OS (ubuntu)      │  ← read-only
└─────────────────────────────────────┘
```

- Each Dockerfile instruction (`RUN`, `COPY`, `ADD`) creates a new read-only layer.
- The container adds a **thin read-write layer on top**.
- Multiple containers can share the same image layers — only the top container layer is unique.
- When a container is deleted, only the top read-write layer is removed; image layers persist.

## 12.5 Weaknesses of UnionFS

| Weakness | Description |
|---|---|
| Filesystem compatibility | Different filesystems have different rules; translation can lose features |
| mmap difficulty | Copy-on-write makes memory-mapped files (`mmap`) complex to implement |
| Not for long-lived data | Data in the container layer is lost when container is deleted |
| Not for sharing data between containers | Use volumes instead for shared or persistent data |

---

## ❓ Q&A: Union Filesystem

**Q1. What is a Union Filesystem and why does Docker use it?**
> A UnionFS merges multiple directory layers into a single coherent view without modifying the originals. Docker uses it to implement layered images — each Dockerfile instruction adds a layer, and layers are shared across images, saving disk space and enabling efficient builds.

**Q2. What is Copy-on-Write in the context of Docker?**
> When a container modifies a read-only file from an image layer, the file is first copied to the container's top read-write layer, and then modified. The original in the image layer remains unchanged. This allows multiple containers to share the same image layers while having private modifications.

**Q3. How does Docker's layer model save storage space?**
> Multiple images that share the same base layers store those layers only once. For example, if ten images all use `ubuntu:20.04` as a base, the ubuntu base layer is stored just once on disk. Similarly, all containers running from the same image share its read-only layers.

**Q4. Why are Docker volumes recommended for persistent data instead of the container layer?**
> The container's read-write layer is deleted when the container is removed. For data that should persist (databases, logs), Docker volumes store data outside the container filesystem. Volumes are not removed when containers are deleted and can be shared among multiple containers.

---

# 13. DevOps and CI/CD

## 13.1 What is DevOps?

**DevOps** is a set of practices that combines **software development (Dev)** and **IT operations (Ops)** to:

- Shorten the **Software Development Life Cycle (SDLC)**.
- Increase an organization's ability to **deliver applications and services** faster.
- Enable continuous delivery of value to users.

Historically, Dev and Ops had **conflicting goals**:
- Dev: "Ship new features fast."
- Ops: "Keep the system stable."

DevOps bridges this gap through automation, collaboration, and shared responsibility.

DevOps is complementary with **Agile** software development. Docker and Kubernetes are among the top 10 most used DevOps tools.

## 13.2 DevOps Cycle

```
PLAN → BUILD → TEST → RELEASE → DEPLOY → OPERATE → MONITOR → (back to PLAN)
```

## 13.3 DevOps Pipeline for Containerized Apps

A typical CI/CD pipeline:

1. Developer pushes code changes to **Git**.
2. Build system automatically **builds** the current version and runs sanity tests.
3. If tests pass, **container image** is published to the central container registry.
4. Newly built container is deployed automatically to a **staging environment**.
5. Staging undergoes **automated acceptance tests**.
6. Verified container image is deployed to **production**.

## 13.4 Principles of Continuous Delivery

| Principle | Description |
|---|---|
| Every build is a potential release | Code is always in a deployable state |
| Eliminate manual bottlenecks | Automate every repeated step |
| Automate wherever possible | Builds, tests, deployments should all be automated |
| Have automated tests you can trust | Tests should be comprehensive enough to trust the release |

## 13.5 Continuous Integration (CI)

**Continuous Integration** is the automatic merging and building of code:

- Every developer's changes are automatically merged into a shared mainline branch.
- The system automatically builds the merged code.
- Sanity tests run automatically.
- Developers get immediate feedback if their change breaks the build.

**Tools:** Git (version control), Jenkins, Travis CI, CircleCI, Drone, Maven, ANT.

**Key insight:** If you're on a feature branch that would break the mainline when merged, CI tells you immediately — not weeks later.

## 13.6 Continuous Delivery (CD)

**Continuous Delivery** is the frequent, automatic shipping of sanitized builds to an environment (test or production).

After a CI build passes:
- The artifact (Docker container) is automatically deployed to staging.
- Automated acceptance tests run.
- If passed, it's ready for production deployment.

**Tool:** Jenkins (Java-based open-source automation server).

## 13.7 Continuous Testing

**Continuous Testing** executes automated tests as part of the delivery pipeline:

- Integration tests
- Functionality tests
- Performance tests
- Acceptance tests

Activities:
- Copy binaries to staging environment.
- Configure correctly, ensure dependencies are installed.
- Execute planned test categories (JUnit, Selenium).
- Generate test reports.

## 13.8 Continuous Deployment

**Continuous Deployment** automatically deploys successful builds to production:

- Developers deploy simply by pushing a Git tag, a merge request, or pressing a button.
- Immutable infrastructure: servers are never modified after deployment; new containers replace old ones.

**Tools:**
- Infrastructure provisioning: Chef, Puppet, Kubernetes.
- Container management: Docker.
- Workflow automation: Jenkins.

## 13.9 Jenkins

**Jenkins** is the most widely adopted CD tool:

- Self-contained, open-source automation server.
- Automates building, testing, and delivering software.
- Can be installed via native packages, Docker, or as a standalone Java application.
- JenkinsX is a version designed for Kubernetes.

---

## ❓ Q&A: DevOps and CI/CD

**Q1. What is DevOps and what problem does it solve?**
> DevOps unifies Development and Operations teams by automating the SDLC — building, testing, deploying, and monitoring software. It solves the conflict between Dev (frequent changes) and Ops (stability), enabling rapid, reliable software delivery.

**Q2. Describe the typical DevOps pipeline for a containerized application.**
> Developer pushes to Git → CI system builds and runs tests → container image built and pushed to registry → image deployed to staging → acceptance tests run → image deployed to production. Each step is automated.

**Q3. What is the difference between Continuous Integration, Continuous Delivery, and Continuous Deployment?**
> CI = automatically build and test every code change. CD (Delivery) = automatically deploy to staging/test environments after passing tests. CD (Deployment) = automatically deploy to production without manual intervention.

**Q4. What is the benefit of "every build is a potential release"?**
> It ensures the codebase is always in a deployable state. Bugs are caught immediately when introduced (during CI builds and tests) rather than accumulating and being discovered during a massive integration phase.

**Q5. What is immutable infrastructure in the context of DevOps?**
> Immutable infrastructure means containers/servers are never modified after deployment. If a change is needed, a new container image is built and deployed to replace the old one. This ensures consistency, eliminates configuration drift, and enables reliable automation.

---

# 14. Container Orchestration and Kubernetes

## 14.1 Why Container Orchestration?

Managing a few containers manually is fine. But in production:
- Applications consist of **dozens of microservices**, each in its own container.
- Containers must be deployed across **a cluster of many physical/virtual servers**.
- Containers fail, need to be restarted, scaled up/down, load-balanced.
- Manual management becomes impossible.

**Container Orchestration** automates the deployment, management, scaling, and networking of containers.

## 14.2 What Orchestration Does

| Function | Description |
|---|---|
| **Provisioning & Deployment** | Schedule containers on appropriate nodes |
| **Configuration** | Manage container configurations and secrets |
| **Resource Allocation** | Assign CPU, memory, network to containers |
| **Availability** | Detect failures and restart/reschedule containers |
| **Scaling** | Add or remove containers based on load |
| **Load Balancing** | Distribute traffic among container instances |
| **Health Monitoring** | Monitor container and node health |
| **Migration** | Move containers between nodes on failures |

## 14.3 Immutable Infrastructure in Orchestration

Container orchestration embraces the concept of **immutable infrastructure**:

- Containers are never modified after deployment.
- If an update is needed, a new image is built and new containers replace old ones.
- Ensures consistency and reliability.
- Enables predictable deployment pipelines.

## 14.4 Kubernetes (K8s) Overview

**Kubernetes** is the most pervasive container orchestration platform. Originally developed by Google, now open-source.

Kubernetes:
- Orchestrates computing, networking, and storage for containerized applications.
- Enables truly immutable infrastructure at scale.
- Manages the entire container lifecycle.

## 14.5 Kubernetes Building Blocks

### Pod

A **Pod** is the smallest deployable unit in Kubernetes — a group of one or more containers:

- Containers in a pod share the same **network namespace** (same IP), **storage** (volumes), and Linux namespaces/cgroups.
- Containers in a pod communicate over **localhost**.
- Pods are assigned their own IP, accessible by other pods within the cluster.
- Pods are **ephemeral** — they are created, destroyed, and re-created on demand.
- A pod is scheduled to a **node** (worker machine) and runs there until termination.

**Pod lifecycle:**
1. **Pending** — scheduled but not yet started.
2. **Running** — at least one container started successfully.
3. **Succeeded** or **Failed** — all containers terminated.

### Service

A **Service** is an abstraction that groups a set of pods and defines how to access them:

- Pods are constantly created/destroyed, changing their IPs.
- A Service provides a **stable virtual IP (VIP)** that doesn't change.
- Other services communicate with pods via this stable VIP.
- The Service tracks pod IP changes and maps them to its stable VIP.
- Services enable **load balancing** across multiple pods.

### Deployment

A **Deployment** provides declarative updates for Pods via a ReplicaSet:

- You specify the desired state (e.g., "3 replicas of this container").
- Kubernetes continuously works to achieve and maintain that state.
- Supports **rolling updates** (gradually replace old pods with new).
- Supports **automatic rollbacks** if health checks fail.

### ReplicaSet (RS)

A **ReplicaSet** ensures a specified number of pod replicas are running at all times:

- If a pod crashes, the ReplicaSet creates a new one.
- Managed through Deployments.

### Volume

A **Volume** provides persistent storage for containers in a pod:

- Containers are ephemeral; data is lost when a container restarts.
- Volumes persist beyond container restarts (as long as the pod exists).
- **PersistentVolumes (PV)** persist even beyond the pod's lifetime.

### Namespace (Kubernetes)

A **Kubernetes Namespace** divides a cluster into virtual sub-clusters:

- Separate teams or projects get their own namespace.
- Resource quotas can be applied per namespace.
- Provides logical isolation within a single cluster.

### DaemonSet, StatefulSet, Job

| Object | Purpose |
|---|---|
| **DaemonSet** | Ensures every node runs a copy of a pod (e.g., logging agent, monitoring agent) |
| **StatefulSet** | Manages stateful applications like databases; provides stable identities |
| **Job** | Creates pods to run a task to completion, then terminates them |

## 14.6 Kubernetes Architecture

### Master Node Components

The master node is the **control plane** — it orchestrates the entire cluster:

| Component | Role |
|---|---|
| **kube-apiserver** | Entry point for all REST API commands to the cluster |
| **etcd** | Distributed key-value store — stores all cluster configuration and state |
| **kube-scheduler** | Assigns new pods to worker nodes based on available resources |
| **kube-controller-manager** | Runs controllers that maintain desired state (e.g., Replication Controller) |

### Worker Node Components

Worker nodes run the actual containers:

| Component | Role |
|---|---|
| **kubelet** | Agent on each node; gets pod specs from apiserver and ensures containers run |
| **kube-proxy** | Network proxy and load balancer on each node; handles TCP/UDP routing |
| **Container Runtime** | Runs the containers (e.g., Docker, containerd) |

```
                ┌──────────────────────────────────┐
                │          Master Node              │
                │  apiserver ─ etcd                 │
                │  scheduler ─ controller-manager   │
                └───────────────┬──────────────────┘
                                │
          ┌─────────────────────┼──────────────────────┐
          │                     │                      │
  ┌───────▼──────┐    ┌────────▼──────┐    ┌──────────▼─────┐
  │ Worker Node 1 │    │ Worker Node 2 │    │  Worker Node 3  │
  │ kubelet       │    │ kubelet       │    │  kubelet        │
  │ kube-proxy    │    │ kube-proxy    │    │  kube-proxy     │
  │ [Pods]        │    │ [Pods]        │    │  [Pods]         │
  └──────────────┘    └───────────────┘    └────────────────┘
```

## 14.7 Kubernetes Benefits

| Benefit | Description |
|---|---|
| **Horizontal Scaling** | Scale apps up/down with a command |
| **Automated Rollouts/Rollbacks** | Controlled updates; auto-rollback if health fails |
| **Service Discovery & Load Balancing** | Expose containers via DNS or IP; distribute traffic |
| **Storage Orchestration** | Automatically mount local or cloud storage |
| **Self-Healing** | Restart failed containers, reschedule on node failure |
| **Secret & Config Management** | Securely store and inject passwords, tokens, SSH keys |
| **Batch Execution** | Manage CI/batch workloads |
| **Automatic Binpacking** | Schedule containers optimally based on resource requirements |

## 14.8 Orchestration Tools Overview

| Tool | Description |
|---|---|
| **Kubernetes** | Most pervasive; full-featured orchestration |
| **Docker Swarm** | Docker's native clustering (simpler than K8s) |
| **Google Container Engine** | Kubernetes on GCP |
| **Amazon ECS** | AWS's container service (Docker on EC2 clusters) |
| **AWS Fargate** | Serverless containers on AWS |
| **Azure Kubernetes Service** | Kubernetes on Azure |

---

## ❓ Q&A: Kubernetes and Orchestration

**Q1. What is container orchestration and why is it necessary?**
> Container orchestration automates the deployment, scaling, networking, and management of containers across a cluster of machines. It is necessary because production applications involve dozens of microservices running as containers; manual management becomes impossible at scale.

**Q2. What is a Pod in Kubernetes?**
> A Pod is the smallest deployable unit in Kubernetes — a group of one or more tightly coupled containers that share the same network namespace (IP address), storage volumes, and Linux namespaces. Containers in a pod communicate over localhost.

**Q3. Why does Kubernetes use Services instead of directly addressing Pods?**
> Pods are ephemeral and their IP addresses change constantly as they are created/destroyed. A Service provides a stable virtual IP that doesn't change, abstracting the dynamic pod IPs. Other services communicate via the stable Service VIP, which routes traffic to healthy pods.

**Q4. What are the components of the Kubernetes Master Node?**
> kube-apiserver (REST API entry point), etcd (cluster state store), kube-scheduler (assigns pods to nodes), kube-controller-manager (maintains desired cluster state via controllers like the Replication Controller).

**Q5. What is etcd and what does it store?**
> etcd is a distributed key-value store used by Kubernetes to store all cluster state: node status, pod/service details, namespaces, configuration, replication information. It is the "source of truth" for the Kubernetes cluster.

**Q6. What is self-healing in Kubernetes?**
> Kubernetes automatically restarts failed containers, reschedules pods from failed nodes to healthy ones, kills containers that fail health checks, and doesn't expose containers to traffic until they're ready. This reduces operational burden and improves reliability.

**Q7. What is the role of kubelet on worker nodes?**
> kubelet is an agent running on each worker node that receives pod specifications from the API server and ensures the described containers are running and healthy. If a container fails, kubelet restarts it.

---

# 15. Microservices Architecture

## 15.1 What are Microservices?

**Microservices** is an architectural style that structures an application as a collection of small, autonomous services, each modeled around a specific business domain.

Each microservice:
- Handles one specific business capability.
- Operates independently.
- Can be developed, deployed, and scaled separately.
- Communicates with other microservices through APIs (REST, gRPC, message queues).

Contrast with **Monolithic Architecture**: all components are tightly integrated into a single application, deployed as one unit.

## 15.2 Key Characteristics

### Independent Components
- Each service handles one business function (e.g., order processing, payments, inventory).
- Services can be in different languages, frameworks, or storage backends.
- A failure in one service doesn't crash others.

### Decentralized Structure
- No central governance.
- Teams can independently choose their technology stack.
- Databases are also decoupled per service.

## 15.3 Why Microservices?

| Category | Advantage |
|---|---|
| **Business** | Organized around business capabilities; cross-functional teams; faster product delivery |
| **Technical** | Independent scalability, resilience (isolated failures), technology flexibility |
| **Development** | Smaller codebases, parallel development, faster deployment, reduced risk |

## 15.4 Real-World Example: Pizza Delivery Service

```
Order Processing Service ──► Payment Processing Service
         │                              │
         ▼                              ▼
Kitchen Management Service ──► Delivery Tracking Service
```

Each service communicates via APIs but operates independently. If the Payment Service goes down, the Order Processing Service can still accept orders.

## 15.5 Microservices + Docker

Microservices pair perfectly with Docker:

| Benefit | How Docker Helps |
|---|---|
| **Task Isolation** | Each microservice runs in its own container |
| **Technology Flexibility** | Different containers can use different languages |
| **Resource Efficiency** | Lightweight containers vs full VMs per service |
| **Data Management** | Each service can have its own persistent storage |

## 15.6 Asynchronous Communication with Message Queues

When microservices are decoupled via a **message queue** (e.g., RabbitMQ, Kafka):

- Services communicate by sending **messages** to a queue.
- Even if a consuming service (e.g., stock management) goes down, **messages accumulate in the queue**.
- When the service comes back up, it processes the queued messages.
- Other services continue functioning without being blocked.

This asynchronous, decoupled communication enables **resilience** — the failure of one service doesn't cascade to others.

## 15.7 Docker Compose for Microservices

**Docker Compose** simplifies multi-container microservice management vs. manual Dockerfile management:

| Feature | Docker Compose | Manual |
|---|---|---|
| Configuration | Single `docker-compose.yml` | Multiple Dockerfiles + commands |
| Starting all services | `docker-compose up` | Multiple docker run commands |
| Networking | Automatic container name DNS | Manual IP management |
| Scaling | `docker-compose up --scale service=N` | Multiple manual starts |
| Environment variables | Defined in compose/`.env` file | Passed manually per run |

---

## ❓ Q&A: Microservices

**Q1. What is microservices architecture and how does it differ from monolithic architecture?**
> Microservices architecture decomposes an application into small, independent services each responsible for a specific business function. They communicate via APIs and can be deployed independently. Monolithic architecture packages all functionality into a single, tightly coupled codebase deployed as one unit.

**Q2. What are the main advantages of microservices?**
> Independent scaling (scale only the bottlenecked service), fault isolation (one service failing doesn't crash the whole app), technology flexibility (different stacks per service), parallel development (teams work on separate services independently), and faster deployment cycles.

**Q3. How does a message queue help with microservices resilience?**
> A message queue decouples producers and consumers. If a consumer service goes down, messages accumulate in the queue rather than being lost. The producer continues working. When the consumer recovers, it processes the queued messages. This prevents cascading failures.

**Q4. Why is Docker an ideal tool for microservices deployment?**
> Each microservice runs in its own container with all its dependencies. This provides isolation, consistent environments across stages (dev/test/prod), easy scaling, and technology flexibility. Docker also enables CI/CD pipelines to build and deploy each microservice independently.

**Q5. What is the difference between Docker Compose and Kubernetes for microservices?**
> Docker Compose is simpler, designed for single-host multi-container applications — good for development and small deployments. Kubernetes is a full orchestration platform designed for multi-host clusters, providing auto-scaling, self-healing, service discovery, rolling updates, and production-grade reliability.

---

# 🗂️ Quick Reference: All Key Terms

| Term | Definition |
|---|---|
| **Virtualization** | Abstracting physical resources into logical views |
| **VMM / Hypervisor** | Software managing virtual machines |
| **Type 1 Hypervisor** | Bare metal hypervisor (runs directly on hardware) |
| **Type 2 Hypervisor** | Hosted hypervisor (runs on top of host OS) |
| **Paravirtualization** | Guest OS modified to use hypercalls |
| **Full Virtualization** | Guest OS unmodified; hypervisor emulates hardware |
| **Hypercall** | Guest OS call to hypervisor (like a system call to VMM) |
| **Trap and Emulate** | Guest instructions trap to VMM; VMM emulates them |
| **Binary Translation** | Hypervisor rewrites sensitive instructions before execution |
| **VT-x / AMD-V** | Hardware extensions making x86 properly virtualizable |
| **VMX Root/Non-Root Mode** | Two hardware modes for hypervisor and guest respectively |
| **VMCS** | Hardware structure defining VM control; stores guest state |
| **VM Exit** | Hardware transfer from guest (non-root) to hypervisor (root) |
| **Shadow Page Table** | VMM-maintained table mapping gVA→sPA directly |
| **EPT (Extended Page Table)** | Hardware two-level page walk (gVA→gPA→sPA) |
| **I/O Virtualization** | Abstracting I/O devices for VMs |
| **Split Driver** | Frontend in guest + backend in Dom0 (Xen I/O model) |
| **Direct I/O (Passthrough)** | VM accesses physical device directly |
| **Popek-Goldberg** | Formal requirements for virtualizable architectures |
| **Sensitive Instruction** | Instruction that accesses/modifies critical hardware state |
| **Privileged Instruction** | Instruction that traps in user mode |
| **VM Migration** | Moving a VM from one host to another |
| **Pre-copy Migration** | Iteratively copy memory, brief pause for final sync |
| **Post-copy Migration** | Transfer CPU state first, memory on-demand after |
| **Container** | OS-level isolated environment sharing host kernel |
| **Namespace** | Linux feature partitioning kernel resources |
| **cgroups** | Linux feature limiting/accounting resource usage |
| **Docker** | Platform for building and running containers |
| **Dockerfile** | Script defining how to build a Docker image |
| **Docker Image** | Read-only template for creating containers |
| **Docker Container** | Running instance of an image |
| **Docker Registry** | Storage for Docker images (e.g., Docker Hub) |
| **UnionFS** | Filesystem merging multiple layers into one view |
| **Copy-on-Write (CoW)** | Modifying shared files by copying to private layer first |
| **DevOps** | Unifying development and operations practices |
| **CI (Continuous Integration)** | Auto-merge, build, and test every code change |
| **CD (Continuous Delivery)** | Auto-deploy to staging; manual production push |
| **CD (Continuous Deployment)** | Fully automatic deployment to production |
| **Kubernetes (K8s)** | Container orchestration platform |
| **Pod** | Smallest K8s deployable unit; group of containers |
| **Service (K8s)** | Stable VIP abstracting a group of pods |
| **Deployment (K8s)** | Declarative pod management with rolling updates |
| **etcd** | K8s distributed key-value cluster state store |
| **kubelet** | K8s worker node agent managing pod containers |
| **Microservices** | Architecture decomposing app into independent services |
| **Message Queue** | Asynchronous communication broker (e.g., RabbitMQ) |

---

*📝 These notes cover the complete Unit 2 content. Good luck with your exams!*