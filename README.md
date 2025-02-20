# SQL Project - Add Perfomance
## Project Overview
#### **Project's Titles** : `SQL Project - Add Perfomance
#### **Database** : `project6_add_perfomance`
#### **Tables** : `add_perfomance`
This project aims to demonstrate SQL skills and techniques in data analysis specifically to explore, clean, and analyze online sales data. The project consisted of setting up a online sales database, performing exploratory data analysis (EDA), and answering business questions through specific SQL queries.
## Project Objectives
#### 1) Set Up the Database :
Prepare a database for this project from previously available data
#### 2) Exploratory Data Analysis (EDA) 
carry out data exploration to better understand the dataset
#### 3) Business Question Analysis
Using SQL queries to answer several business questions related to this retail sales project
## Project Insight
This project aims to analyze the performance of each advertisement carried out on various social media platforms such as 'Instagram'
+ 'LinkedIn'
+ 'YouTube'
+ 'Facebook'
+ 'Google'

Apart from that, through this project we will analyze which platform has the highest effectiveness based on the conversion rate and this project also answers several questions related to the add performance dataset using SQL queries

## STRUCTURE 
### 1) DATABASE and TABLE SET UP
+ **Database Creation** : The first step to start this project is create the database entitled **`project6_add_perfomance`**
+ **Tables Creation** : The table used in this project is entitled **`add_perfomance`** which consists of several columns, such as Columns: campaign_id, budget int 
duration int, platform,content varchar(50), target_audience,target_gender,region,clicks,conversion, ctr,conversion_rate,cost_per_conversion
```sql
create database project6_add_perfomance;
drop table if exists add_perfomance;
create table add_perfomance
(campaign_id varchar(50),
budget int,
duration int,
platform varchar(50),
content varchar(50),
target_audience varchar(50),
target_gender varchar(50),
region varchar(10),
clicks int,
conversion int);
```
### 2) EDA and Project's Insight
This process is carried out by answering several questions related to the dataset with the aim of deepening understanding regarding the dataset
### Task 1. Show all campaigns with a budget of less than 1000
```sql
select * from add_performance
where budget <1000
ordered based on budget description;
```

### Task 2. Calculate the average duration of the campaign 
```SQL
select platform,average(duration) as average_duration 
from add_performance
where platform = 'Linkedin';
```
### Task 3. Sort campaigns by number of clicks
```SQL
select sum(clicks) as total_click,platform
from add_performance
group by platform
order by total_click desc;
```

### TASK 4. Find the campaign with the most conversions
```SQL
select * from add_performance
order by conversion desc
limits 2;
```
### Task 5. Display the number of conversions for each campaign platform
```sql
select platform, sum(conversion) as total_conversion
from add_performance 
group by platform
order by total_conversion desc;
```

### Task 6. Calculate the average conversion for each type of content (content)
```sql
select content,avg(conversion) as average_conversion
from add_performance
group by content
order by average_conversion desc;
```

### Task 7. Find out what content is most popular with men and women
```sql
select content, target_gender, count(*) as total_target
from add_performance
group by 1,2
order by target_gender desc;
```
### Task 8. Find the region with the highest number of clicks
```sql
select region, sum(clicks) as total_click 
from add_performance
group by region
order by total_click desc 
limit 1;
```

### Task 9. Calculate the Click-Through Rate (CTR) for each campaign
```sql
select campaign_id, clicks, budget,
(clicks/ifnull(budget,0)) as ctr
from add_performance;
```

### task 10. Add new columns to existing tables
```sql
alter table add_performance
add column ctr float;

update add_performance
set ctr = clicks/nullif(budget,0);
select * from add_performance;
```

### Task 11.Show conversion effectiveness by platform
```sql
select platform, sum(clicks) as total_click, sum(conversion) as total_conversion
,(sum(clicks)/nullif(sum(conversion),0)) as conversion_rate
from add_performance
group by platform
order by conversion_rate desc;
```

### task 12. Add the conversion_rate column permanently
```sql
alter table add_performance
add column conversion_rate float;

update add_performance
set conversion_rate = (clicks)/nullif((conversion),0);
```

### Task 13.Analyze the performance of high-converting, low-cost campaigns
```sql
select campaign_id, platform, content, budget, conversion 
from add_performance 
where conversion > (select avg(conversion) from add_performance)
and
budget < (select avg(budget) from add_performance)
order by budget asc;
```

### Task 14.Find the platform that has the best effectiveness in conversion
```sql
select platform, sum(conversion) as total_conversion, sum(clicks) as total_click,
(sum(conversion) * 1.0/nullif(sum(clicks),0)) as conversion_rate
from add_performance
group by platform
order by conversion_rate ;
```

### Task 15. Find content that has the best effectiveness in conversion
```sql
select content,sum(conversion) as total_conversion, sum(clicks) as total_clicks,
(sum(conversion)*1.0/nullif(sum(clicks),0)) as total_conversion
from add_performance
group by content
order by total_conversion desc;
```
### Task 16.Determine the best platform for a specific audience based on conversion rates
(For example, looking for the best platform for the ‘Millennials’ audience.)

```sql
select target_audience, platform, content, sum(conversion) as total_conversion, sum(clicks) as total_clicks,
(sum(conversion)*1.0/nullif(sum(clicks),0)) as conversion_rate
from add_performance
where target_audience ='45-54' or target_audience ='55+'
group by target_audience, platform, content
order by conversion_rate desc;
```

### Task 17. Group campaigns based on click effectiveness
(Creates "Low", "Medium", "High" categories based on the number of clicks.)

```sql
select campaign_id, clicks,
	CASE 
    WHEN clicks <15000 then 'Low'
    WHEN clicks between 15000 and 30000 then 'Medium'
    else 'High'
end as click_category
from add_performance;
```

### Task 18. Determine the campaign that has the highest cost per conversion (Cost per Conversion - CPC)
(CPC is calculated as budget / total conversions.)
```sql
SELECT campaign_id, budget, conversion,
       (budget* 1.0 / NULLIF(conversion, 0)) AS cost_per_conversion
FROM add_performance
ORDER BY cost_per_conversion DESC;

alter table add_performance
add column cost_per_conversion float;

UPDATE add_performance
set cost_per_conversion = budget*1.0/NULLIF(conversion,0);
```

### Task 19.Task 19. Determine whether there are significant differences between conversions across platforms
(Using HAVING to filter out platforms with higher than average conversions.)
```sql
select platform,sum(conversion) as total_conversion
from add_performance
group by platform
having sum(conversion) > (select avg(conversion) from add_performance)
order by total_conversion desc;
```

### Task 20. Find campaigns that run longer than the average duration and have low conversions
```sql
select campaign_id, platform, content, duration, conversion
from add_performance
where 
duration > (select avg(duration) from add_performance)
and
conversion < (select avg(conversion) from add_performance)
order by duration desc;
select avg(duration) from add_performance;
```
## CONCLUSION
This project is a form of direct application of the use of SQL for data analysis which consists of creating databases and tables, cleaning data, exploring and analyzing data, and answering several business questions using SQL queries. Through this project, it is hoped that we can improve our ability to process data using SQL and can help businesses make better decisions.

## AUTHOR - PUTU ARISMADI
This project is one of the projects I worked on to enhance my data analysis and processing skills using SQL.
For more information about me or if you're interested in connecting, feel free to reach out to me via:
+ LinkedIn: [Connect me in Linkedin](https://www.linkedin.com/in/putu-arismadi-103223223/)
+ Instagram: [Follow me in Instagram](https://www.instagram.com/arismadi._?igsh=MTVzdDFsZzQyaHpwbQ==)
