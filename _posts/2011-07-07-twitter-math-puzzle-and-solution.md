---
title: Twitter Math Puzzle and Solution
author: John Myles White
layout: post
permalink: /notebook/2011/07/07/twitter-math-puzzle-and-solution/
categories:
  - Mathematics
  - Psychology
  - Statistics
---

Yesterday I posted a very simple math puzzle to Twitter that I found in Jonathan Baron's book, [Thinking and Deciding](http://amzn.to/npM5Uk). The puzzle is the following:

<blockquote>
<p>Show that every number of the form ABC,ABC is divisible by 13.</p>
</blockquote>

The puzzle comes up in Baron's book as an example of an "insight problem" in which one goes from not knowing the answer at all to knowing the complete answering in a sudden moment of insight.

Several people replied to my tweet with solutions: I especially like [Will Townes's](https://twitter.com/#!/willtownes/status/88735472028876800) solution. In particular, if you're familiar with [modular arithmetic](http://en.wikipedia.org/wiki/Modular_arithmetic), I like the logic of Will's answer because it gives a simple generalization. First, represent $ABC,ABC$ as $ABC * 1000 + ABC * 1$ rather than as $ABC * 1001$. Then notice that

* $1 = 1 \mod 13$
* $1000 = -1 \mod 13$

Thus $ABC,ABC$ = $ABC * -1 + ABC * 1 = 0 \mod 13$. This logic can be easily extended to show that $(ABC,ABC,)*ABC,ABC = 0 \mod 13$ no matter how many times you repeat the $ABC,ABC$ pattern.
