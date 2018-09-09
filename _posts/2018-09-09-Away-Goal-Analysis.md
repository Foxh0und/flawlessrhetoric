---
layout: post
title: "Away Goal Analysis"
description: "An in depth look at the recent history of away goals in the UCL"
tag: Data
---

## Introduction
Recently, several managers, including two time UCL winners José Mourinho of Manchester United and Carlo Ancelloti of Napoli, as well as former Spain head coach Julen Lopetegui currently at Real Madrid, Juventus man and two time finalist Massimiliano Allegri, and Arsène Wenger formerly at Arsenal have called for a review of the away goal rule. Giorgio Marchetti, deputy general secretary of UEFA has confirmed that UEFA will "open a discussion" concerning the rule. Many coaches believe that it is no longer as difficult to score away from home [1]. After listening to the Guardian Football Weekly with Max, Barry, and Elis discuss the topic, I decided to spend the international break wisely and take a closer look at the numbers.
<br><br>
### The Rule
The away goal is currently used for breaking ties in many football competitions, including in the UEFA Champions League. According to FIFA's Law of the game, "Competition  rules  may  provide  that  where  teams  play  each  other  
home and away, if the scores are equal after the second match, any 
goals scored at the ground of the opposing team will count double." [2]. 
<br>
In simpler terms, by the final whistle of normal time of the second game in a tie, if both teams are level on goals, and if one team has scored more goals away from home than the other, they will advance. If both teams are still tied, then they will advance to extra time.
<br><br>
### History and Impact
The away goal rule was first introduced by UEFA in the 1965 UEFA Cup Winners Cup [3]. Recently, it has not been overly influential. Since the beginning of the knock out legs in the 1994-95 UEFA Champions League, only one team has ever benefited from the rule, and subsequently gone on to win the competition. This was German side FC Bayern München during the 2012-13 season where they met Arsenal in the round of 16[4].
<br><br>
## A Look at the Numbers

### The Data
I collected the data for all knock out games since the 1994-95 season of the Champions League. I began here as this is where they began having knock out rounds, with the top 8 teams from the group stages making up the quarter finals, with a total of 12 games. With the 2003-04 season, a further 8 teams were added to the knock out stages, expanding it to 28 games played. The data was compiled using LibreOffice Calc. The data does not include the final, where (whilst occasionally not) perfunctory home and away designations are assigned. All plots were constructed using R.
<br><br>
### The Count
Whilst the total count does not show us anything particularly insightful, it is a good visualisation clearly showing the goals, and the obvious increase from the 2003-04 season.
<br>
![Count](https://raw.githubusercontent.com/Foxh0und/Away-Goal-Analysis/master/Plots/Count.png)
It is quite clear to see that the home goals have always been more prevelant than away goals. But does that mean that away goals are worth more?
<br>
### Away Goals Per Game
Here we can see the amount of away goals per game (on average).
<br>
![Away Goals Per Game](https://raw.githubusercontent.com/Foxh0und/Away-Goal-Analysis/master/Plots/PerGame.png)
<br>
It begins quite low, with it only jumping above a single goal per game until the 1998-1999 winning season. We can see that towards the tail end of the 2000s, there was on average at least one away goal scored per game, with 2011-12 and 2015-16 as outliers. Applying a line to this graph indicates that the amount of away goals per game is increasing. 
<br><br>
### Away Goal Percentage
The first graph shows that as seasons progress, the amount of goals scored is increasing during the knock out stages, but are they home or away goals?
<br>
![Away Goal Percentage](https://raw.githubusercontent.com/Foxh0und/Away-Goal-Analysis/master/Plots/AwayGoalPercent.png)
<br>
Correlating with the goals per game, we can see that at the beginning of the data, over 70% of the total amount of goals scored were by the home team, but as we move on away goals are taking up a larger proportion of the amount of goals scored. They however, are still not reaching parity with the home goal tally. If a regression line is added to our plot, we again see that this percentage is increasing.
<br><br>
## Conclusion
After looking at the numbers for the last 23 seasons, it is clear to see that more and more away goals are scored, indicating that the coaches may be correct, it does appear easier to score away from home, and thus, they really should not count for double. 
<br>
I do not mind the use of the away goal rule, but I have seen it ruin ties. A team may only be a goal down on aggregate, but because of the rule, need to score two goals in a matter of five minutes. This often kills the game, as the optimism of the players is all but gone. 
<br>
I do think it is clear that recent trends show that more away goals are being scored, and should definitely not count as double.
<br><br>


## References
- [1] The Guardian, "José Mourinho among top coaches calling on Uefa to review away goals rule", 2018.
- [2] Laws of the Game 2007/2008. Zurich: Fédération Internationale de Football Association, 2018, p. 54.
- [3] "Away goals rule", En.wikipedia.org, 2018. [Online]. Available: https://en.wikipedia.org/wiki/Away_goals_rule. [Accessed: 09- Sep- 2018].
- [4] "UCL - Matches", UEFA.com, 2013. [Online]. Available: https://www.uefa.com/uefachampionsleague/season=2013/matches/index.html#/rd/2000348/2. [Accessed: 09- Sep- 2018].
<br><br>
The source code and data can be found [here](https://github.com/Foxh0und/Away-Goal-Analysis/).
