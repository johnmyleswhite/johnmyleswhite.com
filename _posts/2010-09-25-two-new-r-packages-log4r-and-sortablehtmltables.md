---
title: "Two New R Packages: log4r and SortableHTMLTables"
author: John Myles White
layout: post
permalink: /notebook/2010/09/25/two-new-r-packages-log4r-and-sortablehtmltables/
categories:
  - Programming
  - Statistics
---

I've just released two new packages for R: [log4r](http://github.com/johnmyleswhite/log4r) and [SortableHTMLTables](http://github.com/johnmyleswhite/SortableHTMLTables). log4r is a minimal logging utility for R that's inspired by the [log4j](http://logging.apache.org/log4j/1.2/) family of logging tools. It has substantially fewer features than other logging tools for R, but it's hopefully easier to use. SortableHTMLTables uses [brew](http://cran.r-project.org/web/packages/brew/index.html) and the [jQuery](http://jquery.com/) [Tablesorter](http://tablesorter.com/docs/) plugin to provide a function that dumps a data frame out to an HTML document containing a sortable table. You can see an example of the output [here](http://www.johnmyleswhite.com/SortableHTMLTables/sample.html). Both packages were submitted to CRAN today and should reach the mirrors over the next few days.
