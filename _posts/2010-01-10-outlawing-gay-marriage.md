---
title: Outlawing Gay Marriage
author: John Myles White
layout: post
permalink: /notebook/2010/01/10/outlawing-gay-marriage/
categories:
  - Statistics
---

Given the recent votes on same-sex marriage in New Jersey and Portugal, I wanted to test a seemingly innocuous claim that touches upon very broad issues in political theory: does the degree of directness of a "democratic" vote predict whether the vote will promote or prohibit same-sex marriage? Naively, it seemed clear to me that this was true: every single direct vote in my memory has outlawed same-sex marriage, while the only decisions that have allowed same-sex marriage have originated among elected representatives or unelected judges.

I decided to compile some rough data to test this idea using Wikipedia's articles on the topic. I took what seemed to be the non-redundant decisions since the 1990's from the following three articles:

* [Same-Sex Marriage in the Netherlands](http://en.wikipedia.org/wiki/Same-sex_marriage_in_the_Netherlands)
* [Same-Sex Marriage in Spain](http://en.wikipedia.org/wiki/Same-sex_marriage_in_Spain)
* [Same-Sex Marriage in the United States](http://en.wikipedia.org/wiki/Same-sex_marriage_in_the_United_States)

And I arrived at this table:  

<table summary="Gay Marriage Statistics" class="table table-hover">
    <tr>
      <th>
        Region
      </th>
      <th>
        Date
      </th>
      <th>
        Type of Decision
      </th>
      
      <th>
        Promoted
      </th>
    </tr>
    <tr>
      <td>
        Hawaii
      </td>
      <td>
        1993-05-05
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        U.S.A.
      </td>
      <td>
        1996-09-21
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        Hawaii
      </td>
      <td>
        1998-11-03
      </td>
      <td>
        Direct Vote
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        Vermont
      </td>
      <td>
        1999-12-20
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Holland
      </td>
      <td>
        2000-12-19
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Massachusetts
      </td>
      <td>
        2003-11-18
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Spain
      </td>
      <td>
        2005-06-30
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        California
      </td>
      <td>
        2008-05-15
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Connecticut
      </td>
      <td>
        2008-10-10
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        California
      </td>
      <td>
        2008-11-04
      </td>
      <td>
        Direct Vote
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        New Hampshire
      </td>
      <td>
        2009-03-26
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Iowa
      </td>
      <td>
        2009-04-03
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Vermont
      </td>
      <td>
        2009-04-07
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Connecticut
      </td>
      <td>
        2009-04-23
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Maine
      </td>
      <td>
        2009-05-06
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        New York
      </td>
      <td>
        2009-05-12
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Texas
      </td>
      <td>
        2009-10-02
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Maine
      </td>
      <td>
        2009-11-03
      </td>
      <td>
        Direct Vote
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        Washington, D.C.
      </td>
      <td>
        2009-12-01
      </td>
      <td>
        Court Ruling
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        New York
      </td>
      <td>
        2009-12-02
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        New Jersey
      </td>
      <td>
        2010-01-07
      </td>
      <td>
        Congressional Decision
      </td>
      <td>
      </td>
    </tr>
</table>
  
You can find a CSV file with this data at [the GitHub repository](http://github.com/johnmyleswhite/gay_marriage) where I've stored all of the analyses I've completed so far. I'd love for people to add more data or contribute some visualizations of the patterns I'm pointing out. And, as always, I'd love to hear criticisms or suggestions about my approach.
  
The summary statistics provide pretty clear support for my intuition, though they're rarely statistically significant, given the small sample sizes:
  
* 100% of court rulings promoted same-sex marriage.
* 80% of congressional decisions promoted same-sex marriage.
* 0% of direct votes promoted same-sex marriage.
  
For me the conclusion to be drawn from all of this is not that our respect for the will of the majority should compel us to prohibit same-sex marriage, but rather that we should be more hostile to direct democracy, because it is a powerful force for promoting intolerance in American society. I imagine this will sound un-American to many readers, but I think it's quite clear that this sentiment was foundational for the American experiment in government. You see this distrust of direct democracy repeatedly reiterated in the Federalist Papers, especially in [Federalist Number 10](http://en.wikipedia.org/wiki/Federalist_No._10). Indeed, one can argue that the major purpose of our Constitution is to limit the ability of direct democracy to harm minorities within our society.
