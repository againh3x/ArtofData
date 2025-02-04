# Election Map Visualization: Recreating the New York Times Graph

Hello! To begin, I went to the New York Times graphs to find a graph to emulate, and immediately I was faced with a large and very detailed election map. I ended up choosing that as my first graph.

The graph shows each county in each state and illustrates the margins for that county and the state in general. Democrat states are blue and Republican states are red, by county. I think this graph is important due to the election being a really significant time in everyone’s lives as a democracy, where half the population literally votes as well.

I really like this graph because it's not one of the conventional graphs you see all the time like a bar graph or scatter plot, but it's unique to the United States and important to millions in the country. I like the idea of having states as borders. In this graph, some marks would be the literal borders of the shape, or the geoshape, as well as possibly the text pertaining to and annotating each state.

Some channels are color for the party as well as opacity for the margins.

![NYT Election Map]({{ '/assets/img/Screenshot 2025-02-03 at 11.28.32 PM.png' | relative_url }})
---


## LA Wildfires Map

Another interesting graph that I found was of the LA wildfires map. Again, the LA wildfires are an important issue in current events and have a lot of significance for people affected by them. This graph can tell people where the fires are the worst and can actually influence big decisions in people’s lives. 

The graph shows a map of regions near LA, with some county borders and geographical details like mountains. The graph uses color channel to represent recent and active fires for certain areas. Some marks include borders, geographic details, and map annotations.

![NYT LA Wildfires]({{ '/assets/img/Screenshot 2025-02-03 at 11.38.49 PM.png' | relative_url }})
---

## Creating the Election Map

I think both graphs were pretty similar, but I attempted to recreate the election map graph. I first found a topology feature dataset from Vega datasets that contains each state and its borders. I also found a list of state IDs and names online that connect to their topology. I made this list into a dataframe and assigned a "party" value alternating each state index (Republican if even, Democrat if odd). 

The New York Times graph also uses margins for each state in addition to the party that won, so I created random margins for each state from 1-100. For Democrat states, I used -1 to -100. Now that I think about it, this is completely useless (negatives) for my final version of my graph, since I didn’t attempt the gradient legend that I was trying to do with the margins.

Until I figure that out, negative margins can be ignored. I created my chart next, where I set color (nominal) and opacity (quantitative) as two channels. Altair has a geoshape mark that was really useful to me and made my life a lot easier when doing this, so I could just plug in the Vega dataset and relax. 

I made the margin labels the first initial of the party + margin, because this is how the New York Times did it. I tried pretty hard to create a gradient legend where I combined opacity and color, to make one half red and the other blue, and have the gradient be opacity, but I just couldn't do it. 

I also tried making the margin associated with both color and opacity, where I was able to get a gradient legend, but because the gradient included color as quantitative, some states were shades of purple and it really looked nothing like an election map and more like an art project. Therefore, I just changed it back to without the gradient legend like the NY Times one has, and just put a legend for the nominal party feature instead. 

Overall, I would say this is a success, and I am happy with my work. I couldn't find much for the LA wildfires map, and it was hard to find a dataset of the topological area shown in the NYT graph. I think my election map should hopefully be satisfactory because it covers a lot of the same data visualization and Altair skills of working with geoshapes.



<head>
  <!-- Import Vega & Vega-Lite (does not have to be from CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>
</head>


<div id="vis"></div>

<script type="text/javascript">
  var spec = "us_election_map.json";
  vegaEmbed('#vis', spec).then(function(result) {
    // Access the Vega view instance (https://vega.github.io/vega/docs/api/view/) as result.view
  }).catch(console.error);
</script>

---

## Code for the Election Visualization

Here’s my code for the election visualization:

```python
import altair as alt
import pandas as pd
import random
from vega_datasets import data

#us states data from vega with real life border and names
states = alt.topo_feature(data.us_10m.url, 'states')

#list of state id and their names i found online
state_ids = [
    {"id": 1, "name": "Alabama"}, {"id": 2, "name": "Alaska"}, {"id": 4, "name": "Arizona"},
    {"id": 5, "name": "Arkansas"}, {"id": 6, "name": "California"}, {"id": 8, "name": "Colorado"},
    {"id": 9, "name": "Connecticut"}, {"id": 10, "name": "Delaware"}, {"id": 11, "name": "District of Columbia"},
    {"id": 12, "name": "Florida"}, {"id": 13, "name": "Georgia"}, {"id": 15, "name": "Hawaii"},
    {"id": 16, "name": "Idaho"}, {"id": 17, "name": "Illinois"}, {"id": 18, "name": "Indiana"},
    {"id": 19, "name": "Iowa"}, {"id": 20, "name": "Kansas"}, {"id": 21, "name": "Kentucky"},
    {"id": 22, "name": "Louisiana"}, {"id": 23, "name": "Maine"}, {"id": 24, "name": "Maryland"},
    {"id": 25, "name": "Massachusetts"}, {"id": 26, "name": "Michigan"}, {"id": 27, "name": "Minnesota"},
    {"id": 28, "name": "Mississippi"}, {"id": 29, "name": "Missouri"}, {"id": 30, "name": "Montana"},
    {"id": 31, "name": "Nebraska"}, {"id": 32, "name": "Nevada"}, {"id": 33, "name": "New Hampshire"},
    {"id": 34, "name": "New Jersey"}, {"id": 35, "name": "New Mexico"}, {"id": 36, "name": "New York"},
    {"id": 37, "name": "North Carolina"}, {"id": 38, "name": "North Dakota"}, {"id": 39, "name": "Ohio"},
    {"id": 40, "name": "Oklahoma"}, {"id": 41, "name": "Oregon"}, {"id": 42, "name": "Pennsylvania"},
    {"id": 44, "name": "Rhode Island"}, {"id": 45, "name": "South Carolina"}, {"id": 46, "name": "South Dakota"},
    {"id": 47, "name": "Tennessee"}, {"id": 48, "name": "Texas"}, {"id": 49, "name": "Utah"},
    {"id": 50, "name": "Vermont"}, {"id": 51, "name": "Virginia"}, {"id": 53, "name": "Washington"},
    {"id": 54, "name": "West Virginia"}, {"id": 55, "name": "Wisconsin"}, {"id": 56, "name": "Wyoming"}
]

#makae the fake election dataframe
election_data = pd.DataFrame(state_ids)

#make alternating parties for republican and democrat if state id is even or odd
election_data["party"] = ["Republican" if i % 2 == 0 else "Democrat" for i in range(len(election_data))]


#generate random margins from 1 to 99 for each republican state and -1 to -99 for each democrat state 
election_data["margin"] = [random.randint(1, 99) if i % 2 == 0 else -random.randint(1, 99) for i in range(len(election_data))]

#assign the margin to a new column using the party as the prefix
election_data["party_margin"] = election_data.apply(
    lambda row: f"{'D+' if row['party'] == 'Democrat' else 'R+'}{abs(row['margin'])}", axis=1
)

#create the chart
chart = alt.Chart(states).mark_geoshape(
    stroke='white',
    strokeWidth=0.5
).encode( 
     color=alt.Color(
        'party:N',  #make color the party
        scale=alt.Scale(
            domain=['Democrat', 'Republican'],  
            range=['#0057B8', '#D62728']  #blue dem red republican
        ),
        legend=alt.Legend(title='Party')  #make a legend cause i couldnt do the other one witha gradient
    
    ), #make states opacities depend on the magnitude of the margin
    opacity=alt.Opacity('margin:Q',
        scale=alt.Scale(domain=[-99, 0, 99], range=[1, 0.2, 1]), #make it 0.2 lowest cause 0 is way too white
        legend=None #i couldnt figure out how to make this work with altair for a "gradient" form 
    ),
    tooltip=['name:N', 'party:N', 'party_margin:N']
).transform_lookup(
    lookup='id',
    from_=alt.LookupData(election_data, 'id', ['name', 'party', 'party_margin', 'margin'])
).project(
    type='albersUsa'
).properties(
    width=800,
    height=500,
    title="US Election Map with Margins"
)

chart

```
