# Bellabeat Case Study
- *Google Data Analytics Professional Certificate: Capstone Project*

- Using Excel, SQL & PowerBI

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
      - [5.2.1 Users activity](#521-users-device-activity)
      - [5.2.2 Active daily minutes vs Sedentary daily minutes](#522-active-daily-minutes-vs-sedentary-daily-minutes)
      - [5.2.3 Steps breakdowns](#523-steps-breakdowns)
      - [5.2.4 Sleep breakdowns](#524-sleep-breakdowns)
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
#### 5.2.1 Users device activity
It is important to know how many days we have in the dailyActivity dataset.
 ```
SELECT 
  COUNT(DISTINCT ActivityDate) AS Users_number_activity
 FROM `bellabeat-401316.bellabeat.daily_activity`;
```
The result of SQL code is **31 days**.
Next, I wanted to know how active users were. So I created four groups by number of active days.
- **Light activity** - less than 14 days of device using
- **Moderate activity** - between 14 and 23 days of device using
- **High activity** - between 24 and 30  days of device using
- **Everyday user **- 31 days of device using
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
![Users device activity](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/user_device_activity.png)
#### 5.2.2 Active daily minutes vs Sedentary daily minutes
First I wanted to show Piecahrt of Acitive daily minutes vs Sedentary minutes for a whole month.
```
SELECT 
SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes) AS Daily_Active_Minutes,
SUM(SedentaryMinutes) AS Daily_sedentary_minutes
FROM `bellabeat-401316.bellabeat.daily_activity`;
![Active daily minutes vs Sedentary daily minutes_month](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/Active_minute_sessedentary_minutes_month.png)
```
Next I would like to show difference between Active daily minutes and Sedentary daily minutes by a weekdays.
```
SELECT 
DAY,
SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes) AS Daily_Active_Minutes,
SUM(SedentaryMinutes) AS Daily_sedentary_minutes
FROM `bellabeat-401316.bellabeat.daily_activity` 
GROUP BY Day;
```
![Active daily minutes vs Sedentary daily minutes](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/active%20mintes_sedentary_minutes_days.png)
#### 5.2.3 Steps breakdowns
For first part of steps analysis it was necessary to calculate AVG of steps and SUM of steps by a weekdays.

```
SELECT
Day,
SUM(StepTotal) AS Sum_of_steps,
AVG(StepTotal) AS Avg_of_steps,
 FROM `bellabeat-401316.bellabeat.daily_steps` 
GROUP BY Day;
```
![SUM and AVG of steps by a weekdays](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/sum_avg_steps.png)
>The grapf shows us that Tuesday and Saturday are the most active days. Monday is one of the most inactive days, probably because people have the most responsibilities at work and want to get things done in the city. The seond inactive say is Sunday, probably becasuse it is rest day for most people.

A Healthline.com article [How many steps do I need a day?](https://www.healthline.com/health/how-many-steps-a-day#how-many-steps-per-day)written by Sara Lindberg in 2019 cited a 2011 study by Tudor-Locke et. al. titled “How many steps/day are enough? for adults” which found that 10,000 steps/day is a reasonable target for healthy adults. Lindberg (2019) breaks down activity level by steps into 6 categories based off the 2011 study by Tudor-Locke et. al.:
- **Basal** - below 2500 steps/day
- **Limited** - 2500-4999 steps/day
- **Low**- 5000-7499 steps/day
- **Somewhat activ**e - 7500-9999 steps/day
- **Active** - 10000-12499 steps/day
- **Very activ**e - over 125000 steps/day
```
SELECT
Id,
AVG(StepTotal) as avg_steps,
CASE
  WHEN AVG(StepTotal) <2500 THEN "Basal"
  WHEN AVG(StepTotal) <5000 THEN "Limited"
  WHEN AVG(StepTotal) <7500 THEN "Low"
  WHEN AVG(StepTotal) <10000 THEN "Somewhat active"
  WHEN AVG(StepTotal) <12500 THEN "Active"
  ELSE "Very active"
END Steps_status
 FROM `bellabeat-401316.bellabeat.daily_steps`
GROUP BY Id;
```
![Users status by steps](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/users_status_steps.png)
>The grapf shows us that only 13 users (39%) are very activ or active, i.e. more than 10 000 steps, and 20 users (61%) are less then 10 000 steps per day.
#### 5.2.4 Sleep breakdowns
A Sleepfoundation.org article [Sleep Calculator: Your Personalized Tool for Sleep](https://www.sleepfoundation.org/sleep-calculator) in 2023 which found that 7 or more hours is a reasonable target for healthy adults. So I divided users into two categories:
- **Not enought of time asleep** - less then 7 hours
- **Enought of time asleep** - 7 and more hours 
```
SELECT 
  Id,
  (AVG(TotalMinutesAsleep)/60) as Asleep_time,
  CASE
    WHEN (AVG(TotalMinutesAsleep)/60) < 7.0 THEN "not enought of time asleep"
    ELSE "enought of time asleep"
  END sleep_status,
 FROM `bellabeat-401316.bellabeat.daily_sleep`
 GROUP BY Id;
```
![Users status asleep](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/user_status_asleep.png)

>The graph shows us that only 50% of users have enought time of asleep.

The next graph will confirm or refute the hypothesis that people with 10 000 steps sleep aroud 8 and more hours.

```
SELECT
s.Id, 
 AVG(TotalSteps) AS avg_total_steps,
 AVG(TotalMinutesAsleep)/60 AS avg_total_hours_asleep
 FROM `bellabeat-401316.bellabeat.daily_sleep` AS s
 INNER JOIN `bellabeat-401316.bellabeat.daily_activity` AS a ON
 s.Id = a. Id
 GROUP BY s.Id;
```
 ![Sleep hypothesis](https://github.com/OndrejZapletal99/Google_capstone_project/blob/main/PowerBi/Steps_sleep_relation.png)

 >The graph shows us that people with 10 000 steps (aprox.) sleep around 7-8 hours in average. But the graph shows outliers, which can be measurement errors.

## 7. Act
