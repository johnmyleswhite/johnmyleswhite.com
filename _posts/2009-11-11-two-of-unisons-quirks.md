---
title: "Two of Unison's Quirks"
author: John Myles White
layout: post
permalink: /notebook/2009/11/11/two-of-unisons-quirks/
categories:
  - Mac OS X
  - Programming
---

As many people know, I adore the program [Unison](http://www.cis.upenn.edu/~bcpierce/unison/). That said, Unison has its fair share of quirks. Today I found myself confronting one that I had spent a whole day confused by about a month ago.

Unison stores a cache of information about the file system for every directory that it synchronizes. On Macs, this caches lives in `~/Library/Application Support/Unison/`. What you need to know is that this cache must either be absent on both the client and the server or it must be present on both. If it gets deleted on one but not the other, Unison hangs indefinitely without producing any error messages when you try to synchronize the directories again. Suffice it to say, this is confusing.

Possibly this quirk is already fixed in newer versions, but I've had trouble compiling Unison recently, so I've left that task for the future. In the meantime, the other quirk that irks me is the inconsistency in the icons used by the Mac GUI. Sometimes a top level element in the GUI represents a folder and sometimes it represents a single file inside of a folder. I think that a cleaner GUI would simply render a folder with nested files every single time you had a discrepancy between the client and server -- even if the discrepancy is a single file. Changing the meaning of items at the same spatial positioning seems to produce a Stroop effect for me -- and I suspect for others as well.
