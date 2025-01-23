# SQL + EXCEL-POLLUTION-PROJECT

## Project Overview

**Project Title**: **SQL + EXCEL-POLLUTION-PROJECT**   
**Database**: `updated_pollution_project`

This document outlines key questions and insights derived from the analysis of a comprehensive pollution dataset. The dataset contains vital environmental metrics, including PM2.5, PM10, nitrogen dioxide
(NO₂), sulfur dioxide (SO₂), carbon monoxide (CO), temperature, and humidity. These metrics are analyzed to uncover patterns,correlations, and potential risk factors affecting air quality in various regions. 

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

### **Data Cleaning in Excel**

 - **Generated a Primary Key Column:**
 Created a new column named RecordID to serve as the primary key, as the dataset originally lacked a primary key.

 - **Removed Unnecessary Rows:**
 Deleted 3 rows that were deemed unnecessary.

 - **Checked for Duplicates:**
 Performed a check for duplicate rows, but no duplicates were found in the dataset.

 - **Deleted Null Values:**
 Identified and removed rows containing null values using Excel’s "Go To Special" feature:
 Selected all blank cells.
 Right-clicked and deleted the corresponding rows.


- **"Renaming Columns for Clarity and Compatibility"**
  The column names in the dataset were updated for better understanding.
  NO2 was changed to Nitrogen_Dioxide_Levels to represent nitrogen dioxide levels clearly.
  Similarly, SO2 became Sulfur_Dioxide_Levels, and CO was renamed Carbon_Monoxide_Levels to avoid confusion with carbon dioxide.
  The particulate matter columns were updated as well: PM10 is now PM10_Concentration, and PM2.5 is PM25_Concentration,
  ensuring clarity and compatibility with database systems.
  These changes make the dataset more intuitive for analysis and reporting.


- **Note: The column "PM25_CONCENTRATION" in the dataset refers to "PM2.5 Concentration."**
   The naming avoids using a period (".") for compatibility with database systems.

 - **Here’s a Brief Explanation.** 
 The column "PM25_CONCENTRATION" in the dataset represents "PM2.5 Concentration," which refers
 to particulate matter with a diameter of 2.5 micrometers or smaller. These fine particles are a critical
 pollutant affecting air quality and public health, measured in micrograms per cubic meter (µg/m³).
 Due to technical constraints in database systems, where special characters like periods (".") are not
 supported in column names, "PM2.5" was renamed to "PM25" for compatibility. While the naming
 convention has been modified, the data still reflects the same information about PM2.5 levels.
 This adjustment ensures smooth querying in SQL environments without altering the data's meaning or relevance.


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

- **Air Quality Categories:** The dataset reveals that the majority of areas fall under the "Moderate" air quality category, with relatively fewer areas classified as "Good."

- **Industrial Proximity Impact:** Regions closer to industrial zones (≤10 km) exhibit significantly higher levels of pollutants such as PM2.5, PM10, and SO₂, indicating industrial emissions as a key contributor to poor air quality.

- **Population Density and Pollution:** High population density areas (>500) show elevated NO₂ and CO levels, likely due to urban congestion, vehicular emissions, and limited green spaces.

- **Humidity and Particulate Matter:** High humidity levels (>70%) are associated with increased PM2.5 and PM10 concentrations, as moisture in the air traps pollutants closer to the surface.

- **Exceeding Safe Thresholds:** Pollutants such as PM2.5 and NO₂ frequently exceed safe limits in industrial and densely populated areas, posing significant health risks.

- **Good Air Quality Zones:** Areas with "Good" air quality are generally located farther from industrial zones and have lower population densities, highlighting the importance of distance from pollution sources.

- **Combined Pollutant Effects:** PM2.5 and PM10 often exceed thresholds together, emphasizing their combined impact on air quality and potential health hazards.

- **Data Quality Insights:** The dataset required cleaning, including the removal of null values and duplicates, ensuring consistency and accuracy for analysis.

## Reports

- **Data Analysis:** Translating raw pollution data into actionable insights about air quality and its influencing factors.
- **Predictive Indicators:** Areas near industrial zones with high humidity and population density exhibit a higher likelihood of exceeding safe pollutant thresholds.
- **Environmental Impact:** Identifying the relationship between industrial proximity, pollutant levels, and air quality categories to understand environmental risks.
- **Urban Planning Insights:** Highlighting areas with high population density and poor air quality for prioritizing urban development and green space initiatives.
  
## Conclusion

The SQL analysis of the pollution dataset highlights the effectiveness of structured querying in uncovering meaningful insights from raw data. The findings emphasize the impact of industrial activity, population density, and environmental factors on air quality. Key takeaways include the frequent exceedance of safe pollutant thresholds in industrial zones and the combined effects of multiple pollutants on air quality. These insights underscore the need for targeted interventions, such as stricter emission controls, public awareness campaigns, and urban planning strategies, to improve air quality and mitigate health risks.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional questions.

## DATASET: (https://www.kaggle.com/datasets/mujtabamatin/air-quality-and-pollution-assessment)

## Author - Deepak

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and follow

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media.

- **LinkedIn**: [Connect with me professionally]([https://www.linkedin.com/in/deepak-analyst-485b602a3/](https://www.linkedin.com/in/deepak-analyst-485b602a3/)

Thank you for your support, and I look forward to connecting with you!
