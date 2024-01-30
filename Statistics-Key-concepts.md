# Q1: What's the difference between Correlation and Multi- collinearity ?
Ans:
**Correlation:**

Correlation measures the strength and direction of a linear relationship between two continuous variables. It is a statistical measure that ranges from -1 to 1, where:

- 1 indicates a perfect positive linear relationship,
- -1 indicates a perfect negative linear relationship,
- 0 indicates no linear relationship.

Correlation does not imply causation, and it only captures linear associations between pairs of variables.

**Multi-collinearity:**

Multi-collinearity occurs when two or more independent variables in a regression model are highly correlated, making it challenging to identify the individual contribution of each variable to the dependent variable. It can inflate standard errors and make coefficient estimates unstable.

Here's a simple Python code snippet using the `pandas` library to check correlation and `statsmodels` to detect multi-collinearity:

```python
import pandas as pd
import numpy as np
from statsmodels.stats.outliers_influence import variance_inflation_factor
import statsmodels.api as sm

# Create a sample dataset
data = {'X1': np.random.rand(100),
        'X2': np.random.rand(100),
        'X3': np.random.rand(100),
        'Y': np.random.rand(100)}

df = pd.DataFrame(data)

# Calculate correlation matrix
correlation_matrix = df.corr()

# Display correlation matrix
print("Correlation Matrix:")
print(correlation_matrix)

# Check for multi-collinearity using Variance Inflation Factor (VIF)
X = df[['X1', 'X2', 'X3']]
X = sm.add_constant(X)  # Add a constant term to the predictor matrix
vif = pd.DataFrame()
vif["Variable"] = X.columns
vif["VIF"] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]

# Display VIF values
print("\nVariance Inflation Factor (VIF):")
print(vif)

```
### Output:

```markdown
**Correlation Matrix:**

|      | X1       | X2       | X3       | Y        |
|------|----------|----------|----------|----------|
| X1   | 1.000000 | 0.056477 | -0.026031| -0.008821|
| X2   | 0.056477 | 1.000000 | -0.123652| -0.098301|
| X3   | -0.026031| -0.123652| 1.000000 | -0.025700|
| Y    | -0.008821| -0.098301| -0.025700| 1.000000 |

**Variance Inflation Factor (VIF):**

| Variable | VIF      |
|----------|----------|
| const    | 10.763217|
| X1       | 1.003571 |
| X2       | 1.018463 |
| X3       | 1.015903 |
```

In this example, we generate a sample dataset with three independent variables (`X1`, `X2`, and `X3`) and one dependent variable (`Y`). The code calculates the correlation matrix and displays it. Additionally, it computes the Variance Inflation Factor (VIF) for each independent variable to check for multi-collinearity.

Note that VIF values above a certain threshold (commonly 5 or 10) are often considered indicative of multi-collinearity. High VIF values suggest that the variance of the coefficient estimates is inflated due to strong correlations among predictors.

# Q2: Explain Skewness and Kurtosis along with real time examples, and what to do in those situations?
Ans: **Skewness:**
Skewness is a measure of the asymmetry of a probability distribution. It indicates whether the data is skewed to the left (negative skewness), to the right (positive skewness), or symmetrically distributed (zero skewness). 

$$\text{skew}(X) = E\left[\left(\frac{X - \mu}{\sigma}\right)^3\right]$$

- **Real-time Example:** Suppose you are analyzing the income distribution of a population. If the majority of people have low incomes, with only a few individuals having extremely high incomes, the distribution may be right-skewed. On the other hand, if a few people have very low incomes and the majority have high incomes, the distribution may be left-skewed.

![image](https://github.com/Praveen76/Statistics-Interview-Preparation-Guide/assets/26660076/6ca6922e-800d-48b5-9668-0be4e26ccdfa)

```python
import pandas as pd
from scipy.stats import skew

# Example data
data = [10, 20, 30, 40, 50, 100]

# Calculate skewness
skewness = skew(data)
print("Skewness:", skewness)

```

- **What to do:**
  - **Positive Skewness (Right-skewed):** If the data is right-skewed, you might consider transformations such as taking the logarithm to make the distribution more symmetric.
  - **Negative Skewness (Left-skewed):** If the data is left-skewed, transformations like square root or power transformations may be applied to make the distribution more symmetric.

**Kurtosis:**
Kurtosis measures the "tailedness" of a probability distribution, indicating whether the data has heavy tails or light tails compared to a normal distribution.

$$\text{kurt}(X) = E\left[\left(\frac{X - \mu}{\sigma}\right)^4\right]$$

- **Real-time Example:** Consider the distribution of investment returns. If the distribution has heavy tails, it may indicate that extreme events (positive or negative returns) are more likely than what would be expected in a normal distribution. This would result in higher kurtosis.

```python
from scipy.stats import kurtosis

# Example data
data = [10, 20, 30, 40, 50, 60]

# Calculate kurtosis
kurt = kurtosis(data)
print("Kurtosis:", kurt)
```
- **What to do:**
  - **High Kurtosis (Heavy Tails):** In situations of high kurtosis, one might want to be cautious about extreme events. If necessary, outliers could be investigated to determine if they are valid data points or errors. Alternatively, using robust statistical measures might be considered to mitigate the impact of outliers. **In finance, Kurtosis is used as a measure of financial risk. A large kurtosis is associated with a high level of risk for an investment because it indicates there're high probabilities of extremely large and extremely small returns.**
  - **Low Kurtosis (Light Tails):** If the distribution has low kurtosis, it suggests that extreme events are less likely than in a normal distribution. In such cases, it might be important to understand the potential risks and uncertainties associated with the data. **In finance, a small kurtosis indicates a moderate level of risk because the probabilities of extreme returns are relatively low.**

In both skewness and kurtosis situations, it's crucial to consider the context of the data and the goals of the analysis. Statistical measures should be used to gain insights and guide further investigation rather than as rigid rules for decision-making. Additionally, consulting with domain experts can provide valuable context and help in interpreting the results.