* Processes can execute concurrently and may be interrupted at any time 
* Concurrent access to shared data may result in data inconsistency

#### Race Condition  and Critical Section Problem
* When 2 processes try to fork() at the same time, what output must `next_available_pid` give to each one of them?
* This condition is called as Race
* Each process has critical **section segment** of code
	* Process maybe changing common variables, updating table or writing to file 
	* When one process in is a critical section, no other process can also be in a critical section
	* Each process must ask permission to enter critical section in entry section 
* Following is the structure of a process 
```C
do{
	//entry section 
		critical section 
	//exit section 
		remainder section 
} while (true);
```

#### Solution to Critical Section Problem 
* **Mutual Exclusion**
	* If process Pi is executing in critical section, then no other process can be executing in their critical sections 
* **Progress**
	* If no process is executing in its critical section and their exists some processes that wish to enter their critical section, then the selection of the process that will enter the critical section next, cannot be postponed indefinitely 
* **Bounded Waiting**
	* A bound must exist on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted