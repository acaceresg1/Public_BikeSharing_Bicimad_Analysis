# Public_BikeSharing_Bicimad_Analysis

## Code & Resources used:

**Python Version:** 3.8.14

**Packages:** sci-kit learn, numpy, pandas, matplotlib, seaborn, igraph, folium

The following libraries were used to plot and represent the shortest route through Madrid:
- osmnx
- networkx

To use them, it is needed to create a separate environment within anaconda.

## Executive Summary:

A group project for Social Network Analysis final assignment at IE - University, Master in Business Analytics & Big Data

The goal of the project is to study the Bicimad service, the public bike-sharing service in Madrid, using network analysis to uncover any disregarded fact that could improve the service operation with a low investment.

The Bicimad Service data is public and available through the following link: https://opendata.emtmadrid.es/Datos-estaticos/Datos-generales-(1)

To put it in context, Bicimad Service has been running since 2014 with ups and downs, but with good public acceptance since the beginning.
The main problems to transform the mobility along Madrid are the quality of the service, as not always the bicycles are available, and the difficulties and stress that present biking through the car ways as there is not a proper bike path available in the main streets.
For users used to this road-sharing from the beginning, it is not a problem, but to engage new users and to show consistency and commitment to the environment, the city council should invest in new
bike paths along Madrid to foster the use of the service, reduce pollution and traffic jams, improving mobility overall.

The Madrid government budget is indeed limited, so it is important to optimize the service and the design of new bike paths.
Using network analysis using bicycle stations as nodes and trips as edges, makes it possible to study the service to optimize the new bike paths.
But the study also found operational recommendations, such as emptying/filling stations at some special hours using incentives.

All of this will be explained in detail, showing how a social network analysis improves and uncover insights.

The project is divided into:
- Data set description
- Time Series Exploratory Data Analysis
- Graphing Exploratory Data Analysis using Gephi
- Ride Distribution using folium
- Insights and Recommendations

## Data set description:

There are two main data sets for this project:

- Journey data: There is a unique JSON file per month in the open data web, so to develop the project was needed to merge 2 years of data, 24 JSON files, in a CSV file, from July 1st, 2019 to June 29th, 2021, counting more than 6.8 million of trips, with each record as a journey.
This file has information such as the ID unplug/plug station (from/to), the unplug time, and the travel time.
Because of the file size, it is not available within the documentation.

- Station data: This file is a unique CSV with the information of the 265 stations available. Each row is a station with its ID, District, Neighbourhood, Street, longitude, latitude, and Address.

Merging both datasets, it is possible to get a unique CSV with the information trip, the from/to, and the coordinates. These features will make possible the map representation of the trips in further steps.

## Time Series Exploratory Data Analysis:

Now it is time to explore the data using time series. The goal is to find significant variations using different time variables to understand better the dataset.
The distribution of rides over time was studied using weeks, months, days in a month, hours in a day, months on the dataset, and if was weekend or not.

This study helped to find some seasonal peaks through the year and also some abnormal events such as the pandemia and Filomena snowstorm. Both events stopped the service for weeks.
Also was found the user preferences during the weekend the user/travel and the ride's peak during some hours in the morning and the evening. Both insights show a pattern that the council could use later on.

A basic EDA shows 6,795 million trips, 265 stations, an average degree of 433 trips, a betweenness range of [0, 74.24], and a closeness range of [nan, 0.988].
This Basic EDA could be represented using Gephi, an SNA software, making possible the following graphs (They are not within the python notebook)

![image](https://user-images.githubusercontent.com/115701510/195985905-3f3448ba-c894-46b7-9ad7-c211a2afc154.png)

## Ride Trips:

Another variable studied was the travel time and the count of trips depending on their travel time. There is an exponential decreasing relationship between the mean journey
times and the trip count of each route. That means that the most used routes are on average shorter in trajectory.

Using folium, it was possible to plot the relevant routes with more than 150 use counts and also last more than 25 minutes on average in the period studied. In dark blue are the stations with a degree higher than 500 (75% percentile).
To see the interactive map, you should use the python notebook attached 

## Business Cases:

This study wants to uncover three main problems that the users of the service are suffering:

### 1) Ensuring availability bikes/parking
 
Ensuring supply and demand equilibrium for bikes across bike stations is fundamental for a smooth user experience. Ideally, no bike station should be too full 
that riders cannot find a free plug, and no bike station should be so empty that ride seekers cannot find a free bicycle. This puts pressure on the government 
to act quickly (either repleneshing empty stations or distributing bikes more evenly across stations) to ensure a smooth user experience.

So, how can be identified stations that have a supply/demand disiquelibrium?

**It is possible using three indicators / KPI's:**

**a) Station inflow per hour: Count of records for "idplug_station" for each hour**

**b) Station outflow per hour: Count of records for "idunplug_station" for each hour**

**c) Net station flow = station inflow - station outflow**

Identifying stations with extreme negative or positive net station flows for each hour of the day should foster the system to take action. 
It should refill those stations predicted as pontentially empty using bikes from those stations that are predicted s potentially full.

Studying past data, and using the KPI's, the system should predict at what time the stations are going to be empty/full and take action in advance to improve the service.
- NSF >> 0 : government should move bikes away
- NSF << 0 : government should add bikes

Comparing some stations is easy to see the possibilities the study has, as it could be use as precursor of a station status forecaster. The following figures compare
Net Station Flow between some selected stations, showing pikes on some of them when there is a minimum on others.

![image](https://user-images.githubusercontent.com/115701510/195995715-74256fa3-d02d-4d32-a1c9-205e26ce1456.png)

### 2) Improving movility:

The goal here is to recommend the local government main routes where they should built new bike paths. It is trying to maximize the number of trips through the lowest number of paths to disturb less than possible the road traffic and
pedestrians as new constructions could cause the Bicimad Service popularity decrease.

To create this recommendator, it is needed to find first the most important stations, neighbourhoods and districts.
The weight to take this decission is the number of trips, so those with a higher number of trips will have more importance.

Using together osmnx, networkx and folium libraries, it is possible to represent over a real Madrid map the shortest path between those station selected (using its coordinates).
It is true that the dataset has only the coordinates of the stations, but with some city knowledge it is possible to assign some coordinates to the main pin points of a neighbourhood, to represent the paths between two neighbourhoods.

The representation of this shortest paths between the sixteen more important neighbourhoods and all their stations (99 stations) is as follow.

![image](https://user-images.githubusercontent.com/115701510/195997182-1e3ecc22-bc59-4c1c-a204-f34a9aa92074.png)

#### The number of trips from-to those 99 stations to-from whatever count for 2.9 million of the total, so this map is potentially covering 43% of all the trips during two years!

So only with a small number of bike paths through those routes, could improve the city movility and engage more and more users. A potential recommendation would be as follow:

![image](https://user-images.githubusercontent.com/115701510/195998255-094673a9-2960-4f7e-b593-56b8b46f7c5b.png)

### 3) Self-loops and how to improve operations:

The last case finds self-loops and why they happen.
A self-loop is a trip with the same from/to, what could be intentional or not.

After checking the selfloops depending of its travel time, it is founded that 56% of those self loops are intentional trips. That could be a potentional insights for the service.

