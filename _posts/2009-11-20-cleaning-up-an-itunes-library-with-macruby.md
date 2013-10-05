---
title: Cleaning Up an iTunes Library with MacRuby
author: John Myles White
layout: post
permalink: /notebook/2009/11/20/cleaning-up-an-itunes-library-with-macruby/
categories:
  - Mac OS X
  - Programming
  - Ruby
---

For a little more than a year now, I've been meaning to write a script to rename all of the files in my iTunes library so that they're in proper English title case. In large part, this project was inspired by reading [John Gruber's post](http://daringfireball.net/2008/05/title_case) about a Perl script that he'd written to convert text strings into title case programmatically. After reading Gruber's post, I grabbed a copy of [Sam Souder's translation](http://github.com/samsouder/titlecase/tree/master/lib/) of Gruber's Title Case script into Ruby and set about trying to use it as part of a home-grown Ruby script to clean up my iTunes library. During my first pass, I tried to combine Sam's convenient utility method with RubyCocoa to produce a script that could access the iTunes methods directly and rename my files automatically without editing the MP3 files directly. Unfortunately, the RubyCocoa documentation was too sparse at the time for me to figure out the relevant method calls to be making.

Thankfully, I spent some time this week reading the MacRuby documentation and realized in the process how to write my desired script. The results are now on [GitHub](http://github.com/johnmyleswhite/titlecase-itunes-tracks). The only original bit of code is below:

{% highlight ruby %}
#!/usr/bin/macruby
 
# We iterate over every track, stripping bounding whitespace and putting things into proper title case.
 
require 'titlecase'
 
framework 'cocoa'
 
load_bridge_support_file 'ITunes.bridgesupport'
 
framework("ScriptingBridge")
 
itunes = SBApplication.applicationWithBundleIdentifier("com.apple.itunes")
 
music_playlist_tracks = itunes.sources.objectWithName("Library").userPlaylists.objectWithName("Music").fileTracks
 
music_playlist_tracks.each do |track|
  old_artist = track.artist
  new_artist = track.artist.strip.titlecase
  if old_artist != new_artist
    track.artist = new_artist
    puts "Artist: #{old_artist} => #{new_artist}"
  end
  
  old_album = track.album
  new_album = track.album.strip.titlecase
  if old_album != new_album
    track.album = new_album
    puts "Album: #{old_album} => #{new_album}"
  end
  
  old_name = track.name
  new_name = track.name.strip.titlecase
  if old_name != new_name
    track.name = new_name
    puts "Name: #{old_name} => #{new_name}"
  end
end
{% endhighlight %}

Outside of this little snippet of MacRuby code that I had to write, I made two small changes to Sam Souder's original code:

* I defined a `titlecase` method for the `NSString` class rather than the `String` class.
* I pulled the list of English prepositions out of the main code and put it into a separate YAML file to make it easier for a non-programmer to edit the list.

I know from experience with checking the results in a half gleeful and half paranoid frenzy that this code works properly on my own two machines, both of which are running MacRuby 0.5. That said, I won't vouch for the reliability of this program in any way. If you notice any bugs, please do let me know, so that I can fix my own iTunes library with a revised version of the script.
