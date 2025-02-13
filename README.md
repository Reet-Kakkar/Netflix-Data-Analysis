# Netflix-Data-Analysis using SQL
This project focuses on a comprehensive analysis of Netflix’s movies and TV shows dataset using SQL. The primary goal is to extract valuable insights by answering business-relevant questions related to content distribution, ratings, release years, countries, durations, and specific categorization.

# Objectives:
1. Analyze the distribution of Netflix content into movies and TV shows to identify trends
2. Analyze Netflix content based on release years, countries, and durations to explore content popularity and trend over time
3. Identify the most common ratings for movies and TV shows to uncover patterns and viewer preferences.

Kaggle Dataset Link: ([LINK](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download))

# Table Creation
```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```

# Business Problems and Solutions:
## 1. Count the Number of Movies vs TV shows

```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY 1;
```
(To Determine the distribution of content types on Netflix)
## 2. Find the Most Common Rating for Movies and TV Shows

```sql
WITH RatingCounts AS (
    SELECT 
        type,
        rating,
        COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT 
        type,
        rating,
        rating_count,
        RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT 
    type,
    rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;
```
(To identify the most frequently occurring rating for each type of content.)

## 3. List All Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT * 
FROM netflix
WHERE release_year = 2020;
```
(To find out the movies released in the particular year =2020)

## 4.  Find the Top 5 Countries with the Most Content on Netflix

```sql
SELECT * 
FROM
(
    SELECT 
        UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
        COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
```
(To find those 5 countries with the highest content on Netflix)

## 5.  Identify the Longest Movie

```sql
SELECT 
    *
FROM netflix
WHERE type = 'Movie'
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```
(To find out the longest movie)

## 6. Find Content Added in the Last 5 Years

```sql
SELECT *
FROM netflix
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years';
```
(To find the latest content added in the last 5 years)

## 7. Find All Movies/TV Shows by Director 'Rajiv Chilaka'

```sql
SELECT *
FROM (
    SELECT 
        *,
        UNNEST(STRING_TO_ARRAY(director, ',')) AS director_name
    FROM netflix
) AS t
WHERE director_name = 'Rajiv Chilaka';
```
(To find Movies and Tv shows by director Rajiv Chilaka)

## 8. List All TV Shows with More Than 5 Seasons

```sql
SELECT *
FROM netflix
WHERE type = 'TV Show'
  AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```
(To find TV shows with more than 5 seasons)

## 9. Count the Number of Content Items in Each Genre

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
    COUNT(*) AS total_content
FROM netflix
GROUP BY 1;
```
(Count the number of content generated in each genre)

## 10.  List All Movies that are Documentaries
```sql
SELECT * 
FROM netflix
WHERE listed_in LIKE '%Documentaries';
(To calculate and find )
```
(To find out documentary movies)

## 11. Find How Many Movies Actor 'Salman Khan' Appeared in the Last 10 Years

```sql
SELECT * 
FROM netflix
WHERE casts LIKE '%Salman Khan%'
  AND release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```
(To find out the number of movies in which Salman Khan appeared in Last 10 years)

## 12. Categorize Content Based on the Presence of 'Kill' and 'Violence' Keywords

```sql
SELECT 
    category,
    COUNT(*) AS content_count
FROM (
    SELECT 
        CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```
(To categorize movies based on the keywords)

13. Find the Top 10 Actors Who Have Appeared in the Highest Number of Movies Produced in India

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,
    COUNT(*)
FROM netflix
WHERE country = 'India'
GROUP BY actor
ORDER BY COUNT(*) DESC
LIMIT 10;
```
(To find out the top 10 actors with the most appearances in Indian-produced movies.)

# Conclusion:

This project successfully utilized SQL to analyze Netflix's movie and TV show dataset, answering key business-relevant questions about the distribution of content types, viewer preferences, content release patterns, and specific categorizations based on keywords. Through SQL queries, we were able to uncover insights such as the most common ratings, top countries with the most Netflix content, and the longest movie in the catalog. This comprehensive analysis provided valuable insights into the content structure of Netflix, allowing for better understanding of audience preferences.
