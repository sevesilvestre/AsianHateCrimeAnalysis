# Asian Hate Crime Analysis
As an Japanese-Filipino-American, I was curious to dive into a personal topic that I wanted to learn about: the state of Asian Hate Crimes. In the past few years, there has been more coverage on the increase on Asian Hate Crimes nationwide and I decided to use data on hate crimes to answer some questions I have always had:

- Did the introduction of COVID affect the # of Asian Hate Crimes in 2020?
- What was the percentage increase in Asian Hate Crimes from 2019 to 2020?
- What is the rate of reporting for Asian hate crimes compared to all other crimes?
- What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?
    - Which state and city had the largest number of Asian hate crimes since COVID?
- What is the nature of reported Asian hate crimes?
- Are there any patterns or trends in the location of Asian hate crimes? 
- Is there a correlation between population size/density and the number of Asian hate crimes that occur?

With these questions I had, I downloaded 3 datasets to help me answer these questions:
1. Hate Crime Statistics Dataset (Soruce: FBI Crime Data Explorer)
 - Provides data on all hate crimes from 1991-2023
2. 2020 US Cities Population Dataset (Source: U.S. Census Bureau)
 - Provides data on US city population to gain insight on the relationship between population size and Asian Hate Crime
3. US. Cities Population Density Dataset (Source: Kaggle)
 - Provides data on US city land size to gain insight on the relationship between population density and Asian Hate Crime
 
 Once I had downloaded my 3 datasets, I loaded them into DBeaver and used SQLite to utilize a combination of Data Definition Language (DDL) and Data Manipulation Language (DML) techniques to query my datasets to provide me with workable tables to upload into Tableau. During this provess, I also performed multiple JOIN operations in order to join my 3 datasets together. Here is an example of query I performed in order to get my datasets joined together to provide me insights on population density vs. # of Asian Hate Crimes:

'''
(insert screenshot)
'''

Now that my queriues have been performed and exported in the form of an Excel file, it's time to load them into Tableau and begin data visualizing, my favorite part. After creating multiple dashboard and compiling them into a cohesive storyboard, here are the results to the questions I had regarding Asian Hate Crimes in 2019 to 2020:


