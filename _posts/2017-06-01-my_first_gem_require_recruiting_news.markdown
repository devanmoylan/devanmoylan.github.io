---
layout: post
title:  MY FIRST GEM >> require 'recruiting_news' 
date:   2017-06-01 04:32:28 +0000
---


THE GIST: I wanted to build a gem that would be usable on a personal project of mine. The project is designed to help high school athletes who aspire to play collegiate athletics. I thought pulling in recruiting news from credible sources would be beneficial to the project's user base. The recruiting news primary focuses on high school athletes who are doing well in the recruiting process. The news could serve as good resource for other aspiring athletes who haven't quite made their way into the spotlight yet.

GROUND ZERO - 0.0.0: The gem's initial makeup is as follows. 

### bin/recruiting_news 
### 
The responsibility of this bin file is to require the necessary files relative paths and initialize a new instance from the Scraper class which has an option of adding a path as an argument. After the new instance is created, the instance calls the Scrape instance method incharge of conducting the scrape. 

```
require_relative "../lib/scraper.rb"
require_relative "../lib/post.rb"
require "irb"

scraper = Scraper.new
scraper.run_scrape_on_scouts
```

### lib/scraper 
### 
The previous call takes us into the Scraper class. A step back, when the new scraper instance was initialized, it was initialized with a path (or default path if non-given), an empty post's array and the page to be scraped. All three values are stored in instance variables. 
```
class Scraper

  def initialize(path = path)
    @posts = []
    @path = path
    @page = Nokogiri::HTML(open(path))
  end
	
	...
```

A step back forward, the instance method incharge of the scrape checks to if the instance variables properly initialized. 
```
  def run_scrape_on_scouts
    if !@page.nil? && gather_posts_from_scouts #WE'RE HERE 
      create_new_post
    end
  end
```
If the initialization went as planned, it calls another instance method whose responsibility is to gather the posts from the page. The section containing the posts is iterated over, and the information extracted from the section is placed inside a hash which is stored inside of the posts instance variable. 
```
def gather_posts_from_scouts
    @page.css(".story-list-item").each_with_index do |post, index|
      @posts[index] = {
        :title => post.css(".story-deck h1 a span").last.text,
        :author => post.css(".story-deck .story-from").text,
        :time => post.css(".story-deck .time-stamp").text,
        :description => post.css(".story-deck p").text,
        :link => post.css(".story-deck .story-stuff a").attribute("href").value
      }
    return false if @posts.empty?
    end
  end
```
Once each post is iterated over, the Scrape instance method incharge of conducting the scrape then calls an instance method whose responsibility is to create a new post. 
```
  def run_scrape_on_scouts
    if !@page.nil? && gather_posts_from_scouts
      create_new_post #WE'RE HERE 
    end
  end
```

```
  def create_new_post
    @posts.each {|post| Post.new(post)}
  end
```
Each post within the posts instance variable is iterated over and used to create a new instance of the Post class, passing itself in as an argument. 

### lib/post  
### 
Attributes for the Post class are declared and then passed into intialize as a hash. The attributes are iterated over and the send method is used to assign the keys and values. Lastly, the new instance is stored within the class variable.
```
class Post

  attr_accessor :title, :author, :time, :description, :link

  @@all = []

  def initialize(attributes)
    attributes.map {|k, v| self.send("#{k}=", v)}
    self.class.all << self
  end

  def self.all
    @@all
  end

end
```

So far, the gem has been fun, but the initial rollout already has obvious room for improvement. 

