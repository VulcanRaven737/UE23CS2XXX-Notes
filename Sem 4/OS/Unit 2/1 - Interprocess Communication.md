#### Independent and Cooperating Processes 
* Process within a system maybe independent or cooperating
* Cooperating process can affect or be affected by other processes, including sharing data 
* Independent process cannot affect or be affected by the execution of another process 
* Advantages of cooperating processes are
	* **Information sharing** 
	* **Computation speedup**
	* **Modularity**
	* **Convenience** 
* Cooperating processes use IPC (Interprocess Communication)
* There are 2 models of IPC
	* **Shared Memory** 
	* **Message Passing** 
#### Shared Memory 
* Shared memory allows two or more processes to share a given region of memory
* It is the fastest form of IPC since data does not have to be copied between client and server 
* Synchronization has to be done such that a read and write operation to the same memory segment does not take place simultaneously as it may lead to a deadlock 
* **Semaphores** are used to sync shared memory access

#### Message Passing 
* Mechanism for processes to communicate and to sync their actions 
* Processes communicate with each other without resorting to shared variables 
* IPC provides 2 operations 
	* Send message
	* Receive message
* When two processes wish to communicate they need to establish a communication link between them and exchange messages via the 2 operations, send/receive 
* Implementation of communication link is done in the following ways
	* **Physical** 
		* Shared Memory 
		* Hardware Bus 
		* Network 
	* **Logical
		* Direct or Indirect 
		* Synchronous or Asynchronous 
		* Automatic or explicit buffering 

#### Direct Communication 
* For direct communication processes must name each other explicitly 
	* **send (P,message)**
	* **receive (Q, message)**
* Following are the properties of direct communication link 
	* **Links are established automatically** 
	* **A link is associated with exactly one pair of communication processes** 
	* **Between each pair there can exists exactly one link** 
	* **The link is usually bi-directional** 

#### Indirect Communication 
* Messages are directed and received from mailboxes for indirect communication
	* Each mailbox has a unique id and the processes can communicate only if they share a mailbox 
* Properties of indirect communication link 
	* **Link established only if processes share a common mailbox** 
	* **A link maybe associated with many processes** 
	* **Each pair of processes may share several communication links**
	* **Link is mostly bidirectional but it can also be unidirectional**

#### Message Passing 
* Message passing may be either blocking or non-blocking 
* **Blocking is considered as Synchronous**
	* Blocking send - the sender is blocked until the message is received 
	* Block receive - the receiver is blocked until a message is available
* **Non-Blocking is considered as Asynchronous**
	* Both the sender and receiver can send or receive at any point of time 
* **If both send and receive are blocking, a state of rendezvous is reached between the sender and receiver**

#### Buffering 
* Queues of messages are attached to the link and the messages temporarily reside in this queue or buffer 
* There are 3 main types 
	* **Zero Capacity - No messages are queued on a link. Sender must wait for the receiver (rendezvous state)**
	* **Bounded Capacity - Finite length of messages hence, sender must wait if link full**
	* **Unbounded Capacity - Infinite length hence sender never has to wait to send a message** 