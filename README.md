# Coffee Shops Data Analysis
Caffeine Form is a company that creates coffee cups from recycled material. The company sells cups to coffee shops through their website. They would prefer to partner with the shops directly. They believe that stores with more reviews will help them better market their product. This README provides information and SQL queries to perform data analysis on the Coffee Shops sample practical exam data provided on datacamp.
## Table of Contents
1. [Ensuring Data Cleansing or Scrubbing](#Ensuring-Data-Cleansing-or-Scrubbing)
   
    _a)[ Using the COALESCE function](#Using-the-COALESCE-function)_

    _b) [Using the CASE statement](url)_

3. [Identifying Min and Max Reviews for each Place Type](url)

## Ensuring Data Cleansing or Scrubbing
The data given was cleaned so that each column name matched the given criteria. This was done using either of the following queries without updating the original table to identify and replace the missing values.

## Using the COALESCE function
```
SELECT
    COALESCE("Region", 'Unknown') AS "Region",
    COALESCE("Place name", 'Unknown') AS "Place name",
    COALESCE("Place type", 'Unknown') AS "Place type",
    COALESCE(CAST("Rating" AS NUMERIC), 0) AS "Rating",
    COALESCE("Reviews", (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")) FROM coffee)) AS "Reviews",
    COALESCE("Price", 'Unknown') AS "Price",
    COALESCE("Delivery option", 'False') AS "Delivery option",
    COALESCE("Dine in option", 'False') AS "Dine in option",
    COALESCE("Takeout option", 'False') AS "Takeout option"
FROM coffee;
```

1. Missing Region were replaced with "Unknown".
2. Missing Place name were replaced with "Unknown".
3. Missing Place type were replaced with "Unknown"
4. The values in the Rating column were converted to NUMERIC while the missing values are replaced with 0.
5. The missing values were replaced with the rounded to a whole number median value.
6. Missing Price were replaced with "Unknown".
7. Missing Delivery are replaced with "False".
8. Missing Dine in option were replaced with "False".
9. Missing Takeout option were replaced with "False".

## Using the CASE statement
```
SELECT 
    CASE WHEN "Region" IS NULL THEN 'Unknown' ELSE "Region" END AS "Region",
    CASE WHEN "Place name" IS NULL THEN 'Unknown' ELSE "Place name" END AS "Place name",
    CASE WHEN "Place type" IS NULL THEN 'Unknown' ELSE "Place type" END AS "Place type",
    CASE WHEN "Rating" IS NULL THEN 0 ELSE "Rating" END AS "Rating",
    CASE WHEN "Reviews" IS NULL THEN (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")) FROM coffee) ELSE "Reviews" END AS "Reviews",
    CASE WHEN "Price" IS NULL THEN 'Unknown' ELSE "Price" END AS "Price",
    CASE WHEN "Delivery option" IS NULL THEN 'False' ELSE "Delivery option" END AS "Delivery option",
    CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE "Dine in option" END AS "Dine in option",
    CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE "Takeout option" END AS "Takeout option"
FROM coffee;   
```

The CASE statement also provides the output of the table without updating it for the above query. 
For instance, WHEN clause tests the column condition Region IS NULL (missing). If this condition is TRUE, it returns the item specified after the THEN clause which is Unknown. 
This CASE statement is then ended with an ELSE clause that returns the Region column (i.e. specified value) if all the statements are not true. 
The statement can be used as shown above to match other columns of the table.


## Identifying Min and Max Reviews for each Place Type
To identify the reviews range for each Place type, I found the minimum and maximum Review for each Place using the following query:
```
SELECT "Place type", 
MAX("Reviews") AS max_review, 
MIN("Reviews") AS min_review
FROM coffee
GROUP BY "Place type";
```
This query groups by Place type to display the lowest and highest reviews for each Place type.
