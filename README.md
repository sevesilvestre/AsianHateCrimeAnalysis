<h1 align="center">Asian Hate Crime Data Analysis Project</h1>

## Click [here](https://public.tableau.com/app/profile/seve.silvestre/viz/AsianHateCrimeCaseStudy/AsianHateCrimeStory?publish=yesLink) for the Tableau Story:</h2>
<p align="center">
  <a href="https://public.tableau.com/app/profile/seve.silvestre/viz/AsianHateCrimeCaseStudy/AsianHateCrimeStory?publish=yesLink"> <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/title3.png"/></a>
</p>

## Overview:
As a Japanese-Filipino-American, I was curious to delve into a personal topic that I wanted to learn more about: the state of Asian hate crimes in the United States. In the past few years, there has been increased media coverage on the rise of Asian hate crimes nationwide, and I decided to use data on hate crimes to answer some questions I have always had.

## Key Findings:
- Asian hate crimes increased by 76.6% from 2019 to 2020 in the United States
- "Intimidation" and "Simple Assault" Offenses increased by 91% and 169% respectively from 2019 to 2020
- New York City saw the highest increase in Asian hate crimes with a 1,300% increase from 2019 to 2020
- Population Size proved to have a decently strong relationship to the number of Asian hate crimes while population density proved to have a weaker relationship

### The questions that I aimed to address through my data project are:

- Did the introduction of COVID-19 affect the number of Asian hate crimes in 2020?
- What was the percentage increase in Asian hate crimes from 2019 to 2020?
- What is the rate of reporting for Asian hate crimes compared to all other crimes?
- What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?
- Which state and city had the largest number of Asian hate crimes since the start of the COVID-19 pandemic?
- What is the nature of reported Asian hate crimes?
- Are there any patterns or trends in the location of Asian hate crimes?
- Is there a correlation between population size/density and the number of Asian hate crimes that occur?

Overall, this data project aims to provide insights and shed light on the recent state of Asian hate crimes in the United States.

## Data Gathering
#### With these questions I had, I downloaded 3 datasets to help me answer these questions:
1. **[Hate Crime Statistics Dataset](https://cde.ucr.cjis.gov/LATEST/webapp/#/pages/downloads%23datasets)** (Soruce: FBI Crime Data Explorer)
 - Provides data on all hate crimes from 1991-2023
2. **[2020 US Cities Population Dataset](https://www.census.gov/data/tables/time-series/demo/popest/2020s-total-cities-and-towns.html)** (Source: U.S. Census Bureau)
 - Provides data on US city population to gain insight on the relationship between population size and Asian Hate Crime
3. **[US. Cities Population Density Dataset](https://www.kaggle.com/datasets/mmcgurr/us-city-population-densities)** (Source: Kaggle)
 - Provides data on US city land size to gain insight on the relationship between population density and Asian Hate Crime

## Data Cleaning + Data Manipulation
Once I downloaded my three datasets, I loaded them into DBeaver and utilized SQLite to query my datasets using a combination of Data Definition Language (DDL), Data Manipulation Language (DML), and Data Query Language (DQL) techniques to create workable tables that I could upload into Tableau. During this process, I also performed multiple JOIN operations in order to combine my three datasets together.

Here is an example of a query that I performed to join my datasets together and provide insights on the correlation between population density and the number of Asian hate crimes:

```
ALTER TABLE uscitypopdensity ADD COLUMN city_state TEXT;

UPDATE uscitypopdensity SET city_state = City || ',' || State;

SELECT hc.incident_id AS 'Incident ID',
	hc.data_year AS 'Year',
	hc.offense_name AS 'Offense',
	hc.bias_desc AS 'Bias',
	hc.city_state AS 'City, State',
	c.POPESTIMATE2020 AS 'Population',
	u."Land Area (Square Miles)",
	(c.POPESTIMATE2020/u."Land Area (Square Miles)") AS "Population Density"
FROM hate_crime hc
INNER JOIN citypopulation c ON hc.city_state = c.city_state
INNER JOIN uscitypopdensity u ON c.city_state = u.city_state
WHERE data_year >= 2019 AND bias_desc = "Anti-Asian" AND c.POPESTIMATE2020 != 0
GROUP BY hc.incident_id, hc.city_state, hc.data_year, hc.offense_name, hc.incident_id, hc.bias_desc, c.city_state, c.POPESTIMATE2020, u."Land Area (Square Miles)";
```
The rest of SQL queries that I performed can be accessed [here](https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/SQL%20Queries/Asian%20Hate%20Crime%20SQL%20Queries.txt).

## Data Visualizing
Now that the queries have been performed and exported as an Excel file, it's time to load them into Tableau and begin visualizing the data - my favorite part! After creating multiple dashboards and compiling them into a cohesive storyboard, here are the results to the questions I had regarding Asian hate crimes from 2019 to 2020:

### Did the introduction of COVID affect the # of Asian Hate Crimes in 2020?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/%23ofCrimes.png">
</p>

According to the data, there was an increase in Asian Hate Crimes by **144** from the years 2019 and 2020. Asian Hate Crimes were under **200** for the 4 years prior to COVID-19 and 2 years within COVID, the # of Asian Hate Crimes increased to over **300** per year. 

### What was the percentage increase in Asian Hate Crimes from 2019 to 2020? What is the rate of reporting for Asian hate crimes compared to all other crimes?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/%25ofCrimes.png">
</p>

Along the same note, there was a **76.6%** increase in Asian Hate Crimes from 2019 to 2020 as well. Comparing the # of Asian Hate Crimes to all other Hate Crimes, the percentage of Asian Hate Crimes to the total number of hate crimes also increase from **2.39%** in 2019 to **3.37%** in 2020. 

### What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/State.png">
</p>

California had the highest number of Asian Hate Crimes in 2020 with **90** hate crimes, increasing by **109%** from Pre-COVID times. 

<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/Cities.png" >
</p>

When looking at cities across the U.S., cities that had the highest increase in Asian Hate Crimes within the one year span include New York City with a **1,300%** increase, Los Angeles with a **167%** increase, and San Jose with a **125%** increase. 

### What is the nature of reported Asian hate crimes?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/offense2.png">
</p>

The most committed offense across the two years include "Intimidation" and "Simple Assault". "Intimidation" saw a **91%** increase in 2020 and "Simple Assault" saw a **169%** increase in the COVID Era. 

### Are there any patterns or trends in the location of Asian hate crimes?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/location2.png">
</p>

"Highways/Roads/Alleys/Streets" was the location that had the largest # of Asian Hate Crimes in 2019 and the story was the same in 2020. However, there was a **75%** increase in Asian Hate Crimes that occurred at "Highways/Roads/Alleys/Streets". "Residence/Home" had the second largest # of Asian Hate Crime occurrances in 2019 and saw a **115%** increase in 2020.

### Is there a correlation between population size/density and the number of Asian hate crimes that occur?
<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/popsize1.png">
</p>

The population of a city showed a decent relationship between the population size and the # of Asian Hate Crimes, possessing an R2 of **0.62**. The closer the R2 is to 1, the variables move together in the same direction. The closer R2 is to -1, the variables move in opposite directions. The closer R2 is to 0, the more independent both variables are to each other.

<p align="center">
  <img src= "https://github.com/sevesilvestre/AsianHateCrimeAnalysis/blob/main/images/popdensity1.png">
</p>

On the other hand, population density and the # of Asian Hate Crimes had a weaker relationship between each other with the R2 being **0.33**. 

## Conclusion
In conclusion, the data analysis indicates a concerning trend of rising Asian hate crimes in the United States during the COVID-19 pandemic. There was a 76.6% increase in Asian hate crimes from 2019 to 2020 and the percentage of Asian hate crimes compared to all crimes increased from 2.39% to 3.37%. The number of offenses committed as an Asian hate crime increased as a whole as well, seeing offenses such as "Intimidation" and "Simple Assault" increase by 91% and 169%, respectively. Geographical data also showed that Asian hate crimes across the country increased with states like California increasing by 109% and cities like New York City increasing by 1,300%. This data analysis also concluded that over the 2 year span, the population of a city had a fairly strong relationship to the number of Asian hate crimes while population density had a weaker relationship.

While the main reasons behind this increase may vary, it is clear that more attention and efforts are needed to combat hate crimes against Asian Americans. It is also important to note that the data may not fully capture the extent of hate crimes due to underreporting since not all hate crimes are reported to law enforcement, and not all law enforcement agencies report hate crimes to the FBI, which collects and publishes national hate crime statistics. As a society, we must strive towards creating a safe and inclusive environment for all communities.
