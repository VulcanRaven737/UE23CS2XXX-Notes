#### Terminology
* **Cache Lines** 
	* It is the smallest chunk of memory that can be transferred between the main memory and the cache 
	* It is a fixed-size block of commonly 32, 64 or 128 bytes
	* Helps with **spatial locality**
* **Block**
	* It is the same as cache lines but a theoretical way
* **Cache Hit and Cache Miss**
	* It is called cache hit, when the referenced bit needed by processor is found in the cache itself 
	* It is called cache miss, when the referenced bit is not found in the cache and needs to be brought from main memory to the cache

#### Cache Lookup Process
* When accessing memory in a direct-mapped cache, each memory address is split into 
	* Index Bits (Last 2)
	* Tag Bits (First 2)
* How is a block found?
	* **CPU gets an address**, say `1101` (`Tag = 11`, `Index = 01`)
	* **Goes to cache line 01** (from index).
	* **Compares tag in cache** (should be 11).   
	- If it **matches**, it’s a **cache hit** and the block is found  
	- If it **doesn't match**, it's a **cache miss**, and data is fetched from main memory.

#### Load through or Early restart
* When the processor request a word from the memory
	* **Block Transfer**
		* Instead of fetching just the requested word, a whole block of words containing that word is fetched from the memory
		* This is to take advantage of spatial locality
	* **Forward after Full Block**
		* Processor waits until the entire block is transferred before it receives the desired word
	* **Early Forwarding (Early Restart)**
		* With load-through or early-restart, the requested word is sent to the processor as soon as it arrives, without waiting for the entire block to be transferred
* In **Write-Through** the memory is updated as soon as cache is updated. Slower method due to frequent writing to memory
	* It avoids inconsistency but also dishonors the purpose of having cache
	* Provides with the **cache coherence problem**
* In **Write-Back** the memory is not updated when the cache changes. It waits till the entire block of cache is changed again and only then updates the memory. This leads to inconsistencies if not handled correctly. This method is faster though

#### Write Allocate vs Write no Allocate
* **Write Allocate (Fetch on Write)**
	* Bring the block from memory into the cache and then perform the write operation in the cache
	* Used with Write-Back policy 
* **Write No Allocate (Write Around)**
	* Do not bring the block into the cache and instead directly write to the main memory 
	* Used with Write-Through policy 
	* 'Why load it into the cache if its not going to be used again soon?'