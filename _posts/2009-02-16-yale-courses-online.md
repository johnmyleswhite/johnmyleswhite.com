---
title: Yale Courses Online
author: John Myles White
layout: post
permalink: /notebook/2009/02/16/yale-courses-online/
categories:
  - Programming
---

Yale has updated [its impressive set of videotaped lectures](http://oyc.yale.edu/). For those interested in automating downloading the videos for any course, the script below should be useful. You'll need to install the Perl module WWW::Mechanize before you can run the script. You'll also want to update the list of courses URLs to reflect the courses that you want to download.

{% highlight perl %}
#!/usr/local/bin/perl

use strict;
use warnings;

use WWW::Mechanize;
use File::Spec;

my $mech = WWW::Mechanize->new();

my @courses = (
    'http://oyc.yale.edu/astronomy/frontiers-and-controversies-in-astrophysics/content/downloads',
    'http://oyc.yale.edu/economics/financial-markets/content/downloads'
);

for my $course (@courses)
{
    $mech->get($course);

    for my $link ($mech->find_all_links)
    {
        if ($link->url =~ m/mov$/ and $link->text =~ m/high/i)
        {
            print $link->url;
            print $link->text;
            print "\n\n";

            my (undef, undef, $filename) = File::Spec->splitpath($link->url);

            print "$filename\n";

            $mech->get( $link->url, ':content_file' => $filename );
        }
    }
}
{% endhighlight %}
