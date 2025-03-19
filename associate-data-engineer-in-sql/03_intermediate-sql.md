## [Intermediate SQL](https://app.datacamp.com/learn/courses/intermediate-sql)

-------------------------------------------------------------
INTERMEDIATE SQL                                            |
-------------------------------------------------------------

COUNT()
- Counts the number of records with a value in a field
- Use an alias for clarity
------------------------------------------------
SELECT COUNT(birthdate) AS count_birthdates    |
FROM people;                                   |
------------------------------------------------

------------------------------------------------------------------------
SELECT COUNT(name) AS count_names, COUNT(birthdate) AS count_birthdates |
FROM people;                                                            |
------------------------------------------------------------------------

COUNT(field_name) counts values in a field.
COUNT(*) counts records in a table.

----------------------------------
SELECT COUNT(*) AS total_records  |
FROM people;                      |
----------------------------------

-------------------------------------------------------
SELECT COUNT(DISTINCT birthdate) AS unique_birthdates  |
FROM people;                                           |
-------------------------------------------------------

Query execution:
Order of execution:
- SQL is not processed in its written order

--Order of execution

SELECT name (2)
FROM people (1)
LIMIT 10; (3)

Most common errors:
- Misspelling
- Incorrect capitalization
- Incorrect or missing punctuation, especially commas

SQL formatting
- Formatting is not required but lack of formatting can cause issues

Best practice:
- Capitalize keywords
- Add new lines

Dealing with non-standars field names:
- release year instead of release_year
- Put non-standard field names in double-quotes -> "release year"

Why do we format?
- Easier collaboration
- Clean and readable
- Looks professional
- Easier to understand
- Easier to debug

Filtering numbers:
WHERE clause:
- Filters records based on a condition and comparison operators

----------------------------
SELECT title                |
FROM films                  |
WHERE release_year > 2000;  |
----------------------------

----------------------------
SELECT title                |
FROM films                  |
WHERE country = 'Japan';    |
----------------------------

-- Order of execution
SELECT item (3)
FROM coats (1)
WHERE color = 'green' (2)
LIMIT 5; (4)

---------------------------------------------------
-- Count the records with at least 100,000 votes   |
SELECT COUNT(num_votes) AS films_over_100K_votes   |
FROM reviews                                       |
WHERE num_votes >= 100000;                         |
---------------------------------------------------

Multiple criteria:
- OR, AND, BETWEEN (inclusive, nilai awal dan akhir masuk)
-----------------------------------------------
SELECT *                                       |
FROM coats                                     |
WHERE color = 'yellow' OR length = 'short';    |
-----------------------------------------------

------------------------------------------------
SELECT *                                       |
FROM coats                                     |
WHERE color = 'yellow' AND length = 'short';   |
------------------------------------------------

-----------------------------------
SELECT *                           |
FROM coats                         |
WHERE buttons BETWEEN 1 AND 5;     |
-----------------------------------
--------------------------------------------------------
SELECT title                                            |
FROM films                                              |
WHERE (release_year = 1994 OR release_year = 1995)      |
     AND (certification = 'PG' OR certification = 'R'); |
--------------------------------------------------------

---------------------------------------------------------
SELECT title, release_year                               |
FROM films                                               |
WHERE (release_year = 1990 OR release_year = 1999)       |
	AND (language = 'English' OR language = 'Spanish')   |
-- Filter films with more than $2,000,000 gross          |
	AND gross > 2000000;                                 |
---------------------------------------------------------

------------------------------------------------------------------
SELECT title, release_year                                        |
FROM films                                                        |
WHERE release_year BETWEEN 1990 AND 2000                          |
	AND budget > 100000000                                        |
-- Amend the query to include Spanish or French-language films    |
	AND (language = 'Spanish' OR language = 'French');            |
------------------------------------------------------------------

Filtering text:
- Filter a pattern rather than specific text
- LIKE, NOT LIKE, IN

LIKE:
- Used to search for a pattern in a field
- % match zero, one, or many characters
- _ match a single character

-------------------------------------------
-- Find all name that start with 'Ade'    |
SELECT name                               |
FROM people                               |
WHERE name LIKE 'Ade%'; -> Adelia         |
-------------------------------------------

------------------------------------------
-- Find all name that start with 'Ev'     |
SELECT name                               |
FROM people                               |
WHERE name LIKE 'Ev_'; -> Eve             |
------------------------------------------

NOT LIKE:
---------------------------------------
SELECT name                            |
FROM people                            |
WHERE name NOT LIKE 'A.%'; -> Aaliyah  |
---------------------------------------

---------------------------------
SELECT name                      |
FROM people                      |
WHERE name LIKE '%r'; -> Aaron   |
---------------------------------

-------------------------------------
SELECT name                          |
FROM people                          |
WHERE name LIKE '__t%'; -> Anthony   |
-------------------------------------

------------------------------------------
SELECT title                              |
FROM films                                |
WHERE release_year IN (1920, 1930, 1940); |
------------------------------------------

---------------------------------------------------------------------------------------------------------------------
-- Find the title, certification, and language all films certified NC-17 or R that are in English, Italian, or Greek |
SELECT title, certification, language                                                                                |
FROM films                                                                                                           |
WHERE certification IN ('NC-17', 'R') AND language IN ('English', 'Italian', 'Greek')                                |
---------------------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------
-- Count the unique titles                                         |
SELECT COUNT(DISTINCT title) AS nineties_english_films_for_teens   |
FROM films                                                         |
-- Filter to release_years to between 1990 and 1999                |
WHERE release_year BETWEEN 1990 AND 1999                           |
-- Filter to English-language films                                |
	AND language = 'English'                                       |
-- Narrow it down to G, PG, and PG-13 certifications               |
	AND certification IN ('G', 'PG', 'PG-13');                     |
-------------------------------------------------------------------

NULL values;
- Missing values 
COUNT (field_name) includes only non-missing values
COUNT(*) includes missing values

----------------------------------
SELECT COUNT(*) AS count_records |
FROM people; -> 8397             |
----------------------------------

--------------------------------------------
-- Count the records with null birthdates   |
SELECT name                                 |
FROM people                                 |
WHERE birthdate IS NULL;                    |
--------------------------------------------

-------------------------------------
SELECT COUNT(*) AS no_birthdates    |
FROM people                         |
WHERE birthdate IS NULL; - 2245     |
-------------------------------------

----------------------------------------
SELECT COUNT(name) AS count_birthdates  |
FROM people                             |
WHERE birthdate IS NOT NULL;            |
----------------------------------------

Summarizing Data:
- aggregate functions return a single value
- AVG(), SUM(), MIN(), MAX(), COUNT()

---------------------
SELECT AVG(budget)   |
FROM films;          |
---------------------

---------------------
SELECT SUM(budget)  |
FROM films;         |
---------------------

Summarizing subsets:
----------------------------------
SELECT AVG(budget) AS avg_budget  |
FROM films                        |
WHERE release_year >= 2010;       |
----------------------------------

ROUND() -> round the result to specific decimal places
ROUND(number_to_round, decimal_places)

Aggregate functions vs. arithmetic:
Aggregate = vertikal
arithmatic = horizontal

Order of execution:
- FROM
- WHERE
- SELECT (aliases are defined here)
- LIMIT

- Aliases defined in the SELECT clause cannot be used in the WHERE clause due to order of execution

-- Calculate the percentage of people who are no longer alive
---------------------------------------------------------------
SELECT COUNT(deathdate) * 100.0 / COUNT(*) AS percentage_dead |
FROM people;                                                  |
---------------------------------------------------------------

-- Find the number of decades in the films table
-----------------------------------------------------------------------------
SELECT (MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades   |
FROM films;                                                                  |
-----------------------------------------------------------------------------

Sorting results:
--------------------------------------------
SELECT title, budget                       |
FROM films                                 |
ORDER BY bugdet DESC; (besar ke kecil)     |
--------------------------------------------

Order of execution:
- FROM
- WHERE
- SELECT
- ORDER BY
- LIMIT

Grouping data:
--------------------------------------------------
SELECT certification, COUNT(title) AS title_count |
FROM films                                        |
GROUP BY certification                            |
ORDER BY title_count DESC;                        |
LIMIT 3;                                          |
--------------------------------------------------

Order of execution:
- FROM
- GROUP BY
- SELECT
- ORDER BY
- LIMIT

Filtering grouped data:
HAVING clause:
--------------------------------------------------
SELECT release_year, COUNT(title) AS title_count  |
FROM films                                        |
GROUP BY release_year                             |
HAVING COUNT(title) > 10;                         |
--------------------------------------------------

- Written code:
----------------------------------------------------
SELECT certification, COUNT(title) AS title_count  |
FROM films                                         |
WHERE certification IN ('G', 'PG', 'PG-13')        |
GROUP BY certification                             |
HAVING COUNT(title) > 10                           |
ORDER BY title_count DESC;                         |
LIMIT 3;                                           |
----------------------------------------------------

-----------------------------
Final Order of execution:    |
- FROM                       |
- WHERE                      |
- GROUP BY                   |
- HAVING                     |
- SELECT                     |
- ORDER BY                   |
- LIMIT                      |
-----------------------------

HAVING vs. WHERE:
- WHERE filters individual records, HAVING filters groups
------------------------------------------------------------------------------
-- Select the country and average_budget from films                          |
SELECT country, AVG(budget) AS average_budget                                |
FROM films                                                                   |
-- Group by country                                                          |
GROUP BY country                                                             |
-- Filter to countries with an average_budget of more than one billion       |
HAVING AVG(budget) > 1000000000                                              |
-- Order by descending order of the aggregated budget                        |
ORDER BY average_budget DESC;                                                |
------------------------------------------------------------------------------

-- EXTRACT()
SELECT EXTRACT(YEAR FROM launch) AS year,
       COUNT(*) AS number_of_speakers
FROM speaker
GROUP BY year
ORDER BY number_of_speakers DESC;

-- SUBSTRING()
LEFT()
SELECT LEFT(zip_code, 5) AS zip_code
FROM customer_data;

-- Ubah data type
SELECT station_id::int, 
       station_name::varchar
FROM station_data;

-- LIKE dan ILIKE
LIKE -> case sensitive
ILIKE -> case insensitive
NOT LIKE -> tidak sama
NOT ILIKE -> tidak sama case insensitive

-- Extract jam dan ubah data type
SELECT trip_id, 
         EXTRACT(HOUR FROM start_time) :: NUMERIC AS start_hour
         EXTRACT(HOUR FROM end_time) :: NUMERIC AS end_hour

-- ~ operator untuk regex
SELECT * 
FROM customer_data
WHERE zip_code ~ '^[0-9]{5}$';

SELECT * 
FROM movie 
WHERE title ~ '^The';

-- Split part 
Split data seperti ini -> Transformers: Age of Extinction (2014)
SELECT SPLIT_PART(title, ':', 1) AS title
SPLIT_PART(title, ':', 2) AS series
FROM movie;

-- OFFSET dan LIMIT
OFFSET -> skip rows
LIMIT -> limit rows

SELECT variety, date, price
FROM fruit_2022
ORDER BY price DESC
OFFSET 1; // skip 1 row not row 1

-- Subquery
SELECT COUNT(*) A above_avg
FROM speaker
WHERE rice > (SELECT AVG(price) FROM speaker);