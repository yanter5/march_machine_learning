# march_machine_learning
Kaggle submission for march madness bracket prediction contest

# Approach:
-K-nearest neighbors algorithm
-Predicted win percentages by pulling game outcomes from k-nearest matchups in all regular season matchups since 2003

# Model:
Matchup similarity was based on the following variables:
  1. Average end of season rank across all provided national rankings
  2. Offensive rating (points scored per possession)
  3. Defensive rating (points allowed per possession)
  4. Three point percentage
  5. Free throw percentage
  6. Assist/TO ratio
  7. Turnovers allowed
Each of these were normalized due to the nature of KNN algorithm not requiring any assumptions about the data.
Each team was treated as a completely different team year to year. That is, 2018 Alabama would have entirely different stats from 2019 Alabama.

K=181 returned the lowest logloss after grid search analysis and was the number used in my model.

# Explanation:
Matchup similarity was determined by taking the difference in the normalized team stats discussed above.

Each game had 2 entries for matchup difference:
  1. Home Team - Away Team
  2. Away Team - Home Team
  
I did this because I realized that two matchups between the same teams would not register as being remotely similar if home and away teams were flipped for one of them. 

Example: 

We will suppose that Team B is exactly 1 greater than Team A in all variables.
  1. Team A - Team B
    -1 -1 -1 -1 -1 -1 -1
  2. Team B - Team A
     1  1  1  1  1  1  1
     
When identifying similar matchups to predict the outcome in a neutral setting like the NCAA tournament I wanted to find games with similar team difference profiles to our game. However, if I was to only log one entry for each game we would miss many close matchups that I would like to use in my win percentage prediction. Logging both differences allows me to capture every game in the database for comparison with the tournament matchup.

# Results
Finish: 880/930, overall logloss = 1.19
My submission scored very poorly because I failed to consider the ramifications of my model identifying a matchup as 100% win probability. 
This happened in the first round matchup between St. Peters and Kentucky, with Kentucky predicted to win 100% of matchups. 
I researched solutions for this and arrived at the possibility of "clipping" the probabilities at a number (such as 95% or something slightly more forgiveable to logloss) to prevent this from happening. 
Many people in the competition did this, and I wanted to test different methods of clipping so I played around with it. 
After running logloss calculations on my clipped results I believe I would have been around Top 50 had I clipped predicted win percentages at 95%, which was the most recommended number amongst other  participants in the kaggle forums.

