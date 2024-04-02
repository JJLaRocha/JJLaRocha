# Data Jobs Analysis

## Introduction and Context
In this project I will present the analysis I've made to compare a few jobs in the data area. As an Agricultural Engineer who recently started to enter the field of Data Analytics, there were some questions that I would like to answer for my own knowledge, hence this project! As a person quite unaware of the details and conditions of Data Jobs and, since I took the time cultivate myself a bit in that area, I would now like to know a bit more about it!

Since there are a lot of options in this area, especially if you consider that you can learn online most of the skills that are necessary to thrive in it, I've decided to do it also with the possibility of help me navigate in it with a bit more certainty, particularly learning about what people already working in the area consider to be the most important tools you must have to succeed in it.

With that being said, that is one of the criteria I chose to analyze in this project. Others, like salary, job availability and level of satisfaction of people working in the Data Analytics field in different parameters were also evaluated throughout this project! Remember that this is project of my own initiative and so all this criteria was based in my own criteria and in the topics that I wanted to run through. So, if you're also keen on learning more about this area, stick around! I will gladly share with you what I have found.

## Ask Phase
Like previously stated, my goal with this project was to compare different Data jobs according to my own criteria. In this case, the question I wanted to answer with this project were:

1. What is the average salary for each one of these jobs
2. In which countries do these jobs have more openings
3. For those countries, what is the percentage of employees working remotely
4. What industries have more openings for data jobs
5. What is the average salary in those industries
6. What is the most used programming language in these jobs
7. What percentage of these people swtiched careers into data
8. How difficult was for them to kickstart their career

## Prepare Phase

### Data Source
In this project I used data from two different datasets. Both of them are open source and were downloaded from [Kaggle](https://www.kaggle.com/). The Datasets used were the [Data Science Jobs Analysis](https://www.kaggle.com/datasets/niyalthakkar/data-science-jobs-analysis) and the [Data Analytics Job Survey](https://www.kaggle.com/datasets/yaruunknownu/job-survey).

## Process Phase

### Data Cleaning and Transformation
Data cleaning and transformation was pretty simple in this project. You can find most of steps [here](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Data%20Jobs%20Analysis/Data%20Cleaning%20and%20Transformation.md).

### Data Combining
Like previously stated, I used two different datasets. Since I wanted to present my analysis with a single dashboard, I had to import one spreadsheet into the other, which basically resumes the step of data combining in this project. Importing a spreadsheet is a very standard procedure in spreadsheets. The main thing I had to be cautious about was not to mix columns from both datasets, since they are not related, only complementary.

## Analysis Phase
Most of the analysis for this project was made with Tableau. Like stated in the introduction, my main goal was to compare the three Data Analytics jobs I was most curious about: Data Analyst, Data Scientist and Data Engineer. The criteria am I using was also defined earlier, so let's jump into it!

To each topic of the selected criteria, I will upload the tableau visualizations as .pnf file. I added several filters to the Tableau Sheets and final Dashboard, that you can check in this [link](https://public.tableau.com/app/profile/joao.rocha3459/viz/DataJobsCountry/Painel2?publish=yes), where you can actually interact with those filters, which you won't be able to do through this markdown.

### Job Availability

#### By Country (with Remote Ratio)

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Data%20Jobs%20Analysis/Images/Jobs%20Availability%20%26%20Remote%20Ratio.png)

The datasets I found on Kaggle did not have enough data for Data Jobs in my country (Portugal), so my main objective in this geographical analysis is to check, for other countries, which have more job vacancies and what is their remote ratio. This, of course, is dependent on the people who answered the surveys that constitute the datasets. However, this serves well to at least have an idea of the remote ratio for data jobs in each country. To  make this analysis a little bit more significant from the statistical point of view, I narrowed it to the countries that have more than ten representatives in the survey. You can actually see that filter in the picture above.

This remote ratio is a calculated field I have added to my dataset on Tableau. The formula I used for it was:

SUM(IF [Remote Ratio] = 'remore' THEN 1 ELSE 0 END) / COUNT([Remote Ratio]) .

It essentially divides the remote jobs by all the jobs, and then assigns it to each country in this case. When you go across the map in the dashboard you can check the number of jobs and remote ratio by putting your pointer on the country you want to analyze. Give it a try! By going over the United States in America in the Dashboard, you can check that from the 287 total jobs, 218 are on a remote basis, which makes it an interesting country for me to search for job vacancies.

#### By Industry

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Data%20Jobs%20Analysis/Images/Jobs%20by%20Industry.png)

I also wanted to check the industries where there might be more vacancies for data jobs. As you can see, the Tech, Healthcare and Finance dominate in this field, which can help me narrow down my search even more.





