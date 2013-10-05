---
title: ProjectTemplate
author: John Myles White
layout: post
permalink: /notebook/2010/08/26/projecttemplate/
categories:
  - Statistics
---

### Introduction

As many people already know, I've recently uploaded a new R package called ProjectTemplate to [GitHub](http://github.com/johnmyleswhite/ProjectTemplate
) and [CRAN](http://cran.r-project.org/web/packages/ProjectTemplate/index.html). The ProjectTemplate package provides a function, `create.project()`, that automatically builds a directory for a new R project with a clean sub-directory structure and automatic data and library loading tools. My hope is that standardized data loading, automatic importing of best practice packages, integrated unit testing and useful nudges towards keeping a cleanly organized codebase will improve the quality of R coding.

My inspiration for this approach comes from the `rails` command from Ruby on Rails, which initializes a new Rails project with the proper skeletal structure automatically. Also taken from Rails is ProjectTemplate's approach of preferring convention over configuration: the automatic data and library loading as well as the automatic testing work out of the box because assumptions are made about the directory structure and naming conventions that will be used in your code. You can customize your codebase however you'd like, but you will have to edit the automation scripts to use your conventions instead of the defaults before you'll get their benefits again.

In what follows, I try to highlight the state of the package as of today.

### Installing

ProjectTemplate is available on CRAN and can be installed using a simple call to `install.packages()`:

{% highlight r %}
install.packages('ProjectTemplate')
{% endhighlight %}

If you would like access to changes that are not available in the [current version on CRAN][2], please download the contents of the [GitHub repository][1] and then run,

{% highlight bash %}
R CMD BUILD .
R CMD INSTALL ProjectTemplate_*.tar.gz
{% endhighlight %}

### Example Code

To create a new project called `my-project`, open R and type:

{% highlight r %}
library('ProjectTemplate')
create.project('my-project')
{% endhighlight %}

To enter that project's home directory and start working, type:

{% highlight r %}
setwd('my-project')
load.project()
{% endhighlight %}

Once you have code worth testing, you can also type,

{% highlight r %}
run.tests()
{% endhighlight %}

to automatically run all of the unit tests in your `tests` directory.

If you're interested in these last two functions, you should know that `load.project()` is essentially a mnemonic for calling `source('lib/boot.R')`, which automatically loads all of your libraries and data sets. Similarly, `run.tests()` is essentially a mnemonic for calling `source('lib/run_test.R')`, which automatically runs all of the 'testthat' style unit tests contained in your `tests` directory.

### Overview

As far as ProjectTemplate is concerned, a good project should look like the following:

* project/
* data/
* diagnostics/
* doc/
* graphs/
* lib/
* boot.R
* load_data.R
* load_libraries.R
* preprocess_data.R
* run_tests.R
* utilities.R
* profiling/
* reports/
* tests/
* README
* TODO

To do work on such a project, enter the main directory, open R and type `source('lib/boot.R')`. This will then automatically perform the following actions:

* `source('lib/load_libraries.R')`, which automatically loads the CRAN packages currently deemed best practices. At present, this list includes: 
    * reshape
    * plyr
    * stringr
    * ggplot2
    * testthat
* `source('lib/load_data.R')`, which automatically imports any CSV or TSV data files inside of the `data/` directory.
* `source('lib/preprocess_data.R')`, which allows you to make any run-time modifications to your data sets automatically. This is blank by default.

### Default Project Layout
    
Within your project directory, ProjectTemplate creates the following directories and files whose purpose is explained below:

* `data`: Store your raw data files here. If they are CSV or TSV files, they will automatically be loaded when you call `load.project()` or `source('lib/boot.R')`, for which `load.project()` is essentially a mnemonic.
* `diagnostics/`: Store any scripts you use to diagnose your data sets for corruption or problematic data points. You should also put code that globally censors any data points here.
* `doc/`: Store documentation for your analysis here.
* `graphs/`: Store any graphs that you produce here.
* `lib/`: Store any files that provide useful functionality for your work, but do not constitute a statistical analysis per se here.
* `lib/boot.R`: This script handles automatically loading the other files in `lib/` automatically. Calling `load.project()` automatically loads this file.
* `lib/load_data.R`: This script handles the automatic loading of any CSV and TSV files contained in `data/`.
* `lib/load_libraries.R`: This script handles the automatic loading of the best practice packages, which are reshape, plyr, stringr, ggplot2 and testthat.
* `lib/preprocess_data.R`: This script handles the preprocessing of your data, if you need to add columns at run-time or merge normalized data sets.
* `lib/run_tests.R`: This script automatically runs any test files contained in the `tests/` directory using the 'testthat' package. Calling `run.tests()` automatically runs this script.
* `lib/utilities.R`: This script should contain quick general purpose code that belongs in a package, but hasn't been packaged up yet.
* `profiling/`: Store any scripts you use to benchmark and time your code here.
* `reports/`: Store any output reports, such as HTML or LaTeX versions of tables here. Sweave documents should also go here.
* `tests/`: Store any test cases in this directory. Your test files should use 'testthat' style tests.
* `README`: Write notes to help orient newcomers to your project.
* `TODO`: Write a list of future improvements and bug fixes you have planned.

### Request for Comments
    
I would love to hear feedback about things that ProjectTemplate is missing or should do differently. Please leave any and all comments you have.
