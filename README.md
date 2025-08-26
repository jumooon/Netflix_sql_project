# Netflix Movies and TV shows Data Analysis using Postgresql
![Netflix logo](https://github.com/jumooon/Netflix_sql_project/blob/main/logo.png)
## Overview
This project presents a comprehensive analysis of Netflix’s catalog of movies and television shows using SQL. The primary aim is to investigate the dataset in order to extract meaningful insights and address a series of business-oriented questions regarding Netflix’s content distribution and characteristics.

## Objectives
1. Analyze the distribution of content types, specifically comparing movies and television shows
2. Identify the most frequently occurring ratings across both categories
3. Examine content trends with respect to release years, countries of origin, and durations
4. Categorize and explore content based on defined criteria and relevant keywords

## Schemas
'''sql
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
);'''
