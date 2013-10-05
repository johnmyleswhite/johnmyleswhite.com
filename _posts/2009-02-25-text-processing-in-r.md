---
title: Text Processing in R
author: John Myles White
layout: post
permalink: /notebook/2009/02/25/text-processing-in-r/
categories:
  - Programming
---

On a regular basis, I have to process text in R. I invariably find that I need a function whose name or usage I can't bring to mind. To help my future self, I'm writing this review of R's built-in text processing functions. Hopefully, this review will also be of use to others.

**Character Vectors == Arrays of Strings**  
The first source of confusion for me is the R type system. In R, a string is considered to be a character vector, but *an R character vector would be an array of strings in any other programming language*. Consider the following example:

{% highlight r %}
str = 'string'
str[1] # This evaluates to 'string'.
{% endhighlight %}

To get access to the individual characters in an R string, you need to use the `substr` function:

{% highlight r %}
str = 'string'
substr(str, 1, 1) # This evaluates to 's'.
{% endhighlight %}

For the same reason, you can't use `length` to find the number of characters in a string. You have to use `nchar` instead.

But let's go back to `substr`. The first argument to `substr` is a character vector, the second is the index of the first character you want, and the third is the index of the last character you want. So you can also use `substr` as follows:

{% highlight r %}
str = 'string'
substr(str, 1, 2) == 'st'
substr(str, 5, 6) == 'ng'
{% endhighlight %}

As you can see, `substr` lets you access the individual characters of a string using an indexing/slicing strategy. 

To break strings apart into vectors of characters, you can use the `strsplit` function, which works a lot like the `split` function in Perl. Here's an example:

{% highlight r %}
strsplit('0-0-1', '-') # Evaluates to list('0', '0', '1')
{% endhighlight %}

**Putting Things Back Together Again**  
Now that you can pull strings apart, you need to be able to put the characters back together again into strings. You can do this using `paste`. `paste` is an idiosyncratic function: it is the only function for concatenation of strings in R, but it also handles the work of more sophisticated functions like Perl's `join`. Try the following:

{% highlight r %}
str1 = 'first'
str2 = 'second'
print(paste(str1, str))
{% endhighlight %}

As you'll see, there's an odd space added to the output. That's because `paste` has an optional argument that provides a separator used when combining strings that defaults to a single space. So,

{% highlight r %}
paste('first', 'second') == paste('first', 'second', sep = ' ')
{% endhighlight %}

You can get rid of the space by specifying a null separator instead.

{% highlight r %}
print(paste('first', 'second', sep = ''))
{% endhighlight %}

**Changing Case**  
To change the case of strings or individual characters, you need to use the `tolower` and `toupper` functions. You can use these with `substr` to make a function that turns most common words into their title case form:

{% highlight r %}
pseudo.titlecase = function(str)
{
	substr(str, 1, 1) = toupper(substr(str, 1, 1))
	return(str)
}
{% endhighlight %}

With a little more sophistication, you can make a full title case function à la [John Gruber](http://daringfireball.net/2008/05/title_case). The result of my attempt to do this is fairly long, so I've posted it to my GitHub account. I'll probably see about adding it to the R repository at some point if I can incorporate enough features to make it worth using. If you're interested in helping or using what I've written, you can check out the code [here](http://github.com/johnmyleswhite/stringops/tree/master).

Finally, there is a `chartr` function that translates characters in the input into the corresponding characters you select. For instance, you might try this:

{% highlight r %}
chartr('abc', 'XYZ', 'abcabc') # Evaluates to "XYZXYZ".
{% endhighlight %}

This will remind Perl users of `tr`, which I personally never use. Nevertheless, it's nice knowing that it's there in R, albeit with a slightly different name.

**Substring Containment**  
Finally, you might want to know if a string is contained in another string or set of strings. You can do this using the `charmatch` function:

{% highlight r %}
charmatch("m",   c("mean", "mode")) # returns 0
charmatch("med", c("mean", "median")) # returns 2
{% endhighlight %}

I tend to use regular expressions by the time I would need substring matching, so I'm not sure if I would ever use `charmatch` in practice.

**Future Ideas**  
For more sophisticated text processing, you would want to use regular expressions and the `grep` family of functions. I'll have to read about them and write up something about their use in the future. R also implements an approximate regular expression matching system using Levenshtein edit distances, but I haven't tried using that yet.
