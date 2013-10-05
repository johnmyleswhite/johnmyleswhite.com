---
title: "My New Book: Developing, Deploying and Debugging Multi-Armed Bandit Algorithms"
author: John Myles White
layout: post
permalink: /notebook/2012/07/28/my-new-book-developing-deploying-and-debugging-multi-armed-bandit-algorithms/
categories:
  - Programming
  - Statistics
---

I'm happy to announce that I've started writing a new book for O'Reilly, which will focus on teaching readers how to use Multi-Armed Bandit Algorithms to build better websites. My hope is that the book can help web developers build up an intuition for the core conundrum facing anyone who wants to build a successful business: you have to constantly make trade-offs between (A) making decisions that are likely to yield safe, short-term successes and (B) taking chances on new opportunities that could be good long-term strategies, but could also be spectacular failures.

In the academic literature, this trade-off is usually called the [Explore-Exploit dilemma](http://seedmagazine.com/content/article/to_exploit_or_explore/). It's been studied for decades because there is no universally applicable, out-of-the-box solution. There are several simple methods that nearly everyone knows about (like A/B testing), but these methods can lead to spectacular failures. For that reason, academics have invested a huge amount of intellectual energy into developing better approaches than the sort of randomized experimentation embodied in A/B testing.

In large part, they've made progress by studying an idealized version of the Explore-Exploit tradeoff called the Multi-Armed Bandit Problem. A Multi-Armed Bandit Problem is a thought experiment in which you try to make the most money possible while gambling at a casino that features only an assortment of different slot machines, each of which is called a bandit because of its propensity to steal the gambler's money.

In the book, I'll cover a few of the standard algorithms developed based on this thought experiment, including the $\epsilon$-greedy algorithm, softmax decision rules and a family of algorithms collectively called UCB. All of these algorithms will be implemented in Python, but my primary interest in writing the book isn't to provide code for these algorithms to readers, but to give them a framework that will allow them to reason about how to solve the Explore-Exploit dilemma as it impacts their businesses and the general art of developing better websites. There are now many, many different algorithms available for solving the Multi-Armed Bandit Problem, but they all share a few core intuitions and strategies. I'm hoping to communicate those higher level intuitions to readers.

At the same time, I want to provide readers with a toolkit for deciding between the various options that are available to them. Web developers are very familiar with the importance of debugging, so I'd like to teach them the equivalent of debugging for Multi-Armed Bandit Problems, which involves simulation. Instead of building unit tests in which you confirm that your code can solve toy problems with clear answers, you run simulations to test that your algorithm can learn the right answer to toy problems with unambiguous right answers.

In summary, I hope readers will leave this book with a toolbox of strategies for developing, deploying and debugging multi-armed bandit algorithms in the context of web development. At large companies, these methods are responsible for millions of dollars of increased profits, so I think it's high time that the core ideas trickle down from elite institutions like Google, Microsoft and Facebook to other web companies.

I'm really excited about this project and look forward to sharing more details as the book progresses.
