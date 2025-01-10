I am generally very interested in basketball and the NBA, so I thought it would be fun to look at the data behind the basketball games I watch. I found a dataset on kaggle called "NBA Player Data (1996-2024)."
https://www.kaggle.com/datasets/damirdizdarevic/nba-dataset-eda-and-ml-compatible?select=final_dataset_master.csv
The dataset just has one large csv with data corresponding to an NBA player for a specific season. Thus, the amount of rows dedicated to one player would be equal to the amount of seasons they played.
I was really interested in how a player's draft number affected their career statistics, which include average points, rebounds, and assists. I also thought it would be interesting to include their free throw and turnover percentages. I also included their height (cm) and weight (kg). 
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
Interestingly enough, the data type for the draft_number column was still an object, so I used this pandas command to make them numeric: 
player_averages['draft_number'] = pd.to_numeric(player_averages['draft_number'], errors='coerce')

After ending up with my new dataframe called player_averages, I used pandas' describe() function to summarize the data and give important information like IQR, std, mean, min/max, etc. 
Something that really stood out to me was that both the mean and median (50%) of pts, assists, and rebounds were very low. In fact, the median career average for points per game was only 4.65 ppg, which seems way lower than expected when you see NBA superstars having consistent 40-point games all the time. Because of this, you might expect the mean to be a significant amount higher, but the mean ppg was still only 5.9ppg even with the 40+ points from the best players. Even more shocking was the assists per game, with barely over one assist being the mean. In addition, a mean free throw percentage of only 0.64 felt extremely low from the NBA. I wonder how different the numbers would look if I took a player's best seasons instead of their career averages. 

Next, in order to compare variables and analyze correlations, I created a correlation matrix with all variables. I then unstacked that correlation matrix, filtered out duplicate pairs of variables, created another column with the absolute values of the correlation coefficient, and then used that column to sort the pairs in terms of highest correlation. I then displayed the new correlation data. 













