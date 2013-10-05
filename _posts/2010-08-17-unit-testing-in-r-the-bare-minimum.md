---
title: "Unit Testing in R: The Bare Minimum"
author: John Myles White
layout: post
permalink: /notebook/2010/08/17/unit-testing-in-r-the-bare-minimum/
categories:
  - Statistics
---

### Introduction

This week I decided to start unit testing my R code, so I taught myself the bare minimum about the RUnit and testthat packages to be able to use them. Here's what I found necessary to get started writing tests with both packages.

### RUnit Basic Example

I'm going to assume that you've got a bunch of functions in `sample.R` that you want to test. For example, `sample.R` might contain a definition of the naïve factorial function:

{% highlight r %}
factorial <- function(n)
{
  if (n == 0)
  {
    return(1)
  }
  else
  {
    return(n * factorial(n - 1))
  }
}
{% endhighlight %}

To test your functions, create a directory called `tests` that will store all of your test cases. In `tests`, create a file called `1.R` that will contain your first set of tests. Each set of tests will go in a function inside of `1.R`, named according to the convention `test.*`. For example, you might have this:

{% highlight r %}
test.examples <- function()
{
  checkEquals(6, factorial(3))
  checkEqualsNumeric(6, factorial(3))
  checkIdentical(6, factorial(3))
  checkTrue(2 + 2 == 4, 'Arithmetic works')
  checkException(log('a'), 'Unable to take the log() of a string')
}

test.deactivation <- function()
{
  DEACTIVATED('Deactivating this test function')
}
{% endhighlight %}

To run this set of tests, we need to create a file called `run_tests.R` that will act as a test suite and invoke all of the tests in your `tests` directory:

{% highlight r %}
library('RUnit')

source('sample.R')

test.suite <- defineTestSuite("example",
                              dirs = file.path("tests"),
                              testFileRegexp = '^\\d+\\.R')

test.result <- runTestSuite(test.suite)

printTextProtocol(test.result)
{% endhighlight %}

Here you inform the `defineTestSuite()` function that you're creating a test suite called `example` and that the test files are located in a directory called `tests` where all of the files match the regular expression `^\\d+\\.R`. Then you run the suite explicitly and print out the results in a text format.

That's it. With those ideas, you can write your own test suite for your R code.

### Using the `check*()` Functions 

In general, with RUnit, you use a function named something like `check*` to test the following conditions:

* `checkEquals`: Are two objects equal, including named attributes?
* `checkEqualsNumeric`: Are two numeric values equal?
* `checkIdentical`: Are two objects exactly the same?
* `checkTrue`: Does an expression evaluate to `TRUE`?
* `checkException`: Does an expression raise an error?

In addition to these functions, there's also a `DEACTIVATED()` function that lets you turn off a test function during its execution if you need to do that.

### testthat Basic Example

As above, I'm going to assume that you've got a bunch of functions in `sample.R` that you want to test. And, as before, to test your functions, you should create a directory called `tests` that will store all of your test cases. In `tests`, create a file called `1.R` that will contain your first set of tests. We'll use `expect_that()` for all of our tests, as in the example below:

{% highlight r %}
expect_that(1 ^ 1, equals(1))
expect_that(2 ^ 2, equals(4))

expect_that(2 + 2 == 4, is_true())
expect_that(2 == 1, is_false())

expect_that(1, is_a('numeric'))

expect_that(print('Hello World!'), prints_text('Hello World!'))

expect_that(log('a'), throws_error())

expect_that(factorial(16), takes_less_than(1))
{% endhighlight %}

To run this set of tests, we need to create a file called `run_tests.R` that will act as a test suite and invoke all of the tests in your `tests` directory:

{% highlight r %}
library('testthat')

source('sample.R')

test_dir('tests', reporter = 'Summary')
{% endhighlight %}

To get output, you have to inform `test_dir()` to use the SummaryReporter, which provides more than enough information for my purposes. See the testthat docs for other reporters you could use.

### Using `expect_that()`

In general, you can ask `expect_that()` to test the following conditions:

* `is_true`: Does the expression evaluate to `TRUE`?
* `is_false`: Does the expression evaluate to `FALSE`?
* `is_a`: Did the object inherit from a specified class?
* `equals`: Is the expression equal within numerical tolerance to your expected value?
* `is_equivalent_to`: Is the object equal up to attributes to your expected value?
* `is_identical_to`: Is the object exactly equal to your expected value?
* `matches`: Does a string match the specified regular expression?
* `prints_text`: Does the text that's printed match the specified regular expression?
* `throws_error`: Does the expression raise an error?
* `takes_less_than`: Does the expression take less than a specified number of seconds to run?

### More testthat Tricks

There are some other tricks that testthat can do as well. It can automatically rerun tests on a directory of code whenever the code is edited using a function called `auto_test()` and it can set up contexts to separate tests using a function `context()`. I haven't really explored either, so I can't comment more on them.

### Which to Use?

While I don't think the arguments on behalf of either RUnit or testthat are unquestionable, I'm inclined to think that testthat has a brighter future, especially since it's written by ggplot2's author, [Hadley Wickham][(http://had.co.nz/).
