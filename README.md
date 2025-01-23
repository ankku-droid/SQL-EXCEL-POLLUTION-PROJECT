# SQL + EXCEL-POLLUTION-PROJECT

## Project Overview

**Project Title**: **SQL + EXCEL-POLLUTION-PROJECT**   
**Database**: `updated_pollution_project`

This project focuses on analyzing a comprehensive dataset related to lung cancer, with the objective of identifying key trends, correlations, and risk factors. Using PostgreSQL for data cleaning, transformation, and advanced query execution, the analysis delves into patient demographics, lifestyle factors, and symptom patterns. Key aspects of the project included transforming binary data into meaningful categories, exploring the relationships between smoking, chronic diseases, and symptoms, and identifying high-risk demographic groups.

## Objectives

1. **Set up a pollution dataset database**: Create and populate a database to store pollution data, including pollutant levels, temperature, humidity, and proximity to industrial zones.
2. **Data Cleaning**: Identify and remove missing values, handle duplicates, and ensure the dataset is consistent and ready for analysis.
3. **Exploratory Data Analysis (EDA)**: Perform initial analysis to understand trends, distributions, and relationships within the dataset.
4. **Business Analysis**: Use SQL queries to answer critical questions, uncover patterns, and derive actionable insights about air quality and its influencing factors.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `updated_pollution_project`.
- **Table Creation**: A table named `pollution_data` is created. The table structure includes columns for RecordID,	Temperature,	Humidity,	PM2.5 Concentration,	PM10 Concentration,	Nitrogen dioxide levels,	Sulfur dioxide levels,	Carbon monoxide levels,	Proximity_to_Industrial_Areas,	Population_Density,	Air Quality.



```sql
CREATE DATABASE updated_pollution_project;

CREATE TABLE pollution_data (
    RecordID INT PRIMARY KEY,
    Temperature FLOAT, 
    Humidity FLOAT,
    PM25_Concentration FLOAT,
    PM10_Concentration FLOAT,
    Nitrogen_Dioxide_Levels FLOAT,
    Sulfur_Dioxide_Levels FLOAT,
    Carbon_Monoxide_Levels FLOAT,
    Proximity_to_Industrial_Areas FLOAT,
    Population_Density INT,
    Air_Quality VARCHAR(9)  );
```

### 2. Data Cleaning

- **primary key**:In excel i generated a column name ID for making it primary key because there is no any primary key.
- **convert data**: Convert Numerical Encodings to Descriptive Labels Change 1 to "Yes" and 2 to "No" for relevant columns.
- **Standardize column names**: Standardize Column Names: Use consistent naming conventions (smoking_status instead of SMOKING).
- **Remove Duplicates**:Remove Duplicates:	Identify and remove duplicate rows to ensure data uniqueness. 2 DUPLICATES WERE FOUND AND DELETE. REMAINS 2998 ROWS.
- **Null values check**:Delete Null Values There's no any null values to Delete.
```sql
SELECT * FROM pollution_data
WHERE recordid IS NULL OR
temperature IS NULL OR 
humidity IS NULL OR 
pm25_CONCENTRATION IS NULL OR 
PM10_CONCENTRATION IS NULL OR 
NITROGEN_DIOXIDE_LEVELS IS NULL OR 
SULFUR_DIOXIDE_LEVELS IS NULL OR
CARBON_MONOXIDE_LEVELS IS NULL OR
PROXIMITY_TO_INDUSTRIAL_AREAS IS NULL OR
POPULATION_DENSITY IS NULL OR
AIR_QUALITY IS NULL ;
```


### 2. Data Exploration
**How many Data we have?**

```sql
 SELECT COUNT(*) AS total_record FROM POLLUTION_DATA;
```

**What is the distribution of temperature and humidity in the dataset?**

```sql
  SELECT 
     ROUND(AVG(Temperature)::numeric ,2) AS Avg_Temperature,
     MIN(Temperature) AS Min_Temperature,
     MAX(Temperature) AS Max_Temperature,
     ROUND(AVG(Humidity)::numeric ,2) AS Avg_Humidity,
     MIN(Humidity) AS Min_Humidity,
     MAX(Humidity) AS Max_Humidity
 FROM Pollution_Data;
```
**What are the average, minimum, and maximum values of PM2.5, PM10, NO₂, SO₂, and CO?**

```sql
 SELECT 
     ROUND(AVG(PM25_Concentration)::numeric ,2) AS Avg_PM25,
     MIN(PM25_Concentration) AS Min_PM25,
     MAX(PM25_Concentration) AS Max_PM25,
     ROUND(AVG(PM10_Concentration)::numeric ,2) AS Avg_PM10,
     MIN(PM10_Concentration) AS Min_PM10,
     MAX(PM10_Concentration) AS Max_PM10,
     ROUND(AVG(Nitrogen_dioxide_levels)::numeric ,2) AS Avg_NO2,
     MIN(Nitrogen_dioxide_levels) AS Min_NO2,
     MAX(Nitrogen_dioxide_levels) AS Max_NO2,
     ROUND(AVG(Sulfur_dioxide_levels)::numeric ,2) AS Avg_SO2,
     MIN(Sulfur_dioxide_levels) AS Min_SO2,
     MAX(Sulfur_dioxide_levels) AS Max_SO2,
     ROUND(AVG(Carbon_monoxide_levels)::numeric ,2) AS Avg_CO,
     MIN(Carbon_monoxide_levels) AS Min_CO,
     MAX(Carbon_monoxide_levels) AS Max_CO
 FROM Pollution_Data;

```



### 3. Data Analysis & Business Key Problems & Answers_

The following SQL queries were developed to answer specific business questions:

1. **How many records are present in the dataset?**:
```sql
SELECT COUNT(*) AS Total_Records
 FROM pollution_data;
```

2. **What is the average PM2.5 and PM10 concentration recorded in the dataset?**:
```sql
 SELECT ROUND(AVG(pm25_concentration)::numeric, 2) AS avg_pm2_5,
  ROUND(AVG(pm10_concentration)::numeric, 2) AS avg_pm10
 FROM pollution_data;
```

3. **What is the average temperature and humidity recorded across all entries in the dataset?**:
```sql
SELECT ROUND(AVG(temperature)::numeric, 2) AS avg_temperature,
  ROUND(AVG(humidity)::numeric, 2) AS avg_humidity 
 FROM pollution_data;

```

4. **How many areas are classified under each air quality category such as Good or Moderate?**:
```sql
SELECT 
     Air_Quality,
     COUNT(*) AS area_count
 FROM Pollution_Data
 GROUP BY Air_Quality
 ORDER BY area_count DESC;
```

5. **.How many areas are in close proximity to industrial zones based on recorded distances?**:
```sql
  SELECT COUNT(*) AS High_Proximity_Areas FROM pollution_data
 WHERE proximity_to_industrial_areas <=10;
  
```

6. **Which air quality category reports the highest average PM2.5 concentration in the given dataset?**:
```sql
SELECT 
     Air_Quality, 
     round(AVG(PM25_Concentration)::numeric ,2) AS Avg_PM2_5
 FROM Pollution_Data
 GROUP BY Air_Quality
 ORDER BY Avg_PM2_5 DESC
 LIMIT 1;
```

7. **What percentage of areas with high population density (> 500) have moderate or poor air quality?**:
```sql
 SELECT 
     COUNT(*) * 100.0 / (SELECT COUNT(*) FROM pollution_data
	 WHERE Population_Density > 500) AS Percentage
     FROM Pollution_Data
     WHERE Population_Density > 500 AND Air_Quality IN ('Moderate', 'Poor');

```

8. **How does air quality differ in areas with high humidity (> 70) compared to low humidity (< 30)?**:
```sql
 SELECT 
     CASE 
         WHEN Humidity > 70 THEN 'High Humidity'
         WHEN Humidity < 30 THEN 'Low Humidity'
     END AS Humidity_Category,
      Air_Quality,
     COUNT(*) AS Count
 FROM Pollution_Data
 GROUP BY Humidity_Category, Air_Quality;
```

9. **What is the relationship between industrial zone proximity and sulfur dioxide levels in the dataset?**:
```sql
SELECT 
    Proximity_to_Industrial_Areas, 
    Round(AVG(Sulfur_dioxide_levels)::numeric ,2) AS Avg_SO2
 FROM Pollution_Data
 GROUP BY Proximity_to_Industrial_Areas
 ORDER BY Proximity_to_Industrial_Areas;
	 
```

10. **How does population density correlate with carbon monoxide levels in different regions?**:
```sql
  SELECT 
     Population_Density,
     Round(AVG(Carbon_monoxide_levels)::numeric ,2) AS Avg_CO
 FROM Pollution_Data
 GROUP BY Population_Density
 ORDER BY Population_Density;

```

11. **Which pollutants (PM2.5, PM10, NO₂, SO₂, CO) frequently exceed safe thresholds when measured together?**:
```sql
SELECT 
     COUNT(*) AS Frequency
 FROM Pollution_data
 WHERE PM25_Concentration > 25 
       AND PM10_Concentration > 50 
       AND Nitrogen_dioxide_levels > 40
       AND Sulfur_dioxide_levels > 20 
       AND Carbon_monoxide_levels > 1.0;
```
**--Note:
--The thresholds used in the SQL query are based on air quality standards or guidelines set by organizations
--such as the World Health Organization (WHO) or local regulatory bodies. These thresholds are typically defined
--as the maximum safe limits for specific pollutants to minimize harm to human health. Here’s why these values might be chosen:
--Why Use These Values?
--These thresholds represent safe exposure limits. By checking which pollutants exceed these limits together,
--the query helps identify combinations of harmful air quality factors that occur simultaneously.
--If you want to change these limits (based on local standards), you can adjust the values in the query accordingly. For example:
--If your region's limit for PM2.5 is 35 µg/m³ instead of 25, you would update the query condition to PM2.5 Concentration > 35.**

12. **How does the combination of high PM2.5 and PM10 concentrations affect the air quality classification?**:
```sql
  SELECT 
     Air_Quality, 
     COUNT(*) AS Count
 FROM Pollution_Data
 WHERE PM25_Concentration > 25 AND PM10_Concentration > 50
 GROUP BY Air_Quality;

```

13. **What impact does industrial zone proximity have on air quality when accounting for population density?**:
```sql
 SELECT 
     Proximity_to_Industrial_Areas,
     Population_Density,
     AVG(PM25_Concentration) AS Avg_PM2_5,
     AVG(PM10_Concentration) AS Avg_PM10,
     Air_Quality
 FROM Pollution_data
 GROUP BY Proximity_to_Industrial_Areas, Population_Density, Air_Quality;;


```

14. **Can air quality classification (e.g., Good, Moderate) be predicted based on pollutant and environmental data?**:
```sql
 SELECT 
     Air_Quality, 
     AVG(PM25_Concentration) AS Avg_PM2_5,
     AVG(PM10_Concentration) AS Avg_PM10,
     AVG(Temperature) AS Avg_Temperature,
     AVG(Humidity) AS Avg_Humidity
 FROM Pollution_Data 
 GROUP BY Air_Quality;

```

15. **Which demographic factors (e.g., high population density, proximity to industry) indicate a higher risk of poor air quality?**:
```sql
  SELECT 
     Population_Density,
     Proximity_to_Industrial_Areas,
     COUNT(*) AS Poor_Quality_Count
 FROM Pollution_Data
 WHERE Air_Quality = 'Poor'
 GROUP BY Population_Density, Proximity_to_Industrial_Areas
 ORDER BY Poor_Quality_Count DESC;

```

16. **What percentage of records show "Good" air quality in areas with proximity to industrial zones?**:
```sql
SELECT 
     COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Pollution_Data) AS Good_No_Industrial_Percentage
 FROM Pollution_Data
 WHERE Air_Quality = 'Good' AND Proximity_to_Industrial_Areas =
 (SELECT MIN(Proximity_to_Industrial_Areas) FROM Pollution_Data);

```
**"Air Quality Near Industrial Zones (≤10 km)"**
```sql
 SELECT 
     COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Pollution_Data) AS Good_Nearest_Industrial_Percentage
 FROM Pollution_Data
 WHERE Air_Quality = 'Good' 
       AND Proximity_to_Industrial_Areas <= 10;

```


## Findings

- **Demographic Patterns**: The dataset reveals that the majority of lung cancer cases are concentrated in individuals aged 50 and above.
A higher incidence is observed among male patients compared to female patients.
- **Smoking as a Key Risk Factor**: Over 70% of diagnosed patients have a history of smoking, strongly correlating with the presence of lung cancer.
- **Symptom Analysis**:The most reported symptoms among diagnosed cases are coughing, chest pain, and shortness of breath.
Specific combinations of symptoms have a higher predictive value for lung cancer diagnosis.
- **Lifestyle and Occupational Risk Factors**: Data suggests a potential link between exposure to pollutants and lung cancer, especially in patients reporting occupational hazards.
- **Data Quality Insights**: Several records required normalization to ensure consistency, including converting binary codes (1/2) into descriptive labels.

## Reports

- **Data analysis:**: Translating raw data into actionable insights.
- **Predictive Indicators:**: Smokers with chronic diseases had a higher likelihood of reporting symptoms like chest pain.
- **Healthcare analytics**: Understanding risk factors and symptom correlations.
  
## Conclusion

The SQL analysis of the Lung Cancer dataset demonstrates the power of structured querying for deriving actionable insights from raw data. The findings emphasize the critical role of lifestyle and environmental factors in lung cancer prevalence, highlighting areas for targeted interventions such as anti-smoking campaigns and workplace safety measures.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional questions.

## Author - Deepak

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and follow

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media.

- **LinkedIn**: [Connect with me professionally]([https://www.linkedin.com/in/deepak-analyst-485b602a3/](https://www.linkedin.com/in/deepak-analyst-485b602a3/)

Thank you for your support, and I look forward to connecting with you!
