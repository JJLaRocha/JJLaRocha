# Google Data Analytics Capstone: Cyclistic Case Study
Course: [Google Data Analytics Capstone: Complete a Case Study](https://www.coursera.org/learn/google-data-analytics-capstone) 

## Introduction
I will be presenting here my project to complete my Google Data Analytics Certificate: [Google Data Analytics: Complete a Case Study](https://www.coursera.org/learn/google-data-analytics-capstone)

I will be performing the functions of an entry-level Data Analyst in order to answer questions to improve strategies and results for a fictional company named _Cyclistic_. i will be going through the different stages of Data Analysis: Ask, Prepare, Process, Analyze, Share and Act.

## Context
Cyclistic is a company whose services consist of a bike-share offering. They have fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. **Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a solid opportunity to convert casual riders into members.** She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

**Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.**

### Scenario
As a Junior Business Analyst for Cyclistic, I will be following the instructions given by my manager Moreno, who believes that the way to improve Cyclistic's business is to convert Casual Riders to Members. I will transform, combine and analyze data to decide what are the best decisions to make to reach the goals and to give the possible amount of safety when making these decisions. 

## Ask

Three questions will guide the future marketing program:

1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned me the first question to answer

## Prepare

### Data Source

The data I will use for my analysis is from January 2023 to December 2022 and was made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement) and can be downloaded from [this link](https://divvy-tripdata.s3.amazonaws.com/index.html). 

This is public data that can be used to explore how different customer types are using Cyclistic bikes. But note that data-privacy issues prohibit from using riders’ personally identifiable information. This means that we won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes. This excludes the possibility of trying to prove the hipothesis that for a big share of the casual riders, it would be less costly to subscribe anually to Cyclistic's services, so we'll have to work it from a different angle.

### Data Content

The data consists of a table for every ride taken in each one of the year's months. Its variables are ride id, bike type, start time, end time, start station, end station, start location, end location, and type of client (member or casual). 

## Process

The data of all months was combined in order to paint the picture for the whole year. All of the data merging and analysis was done with BigQuery since this dataset is too large to be worked in spreadsheets. With that being said, all the data was manipulated through SQL queries, more specifically with BigQuery.

The query used to combine all the 12 csv files was [this](https://github.com/JJLaRocha/JJLaRocha/blob/JJLaRocha-patch-2/Data%20Combining).

### Data Cleaning

SQL Query: [Data Cleaning and Transformation](https://github.com/JJLaRocha/JJLaRocha/blob/JJLaRocha-patch-2/Data%20Cleaning%20and%20Transformation)

1. All the rows where the primary key (ride_id) values were null were deleted.
2. I added 3 columns to make the analysis easier and more objective: ride duration, month and day of the week.

## Analyze and Share

![image](https://private-user-images.githubusercontent.com/158205795/304026536-e7514ed4-ecd9-4690-a400-c363c5bf90eb.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc3MzUyODIsIm5iZiI6MTcwNzczNDk4MiwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0MDI2NTM2LWU3NTE0ZWQ0LWVjZDktNDY5MC1hNDAwLWMzNjNjNWJmOTBlYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEyJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxMlQxMDQ5NDJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mOGU2NDExMGZmZWMyZjZmNGU2ZGUxMzdiOTAxNzcxMjIwNTdmZmE4NjU0YzhlYmE1MjVlYmNlYzc5YjllMjRmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.vAXRPpTEF81PgZCe-CVRMaXlbhzNL9Q2nMyHDqkXQL8)







