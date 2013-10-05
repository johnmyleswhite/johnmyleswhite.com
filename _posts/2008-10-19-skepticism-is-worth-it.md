---
title: Skepticism is Worth It
author: John Myles White
layout: post
permalink: /notebook/2008/10/19/skepticism-is-worth-it/
categories:
  - Statistics
---

Being a [Gene Expression](http://www.gnxp.com/) reader, I've wanted to start playing with the [GSS](http://en.wikipedia.org/wiki/General_Social_Survey
) (General Social Survey) data for some time. This past week, I finally started to do just that. Taking advantage of the [freely available downloads of GSS data in SPSS format](http://www.norc.org/GSS+Website/Download/SPSS+Format/
) and [R's "foreign" library](http://cran.r-project.org/doc/manuals/R-data.html) for reading in SPSS data, I've found a result that will probably amuse most American liberals, despite their ostensibly unilateral opposition to wage inequality. The result, which is predictable but still worth having confirmed empirically, is that there is a fairly consistent positive correlation between skepticism regarding the veridical truth of the Bible and one's real income in the 2000-2006 data sets. The graph below illustrates this correlation, which hovers between 0.15 and 0.19 during this 6 year period.

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2008/10/religion-and-income.png" alt="religion_and_income.png" />
</center>

For those interested in verifying this result, the relevant variables are BIBLE and REALINC in the GSS data. I've converted the replies "WORD OF GOD", "INSPIRED WORD", and "BOOK FABLES" into the integer values 2, 3, 4 as numerical measures of skepticism. Real income is clearly already a numerical measure of income.
