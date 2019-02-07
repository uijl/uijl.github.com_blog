---
title: "Hacker News Top Sources"
date: 2019-02-07T12:00:00+01:00
draft: false
tags: [hacker news, data analysis]
categories: []
---

One of the two sites that I visit daily is [Hacker News](news.ycombinator.com). I saw [this tweet](https://twitter.com/paulg/status/1049723540902215681) by its developer (and the co-founder of Y Combinator) a while back and wondered what the most popular sources are for the top posts on Hacker News. Today marks the anniversary of the second launch, so no better time for this blogpost.

![https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-inspiration.png](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-inspiration.png "Tweet by Paul Graham")

The all-time top posts can be found via a very simple [search](https://hn.algolia.com/?q=&query=&sort=byPopularity&prefix&page=0&dateRange=all&type=story), an empy query sorted by popularity over all time. But because the popularity of Hacker News has increased over time, the more popular sources will be skewed towards more recent posts. Therefore I thought it might be more interesting to see what most frequent sources are if I gather the top 5 daily stories since the start of the site.

### Top posts per day

As we can see from the tweet by Paul Graham, Hacker News started on 2006-10-09. At first I tried finding the top 5 stories per day, because that was my goal. However, I noticed that I missed some noteworthy datapoints (the death of Steven Hawking for example). Therefore I tried again by finding the top stories per 12 hours, now I did seem to capture all the data.

![https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top5-stories-per-month.png](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top5-stories-per-month.png "Number of comments for the top 5 stories every month")

The graph above shows the average number of points for the top   5 stories (the blue line) and the bound of the number of points of the most popular daily post and the fifth popular daily post. As I expected, there is a clear trend in higher points for more recent posts. There are three distinctive peaks, the top 3 posts of all time:

1. The death of Steve Jobs ([2011-10-05](https://www.apple.com/stevejobs/))
2. The note from Apple on data protection ([2016-02-16](https://www.apple.com/customer-letter/))
3. The death of Steven Hawking ([2018-03-14](https://www.bbc.com/news/uk-43396008))

### Top sources

Now that we have the list of the top 5 posts per day, we can extract the most common sources of these posts. It is fun to see that (on average) the most popular posts have no URL, these are "Ask HN" posts. I understand why the "Ask HN" posts are so popular, the questions are often relevant for a large number of people and there is an intelligent and diverse community on Hacker News. The second position is held by Github, often "Show HN" posts, used for both launching new projects and asking feedback.

The highest ranking publishers, by far, are The New York Times and TechCrunch as we can see below. I wonder if this is related to quality or exclusives.

![https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top-sources-per-month.png](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top-sources-per-month.png "Top 20 sources based on the top stories every month")

### Top publishers

To obtain the list below I had to remove 17 sources that scored a higher number of top stories than Reuters. Including five sources related to Google (even Google Plus was in the list). I also combined bbc.com and bbc.co.uk, assuming that they run the same content.

The top sources also included two personal websites: the website of the founder of [Hacker News](https://news.ycombinator.com) [Paul Graham](https://http://paulgraham.com/) and the website of the founder of [Stack Overflow](https://stackoverflow.com/) [Jeff Atwood](https://blog.codinghorror.com/).

![https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top-publishers-per-month.png](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/hacker-news-blog-top-publishers-per-month.png "Top 20 publishers based on the top stories every month")