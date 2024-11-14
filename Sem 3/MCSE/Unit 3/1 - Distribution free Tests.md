#### Non-Parametric Tests
* **Rank-based** methods are used when we do not know about the population distribution form from which the data is sampled
* Used for small sample sizes 
* Overcome the requirements of a test like **t-statistic** (Data has to come from a normal distribution)
#### Types of Tests
* **Wilcoxon Signed Rank Test**
	* It is analogous to one sample **t-test**
* **Wilcoxon Rank Sum Test**
	* It is analogous to two sample **t-test**
#### Wilcoxon Signed-Rank Test
* We begin by assigning ranks based on how close the deviation values are, from zero, regardless of signs
* Then, signs are assigned based on the negative or positive deviation and **S+** & **S-** are individually computed 
* **(S+) + (S-) = n(n+1)/2**
* Large values of S+ will provide evidence against a null hypothesis of the form **H0 : μ ≤ μ0**
* Small values of S+ will provide evidence against a null hypothesis of the form **H0 : μ ≥ μ0**
**Note - In case of ties, assign the ranks based on the average of the ranks, each of the tied values would have gotten**
Eg: 3, 4, 4, 5, 7 would be ranked 1, 2.5, 2.5, 4, 5
**Note - If one of the values coincides with the value of the observed mean, the difference would be 0. Drop this value and consider the sample size to be reduced by the number of these observations**

#### Wilcoxon Rank-Sum Test
* Used to test the difference in population means in cases where the population are not normal
* To use this test
	* **Populations must be continuous**
	* **Their Probability density functions must be identical in shape and size**
* Arrange all the values of both the samples by assigning ranks starting from the smallest value
* The total number of ranks would be equal to m+n
* Find **W** which is the rank-sum test statistic for either of the samples. Using the table find the P value

#### Large Sample Approximation
* When the sample size is >20 it approximately normally distributed 
* In such a case, using S+ 
	* **Mean = n(n+1)/4**
	* **Variance = n(n+1)(2n+1)/24**
	* Z score is calculated using the above values to find the P value
		* **z = [ (S+) - n(n+1)/4 ] / root[ n(n+1)(2n+1)/24 ]**
* In such a case, using W
	* **Mean = m(m+n+1)/2**
	* **Variance = mn(m+n+1)/12**
	* Z score is calculated using the above values to find the P value
		* **z = [ W - m(m+n+1)/2 ] / root[ mn(m+n+1)/12 ]**

**Note - Distribution free tests are not free of assumptions**

**Note - If P>0.05, accept H0 else if P<0.05, reject H0** (This is applicable for significance level alpha)