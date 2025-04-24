# bi-p7-basketball
BI project for MS in Data Analytics

## Section 1. The Business Goal
I originally approached this project with business goals tied to market research for selling art and art courses in Hawaii. The datasets I pulled in support of that analysis turned out to not be particularly helpful for multi-dimensional analysis OLAP demonstration though. (See Section 7. Challenges for more info.)  
After spending time down that path, I switched to a more appropriate dataset for this type of analysis and chose to focus my goals on filling in some of the PowerBI skills I thought I still needed to improve from the previous project. I decided to pull the sqlite database I had built of NBA data during a previous course, as it contained four tables with connecting foreign keys and a wide range of data points, including layers that would serve perfectly for practicing drill-down usage.
**Goal: Build reports in PowerBI to allow for an analyst to perform exploratory analysis on player-level data of NBA games with a no/low-code GUI based experience.**
- Use drill-down to expand/collapse time dimension over year, month, and day
- Use drill-down to nest players under their respective teams
- Focus on player-level statistics
- Create a composite metric that condenses multiples stats into one number to compare player impact

## Section 2. Data Source
I used data from the online lessons in "Learn to Code with Basketball" by Nathan Braun. I had previously assembled this into a sqlite databse in the Fundamentals of Data Analytics course. It is a sampling of game, player, and team data from 2019 and 2020.

## Section 3. Tools Used
- Power BI
- Microsoft CoPilot for AI assistance with generating formulas and navigating Power BI features that were new to me.
- ODBC Administrator

## Section 4. Workflow & Logic
1. Loaded the database into Power BI.
2. Created a table of key statistics with drill-down capability on both x and y axes.
3. Created line charts to experiment with displaying the date/time dimension.
4. Implemented calculated columns to create composite metrics measuring player impacts through posessions gained and used:
   `Possessions_gained = SUMX(player_games, player_games[oreb] + player_games[stl] + player_games[blk])`
   `Possessions_used = SUMX(player_games, player_games[fga] + player_games[tov])`
   `Net_possessions = SUMX(player_games, player_games[Possessions_used] - player_games[Possessions_gained])`
```
   VlochEfficiency = DIVIDE(1, 
    DIVIDE(
        player_games[fga] + player_games[tov] + player_games[pf] 
        - player_games[oreb] - player_games[stl] - player_games[blk] - player_games[pfd], 
        player_games[pts] + player_games[ast], 
        BLANK()
    ),
    BLANK()
)
```
    `ImpactPerMin = DIVIDE(player_games[VlochEfficiency], player_games[min]) * 100`

## Section 5. Results (narrative + visualizations)
This exercise helped me develop some skills in Power BI I wasn't comfortable with yet following the previous module:
- Implementing drill-down
- Aggregating composite metrics using SUM, SUMX, and DIVIDE
- Axes and dimensions available in scatter charts

## Section 6. Suggested Business Action
Validate the composite metrics created to determine how useful they are for predicting future performance.

## Section 7. Challenges
The original data I attempted to work with was Hawaii Tourism data. While I had several good spreadsheets to work from, they didn't quite line up to serve as dimension tables with a central fact table.  
Next, I tried working with Google keyword query data to determine the best key words and phrases to target for website SEO and potential paid search marketing campaigns. I was able to do some useful analysis, but this data didn't really line up in a multi-dimensional way for OLAP-style analysis either.
After dedicating a decent amount of time to those, I didn't have much time remaining so I decided to tap into my previously created database of basketball data and focus on experimenting with the weak spots I'd identified in my previous Power BI work.

## Section 8. Ethical Considerations
  1. Is the data being used responsibly?
   Yes, although the dataset used is only a sample of the full dataset so conclusions should not be drawn without pulling in the full season data.
  2. Could the analysis reinforce biases?
   Yes. While most of the stats being used in the composite metrics are sound, some are impacted by the judgement of the statisticians recording the data. What consititutes an assist, block, or steal are left to some interpretation leaving open possibility of some bias introduction.
  3. Are you making decisions based on incomplete or unverified data?
   This work is not sufficient for making decisions. The data is verified, but incomplete. The composite metrics developed can be useful for descriptive statistics to explain the past, but should be validated to determine whether they are useful as predictive analytics.
  4. How can the business use the insights responsibly?
   - The composite metric should only be used to give insight into what happened. For instance, Kyle O'Quinn had an amazing impact for PHI during the covered games.  
   - Testing these metrics over several years to see how much the score from one year predicts future success should be analyzed before making any contract decisions. For instance, if the metric does prove to be predictive Kyle O'Quinn may be a desirable player to sign the next year and/or allocate more minutes to in upcoming games.

