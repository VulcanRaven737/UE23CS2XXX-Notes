#### Multi-threading Models 
* **Many to One** 
	* Many user level threads are mapped to a single kernel thread 
	* One thread blocking causes all threads to get blocked 
* **One to One** 
	* Each user level thread is mapped to a kernel thread 
	* Creating a user-level thread creates a kernel thread 
	* More concurrency achieved in comparison to many to one 
	* Windows and Linux makes use of this 
* **Many to Many**
	* Allows many user level threads to be mapped to many kernel threads 

#### Thread Scheduling 
* In **multithreading**, contention scope determines **how threads compete for CPU time** in a system
* There are 2 types of contention scopes
	* **Process Contention Scope (PCS)**
		- **Definition**: Threads compete **within the same process** for CPU time.
		- **Scope**: The scheduling is handled **at the user level** (user-space thread library).
		- **Scheduling**: The kernel schedules **only the process**, while the thread library schedules threads within the process.
		- **Example**: Many-to-One and Many-to-Many threading models.
		- **Usage**: Used for user-level threads (not directly scheduled by the OS).
	-  **System Contention Scope (SCS)**
		- **Definition**: Threads compete **across all processes in the system** for CPU time.
		- **Scope**: The kernel directly schedules threads across **all processes**.
		- **Scheduling**: The OS scheduler decides which thread gets CPU time, not the user-space thread library.
		- **Example**: One-to-One and Many-to-Many threading models.
		- **Usage**: Used for kernel-level threads (Pthreads in Linux).
* User level threads are scheduled to run on LWP (Leightweight Process)
	* This is known as **process-contention scope (PCS)** since scheduling competition is within the process
	* Kernel thread scheduled onto CPU using **system-contention scope (SCS)**
* API allows specifying either PCS or SCS during thread creation 
	* **PTHREAD_SCOPE_PROCESS** schedules threads using PCS 
	* **PTHREAD_SCOPE_SYSTEM** schedules threads using SCS 

#### Thread Libraries - Pthreads (POSIX Threads)
* It provides the programmer with an API for creating and managing threads
* There are 2 ways in which a thread library is implemented 
	* **Provide a library entirely in user space with no kernel support** 
		* All code and data structures for the library exist in user space 
	* **Implement a kernel level library** 
		* Code and data structures for the library exist in kernel space
		* Functions are invoked directly by the OS
* Pthreads maybe provided as either a user or kernel level library 

#### Windows Threads 
* Windows implements the Win32 API as its primary thread API
* A windows application runs as a separate process and each process may contain one or more threads 
* Windows uses one-to-one mapping natively but using the **fiber** library it also supports many-to-many model
* Threads are created in the Win32 API using the `CreateThread()` function
* `WaitForSingleObject()` function causes the thread to block until the child thread has exited