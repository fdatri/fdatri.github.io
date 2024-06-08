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

The I² index is often misunderstood as an absolute measure of heterogeneity; for example, values over 75% are interpreted as indicative of high heterogeneity in the set of studies being analyzed. However, it merely reflects the ratio between the estimated true variance of the effect \( \tau^2 \) (tau-squared), and the total observed variance, which includes the estimated sampling variance \( \tilde{v} \) (v-tilde) plus \( \tau^2 \). As Borenstein (2020) clearly explains, this definition reveals that effects with vastly different amounts of true heterogeneity can yield the same I² index. Moreover, if an effect has a certain amount of true heterogeneity, increasing the sample size of individual studies indefinitely—while maintaining the total number of subjects constant across all studies—reduces the proportion of total variance accounted for by sampling error, causing I² to tend towards 100%. In this post, we will explore, both through the I² formula and simulation, how this index increases as the sample size of individual studies in a meta-analysis increases.

## References
Borenstein, M. (2020). Research Note: In a meta-analysis, the I² index does not tell us how much the effect size varies across studies. Journal of Physiotherapy, 66(2), 135-139.
