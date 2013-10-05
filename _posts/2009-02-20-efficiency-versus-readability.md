---
title: Efficiency versus Readability
author: John Myles White
layout: post
permalink: /notebook/2009/02/20/efficiency-versus-readability/
categories:
  - Programming
---

Every programming language -- also every programmer -- must trade off between writing code that is readable and producing code that executes the absolute minimum number of instructions to perform a task. This is a constant source of potential decisions, because it is almost always possible to make code less understandable while making it more efficient. For example, iterating over an entire list can be inefficient if you are only looking for three elements that may well be at the front of the list, but code that implements a generic search of the entire list -- think `grep` -- is usually much simpler to understand.
