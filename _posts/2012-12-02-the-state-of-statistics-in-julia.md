---
title: The State of Statistics in Julia
author: John Myles White
layout: post
permalink: /notebook/2012/12/02/the-state-of-statistics-in-julia/
categories:
  - Julia
  - Programming
  - Statistics
---

**Updated 12.2.2012: Added sample output based on a suggestion from Stefan Karpinski.**

### Introduction

Over the last few weeks, the Julia core team has rolled out a demo version of Julia's [package management system](https://github.com/JuliaLang/METADATA.jl). While the Julia package system is still very much in beta, it nevertheless provides the first plausible way for non-expert users to see where Julia's growing community of developers is heading.

To celebrate some of the amazing work that's already been done to make Julia usable for day-to-day data analysis, I'd like to give a brief overview of the state of statistical programming in Julia. There are now several packages that, taken as a whole, suggest that Julia may really live up to its potential and become the next generation language for data analysis.

### Getting Julia Installed

If you'd like to try out Julia for yourself, you'll first need to clone the current Julia repo from [GitHub](https://github.com/JuliaLang/julia
) and then build Julia from source as described in the Julia [README](https://github.com/JuliaLang/julia/blob/master/README.md
). Compiling Julia for the first time can take up to two hours, but updating Julia afterwards will be quite fast once you've gotten a working copy of the language and its dependencies installed on your system. After you have Julia built, you should add its main directory to your path and then open up the Julia REPL by typing `julia` at the command line.

### Installing Packages

Once Julia's REPL is running, you can use the following commands to start installing packages:

{% highlight julia %}
julia> require("pkg")
 
julia> Pkg.init()
Initialized empty Git repository in /Users/johnmyleswhite/.julia/.git/
Cloning into 'METADATA'...
remote: Counting objects: 443, done.
remote: Compressing objects: 100% (208/208), done.
remote: Total 443 (delta 53), reused 423 (delta 33)
Receiving objects: 100% (443/443), 38.98 KiB, done.
Resolving deltas: 100% (53/53), done.
[master (root-commit) dbd486e] empty package repo
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 METADATA
 create mode 100644 REQUIRE
 
julia> Pkg.add("DataFrames", "Distributions", "MCMC", "Optim", "NHST", "Clustering")
Installing DataFrames: v0.0.0
Cloning into 'DataFrames'...
remote: Counting objects: 1340, done.
remote: Compressing objects: 100% (562/562), done.
remote: Total 1340 (delta 760), reused 1229 (delta 655)
Receiving objects: 100% (1340/1340), 494.79 KiB, done.
Resolving deltas: 100% (760/760), done.
Installing Distributions: v0.0.0
Cloning into 'Distributions'...
remote: Counting objects: 49, done.
remote: Compressing objects: 100% (30/30), done.
remote: Total 49 (delta 8), reused 49 (delta 8)
Receiving objects: 100% (49/49), 17.29 KiB, done.
Resolving deltas: 100% (8/8), done.
Installing MCMC: v0.0.0
Cloning into 'MCMC'...
warning: no common commits
remote: Counting objects: 155, done.
remote: Compressing objects: 100% (97/97), done.
remote: Total 155 (delta 66), reused 140 (delta 51)
Receiving objects: 100% (155/155), 256.68 KiB, done.
Resolving deltas: 100% (66/66), done.
Installing NHST: v0.0.0
Cloning into 'NHST'...
remote: Counting objects: 20, done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 20 (delta 2), reused 19 (delta 1)
Receiving objects: 100% (20/20), 4.31 KiB, done.
Resolving deltas: 100% (2/2), done.
Installing Optim: v0.0.0
Cloning into 'Optim'...
remote: Counting objects: 497, done.
remote: Compressing objects: 100% (191/191), done.
remote: Total 497 (delta 318), reused 476 (delta 297)
Receiving objects: 100% (497/497), 79.68 KiB, done.
Resolving deltas: 100% (318/318), done.
Installing Options: v0.0.0
Cloning into 'Options'...
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 10 (delta 1), reused 6 (delta 0)
Receiving objects: 100% (10/10), done.
Resolving deltas: 100% (1/1), done.
Installing Clustering: v0.0.0
Cloning into 'Clustering'...
remote: Counting objects: 38, done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 38 (delta 7), reused 38 (delta 7)
Receiving objects: 100% (38/38), 7.77 KiB, done.
Resolving deltas: 100% (7/7), done.
{% endhighlight %}

That will get you started with some of the core tools for doing statistical programming in Julia. You'll probably also want to install another package called "RDatasets", which provides access to 570 of the classic data sets available in R. This package has a much larger file size than the others, which is why I recommend installing it after you've first installed the other packages:

{% highlight julia %}
require("pkg")
 
julia> Pkg.add("RDatasets")
Installing RDatasets: v0.0.0
Cloning into 'RDatasets'...
remote: Counting objects: 609, done.
remote: Compressing objects: 100% (588/588), done.
remote: Total 609 (delta 21), reused 605 (delta 17)
Receiving objects: 100% (609/609), 10.56 MiB | 1.15 MiB/s, done.
Resolving deltas: 100% (21/21), done.
{% endhighlight %}

Assuming that you've gotten everything working, you can then type the following to load Fisher's classic Iris data set:

{% highlight julia %}
julia> load("RDatasets")
Warning: redefinition of constant NARule ignored.
Warning: New definition ==(NAtype,Any) is ambiguous with ==(Any,AbstractArray{T,N}).
         Make sure ==(NAtype,AbstractArray{T,N}) is defined first.
Warning: New definition ==(Any,NAtype) is ambiguous with ==(AbstractArray{T,N},Any).
         Make sure ==(AbstractArray{T,N},NAtype) is defined first.
Warning: New definition replace!(PooledDataVec{S},NAtype,T) is ambiguous with replace!(PooledDataVec{S},T,NAtype).
         Make sure replace!(PooledDataVec{S},NAtype,NAtype) is defined first.
Warning: New definition promote_rule(Type{AbstractDataVec{T}},Type{T}) is ambiguous with promote_rule(Type{AbstractDataVec{S}},Type{T}).
         Make sure promote_rule(Type{AbstractDataVec{T}},Type{T}) is defined first.
Warning: New definition ^(NAtype,T<:Union(String,Number)) is ambiguous with ^(Any,Integer).
         Make sure ^(NAtype,_<:Integer) is defined first.
Warning: New definition ^(DataVec{T},Number) is ambiguous with ^(Any,Integer).
         Make sure ^(DataVec{T},Integer) is defined first.
Warning: New definition ^(DataFrame,Union(NAtype,Number)) is ambiguous with ^(Any,Integer).
         Make sure ^(DataFrame,Integer) is defined first.
 
julia> using DataFrames
 
julia> using RDatasets
 
julia> iris = data("datasets", "iris")
DataFrame  (150,6)
              Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
[1,]        1          5.1         3.5          1.4         0.2    "setosa"
[2,]        2          4.9         3.0          1.4         0.2    "setosa"
[3,]        3          4.7         3.2          1.3         0.2    "setosa"
[4,]        4          4.6         3.1          1.5         0.2    "setosa"
[5,]        5          5.0         3.6          1.4         0.2    "setosa"
[6,]        6          5.4         3.9          1.7         0.4    "setosa"
[7,]        7          4.6         3.4          1.4         0.3    "setosa"
[8,]        8          5.0         3.4          1.5         0.2    "setosa"
[9,]        9          4.4         2.9          1.4         0.2    "setosa"
[10,]      10          4.9         3.1          1.5         0.1    "setosa"
[11,]      11          5.4         3.7          1.5         0.2    "setosa"
[12,]      12          4.8         3.4          1.6         0.2    "setosa"
[13,]      13          4.8         3.0          1.4         0.1    "setosa"
[14,]      14          4.3         3.0          1.1         0.1    "setosa"
[15,]      15          5.8         4.0          1.2         0.2    "setosa"
[16,]      16          5.7         4.4          1.5         0.4    "setosa"
[17,]      17          5.4         3.9          1.3         0.4    "setosa"
[18,]      18          5.1         3.5          1.4         0.3    "setosa"
[19,]      19          5.7         3.8          1.7         0.3    "setosa"
[20,]      20          5.1         3.8          1.5         0.3    "setosa"
  :
[131,]    131          7.4         2.8          6.1         1.9 "virginica"
[132,]    132          7.9         3.8          6.4         2.0 "virginica"
[133,]    133          6.4         2.8          5.6         2.2 "virginica"
[134,]    134          6.3         2.8          5.1         1.5 "virginica"
[135,]    135          6.1         2.6          5.6         1.4 "virginica"
[136,]    136          7.7         3.0          6.1         2.3 "virginica"
[137,]    137          6.3         3.4          5.6         2.4 "virginica"
[138,]    138          6.4         3.1          5.5         1.8 "virginica"
[139,]    139          6.0         3.0          4.8         1.8 "virginica"
[140,]    140          6.9         3.1          5.4         2.1 "virginica"
[141,]    141          6.7         3.1          5.6         2.4 "virginica"
[142,]    142          6.9         3.1          5.1         2.3 "virginica"
[143,]    143          5.8         2.7          5.1         1.9 "virginica"
[144,]    144          6.8         3.2          5.9         2.3 "virginica"
[145,]    145          6.7         3.3          5.7         2.5 "virginica"
[146,]    146          6.7         3.0          5.2         2.3 "virginica"
[147,]    147          6.3         2.5          5.0         1.9 "virginica"
[148,]    148          6.5         3.0          5.2         2.0 "virginica"
[149,]    149          6.2         3.4          5.4         2.3 "virginica"
[150,]    150          5.9         3.0          5.1         1.8 "virginica"
 
julia> head(iris)
DataFrame  (6,6)
          Sepal.Length Sepal.Width Petal.Length Petal.Width  Species
[1,]    1          5.1         3.5          1.4         0.2 "setosa"
[2,]    2          4.9         3.0          1.4         0.2 "setosa"
[3,]    3          4.7         3.2          1.3         0.2 "setosa"
[4,]    4          4.6         3.1          1.5         0.2 "setosa"
[5,]    5          5.0         3.6          1.4         0.2 "setosa"
[6,]    6          5.4         3.9          1.7         0.4 "setosa"
 
julia> tail(iris)
DataFrame  (6,6)
            Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
[1,]    145          6.7         3.3          5.7         2.5 "virginica"
[2,]    146          6.7         3.0          5.2         2.3 "virginica"
[3,]    147          6.3         2.5          5.0         1.9 "virginica"
[4,]    148          6.5         3.0          5.2         2.0 "virginica"
[5,]    149          6.2         3.4          5.4         2.3 "virginica"
[6,]    150          5.9         3.0          5.1         1.8 "virginica"
{% endhighlight %}

Now that you can see that Julia can handle complex data sets, let's talk a little bit about the packages that make statistical analysis in Julia possible.

### The DataFrames Package

The [DataFrames]() package provides data structures for working with tabular data in Julia. At a minimum, this means that DataFrames provides tools for dealing with individual columns of missing data, which are called `DataVec`'s. A collection of `DataVec`'s allows one to build up a `DataFrame`, which provides a tabular data structure like that used by R's `data.frame` type.

{% highlight julia %}
julia> load("DataFrames")
 
julia> using DataFrames
 
julia> data = {"Value" => [1, 2, 3], "Label" => ["A", "B", "C"]}
Warning: imported binding for data overwritten in module Main
{"Label"=>["A", "B", "C"],"Value"=>[1, 2, 3]}
 
julia> df = DataFrame(data)
DataFrame  (3,2)
        Label Value
[1,]      "A"     1
[2,]      "B"     2
[3,]      "C"     3
 
julia> df["Value"]
3-element DataVec{Int64}
 
[1,2,3]
 
julia> df[1, "Value"] = NA
NA
 
 
julia> head(df)
DataFrame  (3,2)
        Label Value
[1,]      "A"    NA
[2,]      "B"     2
[3,]      "C"     3
{% endhighlight %}

### Distributions

The [Distributions](https://github.com/JuliaStats/Distributions.jl
) package provides tools for working with probability distributions in Julia. It reifies distributions as types in Julia's large type hierarchy, which means that quite generic names like `rand` can be used to sample from complex distributions:

{% highlight julia %}
julia> load("Distributions")
julia> using Distributions
 
julia> x = rand(Normal(11.0, 3.0), 10_000)
10000-element Float64 Array:
  6.87693
 13.3676 
  7.25008
  8.82833
 10.6911 
  7.1004 
 13.7449 
  5.96412
  8.57957
 15.2737 
  ⋮      
  4.89007
 15.1509 
  6.32376
  7.83847
 14.4476 
 14.2974 
  9.74783
  9.67398
 14.4992 
 
julia> mean(x)
11.00366217730023
 
julia> var(x)
Warning: Possible conflict in library symbol ddot_
9.288938550823996
{% endhighlight %}

### Optim

The [Optim](https://github.com/johnmyleswhite/Optim.jl) package provides tools for numerical optimization of arbitrary functions in Julia. It provides a function, `optimize`, which works a bit like R's `optim` function.

{% highlight julia %}
julia> load("Optim")
julia> using Optim
 
julia> f = v -> (10.9 - v[1])^2 + (7.3 - v[2])^2
#<function>
 
julia> initial_guess = [0.0, 0.0]
2-element Float64 Array:
 0.0
 0.0
 
julia> results = optimize(f, initial_guess)
Warning: Possible conflict in library symbol dcopy_
OptimizationResults("Nelder-Mead",[0.333333, 0.333333],[10.9, 7.29994],3.2848148720460163e-9,38,true)
 
julia> results.minimum
2-element Float64 Array:
 10.9    
  7.29994
{% endhighlight %}

### MCMC

The [MCMC](https://github.com/doobwa/mcmc.jl) package provides tools for sampling from arbitrary probability distributions using Markov Chain Monte Carlo. It provides functions like `slice_sampler`, which allows one to sample from a (potentially unnormalized) density function using Radford Neal's slice sampling algorithm.

{% highlight julia %}
julia> load("MCMC")
 
julia> using MCMC
 
julia> d = Normal(17.29, 1.0)
Normal(17.29,1.0)
 
julia> f = x -> logpdf(d, x)
#<function>
 
julia> [slice_sampler(0.0, f) for i in 1:100]
100-element (Float64,Float64) Array:
 (2.7589100475626323,-106.49522613611775) 
 (22.840595204318323,-16.323492094305458) 
 (0.11800384424353683,-148.35766451986206)
 (25.507580447082677,-34.68325273534245)  
 (25.794565860846134,-37.08275877393945)  
 (25.898128716394307,-37.96887853221083)  
 (9.309878825853284,-32.76010551023705)   
 (30.824102772255355,-92.50490745818972)  
 (9.108789186504177,-34.38504372063516)   
 (25.547686903330494,-35.01363502992266)  
 ⋮                                        
 (5.795001414731885,-66.98643477086263)   
 (15.50115292212293,-2.518925467219337)   
 (12.046429369881345,-14.666455009726143) 
 (17.25455052645699,-0.919566865791911)   
 (25.494698549206657,-34.57747767488159)  
 (1.8340810959111111,-120.36165311809079) 
 (2.7112428736526177,-107.18901820771696) 
 (9.21203292192012,-33.54571459047587)    
 (19.12274407701784,-2.5984139591266584)
{% endhighlight %}

### NHST

The [NHST](https://github.com/johnmyleswhite/NHST.jl) package provides tools for testing standard statistical hypotheses using null hypothesis significance testing tools like the t-test and the chi-squared test.

{% highlight julia %}
julia> load("Distributions")
 
julia> using Distributions
 
julia> load("NHST")
 
julia> using NHST
 
julia> d1 = Normal(17.29, 1.0)
Normal(17.29,1.0)
 
julia> d2 = Normal(0.0, 1.0)
Normal(0.0,1.0)
 
julia> x = rand(d1, 1_000)
1000-element Float64 Array:
 15.7085
 18.585 
 16.6036
 18.962 
 17.8715
 16.6814
 17.9676
 16.8924
 16.6022
 17.9813
  ⋮     
 17.1339
 17.3964
 18.6184
 16.7238
 18.5003
 16.1618
 17.9198
 17.4928
 18.715 
 
julia> y = rand(d2, 1_000)
1000-element Float64 Array:
  0.664885 
  0.147182 
  0.96265  
  0.24282  
  1.881    
 -0.632478 
  0.539297 
  0.996562 
 -0.483302 
  0.514629 
  ⋮        
  2.06249  
 -0.549444 
  0.857575 
 -1.47464  
 -2.33243  
  0.510751 
 -0.381069 
 -1.49165  
  0.0521203
 
julia> t_test(x, y)
HypothesisTest("t-Test",{"t"=>392.2838409538002},{"df"=>1989.732411290855},0.0,[17.1535, 17.3293],{"mean of x"=>17.24357323225425,"mean of y"=>0.0021786523177457794},0.0,"two-sided","Welch Two Sample t-test","x and y",1989.732411290855)
{% endhighlight %}

### Clustering

The [Clustering](https://github.com/johnmyleswhite/Clustering.jl) package provides tools for doing simple k-means style clustering.

{% highlight julia %}
julia> load("Clustering")
 
julia> using Clustering
 
julia> srand(1)
 
julia> n = 100
100
 
julia> x = vcat(randn(n, 2), randn(n, 2) .+ 10)
200x2 Float64 Array:
  0.0575636  -0.112322 
 -1.8329     -0.101326 
  0.370699   -0.956183 
  1.31816    -1.44351  
  0.787598    0.148386 
  0.712214   -1.293    
 -1.8578     -1.06208  
 -0.746303   -0.0439182
  1.12082    -2.00616  
  0.364646   -1.09331  
  ⋮                    
 10.1974     10.5583   
 11.0832      8.92082  
 11.5414     11.6022   
  9.0453     11.5093   
  8.86714    10.4233   
 10.7336     10.7201   
  8.60415     9.13942  
  8.62482     8.51701  
 10.5044     10.3841   
 
julia> true_assignments = vcat(zeros(n), ones(n))
200-element Float64 Array:
 0.0
 0.0
 0.0
 0.0
 0.0
 0.0
 0.0
 0.0
 0.0
 0.0
 ⋮  
 1.0
 1.0
 1.0
 1.0
 1.0
 1.0
 1.0
 1.0
 1.0
 
julia> results = k_means(x, 2)
Warning: Possible conflict in library symbol dgesdd_
Warning: Possible conflict in library symbol dsyrk_
Warning: Possible conflict in library symbol dgemm_
KMeansOutput([1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1  ...  2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],2x2 Float64 Array:
 -0.0166203  -0.248904
 10.0418     10.0074  ,3,422.9820560670007,true)
 
julia> results.assignments
200-element Int64 Array:
 1
 1
 1
 1
 1
 1
 1
 1
 1
 1
 ⋮
 2
 2
 2
 2
 2
 2
 2
 2
 2

{% endhighlight %}

While all of this software is still quite new and often still buggy, being able to work with these tools through a simple package systems had made me more excited than ever before about the future of Julia as a language for data analysis. There is, of course, one thing conspicuously lacking right now: a really powerful visualization toolkit for interactive graphics like that provided by R's ggplot2 package. Hopefully something will come into being within the next few months.
