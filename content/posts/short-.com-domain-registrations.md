---
title: "When are all two and three-character .com domains registered?"
date: 2019-01-01T12:00:00+01:00
draft: true
tags: [Webscraping, Data Analysis, Data]
categories: []
---

Short domain names are worth money. Some of the most popular domain names are two- or three-letter .com domains. In the top 20 of the [Alexa top 500](https://www.alexa.com/topsites) there are three short domains listed: [rank 7 (qq.com)](https://www.qq.com), [rank 15 (jd.com)](https://www.jd.com) and [rank 17 (vk.com)](https://www.vk.com). Or even four if you count [rank 3 (fb.com)](https://www.fb.com), which is listed as [facebook.com](https://www.facebook.com).

Inspired by some posts on domain registration dates, such as [this one](https://www.cambus.net/oldest-domains-in-the-com-net-and-org-tlds/), I decided to inspect the registration date of all two-letter and three-letter .com domains. For this analysis I only looked to the ascii-letters, which gives us a total of 26^2 + 26^3 = 18,252 domain names.

Unfortunately all domains are already taken. However, I still think the analysis of the registration dates is interesting. For example, most of the domains were registered during the dot-com bubble. The peak of the NASDAQ Composite and the monthly number of registrations even coincides, peaking in early 2000.

In the upcoming sections I will go deeper in the analysis and list some interesting findings. The registration dates of all websites, the code I used for obtaining the data and the code I used for the analysis can be found on my [github](https://github.com/uijl/Scraping-Projects/Oldest-Domains).

### Top 20

The first registered .com domain is [symbolics.com](https://www.symbolics.com), which was registered on March 15, 1985. The first registered domain in the dataset that I am looking at was [bbn.com](https://www.bbn.com) and according to both [Wikipedia](https://en.wikipedia.org/wiki/List_of_the_oldest_currently_registered_Internet_domain_names) and [Frederic Cambus](https://www.cambus.net/oldest-domains-in-the-com-net-and-org-tlds/) this was the second .com domain ever registered. It registered on April 24, 1985.

It took over two and a half years to register the first 20 domain two- or three-letter .com domains, number twenty was [gte.com](https://www.gte.com) and registered on November 5, 1986. The twenty earliest registered domains in the dataset that I am looking at are listed below:

| Domain names | Creation date |
|--------------|---------------|
| bbn.com      | 1985-04-24    |
| mcc.com      | 1985-07-11    |
| dec.com      | 1985-09-30    |
| sri.com      | 1986-01-17    |
| hp.com       | 1986-03-03    |
| ibm.com      | 1986-03-19    |
| sun.com      | 1986-03-19    |
| ti.com       | 1986-03-25    |
| att.com      | 1986-04-25    |
| gmr.com      | 1986-05-08    |
| tek.com      | 1986-05-08    |
| fmc.com      | 1986-07-10    |
| ub.com       | 1986-07-10    |
| isc.com      | 1986-08-05    |
| nsc.com      | 1986-08-05    |
| ge.com       | 1986-08-05    |
| ray.com      | 1986-10-27    |
| bdm.com      | 1986-10-27    |
| nec.com      | 1986-10-27    |
| gte.com      | 1986-11-05    |

### Bottom 20

The latest registration of any of the domains was on July 19th, 2018. Only a month ago. But given the fact that it has been registered by DropCatch.com it probably has been registered before, unfortunately I have not been able to retrieve the original registration date. The same is true for the second-to-last registration, this was on May 13, 2018 and is also held bij DropCatch.com.

The twenty latest registrations in the dataset that I am looking at are listed below. It is interesting to see that only 20 domains, out of a dataset of 18,252, have been (re-)registered over the past eight years.

| Domain names | Creation date |
|--------------|---------------|
| qpe.com      | 2010-06-05    |
| yds.com      | 2010-11-21    |
| ihh.com      | 2011-04-01    |
| xte.com      | 2011-04-15    |
| ycp.com      | 2011-05-19    |
| syc.com      | 2011-06-24    |
| hzg.com      | 2011-07-03    |
| iqw.com      | 2012-02-01    |
| wgj.com      | 2013-04-06    |
| kjq.com      | 2013-04-28    |
| bsf.com      | 2013-09-03    |
| cnm.com      | 2013-09-25    |
| zfn.com      | 2014-07-24    |
| cm.com       | 2015-01-16    |
| pdm.com      | 2015-04-05    |
| viq.com      | 2016-11-05    |
| xci.com      | 2017-02-15    |
| rxj.com      | 2018-05-09    |
| njd.com      | 2018-05-13    |
| btm.com      | 2018-07-19    |

### Cumulative build of registrations

Given that it took over two and a half years to register the first twenty domain names, it is interesting to see how the cumulative number of registrations has developed over time. From registration number 1 to registration number 18,252. This is visualized in the image below, the solid lines shows the total number of registrations at any given time. The two blue dots represent registration number 1,000 and registration number 17,252: mark the end of the first thousand registerations and the marking the start of the last thousand registrations.

It took approximately 9 years to register the first 5% of all domains, 9 more years to register 90% of the total population and finally another 9 years for the remaining 5%.

![Cumulative registrations](https://raw.githubusercontent.com/uijl/Scraping-Projects/master/Oldest-Domains/Figures/Cumulative_registrations.png "The cumulative number of registrations")

### Number of registrations during .com bubble

Most of the domain name registrations took place in the 1990s. And it is very interesting (and obvious, but still ..) that the steep increase in registrations start approximately in 1993 / 1994. Right after the release of the [Mosaic web browser](https://en.wikipedia.org/wiki/Mosaic_(web_browser)), which has been said to start the .com bubble.

As the image below shows, the maximum number of monthly domain registerations peaked almost simultaneously with the [NASDAQ Composite](https://finance.yahoo.com/quote/%5EIXIC/history?period1=441759600&period2=1525125600&interval=1mo&filter=history&frequency=1mo). The maximum number of domains registered in a month is 1,107 and this was in January, 2000. The NASDAQ Composite peaked March 10, 2000 and the bubble burst right after. The NASDAQ only broke the record of 2000 in 2015, 15 years (and another large stock crash) later.

![Registrations during bubble](https://raw.githubusercontent.com/uijl/Scraping-Projects/master/Oldest-Domains/Figures/Registrations_vs_Nasdaq.png "The number of domain name registrations during the .com bubble")