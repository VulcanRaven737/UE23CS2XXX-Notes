#### Pipes 
* Following are the limitations of pipes 
	* They have always been half duplex that means data flows only in one direction 
	* They can be used only between processes that have a common ancestor. When a process calls fork, the pipe is used to communicate between the parent and the newly created child
	* **Pipe is created by calling the `pipe()` function**
		* Returns 0 if successful 
		* Returns -1 on error
	* The data in the pipe flows through the kernel
	* **`popen() and pclose()`**
		* Used to create a pipe to another process to either read its output or send it input 
		* These 2 functions handle all work like creating a pipe, forking a child and closing the unused ends of the pipe
		* popen() does a fork and exec to execute the cmdstring and returns an IO file pointer. If type is 'r' the file pointer is connected to standard output of cmdstring, It type is 'w' the file pointer is connected to standard input of the cmdstring

#### FIFOs
* They are called as **named pipes**
* Unnamed pipes can only be used between processes which have the same ancestor which has created the pipe
* With FIFO, two unrelated processes can communicate with each other and exchange data 
``` c
#include <sys/stat.h>
int mkfifo (const char *path, mode_t mode);
int mkfifoat (int fd, const char *path, mode_t mode)
```
* **`mkfifo()`** **creates a named pipe (FIFO) at the specified `path`.**
	- `mode` specifies the permissions (similar to file permissions, e.g., `0644` for read/write).
- **`mkfifoat()`** works **similar to `mkfifo()`**, but with an additional `fd` parameter.
	- If `path` is **relative**, it is interpreted **relative to the directory referenced by `fd`**.
	- If `path` is **absolute**, `fd` is ignored.
	- Useful when working with **file descriptors to directories** (e.g., using `open()` with `O_DIRECTORY`).
- FIFOs are used by shell commands to pass data from one shell pipleline to another without having to create an intermediate temp file
- FIFOs are used as rendezvous points in client-server application which helps to pass data between the clients and the servers 

#### Message Queues
* A message queue is a linked list of messages stored within the kernel 
* `msgget` is used to create or open an existing queue
```C
#include <sys/msg.h>
int msgget (key_t key, int flag);
```
* New messages are added to the end of a queue by `msgsnd`
```C
#include <sys/msg.h>
int msgsnd (int msgid, const void *ptr, size_t nbytes, int flag);
```
* Every message appended to the queue has a positive long integer type field, a non-negative length and the actual data itself 
* Messages are fetched from the queue using `msgrcv`
```C
#include <sys/msg.h>
ssize_t msgrcv(int msgid, void *ptr, size_t nbytes, long type, int flag)
```

#### Semaphores 
* A semaphore is a counter which is used to provide access to a shared data object for multiple processes 
* A process needs to do the following to obtain a shared resource 
	* **Test the semaphore which is responsible for giving control** 
	* **If value of semaphore is +ve, the process can use the resource. When this happens, the process decrements the value of the semaphore by 1, indicating that it has used up 1 unit of the resource** 
	* **If the value of the semaphore is 0, the process goes to sleep and waits until the semaphore value is greater than 0**
	* **Once the process is done using the shared resource, it increments the semaphore value thus making it 1 again**
	
* `semget` - used to obtain the semaphore ID
```C
#include <sys/sem.h>
int semget (key_t key, int nsems, int flag);
```

* `semctl` - it is the catchall for various operations 
```C
#include <sys/sem.h>
int semctl (int semid, int semnum, int cmd);
```

* `semop` - used to atomically perform an array of operations on a semaphore set 
``` C
#include <sys/sem.h>
int semop (int semid, struct sembuf semoparray[], size_t nops);
```
`semoparray arguement is a pointer to an array of semaphore ops`

#### Shared Memory 
* It allows two or more processes to share a given region of memory
* It is the fastest form of IPC
* `shmget` - used to obtain a shared memory ID
```C
#include <sys/shm.h>
int shmget (key_t key, size_t size, int flag);
```

* `shmctl` - it is the catchall for various operations 
``` C
#include <sys/shm.h>
int shmctl (int shmid, int cmd, struct shmid_ds *buf);￼
```
`cmd arguement specifies one of the commands to be perfoemd on the segment specified by shmid`

* `shmat` is used to attach a process to the shared memory segment once its created
``` C 
#include <sys/shm.h>
int shmat (int shmid, const void *addr, int flag);
```

* `shmdt` is used to detach the process from a shared memory segment 
``` C
#include <sys/shm.h>
int shmdt (const void *addr);
```

* A shared memory segment can be removed by calling `shmctl` with a command of IPC_RMID