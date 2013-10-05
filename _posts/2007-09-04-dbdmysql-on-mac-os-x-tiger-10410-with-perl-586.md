---
title: "DBD::MySQL On Mac OS X Tiger 10.4.10 With Perl 5.8.6"
author: John Myles White
layout: post
permalink: /notebook/2007/09/04/dbdmysql-on-mac-os-x-tiger-10410-with-perl-586/
categories:
  - Perl
tags:
  - DBD::MySQL Mac OS X Tiger Perl
---

My attempts to work this weekend on updates to a MySQL database-driven CGI application I wrote a few years ago were all delayed by a recurring problem during the installation of the DBD::MySQL module on my Powerbook. I finally solved the problem a few minutes ago after reading several articles, particularly one from [JayAllen.org](http://www.jayallen.org). In the interests of saving others' time, here is the solution I came upon to getting DBD::MySQL to run on Mac OS X Tiger 10.4.10 with Perl 5.8.6.

The first set of problems I had were caused by the makefile for the CPAN module trying to access a non-existent library directory, /usr/local/mysql/lib/mysql. To solve this problem, I created a symbolic link to the missing directory:

{% highlight bash %}
sudo ln -s /usr/local/mysql/lib /usr/local/mysql/lib/mysql
{% endhighlight %}

After this, I got a new batch of errors about password problems during the test phase. The module was attempting to log in as root to the MySQL server without giving a password. The solution was to temporarily open the MySQL server with root requiring no password:

{% highlight bash %}
sudo mysqld --skip-grant-tables -u root &
{% endhighlight %}

These two steps, discovered after considerable bug-tracking, solved all of my problems. Hopefully this can save other Tiger users a great deal of time.
