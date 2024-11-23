#### Correlation
* Gives us the relationship between two variables and how to use one variable to predict the other variable. This kind of data is called bi-variate data
* Analysis of bi-variate data can be done using 
	* **Analytical Technique**
		* Correlation Co-efficient 
		* Regression Analysis
	* **Visualization Technique**
		* Scatter Plot 
* Correlation involves an **independent variable** and a **dependent variable**. They are usually specified as a set of **(Regressor, Regresand)**
* Correlation Coefficient ![[Pasted image 20241122095015.png]]
* Type of correlation 
	* **Positive Correlation** 
		* 0 < r < 1
		* if **r = +1** it is a perfect positive correlation
	* **Negative Correlation** 
		* -1 < r < 0
		* if **r = -1** it is a perfect negative correlation
	* **No Correlation/ Poor Correlation**
		* r = 0
* The correlation coefficient remains unaltered in the following cases 
	* **Interchanging the values of X and Y**
	* **Adding a constant to each value of a variable** 
	* **Multiplying each value of a variable by a positive constant**
* **Correlation coefficient only measures linear association. If correlation between X and Y is equal to 0 then there is no linear relationship between the two. Instead they made be quadratic**
* Anscombe's Quartet is a classic example used in statistics to highlight the importance of data visualization alongside numerical analysis

#### Confounding Variable 
* It is a third variable that influences both the independent variable as well as the dependent variable causing a **spurious correlation**
* This interferes with your analysis and ruins your experiment by giving useless results
* It may increase **variance** as well as introduce **bias**
* They have a hidden effect on the dependent variables and may be the reason why the dependent variable correlates with the independent variable in the first place
* Confounding can be reduced or eliminated by using controlled experiments wherein the values of factors are pre-defined.
* Alternatively the study must be repeated a number of times under a variety of conditions before drawing reliable conclusions

#### Regression Analysis
* Regression analysis is the study of a set of data to make the best guess or a prediction 
* It is widely used for prediction and forecasting
* Earliest form of regression was **Linear Regression**, in which the researcher find the line that most closely fits the data according to a specific mathematical criterion. This line is called the **Least-Squares Line**![[Pasted image 20241122120906.png]]
	* Above it the equation to find the least squares line. 
		* **β1 is the slope of the line**
		* **y(bar) is the mean of y 
		* **x(bar) is the mean of x 
	* Sometimes there are measurement errors and they need to be accounted for. When data points do not lie on the straight line it means there are errors in the measurement
**Note - 
* **The estimates are not the same as the true values**
* **The residuals are not the same as the errors**
* **Don't use Least Squares Line when the data is not linear**

#### Goodness of Fit
* Goodness of fit statistic is a quantity that measures how well a model explains a given set of data