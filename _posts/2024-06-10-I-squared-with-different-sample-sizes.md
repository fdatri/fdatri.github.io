---
layout: post
title: "I² in Random Effects Meta-Analysis: What Happens When Increasing the Sample Size?"
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
mathjax: true
author: Federico D'Atri
---

The **$I^2$** statistic is often misunderstood as an absolute measure of heterogeneity; for example, values over 75% are usually interpreted as evidence of high heterogeneity in the set of studies being analyzed. However, this index just reflects the ratio between the estimated variance of the true effects **$\tilde{\tau}^2$** and the total observed variance, which includes the estimated sampling variance **$\tilde{v}$** plus **$\tilde{\tau}^2$**, thus making it a relative measure of heterogeneity (Viechtbauer, 2022). As explained by Borenstein (2020), this definition implies that effects with different amounts of heterogeneity can yield the same **$I^2$** index. Moreover, if an effect has a certain amount of true heterogeneity, increasing the sample size of individual studies indefinitely, while keeping the total number of subjects constant across all studies, reduces the proportion of total variance accounted for by sampling error, making **$I^2$** tend towards 100%. 
  
In this post, we will have a look, both using the **$I^2$** formula and simulation, at how this index increases as the sample size of individual studies in a meta-analysis increases.



## Our example
The setting we'll be using is the following: a treatment with true effects, measured in terms of the standardized difference between *treatment* and *control* (*Cohen's d*), following a distribution with a mean of $1$ and a standard deviation of $0.1$. The total number of subjects, **$N$** will be kept almost constant, in particular in our setup, we will use 200,000 subjects:  
```r
# Setting parameters
N <- 200000
mu_d <- 1
tau <- 0.1
tau2 <- tau^2
```
We consider the *treatment vs. control* variable as manipulated within subject, with the measurements in these two conditions having a correlation of 0.5. According to Borenstein et al. (2009), the formula used in this instance to calculate the sampling variance for each study is:

$$
\textit{Var}(d) = \left(\frac{1}{n} + \frac{d^2}{2n}\right) \cdot 2 \cdot (1 - r(\textit{treatment}, \textit{control}))
$$

With a correlation between *treatment* and *control* of $0.5$, this becomes:

$$
\textit{Var}(d) = \left(\frac{1}{n} + \frac{d^2}{2n}\right)
$$

We make the sample size of each study, **$n$**, vary from 5 to 10,000, while calculating the number of individual studies, **$m$**, as **$N / n$**, rounded down when not an integer:

```r
# Function to calculate I² 
I2 <- function(v, tau2) {
  wi <- 1 / v
  m <- length(v)  
  v_tilde <- (m * sum(wi)) / (sum(wi)^2 - sum(wi^2))  # Calculate estimated sampling variance
  return(100 * (tau2 / (tau2 + v_tilde)))  
}

# Initialize data frame for storing I² values
I2_data <- data.frame(n = integer(), m = integer(), I2 = numeric())

# Calculate I² values for varying sample sizes
for (n in seq(5, 10000, by = 5)) {
  m <- floor(N / n)  
  d_values <- rnorm(m, mean = mu_d, sd = tau)  
  var_d_values <- 1 / n + d_values^2 / (2 * n)  # Calculate variance for each study
  I2_value <- I2(var_d_values, tau_squared)  
  I2_data <- rbind(I2_data, data.frame(n = n, m = m, I2 = I2_value)) 
}
```
If we plot our data we obtain:
![I² Formula Plot](https://github.com/fdatri/I2-and-sample-size/blob/main/I2_formula_plot.png?raw=true)


## References
Borenstein, M. (2020). Research Note: In a meta-analysis, the I² index does not tell us how much the effect size varies across studies. Journal of Physiotherapy, 66(2), 135-139.


