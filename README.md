# 2022-World-Cup-Model
## Model predictitions published at [BetMGM](https://sports.betmgm.com/en/blog/fifa-world-cup-win-prediction-every-game-jaa/) 

![f](https://user-images.githubusercontent.com/73828790/225350376-d33bdee1-08ff-461d-950e-282ffbff287e.jpeg)

### Overview
#### Can we use Elo ratings and machine learning to predict winners of the 2022 World Cup Matches?

The repository contains two R markdown files: 
#### 1. Creating, Training and Validating Model (WorldCup2022UpdatedELO)
The XG Boost model is trained on over 6,000 international friendly, world cup qualifying, and world cup tournament matches between 2010-2022 using data provided via the WorldfootballR package. As a logit model, its output is the probability of each team winning or a draw.

#### 2. Deploying the Model for Upcoming Games (WorldCupPredictionsFinal)
This file allows the user to deploy the model against the day’s games after entering the teams playing one another and the posted odds for each team winning. The output is a csv file which shows the “edge” of each pick as defined by the implied probability of the given odds to win minus the probability of winning provided by the model. 

### Data Collection
The data used for this model is:
1. Outcomes of international friendly, world cup qualifying, and world cup tournament matches between 2010-2022, and
2. The Elo ratings of each team playing at the time of their match.

#### Outcome Data
Using the WorldfootballR package, I collected the results over 8,000 matches between national teams from all over the world between 2010-2022 from international friendly, world cup qualifying, and world cup tournament matches. 

#### Elo Ratings
I scraped each team's most recent Elo rating prior to its match day from [Elo ratings](eloratings.net). This data provided average and one-year change in ranks and Elo ratings, goal differential, and historical records.  Originating from the world of chess, the Elo ratings use the difference in rankings between two teams as a predictor of the match. Each team is immediately debited or credited as a result of the match.  A detailed description can be found [here](https://en.wikipedia.org/wiki/Elo_rating_system#:~:text=The_Elo_rating_system_is,a_Hungarian-American_physics_professor). The system was popularized by Nate Silver at his site [538](https://fivethirtyeight.com/). 

<img width="1054" alt="Capture" src="https://user-images.githubusercontent.com/73828790/225359406-b36ca7a7-5fa4-46f2-a4c1-3e6e3e450590.PNG">

### Data Cleaning 
#### Consistency
The biggest challenge was creating consistency country names as they are abbreviated differently in each dataset.  Additionally, numerous countries changed their names over the 12-year period of investigation. 

#### Joining Data
Outcome data needed to be joined with rating data. Teams also needed to be assigned a unique game ID resulting the two teams playing one another having the same game ID. Next the teams are split based on being home or away to then be joined based on the unique ID to create a match as a single observation. 

### Feature Engineering
In addition to the data collected, I added variables to measure:
1. Difference between each team’s ranks and ratings and 1-year changes in them, 
2. Differences in historical winning percentages of the teams, 
3. Created an average goals for and against,
4. The interaction between a team’s goals scored and the opponent’s goals allowed.  

### Model Creation
I perform cross validation to determine the optimal number of trees to train. Because there are 3 possible outcomes in a soccer match, I used a 'softprob' objective function and set the number of classes equal to 3. 

### Model Performance
After fitting the model, it is validated on a random selection of games of over 2000 games between 2010-2022.  The overall accuracy is 57%. With 95% confidence, we can expect the overall accuracy to fall between 55-59%. 

<img width="347" alt="Capture" src="https://user-images.githubusercontent.com/73828790/225379812-00e72523-887b-4eb0-ab3c-69cb59d0c6e9.PNG">

### Variable Importance
I’ve also included an importance matrix to show which variables are most influential in predicting winners. As you can see, the difference in ratings is far and away the most important predictor of who will win the game.  

![Picture2](https://user-images.githubusercontent.com/73828790/225384399-cd71bcf2-50fc-4068-bf53-a23145c0d851.png)

### Deployment
The deployment file creates and trains the model in the same way as the previous file. However rather than validating the model, the user creates a csv to be read in which contains the input variables for each team required by the model take from [Elo](eloratings.net). The user then creates a second csv with the odds of each team winning.  The output is a third csv containing the difference between the implied probability of the odds and the model's predicted probability of winning. 


<img width="769" alt="Capture" src="https://user-images.githubusercontent.com/73828790/225389074-e9010aca-fd8e-4cbf-9743-c96bb9189712.PNG">


