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
