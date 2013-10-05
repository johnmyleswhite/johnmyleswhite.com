---
title: Negative Items Left to Delete under Mac OS X Leopard
author: John Myles White
layout: post
permalink: /notebook/2008/08/30/negative-items-left-to-delete-under-mac-os-x-leopard/
categories:
  - Mac OS X
---

Until today I've felt that, at this point, Leopard has been patched enough to be relatively bug free. While transferring files across my home network today I changed my mind, because I found that the number of files left to delete that's shown in progress dialogues can become negative. You can see what I mean below:

<img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2008/08/2593-items.jpg" alt="-2593_Items.jpg" />

And just to make clear that the first number isn't a one-time fluke, here's another shot of the same progress dialogue a second later:

<img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2008/08/3697-items.jpg" alt="-3697_Items.jpg" />

I'm not entirely sure what caused the negative count: most likely it was the very large number of small files that had to be deleted. I'd bet that the number was originally underestimated when the delete operation began, and that, when more files were found later, this took the count of remaining files beyond 0 into the negative integers.

Personally, I find this a little worrisome.
