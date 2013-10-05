---
title: Higher Order Programming
author: John Myles White
layout: post
permalink: /notebook/2009/02/15/higher-order-programming/
categories:
  - Programming
---

**Updated: 2.17.09**

Advocates of higher order programming languages, ranging from Lisp to Ruby, usually claim that programming in a higher order language is more efficient than programming in a language that is designed for easy translation into machine code, such as C. The case for higher order programming languages can be made on many points: (1) mental power, rather than computing power, is usually the limiting factor in modern program design; (2) fully generic operations are nearly or entirely impossible to express in languages that are too strongly typed; (3) etc., etc.

I think that we should emphasize cognitive factors instead: higher order programmers are experts in the application of a set of primitive operations that are more universally applicable to the problems we are asked to solve as programmers than the operations provided by lower level languages. 

There is a considerable literature in cognitive psychology on problem solving and expertise. Experts solve problems more efficiently than novices in part because they immediately recognize the relevant set of abstractions to apply to a problem. All of the modern higher order programming languages share a set of abstractions that I would claim are better suited for problem solving than the abstractions offered by C or Java.

First and foremost, to describe algorithms cleanly, you want to be able to write functions that use other functions as input. If you cannot do this, you end up with design patterns -- the accumulated knowledge of how to write scaffold code that is not unique to your problem at hand. This wastes your time and it also makes the resulting code ugly. In code, beauty comes very near to being truth, because bugs tend to hide in the bland sections of your programs that you find tedious to read.

To convince yourself that treating functions as a primitive data type is invaluable, ask yourself how you would write a function that returns the derivative of a given function *as a new function*. This is a trivial task in higher order programming languages, but it is much subtler in lower level languages. Sometimes it's simply impossible.

Once you have the ability to recognize that there are general patterns in how you apply functions to problems, you can encode these patterns as new functions in higher order programming languages. These functions of functions are the primitive operations that functional programmers use constantly, but they're entirely lacking in lower level languages.

To illustrate how these patterns can be encoded as generic functions to produce clearer code, I'll discuss the use of the `map` and `reduce` functions provided by four languages: Perl, Python, Ruby and Clojure. I'll give a brief explanation of these functions and then show how they can be used to make code shorter. In particular, I'll solve the following easy problems: (1) finding the sum of the first five squares and (2) creating a string of upper case characters separated by dashes from list of lower case characters.

**Map**  
`map` is function that applies functions to the entries of an array in order and returns the sequence that results. It therefore substitutes for explicit looping. You can use `map` to easily find the factorial of each of the first five numbers:

{% highlight ruby %}
[1, 2, 3, 4, 5].map { |n| factorial(n) }
{% endhighlight %}

**Reduce**  
`reduce` is a function that transforms an array into a single value. You iterate over the pairs of items in the array and combine them a function you specify. This lets you express ideas like summing a sequence of numbers or joining a set of strings together without any explicit loops. Here's how you can use reduce to find the sum of the first five numbers in Ruby:

{% highlight ruby %}
[1, 2, 3, 4, 5].reduce { |a,b| a + b }
{% endhighlight %}

When you can combine these operations, you can write more complex operations quickly that would require multiple nested loops in lower level languages. To see how this helps as the functions get larger, let's go over the use of compositions of `map` and `reduce` in four languages.

**Perl**  
We're going to start with Perl, but there's a problem at the start: Perl 5 does not implement `reduce`. `reduce` does exist in Perl 6, but, as always, no one knows when Perl 6 will be available. In the absence of `reduce`, you have to write an explicit `for` loop to do the work `reduce` would do in the background. This adds dummy variables equivalent to useless pronouns in written English, which makes the code less clean:

{% highlight perl %}
my @squares = map {$_**2} (1, 2, 3, 4, 5);

$sum = 0;

for my $square (@squares)
{
  $sum = $sum + $square;
}
{% endhighlight %}

That solves the numeric calculation. For strings, it turns out that Perl implements a function that is really a special case of using `reduce`: `join`. `join` takes a separator and an array of strings and returns the string you get by concatenating all of the strings while placing the separator in between. This is particularly helpful, because an explicit `for` loop would be complicated by a test for your position in the array to insure that you didn't add the separator at the start or the end of the output.

{% highlight perl %}
join '-', map {uc $_} ('a', 'b', 'c');
{% endhighlight %}

**Python**  
Python does implement both `map` and `reduce`, so there's no need for much fuss.

{% highlight python %}
reduce(lambda a, b: a + b,
       map(lambda a: a**2,
           [1, 2, 3, 4, 5]))
{% endhighlight %}

{% highlight python %}
reduce(lambda a, b: a + '-' + b,
       map(lambda a: a.upper(),
          ['a', 'b', 'c']))
{% endhighlight %}

**Ruby**  
Ruby also implements both `map` and `reduce`:

{% highlight ruby %}
[1, 2, 3, 4, 5].map { |a| a**2 }.reduce { |a,b| a + b }
{% endhighlight %}

{% highlight ruby %}
['a', 'b', 'c'].map { |a| a.upcase }.reduce { |a,b| a + '-' + b }
{% endhighlight %}

**Clojure**  
And Clojure, being a Lisp, of course implements both `map` and `reduce`:

{% highlight clojure %}
(reduce + 
        (map (fn [n] (* n n))
             '(1 2 3 4 5)))
{% endhighlight %}

{% highlight clojure %}
(reduce (fn [a, b] (str a "-" b))
        (map (fn [a] (. a toUpperCase)) 
             '("a", "b", "c")))
{% endhighlight %}
