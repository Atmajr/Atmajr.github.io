---
layout: post
title: Building a scraper for new Comixology releases
categories: dev code ruby nokogiri scraping
tags:
- Ruby
- scraping
- comics
- dev
...

When tasked to write a command line tool utilizing a web scraper for my program at The Flatiron School, I chose a path that's pretty standard for me. Namely, I tried to find something I could do that would maybe come in handy later. In this particular situation, I've been wanting to make a tool with access to the Comic Vine API for some time. While API access was not on the menu for a scraping tool, I felt like doing something else in the realm of comics might be a useful start. At the very least, perhaps I'd have a start on some object frameworks for the future.

To this end, I chose to build a scraper that would look at the Comixology new release page and extract the new comics that were available, allowing the user to drill down into individual issue to retrieve more information. 

When the project first started out, things were fairly easy. Using Nokogiri to scrape the site, I found the CSS selectors fairly simple to differentiate. Comixology has used many specific attributes for the various sections of their pages, which made selecting specific information a breeze.

### Strange text formatting

The first minor hurdle came with the formatting of some of the text on the page. Instead of using raw CSS for positioning, page used tabs and newlines for some of their text positioning. This resulted in output like this (in this case, artists): 

```
\n\t\t\t\t\t\t\t\n\t\t\t\t\t\t\t\tJohn Buscema\t\t\t\t\t\t\t\n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\n\t\t\t\t\t\t\t\tKim DeMulder\t\t\t\t\t\t\
t\n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\n\t\t\t\t\t\t\t\tDavid Mazzucchelli\t\t\t\t\t\t\t\n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\n\t\t\t\t\t\t\t\tGerry Ta
loc\t\t\t\t\t\t\t\n\t\t\t\t\t\t
```
<br>

Obviously, that's no good for a program that's designed to be human readable. I needed to remove these non-characters, and I also wanted a format that would differentiate names, no matter how many different contributors had worked on a specific book. I ended up using the following line:

```ruby
issue_hash[:artist] = page.css("h2[title='Art by']").text.gsub("\n\t\t\t\t\t\t\n\t\t\t\t\t\t\t\n", ", ").gsub("\t", "").gsub("\n", "").split.join(" ")
```
<br>

Essentially, what we do here is find a very specific string that Comixology uses to achieve newlines and position between contributors and replace it with a comma and a space. Then, we find every other tab and newline and remove them outright. Finally, we split the string on space characters and then rejoin. We do this cleanse our strings of repeated spaces, since some strings within the Comixology site use long strings of spaces for positioning. This may seem like a roundabout method of achieving this (versus something like .squeeze), but as it turns out, this is one of [the most efficient methods](http://stackoverflow.com/questions/4907068/how-do-i-remove-repeated-spaces-in-a-string) for small strings.

### Foreign characters in URLs

The next foible had to do with URL parsing. Scraping Comixology's main site was working just fine, but I found the app failing when attempting to drill down into some of the issues on the site. Upon further investigation, I found these issues were in foreign languages (French, Italian, etc) and the URLs referring specifically to them included non-unicode characters. These charcaters are not supported by Ruby's URI implementation.

To deal with this issue, I chose to implement the [Addressable gem](https://github.com/sporkmonger/addressable). Addressable provides many functions for dealing with URIs more robustly than Ruby can natively, but in this case what I wanted was the ability to normalize my path, replacing the foreign characters with their percent-encoded equivalent. See below:

```ruby
issue_uri = Addressable::URI.parse(issue_url)
parse_path = "https://www.comixology.com/" + issue_uri.normalized_path
page = Nokogiri::HTML(open(parse_path))
```

<br>
### Future plans

The Comixology New Release Extractor is now at a point where I feel comfortable submitting it as a project. However, I would not feel comfortable submitting it to the general public or attempting to publish it as a gem. There are a few changes that need to be made before that can be considered.

**Formatting and pagination**

Right now, user output is pretty good, if ascetic, at the single issue level. But the full list level is a different story. In its current format, the list option returns 188 issues, at one per line. That's a lot for a user to scroll through in a terminal window. I tried [Pager](https://github.com/sferik/pager), but it wasn't very user friendly. One option might be to use [Command Line Reporter](https://github.com/wbailey/command_line_reporter), but another possibility would be to split the lines up via nested arrays and build the paging in myself.

I'd also like to begin extracting the release dates and optionally grouping comic releases beneath them, since the new release page displays releases from multiple dates.

**Reading ALL comic releases**

Comixology's release page pages out the releases at a maximum of 12 per date. I believe we could read all of the pages from each date by reading the hrefs of the buttons and then checking the CSS of the page they lead to, but that will be both network intensive (slow) and involved to implement.

**Making the app into a usable gem**

Once all the above are completed, I'll consider this a possibility.

-----

### Summation

Creating this app has definitely been a learning experience. Some things that I thought would be difficult (CSS selectors) ended up being very simple. Other things that I expected to be trivial (output format) turned out to be major issues. I look forward to my instructor code review and my continued improvement on this project.