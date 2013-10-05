---
title: Why Use ProjectTemplate or Any Other Framework?
author: John Myles White
layout: post
permalink: /notebook/2010/09/19/why-use-projecttemplate-or-any-other-framework/
categories:
  - Statistics
---

We use frameworks like [Ruby on Rails](http://rubyonrails.org/) or [ProjectTemplate](http://github.com/johnmyleswhite/ProjectTemplate) to minimize the time we spend on irrelevant details. By definition, an irrelevant detail isn't of interest to us. But how can we tell which details are irrelevant? This isn't a trivial task and it seems to be, on the surface, a profoundly subjective matter.

Thankfully, it's much simpler to point to examples of irrelevant details than to provide a general theory, though I do have a general theory that I'll describe in a follow up post. For now, let's look at a source code file for a theoretical data analysis project written in R. The file is called `load_data.R`:

{% highlight r %}
choices <- read.csv('data/choices.csv', header = TRUE, sep = ',')
conditions <- read.csv('data/conditions.csv', header = TRUE, sep = ',')
metadata <- read.csv('data/metadata.csv', header = TRUE, sep = ',')
{% endhighlight %}

My claim is that most of the code in this file is concerned with irrelevant details. How can you tell that these details are irrelevant? In this example, there are two flaws that are obvious to me:

* I repeat the code that calls `read.csv()` for each of the data files I'm loading.
* I've hardcoded the data set's filenames, so I can't use this script in another project without editing it first.
* Because the filenames and variables are all hardcoded, I have to edit this file manually even when I add new data files to this project.

Solving these three problems was the first step that led me to design ProjectTemplate. Rather than rewrite `load_data.R` each time I start a new analysis project, it's much easier to create a single generic script that can be used in all of my projects, even if I will only use the generic script as a base for something customized to the project on hand. By using a script, rather than a function, I can make it easy to change anything that needs to be customized for each specific project, such as skipping one enormous file that I don't need to load each time or loading some data set from SQL databases. This default script approach is how I get around having to exploit R's inheritance system, which seems sufficiently complex that I'd rather not emulate Rails' approach directly.

So let's solve the data loading problem by making three assumptions:

* CSV files always have a header line and use comma separators.
* Every data file for a project is contained in the `data` directory.
* The file `X.csv` will always get loaded into a data frame called `X`.

With these assumptions, coding a generic script is simple enough, if we use some of the more abstract functions in R like `assign`:

{% highlight r %}
data.files <- dir('data')
for (data.file in data.files)
{
  filename <- file.path('data', data.file)
  variable.name <- sub('\\.csv$', '', data.file, ignore.case = TRUE, perl = TRUE)
  assign(variable.name, read.csv(filename, header = TRUE, sep = ','))
}
{% endhighlight %}

In practice, the new version of ProjectTemplate I'll release soon does a little more than the snippet of code above: it handles a small number of different file formats based on naming conventions and it will also decompress files on the fly. If you're feeling entrepid, I'd love to add additional code that can automatically access databases, but that's a good deal more work than I'm prepared to do myself.

This example leads to a more general lesson that helps to explain why you should use some sort of framework for many of your projects: if you don't absolutely have to, **don't repeat yourself**. This is so important that the acronym [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) has become popular in the programming community as a reminder that you should build abstractions that are flexible, reusable and appropriate to the task at hand. Frameworks, in my mind, are like [domain specific languages](http://en.wikipedia.org/wiki/Domain-specific_language), in that they make a bunch of assumptions about what you're trying to achieve and thereby allow you to articulate only what makes your project different from all other similar projects. If you don't do this, you end having to restate the details that all projects have in common. In general, keep in mind that code that avoids repetition is easier to understand, easier to change and and more likely to be valuable to the whole world if you distribute it.

There are a variety of ways to avoid repeating yourself. First off, you can avoid repeating yourself by moving inline code that varies only in the operands into functions and specialized scripts. This is arguably the crux of programming well: knowing which pieces of code can be rewritten as functions. The functions in Hadley's 'plyr' package are a wonderful example: the `ddply()` function performs a single operation that many of us previously rewrote ad hoc every time we analyzed a new data set because we hadn't realized that there was an appropriate abstraction that encompassed all of our requirements. Rewriting this sort of code that contains an unrecognized potential abstraction, often called a <a href="http://en.wikipedia.org/wiki/Design_pattern_(computer_science))">design pattern</a>, was a waste of our time, introduced new bugs and inefficiencies haphazardly into our work, and prevented the fruits of our efforts from being shared in a way that could benefit others. Hadley's generic function, in contrast, solves all of the problems in this class once and for all. It's a time saver for Hadley himself, but it's also a major time saver for the human race. If you save 10,000 people five minutes of their time, you've done a great deed. I think this is what makes programming so enjoyable for many people. It's certainly what I find so fulfilling about programming.

It's important to note, though, that we did more than write a very generic function in the code I listed above. We also made a decision that many programmers have been uncomfortable with historically: we allowed part of our process to become completely implicit for end users who won't actually read our new version of `load_data.R`, but will simply call it at the start of their own scripts. We made it possible for users to skip the step of specifying the location and type of their data files by making assumptions about how the user has set up their directory structure and how they've named their files. By exploiting a general strategy of preferring convention over configuration, we can often get a lot of results with very little work. And we can do it in a way that allows us total flexibility to override the default behavior when it's necessary.

This is a egoistic reason for standardizing on a set of conventions: it saves you time when you write code. But there are altruistic reasons as well, including how you relate to your future self, who [Derek Parfit](http://en.wikipedia.org/wiki/Reasons_and_Persons) will tell you is a separate person from your current self in some important senses. If you employ the same conventions in all of your work, it's easier for you to return to an old project or for outsiders to come into a new one. Standard conventions mean that everyone knows that the data files are in a specific location. Similarly, standardizing preprocessing makes it easy to figure out what happens between loading data and analyzing it. In general, following some set of conventions, even slightly inane ones, saves time by allowing you to focus only on the parts of a program that are different from all other programs.

To summarize what I've said so far,

* Don't repeat yourself. If something can be abstracted, it usually should be.
* Don't demand that everything be explicit. Using conventions instead of configuration can be hard on newcomers who don't know the assumptions you make, but it eventually pays off when they (and you) can skip over configuration steps and immediately get to work.
* Don't make conventions inescapable. Make it easy to edit the very abstract code that exploits your conventions.

Living up to these ideals is the inspiration for ProjectTemplate, but there a few other ways in which I hope ProjectTemplate helps the R programming community. Most of these revolve around good programming practices that are boring and tedious, but make your life much better in the long term:

* **Package Proslytizing**: You should learn to use some of the generic functions that Hadley and others have developed for R. ProjectTemplate automatically loads packages like plyr and ggplot2, which will hopefully encourage you to use them more frequently. Once you start using these packages, I am sure that you'll find that they save you time, even though you will need to expend some effort at the start to wrap your head around them.
* **Document Your Code**: Maintain a README that provides an overview, in high-level English, of what you're doing. Explain how you got to your current state, what you know about the data and what you're doing with it. And maintain a TODO file that lists what you're planning to do. Most people invent plans faster than they can implement them. Something as simple as a TODO helps you keep track of your goals and prioritize them. If you work on a project that only makes sense when you're really in the zone every few weeks, these two files can make your life enormously better.
* **Unit Testing**: People often write R code without being sure that it does what they intend it to do. Unit testing is the solution to this: you write a series of tests that confirm that your functions produce the expected outputs when you give them certain inputs. ProjectTemplate tries to make this easier by assuming that you will write tests using the testthat package and that the tests will go into the `tests` directory. As such, all you have to do is to write some simple tests inside that directory and then call `run.tests()`. In practice, test-driven development, in which you use tests to describe what a function is supposed to do, can make it much easier to produce working code quickly.
