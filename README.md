# Netflix-Data-Analysis using SQL
This project focuses on a comprehensive analysis of Netflix’s movies and TV shows dataset using SQL. The primary goal is to extract valuable insights by answering business-relevant questions related to content distribution, ratings, release years, countries, durations, and specific categorization.

# Objectives:
1. Analyze the distribution of Netflix content into movies and TV shows to identify trends
2. Analyze Netflix content based on release years, countries, and durations to explore content popularity and trend over time
3. Identify the most common ratings for movies and TV shows to uncover patterns and viewer preferences.

Kaggle Dataset Link: ([LINK](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download))

# SCHEMA
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
