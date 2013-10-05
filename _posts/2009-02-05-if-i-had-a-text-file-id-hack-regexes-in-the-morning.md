---
title: "If I Had a Text File, I'd Hack Regexes in the Morning"
author: John Myles White
layout: post
permalink: /notebook/2009/02/05/if-i-had-a-text-file-id-hack-regexes-in-the-morning/
categories:
  - Statistics
---

Yesterday the topic of academic citation counts came up, so I decided that I should write up some tools for exploring cite counts. The first thing I did was to build a cheap screenscraper in Ruby for pulling citation count information from Google scholar. You'll see the ugly hack I produced below.

{% highlight ruby %}
module CitationTools
  require 'rubygems'
  require 'open-uri'

  def get_ten_most_cited_works_for_author(author_name)
    # First, let's clean up the author's name before using it in a URL.
    escaped_author_name = author_name.gsub(/\s+/, '+')

    # Let's create a variable we'll place the Google Scholar HTML in.
    page_content = nil

    # Let's figure out the right URL for Google Scholar.
    url = "http://scholar.google.com/scholar?q=#{escaped_author_name}"
    
    # Let's access that URL using open-uri and get the HTML from the page.
    open(url) do |page|
      page_content = page.read()
    end

    # Let's scan the HTML for the names of this author's works.
    work_titles = page_content.scan(/<p class=g>.*?>([^<]+)(?:<\/a><\/span>)?(?:(?:<font size=-1>)|(?:\s+-\s+<span class=a>)|(?:\s+-\s+<a class=fl))/)

    # Let's scan the HTML for the citation counts for each work.
    cite_counts = page_content.scan(/Cited by (\d+)/)

    # Let's set aside an array of hashes to store all of this data.
    works = []

    # As long as we have the same number of titles and counts, we're good.
    if work_titles.size == cite_counts.size
      work_titles.each_with_index do |title, index|
        works << {:title => title, :citation_count => cite_counts[index]}
      end
      return works
    else
      puts "Failed to process HTML for #{author_name}"
      return nil
    end

  end
end
{% endhighlight %}

With that in hand, I wrote a simple wrapper to pull information for a list of authors you store in a file called `authors.txt` from Google Scholar. The wrapper then prints a CSV file to STDOUT that can be redirected to a file for later analysis.

{% highlight ruby %}
# Let's include a mix-in with some methods for parsing Google scholar data.
require 'CitationTools'
include CitationTools

# Let's pick a haphazard sample of authors.
authors = File.new('authors.txt', 'r').readlines.map {|line| line.chomp}

# Let's add a header line to our output.
puts '"Author","Work","Citations"'

# And then let's iterate over those authors.
authors.each do |author_name|
  cited_work_data = get_ten_most_cited_works_for_author(author_name)

  if cited_work_data.nil?
    print "Skipping #{author_name}"
  end

  cited_work_data.each do |cited_work|
    puts "\"#{author_name}\",\"#{cited_work[:title]}\",#{cited_work[:citation_count]}"
  end
end
{% endhighlight %}

Then I coded up a simple barplot in R to give you a sense of the citation count for the first few authors that came to mind. The result is below.

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2009/02/citation-values.png" alt="citation_values.png" />
</center>

Now I think the goal should be to put these tools to a good use.
