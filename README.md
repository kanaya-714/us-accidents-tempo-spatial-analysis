# US Accidents Tempo-Spatial Analysis

## Project Overview

This group project analyzes the US Accidents dataset using Hadoop and Hive. The dataset is over 7GB and contains detailed traffic accident records across the United States, including timestamps, latitude and longitude, city, state, weather conditions, and road/environmental features.

The goal of this project is to perform tempo-spatial analysis, which means analyzing accident patterns across both time and location. Using HiveQL, this project identifies trends such as peak accident hours, accident counts by state and city, weather-related accident patterns, and possible geographic hotspots.

This analysis is important because understanding when and where accidents happen most often can help improve road safety awareness, support better traffic management strategies, and contribute to more informed infrastructure planning.

## Dataset

Dataset used:  
[US Accidents Dataset on Kaggle](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents)

Dataset size: Over 7GB

The dataset includes traffic accident records from across the United States. Important fields include:

- Start_Time
- End_Time
- Start_Lat
- Start_Lng
- City
- State
- Weather_Condition
- Temperature
- Visibility
- Wind_Speed
- Severity

Note: The full dataset is not included in this repository because of its large file size and Kaggle dataset terms. Users should download the dataset directly from Kaggle.

## Tools and Technologies

- Hadoop
- HDFS
- Hive
- HiveQL
- YARN
- Linux command line
- Power BI / Excel / Tableau for visualization
- GitHub for code and documentation

## Hardware / Cluster Specifications

The analysis was completed using a Hadoop cluster environment.

- Hadoop Version: Hadoop 3.4.1.odh.2.0.e96aeb80f39
- Cluster Layout: 5 total nodes based on course cluster design
  - 2 master nodes
  - 3 worker nodes
- Active YARN Worker Nodes During Testing: 3 running nodes
  - bigdaim0
  - bigdaim1
  - bigdaim2
- Memory: 31 GB RAM on the current node
- Available Memory During Testing: 20 GB
- CPU: AMD EPYC 7J13 64-Core Processor
- CPU Cores / vCPUs Shown: 6
- CPU Speed: 2.45 GHz
- Storage: 117 GB root disk
- Available Storage During Testing: 59 GB
- Architecture: Linux x86_64

## Project Objectives

- Upload the US Accidents dataset into HDFS
- Create Hive database and external tables
- Process a dataset larger than 2GB using Hadoop and Hive
- Analyze accidents by state and city
- Analyze accident frequency by hour of day
- Analyze accident trends by weather condition
- Identify high-accident locations using latitude and longitude
- Create tempo-spatial visualizations based on time and location
- Present findings using charts, maps, and workflow diagrams

## Big Data Processing Workflow

1. Download the US Accidents dataset from Kaggle
2. Upload the dataset to Hadoop HDFS
3. Create a Hive database
4. Create an external Hive table for the accident data
5. Run HiveQL queries for time, location, and weather analysis
6. Export Hive query results as CSV files
7. Create charts and map visualizations
8. Organize code, results, visuals, and documentation in GitHub
9. Present project findings in the final presentation

## Implementation Architecture

```text
Kaggle Dataset
      |
      v
Local / Cluster Storage
      |
      v
HDFS Upload
      |
      v
Hive External Table
      |
      v
HiveQL Queries
      |
      v
CSV Query Results
      |
      v
Power BI / Excel / Tableau Visualizations
      |
      v
Final Presentation and GitHub Documentation
```
## Step-by-Step Implementation


1. **Upload Dataset to Cluster**
   

   -- open terminal --
  
   scp ~/Downloads/US_Accidents_March23.csv user@129.146.237.12:/tmp/
   
   -- new linux terminal --
   
   ssh username@ipaddress
   
   cd /tmp
   
   ls -lh
   

2. **Upload Data to HDFS**
   
   
   hdfs dfs -mkdir -p accidents_full
   
   hdfs dfs -put US_Accidents_March23.csv accidents_full/
   
   hdfs dfs -ls accidents_full
   
   
3. **Create Hive Table**
   

   -- new hive terminal --
   
   
   ssh username@ipaddress
   
   beeline
   
   USE username;

   DROP TABLE IF EXISTS accidents_full;

CREATE EXTERNAL TABLE accidents_full (

    id STRING,
    source STRING,
    severity STRING,
    start_time STRING,
    end_time STRING,
    start_lat STRING,
    start_lng STRING,
    end_lat STRING,
    end_lng STRING,
    distance_mi STRING,
    description STRING,
    street STRING,
    city STRING,
    county STRING,
    state STRING,
    zipcode STRING,
    country STRING,
    timezone STRING,
    airport_code STRING,
    weather_timestamp STRING,
    temperature_f STRING,
    wind_chill_f STRING,
    humidity STRING,
    pressure_in STRING,
    visibility_mi STRING,
    wind_direction STRING,
    wind_speed_mph STRING,
    precipitation_in STRING,
    weather_condition STRING,
    amenity STRING,
    bump STRING,
    crossing STRING,
    give_way STRING,
    junction STRING,
    no_exit STRING,
    railway STRING,
    roundabout STRING,
    station STRING,
    stop STRING,
    traffic_calming STRING,
    traffic_signal STRING,
    turning_loop STRING,
    sunrise_sunset STRING,
    civil_twilight STRING,
    nautical_twilight STRING,
    astronomical_twilight STRING
  )
  ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
  
  WITH SERDEPROPERTIES (
  
  "separatorChar" = ",",
  
  "quoteChar" = "\""
  )
  
  STORED AS TEXTFILE
  
  LOCATION '/user/username/accidents_full'
  
  TBLPROPERTIES ("skip.header.line.count"="1");
  

4. **Create Sample Table for Debugging**
   
   
   DROP TABLE IF EXISTS accidents_debug;
   
   CREATE TABLE accidents_debug AS
   
   SELECT *
   
   FROM accidents_full
   
   LIMIT 10000;
   

5. **Exploratory Analysis for Debugging**
   
   
   **Accidents by Hour**

   SELECT hour(start_time) AS accident_hour,
   
   COUNT(*) AS accident_count
   
  FROM accidents_debug
  
  GROUP BY hour(start_time)
  
  ORDER BY accident_count DESC;

  **Top States**

  SELECT state, COUNT(*) AS accident_count
  
  FROM accidents_debug
  
  GROUP BY state
  
  ORDER BY accident_count DESC
  
  LIMIT 10;

  **Top Cities**

  SELECT city, state, COUNT(*) AS accident_count
  
  FROM accidents_debug
  
  WHERE city IS NOT NULL
  
  GROUP BY city, state
  
  ORDER BY accident_count DESC
  
  LIMIT 10;
  
  **Weather Conditions**
  
  SELECT weather_condition, COUNT(*) AS accident_count
  
  FROM accidents_debug
  
  WHERE weather_condition IS NOT NULL
  
  GROUP BY weather_condition
  
  ORDER BY accident_count DESC
  
  LIMIT 20;
  

6. **Create Large Data Subset (2GB)**
   

   DROP TABLE IF EXISTS accidents_subset;
   
   CREATE TABLE accidents_subset AS
   
   SELECT *
   
   FROM accidents_full
   
   LIMIT 5019505;
   
  
7. **Full Dataset Analysis**

 
   **Top States**
   
     SELECT state, COUNT(*) AS accident_count
   
  FROM accidents_subset
  
  GROUP BY state
  
  ORDER BY accident_count DESC
  
  LIMIT 10;

  **Top Cities**
  
  SELECT city, state, COUNT(*) AS accident_count
  
  FROM accidents_subset
  
  WHERE city IS NOT NULL
  
  GROUP BY city, state
  
  ORDER BY accident_count DESC
  
  LIMIT 10;

   **Accidents by Hour**
  
  SELECT hour(start_time) AS accident_hour,
  
  COUNT(*) AS accident_count
  
  FROM accidents_subset
  
  GROUP BY hour(start_time)
  
  ORDER BY accident_hour;

  **Weather Conditions**
  
  SELECT weather_condition, severity,
  
  COUNT(*) AS accident_count
  
  FROM accidents_subset
  
  WHERE weather_condition IS NOT NULL
  
  GROUP BY weather_condition, severity
  
  ORDER BY accident_count DESC
  
  LIMIT 30;
  

  8. **Export Results to HDFS**
  

  INSERT OVERWRITE DIRECTORY '/user/username/accident_hotspots'
  
  ROW FORMAT DELIMITED
  
  FIELDS TERMINATED BY '\t'
  
  SELECT start_lat,
  
         start_lng,
         start_time,
         city,
         state
         
  FROM accidents_subset
  
  WHERE start_lat IS NOT NULL
  
  AND start_lng IS NOT NULL

  LIMIT 10000;
  
  
  9. **Retrieve Results**

 
  -- linux terminal --
  
  hdfs dfs -ls accident_hotspots
  
  hdfs dfs -get accident_hotspots/000000_0 accident_hotspots.txt
  

  10. **Export Results to Personal Computer for Tempo-Spatial Analysis**

 
  -- new terminal --

  scp username@ipaddress:~/accident_hotspots.txt .
  
  
  11. **Cleanup**

 
  -- hive terminal --
  
  DROP TABLE accidents_full;
  
  DROP TABLE accidents_debug;
  

  -- linux terminal --
  
  hdfs dfs -rm -r accidents_full
  
  rm /tmp/US_Accidents_March23.csv
