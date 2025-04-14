#### Features 
* A thread is a fundamental unit of CPU utilization and forms the basis of multi-threaded computer systems
* A thread consists of the following 
	* **Thread ID** 
	* **Program Counter** 
	* **Register Set** 
	* **Stack**
* A thread automatically shares its data, code, file descriptors and signals, with other threads which are working on the same process 
* Threads run within application 
* Multiple tasks of an application can be implemented with separate threads
* Process creation is heavy whereas thread creation is comparatively much lighter
* Following are the benefits of threads 
	* **Responsiveness** 
		* It allows continued execution of a process even if a part of it is blocked or performing a lengthy operation
	* **Resource Sharing** 
		* Threads share resources automatically. It is much easier than shared memory or message passing 
	* **Economy** 
		* It is cheaper than process creation and thread switching has a much lower overhead compared to context switching 
	* **Scalability** 
		* Threads can run on multi-cores parallelly 

#### Multi-core Processing 
* **Parallelism** implies that a system can perform more than one task simultaneously
	* **Data Parallelism**
		* Distributes subsets of the same data across multiple cores, doing the same operations on each
	* **Task Parallelism**
		* Distributes threads across cores, each thread performing unique operation
* **Concurrency** supports more than one task making progress 

#### Amdahl's Law 
![[Pasted image 20250220233902.png]]
* **S** is the portion of the task which needs to be done in serial
* **N** is the number of processing cores 

#### User Threads and Kernel Threads 
* **User Threads** - management done by user-level threads library 
	* Three primary threads libraries
		* **POSIX** 
		* **Windows Threads** 
		* **JAVA Threads** 
* **Kernel Threads** - supported by kernel 
	* All OS support this 