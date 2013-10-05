---
title: R Function Usage Frequencies
author: John Myles White
layout: post
permalink: /notebook/2009/12/07/r-function-usage-frequencies/
categories:
  - Statistics
---

A few months ago I decided to apply word frequency analysis ideas to R code. My idea was simple: *the functions that one should invest the most effort into learning are precisely those functions that are used most frequently in real world code*. In fact, this simple idea can be applied to spoken languages as valuably as to programming languages: the words that will most improve your ability to understand day-to-day speech in a foreign language are precisely the words that occur most frequently in day-to-day speech. Our language classrooms tend to screw this up by emphasizing words like "spoon" over "dude", even though people my age will use the word "dude" many more times a day than they'll use "spoon". (See footnote) In large part, this is because the usefulness of a word tends to be confounded with its respectability when you learn a language in an academic setting. This over-emphasis on respectability is why you're never taught curse words, even though they're as practically useful as the majority of other words.

But I digress. Returning to R, [the attached CSV data set](http://www.johnmyleswhite.com/content/data_sets/r_function_frequencies.csv) contains the results of my word frequency analysis. To produce this data set, I spidered all of the source code for every R package on CRAN, split the resulting corpus text into lexical tokens, and counted the occurrences of each token. To make things simpler, I decided to treat every token followed by a parenthesized expression as a function, even though this gives me some syntactical units like `if` in my list of functions.

I think the resulting raw data set is probably as useful as any summaries I can offer of it, but I'll offer some obvious results for those interested.

Function call frequencies in R, unsurprisingly, seem to follow [Zipf's law](http://en.wikipedia.org/wiki/Zipf's_law) or some variety thereof. You can see this fairly clearly in the following logarithmic plot:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2009/12/r_log_frequencies1.png" alt="r_log_frequencies.png" />
</center>

And here are the top 25 most frequently used functions in R, including some syntactical units:  

<table class="table table-hover">
    <tr>
      <th>
        Function Name
      </th>
      <th>
        Occurrences in Corpus
      </th>
    </tr>
    <tr>
      <td>
        if
      </td>
      <td>
        333421
      </td>
    </tr>
    <tr>
      <td>
        c
      </td>
      <td>
        143123
      </td>
    </tr>
    <tr>
      <td>
        function
      </td>
      <td>
        137490
      </td>
    </tr>
    <tr>
      <td>
        length
      </td>
      <td>
        109847
      </td>
    </tr>
    <tr>
      <td>
        paste
      </td>
      <td>
        62906
      </td>
    </tr>
    <tr>
      <td>
        cat
      </td>
      <td>
        56199
      </td>
    </tr>
    <tr>
      <td>
        return
      </td>
      <td>
        56001
      </td>
    </tr>
    <tr>
      <td>
        stop
      </td>
      <td>
        54161
      </td>
    </tr>
    <tr>
      <td>
        is.null
      </td>
      <td>
        53575
      </td>
    </tr>
    <tr>
      <td>
        list
      </td>
      <td>
        51862
      </td>
    </tr>
    <tr>
      <td>
        log
      </td>
      <td>
        49479
      </td>
    </tr>
    <tr>
      <td>
        for
      </td>
      <td>
        48950
      </td>
    </tr>
    <tr>
      <td>
        rep
      </td>
      <td>
        39090
      </td>
    </tr>
    <tr>
      <td>
        names
      </td>
      <td>
        34439
      </td>
    </tr>
    <tr>
      <td>
        sum
      </td>
      <td>
        31475
      </td>
    </tr>
    <tr>
      <td>
        as.integer
      </td>
      <td>
        29417
      </td>
    </tr>
    <tr>
      <td>
        matrix
      </td>
      <td>
        29344
      </td>
    </tr>
    <tr>
      <td>
        is.na
      </td>
      <td>
        20230
      </td>
    </tr>
    <tr>
      <td>
        dim
      </td>
      <td>
        20202
      </td>
    </tr>
    <tr>
      <td>
        max
      </td>
      <td>
        19995
      </td>
    </tr>
    <tr>
      <td>
        nrow
      </td>
      <td>
        19789
      </td>
    </tr>
    <tr>
      <td>
        as.double
      </td>
      <td>
        19705
      </td>
    </tr>
    <tr>
      <td>
        attr
      </td>
      <td>
        18802
      </td>
    </tr>
    <tr>
      <td>
        t
      </td>
      <td>
        17889
      </td>
    </tr>
    <tr>
      <td>
        print
      </td>
      <td>
        17461
      </td>
    </tr>
  </table>
  
Some fun extensions to this work would be to use this frequency data set as a standard for the abnormality of code style: you could compare an individual programmer's code to the standard frequency data to see which functions such-and-such a programmer tends to overuse and underuse. I personally tend to underuse `stop` and I never use `attr` at all.
  
Another interesting project would be to use this data set as input to a text classifier that would attempt to predict the author of a piece of R code based on token frequency information, in line with [Mosteller and Wallace's famous analysis of the Federalist Papers](http://en.wikipedia.org/wiki/Frederick_Mosteller) or [anti-spam programs](http://en.wikipedia.org/wiki/Bayesian_spam_filtering)

**Footnote**: To this day, I still mix up the words for "spoon" ("cuchara") and "knife" ("cuchillo") in Spanish, thought I never make any mistakes with slang words like "dude" ("tio").
