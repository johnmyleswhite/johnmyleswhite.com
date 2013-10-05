---
title: Review of R Graphs Cookbook
author: John Myles White
layout: post
permalink: /notebook/2011/03/01/review-of-r-graphs-cookbook/
categories:
  - Programming
  - Statistics
---

The kind people at [Packt Publishing](http://www.packtpub.com) recently asked me to review one of their newest R books: the [R Graphs Cookbook][2]. In general, I think pretty highly of the book: it provides a nice overview of the basic tools for visualizing data in R. If you're just getting started with creating graphs in R, this book could be a very valuable resource. It's clearly targeted at beginners, though it does seem to assume at least a little familiarity with R's basic data structures and control flow. Perhaps more importantly for potential readers, the book actually works through some comprehensible, extended examples, which makes it substantially more readable than the default R documentation. I'd probably have benefited from having this book when I was first starting to learn to program in R.

Another plus for the book is that it covers some of the graphing functionality that's provided by the base, lattice and ggplot2 packages. That last bit is particularly important to me: I hope this book represents one of the first members of a new generation of books on R that will treat ggplot2 as a default tool for building graphs in R -- if not the default tool. That said, I'd actually have been happy if the book had proselytized more strongly for ggplot2, even though I can imagine that it's probably a good thing to expose beginner users to the traditional tools in R for visualizing data in addition to the newest favorite contender.

For intermediate users, I suspect that the most useful part of the book will be the discussion in the second chapter of some of the lower level graphic parameters that `par()` gives you access to. I'd never learned to use those features very effectively, so the chapter covering those details was particularly helpful for me. Also, there's a later chapter on creating maps that's quite nice, as well as a final chapter that's focused on producing publication-ready graphics. Both of those chapters could be very useful for more advanced readers: for example, I knew absolutely nothing about working with fonts in R before reading those sections.

In short, the [R Graphs Cookbook](http://link.packtpub.com/FQqhZX) seems like a useful book to have around for most R hackers and a potentially very valuable resource for new programmers. If you're interested in finding a starting point for learning to build graphs in R, I'd suggest considering this book.
