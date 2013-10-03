---
title: Symbolic Differentiation in Julia
author: John Myles White
layout: post
permalink: /notebook/2013/01/07/symbolic-differentiation-in-julia/
categories:
  - Programming
  - Statistics
---

### A Brief Introduction to Metaprogramming in Julia

In contrast to [my previous post](http://www.johnmyleswhite.com/notebook/2013/01/03/computers-are-machines/), which described one way in which Julia allows (and expects) the programmer to write code that directly employs the atomic operations offered by computers, this post is meant to introduce newcomers to some of Julia's higher level functions for metaprogramming. To make metaprogramming more interesting, we're going to build a system for symbolic differentiation in Julia.

Like Lisp, the Julia interpreter represents Julian expressions using normal data structures: every Julian expression is represented using an object of type `Expr`. You can see this by typing something like `:(x + 1)` into the Julia REPL:

{% highlight julia %}
:(x + 1) # => :(+(x,1))
 
typeof(:(x + 1)) #=> Expr
{% endhighlight %}

Looking at the REPL output when we enter an expression quoted using the `:` operator, we can see that Julia has rewritten our input expression, originally written using infix notation, as an expression that uses prefix notation. This standardization to prefix notation makes it easier to work with arbitrary expressions because it removes a needless source of variation in the format of expressions.

To develop an intuition for what this kind of expression means to Julia, we can use the `dump` function to examine its contents:

{% highlight julia %}
dump(:(x + 1))
# => Expr 
#   head: Symbol call
#   args: Array(Any,(3,))
#     1: Symbol +
#     2: Symbol x
#     3: Int64 1
#   typ: Any
{% endhighlight %}

Here you can see that a Julian expression consists of three parts:

1.  A `head` symbol, which describes the basic type of the expression. For this blog post, all of the expressions we'll work with have `head` equal to `:call`.
2.  An `Array{Any}` that contains the arguments of the `head`. In our example, the `head` is `:call`, which indicates a function call is being made in this expression. The arguments for the function call are:
1.  `:+`, the symbol denoting the addition function that we are calling.
2.  `:x`, the symbol denoting the variable `x`
3.  `1`, the number 1 represented as a 64-bit integer.

3.  A `typ` which stores type inference information. We'll ignore this information as it's not relevant to us right now.

Because each expression is built out of normal components, we can construct one piecemeal:

{% highlight julia %}
Expr(:call, {:+, 1, 1}, Any) # => :(+(1,1))
{% endhighlight %}

Because this expression only depends upon constants, we can immediately evaluate it using the `eval` function:

{% highlight julia %}
eval(Expr(:call, {:+, 1, 1}, Any)) # => 2
{% endhighlight %}

### Symbolic Differentiation in Julia

Now that we know how Julia expressions are built, we can design a very simple prototype system for doing symbolic differentiation in Julia. We'll build up our system in pieces using some of the most basic rules of calculus:

1.  **The Constant Rule**: `d/dx c = 0`
2.  **The Symbol Rule**: `d/dx x = 1`, `d/dx y = 0`
3.  **The Sum Rule**: `d/dx (f + g) = (d/dx f) + (d/dx g)`
4.  **The Subtraction Rule**: `d/dx (f - g) = (d/dx f) - (d/dx g)`
5.  **The Product Rule**: `d/dx (f * g) = (d/dx f) * g + f * (d/dx g)`
6.  **The Quotient Rule**: `d/dx (f / g) = [(d/dx f) * g - f * (d/dx g)] / g^2`

Implementing these operations is quite easy once you understand the data structure Julia uses to represent expressions. And some of these operations would be trivial regardless.

For example, here's the Constant Rule in Julia:

{% highlight julia %}
differentiate(x::Number, target::Symbol) = 0
{% endhighlight %}

And here's the Symbol rule:

{% highlight julia %}
function differentiate(s::Symbol, target::Symbol)
    if s == target
        return 1
    else
        return 0
    end
end
{% endhighlight %}

The first two rules of calculus don't actually require us to understand anything about Julian expressions. But the interesting parts of a symbolic differentiation system do. To see that, let's look at the Sum Rule:

{% highlight julia %}
function differentiate_sum(ex::Expr, target::Symbol)
    n = length(ex.args)
    new_args = Array(Any, n)
    new_args[1] = :+
    for i in 2:n
        new_args[i] = differentiate(ex.args[i], target)
    end
    return Expr(:call, new_args, Any)
end
{% endhighlight %}

The Subtraction Rule can be defined almost identically:

{% highlight julia %}
function differentiate_subtraction(ex::Expr, target::Symbol)
    n = length(ex.args)
    new_args = Array(Any, n)
    new_args[1] = :-
    for i in 2:n
        new_args[i] = differentiate(ex.args[i], target)
    end
    return Expr(:call, new_args, Any)
end
{% endhighlight %}

The Product Rule is a little more interesting because we need to build up an expression whose components are themselves expressions:

{% highlight julia %}
function differentiate_product(ex::Expr, target::Symbol)
    n = length(ex.args)
    res_args = Array(Any, n)
    res_args[1] = :+
    for i in 2:n
       new_args = Array(Any, n)
       new_args[1] = :*
       for j in 2:n
           if j == i
               new_args[j] = differentiate(ex.args[j], target)
           else
               new_args[j] = ex.args[j]
           end
       end
       res_args[i] = Expr(:call, new_args, Any)
    end
    return Expr(:call, res_args, Any)
end
{% endhighlight %}

Last, but not least, here's the Quotient Rule, which is a little more complex. We can code this rule up in a more explicit fashion that doesn't use any loops so that we can directly see the steps we're taking:

{% highlight julia %}
function differentiate_quotient(ex::Expr, target::Symbol)
    return Expr(:call,
                {
                    :/,
                    Expr(:call,
                         {
                            :-,
                            Expr(:call,
                                 {
                                    :*,
                                    differentiate(ex.args[2], target),
                                    ex.args[3]
                                 },
                                 Any),
                            Expr(:call,
                                 {
                                    :*,
                                    ex.args[2],
                                    differentiate(ex.args[3], target)
                                 },
                                 Any)
                         },
                         Any),
                    Expr(:call,
                         {
                            :^,
                            ex.args[3],
                            2
                         },
                         Any)
                },
                Any)
end
{% endhighlight %}

Now that we have all of those basic rules of calculus implemented as functions, we'll build up a lookup table that we can use to tell our final `differentiate` function where to send new expressions based on the kind of function's that being differentiated during each call to `differentiate`:

{% highlight julia %}
differentiate_lookup = {
                          :+ => differentiate_sum,
                          :- => differentiate_subtraction,
                          :* => differentiate_product,
                          :/ => differentiate_quotient
                       }
{% endhighlight %}

With all of the core machinery in place, the final definition of `differentiate` is very simple:

{% highlight julia %}
function differentiate(ex::Expr, target::Symbol)
    if ex.head == :call
        if has(differentiate_lookup, ex.args[1])
            return differentiate_lookup[ex.args[1]](ex, target)
        else
            error("Don't know how to differentiate $(ex.args[1])")
        end
    else
        return differentiate(ex.head)
    end
end
{% endhighlight %}

Ive put all of these snippets together in a single [GitHub](https://gist.github.com/4475902) Gist. To try out this new differentiation function, let's copy the contents of that GitHub gist into a file called `differentiate.jl`. We can then load the contents of that file into Julia at the REPL using `include`, which will allow us try out our differentiation tool:

{% highlight julia %}
include("differentiate.jl")
 
differentiate(:(x + x*x), :x)
# => :(+(1,+(*(1,x),*(x,1))))
 
differentiate(:(x + a*x), :x)
# => :(+(1,+(*(0,x),*(a,1))))
{% endhighlight %}

While the expressions that are constructed by our `differentiate` function are ugly, they are correct: they just need to be simplified so that things like `*(0, x)` are replaced with ``. If you'd like to see how to write code to perform some basic simplifications, you can see the [`simplify` function](https://github.com/johnmyleswhite/Calculus.jl/blob/master/src/symbolic.jl) I've been building for Julia's new [Calculus package](https://github.com/johnmyleswhite/Calculus.jl). That codebase includes all of the functionality shown here for `differentiate`, along with several other rules that make the system more powerful.

What I love about Julia is the ease with which one can move from low-level bit operations like those described in my previous post to high-level operations that manipulate Julian expressions. By allowing the programmer to manipulate expressions programmatically, Julia has copied one of the most beautiful parts of Lisp.
