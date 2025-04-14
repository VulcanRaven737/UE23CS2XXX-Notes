#### Processor vs Memory 
* Program is stored in memory and the data required by the program is also stored in memory
* Processor executes the program faster than accessing the instruction and data from the memory 
* CPU performance increases by 55% whereas memory performance increases by 7% per year, and this gap increases year on year 
* Fast memory tech is more expensive per bit
* **Memory Latency - Time it takes to transfer a word of data to or from memory**
* **Memory Bandwidth - Number of bits or bytes that can be transferred in one second**

#### Cache Memory
* Cache is an architectural arrangement which makes the main memory appear faster to the processor than it really is
![[Pasted image 20250405123533.png]]
* Processor is much faster than the main memory and as a result, the processor has to spend much more of its time waiting while instructions and data are being fetched from the main memory
* This is where cache memory comes which removes the major obstacle to achieve good performance
* Cache memory is based on the property of computer programs known as **'Locality of Reference'**

#### Principle of Locality 
* Temporal locality (locality in time) - If an item is referenced, it will tend to be referenced again soon
	* Recently executed instruction is likely to be executed again very soon, hence it should be brought into a cache hoping that it will be used again 
* Spatial locality (locality in space) - If an item is referenced, items whose addresses are close by will tend to be referenced soon
	* Instead of fetching just one item from main memory to the cache at a time, several items having addresses adjacent to the item being fetched maybe useful
	* A 'block' of address locations are fetched into the cache
