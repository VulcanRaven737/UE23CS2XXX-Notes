* OS provides an environment for execution of programs and services to programs and users 
* Following are set of some OS services 
	* **User Interface** 
	* **Program Execution** 
	* **IO Operation** 
	* **File System Manipulation** 
	* **Communications** 
	* **Error Detection** 
	* **Resource allocation** 
		* When multiple users or multiple jobs are running concurrently, resources must be allocated properly to each one of them 
	* **Accounting** 
		* Keep track of which users have used how much and what kinds of computer resources 
	* **Protection and Security** 

### System Calls 
* Provide an interface to the services made available by an OS
* These calls are available as routines written in a higher programming language or ASM
* Following are the groups of all syscalls available 
	* **Process Control**
		* fork()
		* exit()
		* wait()
	* **File Manipulation** 
		* open()
		* read()
		* write()
		* close()
	* **Device Manipulation** 
		* ioctl()
		* read()
		* write()
	* **Information Maintenance** 
		* getpid()
		* alarm()
		* sleep()
	* **Communication** 
		* pipe()
		* shm_open()
		* mmap()
	* **Protection** 
		* chmod()
		* unmask()
		* chown()
* When a C program calls the printf() function, the C library actually intercepts this function and invokes the system call write() in the OS

### Policy vs Mechanism 
* Policy is **What will be done?**
* Mechanism is **How to do it?**
* The separation of policy from mechanism is a very important principle and it allows maximum flexibility if policy decisions are to be changed later
* OS is implementation using a mix of ASM for low levels, C for the main body, C, C++ and other scripting languages like PERL, Python and shell scripts for systems programs

### Process Concept 
* An OS executes a variety of programs 
	* Batch System - jobs
	* Time shared systems - user programs or tasks
* **Process is a program in execution**. It is an active entity 
* Program is a passive entity stored on disk as an executable file 
* A single program can have multiple services 
* Following is the structure of a process in memory 
	* **Text Section** 
		* Contains the program code and also current activity along with the program counter and processor registers 
	* **Stack** 
		* Contains temporary data like function parameters, return addresses, local variables 
	* **Data Section** 
		* Contains global variables 
	* **Heap** 
		* Contains memory which is dynamically allocated during run time 
* As a process executes, it changes state 
	* **New - The process is being created** 
	* **Running - Instructions are being executed** 
	* **Waiting - The process is waiting for some event to occur** 
	* **Ready - The process is waiting to be assigned to a processor** 
	* **Terminated - The process has finished execution** 
* Each process is represented in the OS by a Process Control Block also called the task control block
	* **Process State** - running, waiting, etc
	* **Program Counter** - location of instruction to next execute
	* **CPU Registers** - contents of all process-centric registers
	* **CPU Scheduling Information** - priorities, scheduling queue pointers 
	* **Memory Management Information** - memory allocated to the process 
	* **Accounting Information** - CPU used, clock time elapsed since start time
	* **IO Status Information** - IO devices allocated to process