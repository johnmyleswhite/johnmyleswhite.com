---
title: "The Psychology of Music and the 'tuneR' Package"
author: John Myles White
layout: post
permalink: /notebook/2011/10/25/the-psychology-of-music-and-the-tuner-package/
categories:
  - Music
  - Programming
  - Psychology
  - Statistics
---

### Introduction

This semester I'm TA'ing a course on the [Psychology of Music](http://psych.princeton.edu/psychology/research/johnson_laird/music.php) taught by [Phil Johnson-Laird](http://psych.princeton.edu/psychology/research/johnson_laird/). It's been a great course to teach because (i) so much of the material is new to me and (ii) because the study of the psychology of music brings together so many of the intellectual tools I enjoy, including music theory, psychophysics and Fourier analysis.

One topic this semester that was completely new to me was the theory of tuning: I had known about the invention of the [well-tempered system of tuning](http://en.wikipedia.org/wiki/Well_temperament), but had never heard of [Pythagorean tuning](http://en.wikipedia.org/wiki/Pythagorean_tuning) or [just tuning](http://en.wikipedia.org/wiki/Just_intonation) -- and certainly was not aware that the well-tempered system [Bach celebrated](http://en.wikipedia.org/wiki/The_Well-Tempered_Clavier) was not identical to our current equal-tempered system of tuning.

As a way of consolidating some of the knowledge I've gained, I decided I'd write a blog entry after several months of neglecting this blog. (For that neglect, I'll blame a combination of grant writing, book writing, ongoing research projects and personal life developments.) In what follows, I'll give a brief overview of the theory of tuning at a theoretical level that should be accessible to anyone who's familiar with the names of intervals and feels comfortable thinking quantitatively.

After surveying the field, I'll turn to a discussion of some code I've written in R that implements these ideas using the 'tuneR' package, which is one of my favorite hidden gems from CRAN. Along the way, I'll introduce some of the simplest tools from the 'tuneR' package that can be used for generating computer music.

### Tuning Systems: Pythagorean, Just and 12-Tet

It's worth noting right at the start that tuning is a misleading name for the topic we'll be discussing: we're not talking about how one tunes a fixed instrument so that it sounds in tune, but rather we're interested in how one defines the very notes that the instrument should be able to produce when it's perfectly in tune.

To make that clear, let's assume that we've accepted as a given that a frequency of 440 Hz will be called A. Our problem then becomes one of deciding which of the infinitely many frequencies we could produce actually deserves the label of A#, B, C, C#, and so on.

#### Pythagorean Tuning

The simplest solution to this problem I know of is the [Pythagorean tuning system](http://en.wikipedia.org/wiki/Just_intonation). It's based on constructing all of the possible notes using a series of perfect fifths. If you remember the [Circle of Fifths](http://en.wikipedia.org/wiki/Circle_of_fifths), you'll remember that you can reach every chromatic note by ascending fifths: if you start at A, you'll proceed through E, B, F# and so on.

The Pythagorean system implements the Circle of Fifths directly using repeated multiplication of a base frequency. To do this, you first declare that a perfect fifth is at a frequency 3/2 above your base frequency. For example, this definition implies that the perfect fifth above the A at 440 Hz has to be at a frequency of 3/2 * 440 = 660 Hz. Once you do this, you've defined the frequency we'll call E.

And following on with this logic, you produce a B at 990 Hz. Of course, this B occurs an octave above the base A at 440 Hz, so you transpose it down an octave to produce the B you'll actually use. To do this, you need to assume that an octave is at a frequency 2 times the base frequency. Since we've accepted that 990 Hz is a B, we divide 990 by 2 and conclude that 495 Hz should be B.

With these three notes defined, we have the following table of frequency/note pairs:

<table class="table table-hover">
<tr>
	<th>Note</th>
	<th>Frequency</th>
	<th>Ratio with 440 Hz</th>
</tr>
</thead>

<tbody>
<tr>
	<td>A</td>
	<td>440 Hz</td>
	<td>1</td>
</tr>
<tr>
	<td>E</td>
	<td>660 Hz</td>
	<td>3/2</td>
</tr>
<tr>
	<td>B</td>
	<td>495 Hz</td>
	<td>9/8</td>
</tr>
</tbody>
</table>

If we continue on with this logic and calculate many more multiplications by 3/2 and divisions by 2, we will eventually produce a complete table for all of the notes in the chromatic scale that looks like the following:

<table class="table table-hover">
<thead>
<tr>
	<th>Note</th>
	<th>Frequency</th>
	<th>Ratio</th>
</tr>
</thead>

<tbody>
<tr>
	<td>A</td>
	<td>440</td>
	<td>1</td>
</tr>
<tr>
	<td>A#</td>
	<td>463.5391</td>
	<td>256/243</td>
</tr>
<tr>
	<td>B</td>
	<td>495</td>
	<td>9/8</td>
</tr>
<tr>
	<td>C</td>
	<td>521.4815</td>
	<td>32/27</td>
</tr>
<tr>
	<td>C#</td>
	<td>556.875</td>
	<td>81/64</td>
</tr>
<tr>
	<td>D</td>
	<td>586.6667</td>
	<td>4/3</td>
</tr>
<tr>
	<td>D#</td>
	<td>626.4844</td>
	<td>729/512</td>
</tr>
<tr>
	<td>E</td>
	<td>660</td>
	<td>3/2</td>
</tr>
<tr>
	<td>F</td>
	<td>695.3086</td>
	<td>128/81</td>
</tr>
<tr>
	<td>F#</td>
	<td>742.5</td>
	<td>27/16</td>
</tr>
<tr>
	<td>G</td>
	<td>782.2222</td>
	<td>16/9</td>
</tr>
<tr>
	<td>G#</td>
	<td>835.3125</td>
	<td>243/128</td>
</tr>
<tr>
	<td>A</td>
	<td>880</td>
	<td>2</td>
</tr>
</tbody>
</table>

One thing about this table might strike you as odd if you're mathematically savvy: the octave, which we've defined by fiat as a ratio of 2:1, could never have been produced by successive multiplication by 3/2, since no power of 3 will be evenly divisible by a power of 2. This is the one flub in the Pythagorean system: you can't really produce the entire chromatic scale using only multiples of 3/2. Here we've solved that problem by replacing the note we would have called A with a true octave generated using multiplication by 2. Because the exact octave produced by Pythagorean tuning is slightly out of tune with our preferred definition of an octave, you may hear people refer to this discrepancy as the [the Pythagorean comma](http://en.wikipedia.org/wiki/Pythagorean_comma).

#### Just Tuning

Given that we had to cheat a bit to create a proper octave using the Pythagorean tuning system based on multiples of 3/2, it makes sense to ask why we shouldn't just allow ourselves to use other multipliers than 3/2. Looking at the Pythagoren tuning table, we see some pretty ugly fractions like 729/512. What if we forced these fractions to be simpler by employing ratios like 4/3 and 5/4 to build up the whole system?

The result of allowing ourselves several fractions beyond just those derived from 3/2 is called the [just tuning system][5]. Here we assume that perfect fifths occur at a frequency ratio of 3/2 and that perfect fourths occur at a frequency ratio of 4/3. Continuing on with this process, we eventually end up with the following tuning table:

<table class="table table-hover">
<thead>
<tr>
	<th>Note</th>
	<th>Frequency</th>
	<th>Ratio</th>
</tr>
</thead>

<tbody>
<tr>
	<td>A</td>
	<td>440</td>
	<td>1</td>
</tr>
<tr>
	<td>A#</td>
	<td>469.3333</td>
	<td>16/15</td>
</tr>
<tr>
	<td>B</td>
	<td>495</td>
	<td>9/8</td>
</tr>
<tr>
	<td>C</td>
	<td>528</td>
	<td>6/5</td>
</tr>
<tr>
	<td>C#</td>
	<td>550</td>
	<td>5/4</td>
</tr>
<tr>
	<td>D</td>
	<td>586.6667</td>
	<td>4/3</td>
</tr>
<tr>
	<td>D#</td>
	<td>625.7778</td>
	<td>64/45</td>
</tr>
<tr>
	<td>E</td>
	<td>660</td>
	<td>3/2</td>
</tr>
<tr>
	<td>F</td>
	<td>704</td>
	<td>8/5</td>
</tr>
<tr>
	<td>F#</td>
	<td>733.3333</td>
	<td>5/3</td>
</tr>
<tr>
	<td>G</td>
	<td>782.2222</td>
	<td>16/9</td>
</tr>
<tr>
	<td>G#</td>
	<td>825</td>
	<td>15/8</td>
</tr>
<tr>
	<td>A</td>
	<td>880</td>
	<td>2</td>
</tr>
</tbody>
</table>

This is the tuning that early Classical music was written in. Looking at the table you con immediately appreciate the theoretical assertion that the relative dissonance of an interval is determined by the simplicity of the ratio of frequencies between the two notes: perfect fifths are 3/2 and major thirds are 5/4, while minor seconds are 16/15 and major sevenths are 15/8. This is one of the things I most enjoy about the theory of harmony: there's a match between the aesthetics of fractions and the aesthetics of sounds that, for me, helps to justify my sense that certain fractions are more beautiful than others.

#### 12 Tet / Equal-Temperament

Now, if you know the history of Bach's Well-Tempered Clavier, you know that there is a problem with the just tuning system: it sounds great in the key you used as the base (here A), but it sounds a bit out of tune in other keys. The modern [12-tet system](http://en.wikipedia.org/wiki/Equal_temperament) is the most recent approach to solving this problem: you assume the gap between two semitones (e.g. A to A# or A# to B) is always the exact same multiple. Since you'll repeat this multiplication 12 times before reaching an octave, you can conclude that two notes that are a semitone apart must be separated by the 12th root of 2. Building a tuning system using that ratio alone gives us our modern system of tuning, which is shown in the table above using the decimal expansion of the ratios instead of their representation as powers of the 12th root of 2:

<table class="table table-hover">
<thead>
<tr>
	<th>Note</th>
	<th>Frequency</th>
	<th>Ratio</th>
</tr>
</thead>

<tbody>
<tr>
	<td>A</td>
	<td>440</td>
	<td>1.000000</td>
</tr>
<tr>
	<td>A#</td>
	<td>466.1638</td>
	<td>1.059463</td>
</tr>
<tr>
	<td>B</td>
	<td>493.8833</td>
	<td>1.122462</td>
</tr>
<tr>
	<td>C</td>
	<td>523.2511</td>
	<td>1.189207</td>
</tr>
<tr>
	<td>C#</td>
	<td>554.3653</td>
	<td>1.259921</td>
</tr>
<tr>
	<td>D</td>
	<td>587.3295</td>
	<td>1.334840</td>
</tr>
<tr>
	<td>D#</td>
	<td>622.2540</td>
	<td>1.414214</td>
</tr>
<tr>
	<td>E</td>
	<td>659.2551</td>
	<td>1.498307</td>
</tr>
<tr>
	<td>F</td>
	<td>698.4565</td>
	<td>1.587401</td>
</tr>
<tr>
	<td>F#</td>
	<td>739.9888</td>
	<td>1.681793</td>
</tr>
<tr>
	<td>G</td>
	<td>783.9909</td>
	<td>1.781797</td>
</tr>
<tr>
	<td>G#</td>
	<td>830.6094</td>
	<td>1.887749</td>
</tr>
<tr>
	<td>A</td>
	<td>880</td>
	<td>2.000000</td>
</tr>
</tbody>
</table>

### Listening to the Results

We've just described three ways to define the notes used in Western music. But how different do they sound? To answer that, I decided to produce a series of simple sine wave audio samples that were tuned using each of the three tuning systems. To produce those audio samples, I used the 'tuneR' package, which I'll describe now. Before you read on, you should install it from CRAN using the standard `install.packages('tuneR')` invocation.

### A tuneR Tutorial

The [tuneR](http://cran.r-project.org/web/packages/tuneR/index.html) package is an extremely convenient tool for generating audio files from R based on a numeric description of the audio stream. For the purposes of this discussion of tuning systems, we simply need to produce basic sine waves. Thankfully, that's very easy to do with tuneR. Here's an example:

{% highlight r %}
library('tuneR')

sound <- sine(440, bit = 16)

writeWave(sound, '440.wav')
{% endhighlight %}

Here we've loaded the tuneR package, created a 1s snippet of sine wave audio at 16 bits resolution using the `sine` function, and then written out the audio to a WAV file using `writeWave`. If you look at your current directory and listen to this file, you'll hear a sine wave at 440 Hz.

If you want to explore the use of `sine`, you can easily play with the duration of the sound by changing the `duration` parameter. If you want to, you can also change the sample rate and the bit rate, but I don't see any reason to do that while exploring ideas about tuning.

More important is knowing that you can superimpose two sine waves using the `` `+` `` operator and that you can concatenate them using the `bind` function. To show off producing octaves, for example, you might use the following code to hear an A at 440 Hz, then an A an octave above it, and finally the harmony they produce together:

{% highlight r %}
library('tuneR')

sound <- bind(sine(440, bit = 16),
              sine(880, bit = 16),
              sine(440, bit = 16) + sine(880, bit = 16))

writeWave(sound, 'octaves.wav')
{% endhighlight %}

Unfortunately, this sample code produces an error because of the naive addition we've implemented using the `` `+` `` operator. Adding two sine waves directly together overfills the bit rate we're using. To safely perform addition of two sine waves, we need to normalize the results of our summation using the `normalize` function. This gives us just one more line of code:

{% highlight r %}
library('tuneR')

sound <- bind(sine(440, bit = 16),
              sine(880, bit = 16),
              sine(440, bit = 16) + sine(880, bit = 16))

sound <- normalize(sound, unit = '16')

writeWave(sound, 'octaves.wav')
{% endhighlight %}

For reasons that are not clear to me, you have to specify the bit rate to `normalize` using the `unit` parameter rather than the `bit` parameter.

### Demoing Tuning Systems

Our little octave demo is cute, but we really want to know what more interesting harmonies like major thirds and minor seconds sound like in the various tuning systems we described. To do that, I first wrote a function called `interval` that spits out the multiplier you need to use to produce a given interval for any of the three tuning systems. That function is in a [GitHub repository](https://github.com/johnmyleswhite/computer_music) I've set up with code for making these demos. If you download that repository, you could load my `interval` function using a simple call to `source` like the one seen below. And using this `interval` function, we can generate demos of various intervals as follows:

{% highlight r %}
library('tuneR')
source('interval.R')

base <- 440

sound <- sine(base) + sine(interval('minor-second',
                                    tuning = 'pythagorean') * base)

sound <- normalize(sound, unit = '16')

writeWave(sound, 'minor_second_pythagorean.wav')
{% endhighlight %}

On GitHub there's a file called `test_intervals.R` that will go through and generate all of the intervals in all three tuning systems. If you run that file, you'll generate a lot of audio files you can listen to as demos of the three tuning systems we've described. For me, these tuning systems all produce intervals that sound surprisingly similar, though at high volumes I find it moderately easy to hear slight differences between the tuning systems. That said, I very much doubt I would pick up on them in a normal musical context.

That's the end of my little introduction to tuning systems and the use of the tuneR package to explore them. If you're interested in thinking computationally about music, I highly recommend playing around with tuneR until you feel like you can produce interesting results. I'm already working on trying to build up some interesting timbres to work with.
