---
title: Blegging for Data
author: John Myles White
layout: post
permalink: /notebook/2010/08/28/blegging-for-data/
categories:
  - Statistics
---

I'm in the middle of a new project that involves analyzing the packages that are currently on CRAN. As part of my work, I could really benefit from information about which packages are installed on people's computers. If you're willing to part with a bit of your time and privacy, I'd very much appreciate you running the following script in R,

{% highlight r %}
package.info <- installed.packages()[,c(1,3)]
write.csv(package.info,
          file = 'my_installed_packages.csv',
          row.names = FALSE)
{% endhighlight %}

and sending me the output file `my_installed_packages.csv` by e-mail to `jmw@johnmyleswhite.com`.
