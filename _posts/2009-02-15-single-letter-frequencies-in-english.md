---
title: Single Letter Frequencies in English
author: John Myles White
layout: post
permalink: /notebook/2009/02/15/single-letter-frequencies-in-english/
categories:
  - Statistics
---

Every time that I read a paper that discusses the frequencies of single letters in English, I feel like I should sit down and calculate them for myself from a sample of English text. Today, I finally did. Here are the probabilities and negative log probabilities of the characters in English over the corpus of Shakespeare's plays:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2009/02/single-letter-probabilities.png" alt="Single Letter Probabilities.png" />
</center>

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2009/02/single-letter-inverse-probabilities.png" alt="Single Letter Inverse Probabilities.png" />
</center>

And, for those who care, here's the code to generate the data from the plays, which I downloaded from [Project Gutenberg](http://www.gutenberg.org/wiki/Main_Page):

{% highlight ruby %}
def initialize_letter_counts(letter_counts)
  ('a'..'z').each do |chr|
    letter_counts[chr] = 0
  end
end

def parse_file(filename, letter_counts)
  f = File.new(filename)
  begin
    while 1
      char = f.readchar().chr.downcase
      if char.match(/[a-z]/)
        letter_counts[char] = letter_counts[char] + 1
      end
    end
  rescue EOFError
    return nil
  end
end

directory = '/Users/johnmyleswhite/Princeton/Research/Letter Frequency'

Dir.chdir(directory)

letter_counts = {}

initialize_letter_counts(letter_counts)

Dir.new('Data').entries.each do |entry|
  if entry.match(/\.txt$/)
    entry = File.expand_path(entry, directory + '/Data')
    parse_file(entry, letter_counts)
  end
end

letter_counts.keys.sort.each do |key|
  puts "'#{key}',#{letter_counts[key]}"
end
{% endhighlight %}
