---
title: Simulated Annealing in Julia
author: John Myles White
layout: post
permalink: /notebook/2012/04/04/simulated-annealing-in-julia/
categories:
  - Statistics
---

### Building Optimization Functions for Julia

In hopes of adding enough statistical functionality to [Julia](http://julialang.org/) to make it usable for my day-to-day modeling projects, I've written a very basic implementation of the simulated annealing (SA) algorithm, which I've placed in the same [JuliaVsR GitHub](https://github.com/johnmyleswhite/JuliaVsR) repository that I used for the code for [my previous post about Julia](http://www.johnmyleswhite.com/notebook/2012/03/31/julia-i-love-you/). For easier reading, my current code for SA is shown below:

### The Simulated Annealing Algorithm

{% highlight julia %}
# simulated_annealing()
# Arguments:
# * cost: Function from states to the real numbers. Often called an energy function, but this algorithm works for both positive and negative costs.
# * s0: The initial state of the system.
# * neighbor: Function from states to states. Produces what the Metropolis algorithm would call a proposal.
# * temperature: Function specifying the temperature at time i.
# * iterations: How many iterations of the algorithm should be run? This is the only termination condition.
# * keep_best: Do we return the best state visited or the last state visisted? (Should default to true.)
# * trace: Do we show a trace of the system's evolution?

function simulated_annealing(cost,
                             s0,
                             neighbor,
                             temperature,
                             iterations,
                             keep_best,
                             trace)
                             
  # Set our current state to the specified intial state.
  s = s0

  # Set the best state we've seen to the intial state.
  best_s = s0

  # We always perform a fixed number of iterations.
  for i = 1:iterations
    t = temperature(i)
    s_n = neighbor(s)
    if trace println("$i: s = $s") end
    if trace println("$i: s_n = $s_n") end
    y = cost(s)
    y_n = cost(s_n)
    if trace println("$i: y = $y") end
    if trace println("$i: y_n = $y_n") end
    if y_n <= y
      s = s_n
      if trace println("Accepted") end
    else
      p = exp(-(y_n - y) / t)
      if trace println("$i: p = $p") end
      if rand() <= p
        s = s_n
        if trace println("Accepted") end
      else
        s = s
        if trace println("Rejected") end
      end
    end
    if trace println() end
    if cost(s) < cost(best_s)
      best_s = s
    end
  end
  
  if keep_best
    best_s
  else
    s
  end
end
{% endhighlight %}

I've tried to implement the algorithm as it was presented by [Bertsimas](http://www.mit.edu/~dbertsim/) and [Tsitsiklis](http://www.mit.edu/~jnt/home.html) in their [1993 review paper in Statistical Science](http://www.mit.edu/~jnt/Papers/J045-93-ber-anneal.pdf). The major differences between my implementation and their description of the algorithm is that (1) I've made it possible to keep the best solution found during the search rather than always use the last solution found and (2) I've made no effort to select a value for their `d` parameter carefully: I've simply set it to 1 for all of my examples, though my code will allow you to specify another rule for setting the temperature of the annealer at time t other than the `1 / log(t)` cooling scheme I've been using. (In fact, the code currently forces you to select a cooling scheme, since there are no default arguments in Julia yet.)

I chose simulated annealing as my first optimization algorithm to implement in Julia because it's a natural relative of the [Metropolis algorithm](http://en.wikipedia.org/wiki/Metropolis–Hastings_algorithm) that I used in the previous post. Indeed, coding up an implementation of SA made me appreciate the fact that the Metropolis algorithm as used in Bayesian statistics can be considered a special case of the SA algorithm in which the temperature is always 1 and in which the cost function for a state with posterior probability `p` is `-log(p)`.

Coding up the SA algorithm for myself also me made realize why it's important that it uses an additive comparison of cost functions rather than a ratio (as in the Metropolis algorithm): the ratio goes haywire when the cost function can take on both positive and negative values (which, of course, doesn't matter for Bayesian methods because probabilities are strictly non-negative). I discovered this when I initially tried to code up SA from my inaccurate memory without first consulting the literature and discovered that I couldn't get a ratio-based algorithm to work no matter how many times I tried changing the cooling schedule.

To test out the SA implementation I've written, I've written two tests cases that attempt to minimize the [Rosenbrock](http://en.wikipedia.org/wiki/Rosenbrock_function) and [Himmelbrau](http://en.wikipedia.org/wiki/Himmelblau%27s_function) functions, which I found listed as examples of hard-to-minimize functions in the [Wikipedia description of the Nelder-Mead method](http://en.wikipedia.org/wiki/Nelder–Mead_method). You can see those two examples below this paragraph. In addition, I've used R to generate plots showing how the SA algorithm works under repeated application on the same optimization problem; in these plots, I've used a heatmap to show the cost functions value at each `(x, y)` position, colored crosshairs to indicate the position of a true minimum of the function in question, and red dots to indicate the purported solutions found by my implementation of SA.

### Finding the Minimum of the Rosenbrock Function

{% highlight julia %}
# Find a solution of the Rosenbrock function using SA.
load("simulated_annealing.jl")
load("../rng.jl")

function rosenbrock(x, y)
  (1 - x)^2 + 100(y - x^2)^2
end

function neighbors(z)
  [rand_uniform(z[1] - 1, z[1] + 1), rand_uniform(z[2] - 1, z[2] + 1)]
end

srand(1)

solution = simulated_annealing(z -> rosenbrock(z[1], z[2]),
                               [0, 0],
                               neighbors,
                               log_temperature,
                               10000,
                               true,
                               false)

{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2012/04/rosenbrock.png" alt="rosenbrock.png" />
</center>

### Finding the Minima of the Himmelbrau Function

{% highlight julia %}
# Find a solution of the Himmelbrau function using SA.
load("simulated_annealing.jl")
load("../rng.jl")

function himmelbrau(x, y)
  (x^2 + y - 11)^2 + (x + y^2 - 7)^2
end

function neighbors(z)
  [rand_uniform(z[1] - 1, z[1] + 1), rand_uniform(z[2] - 1, z[2] + 1)]
end

srand(1)

solution = simulated_annealing(z -> himmelbrau(z[1], z[2]),
                               [0, 0],
                               neighbors,
                               log_temperature,
                               10000,
                               true,
                               false)

{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2012/04/himmelbrau.png" alt="himmelbrau.png" />
</center>

### Moving Forward

Now that I've got a form of SA working, I'm interested in coding up a suite of optimization functions for Julia so that I can start to do maximum likelihood estimation in pure Julia. Once that's possible, I can use Julia to do real science, e.g. when I need to fit simple models for which finding the MLE is appropriate. (I will leave the development of cleaner statistical functions for special cases of maximum likelihood estimation to more capable people, like Douglas Bates, [who has already produced some GLM code](https://groups.google.com/d/topic/julia-dev/GeouH--RPZo/discussion).)

At present my code is meant simply to demonstrate how one could write an implementation of simulated annealing in Julia. I'm sure that the code can be more efficient and I suspect that I've violated some of the idioms of the language. In addition, I'd much prefer that this function use default values for many of the arguments, as there is no reason that an end-user needs to be concerned with finding the best cooling schedule if SA seems to work out of the box on their problem with the cooling schedule I've been using.

With those disclaimers about my code in place, I'd like to think that I haven't made any mathematical errors and that this function can be used by others. So, I'd ask that those interested please tear apart my code so that I can make it usable as a general purpose function for optimization in Julia. Alternatively, please tell me that there's no need for a pure Julia implementation of SA because, for example, there are nice C libraries that would be much easier to link to than to re-implement.

With an implementation of SA in place, I'll probably start working on implementing L-BFGS-S soon, which is the other optimization algorithm I use often in R. (To be honest, I use L-BFGS-S almost exclusively, but SA was much easier to implement.)

Incidentally, this code base demonstrates how I view the relationship between R and Julia: Julia is a beautiful new language that is still missing many important pieces. We can all work together to build the best pieces of R that are missing from Julia. While we're working on improving Julia, we'll need to keep using R to handle things like visualization of our results. For this post, I turned back to ggplot2 for all of the graphics generation.
