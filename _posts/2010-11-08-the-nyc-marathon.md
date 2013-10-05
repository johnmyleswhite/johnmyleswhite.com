---
title: The NYC Marathon
author: John Myles White
layout: post
permalink: /notebook/2010/11/08/the-nyc-marathon/
categories:
  - Programming
  - Statistics
---

New York's annual marathon took place yesterday. Watching a bit of it on television with my friends, I was struck by the much earlier starting time for women than men. Specifically, professional women started running yesterday at 9:10 AM, while professional men start running at 9:40 AM. (This information comes from the [runner's handbook](http://www.ingnycmarathon.org/entrantinfo/handbook.htm).) I wanted to get a sense of how much this head start depended on real differences in their performance, because I found it very hard to imagine why professional women would run significantly slower than professional men.

Of course, I have seen discussions of the [speed difference between men and women](http://www.nytimes.com/2008/08/22/sports/olympics/22women.html) before, but I was still very surprised by it yesterday. To get a sense of the scope of the differences, I found some data this morning from the [ING Marathon website](http://www.nycmarathon.org/Results.htm) and made a quick density estimate plot, which you can see below:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/11/hours_gender.png" alt="hours_gender.png" />
</center>

It's clear that men and women had quite difference average speeds yesterday, and that their times had very different distributions. Of course, these plots are each based on 100 observations, so I'm hesitant to make any strong conclusions. Having confirmed for myself that there are real differences in the performance of men and women, I have to confess that I still find it surprising.

For those interested in following up on this, the code I used to produce this plot and the data set I used are both available on [GitHub](http://bit.ly/cAY98C). I'm sure there are other interesting questions one can ask of this data beyond simple comparisons across genders.
