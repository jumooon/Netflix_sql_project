# Netflix Movies and TV shows Data Analysis using PostgreSQL
![Netflix logo](https://github.com/jumooon/Netflix_sql_project/blob/main/logo.png)
## Overview
This project presents a comprehensive analysis of Netflix’s catalog of movies and television shows using SQL. The primary aim is to investigate the dataset in order to extract meaningful insights and address a series of business-oriented questions regarding Netflix’s content distribution and characteristics.

## Objectives
1. Analyze the distribution of content types, specifically comparing movies and television shows
2. Identify the most frequently occurring ratings across both categories
3. Examine content trends with respect to release years, countries of origin, and durations
4. Categorize and explore content based on defined criteria and relevant keywords

## Datasets
This data is sourced from Kaggle
Over 8000 Movies and TV shows
Data Set : [Kaggle link](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)
## Schemas
```sql
drop table if exists netflix;
create TABLE netflix (
	show_id varchar(6) NULL,
	"type" varchar(10) NULL,
	title varchar(150) NULL,
	director varchar(208) NULL,
	"cast" varchar(1000) NULL,
	country varchar(150) NULL,
	date_added varchar(50) NULL,
	release_year int4 NULL,
	rating varchar(10) NULL,
	duration varchar(15) NULL,
	listed_in varchar(100) NULL,
	description varchar(250) NULL
);
```
## Business problems and solutions
1. Count the number of Movies vs TV Shows
```sql
select
distinct type
from netflix;
select * from netflix;
```
2. Find the most common rating for movies and TV shows
```sql
select type, rating
from
(select "type", rating, count(rating), rank() over(partition by type order by count(*) desc) as ranking
from netflix
group by 1, 2) as table1
where ranking = 1
;
```
3. List all movies released in a specific year (e.g., 2020)
```sql
select title
from netflix
where release_year = 2020;
```
4. Find the top 5 countries with the most content on Netflix
```sql
select unnest(string_to_array(country, ',')) as new_country, count(*) as num_contents, rank() over(order by count(*) desc)
from netflix
group by new_country
limit 5;
```
5. Identify the longest movie
```sql
select title, "type", duration
from netflix
where "type" = 'Movie'
group by title, "type", duration
order by duration desc;
```
6. Find content added in the last 5 years
```sql
select title, date_added
from netflix
where
	TO_DATE(date_added, 'Month DD, YYYY') >= current_date - interval '5 years';
```	
7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
```sql
select title, director
from netflix 
where director Ilike '%Rajiv Chilaka%'
```
8. List all TV shows with more than 5 seasons
```sql
select *
from netflix
where
	type = 'TV Show' and split_part(duration, ' ',1)::int > 5 ;
```
9. Count the number of content items in each genre
```sql
select  unnest(string_to_array(listed_in, ',')) as genre, count(*)
from netflix
group by unnest(string_to_array(listed_in, ','))
```
10.Find each year and the average numbers of content release in India on netflix.
return top 5 year with highest avg content release
```sql
select extract(year from (to_date(date_added, 'month dd, yyyy'))), count(*), 
count(*):: numeric/(select count(*) from netflix where country ilike 'india'):: numeric * 100 as avg_year 
from netflix
where country ilike 'india'
group by 1
order by 2 desc;
```
11. List all movies that are documentaries
```sql
select title
from netflix
where listed_in ilike 'documentaries';
```
12. Find all content without a director
```sql
select title
from netflix
where director is null;
```
13. Find how many movies actor 'Salman Khan' appeared in last 10 years!
```sql
select count(*) as num_movies
from netflix 
where "cast" ilike '%Salman Khan%' and release_year >= extract(year from current_date) - 10;
```
14. Find the top 10 actors who have appeared in the highest number of movies produced in India.
```sql
select unnest(string_to_array("cast", ',')), count(*)
from netflix
where country ilike '%india%'
group by 1
order by 2 desc
limit 10;
```
15. Categorize the content based on the presence of the keywords 'kill' and 'violence' in 
the description field. Label content containing these keywords as 'Bad' and all other 
content as 'Good'. Count how many items fall into each category.
```sql
with new_table
as
(
select *,
	case 
	when description ilike '%kill%' 
	or description ilike '%violence%' then 'BAD'
	else 'Good'
	end category
from netflix )
select category, count(*) as total_content
from new_table
group by category;
```
