---
title: A Request for Foursquare Data
author: John Myles White
layout: post
permalink: /notebook/2011/03/25/a-request-for-foursquare-data/
categories:
  - Programming
  - Statistics
---

[**UPDATE 3/28/2011: Fixed an enormous bug in the R code.**]

I'm trying to collect data sets that showcase how the classical statistical distributions appear in modern contexts. I've [already got some data](http://www.johnmyleswhite.com/notebook/2011/03/16/canabalt-revisited-gamma-distributions-multinomial-distributions-and-more-jags-goodness/) that shows how the [gamma distribution](http://en.wikipedia.org/wiki/Gamma_distribution) appears in video game scores, and now I'm hoping to find an example where the [exponential distribution](http://en.wikipedia.org/wiki/Exponential_distribution) shows up. I think that checkins for Foursquare might be a good place to start.

To test this intuition, I'm hoping to collect some pilot data. Below you'll find some code that you can use to help me gather data.

First, there's a shell script to gather your own checkin data from FourSquare. To use this script, you need to substitute your e-mail address where EMAIL appears and your password where PASSWORD appears in the code below:

{% highlight bash %}
curl -u 'EMAIL:PASSWORD' https://api.foursquare.com/v1/history?l=250 > checkin_history.xml
{% endhighlight %}

And second there's an R script you can use to preprocess the data from the last step into a nice format before sending it to me. If you're not an R user, you can easily skip this step and send the data you have in its raw XML format.

{% highlight r %}
library('plyr')
library('XML')
filename <- 'checkin_history.xml'
tree <- xmlTreeParse(filename, asTree = TRUE)
checkins <- tree$doc$children$checkins
venue.names <- c()
latitudes <- c()
longitudes <- c()
for (i in 1:length(checkins))
{
  venue.names <- c(venue.names, as.character(checkins[i]$checkin[['venue']][['name']][['text']])[6])
  latitudes <- c(latitudes, as.numeric(unclass(checkins[i]$checkin[['venue']][['geolat']][['text']])$value))
  longitudes <- c(longitudes, as.numeric(unclass(checkins[i]$checkin[['venue']][['geolong']][['text']])$value))
}
checkin.data <- data.frame(Venue = factor(venue.names), Latitude = as.numeric(latitudes), Longitude = as.numeric(longitudes))
count.data <- ddply(checkin.data, 'Venue', nrow)
names(count.data) <- c('Venue', 'TotalCheckins')
write.csv(count.data, file = 'count_data.csv', row.names = FALSE)
{% endhighlight %}

After running these two pieces of code, the output file, `count_data.csv`, should look like this:

| Venue               | TotalCheckins |
| ------------------- | ------------- |
| "Brooklyn Boulders" | 13            |
| ...                 | ...           |

Once you've got data, you can send it to me by e-mail at `jmw@johnmyleswhite.com`.
