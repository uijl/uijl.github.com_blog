---
title: "Benefit of Scoring Right Before The Break"
date: 2019-01-07T12:00:00+01:00
draft: true
tags: [Football, Webscraping, Data Analysis, Data]
categories: []
---

I love watching football but there are several commentators in the Netherlands that have the annoying ability to kick in open doors and present useless facts. Unfortunately Dutch teams are often behind in international matches, so a statement that often returns is *"If they score now, right before the break, they will start the second half with a morale boost"*.

Although I understand the logic behind this reasoning, I have never seen data that substantiates it. Therefore I will try to see for myself whether this statement is correct or not. It is hard to find a large dataset of football goals online, so I'll construct one myself. I know that on [Uefa.com](https://www.uefa.com/uefachampionsleague/season=2016/matches/round=2000638/match=2015789/index.html "Example match") data per match event, per Champions League match, is available.

### Scraping UEFA.com

The [first Champions League match ever played](https://www.uefa.com/uefachampionsleague/history/season=1992/matches/round=46/match=6199/events/index.html "First Champions League match") was between *KI Klaksvik* and *Skonto Riga*. I'll admit that I had never ever heard from those teams. In order to see whether there is an advantage in scoring right before the break we need to know from every goal in what minute is was scored and by which team. The final score was 1-3 and scraping the webpage should therefore result in something similar as the table below. I want to know which period each goal is to distinguish between *45 + 1'* (added time first half) and *46'* (first minute second half).

| Team | Minute | Period      |
|:---- |-------:|:----------- |
| Home |     46 | Second Half |
| Away |     90 | Second Half |
| Away |     46 | Second Half |
| Away |     46 | Second Half |
| Away |     28 | First Half  |

To find all matches we have to scrape the match-overview pages. These all have the same pattern, with three variables: season, day and session. See this [example](https://www.uefa.com/uefachampionsleague/season=2017/matches/library/fixtures/day=11/session=1/_matchesbydate.html?_=1).

Variable *season* is easy to guess, this varies between 1992 and 2017. However, 2007 is missing (I guess due to a change in UEFA naming of the seasons). Season ['07-'08](https://www.uefa.com/uefachampionsleague/history/season=2008/ "Season 2007-2008") has variable 2008 and season ['06-'07](https://www.uefa.com/uefachampionsleague/history/season=2006/ "Seasons 2006-2007") has variable 2006. The day variable is a bit more difficult. I could have automated this as well, but by trial-and-error I found out that *day* stands for the week and can vary between -13 and 26 (this varies per seasons). Finally, *session* stands for the matches played on Tuesday (0) and Wednesday (1).

After scraping all goal related match-events ([using this code](https://github.com/uijl/uijl.github.io_code/blob/master/Football/Scrape%20UEFA%20data.ipynb)) I ended up with a json file per season containing all goals ([stored here](https://github.com/uijl/uijl.github.io_code/tree/master/Football/Goal%20information)) and a csv file per seasons containing all links to matches ([stored here](https://github.com/uijl/uijl.github.io_code/tree/master/Football/Match%20links)).

### Overview of goals per Champions League match

The first basic analysis of the dataset I just obtained is checking the number of goals per seasons and per match. The total number of goals is 25,262 and the number of matches in the dataset is 9,532. Resulting in an average of 2.65 goals per match. This is slightly higher than I expected. The plot below shows the average number of goals per match, per season, over the full dataset. It is interesting to see that it fluctuates strongly in the first few years. The fluctuations decrease – due to an increase in professionality? – with time.

![Average number of goals per Champions League match](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/football-average-goals-per-match.png "Average number of goals per Champions League match")

### What is *right before the brak*

So, finally the analysis that is the reason for me to start this exercise. For an actual scientific answer I would like to have had a larger dataset, but with close to 10,000 matches I hope to get close to an accurate answer. What would be an interesting *period of interest* in which the scoring team would actually benefit from scoring. To answer that, I plotted the percentage of goals scored for every minute of the match, see the example below.

![Percentage of goals per minute](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/football-goals-per-minute.png "Percentage of goals per minute")

It seems to be quite scattered, but what is interesting is that the percentage of goals scored in the first half is skewed towards the final minutes. In the second half it seems to be more uniformly distributed, except for the 90th minute. The final 5 minutes of the first half are somewhat uniformly distributed as well, and therefore I define *right before the break* as the time between minute 40 and the break. The plot below shows the percentage of goals as well, but bucketed into 5 minute windows.

![Percentage of goals per minute](https://raw.githubusercontent.com/uijl/uijl.github.io_blog/master/figures/football-goals-per-minute-buckets.png "Percentage of goals per minute")

### Benefit of scoring right before the break

For this analysis I do not care whether the goal is scored by the home-team or the away-team. I also do not care if more than one goal is scored during the *period of interest*. Only about the last goal scored during the *period of interest*. In my opinion there are four interesting options if a team scores:

- **Team increases its lead.** Only the case if the team increases a one-goal advantage.
- **Team takes the lead.** Game is tied and before the break one of teams scores.
- **Team ties the game.** The trailing team scores an equalizer right before the break.
- **Team decreases goal difference.** Only if the trailing team reduces the goal difference to 1 right before the break.

Of the total dataset with 9,532 matches there are 7,789 matches with no goal within the period of interest. From the remaining 1,743 matches I take 22 matches not into account because the lead of one of the teams is too large. So the final dataset contains 1,721 matches.

| Option   | Matches | Wins   | Ties   | Losses |
|:-------- | -------:| ------:| ------:| ------:|
| Option 1 |     617 | 92.87% |  5.51% |  1.62% |
| Option 2 |     782 | 74.42% | 15.86% |  9.72% |
| Option 3 |     239 | 41.84% | 23.85% | 34.31% |
| Option 4 |      83 | 18.07% | 15.66% | 66.27% |

It suprised me how strong this effect is. Especially *option 3*, you would expect that a tied score during the break would be like "starting over". However, the data showed that in the 239 games in which this specific event took place, the team scoring the equalizer right before the break is more likely to win.

To see to what extend scoring *right before the break* has an effect, I did the same analysis without a specific period. Just by looking at the team that scored the last goal before the break. As the table below shows, for option 1 and option 2 the results are similar. However, for a team that is trailing (option 3 and option 4) the effect of scoring right before the break is much more significant. Gaining a point (win +3 or tie +1) is much more likely if you score right before the break.

| Option   | Matches | Wins   | Ties   | Losses |
|:-------- | -------:| ------:| ------:| ------:|
| Option 1 |   1,545 | 94.56% |  4.01% |  1.42% |
| Option 2 |   3,716 | 74.35% | 16.87% |  8.77% |
| Option 3 |     879 | 33.67% | 33.11% | 33.22% |
| Option 4 |     198 | 16.16% | 26.26% | 57.58% |

### Conclusion

This shows that the Dutch commentators are right when they come with their favorite open door. For a trailing team there seems to be a significant boost when they score right before the break.

<br>
**This was my first blog post. If you have any recommendations, please contact me on [Twitter](https://www.twitter.com/jorisdenuijl "Twitter of Joris") or [GitHub](https://github.com/uijl/uijl.github.io_code "GitHub of Joris"). And if you like it, please share!**