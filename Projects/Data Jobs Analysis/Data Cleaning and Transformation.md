## Data Cleaning and Transformation

First of all, I removed all duplicates with the Sheets function “Remove Duplicates”. Since I only wanted jobs related to the Data Analytics field, I created filters with the criteria “ does not contain “data”” and “contain “internship””  to remove all unrelated jobs. I also removed all internship positions, since they don’t interest me, following the same steps. Also, I have used the “unique” and “countif” functions to determine how many people for each position have completed the survey, and also to remove the outliers, i.e, removed all positions who only had one or few people in the survey (statistically irrelevant), as an example. I also used the spreadsheets filters to get rid of irregularities like typos, trim cells and stuff like that. Let’s say there were 2 answers to the survey that types “Data Analystt” instead of Data Analyst”. With the unique function combined with the filter for that column, I was able to uniformize all that data.

I only wanted to analyze the following jobs: Data Analyst, Data Engineer and Data Scientist, since those are the ones that I am interested in. To do so, I’ve added a filter to the current job column to leave only all the remaining jobs of that dataset, and then I deleted all those rows (109 total).

I forgot to transform my Excel table in the Industry column to leave only significant industries (count>10) and to change the names (ex: “Retail” was “Other: Please Specify(Retail)”. So I added a Filter in Tableau for Industry for count by Industry superior to 10 and created a calculated field to change the name into “Retail”. The code to that calculated field was:

IF [Industry] = “Other: Please Specify(Retail)” THEN “Retail” ELSE [Industry] END

