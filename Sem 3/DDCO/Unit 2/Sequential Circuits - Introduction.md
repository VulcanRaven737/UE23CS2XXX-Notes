#### Introduction
* Sequential circuits use differ from combinations circuits as they have memory components in them
* Flip-Flops are used as memory components in sequential circuits
* A Flip-Flop is a binary storage device capable of storing one bit of information. The output of a Flip-Flop is either 1 or 0
* The clock signal dictates when the output change takes place

#### Bistable operation of cross-coupled inverters
* Cross coupled inverters have 2 stable states
* N states log2(N) bits of information
* Bistable elements have 2 stable states and stores one bit\

#### Latches and Flip-Flops

| Latches                                                                                                 | Flip-Flops                                                                                                                           |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| They are level triggered (Output is seen as soon as the inputs change)                                  | Flip-Flops are edge triggered  (Only changes states when a control signal goes from high to low or low to high, controlled by clock) |
| Latch continuously checks its inputs and changes its output accordingly                                 | FF checks its inputs only at times determined by the clock signal                                                                    |
| Latch is sensitive to the duration of the pulse and can change output at any time when the switch is on | FF can change its output only when the clock signal changes from low to high or vice-versa                                           |
| Works on the basis of enable input signal                                                               | Works on the basis of clock pulse signal                                                                                             |
| They are building blocks of the sequential circuits                                                     | The are built using Latches                                                                                                          |
#### Types of Latches 
* S-R Latch (Constructed using NOR or NAND Gates)
* D Latch (Transparent Latch)
**Note - When the enable input is asserted, only then the data input can change values. If the the enable input is deasserted the binary information which was present at the data input before the transition occurred is stored**
