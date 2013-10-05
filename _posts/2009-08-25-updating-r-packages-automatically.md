---
title: Updating R Packages Automatically
author: John Myles White
layout: post
permalink: /notebook/2009/08/25/updating-r-packages-automatically/
categories:
  - Programming
---

Here's a very naive program I just wrote to update all of the R packages I have on my system after I update the core R binary. Please let me know if there's anything obviously wrong with this, such as failing to update items with chains of dependencies.

{% highlight r %}
all.packages <- installed.packages()
r.version <- paste(version[['major']], '.', version[['minor']], sep = '')

for (i in 1:nrow(all.packages))
{
    package.name <- all.packages[i, 1]
    package.version <- all.packages[i, 3]
    if (package.version != r.version)
    {
        print(paste('Installing', package.name))
        install.packages(package.name)
    }
}
{% endhighlight %}
