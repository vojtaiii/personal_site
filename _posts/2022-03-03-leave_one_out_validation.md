---
title: "Find the combination of features for the best prediction accuracy"
date: 2022-03-03T12:00:00-04:00
categories:
  - tools
tags:
  - classification
  - matlab
  - statistical analysis
---

The following MATLAB script takes all features in columns of `data`.It then creates all possible combinations of them and performs classification and leave one out cross validation. You then need to set `y` to be a vector of zeroes and ones as labels to the two groups. Also set `v` to reflect the number of features you have. And that is all. You need also the `loo.m` function.

Author: Tomas Kouba

Check the git repository below:

<div class="github-card" data-github="t0mka14/classification_validation_script" data-width="400" data-height="" data-theme="default"></div>
<script src="//cdn.jsdelivr.net/github-cards/latest/widget.js"></script>
