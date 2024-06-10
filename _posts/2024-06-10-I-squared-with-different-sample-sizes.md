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

The **$I^2$** statistic is often misunderstood as an absolute measure of heterogeneity; for example, values over 75% are usually interpreted as evidence of high heterogeneity in the set of studies being analyzed. However, this index just reflects the ratio between the estimated variance of the true effects **$\tilde{\tau}^2$** and the total observed variance, which includes the estimated sampling variance **$\tilde{v}$** plus **$\tilde{\tau}^2$**, thus making it a relative measure of heterogeneity ([metafor-project](https://metafor-project.org/doku.php/tips:i2_multilevel_multivariate)). I suspect that this misunderstanding arises from the willingness to adopt an index with identifiable cutoffs. No matter the set of studies you have, you can measure **$I^2$**, and it will always be a value between 0 and 100%. Conversely, it is impossible to provide predefined cutoffs for $\tau$, since its absolute value depends on the measurement scale being used. For instance, a $\tau$ of $0.4$ could indicate a high amount of heterogeneity for a relative risk $(RR)$ of mortality with a mean effect of the treatment at $0.95$, or it could be considered an indicator of extremely low heterogeneity if we have a mean effect for an educational protocol on an increase in salary expressed as a percentage of 50%.

As explained by Borenstein (2020), the definition of **$I^2$** implies that effects with different amounts of heterogeneity can yield the same **$I^2$** index. Moreover, if an effect has a certain amount of true heterogeneity, increasing the sample size of individual studies indefinitely, while keeping the total number of subjects constant across all studies, reduces the proportion of total variance accounted for by sampling error, making **$I^2$** tend towards 100%. In this post, we will have a look, both using the **$I^2$** formula and simulation, at how this index increases as the sample size of individual studies in a meta-analysis increases.



## Sample Scenario
The setting we'll be using is the following: a treatment with true effects, measured in terms of the standardized difference between *treatment* and *control* (*Cohen's d*), following a distribution with mean  $1$ and  standard deviation $0.1$. The total number of subjects, **$N$** will be kept almost constant, in particular in our setup, we will use 200,000 subjects:  
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

![I² Formula Plot](https://github.com/fdatri/Blog-Material/blob/main/I2-and-sample-size/I2%20formula%20plot.png)


We can see how, even if our effect sizes have relatively low variability and our total subject count remains almost constant, with larger sample sizes, $I²$ approaches 100%. 

Following an approach where data are simulated for each subject, and for each sample size a random effects model is fitted using the *metafor* package, we arrive at the following plot (yellow dots represent $I²$ values obtained for different sample sizes ranging from 25 to 10,000):

![I² Formula vs. Simulation Plot](https://github.com/fdatri/Blog-Material/blob/main/I2-and-sample-size/I2%20formula%20vs.%20simulation%20plot.png)  

*Yellow dots represent I² values obtained for different sample sizes ranging from 25 to 10,000.*


Interestingly, the value obtained via simulation and model fitting seem to overestimate $I^2$ for $n = 25$ (first yellow dot on the left), but apart from this, the two values look very close. [Here](https://github.com/fdatri/Blog-Material/blob/main/I2-and-sample-size/simulation_I_squared.R) you can find the full code for this simulation.  
If, instead of estimating the correlation between *treatment* and *control* for each study to calculate the variance of each effect size, we set it to its true value of 0.5, this discrepancy seems to disappear:

![I² Formula true cor](https://github.com/fdatri/Blog-Material/blob/main/I2-and-sample-size/I2%20true%20cor.png)  

Since for $n = 25$ the estimate for $d$ is very close to $1$, but $\tau^2$ is being overestimated at $0.16$, I suspect that the $\textit{Var}(d)$ formula I used is biased for small samples and, in this instance, is underestimating the sampling variance. Perhaps someone can suggest why this happens in the comments.


## Conclusions: Interpreting Heterogeneity

In our example, we have a $\tau$ of 0.1 with a mean effect of 1; this can be interpreted as the effect of the treatment varying with a standard deviation of 10% relative to its absolute value. This also means that the true negative effects are almost 0%, and that 95% of the true effects are expected to lie within an interval of ±19.6% around the mean effect size. Without further context, I would say that this is an effect with low heterogeneity. Generally, $\tau$ should be interpreted contextually within the field of studies, relative to how similar treatments or effects within that field vary. However, the same principle should also be true for the interpretation of Cohen's *d*, where too often the usual cutoffs are mindlessly used to interpret it.




## References
Borenstein, M. (2020). Research Note: In a meta-analysis, the I² index does not tell us how much the effect size varies across studies. *Journal of Physiotherapy*, 66(2), 135-139.

Borenstein, M., Cooper, H., Hedges, L., & Valentine, J. (2009). Effect sizes for continuous data. *The handbook of research synthesis and meta-analysis*, 2, 221-235.

Viechtbauer, W. [metafor project](https://metafor-project.org/doku.php/tips:i2_multilevel_multivariate): Useful resource for computing $I^2$ in more complex models.



