#### Formal Definition of E-NFA
* E-NFA can be represented by a 5-tuple **M=(Q, ∑, δ, q0, F)** where
	* Q is a finite set of states.
	* ∑ is a finite non-empty set of input symbols called the alphabet.
	* δ is the transition function where **δ: Q × (Σ∪{E})→ 2^Q** 
	* q0 is the initial state from where any input is processed **(q0 ∈ Q)**.
	* F is a set of final state/states **(F ⊆ Q)**.
* E-transitions let us allow to jump states without reading inputs
#### E-Closure of a state or set of states
* E-closure denotes the set of states that reachable from q by E-transitions including the state q itself

**NOTE - In real time, only DFA can be implemented**