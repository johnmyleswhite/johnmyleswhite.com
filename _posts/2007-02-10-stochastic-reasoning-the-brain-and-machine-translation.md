---
title: Stochastic Reasoning, The Brain, And Machine Translation
author: John Myles White
layout: post
permalink: /notebook/2007/02/10/stochastic-reasoning-the-brain-and-machine-translation/
categories:
  - Observations
---

The basic rule for processing language, expressed dangerously unspecifically, is this:

<blockquote>
<p>Break up the sentence into as large units as possible, resolving every ambiguity using a probabilistic system that recalls the likelihood of one interpretation over another. (<em>This mode of processing is so fundamental to the brain's functioning that one seldom realizes that there is any ambiguity in a given sentence.</em>)</p>
</blockquote>

Examples of how this should be handled abound: the sentence "he is being chased by a red coat" does not involve a man being chased by a sinister work of sartory because the probablistic parsing of "red coat" is not to parse it as adjective, then noun, but as a compound noun phrase. In Spanish, one would always interpret "te echo de menos" taking "echo de menos" as a complete chunk. For computers, this chunking is made complicated by the way that most languages allow words to be reordered. (One solution thus would be a system for reordering sentences back into a canonical ordering.) Babelfish fails badly when the units of a chunk are separated (for much the same reason that some writers try to avoid split infinitives): "te echo tanto de menos" becomes "I throw to you as much of less" while "te echo de menos tanto" is close to correct, though still rather derisory, in being translated as "I miss to you as much".

One should keep in mind that the brain is radically more efficient than computers when faced with problems like these that involve probabilistic reasoning because calculations of probability do not need to be done explicitly in the brain: the pathway that has most often executed in the past executes most strongly in the present by default because of long-term potentiation. Ambiguity is often scarcely felt because the most probable interpretation always wins by physiological necessity; by this same physiological necessity, new idiomatic expressions and clichés are inevitably being created every day. This is often as much a help as a hindrance in a world that does contain ambiguities that ought to be recognized as ambiguous, but it is a basic design constraint of the brain.
