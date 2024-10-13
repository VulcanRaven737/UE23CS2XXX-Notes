#### Formal Definition of an NFA 
* NFA can be represented by a 5-tuple **M=(Q, ∑, δ, q0, F)** where
	* Q is a finite set of states.	
	* ∑ is a finite non-empty set of input symbols called the alphabet.	
	* δ is the transition function where **δ: Q × ∑ → 2^Q**	
	* q0 is the initial state from where any input is processed **(q0 ∈ Q)**.
	* F is a set of final state/states **(F ⊆ Q)**.
#### Why NFA?
* NFAs are used in the implementation of regular expressions (Thompson's Construction)
* NFA is sometimes easier to construct instead of DFA for that language
#### Transition function of an NFA 
* It is **δ: Q × ∑ → 2^Q**
* Here the power set (i.e., P(Q)=2^Q) has been taken because in case of an NFA,
	from a state, transition can occur to any combination of Q states
#### Why NFA is called Non-Deterministic?
* In automata theory, a finite-state machine is called a deterministic finite
	automaton (DFA), if
	* **Each of its transitions is uniquely determined by its source state and input symbol, and**
	* **Reading an input symbol is required for each state transition.**
* A nondeterministic finite automaton (NFA), does not need to obey the above restrictions of DFA. In particular, every DFA is also an NFA. NFA may have several arrows of the same label starting from a state.
 * A Finite Automata(FA) is said to be non-deterministic if there is more than one possible transition from a state on the same input symbol.
#### Computation in NFA
* For an input string, NFA will create a ‘computation tree’ rather than a computation sequence in case of DFA
* An NFA accepts a string if any one of the paths in the tree leads to some accept state	
* Nondeterminism in NFA causes multiple runs. There can be several or no runs for a given input word
* If there is at least one run that leads to an accepting state, the machine will accept the input string

**Note - Non deterministic means if you put the automata in the same situation twice, it may or may not make the same choice**