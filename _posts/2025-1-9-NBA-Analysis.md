I am generally very interested in basketball and the NBA, so I thought it would be fun to look at the data behind the basketball games I watch. I found a dataset on kaggle called "NBA Player Data (1996-2024)."
https://www.kaggle.com/datasets/damirdizdarevic/nba-dataset-eda-and-ml-compatible?select=final_dataset_master.csv
The dataset just has one large csv with data corresponding to an NBA player for a specific season. Thus, the amount of rows dedicated to one player would be equal to the amount of seasons they played.
I was really interested in how a players draft number affected their career statistics, which include average points, rebounds, and assists. I also thought it would be interesting to include their free throw and turnover percentages. 
In this case, their turnover percentage denotes a percentage of the entire team's turnovers. Their free throw percentage is just the total makes from all FT attempts. There were 2 things I needed to accomplish in order to clean my data and begin my analysis and visualization.
Firstly, a player can go "undrafted," which means that they joined the NBA some other way than being formally drafted. However, this usually means a pretty low draft class, and the lowest you can be drafted is 60th, so I just replaced all players who were undrafted with 61 as their draft number.
df["draft_number"] = df["draft_number"].replace("Undrafted", 61)
Next, I needed to format the data so I have one row per player, as I was more interesting in analyzing each player's career averages as a data point opposed to per season. To do this, I grouped the dataframe by the column normalized_name (player names), and used pandas' .agg() function to specify which aggregation opperations to each column for each group of the dataframe. Then, I reset the index to have "normalized_name" as a column again.
player_averages = df.groupby('normalized_name').agg({
    'player_height': 'mean',   
    'player_weight': 'mean',
    'draft_number': 'first',  
    'pts': 'mean',
    'reb': 'mean',
    'ast': 'mean',
    'FT.': 'mean',
    'TOV.': 'mean'
}).reset_index()

