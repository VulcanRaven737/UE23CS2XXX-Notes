#### Formal Definition of DFA
* M = **(Q, ∑, δ, q0, F)**
	* **Q** is a finite set of states
	* **∑** is a finite non-empty set of input symbols
	* **δ** is the transition function where **δ: Q × ∑ → Q**
	* **q0** is the initial state from where input is taken **(q0 ⊆ Q)**
	* **F** is the set of final states **(F ⊆ Q)**
* M can be defined in 3 ways
	* Transition Diagram/State Diagram
	* Transition Table
	* Transition Function
* FSM is considered a DFA only when
	* If each of its transitions is uniquely determined by its source state and input symbol
	* For every state, a transition is required for all the input symbols in **∑** 
	* In DFA, for each state there must be exactly one transition defined for each symbol in **Σ**. This is the “**deterministic”** part of DFA.

**Note - Deterministic means, if you put the automata in the same situation twice, it is guaranteed to make the same choice**