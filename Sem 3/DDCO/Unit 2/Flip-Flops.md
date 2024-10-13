#### Why FF's
* When latches are used for the storage elements, a serious difficulty arises. The state transitions of the latches start as soon as the clock pulse changes to the logic 1 level. 
* If the inputs applied to the latches change while the clock pulse is still at the logic 1 level, the latches will respond to new values and a new output state may occur. The result is an unpredictable situation. 
* In contrast, A FF triggers only during a signal transition (from 0 to 1 or from 1 to 0) of the synchronizing signal (clock) and is disabled during the rest of the clock pulse.

#### Types of FF's
* **S-R Flip Flop**
	* Using NOR Gate
		* Requires a S-R NOR Latch and 2 AND gates
	* Using NAND Gate
		* Requires a S-R NAND Latch and 2 NAND gates
	* **Drawback is the fact that when S = 1 & R = 1, the circuit enters an indeterminate state**
* **D Flip Flop**
	* Edge Triggered Master-Slave D Flip Flop
		* When clock = 1, master is enabled and when clock = 0, slave is enabled
		* It is **negative-edge** triggered 
* **J-K Flip Flop**
	* Overcomes the drawback faced by S-R Latch
	* **When J = 1 & K = 1, the FF enters a toggle state rather than an indeterminate state**
* **T Flip Flop**
![[Pasted image 20240919161843.png]]![[Pasted image 20240919161902.png]]![[Pasted image 20240919161927.png]]
#### Response of a FF to input data
* **Setup Time** - It is the minimum time during which the D input must be maintained at a constant value prior to the occurrence of the clock transition
* **Hold Time** - It is the minimum time during which the D input must not change after the application of the positive transition of the clock
* **Propagation Delay Time** - It is defined as the interval between the trigger edge and the stabilization of the output to a new state

#### Differentiating between Positive Edge Triggered and Negative Edge Triggered 
* The indicator (>) denoted the fact the the FF responds to the edge transition of the clock
* A bubble outside the block adjacent to the indicator designates a negative edge trigger while the absence of a bubble designates a positive edge trigger response 
![[Pasted image 20240919162952.png]]
#### Characteristic Equations of FF's
* **D Flip-Flop**
	* *Q(t + 1) = D*
* **J-K Flip-Flop**
	* *Q(t + 1) = JQ'+ K'Q*
* **T Flip-Flop**
	* *Q(t+1) = TQ' + T'Q = T xor Q*

#### Conversion of Flip-Flops
* **S-R Flip-Flop to J-K Flip-Flop**
* **J-K Flip-Flop to S-R Flip-Flop**
* **S-R Flip-Flop to D Flip-Flop**
* **D Flip-Flop to S-R Flip-Flop**
* **J-K Flip-Flop to T-Flip-Flop**
* **J-K Flip-Flop to D Flip-Flop**
* **D Flip-Flop to J-K Flip-Flop**

#### Direct Inputs 
* Flip-Flops also have asynchronous inputs that are used to force the FF to a particular state independent of the clock signal
* Input that sets FF to 1 is called preset or **Direct Set**
* Input that clears FF to 0 is called clear or **Direct Reset**
* Direct inputs are useful for bringing all FF's in the system to a known starting state prior to the clocked operation
