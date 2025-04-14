* There are 2 types of Multiprocessor Systems 
	* **Asymmetric Multiprocessing** 
		* Each processor is assigned a specific task 
		* No shared memory present
	* **Symmetric Multiprocessing** 
		* Each processor perform all tasks 
		* Shared memory exists between all processor

### Symmetric Multiprocessing Architecture 
* All processors are peers and there does not exist a master-slave relationship
* Each processor has its own set of registers as well as local cache 
* Each processor shares the physical memory 
* Recently multi-cores is becoming more popular than multi-processors since they are more efficient due to on-chip communication 
* **Blade Servers** are a recent development in which multiple processor boards, IO boards and networking boards are placed in the same chassis

### Multitasking 
* Timesharing or multitasking is a logical extension in which CPU switches jobs so frequently that users can interact with each job while it is running, creating an interactive computing experience 
* Every user has atleast one program executing in memory and if several jobs are available then CPU scheduling occurs
* If processes don't fit in memory, swapping takes place

#### Dual-Mode and Multimode Operation 
* Dual-Mode operation allows OS to protect itself and other system components
* It provides the ability to distinguish when system is running user code or kernel code
* Some instructions which are designated as privileged are only executable in kernel mode 
* Syscalls can change the mode of operation  

NOTE - User Mode sets the **mode bit** to 1 whereas, in Kernel Mode, the mode bit is set to 0