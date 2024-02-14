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

SQL Query: [Data Analysis](https://github.com/JJLaRocha/JJLaRocha/blob/JJLaRocha-patch-2/Data%20Analysis)
Visualizations: The visualizations were made in Microsoft Excel and Tableau after collecting the necessary data through the SQL queries. You can find them in [here].(https://github.com/JJLaRocha/JJLaRocha/issues)


### Bike Types


![image](https://private-user-images.githubusercontent.com/158205795/304415395-b1fedfd0-99c2-48ec-9e53-750b3f3f2aa3.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc4Mjk0NjYsIm5iZiI6MTcwNzgyOTE2NiwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0NDE1Mzk1LWIxZmVkZmQwLTk5YzItNDhlYy05ZTUzLTc1MGIzZjNmMmFhMy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxM1QxMjU5MjZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yMWJiYWI4N2NiYjU5ZWUxYWVlZmQ3M2E4ODFkNTQzMDdmMjM0ZjZiN2I3NWNiODJhZjI4NjBiNmQ0N2U2ZTI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.c3_7mN_7633bl2kb5Zc04BfkPSmRE1alSRfaYDUbdYs)


As it is clear, no significant correlation between subscription type and bicycle types if you keep the project goals in mind, since for classic and electric bikes the proportion is identical for both member and casual riders, and the docked bikes, eventhough only used by casual riders, represent a residual value.


### Ride Duration



![image](https://private-user-images.githubusercontent.com/158205795/304424279-0ffd6a71-d57b-455e-b7f4-dd67572ad717.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc4MzEzODIsIm5iZiI6MTcwNzgzMTA4MiwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0NDI0Mjc5LTBmZmQ2YTcxLWQ1N2ItNDU1ZS1iN2Y0LWRkNjc1NzJhZDcxNy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxM1QxMzMxMjJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xNDM4YmYzMzgyOTYxODI5OGIwNDcwYjJlOTA2YjE1YjY3ZjcxZjdlZTQzZDM0OWRhMjBiMTdkZDMzYzU1OTY2JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.xaL2n2jAy2k-p80NR6yJ9usKRmsVCJVQp-eHOfcs4_A)



![image](https://private-user-images.githubusercontent.com/158205795/304424396-fc91e188-a1be-4c38-8147-978e2052e406.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc4MzE0NDQsIm5iZiI6MTcwNzgzMTE0NCwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0NDI0Mzk2LWZjOTFlMTg4LWExYmUtNGMzOC04MTQ3LTk3OGUyMDUyZTQwNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxM1QxMzMyMjRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mYjliZmUwZjMwOGFkNTNhYjNiYzU5ZTBhYjAzNjJhMWJhYTFmNzQzZjU5YmY3ZTI1NDA3OWQwNmE3NWYwYzAzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.8ej0aMqpCHRs6pmWgrm--vCDRORSXydTxxhmrb6w5A8)


As it is possible to conclude based on the graphics above, the casual riders make longer rides on the spring/summer seasons. This is also true for members, eventhough the tendency there is not so obvious. In opposite to members, casual riders tend to ride longer at weekends.


### Total Number of Rides

![image](https://private-user-images.githubusercontent.com/158205795/304424588-8e0fd403-b6b5-4683-a170-63b86d064044.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc4MzE3MzQsIm5iZiI6MTcwNzgzMTQzNCwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0NDI0NTg4LThlMGZkNDAzLWI2YjUtNDY4My1hMTcwLTYzYjg2ZDA2NDA0NC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxM1QxMzM3MTRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hNWU4MmM3MmJiNTMxNWExY2VlMmJmZDNjNTQ3OWU0MTJiOTFmOGViYTAzNzIyNWFlYzE1MWMyYzAzZDUwNGRhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.kF1780lDblRgS-_joFhf83SJJHBuTrkAMjanl8S1mNQ)



![image](https://private-user-images.githubusercontent.com/158205795/304424685-501d0be9-b349-4933-a749-80161814cde6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc4MzE3NzMsIm5iZiI6MTcwNzgzMTQ3MywicGF0aCI6Ii8xNTgyMDU3OTUvMzA0NDI0Njg1LTUwMWQwYmU5LWIzNDktNDkzMy1hNzQ5LTgwMTYxODE0Y2RlNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjEzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxM1QxMzM3NTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iMDBmZGZlZTdiZjQxNWUyNzFjM2Y3ZjZmM2E1NzhlZDEwMDc5YjAxMWU5ZjBkMWNhMTM3OTRmOTUzMzZmODg2JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.Lri4-wh-6Ij7q81J5nFNBJW87Dtxzh7cQnxzZmAdjOY)

It is also clear that both casual and member riders seem to prefer the spring and summer seasons when regarding the total number of rides for each month. However, concerning the weekdays, the tendency of casual riders is to have more rides in the weekend while members seem to prefer the work-week.

### Locations



![image](https://private-user-images.githubusercontent.com/158205795/304031610-5f01a8eb-e64d-4f64-8cf2-1886381bdf2a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc5MzUwODEsIm5iZiI6MTcwNzkzNDc4MSwicGF0aCI6Ii8xNTgyMDU3OTUvMzA0MDMxNjEwLTVmMDFhOGViLWU2NGQtNGY2NC04Y2YyLTE4ODYzODFiZGYyYS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwMjE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDIxNFQxODE5NDFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jNWQwY2E1ZTA1ZWU3MmMxZWYzNDQ1OWMyOTFmNzQzYzgyOGUxYjhjYWFlM2E4MDRmNGIwZWUzOWEyZGFhN2U3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.J-4Kl42IYTDkCCcNgww50ymAoYQTCalslsbtWss2fL8)


In terms of location, as it is possible to see in the sequence above, the casual riders seem to prefer the riverside area of the city when they ride. The top 50 stations where more rides end are represented in the figure at the left (with the intire city of Chicago delimited with red) and these 50 stations represent only about 3% of the total stations in the dataset. Eventhough they only account for 3% of the stations, about 33% of rides end in these locations. This can be useful to reinforce the advertisement of the brand in this area, possibly through eventual partnerships with enterprises and touristic entities in the area, or through the realization of the Cyclistic company's events in order to reach out to casual riders directly, since we now have the where (riverside area of Chicago as a main focus) and the when (during the week mostly and weekends and throughout the year mainly during Spring as specially Summer). Eventhough there is no graphic representation in this presentation, the locations where casual riders usually start their trips overlap with the most popular ending trips locations, which reinforces the idea that this area is very important in terms of casual riders density.








