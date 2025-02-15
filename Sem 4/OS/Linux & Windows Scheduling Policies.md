* Linux kernel in the early stages did not support SMP systems. It has poor performance for large processes (SMP - Symmetric Processing)
* Soon it moved to constant order O(1) scheduling time and supported SMP systems. It had good performance with processor affinity and load balancing between processors
* **Completely Fair Scheduler (CFS)** is now the default scheduling algorithm