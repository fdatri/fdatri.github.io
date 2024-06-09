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

The **$I^2$** statistic is often misunderstood as an absolute measure of heterogeneity; for example, values over 75% are usually interpreted as evidence of high heterogeneity in the set of studies being analyzed. However, this index merely reflects the ratio between the estimated variance of the true effects **$\tilde{\tau}^2$** and the total observed variance, which includes the estimated sampling variance **$\tilde{v}$** plus **$\tilde{\tau}^2$**, thus making it a relative measure of heterogeneity (Viechtbauer, 2022). As explained by Borenstein (2020), this definition implies that effects with different amounts of heterogeneity can yield the same **$I^2$** index. Moreover, if an effect has a certain amount of true heterogeneity, increasing the sample size of individual studies indefinitely, while keeping the total number of subjects constant across all studies, reduces the proportion of total variance accounted for by sampling error, making **$I^2$** tend towards 100%. In this post, we will have a look, both using the **$I^2$** formula and simulation, at how this index increases as the sample size of individual studies in a meta-analysis increases.



## Our example
The setting we'll be using is the following: a treatment with true effects, measured in terms of the standardized difference between *treatment* and *control* (Cohen's *d*), following a distribution with a mean of 1 and a standard deviation of 0.1. The total number of subjects, **$N$** will be kept almost constant, in our setup, we will use 200,000 subjects. The sample size of each study, **$n$**, will vary from 5 to 20,000, while The number of studies, **$m$**, will be calculated as **$N / n$** , rounded down when not an integer .



## References
Borenstein, M. (2020). Research Note: In a meta-analysis, the I² index does not tell us how much the effect size varies across studies. Journal of Physiotherapy, 66(2), 135-139.


