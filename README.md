# Bellabeat Case Study
- *Google Data Analytics Professional Certificate: Capstone Project*

- Using Excel, SQL & Tableau

---

- ## Table of content
  - [1. Company](#1-company)
    - [1.1 Products](#11-products)
  - [2. Ask](#2-ask)
    - [2.1 Business task](#21-business-task)
    - [2.2 Stakeholders](#22-stakeholders)
  - [3. Prepare](#3-prepare)
    - [3.1 Data used for analysis](#31-data-used-for-analysis)
    - [3.2 Data accessibility and credibility](#32-data-accessibility-and-credibility)
    - [3.3 Data summary and organization](#33-data-summary-and-organization)
      - [3.3.1 Limitation](#331-limitation)
  - [4. Process](#4-process)
    - [4.1 Selected datasets](#41-selected-datasets)
    - [4.2 Excel for data cleaning](#42-excel-for-data-cleaning)
    - [4.3 Preparing for SQL data analysis](#43-preparing-for-sql-data-analysis)
  - [5. Analyze](#5-analyze)
    - [5.1 Number of users](#51-number-of-users)
    - [5.2 Users insights](#52-users-insights)
      - [5.2.1 Users activity](#521-users-activity)
  - [7. Act](#7-act)
---
## 1. Company
---
Bellabeat is a high-tech company that manufactures health-focused smart product. Company was founded by Urška Sršen and Sando Mur. Bellabeat has grown very quickly due to products that aim to women.

>                       "Tech-driven wellness company for woman"
Bellabeat invests into Google Seaech, Facebook adds, Instagram Pages and Youtube videos to catch customers. 
Since 2016 Bellabeat has offices around the world and brought many products to the market.

Right now they want to make analysis of their customer's data, because it is a opportunity for growth and the company can simply focus on customer needs.

### 1.1 Products
- **Bellabeat app** - Provides users with their health data.
- **Leaf** - Classis wellness tracker that can be worn as clip, necklace or bracelet
- **Time** - High-tech wellnes watches
- **Spring** - Water bottle with intagrated chip, which tracks daily water intake
## 2. Ask
---
 ### 2.1 Business task
 >*Identify potential opportunities for growth of the company and set some recommendations for marketing team to improved their strategies based on trends in usage of smart device*
 - What are some trednds in smart device usage?
 - How could these trends apply to Bellabeat customers?
 - How cold these trends help influence Bellabeat marketing strategy?
 ### 2.2 Stakeholders
  - **Urška Sršen** - Cofounder and CCO
  - **Sando Mur** - Cofounder and member of the Bellabeat executive team
  - **Bellabeat** marketing analytics team - Team resposnible of data collecting, analyzing and reporting. 
## 3. Prepare
---
### 3.1 Data used for analysis
The Dataset is available on **[FitBit Fitness Tracked Data](https://www.kaggle.com/datasets/arashnic/fitbit)** and includes 18 CSV files.

The data is published by Mobius in open-sourced format under CC0:Public Domain Creative Common License. Thanks to **CC0:Public** Domain the data can be modified and distributed without permision.
### 3.3 Data summary and organization
The dataset includes data about activity-time, heart rate, sleep monitoring 
The data was collected by survey via Amazon Mechanical Turk between 03.12.2016-05.12.2016
#### 3.3.1 Limitation
- Small sample size (around 30 participants)
- Short time period of data collecting
- The possibility of inaccurate conclusion due to old data from 2016
- No additional demographic information
## 4. Process
---
### 4.1 Selected Datasets
For the Bellabeat case study the following datasets were chosen:
- dailyActivity_merged
- dailySteps_merged
- sleepDay_merged
### 4.2 Excel for data cleaning
1. Impoerting data from CSV to excel file
2. Removing duplicates
3. Check column ID that the records have the exact number of the characters (10)
4. Check column ActivityDate that the records have the same format of date (mm/dd/yyyy)
5. Farmating the numerical data to be a number with 0 or 1 decimal place.
6. Column TrackerDistance deleted (same values as TotalDistance)
7. Column LoggedActivitiesDistance deleted (only zero values)
8. Added Column Day (text function) based on Activity date column
9. Deleted all rows with zero values in all columns (dailyActivity_merged file)
10. Deleted all rows with zero values in "StepsTotal" column. (dailySteps_merged file)
11. Export to CSV
### 4.3 Preparing for SQL data analysis
After cleaning all the data and exporting them to CSV file, I decided to import the data to the BigQuery (SQL).
SQl is probably the biggest data analysis skill you need to know how to work with.
1. Create a project in the BiqQuery
2. Create a dataset and tables
3. Import the data from CSV to tables
## 5. Analyze & Share
---
### 5.1 Number of users 
Number of users for easch dataset by SELECT, COUNT and DISTINCT in SQL.
```
SELECT 
  COUNT(DISTINCT Id) AS Users_number_activity
 FROM `bellabeat-401316.bellabeat.daily_activity`;
```
 ```
SELECT 
  COUNT (DISTINCT Id) AS Users_number_sleep
 FROM `bellabeat-401316.bellabeat.daily_sleep`;
``` 
 ```
SELECT 
  COUNT(DISTINCT Id) AS Users_nember_steps
 FROM `bellabeat-401316.bellabeat.daily_steps`;
```
- Two datasets (dailyActivity and dailySteps ) have 33 users and dataset of dailySleep has 24 users
### 5.2 Users insights
#### 5.2.1 Users activity
It is important to know how many days we have in the dailyActivity dataset.
 ```
SELECT 
  COUNT(DISTINCT ActivityDate) AS Users_number_activity
 FROM `bellabeat-401316.bellabeat.daily_activity`;
```
The result of SQL code is 31 days.
Next, I wanted to know how active users were. So i created four groups by number of active days.
 ```
SELECT 
  COUNT(Id) AS logged,
  CASE
    WHEN COUNT(Id) < 14 THEN 'Light activity'
    WHEN count(Id) < 24 THEN 'Moderate activity'
    WHEN COUNT(Id) < 31 THEN 'High activity'
    ELSE 'Everyday user'
  END Activity_status
 FROM `bellabeat-401316.bellabeat.daily_activity`
 GROUP BY Id;
```
## 7. Act
