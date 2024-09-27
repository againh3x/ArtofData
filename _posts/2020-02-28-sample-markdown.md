---
layout: post
title: Digimon Lab Blog Post
subtitle: My process in solving the three questions
author: Arda Altintepe
---


## Read CSV

I first opened the csv and used dictreader to read it. I then converted it into a nested list, which I could later use to extract attributes of specific digimon.

| Digimon  | Stage | Type  | Attribute | Memory | Equip Slots | HP  | SP  | Atk | Def | Int | Spd |
|----------|-------|-------|-----------|--------|-------------|-----|-----|-----|-----|-----|-----|
| Kuramon  | Baby  | Free  | Neutral   | 2      | 0           | 590 | 77  | 79  | 69  | 68  | 95  |
| Pabumon  | Baby  | Free  | Neutral   | 2      | 0           | 950 | 62  | 76  | 76  | 69  | 68  |
| Punimon  | Baby  | Free  | Neutral   | 2      | 0           | 870 | 50  | 97  | 87  | 50  | 75  |
| Botamon  | Baby  | Free  | Neutral   | 2      | 0           | 690 | 68  | 77  | 95  | 76  | 61  |


## Summing Digimon HP and Storing Data

Next, I looped through the data, extracting each Digimon's name, memory, and attack. While looping through, I also added up all the HP values to calculate the total HP of all Digimon.

```python
 for row in data:
      digimon_data[row['Digimon']] = {'Memory': int(row['Memory']), 'Atk': int(row['Atk'])}
      Total_HP = Total_HP + int(row["HP"])
```

## Function to count Digimon of specific attribute

I wanted to create a function to count how many Digimon matched a specific attribute and value. This function, count_digimon, takes the data, the attribute to check, and the value to look for. It loops through the data and counts how many Digimon have the specified value for the given attribute. One thing I struggled with here was getting the int and string types to match, because the value inputted is a string. Luckily, a quick solution was to just wrap the value in str() braces. 

```python
def count_digimon(data, attribute, value):
    count = 0
    for row in data:
        if row[attribute] == str(value):
            count += 1
    return f'there are {count} digimon with a value of {value} for {attribute}'
```

# The Third Problem

The hardest part of this lab was by far the third problem, in which the problem asked to find **a** team of digimon with a combined memory of at most 15, and a combined attack power of at least 300. 
I first approached this problem normally, where I returned the function as soon as a team of digimon with combined memory under or equal to 15 and attack power >= 300 was found. 
I did this through creating three nested for-loops on the stored data, where I converted all digimon indexes to names, and used these names to loop through the datat three times and assemble a team of 3 that fit the criteria. Throughout this process, I stored the memory and attack of each digimon in the team, as well as the totals. My approach worked well, and after a lot of thinking, I was able to get the function to return the first digimon trio that satisfied the criteria. 

However, a more challenging approach to this problem would be to return the team with **_the highest attack power_**, rather than just the first team that answered the question. To do this, I added the best_team and best_attack variables, and once a team of three was assembled, I constructed an if-statement that checks if it satisfies the memory criteria and if the total attack is greater than the best_attack, or a new "high score." If so, I set best_team to a dictionary of the digimon names, as well as total attack power and memory of the team. At the end, the function returns the best_team variable. 

One problem with this code is that it is inefficient, and runs through every single possible combination of teams of all digimon. When I was thinking of ways to fix this, I remembered the memory criteria, and that all digimon teams must be under 16 memory, because it is an integer. Also, if a digimon has 12 or over memory, they automically could just be discarded from the for loops, since the lowest memory of all digimon is 2, meaning the lowest possible memory for a trio would be 16 assuming one digimon has memory 12. Therefore, I set the digimon names list to only include digimon with a memory less than 12. If I had more time, I would automate this based on the lowest memory in the dataset. Specifically, I would look at the 2 lowest memory values in the dataset, and write code to only keep the digimon with memory values less than or equal to 15 - ((lowest value) + (second lowest value)). In this case, the lowest and second lowest values are both 2, so the highest possible memory a digimon could have is 11 (15- (2+2) = 11).

```python
# Function to find the team of up to 3 Digimon with the highest attack and with total memory <= 15 
def find_team(data, max_memory=15):
    
  digimon_names = list(data.keys())  # List of all Digimon names
  
  #Removes all digimon with 12 and over memory (run-time goes down from 0.4s to 0.0s)
  digimon_names = [name for name in digimon_names if data[name]['Memory'] < 12]
  
  #Inititialize best team and best attack variables
  best_team = None
  best_attack = 0

 #First loop through all digimon, stores attributes to memory_1 and attack_1 and name to digimon_1
  for x in range(len(digimon_names)):
    
      digimon_1 = digimon_names[x]
      memory_1 = data[digimon_1]['Memory']
      attack_1 = data[digimon_1]['Atk']
     #Second loop through all digimon, testing adding a combination of all second digimons to that first one's attributes and storing under total_memory_2 and total_attack_2
      for y in range(x + 1, len(digimon_names)):
          digimon_2 = digimon_names[y]
          memory_2 = data[digimon_2]['Memory']
          attack_2 = data[digimon_2]['Atk']
         
          total_memory_2 = memory_1 + memory_2
          total_attack_2 = attack_1 + attack_2
         #Third and final loop through all digimon, adding a third membor to the team of 2 and storing all new attack and memory to the new total
          for z in range(y + 1, len(digimon_names)):
              digimon_3 = digimon_names[z]
              memory_3 = data[digimon_3]['Memory']
              attack_3 = data[digimon_3]['Atk']
              total_memory_3 = total_memory_2 + memory_3
              total_attack_3 = total_attack_2 + attack_3

             #If-statement to check if the memory is under 15 and the total attack is a new high score for those under 15.
              if total_memory_3 <= max_memory and total_attack_3 > best_attack:
                # Update the new best team if this one has a higher attack value
                best_attack = total_attack_3
                #store it to a dictionary for that team
                best_team = {'Best Team': (digimon_1, digimon_2, digimon_3), 'Total Memory': total_memory_3, 'Total Attack': total_attack_3}
                
  return best_team
```
