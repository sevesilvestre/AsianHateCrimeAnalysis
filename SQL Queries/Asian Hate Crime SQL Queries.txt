-- Dropping Unused Columns
ALTER TABLE hate_crime 
DROP ori, population_group_code;

-- What was the percentage increase in Asian Hate Crimes from the start of COVID to 2021? (time-series chart)
SELECT data_year, 
	COUNT(*) AS "# of Asian Hate Crimes"
FROM hate_crime hc 
WHERE bias_desc = "Anti-Asian"
GROUP BY data_year;

-- What is the rate of reporting for Asian hate crimes compared to all other crimes? 
SELECT data_year, 
	COUNT(*) AS "# of Asian Hate Crimes",
	(SELECT COUNT(*)
	FROM hate_crime hc2
	WHERE hc2.data_year >= 2016 AND 
	hc2.data_year = hc.data_year) AS "Total # of Cases",
	ROUND(100.0 * COUNT(*) / (SELECT COUNT(*)
                        FROM hate_crime hc3
                        WHERE hc3.data_year >= 2016 AND
                        hc3.data_year = hc.data_year), 2) AS "Percentage of Total"
FROM hate_crime hc 
WHERE data_year >= 2016 AND bias_desc = "Anti-Asian"
GROUP BY data_year;

-- What is the geographic distribution of reported Asian hate crimes in 2021 across different regions and cities in the U.S.?
    -- Which state had the largest/least number of Asian hate crimes since COVID? (geo map)
SELECT  data_year AS "Year",
state_name AS "State",
	COUNT(*) AS "# of Asian Hate Crimes",
	SUM(victim_count) AS "# of Victims"
FROM hate_crime hc
WHERE data_year >= 2019 AND bias_desc = "Anti-Asian"
GROUP BY state_name, data_year 
ORDER BY "# of Asian Hate Crimes" DESC;

	-- Which cities had the largest/least number of Asian hate crimes since COVID?
SELECT data_year AS "Year",
	state_name AS "State",
	pug_agency_name AS "City",
	COUNT(*) AS "# of Asian Hate Crimes",
	SUM(victim_count) AS "# of Victims"
FROM hate_crime hc
WHERE data_year >= 2019 AND bias_desc = "Anti-Asian"
GROUP BY pug_agency_name, state_name, data_year  
ORDER BY "# of Asian Hate Crimes" DESC;

-- What is the nature of reported Asian hate crimes?
    -- Which type of crime did Asians endure most?
SELECT data_year AS "Year", offense_name AS "Offense Name",
	COUNT(*) AS "# of Asian Hate Crimes",
	SUM(victim_count) AS "# of Victims"
	FROM hate_crime hc
WHERE data_year >= 2019 AND 
	bias_desc = "Anti-Asian"
GROUP BY offense_name, data_year 
ORDER BY "# of Asian Hate Crimes" DESC;


-- Are there any patterns or trends in the location of Asian hate crimes? (Days/Weeks)
SELECT data_year AS "Year", location_name AS "Location",
	COUNT(*) AS "# of Asian Hate Crimes",
	SUM(victim_count) AS "# of Victims"
FROM hate_crime hc
WHERE data_year >= 2019 AND 
	bias_desc = "Anti-Asian"
GROUP BY location_name, data_year 
ORDER BY "# of Asian Hate Crimes" DESC;

-- Is there a correlation between population # and the number of Asian hate crimes that occur? (histogram) (join data)
UPDATE citypopulation 
SET NAME = SUBSTR(NAME, 1, LENGTH(NAME)-5);

ALTER TABLE hate_crime ADD COLUMN city_state TEXT;

UPDATE hate_crime SET city_state = pug_agency_name || ', ' || state_name;

ALTER TABLE citypopulation ADD COLUMN city_state TEXT;

UPDATE citypopulation SET city_state = NAME || ', ' || STNAME;

SELECT hc.incident_id AS 'Incident ID',
	hc.data_year AS 'Year',
	hc.offense_name AS 'Offense',
	hc.bias_desc AS 'Bias',
	hc.city_state AS 'City, State',
	c.POPESTIMATE2020 AS 'Population'
FROM hate_crime hc
INNER JOIN citypopulation c ON hc.city_state = c.city_state
WHERE data_year >= 2019 AND bias_desc = "Anti-Asian" AND c.POPESTIMATE2020 != 0
GROUP BY hc.incident_id, hc.city_state, hc.data_year, hc.offense_name, hc.incident_id, hc.bias_desc, c.city_state, c.POPESTIMATE2020;

-- Is there a correlation between population density and the number of Asian hate crimes that occur? (histogram) (join data)
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


-- How many Asian Hate Crimes were performed by solo offenders vs multiple offenders?
SELECT data_year AS "Year", total_offender_count AS "# of Offenders",
	COUNT(*) AS "# of Asian Hate Crimes",
	SUM(victim_count) AS "# of Victims"
FROM hate_crime hc
WHERE data_year >= 2019 AND 
	bias_desc = "Anti-Asian" AND
	"# of Offenders" > 0
GROUP BY "# of Offenders", data_year 
ORDER BY "# of Asian Hate Crimes" DESC;

-- Are there any differences or similarities between hate crimes against Asians and hate crimes against other groups? (Parameter)
SELECT data_year AS "Year", bias_desc AS "Bias Description",
	offense_name AS "Offense Name",
	COUNT(*) AS "# of Hate Crimes",
	SUM(victim_count) AS "# of Victims"
	FROM hate_crime hc
WHERE data_year >= 2019
GROUP BY offense_name, data_year 
ORDER BY "# of Hate Crimes" DESC;

--Things I wish I could anayze
-- Age of Offenders
-- Age of Victims (Older?)
-- Pattern of time of day