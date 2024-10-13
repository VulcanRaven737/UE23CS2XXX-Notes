* It is a single numeric value specified for the data which is also referred to as sample statistic
* Point estimate infers about the population parameters
* It is also used to make an estimation of unknown population parameters, including population mean, SD and proportion
#### Properties of Point Estimator
* **Bias** 
	* When the expected value of an estimator is different from the value of the parameter that is being estimated 
	* When they are equal they are called unbiased 
	* **Normal Distribution is Unbiased**
* **Consistency**
	* Portrays how close the point estimator can be to the true value of the parameter even if it increases in size
	* To be more consistent the mean of the sample should move towards the true value of the population parameter
* **Efficiency**
	* A very efficient point estimator should have the following 
		* Smallest Variance 
		* Unbiased Observation 
		* Consistent
#### Mean Squared Error (MSE)
* We determine how good a point estimator is, using MSE
* We use **Maximum Likelihood Estimator (MLE)** to construct a point estimator
* MSE combines bias and uncertainty
* MSE = Variance + Bias^2
	* Bias = Mean - Observation
* MSE for the following distributions
	* Poisson
		* **MSE = μ/n**
	* Binomial 
		* **MSE = (p(1-p))/n**
	* Bernoulli
		* **MSE = (p(1-p))/n**\
	* Normal
		* **MSE = σ^2/n**
