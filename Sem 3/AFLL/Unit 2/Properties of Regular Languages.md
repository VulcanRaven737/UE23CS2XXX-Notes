* Regular Expressions and Regular Grammars are considered as generators of Regular Languages
* Finite Automate (DFA, NFA, E-NFA) are considered as acceptors of Regular Languages
* Properties of Regular Languages can be broadly classifies as 
	* **Closure Properties**
	* **Decision Properties**
#### Closure Properties
* Carry out operations on one or more languages to obtain a new language
* **Complementation**
	* If a language L is regular, its complement L^c is also regular.
	* Let M(L) denote the DFA for the language L. Modify the DFA as follows to obtain M(L^c).
		* **Change the final states to non-final states.**
		* **Change the non-final states to final states.**
* **Union**
	* If L1 and L2 are regular, then L1 ∪ L2 is regular.