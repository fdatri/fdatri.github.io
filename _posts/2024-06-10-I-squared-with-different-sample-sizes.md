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

The **$I^2$** index is often misunderstood as an absolute measure of heterogeneity; for example, values over 75% are interpreted as indicative of high heterogeneity in the set of studies being analyzed. However, it just reflects the ratio between the estimated true variance of the effect  **$\tau^2$**, and the total observed variance, which includes the estimated sampling variance **$\tilde{v}$**  plus **$\tau^2$**. As clearly explained by Borenstein, this definition implies that effects with different amounts of true heterogeneity can yield the same I² index. Moreover, if an effect has a certain amount of true heterogeneity, increasing the sample size of individual studies indefinitely—while maintaining the total number of subjects constant across all studies—reduces the proportion of total variance accounted for by sampling error, causing I² to tend towards 100%. In this post, we will have a look, both through the I² formula and simulation, at how this index increases as the sample size of individual studies in a meta-analysis increases.

## References
Borenstein, M. (2020). Research Note: In a meta-analysis, the I² index does not tell us how much the effect size varies across studies. Journal of Physiotherapy, 66(2), 135-139.


