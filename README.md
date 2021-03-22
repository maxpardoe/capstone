# Predicting Clean Sheets in Europe's Top 5 Leagues and MLS 

## Goals of the Project

Due to my love of sport I have always wanted to get into sports analytics and sports predicting. Therefore, due to this I decided to do my final capstone project on predicting clean sheets based on the 7 most attacking player’s form and 7 most defensive player’s form of the two teams playing each other. The goal for my project was to predict clean sheets above the baseline rate. 

## Step 1 - Dataset and Data Collection 

### Dataset:

I collected my data through a website called [FBREF](https://fbref.com/en/). This is a website that has varying levels of in depth statistics. With the most consistently in depth statistics being Europe’s top 5 leagues and the MLS. 

### Data Collection: 

I then scraped each match individually and obtained both the attack and defence, including goalkeeper, statistics for the home and away teams. I did this through pandas pd.read_html function as all the data was in a table format. Then using Selenium WebDriver I also scraped some metadata from each game. This included the captains, the referee officiating, the attendance and many others. This was for over 7500 games. 
This left me with 4 datasets, one for each of the attacking, defensive, and goalkeeping statistics 
of each player for every game and one for the metadata of each game. 
The Notebook can be found here - [FBREF Scrape](https://github.com/maxpardoe/capstone/blob/main/Clean%20Scrape.ipynb)

## Step 2 - Cleaning and Feature Engineering

### Cleaning 

Cleaning was a fairly straightforward task, due to the fact that I was scraping from other datasets. However, there were a few duplicated games that were scraped, some of which that had different game IDs. This would cause problems had I not discovered this as it would have meant that players form would have been incorectly judged. However, I was able to remove these duplications. There were also some players who, for every game they played, only had NaN values. Due to this I decided they were irrelvant and dropped all of these players from both the defence and attack dataframes. 

### Feature Engineering

Now that I had clean dataframes I had to obtain the form of the players. I did this through creating the exponentially weighted mean of each player’s in-game statistics over their last 10 games. I was able to do this through grouping by each unique player in my dataset and iterating through, gathering the EWM of all their in game stats and then joining their non-numeric stats (name, team etc.) back. I also shifted down their form so their form is from previous games.  
Now that I had the form of each player I then concatenated each player back into their respective dataframes, so for the attacking stats they would go back into the attacking dataframe etc. After that I categorised the position of each player so that the positions were in a similar order. I then grouped by team and game ID and took the most attacking and most defensive 7 players from each team. 
My next step was to set it out so that each row was in a head to head format, ie attack vs defence plus metadata on each row. I did it through this function below. 

![Screenshot 2021-03-10 at 19 07 04](https://user-images.githubusercontent.com/74013880/110784843-16736800-8262-11eb-8c64-5965ce9a8fe6.png)

What this function does is it groups by game_id and team, I then .ravel() each group, in this case each group is a team that played in that game, and appended it to a list. This list then consists of all the player data from each game along with the group name. As we don’t need the group name (team name and game ID) I index in and append just the statistics to another list. I then created a data frame with each team as one line and then concatenated all the teams to this data frame. 
My full cleaning feature engineering notebook can be found [here](https://github.com/maxpardoe/capstone/blob/main/feature_engineering.ipynb)

## Step 3 - Exploratory Data Analysis 

### Home Advantage

Now that I had my fully sorted dataframe I was able to have a look at what features could lead to a clean sheet. I found that there was quite a large difference between the chances of keeping a home clean sheet and an away clean sheet. There was also quite a large variation in that likelihood between the 5 leagues, with the likelihood of keeping an away clean sheet in the MLS being the most unlikely. I concluded that the potential reason for this being due to the distance that each team would have to travel would be considerably greater than that in other leagues so travel time and logistics are much smaller and far simpler than that in US. Moreover, more often than not there are no away fans that travel, and so there is no support for th away team, thus, creating an even greater home advantage.<img width="893" alt="home:away_base" src="https://user-images.githubusercontent.com/74013880/111187625-1352e180-85ac-11eb-9684-bf3715bb94ab.png">



## Step 4 - Predictive Modeling

In order to include my metadata, which is in string format, I dummified the features, these included the manager, the teams, the referee and others. 
I first started with a simple logistic regression model. However, these were either not reaching or were below baseline. 
I moved on to decision trees. This showed some promise as it brought my score closer to the baseline after implementing an AdaBoost. I then put in place some principal component analysis and it took the results above the baseline to 0.7351. 

For Logistic Regression Adaboost [notebook](https://github.com/maxpardoe/capstone/blob/main/LR%20PCA.ipynb)

For DecisionTree AdaBoost [notebook](https://github.com/maxpardoe/capstone/blob/main/Final%20Modelling.ipynb)

## Step 5 - Conclusion 

In conclusion, I am able to say that I have been able to achieve my project goal - to predict clean sheets above the baseline rate, even though it was only a little increase on the baseline. Some things that I would do differently is that I would perhaps try and get more meta-data, such as, whether or not the game was a derby match. I would also have liked to obtain the league positions of each team at the point of the game, as a big difference between the two teams could have had an effect on the clean sheet outcome. 

## Step 6 - Evaluation 

Overall, I believe that this was a successful project as I was able to achieve my goal of predicting clean sheets above the baseline rate. However, disappointingly I was unable to factor in certain features that I would have liked to. For example, I was unable to feature the captain data that I scraped for each match, due to my limited time frame I was unable to appropriately engineer a way of to fit this data into my model. However, this is something that I would like to add onto during my future developments.

Moreover, if I were to do this again, I would also try and attempt to try and engineer a way of creating a feature that relays the difference in points/table positioning during that particular game week. This is because I feel this could be another strong indication as to whether a team would be more likely to achieve a clean sheet or not. 
![image](https://user-images.githubusercontent.com/74013880/112026694-5f62d080-8b2e-11eb-91d0-b2eaea69fb94.png)

## Key Learnings 

### Functions
Creating appropriate functions in order for me to come back to parts later and easily repeat processes without having to re-order my code. This is crucial as it saves valuable time.

### Appropriate planning
Another important lesson that I can take from this project is thoroughly plan how you want the data to look before gathering the data. This saves valuable time on feature engineering and cleaning of the data. This also constitutes of what you are wanting to scrape and so in order to make sure to not have regrets of not scraping something gather as much information as possible and so can always remove data if not needed. 




