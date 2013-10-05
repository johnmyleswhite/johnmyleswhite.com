---
title: The Social Dynamics of the R Core Team
author: John Myles White
layout: post
permalink: /notebook/2012/08/12/the-social-dynamics-of-the-r-core-team/
categories:
  - Psychology
  - Statistics
---

Recently a few members of R Core have indicated that part of what slows down the development of R as a language is that it has become increasingly difficult over the years to achieve consensus among the core developers of the language. Inspired by these claims, I decided to look into this issue quantitatively by measuring the quantity of commits to R's SVN repository that were made by each of the R Core developers. I wanted to know whether a small group of developers were overwhelmingly responsible for changes to R or whether all of the members of R Core had contributed equally. To follow along with what I did, you can grab the data and analysis scripts from [GitHub](https://github.com/johnmyleswhite/r_social_dynamics).

First, I downloaded the R Core team's SVN logs from <http://developer.r-project.org/>. I then used a simple regex to parse the SVN logs to count commits coming from each core committer.

After that, I tabulated the number of commits from each developer, pooling across the years 2003-2012 for which I had logs. You can see the results below, sorted by total commits in decreasing order:

<table class="table table-hover">
	<thead>
		<tr>
			<th>Committer</th>
			<th>Total Number of Commits</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>ripley</td>
			<td>22730</td>
		</tr>
		<tr>
			<td>maechler</td>
			<td>3605</td>
		</tr>
		<tr>
			<td>hornik</td>
			<td>3602</td>
		</tr>
		<tr>
			<td>murdoch</td>
			<td>1978</td>
		</tr>
		<tr>
			<td>pd</td>
			<td>1781</td>
		</tr>
		<tr>
			<td>apache</td>
			<td>658</td>
		</tr>
		<tr>
			<td>jmc</td>
			<td>599</td>
		</tr>
		<tr>
			<td>luke</td>
			<td>576</td>
		</tr>
		<tr>
			<td>urbaneks</td>
			<td>414</td>
		</tr>
		<tr>
			<td>iacus</td>
			<td>382</td>
		</tr>
		<tr>
			<td>murrell</td>
			<td>324</td>
		</tr>
		<tr>
			<td>leisch</td>
			<td>274</td>
		</tr>
		<tr>
			<td>tlumley</td>
			<td>153</td>
		</tr>
		<tr>
			<td>rgentlem</td>
			<td>141</td>
		</tr>
		<tr>
			<td>root</td>
			<td>87</td>
		</tr>
		<tr>
			<td>duncan</td>
			<td>81</td>
		</tr>
		<tr>
			<td>bates</td>
			<td>76</td>
		</tr>
		<tr>
			<td>falcon</td>
			<td>45</td>
		</tr>
		<tr>
			<td>deepayan</td>
			<td>40</td>
		</tr>
		<tr>
			<td>plummer</td>
			<td>28</td>
		</tr>
		<tr>
			<td>ligges</td>
			<td>24</td>
		</tr>
		<tr>
			<td>martyn</td>
			<td>20</td>
		</tr>
		<tr>
			<td>ihaka</td>
			<td>14</td>
		</tr>
	</tbody>
</table>

After that, I tried to visualize evolving trends over the years. First, I visualized the number of commits per developer per year:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2012/08/commits.png" alt="Commits" />
</center>
  
And then I visualized the evenness of contributions from different developers by measuring the entropy of the distribution of commits on a yearly basis:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2012/08/entropy1.png" alt="Entropy" />
</center>

There seems to be some weak evidence that the community is either finding consensus more difficult and tending towards a single leader who makes final decisions or that some developers are progressively dropping out because of the difficulty of achieving consensus. There is unambiguous evidence that a single developer makes the overwhelming majority of commits to R's SVN repo.

I leave it to others to understand what all of this means for R and for programming language communities in general.
