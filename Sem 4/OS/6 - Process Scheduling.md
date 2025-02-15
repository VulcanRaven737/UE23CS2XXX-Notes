* To maximize CPU use, quickly switching processes onto CPU for time sharing is necessary
* Process Scheduler selects among available processes for next execution on CPU
* Maintains scheduling queues of processes 
	* Job Queue - Set of all processes in the system
	* Ready Queue - Set of all processes residing in main memory, ready to executed
	* Device Queue - Set of processes waiting for an IO device 

### Schedulers 
* **Short-term Scheduler (CPU Scheduler)** - Selects which process should be executed next and allocated CPU
	* Sometimes the only scheduler available in a system 
	* Short term scheduler is invoked frequently and hence must be fast 
* **Long-term Scheduler (JOB Scheduler)** - Selects which processes should be brought into the read queue
	* Long term scheduler is invoked infrequently and can hence be slow 
* **Medium-term Scheduler** - Added to reduce the degree of multi-programming
	* Removes processes from memory and stores in on disk instead. It brings it back from the disk to continue execution which is called as **swapping** 
* The long-term scheduler controls the degree of multi-programming 
* Processes can be described as either 
	* **IO bound process** - Spends more time doing IO computation and many short CPU bursts  
	* **CPU bound process** - Spends more time doing computation and very few long CPU bursts 
* When a CPU switches from one process to another, it must save the state of the old process and load the saved state for the new process via a context switch 
* Context of a process is represented in the PCB. Context-switch time is overhead, the system does no useful work while switching 

### Process Creation 
* Parent process creates children processes which in turn create other processes forming a tree of processes 
* Processes are identified and managed via a process identifier (pid)
* Following are 3 resource sharing options 
	* Parent and children share all resources 
	* Children share a subset of parent's resources 
	* Parent and child share no resources 
* Execution options 
	* Parent and children execute concurrently 
	* Parent waits until child is terminated 
* Address space 
	* Child is duplicate of the parent 
	* Child has a program loaded into it 
* Example 
	* fork() creates a new process 
	* exec() used after fork() to replace the process' memory space with a new program

### Process Termination 
* Process executes last statement and then asks the OS to delete it using the exit() syscall
* Parent may terminate the execution of children processes using the abort() syscall
* Some OSes do not allow child to exist if its parent has been terminated. It results in a cascading termination whereby all children, grandchildren etc are terminated 
* **If no parent waiting (did not invoke wait()), process is a zombie** 
	* Process is called a zombie when it still exists in the PCB but the parent process has been terminated 
	* This occurs when the parent process does not terminate the process properly 
	* wait() or waitpid() must be called to terminate the process properly in order to not make its child process a zombie process 
* **If parent terminated without invoking wait(), process is an orphan** 
	* It is a process whose parent has already been terminated before it 
	* Occurs when the parent process exits while the child process is still running 
	* It gets adopted by init (pid1) and still continues running
