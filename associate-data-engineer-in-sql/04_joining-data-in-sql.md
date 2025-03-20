## [Joining Data in SQL](https://app.datacamp.com/learn/courses/joining-data-in-sql)

### INNER JOIN 
```
- Key field that uniquely identifies each record
- INNER JOIN looks for records in both tables which match on a given field
```

### INNER JOIN with ON
```sql
-- Inner join of presidents and prime_minister, joining on country                    
SELECT prime_minister.country, prime_minister.continent, prime_minister, president   
FROM presidents                
INNER JOIN prime_minister     
ON presidents.country = prime_minister.country;           
```

### REMEMBER!!!
```
The table.column_name format must be used when selecting columns that exist in both tables to avoid a SQL error.
```

### Aliasing tables
``` 
Aliasing tables is a common practice to make queries more readable and easier to write
```

### INNER JOIN with AS
```sql
--Inner join of presidents and prime_ministers, joining on country    
SELECT p2.country, p2.continent, prime_minister, president            
FROM presidents AS p1                      
INNER JOIN prime_ministers AS p2           
ON p1.country = p2.country;               
```

### INNER JOIN Using USING
```sql
--Inner join of presidents and prime_ministers, joining on country 
SELECT p2.country, p2.continent, prime_minister, president         
FROM presidents AS p1                   
INNER JOIN prime_ministers AS p2        
USING(country);                         
```

```sql
-- Select fields with aliases   
SELECT c.code AS country_code, name, year, inflation_rate  
FROM countries AS c             
-- Join to economies (alias e)  
INNER JOIN economies AS e       
-- Match on code field using table aliases                 
ON c.code = e.code              
```

### Defining relationships
```
1.  One-to-many relationships
    example: Jane Austen -> Persuasion, Pride and Prejudice, Emma
    1 author can write many books

2.  One-to-one relationships
    example: Finger -> fingerprint
    1 finger can have 1 fingerprint

3.  Many-to-many relationships
    examples: German -> Germany, Belgium, French -> Belgium 
    many languages can be spoken in many countries
```

### Multiple joins
```
Joins on joins
 ```

### Multiple INNER JOIN syntax
```sql
SELECT *      
FROM left_table                          
INNER JOIN right_table                   
ON left_table.id = right_table.id        
INNER JOIN another_table                 
ON left_table.id = another_table.id;     
```

### Joining on multiple key syntax
```sql               
SELECT *       
FROM left_table
INNER JOIN right_table                    
ON left_table.id = right_table.id         
AND left_table.name = right_table.name;   
```

### Multiple INNER JOIN with AS
```sql
-- Select fields                
SELECT name, e.year, fertility_rate, unemployment_rate     
FROM countries AS c             
INNER JOIN populations AS p     
ON c.code = p.country_code      
-- Join to economies (as e)     
INNER JOIN economies AS e       
-- Match on country code        
ON p.country_code = e.code;     
```

### Joining on different column names
```sql
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c        
INNER JOIN populations AS p
ON c.code = p.country_code 
INNER JOIN economies AS e  
ON c.code = e.code         
-- Add an additional joining condition such that you are also joining on year    
	AND p.year = e.year;   
```

### Outer Joins, Cross Joins and Self Joins
```
Left join:
- return all records in the left table, and those records in the right table that match on the joining fields provided.

Note. LEFT JOIN can also be written as LEFT OUTER JOIN.

Right join:
- return all records in the right table, and those records in the left table that match on the joining fields provided.

Note. RIGHT JOIN can also be written as RIGHT OUTER JOIN.
```

### LEFT JOIN using USING
```sql
SELECT p1.country, prime_minister     
FROM presidents AS p1                 
LEFT JOIN prime_ministers AS p2       
USING(country);                       
```

### RIGHT JOIN using ON syntax
```sql
SELECT *   
FROM left_table                       
RIGHT JOIN right_table                
ON left_table.id = right_table.id;    
```

### RIGHT JOIN using USING
```sql
SELECT p1.country, prime_minister     
FROM presidents AS p1                 
RIGHT JOIN prime_ministers AS p2       
USING(country);                       
```

### Modify this query to use RIGHT JOIN instead of LEFT JOIN
```sql
SELECT countries.name AS country, languages.name AS language, percent 
FROM languages  
RIGHT JOIN countries                       
USING(code)     
ORDER BY language;                         
```

### Full join
```
- A full join combines a left join and a right join
- It returns all records when there is a match in either left (table1) or right (table2) table records.

Note. FULL JOIN can also be written as FULL OUTER JOIN.
```

### FULL JOIN syntax
```sql
SELECT left_table.id AS L_id, right_table.id AS R_id  
FROM left_table            
FULL JOIN right_table      
ON L_id = R_id;            
```

### FULL JOIN using USING
```sql
SELECT p1.country, prime_minister, president   
FROM presidents AS p1                          
FULL JOIN prime_ministers AS p2                
USING(country);     
LIMIT 10;           
```

### FULL JOIN with WHERE
```sql
SELECT name AS country, code, region, basic_unit  
FROM countries         
-- Join to currencies  
FULL JOIN currencies   
USING (code)           
-- Where region is North America or name is null  
WHERE region = 'North America' OR name IS NULL    
ORDER BY region;       
```

### Multiple FULL JOIN
```sql
SELECT                  
	c1.name AS country, 
    region,             
    l.name AS language, 
	basic_unit,         
    frac_unit           
FROM countries as c1    
-- Full join with languages (alias as l)           
FULL JOIN languages AS l
USING(code)             
-- Full join with currencies (alias as c2)         
FULL JOIN currencies as c2                         
USING(code)             
WHERE region LIKE 'M%esia';                        
```

### Cross join
```
CROSS JOIN creates all possible combinations of two tables
```

### CROSS JOIN syntax
```sql
SELECT id1, id2    
FROM table1        
CROSS JOIN table2; 
```

### CROSS JOIN with WHERE
```sql
SELECT prime_minister, president       
FROM prime_ministers AS p1             
CROSS JOIN presidents AS p2;           
WHERE p1.continent IN ('Asia)          
AND p2.continent IN ('North America'); 
```

### Self join
```
Self joins are tables joined with themselves
They can be used to compare parts of the same table
```

### Self join
```sql
SELECT p1.country AS country1, p2.country AS country2  
FROM prime_ministers AS p1  
INNER JOIN prime_ministers AS p2                        
ON p1.continent = p2.continent                         
AND p1.country <> p2.country
LIMIT 10;                   
```

### REMEMBER!!!
```
INNER JOIN:
You sell house and have two tables, listing prices and price_sold.
You want a table with sale prices and listing prices, only if you know both.

LEFT JOIN:
You run a pizza delivery service with loyal clients. You want a table of clients and their weekly orders, with nulls if there are 
no orders.

FULL JOIN:
You want a report of wheter your parents have reached out to you, or you have reached out to them. 
You are fine with nulls either condition.
```

### Set theory for SQL Joins
```
Both tables must have the same number of columns

UNION
- UNION takes two tables as input, and returns all records from both tables

UNION ALL
- UNION ALL returns all records from both tables, but does not remove duplicates
```

### UNION syntax
```sql
SELECT *                   
FROM left_table            
UNION                      
SELECT *                   
FROM right_table;          
```

### UNION ALL syntax
```sql
SELECT *                   
FROM left_table            
UNION ALL                  
SELECT *                   
FROM right_table;          
```

### UNION example
```sql
SELECT monarch AS leader, country
FROM monarchs
UNION
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;
```

### UNION ALL example
```sql
SELECT monarch AS leader, country
FROM monarchs
UNION ALL
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;
```

```
Note. 
- Both queries on the left and right of the set operation must have the same data types. 
- The names of the fields do not need to be the same, as the result will always contain field names from the left query.
```

### INTERSECT
```
INTERSECT returns all records that are in both tables
```

### INTERSECT syntax
```sql
SELECT id, val
FROM left_table
INTERSECT
SELECT id, val
FROM right_table;
```

### INTERSECT example
```sql
-- Countries with prime ministers and president
SELECT country AS intersect_country
FROM prime_ministers
INTERSECT
SELECT country
FROM presidents;
```

### INTERSECT examle (2)
```sql
-- Return all cities with the same name as a country
SELECT name
FROM cities
INTERSECT 
SELECT name
FROM countries;
```

### EXCEPT
```
EXCEPT returns all records from the first table that are not in the second table
```

### EXCEPT syntax
```sql
SELECT id, val
FROM left_table
EXCEPT
SELECT id, val
FROM right_table;
```

### EXCEPT example
```sql
-- Return all cities that do not have the same name as a country
SELECT name
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;
```

### Subqueries
```
Subqueries are queries nested within other queries

Subquerying with semi joins and anti joins:
Semi join:
- Semi join returns all records from the left table where a condition is met in the right table
- Return all values from left_table where values of col1 are in right_table
```

### Semi join syntax
```sql
SELECT president, country, continent
FROM presidents
WHERE country IN
    (SELECT country
     FROM states
     WHERE indep_year < 1800;)
```

### Anti join
```
Anti join returns all records from the left table where a condition is not met in the right table
```

### Anti join example
```sql
-- Return all countries that are in America and indep_year is after 1800
SELECT country, president
FROM presidents
WHERE continent LIKE '%America'
AND country NOT IN
    (SELECT country
     FROM states
     WHERE indep_year < 1800;)
```

### Subqueries inside WHERE
```sql
SELECT DISTINCT name
FROM languages
-- Add syntax to use bracketed subquery below as a filter
WHERE code IN
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;
```

### Subqueries inside WHERE and SELECT
```
- All semi joins and anti joins can be written as subqueries inside WHERE
- WHERE is the most common place to use subqueries
```

### Subqueries inside WHERE
```sql
SELECT * 
FROM some_table
WHERE some_field IN 
    (SELECT some_numeric_field
     FROM another_table
     WHERE another_field = 'condition');
```

### Subqueries inside SELECT
```sql
SELECT DISTINCT continet,
    (SELECT COUNT(*)
    FROM monarchs
    WHERE states.continent = monarchs.continent) AS monarchs_count
FROM states;
```

### Subqueries inside WHERE (2)
```sql
SELECT *
FROM populations
WHERE year = 2015
-- Filter for only those populations where life expectancy is 1.15 times higher than average
  AND life_expectancy > 1.15 *
  (SELECT AVG(life_expectancy)
   FROM populations
   WHERE year = 2015);
```

### Subqueries inside WHERE (3)
```sql
-- Select relevant fields from cities table
SELECT name, country_code, urbanarea_pop
FROM cities
-- Filter using a subquery on the countries table
WHERE name IN
    (SELECT capital
     FROM countries
    )
ORDER BY urbanarea_pop DESC;
```

### Subqueries inside SELECT
```sql
SELECT countries.name AS country,
-- Subquery that provides the count of cities   
  (SELECT COUNT(name)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```

### Subqueries inside FROM
```
Subqueries can be used inside FROM to create a temporary table
```

### Subqueries inside FROM example
```sql
SELECT DICTINCT monarch.continent, sub.most_recent
FROM monarchs,
    (SELECT continent, MAX(indep_year) AS most_recent
     FROM states
     GROUP BY continent) AS sub
WHERE monarch.continent = sub.continent
oRDER BY continent;
```

### Subqueries inside FROM (2) example
```sql
-- Select local_name and lang_num from appropriate tables
SELECT countries.local_name, sub.lang_num
FROM countries, 
  (SELECT code, COUNT(*) AS lang_num
  FROM languages
  GROUP BY code) AS sub
-- Where codes match
WHERE countries.code = sub.code
ORDER BY lang_num DESC;
```

### Subqueries inside WHERE (4)
```sql
-- Select relevant fields
SELECT economies.code, economies.inflation_rate, economies.unemployment_rate
FROM economies
WHERE year = 2015 
  AND code IN
-- Subquery returning country codes filtered on gov_form
	(SELECT code 
  FROM countries WHERE gov_form LIKE '%Republic%' OR gov_form LIKE '%Monarchy%')
ORDER BY inflation_rate;
```

### Subqueries inside WHERE (5)
```sql
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
```