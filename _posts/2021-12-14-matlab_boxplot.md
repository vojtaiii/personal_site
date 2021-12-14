---
title: "Automatically add significant difference lines to a MATLAB boxplot"
date: 2021-12-14T21:35:00-04:00
categories:
  - tools
tags:
  - matlab
  - plot
---

![alt text][boxplotpic]

There is currently no tool which has the capability to add the lines in a user friendly way. I created a simple function, acting in a very similar way to classic `boxplot()`, implementing the automatic line creation.

Instead of calling

```Matlab
boxplot(data, groups, labels)
```

you simply call

```Matlab
boxplotdifferences(data, groups, diffs, labels)
```

where `diffs` is a double matrix containing info about the difference lines. Each row represents a single difference. First column is the position of the group A, second column the position of group B and third column is the degree of significance (1 - \*', 2 - '\*\*', 3 - '\*\*\*'). For example, the image on this page is described as

```Matlab
diffs = [1 2 2; 1 3 1; 1 4 3];
```

Check the git repository below:

<div class="github-card" data-github="vojtaiii/Matlab_boxplot_groupdiff" data-width="400" data-height="" data-theme="default"></div>
<script src="//cdn.jsdelivr.net/github-cards/latest/widget.js"></script>

[boxplotpic]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/,atlab_boxplots/boxplot.png?raw=true
