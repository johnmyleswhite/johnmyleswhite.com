---
title: "Comparing Julia and R's Vocabularies"
author: John Myles White
layout: post
permalink: /notebook/2012/04/09/comparing-julia-and-rs-vocabularies/
categories:
  - Programming
  - Statistics
---

While exploring the [Julia manual](http://julialang.org/manual) recently, I realized that it might be helpful to put the basic vocabularies of Julia and R side-by-side for easy comparison. So I took [Hadley Wickham's](http://had.co.nz/) [R Vocabulary](https://github.com/hadley/devtools/wiki/vocabulary) section from the book he's putting together on the [devtools wiki](https://github.com/hadley/devtools/wiki), put all of the functions Hadley listed into a CSV file, and proceeded to fill in entries where I knew of an obvious Julia equivalent to an R function.

The results are on [GitHub](https://github.com/johnmyleswhite/JuliaVsR/blob/master/vocab.csv) and, as they stand today, are shown below:

<table summary="Julia and R's Vocabularies" class="table table-hover">
  <tr>
    <th>
      R
    </th>
    <th>
      Julia
    </th>
    <th>
      Category
    </th>
    <th>
      Subcategory
    </th>
  </tr>
  <tr>
    <td>
      <p>
        https://github.com/hadley/devtools/wiki/vocabulary
      </p>
    </td>
    <td>
      <p>
        http://julialang.org/manual/standard-library-reference/
      </p>
    </td>
    <td>
      Resources
    </td>
    <td>
      Vocabulary
    </td>
  </tr>
  <tr>
    <td>
      ?
    </td>
    <td>
      help
    </td>
    <td>
      Basics
    </td>
    <td>
      First Functions
    </td>
  </tr>
  <tr>
    <td>
      str
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      First Functions
    </td>
  </tr>
  <tr>
    <td>
      %in%
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      match
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      =
    </td>
    <td>
      =
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      <-
    </td>
    <td>
      =
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      <<-
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      assign
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      $
    </td>
    <td>
      []
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      []
    </td>
    <td>
      []
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      [[]]
    </td>
    <td>
      []
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      replace
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      head
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      tail
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      subset
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      with
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      within
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Operators
    </td>
  </tr>
  <tr>
    <td>
      all.equal
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      identical
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      !=
    </td>
    <td>
      !=
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      ==
    </td>
    <td>
      ==
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      >
    </td>
    <td>
      >
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      >=
    </td>
    <td>
      >=
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      <
    </td>
    <td>
      <
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      <=
    </td>
    <td>
      <=
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      is.na
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  <tr>
    <td>
      is.nan
    </td>
    <td>
    </td>
    <td>
      Basics
    </td>
    <td>
      Comparison
    </td>
  </tr>
  
  <tr>
    <td>
      is.finite
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Comparison
    </td>
  </tr>
  
  <tr>
    <td>
      complete.cases
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Comparison
    </td>
  </tr>
  
  <tr>
    <td>
      *
    </td>
    
    <td>
      *
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      +
    </td>
    
    <td>
      +
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      -
    </td>
    
    <td>
      -
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      /
    </td>
    
    <td>
      /
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      ^
    </td>
    
    <td>
      ^
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      %%
    </td>
    
    <td>
      mod (%%)
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      %/%
    </td>
    
    <td>
      div
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      abs
    </td>
    
    <td>
      abs
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sign
    </td>
    
    <td>
      sign
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      acos
    </td>
    
    <td>
      acos
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      acosh
    </td>
    
    <td>
      acosh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      asin
    </td>
    
    <td>
      asin
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      asinh
    </td>
    
    <td>
      asinh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      atan
    </td>
    
    <td>
      atan
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      atan2
    </td>
    
    <td>
      atan2
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      atanh
    </td>
    
    <td>
      atanh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sin
    </td>
    
    <td>
      sin
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sinh
    </td>
    
    <td>
      sinh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cos
    </td>
    
    <td>
      cos
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cosh
    </td>
    
    <td>
      cosh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      tan
    </td>
    
    <td>
      tan
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      tanh
    </td>
    
    <td>
      tanh
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      ceiling
    </td>
    
    <td>
      ceil
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      floor
    </td>
    
    <td>
      floor
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      round
    </td>
    
    <td>
      round
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      trunc
    </td>
    
    <td>
      trunc
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      signif
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      exp
    </td>
    
    <td>
      exp
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      log
    </td>
    
    <td>
      log
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      log10
    </td>
    
    <td>
      log10
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      log1p
    </td>
    
    <td>
      log1p
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      log2
    </td>
    
    <td>
      log2
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      logb
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sqrt
    </td>
    
    <td>
      sqrt
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cummax
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cummin
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cumprod
    </td>
    
    <td>
      cumprod
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cumsum
    </td>
    
    <td>
      cumsum
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      diff
    </td>
    
    <td>
      diff
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      max
    </td>
    
    <td>
      max
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      min
    </td>
    
    <td>
      min
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      prod
    </td>
    
    <td>
      prod
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sum
    </td>
    
    <td>
      sum
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      range
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      mean
    </td>
    
    <td>
      mean
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      median
    </td>
    
    <td>
      median
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cor
    </td>
    
    <td>
      cor_pearson
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      cov
    </td>
    
    <td>
      cov_pearson
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      sd
    </td>
    
    <td>
      std
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      var
    </td>
    
    <td>
      var
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      pmax
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      pmin
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      rle
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Basic Math
    </td>
  </tr>
  
  <tr>
    <td>
      function
    </td>
    
    <td>
      function
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Functions
    </td>
  </tr>
  
  <tr>
    <td>
      missing
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Functions
    </td>
  </tr>
  
  <tr>
    <td>
      on.exit
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Functions
    </td>
  </tr>
  
  <tr>
    <td>
      return
    </td>
    
    <td>
      return
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Functions
    </td>
  </tr>
  
  <tr>
    <td>
      invisible
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Functions
    </td>
  </tr>
  
  <tr>
    <td>
      &
    </td>
    
    <td>
      &
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      |
    </td>
    
    <td>
      |
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      !
    </td>
    
    <td>
      !
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      xor
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      all
    </td>
    
    <td>
      all
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      any
    </td>
    
    <td>
      any
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      intersect
    </td>
    
    <td>
      intersect
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      union
    </td>
    
    <td>
      union
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      setdiff
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      setequal
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      which
    </td>
    
    <td>
      find
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Logical & Set Operations
    </td>
  </tr>
  
  <tr>
    <td>
      c
    </td>
    
    <td>
      [] ({})
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      matrix
    </td>
    
    <td>
      [] ({})
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      length
    </td>
    
    <td>
      size (length)
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      dim
    </td>
    
    <td>
      size
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      ncol
    </td>
    
    <td>
      size(x, 1)
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      nrow
    </td>
    
    <td>
      size(x, 2)
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      cbind
    </td>
    
    <td>
      hcat
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      rbind
    </td>
    
    <td>
      vcat
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      names
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      colnames
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      rownames
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      t
    </td>
    
    <td>
      '
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      diag
    </td>
    
    <td>
      eye
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      sweep
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      as.matrix
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      data.matrix
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Vectors and Matrices
    </td>
  </tr>
  
  <tr>
    <td>
      c
    </td>
    
    <td>
      [] ({})
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      rep
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      seq
    </td>
    
    <td>
      [from:by:to]
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      seq_along
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      seq_len
    </td>
    
    <td>
      [1:len]
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      rev
    </td>
    
    <td>
      reverse
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      sample
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      choose
    </td>
    
    <td>
      factorial
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      factorial
    </td>
    
    <td>
      factorial
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      combn
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      (is/as).(character/numeric/logical)
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Making Vectors
    </td>
  </tr>
  
  <tr>
    <td>
      list
    </td>
    
    <td>
      HashTable ([])
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      unlist
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      data.frame
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      as.data.frame
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      split
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      expand.grid
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Lists & Data Frames
    </td>
  </tr>
  
  <tr>
    <td>
      if
    </td>
    
    <td>
      if
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      &&
    </td>
    
    <td>
      &&
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      ||
    </td>
    
    <td>
      ||
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      for
    </td>
    
    <td>
      for
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      while
    </td>
    
    <td>
      while
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      next
    </td>
    
    <td>
      continue
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      break
    </td>
    
    <td>
      break
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      switch
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      ifelse
    </td>
    
    <td>
    </td>
    
    <td>
      Basics
    </td>
    
    <td>
      Control Flow
    </td>
  </tr>
  
  <tr>
    <td>
      fitted
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      predict
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      resid
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      rstandard
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      lm
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      glm
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      hat
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      influence.measures
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      logLik
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      df
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      deviance
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      formula
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      ~
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      I
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      anova
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      coef
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      confint
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      vcov
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      contrasts
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Linear Models
    </td>
  </tr>
  
  <tr>
    <td>
      apropos('\\.test$')
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Miscellaneous Statistical Tests
    </td>
  </tr>
  
  <tr>
    <td>
      beta
    </td>
    
    <td>
      beta
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      binom
    </td>
    
    <td>
      binom
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      cauchy
    </td>
    
    <td>
      cauchy
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      chisq
    </td>
    
    <td>
      chisq
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      exp
    </td>
    
    <td>
      exp
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      f
    </td>
    
    <td>
      f
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      gamma
    </td>
    
    <td>
      gamma
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      geom
    </td>
    
    <td>
      geom
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      hyper
    </td>
    
    <td>
      hyper
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      lnorm
    </td>
    
    <td>
      lnorm
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      logis
    </td>
    
    <td>
      logis
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      multinom
    </td>
    
    <td>
      multinom
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      nbinom
    </td>
    
    <td>
      nbinom
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      norm
    </td>
    
    <td>
      norm
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      pois
    </td>
    
    <td>
      pois
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      signrank
    </td>
    
    <td>
      signrank
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      t
    </td>
    
    <td>
      t
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      unif
    </td>
    
    <td>
      unif (rand)
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      weibull
    </td>
    
    <td>
      weibull
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      wilcox
    </td>
    
    <td>
      wilcox
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      birthday
    </td>
    
    <td>
      birthday
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      tukey
    </td>
    
    <td>
      tukey
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Random Numbers
    </td>
  </tr>
  
  <tr>
    <td>
      crossprod
    </td>
    
    <td>
      *
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      tcrossprod
    </td>
    
    <td>
      *
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      eigen
    </td>
    
    <td>
      eig
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      qr
    </td>
    
    <td>
      qr
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      svd
    </td>
    
    <td>
      svd
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      %*%
    </td>
    
    <td>
      *
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      %o%
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      outer
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      rcond
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      solve
    </td>
    
    <td>
      \
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Matrix Algebra
    </td>
  </tr>
  
  <tr>
    <td>
      duplicated
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      unique
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      merge
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      order
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      rank
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      quantile
    </td>
    
    <td>
      quantile
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      sort
    </td>
    
    <td>
      sort
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      table
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      ftable
    </td>
    
    <td>
    </td>
    
    <td>
      Statistics
    </td>
    
    <td>
      Ordering and Tabulating
    </td>
  </tr>
  
  <tr>
    <td>
      ls
    </td>
    
    <td>
      whos
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      exists
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      get
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      rm
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      getwd
    </td>
    
    <td>
      getcwd
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      setwd
    </td>
    
    <td>
      setcwd
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      q
    </td>
    
    <td>
      Ctrl-D
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      source
    </td>
    
    <td>
      load
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      install.packages
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      library
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      require
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Workspace
    </td>
  </tr>
  
  <tr>
    <td>
      help
    </td>
    
    <td>
      help
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      ?
    </td>
    
    <td>
      help
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      help.search
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      apropos
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      RSiteSearch
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      citation
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      demo
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      example
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      vignette
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Help
    </td>
  </tr>
  
  <tr>
    <td>
      traceback
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      browser
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      recover
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      options(error =)
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      stop
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      warning
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      message
    </td>
    
    <td>
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      tryCatch
    </td>
    
    <td>
      try/catch
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      try
    </td>
    
    <td>
      try
    </td>
    
    <td>
      Working with R
    </td>
    
    <td>
      Debugging
    </td>
  </tr>
  
  <tr>
    <td>
      print
    </td>
    
    <td>
      print (println)
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      cat
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      message
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      warning
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      dput
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      format
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      sink
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Output
    </td>
  </tr>
  
  <tr>
    <td>
      data
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      count.fields
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      read.csv
    </td>
    
    <td>
      csvread
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      read.delim
    </td>
    
    <td>
      dlmread
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      read.fwf
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      read.table
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      library(foreign)
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      write.table
    </td>
    
    <td>
      dlmwrite
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      readLines
    </td>
    
    <td>
      readlines
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      writeLines
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      load
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      save
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      readRDS
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      saveRDS
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Reading and Writing Data
    </td>
  </tr>
  
  <tr>
    <td>
      dir
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      basename
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      dirname
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.path
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      path.expand
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.choose
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.copy
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.create
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.remove
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      path.rename
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      dir.create
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      file.exists
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      tempdir
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      tempfile
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      download.file
    </td>
    
    <td>
    </td>
    
    <td>
      I/O
    </td>
    
    <td>
      Files and Directories
    </td>
  </tr>
  
  <tr>
    <td>
      ISOdate
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      ISOdatetime
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      strftime
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      strptime
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      date
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      difftime
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      julian
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      months
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      quarters
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      weekdays
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      library(lubridate)
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Date / Time
    </td>
  </tr>
  
  <tr>
    <td>
      grep
    </td>
    
    <td>
      match
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      agrep
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      gsub
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      strsplit
    </td>
    
    <td>
      split
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      chartr
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      nchar
    </td>
    
    <td>
      strlen
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      tolower
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      toupper
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      substr
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      paste
    </td>
    
    <td>
      join
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      library(stringr)
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Character Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      factor
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      levels
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      nlevels
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      reorder
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      relevel
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      cut
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      findInterval
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      interaction
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      options(stringsAsFactors = FALSE)
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Factors
    </td>
  </tr>
  
  <tr>
    <td>
      array
    </td>
    
    <td>
      []
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Array Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      dim
    </td>
    
    <td>
      size
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Array Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      dimnames
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    
    <td>
      Array Manipulation
    </td>
  </tr>
  
  <tr>
    <td>
      aperm
    </td>
    
    <td>
    </td>
    
    <td>
      Special Data
    </td>
    <td>
      Array Manipulation
    </td>
  </tr>
  <tr>
    <td>
      library(abind)
    </td>
    <td>
    </td>
    <td>
      Special Data
    </td>
    <td>
      Array Manipulation
    </td>
  </tr>
</table>

I'd like to note that holes in the list of Julia functions can exist for several reasons:

* The language does not yet have the relevant features. This is true of things like `factor()` or `data.frame()`.
* The language has draft implementations of the relevant features, but they are not yet ready to make their way into this list. This is true of Doug Bates' GLM code, for example.
* I simply don't know what the Julia equivalent is for an R function, but it may well exist. If you know of one, please fork the GitHub repository I'm using and revise the CSV file appropriately. I'll integrate relevant pull requests as soon as I can find time.

In addition to explaining the presence of the many holes you can see this in this list, I'd also like to note how quickly these holes are being filled in: Doug Bates already finished a wrapper for the Rmath library, which means that Julia now has tools for calculating the PDF's, CDF's, and inverse CDF's of most statistical distributions as well as the ability to draw random samples from them. That means that almost any sort of MCMC you'd like to do is already possible in Julia. (I, for one, am really interested to see if someone will use Julia's sparse matrix support and these new Rmath functions to build MCMC code that's easy on the eyes while also running at an appropriately fast speed on complicated, big data problems like matrix factorizations.)

On my end, I've been working on filling some of the missing entries in this list by adding in pieces that I think I understand well enough to implement from scratch, such as:

* Optimization algorithms ([optim.jl](https://github.com/johnmyleswhite/optim.jl)):
    * Simulated annealing
    * Gradient descent
    * Newton's method

* Statistical hypothesis tests ([stats.jl](https://github.com/johnmyleswhite/stats.jl)):
    * t-Tests

* Utility functions ([utils.jl](https://github.com/johnmyleswhite/utils.jl)):
    * range
    * keys
    * cummax
    * cummin
