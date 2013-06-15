---
layout: post
title: "Open source books @ Kindle"
date: 2012-12-22 14:07
comments: true
categories: 
---

I finally bought my [Kindle Paperwhite](http://www.amazon.com/Kindle-Paperwhite-Touch-light/dp/B007OZNZG0) (which is [jaibroken](http://wiki.mobileread.com/wiki/Kindle_Touch_Hacking) btw). The cool thing about Kindle that it has community that provides a lot of good stuff:

 * [Converter of Apple Docset documentation to Kindle](https://github.com/omz/docset2kindle)
 * [Converter of source code trees](https://github.com/agentzh/src2kindle)
 * [Converter of](https://github.com/pingwin/RFC-2-Kindle) [RFCs documents](http://en.wikipedia.org/wiki/Request_for_Comments)
 * [Converter of Python documentation](https://github.com/charlax/Python-Documentation-Kindle)
 * [Port of Coolreader](https://github.com/CrazyCoder/coolreader-kindle-qt)
 * [Framework for writing "recipes" to convert any book](https://github.com/danchoi/kindlefodder)
 
I used last one, `kindlefodder`, to write recipes for books [The Little Book on CoffeeScript](http://arcturo.github.com/library/coffeescript/) and [Smooth CoffeeScript](http://autotelicum.github.com/Smooth-CoffeeScript/) 

<!-- more -->

Kindlefodder takes a lot of routine work to itself.

First, I specified document's info, including cover:

``` ruby
def document 
  # download cover image
  if !File.size?("cover.gif")
    `curl -s 'http://akamaicovers.oreilly.com/images/0636920024309/lrg.jpg' > cover.jpg`
    run_shell_command "convert cover.jpg -type Grayscale -resize '400x300>' cover.gif"
  end
  # book's info
  {
    'title' => 'The Little Book on CoffeeScript',
    'author' => 'Alex MacCaw',
    'cover' => 'cover.gif',
    'masthead' => nil
  }
end
```

Then, we are fetching first page (with TOC) and formatting YAML with articles list:

``` ruby
def get_source_files
  # fetch first page (with TOC)
  @start_url = "http://arcturo.github.com/library/coffeescript/"
  @start_doc = Nokogiri::HTML run_shell_command("curl -s #{@start_url}")

  # create sections.yml
  sections = [{
    title:"Main",
    articles:extract_articles
    }]
  File.open("#{output_dir}/sections.yml", 'w') {|f|
    f.puts sections.to_yaml
  }
end
```

Resulted `sections.yml` will look like:

``` yaml
---
- :title: Main
  :articles:
  - :title: Introduction
    :path: articles/01_introduction.html
  - :title: Syntax
    :path: articles/02_syntax.html
  - :title: Classes
    :path: articles/03_classes.html
  - :title: Idioms
    :path: articles/04_idioms.html
  - :title: Compiling
    :path: articles/05_compiling.html
  - :title: Applications
    :path: articles/06_applications.html
  - :title: The Bad Parts
    :path: articles/07_the_bad_parts.html
```

So, it has one section (`Main`) and a lot of articles. More complex books could have multiple sections, of course.

Let's take a look on `extract_articles` method:

``` ruby
def extract_articles
  # iterating over Table of Contents and extracting articles
  @start_doc.search('ol.pages li a').map do |o|
    title = o.inner_text

    FileUtils::mkdir_p "#{output_dir}/articles"

    {
      title: title,
      path: save_article_and_return_path(o[:href])
    }
  end
end
```

And `save_article_and_return_path` method, which fetches actual article, cleans it, saves it and returns saved article path:

``` ruby
def save_article_and_return_path href, filename=nil
  path = filename || "articles/" + href.sub(/^\//, '').sub(/\/$/, '').gsub('/', '.')
  # fetching article
  full_url = @start_url + href.sub(/^\//, '')
  html = run_shell_command "curl -s #{full_url}"
  # cleaning article
  article_doc = Nokogiri::HTML html
  b = article_doc.at(".back")
  b.remove if b
  # saving article
  res = article_doc.at('#content').inner_html
  File.open("#{output_dir}/#{path}", 'w') {|f| f.puts res}
  path
end
```

I'm using beautiful [Nokogiri](http://nokogiri.org) ruby library for chopping of HTML here.

You don't have to care about images, `.mobi` format and stuff like that, Kindlefodder does it for you.

So, resulted recipes:

* [The Little Book on CoffeeScript](https://github.com/danchoi/kindlefodder/blob/master/recipes/little_book_on_coffeescript.rb)
* [Smooth CoffeeScript](https://github.com/danchoi/kindlefodder/blob/master/recipes/smooth_coffeescript.rb)