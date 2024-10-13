#### What is Automata Theory?
* Study of abstract computing devices or 'Machines'
* **Automaton** is an abstract computing device
* 4 types of automaton
	* **Finite State Machines**
	* **Pushdown Automata**
	* **Linear-Bounded Automata**
	* **Turing Machine**
#### What is a Finite State Machine (FSM)
* An automaton with a finite number of states is called as a FSM
* It is an abstract mathematical model of computation
* FSM can be in only one finite state at any given time
* It can change from one state to another in response to a sequence of inputs. This is called ***transition***
	* FSM with finite memory is called **Finite State Automata (FSA)**
	* FSM with unbound memory is called a **Turing Machine**
	**Finite State Machine**
	* Acceptors (Simply Accepts or Rejects)
		* Deterministic Finite Acceptor (DFA)
		* Non-Deterministic Acceptor (NFA)
		* Lambda-Non Deterministic Acceptor (LambdaNFA)
	* Transducers (Produce Output)
		* Mealy Machines
		* Moore Machines
#### What is a Formal Language?
* A formal language consists of words or strings whose letters are taken from a finite non-empty alphabet and are well formed according to a specific set of rules called a formal grammar
* Formal Language is defined by means of a formal grammar usually in the form of regular grammar and context free grammar
* Formal Languages are defined in one of three ways
	* Regular Expressions (only for regular languages)
	* Standard Automata (accepts a language)
	* A formal grammar system (generates a language)
		* Regular Grammar - **recognized by FSM**
		* Context-free Grammar - **recognized by PDA**
		* Context-sensitive Grammar - **recognized by LBA**
		* Recursively Enumerable Grammar - **recognized by TM**
