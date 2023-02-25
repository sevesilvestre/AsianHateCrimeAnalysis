# Asian Hate Crime Analysis
## Overview:
### As an Japanese-Filipino-American, I was curious to dive into a personal topic that I wanted to learn about: the state of Asian Hate Crimes. In the past few years, there has been more coverage on the increase on Asian Hate Crimes nationwide and I decided to use data on hate crimes to answer some questions I have always had:

- Did the introduction of COVID affect the # of Asian Hate Crimes in 2020?
- What was the percentage increase in Asian Hate Crimes from 2019 to 2020?
- What is the rate of reporting for Asian hate crimes compared to all other crimes?
- What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?
    - Which state and city had the largest number of Asian hate crimes since COVID?
- What is the nature of reported Asian hate crimes?
- Are there any patterns or trends in the location of Asian hate crimes? 
- Is there a correlation between population size/density and the number of Asian hate crimes that occur?

## Data Gathering
#### With these questions I had, I downloaded 3 datasets to help me answer these questions:
1. Hate Crime Statistics Dataset (Soruce: FBI Crime Data Explorer)
 - Provides data on all hate crimes from 1991-2023
2. 2020 US Cities Population Dataset (Source: U.S. Census Bureau)
 - Provides data on US city population to gain insight on the relationship between population size and Asian Hate Crime
3. US. Cities Population Density Dataset (Source: Kaggle)
 - Provides data on US city land size to gain insight on the relationship between population density and Asian Hate Crime

## Data Cleaning + Data Manipulation
Once I had downloaded my 3 datasets, I loaded them into DBeaver and used SQLite to utilize a combination of Data Definition Language (DDL) and Data Manipulation Language (DML) techniques to query my datasets to provide me with workable tables to upload into Tableau. During this provess, I also performed multiple JOIN operations in order to join my 3 datasets together. Here is an example of query I performed in order to get my datasets joined together to provide me insights on population density vs. # of Asian Hate Crimes:

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
## Data Visualizing
Now that my queriues have been performed and exported in the form of an Excel file, it's time to load them into Tableau and begin data visualizing, my favorite part. After creating multiple dashboard and compiling them into a cohesive storyboard, here are the results to the questions I had regarding Asian Hate Crimes in 2019 to 2020:


### Did the introduction of COVID affect the # of Asian Hate Crimes in 2020?
According to the data, there was an increase in Asian Hate Crimes by 144 from the years 2019 and 2020. Asian Hate Crimes were under 200 for the 4 years prior to COVID-19 and 2 years within COVID, the # of Asian Hate Crimes increased to over 300 per year. 

### What was the percentage increase in Asian Hate Crimes from 2019 to 2020? What is the rate of reporting for Asian hate crimes compared to all other crimes?
Along the same note, there was a 76.6% increase in Asian Hate Crimes from 2019 to 2020 as well. Comparing the # of Asian Hate Crimes to all other Hate Crimes, the percentage of Asian Hate Crimes to the total number of hate crimes also increase from 2.39% in 2019 to 3.37% in 2020. 

### What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?

California had the highest number of Asian Hate Crimes in 2020 with 90 hate crimes, increasing by 209% from Pre-COVID times. 

When looking at cities across the U.S., cities that had the highest increase in Asian Hate Crimes within the one year span include New York City with a 1,400% increase, Los Angeles with a 267% increase, and San Jose with a 225% increase. 

### What is the nature of reported Asian hate crimes?

The most committed offense across the two years include "Intimidation" and "Simple Assault". "Intimidation" saw a 191% increase in 2020 and "Simple Assault" saw a 269% increase in the COVID Era. 

### Are there any patterns or trends in the location of Asian hate crimes?

"Highways/Roads/Alleys/Streets" was the location that had the largest # of Asian Hate Crimes in 2019 and the story was the same in 2020. However, there was a 175% increase in Asian Hate Crimes that occurred at "Highways/Roads/Alleys/Streets". "Residence/Home" had the second largest # of Asian Hate Crime occurrances in 2019 and saw a 215% increase in 2020.

### Is there a correlation between population size/density and the number of Asian hate crimes that occur?

The population of a city showed a decent relationship between the population size and the # of Asian Hate Crimes, possessing an R2 of 0.62. On the other hand, population density and the # of Asian Hate Crimes had a weaker relationship between each other with the R2 being 0.33. 

## Conclusion
Overall, the data shows that there seems to have been an overall increase in Asian Hate Crimes from 2019 to 2020. The introduction of COVID does seem to correlate to how many Asian Hate Crimes are occurring on a yearly basis. The number of Asian Hate Crime offenses have increased significantly as well as the number of Asian Hate Crimes in cities with larger population sizes.

