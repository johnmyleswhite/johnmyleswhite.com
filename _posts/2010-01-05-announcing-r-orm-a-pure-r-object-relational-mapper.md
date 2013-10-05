---
title: "Announcing r-ORM: A Pure R Object-Relational Mapper"
author: John Myles White
layout: post
permalink: /notebook/2010/01/05/announcing-r-orm-a-pure-r-object-relational-mapper/
categories:
  - Statistics
---

My apologies for the long break between posts. Before the end of this week I'll return to my series of posts on image processing in R. In the intervening time, I've finished a piece of code that I'd like to officially release to the public.

The code in question is a very minimal object-relational mapper written entirely in R. As of today, you can find the code on GitHub [here](http://github.com/johnmyleswhite/r-ORM). If you're not familiar with using an ORM, I'd suggest reading a bit about how to use [ActiveRecord](http://www.oracle.com/technology/pub/articles/tate-activeerecord.html), the ORM I've tried to emulate.

As it stands, the code I have is able to connect to a MySQL database and extract information about a specified table using MySQL's `SHOW COLUMNS`. The code then automatically builds up R code that will map the rows of the table onto R objects. This code can subsequently be eval'd or cat'd to a file.

Let me note right off the bat that the code I've written is fairly heinous stylistically: because my understanding of R metaprogramming is rather limited, I've produced what amounts to the ugliest sort of code generation tool. If you check out the source in `orm.R`, you'll quickly see what I mean: the output R code is generated as a string using a long series of `paste()` operations, many of which embed `paste()` operations inside of themselves. In the future, I would like to rewrite the code in a clearer fashion, possibly constructing expressions and parse trees directly, rather than using an intermediate layer of code as string.

That said, I want to release the code so that I can get feedback on my approach. To help you give me feedback, here's a quick walkthrough of how'd you use the current code:

(1) Download the code from its GitHub [home][1]. Place the files you get into a directory of your choice. Let's assume you use `~/r-ORM`.

(2) Install the R YAML and MySQL packages if you don't already have them on your system: 

{% highlight r %}
install.packages('yaml')
install.packages('RMySQL')
{% endhighlight %}

(3) Create a test database in MySQL called `sample_database`:

{% highlight sql %}
CREATE DATABASE `sample_database`;
{% endhighlight %}

(4) Give permissions on this test database to `sample_user`:

{% highlight sql %}
GRANT ALL ON `sample_database`.* TO 'sample_user'@'localhost' IDENTIFIED BY 'sample_password';

FLUSH PRIVILEGES;
{% endhighlight %}

(5) Create a test table called `users` in `sample_database`:

{% highlight sql %}
USE `sample_database`;

CREATE TABLE `users` (
  `user_id` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) NOT NULL,
  `password` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`user_id`)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;
{% endhighlight %}

(6) Edit the `database.yml` file if you didn't use the database name, user name or password I just suggested. You'll also need to edit it if you're not working on `localhost`.

(7) Open up an R interpreter. Set your working directory and source `orm.R`:

{% highlight r %}
setwd('~/r-ORM')
source('orm.R')
{% endhighlight %}

(8) Build code for our user objects using `orm.build.model('user')`:

{% highlight r %}
code <- orm.build.model('user')
cat(code)
{% endhighlight %}

At this point, you can review the code that's been generated to see whether it should work on your system. If it will, you can eval it now:

{% highlight r %}
eval(parse(text = code))
{% endhighlight %}

With that done, you should have a working set of functions that handle creating, finding, manipulating and deleting R objects that are serialized to the database. To test out the resulting model, let's start by creating a user object. In general, an object of class `foo` will be created using an auto-generated function called `create.foo`:

{% highlight r %}
user <- create.user()
{% endhighlight %}

Calling this functions builds a user object in memory. Nothing is in the database so far. To see the object, type `user` at the command line:

{% highlight r %}
user
{% endhighlight %}

Now you can edit this user to make it a real piece of data. The columns of the database table are mapped onto object attributes with appropriate getter and setter methods, like so:

{% highlight r %}
name(user) <- 'test_user'
password(user) <- 'test_password'
{% endhighlight %}

Once the user object is worth keeping, we store it in the database using `store`:

{% highlight r %}
user <- store(user)
{% endhighlight %}

This stores the user object in the database. To get the ID's edited correctly, you have to perform the assignment as indicated above. In the future I may change this.

After storing something, you might want to retrieve it later. Since we know that we just created the first user object, we can get it again by using a `find.user` call:

{% highlight r %}
user <- find.user(1)
{% endhighlight %}

In general, you can find objects of class `foo` by calling `find.foo()`. If you provide an integer as an input, you'll get the object with that ID. If you provide the string `'all'`, you'll get a list containing all of the objects in your database. Other inputs produce an error.

You can see that we got the correct object using the getter methods:

{% highlight r %}
user.id(user) == 1
name(user) == 'test_user'
password(user) == 'test_password'
{% endhighlight %}

We can edit it again to see that updating rows of the database works as expected:

{% highlight r %}
name(user) <- 'new_user'
store(user)
{% endhighlight %}

Finally, now that we're done with it, we can delete it from the database:

{% highlight r %}
delete(user)
{% endhighlight %}

That, in a nutshell, is the use of this ORM solution. If you have any questions, please let me know: I'll be happy to answer them.

I should note that there are many weaknesses of functionality with the current code. Notably:

* If you want a 'foo' object, it must be in a 'foos' table. This is inspired by ActiveRecord's default pluralization rule.
* There's no type checking. You can try to set string values on numeric columns; you'll just get strange errors as a result.
* There's no checking whether you're inserting a NULL into a column that won't allow NULL's.
* The system might clobber an existing generic method or function if there's a naming conflict. I'd recommend writing everything to disk before using this code until you're familiar with the results.
* The default objects are completely blank: the database default values are not used to initialize any attibutes of a new object.
* The SQL generated is not sanitized. You should not use this system to process end-user input.

That said, I find the system usable for my current needs. I hope it will help you, and I really hope that you'll consider making suggestions for hoping to improve the code. Patches would be especially welcome.

I would also love feedback about the interface that's shown to the end user of this code. Here are some outstanding questions I'd like to hear from people about:

* Are function names like `orm.build.model()` memorable?
* Does the syntax of calls on the resulting objects seem sufficiently R-like?
* Would it be useful to break out my minimal database abstraction layer into a separate package? Is there already a package to do this that I should be using instead?
* How can I avoid the "code as string" approach I'm taking?
* Do you have ideas about the ideal user syntax for implementing relations across tables, i.e. `has_one` or `has_many` relationships?
* Should a caching mechanism be designed?
* Should the database queries be minimized using some sort of pooling?
* Should transactions be used?
