* Every process has a unique process ID, a non negative integer
* It is the only well known identifier of a process that is always unique
* Process IDs are reused as and when their previous owners are terminated 
* UNIX systems implement algorithms to delay reuse, so that newly created processes are assigned IDs different from those used by processes that terminated recently 

#### fork()
* An existing process can create a new one by calling the fork() function
* The new process created by fork() is a child process
* Return value of child is 0 when successful. Return value to the parent is the PID of the child 
* On failure return value is -1
* The child gets a copy of the parent's data space, heap and stack 

#### exit()
* A process can terminate in 5 steps 