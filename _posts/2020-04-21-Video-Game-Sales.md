---
title: "Video Game Sales"
date: 2020-04-21
tags: [SQL Server, Tableau]
#header:
#  image: ""
excerpt: "SQL server database and Tableau Visualisation"
---

# Video Game Sales Project
Here I wanted to showcase the use of MS SQL server alongside Tableau, as they work very well together and you can easily visualise datasets.

# Introduction
I used MS SQL server in this project, alongside Tableau. Importing the csv data file into the server enabled me to directly link this to Tableau.

I chose the dataset of video games as I thought I could get good insight and ask some good questions of the data in order to obtain some conclusions as an exercise for myself.

In this I used what I normally use to help me organise and plan out projects, Trello. Which I will put as a link so that you can see how I worked through the data and my thought process behind what I did.

# What I want to find out
- Which game platform is most successful and does this change based on the region/year
- What is the general trend in sales over time, according to this data
- What genre of game is most successful by year and region
- Who is the most successful publisher, and what is this linked to? A number of titles over multiple genres or less so.

# Set up
The first problem I encountered was that some of the names were continuing on into the next column due to them having a comma inside the name. This was easy to fix as I just made sure that there was a text qualifier present and surrounded the names of the titles in speech marks to make this work. 

After doing this I made sure that the various columns were formatted correctly and would work well in later analysis such as ensuring the Year column was formatted to be an integer value, rather than a date as there was only the year present in the data.

# My workthrough
I made stored procedures to easily contain a number of queries. The queries consisted of a number of CTEs and pivot statements where I try making the data more readable to draw conclusions from it.
For example, one procedure shown here:

```sql
CREATE PROCEDURE [dbo].[top_5]
AS
BEGIN
	SELECT Year, Nintendo,[Electronic Arts],Activision,[Sony Computer Entertainment],Ubisoft FROM
	(SELECT Publisher, Year, Global_Sales FROM vgsales)Tab1
	PIVOT
(
		SUM(Global_Sales) FOR Publisher IN (Nintendo,[Electronic Arts],Activision,[Sony Computer Entertainment],Ubisoft)) AS Tab2
	ORDER BY [Tab2].Year

	SELECT Year, PS2,X360,PS3,Wii,DS FROM
	(SELECT Platform, Year, Global_Sales FROM vgsales)Tab1
	PIVOT
(
		SUM(Global_Sales) FOR Platform IN (PS2,X360,PS3,Wii,DS)) AS Tab2
	ORDER BY [Tab2].Year

	SELECT Year, Action,[Sports],Platform,[Role-Playing],[Shooter] FROM
	(SELECT Genre, Year, Global_Sales FROM vgsales)Tab1
	PIVOT
(
		SUM(Global_Sales) FOR Genre IN (Action,[Sports],Platform,[Role-Playing],[Shooter])) AS Tab2
	ORDER BY [Tab2].Year
END;
```

This takes the top 5 contenders for each main variable, finding the top publishers, genres and game platforms.

# Analyses
Using Tableau Public I visualised the data in a way to answer the questions I initially came up with when considering data of this sort.
I am going to link to a number of visualisations I made that are published on my Tableau account.

[Sales-Over-Time](https://public.tableau.com/profile/dillon6327#!/vizhome/vgsales_15891212354570/Dashboard1)

[Top-5-Platforms](https://public.tableau.com/profile/dillon6327#!/vizhome/vgsales_15891212354570/Dashboard2)

[Top-5-Genres](https://public.tableau.com/profile/dillon6327#!/vizhome/vgsales_15891212354570/Dashboard3)
