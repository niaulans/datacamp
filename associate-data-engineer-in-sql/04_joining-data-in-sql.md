## [Joining Data in SQL](https://app.datacamp.com/learn/courses/joining-data-in-sql)

-------------------------------------------------------------
JOINING DATA IN SQL                                         |
-------------------------------------------------------------

INNER JOIN 
- key field that uniquely identifies each record
- INNER JOIN looks for records in both tables which match on a given field

-------------------------------------------------------------------------------------
--Inner join of presidents and prime_minister, joining on country                    |
SELECT prime_minister.country, prime_minister.continent, prime_minister, president   |
FROM presidents                                                                      |
INNER JOIN prime_minister                                                           |
ON presidents.country = prime_minister.country;                                      |
-------------------------------------------------------------------------------------

Note. The table.column_name format must be used when selecting columns that exist in both tables to avoid a SQL error.

Aliasing tables:
----------------------------------------------------------------------
--Inner join of presidents and prime_ministers, joining on country    |
SELECT p2.country, p2.continent, prime_minister, president            |
FROM presidents AS p1                                                 |
INNER JOIN prime_ministers AS p2                                      |
ON p1.country = p2.country;                                          |
----------------------------------------------------------------------

Using USING:
--------------------------------------------------------------------
--Inner join of presidents and prime_ministers, joining on country |
SELECT p2.country, p2.continent, prime_minister, president         |
FROM presidents AS p1                                              |
INNER JOIN prime_ministers AS p2                                   |
USING(country);                                                    |
--------------------------------------------------------------------

-----------------------------------------------------------
-- Select fields with aliases                              |
SELECT c.code AS country_code, name, year, inflation_rate  |
FROM countries AS c                                        |
-- Join to economies (alias e)                             |
INNER JOIN economies AS e                                  |
-- Match on code field using table aliases                 |
ON c.code = e.code                                         |
-----------------------------------------------------------

Defining relationships:
- One-to-many relationships:
example: Jane Austen -> Persuasion, Pride and Prejudice, Emma
1 author can write many books

- One-to-one relationships:
example: Finger -> fingerprint
1 finger can have 1 fingerprint

- Many-to-many relationships:
examples: German -> Germany, Belgium, French -> Belgium 
many languages can be spoken in many countries

----------------------------------------------------------
-- Select country and language names (aliased)           |
SELECT c.name AS country, l.name AS language             |
-- From countries (aliased)                              |
FROM countries AS c                                      |
-- Join to languages (aliased)                           |
INNER JOIN languages AS l                                |
-- Use code as the joining field with the USING keyword  |
USING(code);                                             |
----------------------------------------------------------

Multiple joins:
- Joins on joins
------------------------------------------
SELECT *                                 |
FROM left_table                          |
INNER JOIN right_table                   |
ON left_table.id = right_table.id        |
INNER JOIN another_table                 |
ON left_table.id = another_table.id;     |
------------------------------------------

------------------------------------------
- Joining on multiple key                 |
SELECT *                                  |
FROM left_table                           |
INNER JOIN right_table                    |
ON left_table.id = right_table.id         |
AND left_table.name = right_table.name;   |
------------------------------------------

-----------------------------------------------------------
-- Select fields                                           |
SELECT name, e.year, fertility_rate, unemployment_rate     |
FROM countries AS c                                        |
INNER JOIN populations AS p                                |
ON c.code = p.country_code                                 |
-- Join to economies (as e)                                |
INNER JOIN economies AS e                                  |
-- Match on country code                                   |
ON p.country_code = e.code;                                |
-----------------------------------------------------------

----------------------------------------------------------------------------------
SELECT name, e.year, fertility_rate, unemployment_rate                           |
FROM countries AS c                                                              |
INNER JOIN populations AS p                                                      |
ON c.code = p.country_code                                                       |
INNER JOIN economies AS e                                                        |
ON c.code = e.code                                                               |
-- Add an additional joining condition such that you are also joining on year    |
	AND p.year = e.year;                                                         |
----------------------------------------------------------------------------------

Outer Joins, Cross Joins and Self Joins
- Left and Right joins
Left join:
- return all records in the left table, and those records in the right table that match on the joining fields provided

---------------------------------------
SELECT p1.country, prime_minister     |
FROM presidents AS p1                 |
LEFT JOIN prime_ministers AS p2       |
USING(country);                       |
---------------------------------------
Note. LEFT JOIN can also be written as LEFT OUTER JOIN.

Right join:
- return all records in the right table, and those records in the left table that match on the joining fields provided

---------------------------------------
SELECT *                              |
FROM left_table                       |
RIGHT JOIN right_table                |
ON left_table.id = right_table.id;    |
---------------------------------------
Note. RIGHT JOIN can also be written as RIGHT OUTER JOIN.

---------------------------------------
SELECT p1.country, prime_minister     |
FROM presidents AS p1                 |
RIGHT JOIN prime_ministers AS p2       |
USING(country);                       |
---------------------------------------

----------------------------------------------------------------------
-- Modify this query to use RIGHT JOIN instead of LEFT JOIN           |
SELECT countries.name AS country, languages.name AS language, percent |
FROM languages                                                        |
RIGHT JOIN countries                                                  |
USING(code)                                                           |
ORDER BY language;                                                    |
----------------------------------------------------------------------

Full join:
- A full join combines a left join and a right join
- It returns all records when there is a match in either left (table1) or right (table2) table records

------------------------------------------------------
SELECT left_table.id AS L_id, right_table.id AS R_id  |
FROM left_table                                       |
FULL JOIN right_table                                 |
ON L_id = R_id;                                       |
------------------------------------------------------
Note. FULL JOIN can also be written as FULL OUTER JOIN.

-----------------------------------------------
SELECT p1.country, prime_minister, president   |
FROM presidents AS p1                          |
FULL JOIN prime_ministers AS p2                |
USING(country);                                |
LIMIT 10;                                      |
-----------------------------------------------

---------------------------------------------------
SELECT name AS country, code, region, basic_unit  |
FROM countries                                    |
-- Join to currencies                             |
FULL JOIN currencies                              |
USING (code)                                      |
-- Where region is North America or name is null  |
WHERE region = 'North America' OR name IS NULL    |
ORDER BY region;                                  |
---------------------------------------------------

---------------------------------------------------
SELECT                                             |
	c1.name AS country,                            |
    region,                                        |
    l.name AS language,                            |
	basic_unit,                                    |
    frac_unit                                      |
FROM countries as c1                               |
-- Full join with languages (alias as l)           |
FULL JOIN languages AS l                           |
USING(code)                                        |
-- Full join with currencies (alias as c2)         |
FULL JOIN currencies as c2                         |
USING(code)                                        |
WHERE region LIKE 'M%esia';                        |
---------------------------------------------------

Cross join:
- CROSS JOIN creates all possible combinations of two tables

--------------------
SELECT id1, id2    |
FROM table1        |
CROSS JOIN table2; |
--------------------

----------------------------------------
SELECT prime_minister, president       |
FROM prime_ministers AS p1             |
CROSS JOIN presidents AS p2;           |
WHERE p1.continent IN ('Asia)          |
AND p2.continent IN ('North America'); |
----------------------------------------

Self join:
- Self joins are tables joined with themselves
- They can be used to compare parts of the same table

-------------------------------------------------------
SELECT p1.country AS country1, p2.country AS country2  |
FROM prime_ministers AS p1                             |
INNER JOIN prime_ministers AS p2                       | 
ON p1.continent = p2.continent                         |
AND p1.country <> p2.country                           |
LIMIT 10;                                              |
-------------------------------------------------------

----------------------------------------------------------------------
-- Select aliased fields from populations as p1                       |
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015      |
FROM populations AS p1                                                |
-- Join populations as p1 to itself, alias as p2, on country code     |
INNER JOIN populations AS p2                                          |
ON p1.country_code = p2.country_code;                                 |
----------------------------------------------------------------------

----------------------------------------------------------------
SELECT                                                          |
    p1.country_code,                                            |
    p1.size AS size2010,                                        |
    p2.size AS size2015                                         |
	p1.country_code,                                            |
    p1.size AS size2010,                                        |
    p2.size AS size2015                                         |
FROM populations AS p1                                          |
INNER JOIN populations AS p2                                    |
ON p1.country_code = p2.country_code                            |
WHERE p1.year = 2010                                            |
-- Filter such that p1.year is always five years before p2.year |
    AND p1.year = p2.year -5;                                   |
----------------------------------------------------------------

INNER JOIN:
You sell house and have two tables, listing prices and price_sold.
You want a table with sale prices and listing prices, only if you know both.

LEFT JOIN:
You run a pizza delivery service with loyal clients. You want a table of clients and their weekly orders, with nulls if there are 
no orders.

FULL JOIN:
You want a report of wheter your parents have reached out to you, or you have reached out to them. 
You are fine with nulls either condition.

Set theory for SQL Joins: => both tables must have the same number of columns
- UNION
- UNION takes two tables as input, and returns all records from both tables
---------------------------
SELECT *                   |
FROM left_table            |
UNION                      |
SELECT *                   |
FROM right_table;          |
---------------------------

- UNION ALL
- UNION ALL returns all records from both tables, but does not remove duplicates
---------------------------
SELECT *                   |
FROM left_table            |
UNION ALL                  |
SELECT *                   |
FROM right_table;          |
---------------------------

SELECT monarch AS leader, country
FROM monarchs
UNION
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;

SELECT monarch AS leader, country
FROM monarchs
UNION ALL
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;

Note. Both queries on the left and right of the set operation must have the same data types. The names of the fields do not need to be the same, as the result will always contain field names from the left query.

-- Query that determines all pairs of code and year from economies and populations, without duplicates
SELECT code, year
FROM economies
UNION 
SELECT country_code, year
FROM populations
ORDER BY code, year;

INTERSECT:
- INTERSECT returns all records that are in both tables

SELECT id, val
FROM left_table
INTERSECT
SELECT id, val
FROM right_table;

-Countries with prime ministers and president
SELECT country AS intersect_country
FROM prime_ministers
INTERSECT
SELECT country
FROM presidents;

-- Return all cities with the same name as a country
SELECT name
FROM cities
INTERSECT 
SELECT name
FROM countries;

EXCEPT:
- EXCEPT returns all records from the first table that are not in the second table

SELECT id, val
FROM left_table
EXCEPT
SELECT id, val
FROM right_table;

-- Return all cities that do not have the same name as a country
SELECT name
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;

Subqueries:
- Subqueries are queries nested within other queries

Subquerying with semi joins and anti joins:
Semi join:
- Semi join returns all records from the left table where a condition is met in the right table
- Return all values from left_table where values of col1 are in right_table

SELECT president, country, continent
FROM presidents
WHERE country IN
    (SELECT country
     FROM states
     WHERE indep_year < 1800;)

Anti join:
- Anti join returns all records from the left table where a condition is not met in the right table

-- Return all countries that are in America and indep_year is after 1800
SELECT country, president
FROM presidents
WHERE continent LIKE '%America'
AND country NOT IN
    (SELECT country
     FROM states
     WHERE indep_year < 1800;)

SELECT DISTINCT name
FROM languages
-- Add syntax to use bracketed subquery below as a filter
WHERE code IN
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;

Subqueries inside WHERE and SELECT:
- All semi joins and anti joins can be written as subqueries inside WHERE
- WHERE is the most common place to use subqueries

Subqueries inside WHERE:
SELECT * 
FROM some_table
WHERE some_field IN 
    (SELECT some_numeric_field
     FROM another_table
     WHERE another_field = 'condition');

Subqueries inside SELECT:
SELECT DISTINCT continet,
    (SELECT COUNT(*)
    FROM monarchs
    WHERE states.continent = monarchs.continent) AS monarchs_count
FROM states;

SELECT *
FROM populations
WHERE year = 2015
-- Filter for only those populations where life expectancy is 1.15 times higher than average
  AND life_expectancy > 1.15 *
  (SELECT AVG(life_expectancy)
   FROM populations
   WHERE year = 2015);

-- Select relevant fields from cities table
SELECT name, country_code, urbanarea_pop
FROM cities
-- Filter using a subquery on the countries table
WHERE name IN
    (SELECT capital
     FROM countries
    )
ORDER BY urbanarea_pop DESC;

-- Find top nine countries with the most cities
SELECT countries.name AS country, COUNT(cities.name) AS cities_num
FROM countries 
LEFT JOIN cities
ON countries.code = cities.country_code
GROUP BY countries.name
-- Order by count of cities as cities_num
ORDER BY cities_num DESC, country
-- Limit the results
LIMIT 9;

SELECT countries.name AS country,
-- Subquery that provides the count of cities   
  (SELECT COUNT(name)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;

Subqueries inside FROM:
- Subqueries can be used inside FROM to create a temporary table

SELECT DICTINCT monarch.continent, sub.most_recent
FROM monarchs,
    (SELECT continent, MAX(indep_year) AS most_recent
     FROM states
     GROUP BY continent) AS sub
WHERE monarch.continent = sub.continent
oRDER BY continent;

-- Select local_name and lang_num from appropriate tables
SELECT countries.local_name, sub.lang_num
FROM countries, 
  (SELECT code, COUNT(*) AS lang_num
  FROM languages
  GROUP BY code) AS sub
-- Where codes match
WHERE countries.code = sub.code
ORDER BY lang_num DESC;

-- Select relevant fields
SELECT economies.code, economies.inflation_rate, economies.unemployment_rate
FROM economies
WHERE year = 2015 
  AND code IN
-- Subquery returning country codes filtered on gov_form
	(SELECT code 
  FROM countries WHERE gov_form LIKE '%Republic%' OR gov_form LIKE '%Monarchy%')
ORDER BY inflation_rate;

-- Select fields from cities
SELECT name, country_code, city_proper_pop, metroarea_pop, (city_proper_pop / metroarea_pop * 100) AS city_perc
FROM cities
-- Use subquery to filter city name
WHERE name IN 
    (SELECT capital
    FROM countries
    WHERE continent = 'Europe' OR continent LIKE '%America')
-- Add filter condition such that metroarea_pop does not have null values
AND metroarea_pop IS NOT NULL
-- Sort and limit the result
ORDER BY city_perc DESC
LIMIT 10;