# Case Study - UK Road Accident Using Python, SQL and Tableau
# *Introduction*
The case study involves a Dataset of UK Accidents.

The UK government maintains an exhaustive repository of data on traffic accidents, which is updated annually and made available to the public via the Open Data website.

The analysis will follow the 6 phases of the Data Analysis process: Ask, Prepare, Process, Analyze, and Act (APPAA).

# *ASK*
In this case study, we will explore the UK Road Accident dataset to analyze different factors that contribute to traffic accidents. The dataset includes a range of variables, such as geographic locations, weather conditions, vehicle types, and the number of casualties involved, making it a valuable resource for understanding the causes and effects of road accidents. To achieve this goal, we will address the following questions:  

+ What are the most common types of vehicles involved in accidents?  

+ What are the most frequent weather conditions associated with accidents?  

+ How do the number of casualties vary across different types of accidents?  

+ Which geographic areas have the highest occurrence of accidents?  

By answering these questions, we can gain valuable insights into the factors that contribute to road accidents and identify areas where interventions may be necessary to reduce the occurrence of accidents.      

# *PROCESS*
The dataset contains an array of information, ranging from geographic locations to weather conditions, vehicle types, and the number of casualties involved. This comprehensive and fascinating collection of data provides an excellent opportunity for in-depth analysis and research, and the Department of Transport is responsible for publishing it.

# *Process* 
+ Refer :- cleaning and preparation of data.ipynb
+ after performing the above ipynb we get Cleaned data

# *ANALYZE*
## *USING MYSQL*
-- check data

```sql
SELECT * FROM accidents;
```

-- Total number of accidents

```sql
SELECT
	COUNT(ID) AS total_accidents
FROM 
	accidents;
```

-- What are the number of casualties in each day of the week? Sort them in descending order.

```sql
SELECT
  DATE_FORMAT(Accident_Date, '%W') AS Day_of_Week,
  SUM(Number_of_Casualties) AS Total_Casualties
FROM
  accidents 
GROUP BY
  Day_of_Week
ORDER BY
  Total_Casualties DESC;
```
 

-- What are the number of casualties in each month?

```sql
SELECT 
    DATE_FORMAT(Accident_Date, '%Y-%m') AS Month, 
    SUM(Number_of_Casualties) AS Total_Casualties
FROM 
    accidents
GROUP BY 
    Month
ORDER BY 
    Month ASC;
```
 

-- Which geographic areas have the highest occurrence of accidents?

```sql
SELECT 
    District_Area, 
    SUM(Number_of_Casualties) AS Total_Casualties
FROM 
    accidents
GROUP BY 
    District_Area
ORDER BY 
    Total_Casualties DESC
LIMIT 10;
 ```

-- What are the most frequent weather conditions associated with accidents?

```sql
SELECT 
    Weather_Conditions, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
GROUP BY 
    Weather_Conditions
ORDER BY 
    Num_Accidents DESC;
 ```

-- What are the most common types of vehicles involved in accidents?

```sql
SELECT 
    Vehicle_Type, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
GROUP BY 
    Vehicle_Type
ORDER BY 
    Num_Accidents DESC;
 ```

-- How do the number of casualties vary across different types of accidents?

```sql
SELECT 
    Accident_Severity, 
    Road_Type, 
    COUNT(*) AS Num_Accidents, 
    SUM(Number_of_Casualties) AS Num_Casualties
FROM 
    accidents
GROUP BY 
    Accident_Severity, Road_Type
ORDER BY 
    Accident_Severity ASC, Num_Casualties DESC;
 ```

-- Quantity and ratio of severity.

```sql
SELECT 
	accident_severity,
	COUNT(accident_severity) AS total_severity,
	ROUND(COUNT(*) * 100./ SUM(COUNT(*)) OVER (),2) as percentage_of_accidents
from
	accidents
GROUP BY accident_severity
ORDER BY 3 DESC;
 ```

-- Verify if most accidents happens during the night or day.

```sql
SELECT 
    Light_Conditions, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
WHERE 
    Light_Conditions IN ('Daylight', 'Darkness - lights lit', 'Darkness - no lighting', 'Darkness - lighting unknown', 'Darkness - lights unlit')
GROUP BY 
    Light_Conditions
ORDER BY 
    Num_Accidents DESC;
```
