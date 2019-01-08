---
title: "Benefit of Scoring Right Before The Break"
date: 2019-01-01T12:00:00+01:00
draft: false
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

### What is *right before the break*

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

<section class = "sharing-buttons">
<!-- Sharingbutton Twitter -->
<a class="resp-sharing-button__link" href="https://twitter.com/intent/tweet/?text=What%20is%20the%20benefit%20of%20scoring%20right%20before%20the%20break%3F&amp;url=https%3A%2F%2Fuijl.github.io%2Fposts%2Fbenefit-of-scoring-right-before-the-break%2F" target="_blank" rel="noopener" aria-label="">
  <div class="resp-sharing-button resp-sharing-button--twitter resp-sharing-button--small"><div aria-hidden="true" class="resp-sharing-button__icon resp-sharing-button__icon--solid">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M23.44 4.83c-.8.37-1.5.38-2.22.02.93-.56.98-.96 1.32-2.02-.88.52-1.86.9-2.9 1.1-.82-.88-2-1.43-3.3-1.43-2.5 0-4.55 2.04-4.55 4.54 0 .36.03.7.1 1.04-3.77-.2-7.12-2-9.36-4.75-.4.67-.6 1.45-.6 2.3 0 1.56.8 2.95 2 3.77-.74-.03-1.44-.23-2.05-.57v.06c0 2.2 1.56 4.03 3.64 4.44-.67.2-1.37.2-2.06.08.58 1.8 2.26 3.12 4.25 3.16C5.78 18.1 3.37 18.74 1 18.46c2 1.3 4.4 2.04 6.97 2.04 8.35 0 12.92-6.92 12.92-12.93 0-.2 0-.4-.02-.6.9-.63 1.96-1.22 2.56-2.14z"/></svg>
    </div>
  </div>
</a>

<!-- Sharingbutton E-Mail -->
<a class="resp-sharing-button__link" href="mailto:?subject=What%20is%20the%20benefit%20of%20scoring%20right%20before%20the%20break%3F&amp;body=https%3A%2F%2Fuijl.github.io%2Fposts%2Fbenefit-of-scoring-right-before-the-break%2F" target="_self" rel="noopener" aria-label="">
  <div class="resp-sharing-button resp-sharing-button--email resp-sharing-button--small"><div aria-hidden="true" class="resp-sharing-button__icon resp-sharing-button__icon--solid">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M22 4H2C.9 4 0 4.9 0 6v12c0 1.1.9 2 2 2h20c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zM7.25 14.43l-3.5 2c-.08.05-.17.07-.25.07-.17 0-.34-.1-.43-.25-.14-.24-.06-.55.18-.68l3.5-2c.24-.14.55-.06.68.18.14.24.06.55-.18.68zm4.75.07c-.1 0-.2-.03-.27-.08l-8.5-5.5c-.23-.15-.3-.46-.15-.7.15-.22.46-.3.7-.14L12 13.4l8.23-5.32c.23-.15.54-.08.7.15.14.23.07.54-.16.7l-8.5 5.5c-.08.04-.17.07-.27.07zm8.93 1.75c-.1.16-.26.25-.43.25-.08 0-.17-.02-.25-.07l-3.5-2c-.24-.13-.32-.44-.18-.68s.44-.32.68-.18l3.5 2c.24.13.32.44.18.68z"/></svg>
    </div>
  </div>
</a>

<!-- Sharingbutton LinkedIn -->
<a class="resp-sharing-button__link" href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https%3A%2F%2Fuijl.github.io%2Fposts%2Fbenefit-of-scoring-right-before-the-break%2F&amp;title=What%20is%20the%20benefit%20of%20scoring%20right%20before%20the%20break%3F&amp;summary=What%20is%20the%20benefit%20of%20scoring%20right%20before%20the%20break%3F&amp;source=https%3A%2F%2Fuijl.github.io%2Fposts%2Fbenefit-of-scoring-right-before-the-break%2F" target="_blank" rel="noopener" aria-label="">
  <div class="resp-sharing-button resp-sharing-button--linkedin resp-sharing-button--small"><div aria-hidden="true" class="resp-sharing-button__icon resp-sharing-button__icon--solid">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M6.5 21.5h-5v-13h5v13zM4 6.5C2.5 6.5 1.5 5.3 1.5 4s1-2.4 2.5-2.4c1.6 0 2.5 1 2.6 2.5 0 1.4-1 2.5-2.6 2.5zm11.5 6c-1 0-2 1-2 2v7h-5v-13h5V10s1.6-1.5 4-1.5c3 0 5 2.2 5 6.3v6.7h-5v-7c0-1-1-2-2-2z"/></svg>
    </div>
  </div>
</a>

<!-- Sharingbutton WhatsApp -->
<a class="resp-sharing-button__link" href="whatsapp://send?text=What%20is%20the%20benefit%20of%20scoring%20right%20before%20the%20break%3F%20https%3A%2F%2Fuijl.github.io%2Fposts%2Fbenefit-of-scoring-right-before-the-break%2F" target="_blank" rel="noopener" aria-label="">
  <div class="resp-sharing-button resp-sharing-button--whatsapp resp-sharing-button--small"><div aria-hidden="true" class="resp-sharing-button__icon resp-sharing-button__icon--solid">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M20.1 3.9C17.9 1.7 15 .5 12 .5 5.8.5.7 5.6.7 11.9c0 2 .5 3.9 1.5 5.6L.6 23.4l6-1.6c1.6.9 3.5 1.3 5.4 1.3 6.3 0 11.4-5.1 11.4-11.4-.1-2.8-1.2-5.7-3.3-7.8zM12 21.4c-1.7 0-3.3-.5-4.8-1.3l-.4-.2-3.5 1 1-3.4L4 17c-1-1.5-1.4-3.2-1.4-5.1 0-5.2 4.2-9.4 9.4-9.4 2.5 0 4.9 1 6.7 2.8 1.8 1.8 2.8 4.2 2.8 6.7-.1 5.2-4.3 9.4-9.5 9.4zm5.1-7.1c-.3-.1-1.7-.9-1.9-1-.3-.1-.5-.1-.7.1-.2.3-.8 1-.9 1.1-.2.2-.3.2-.6.1s-1.2-.5-2.3-1.4c-.9-.8-1.4-1.7-1.6-2-.2-.3 0-.5.1-.6s.3-.3.4-.5c.2-.1.3-.3.4-.5.1-.2 0-.4 0-.5C10 9 9.3 7.6 9 7c-.1-.4-.4-.3-.5-.3h-.6s-.4.1-.7.3c-.3.3-1 1-1 2.4s1 2.8 1.1 3c.1.2 2 3.1 4.9 4.3.7.3 1.2.5 1.6.6.7.2 1.3.2 1.8.1.6-.1 1.7-.7 1.9-1.3.2-.7.2-1.2.2-1.3-.1-.3-.3-.4-.6-.5z"/></svg>
    </div>
  </div>
</a>
</section>