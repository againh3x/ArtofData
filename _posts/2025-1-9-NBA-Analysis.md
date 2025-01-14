
I am generally very interested in basketball and the NBA, so I thought it would be fun to look at the data behind the basketball games I watch. I found a dataset on Kaggle called "NBA Player Data (1996-2024)."  
[Dataset URL](https://www.kaggle.com/datasets/damirdizdarevic/nba-dataset-eda-and-ml-compatible?select=final_dataset_master.csv)

The dataset just has one large csv with data corresponding to an NBA player for a specific season. Thus, the amount of rows dedicated to one player would be equal to the amount of seasons they played.

I was really interested in how a player's draft number affected their career statistics, which include average points, rebounds, and assists. I also thought it would be interesting to include their free throw and turnover percentages. I also included their height (cm) and weight (kg). In this case, their turnover percentage denotes a percentage of the entire team's turnovers. Their free throw percentage is just the total makes from all FT attempts.

### Data Cleaning and Transformation Steps

There were two things I needed to accomplish in order to clean my data and begin my analysis and visualization.

1. **Handling Undrafted Players**  
   A player can go "undrafted," which means that they joined the NBA some other way than being formally drafted. However, this usually means a pretty low draft class, and the lowest you can be drafted is 60th, so I just replaced all players who were undrafted with 61 as their draft number.
   ```python
   df["draft_number"] = df["draft_number"].replace("Undrafted", 61)
   ```

2. **Aggregating Player Data**  
   Next, I needed to format the data so I have one row per player, as I was more interested in analyzing each player's career averages as a data point as opposed to per season. To do this, I grouped the dataframe by the column `normalized_name` (player names), and used pandas' `.agg()` function to specify which aggregation operations to apply to each column for each group of the dataframe. Then, I reset the index to have "normalized_name" as a column again.
   ```python
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
   ```

### Data Analysis Insights

Interestingly enough, the data type for the draft_number column was still an object, so I used this pandas command to make them numeric:
```python
player_averages['draft_number'] = pd.to_numeric(player_averages['draft_number'], errors='coerce')
```

After ending up with my new dataframe called `player_averages`, I used pandas' `describe()` function to summarize the data and give important information like IQR, std, mean, min/max, etc.

Something that really stood out to me was that both the mean and median (50%) of points, assists, and rebounds were very low. The median career average for points per game was only 4.65 ppg, which seems way lower than expected when you see NBA superstars having consistent 40-point games all the time.

Because of this, you might expect the mean to be a significant amount higher, but the mean ppg was still only 5.9 ppg even with the 40+ points from the best players. One thing I found "fishy" was that one singular player had a career average of 100% of his team's total turnovers. It may be that he only played one game and had all the turnovers, but this is interesting.

Even more shocking was the career average assists per game, with barely over one assist being the mean. In addition, a mean free throw percentage of only 0.64 felt extremely low from the NBA. I wonder how different the numbers would look if I took a player's best seasons instead of their career averages.

When analyzing the box plots of each variable, I was surprised to  see that a select few players (<20), had a draft number greater than 61 (undrafted), which should not be possible. The box plot of draft number is featured below, where you can see the outer quartile is not on 61 as it should be: 

![Draft Number Box Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.23.36 AM.png' | relative_url }})

After making a new dataframe to view all players with draft numbers >61, I found that the player with the highest draft number was "Charles Jones" with 165 draft number which makes no sense. I looked him up on google, and it confirmed his draft number was #165 in the 1979 NBA Draft. After some digging, I found that before 1989, NBA drafts could have up to 7 rounds, which means a whopping 210 maximum draft number. In 1984, there was a whopping 300 picks. Therefore, I didn't remove Charles Jones or any other player from the dataset, because this was their true draft number. Also, the reason so few players had this draft number was because the dataset only goes back to 1996, where not a lot of players would have been drafted in a late round before 1989. 


### Correlation Analysis

Next, in order to compare variables and analyze correlations, I created a correlation matrix with all variables. I then unstacked that correlation matrix, filtered out duplicate pairs of variables, created another column with the absolute values of the correlation coefficient, and then used that column to sort the pairs in terms of highest correlation. I then displayed the new correlation data.

```python
correlation_matrix = player_averages[['player_height', 'player_weight', 'pts', 'reb', 'ast', 'FT.', 'TOV.', 'draft_number']].corr()

print("Correlation Matrix:")
display(correlation_matrix)

# Unstack and turn into a longer format
correlation_pairs = correlation_matrix.unstack().reset_index()

# Rename some columns
correlation_pairs.columns = ['Variable 1', 'Variable 2', 'Correlation']

# Remove duplicates
correlation_pairs = correlation_pairs[correlation_pairs['Variable 1'] < correlation_pairs['Variable 2']]

# Make absolute value column to make sorting easier and see "strength" of the correlations
correlation_pairs['Abs Correlation'] = correlation_pairs['Correlation'].abs()

# Sort
correlation_pairs = correlation_pairs.sort_values(by='Abs Correlation', ascending=False)

print("Strongest Correlations:")
display(correlation_pairs)
```

![Points vs. Assists Regression Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.08.28 AM.png' | relative_url }})
![Points vs. Rebounds Regression Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.08.56 AM.png' | relative_url }})
![Draft Number vs. Assists Regression Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.09.07 AM.png' | relative_url }})
![Draft Number vs. Points Regression Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.09.20 AM.png' | relative_url }})
![Draft Number vs. Rebounds Regression Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.09.39 AM.png' | relative_url }})
![Player Weight vs. Assists Plot]({{ '/assets/img/Screenshot 2025-01-14 at 8.09.56 AM.png' | relative_url }})
![Free Throw % vs. Assists]({{ '/assets/img/Screenshot 2025-01-14 at 8.10.12 AM.png' | relative_url }})

### Interpretation

Out of all the correlation pairs, player_height and player_weight had the highest correlation of 0.81. This is to be expected, as the taller a player is, the more mass he will have and the heavier he will be. There were also strong positive correlations between points and assists, as well as points and rebounds. This makes sense, as the more points a player scores, the better he is, meaning he will probably play longer in the game and rack up more rebounds and assists. Also, he will just be more skilled and be able to get rebounds and assists easier.

I was really curious about how a player's draft number indicated their career average points/rebounds/assists since there have been countless examples in the past of second-round draft picks (30+ draft number) being the best players. For instance, arguably the best player in the NBA, Nikola Jokic, was drafted 41st overall. I was curious whether I would still see a correlation with draft number and career averages. The results were as follows: draft number had a moderately strong negative correlation with all points, rebounds, and assists (-0.54, -0.48, and -0.35). This means that as players were picked later in the draft, their career averages in all three categories would decrease. It is a negative correlation because being picked "lower" is still indicated by a "higher" draft number. For instance, if a player was picked first overall, a player picked 40th would have a "higher" draft number. This made sense and showed me that even the star players who were picked low in the draft aren't enough to change the overall rule.

It was also nice to see that as a player got taller, their career assists would go down (negative correlation), which makes sense as usually taller players focus on rebounding and less on passing. I did find a positive correlation with player height/weight and rebounds as well.

Another interesting correlation I found was with assists and free throw percentages (0.35). My best explanation for this is that usually players with higher career assist averages tend to be guards or positions that rely more on skill than size, which could explain being better at shooting free throws. I didn't expect to see that one.

I didn't really find much correlation for any variables paired with a player's turnover percentage, nor did I find any correlation with a player's height/weight and their career points. A lot of people have the idea that being taller results in scoring more points, but in fact, there was a very small negative correlation between the two in my data (-0.04). This was really interesting to see, and my guess is that to a certain level of basketball like the NBA, height does not matter much as everybody is already so tall, and it's more about skill and how you fit in with your team. I'm sure this will be a surprising lack of correlation to a lot of people who believe otherwise.

### Conclusion

It was cool to see some misconceptions like draft number not mattering and also height correlating to points being proven wrong. The real value of a large data analysis like this is simply remaining impartial and seeing a much bigger picture than the average NBA fan who only sees a couple of examples and may choose to generalize to the rule.

In sum, I think my biggest lesson from this lab, aside from the power of having large amounts of data, is understanding truly how hard it is to succeed in the NBA. We spend so much time watching the top 1% of NBA players that we forget the seemingly mediocre stats of an average NBA player, who might not even play in the game at all. This really made me appreciate how talented those top 1% of NBA players are, as they are outliers in the hardest basketball league in the world.
