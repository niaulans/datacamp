## 📌 Table of Contents  
- [Understanding Data Engineering](#understanding-data-engineering)  
- [Introduction to SQL](#introduction-to-sql)
- [Intermediate SQL](#intermediate-sql)
- [Joining Data in SQL](#joining-data-in-sql)
- [Database Design](#database-design)


## Understanding Data Engineering  

**Data workflow:**
1. Data collection and storage (from web traffic, surveys, media consumption)
2. Data preparation (cleaning data, converting data into a more organized format)
3. Exploration and visualization
4. Experimentation and prediction (prediksi harga saham)

> Data engineer bertanggung jawab atas proses pertama.  

**Data engineer deliver:**
- the correct data  
- in the right format
- to the right people

**Data engineer's responsibilities:**
- ingest data from different sources
- optimize databases for analysis
- remove corrupted data
- develop, construct, test, and maintain architectures

```
The five Vs:                      
- Volume (how much?)              
- Variety (what kind?)            
- Velocity (how frequent?)       
- Veracity (how accurate?)        
- Value (how useful?)             
```

**Data Engineer VS Data Scientist:**
| Data Enginner | Data Scientist |
| --- | --- |
| ingest and store data | exploit data |
| setup databases | access databases |
| build data pipelines | use pipeline outputs |
| strong software skills | strong analytical skills |

**Data engineering:**
- ingest
- process
- store
- need pipelines
- automate flow from one station to the next
- provide up-to-date, accurate, relevant data

> Data pipeline ensure an efficient flow of the data

**automate:**
- extracting
- transforming
- combining
- validating
- loading

**reduce:**
- human intervention 
- errors
- time it takes data to flow

**ETL**
- Popular frameworks for designing data pipelines 
- Extract data, Transform extracted data, Load transformed data to another databases

**Data pipelines:**
- Move data from one system to another
- May follow ETL
- Data may not be transformed
- Data may be directly loaded in applications

**Example data pipeline generating a weekly playlist for each user based on their taste:**
1. Extract the songs Julian listened to the most over the past month
2. Find other user who listened to these same songs a lot as well
3. Load only the 10 top songs these users listened to the most over the past week into a table called "Similar profiles"
4. Extract only songs these other users listen to that are of the same genre as the ones in Julian's listening sessions. These are our recommendations
5. Load the recommendations song into a new table. That's Julian's weekly playlist.

**Data structure:**
- **Structured data:**
	- easy to search and organized
	- consisten model, rows, and columns
	- define types
	- can be grouped to form relations
	- stored in relational databases
	- about 20% of the data is structured
	- created and queried using SQL
 - **Semi-structured data**
	- relatively easy to search and organize
	- consistent model, less-rigid implementation: different observations have different sizes
	- different types
	- can be grouped, but needs more work
	- NoSQL databases: JSON, XML, YAML
- **Unstructured data**
	- doesn't follow a model, can't be contained in rows and columns
	- difficult to search and organize
	- usually text, sound, pictures or video
	- usually stored in data lakes, can appear in data warehouses  or databases
	- most of the data is unstructured
	- can be extremely valuable

**SQL databases:**
- Structured Query Language
- Industry standard for Relational Database Management Systems (RDBMS)
- Allows you to access many records at once, and group, filter, or aggregate them
- Close to written English, easy to write and understand
- DE use SQL to create and maintain databases

```sql
  CREATE TABLE employees(    
    employee_id INT,           
    first_name VARCHAR(255),   
    last_name VARCHAR(255),    
    role VARCHAR(255),         
    team VARCHAR(255),         
    full_time BOOLEAN,         
    office VARCHAR(255)
  );
```

```sql
SELECT first_name, last_name
FROM employees
WHERE role LIKE '%Data%';
```

**Database schema:**
- Databases are made of tables
- The database schema governs how tables are related

**Several implementations of SQL:**
- SQLite
- MySQL
- PostgreSQL
- Oracle SQL
- SQL Server

**Data warehouses and data lakes:**
- **Data lakes**
	- Stores all the raw data (unprocessed and messy)
	- Can be petabytes in size (1 million GBs)
	- Stores all data structures
	- Cost-effective
	- Difficult to analyze
	- Requires an up-to-date data catalog
	- Used by data scientists
	- Big data, real-time analytics
- **Data warehouses**
	- Specific data for specific use
	- Relatively small in size
	- Stores mainly structured data
	- More costly to update
	- Optimized for data analysis
	- Ad-hoc, read-only queries

**Data catalog for data lakes:**
- What is the source of this data?
- Where is this data used?
- Who is the owner of the data?
- How often is this data updated?
- Good practice in terms of data governance
- Ensures reproducibility
- No catalog --> data swamp

**Good practice for any data storage:**
- Reliability
- Autonomy
- Scalability
- Speed

**Database vs. data warehouse**
- **Database:**
	- General term
	- Loosely defined as organized data stored and accessed on a computer
- **Data warehouse:**
	- type of database

**Processing data** => converting raw data into meaningful information.
**Data processing value:**
- Remove unwanted data
- Optimize memory, process, and network costs
- Convert data from one type to another
- Organized data
- To fit into a schema/structure
- Increase productivity

**Example at Spotflix:**
- No long term need for testing feature data
- Can't afford to store and stream files this big
- Convert songs from .flac to .ogg
- Reorganized data from the data lake to data warehouses
- Employee table example
- Enable data scientists

**How data engineers process data:**
- Data manipulation, cleaning, and tidying tasks
  - that can be automated
  - that will always need to be done
- Store data in a sanely structured database
- Create views on top of the database tables
- Optimizing the performance of the database

**Example:**
- Rejecting corrupt song files
- Deciding what happens with missing metadata
- Separate artists and albums tables
- ..but provide view combining them
- indexing

**Scheduling data (Apache Airflow):**
- Can apply to any task listed in data processing
- Scheduling is the glue of your system
- Holds each piece and organize how they work together
- Run tasks in a specific order and resolves all dependencies

**Manual, time, and sensor scheduling:**
- Manually (manually update the employee tables)
- Automatically run at a specific time (update the employee table at 6 AM)
- Automatically run if a specific condition is met 
    - sensor scheduling (update the departement tables if a new employee was added)

**Batch and streams:**
- **Batch** 
	- Group records at intervals (song uploaded by artists every 24 hours, employee table update, revenue table)
	- Often cheaper
- **Streams**
	- Send individual records right away (new users signing in, online vs. offline)

**Parallel computing:**
- Basis of modern data processing tools
- Necessary :
    - mainly because of memory
    - also for processing power
- How it works:
    - Breaks down tasks into smaller tasks
    - Distributes these tasks to multiple computers
    - Combines the results

**Benefit and risks of parallel computing:**
- Employees = processing unit
- Adv: 
    - Extra processing power
    - Reduced memory footprint
- Disadv:
    - Moving data incurs a cost
    - Communication time

**Cloud Computing:**
- **Servers on premises:**
	- Bought
	- Need space
	- Electrical and maintenance costs
	- Enough power for peak moments
	- Processing power unused at quieter times
- **Servers on the cloud:**
	- Rented
	- Don't need space
	- Use just the resources when we need them
	- The closer to the user the better

**Cloud computing for data storage:**
- Database reliability : data replication
- Risk with sensitive data  
<br>

- AWS -> AWS S3 -> AWS EC2 -> AWS RDS  
- Microsoft Azure -> Azure Blob Storage -> Azure Virtual Machines -> Azure SQL Database  
- Google Cloud -> Google Cloud Storage -> Google Compute Engine -> Google Cloud SQL

**Spotflix use AWS:**
- S3 to store cover albums
- EC2 to process songs
- RDS to store employee data

**Multicloud:**  
- **Pros:**
	- Reducing reliance on a single vendor
	- Cost-efficiencies
	- Local laws requiring certain data to be physically present within the country
	- Mitigating againts disasters
- **Cons:**
	- Cloud providers try to lock in consumers
	- Incompatibility
	- Security and governance  
<br>

## Introduction to SQL

**Database advantages:**
- More storage than spreadsheet applications
- Storage is more secure

**SQL (Structured Query Language)**
- The most widely used programming language for databases

**Tables:**
- organized into rows and columns
- rows are records, and columns are fields

**Good table naming:**
- be lowercase
- have no spaces - use underscores instead
- refer to a collective group or be plural

> a record is a row that holds data on an individual observation  
> a field is a column that holds one piece of information about all records

**fields names should:**
- be lowercase
- have no spaces
- be singular
- be different from other field names
- be different from the table name

**Assigned seats:**
- unique identifiers are used to identify records in a table
- they are unique and often numbers

**SQL data types:**
- Different types of data are stored differently and take up different space
- Some operations only apply to certain data types

> A string is a sequence of characters such as letters or punctutation <br>
  VARCHAR is a flexible and popular string data type in SQL

> Integers store whole numbers <br>
  INT is a flexible and popular

> Float store numbers thet include a fractional part <br>
  NUMERIC is a flexible and popular Float data type in SQL

> Schemas are often referred to as "blueprints" of databases <br>
  A schema show a database's design such as table, relationships, and constraints

**Database storage:**
- Servers are centralized computers that perform services via requests made over a network

> Use SQL queries to uncover trends in website traffic, customer reviews, and product sales

**Keywords:**
- Keywords are reserved words for operations
- Common keywords: SELECT, FROM

```sql
# Select all names from the patrons table  

SELECT name       
FROM patrons;
```
> Query results often called result **set**

```sql
-- Select card_num and name from the patrons table  
-- Multiple select   
SELECT card_num, name   
FROM  patrons;
```

```sql
-- Select all    
SELECT *          
FROM patrons;
```

> **Aliasing** -> use aliasing to rename columns

```sql
-- Select name as first_name and year_hired from the employees table  
SELECT name AS first_name, year_hired     
FROM employees;
```

```sql
-- Select distinct year_hired from the employees table 
SELECT DISTINCT year_hired 
FROM employees;            
```
<br>

**VIEWS:**
- a view is a virtual table that is the result of a saved SQL SELECT statement
- When accesed, views automatically update in response to updates in the underlying data

```sql
-- Buat view dengan nama employee_hire_years yang isinya id, name, year_hired 
CREATE VIEW employee_hire_years AS                
SELECT id, name, year_hired                       
FROM employees;         
```

```sql
CREATE VIEW library_authors AS          
SELECT DISTINCT author AS unique_author 
FROM books; 
```
<br>

**SQL flavors:**
- Both free and paid
- All used with relational databases
- Vast majority of keywords are the same
- All must follow universal standards
- Only the additions on top of there standards make flavors different

    **PostgreSQL:**
    - Free and open-source relational database system
    - Created at university of california, berkeley
    - Research and fund by DARPA 
    - "PostgreSQL" referens to both the postgresql databse system and its associated SQL flavor

    **SQL server:**
    - Has free and paid versions
    - Created by microsoft
    - T-SQL is microsoft SQL flavor, used with SQL Server databases 

**PostgreSQL vs. SQL Server**  
    **PostgreSQL:**
```sql
SELECT id, name  
FROM employees   
LIMIT 2; 
```

    **SQL Server:**
```sql
SELECT TOP(2) id, name 
FROM employees;       
```

## Intermediate SQL

**COUNT()**
- Counts the number of records with a value in a field
- Use an alias for clarity

```sql
SELECT COUNT(birthdate) AS count_birthdates    
FROM people;         
```

```sql
SELECT COUNT(name) AS count_names, COUNT(birthdate) AS count_birthdates 
FROM people;   
```

**COUNT(field_name)** counts values in a field.  
**COUNT(*)** counts records in a table.

```sql
SELECT COUNT(*) AS total_records  
FROM people;
```

```sql
SELECT COUNT(DISTINCT birthdate) AS unique_birthdates  
FROM people;   
```

**Order of execution:**
- SQL is not processed in its written order

> -- Order of execution  
    SELECT name (2)  
    FROM people (1)  
    LIMIT 10; (3)

**Most common errors:**
- Misspelling
- Incorrect capitalization
- Incorrect or missing punctuation, especially commas

**SQL formatting**
- Formatting is not required but lack of formatting can cause issues

**Best practice:**
- Capitalize keywords
- Add new lines

**Dealing with non-standars field names:**
- release year instead of release_year
- Put non-standard field names in double-quotes -> "release year"

**Why do we format?**
- Easier collaboration
- Clean and readable
- Looks professional
- Easier to understand
- Easier to debug

**Filtering numbers:**  
    **WHERE clause:**  
    - Filters records based on a condition and comparison operators

```sql
SELECT title                
FROM films                  
WHERE release_year > 2000; 
```

```sql
SELECT title                
FROM films                  
WHERE country = 'Japan';  
```

> -- Order of execution  
SELECT item (3)  
FROM coats (1)   
WHERE color = 'green' (2)  
LIMIT 5; (4)  

```sql
-- Count the records with at least 100,000 votes   
SELECT COUNT(num_votes) AS films_over_100K_votes   
FROM reviews             
WHERE num_votes >= 100000; 
```  

**Multiple criteria:**
- OR, AND, BETWEEN (inclusive, nilai awal dan akhir masuk)

```sql
SELECT *             
FROM coats           
WHERE color = 'yellow' OR length = 'short';
```

```sql
SELECT * 
FROM coats                         
WHERE buttons BETWEEN 1 AND 5;  
```

```sql
SELECT title                  
FROM films                    
WHERE (release_year = 1994 OR release_year = 1995)      
     AND (certification = 'PG' OR certification = 'R'); 
```

```sql
SELECT title, release_year     
FROM films                     
WHERE (release_year = 1990 OR release_year = 1999)      
	AND (language = 'English' OR language = 'Spanish')   
-- Filter films with more than $2,000,000 gross          
	AND gross > 2000000;       
```

```sql
SELECT title, release_year              
FROM films    
WHERE release_year BETWEEN 1990 AND 2000
	AND budget > 100000000              
    -- Amend the query to include Spanish or French-language films    
	AND (language = 'Spanish' OR language = 'French');            
```  

**Filtering text:**
- Filter a pattern rather than specific text
- LIKE, NOT LIKE, IN

**LIKE:**
- Used to search for a pattern in a field
- % match zero, one, or many characters
- _ match a single character

```sql
-- Find all name that start with 'Ade'    
SELECT name     
FROM people     
WHERE name LIKE 'Ade%'; -> Adelia; 
```

```sql
-- Find all name that start with 'Ev'     
SELECT name     
FROM people     
WHERE name LIKE 'Ev_'; -> Eve;
```

```sql
SELECT name                      
FROM people                      
WHERE name LIKE '%r'; -> Aaron;
```

```sql
SELECT name
FROM people
WHERE name LIKE '__t%'; -> Anthony   
```

**NOT LIKE:**
```sql
SELECT name  
FROM people  
WHERE name NOT LIKE 'A.%'; -> Aaliyah  
```

**IN:**
```sql
SELECT title    
FROM films      
WHERE release_year IN (1920, 1930, 1940); 
```

```sql
-- Find the title, certification, and language all films certified NC-17 or R that are in English, Italian, or Greek 
SELECT title, certification, language            
FROM films
WHERE certification IN ('NC-17', 'R') AND language IN ('English', 'Italian', 'Greek');
```

```sql
-- Count the unique titles               
SELECT COUNT(DISTINCT title) AS nineties_english_films_for_teens   
FROM films     
-- Filter to release_years to between 1990 and 1999                
WHERE release_year BETWEEN 1990 AND 1999 
-- Filter to English-language films      
	AND language = 'English'             
-- Narrow it down to G, PG, and PG-13 certifications               
	AND certification IN ('G', 'PG', 'PG-13');                     
```
<br>

**NULL values**  
**Missing values**  
- COUNT (field_name) includes only non-missing values  
- COUNT(*) includes missing values

```sql
SELECT COUNT(*) AS count_records 
FROM people; -> 8397             
```

```sql
-- Count the records with null birthdates   
SELECT name       
FROM people       
WHERE birthdate IS NULL;                    
```

```sql
SELECT COUNT(*) AS no_birthdates    
FROM people                         
WHERE birthdate IS NULL; - 2245     
```

```sql
SELECT COUNT(name) AS count_birthdates  
FROM people   
WHERE birthdate IS NOT NULL;            
```

**Summarizing Data:**
- aggregate functions return a single value
- AVG(), SUM(), MIN(), MAX(), COUNT()

```sql
SELECT AVG(budget)   
FROM films;          
```

```sql
SELECT SUM(budget)  
FROM films;         
```

**Summarizing subsets**  
```sql
SELECT AVG(budget) AS avg_budget  
FROM films                        
WHERE release_year >= 2010;       
```

**ROUND()** -> round the result to specific decimal places
**ROUND(number_to_round, decimal_places)**

**Aggregate functions vs. arithmetic:**  
    - Aggregate = vertikal  
    - Arithmatic = horizontal

> **Order of execution:**  
    - FROM  
    - WHERE  
    - SELECT (aliases are defined here)  
    - LIMIT

- Aliases defined in the **SELECT** clause cannot be used in the WHERE clause due to order of execution

```sql
-- Calculate the percentage of people who are no longer alive
SELECT COUNT(deathdate) * 100.0 / COUNT(*) AS percentage_dead 
FROM people;                        
```

```sql
-- Find the number of decades in the films table
SELECT (MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades   |
FROM films;              
```

```sql
-- Sorting results
SELECT title, budget                       
FROM films       
ORDER BY bugdet DESC; (besar ke kecil)     
```

> **Order of execution**:  
    - FROM  
    - WHERE  
    - SELECT  
    - ORDER BY  
    - LIMIT

```sql 
-- Grouping data
SELECT certification, COUNT(title) AS title_count 
FROM films              
GROUP BY certification  
ORDER BY title_count DESC;                        
LIMIT 3;                
```

> **Order of execution:**  `
    - FROM  
    - GROUP BY  
    - SELECT  
    - ORDER BY  
    - LIMIT

**Filtering grouped data:**
```sql
-- HAVING clause:
SELECT release_year, COUNT(title) AS title_count  
FROM films              
GROUP BY release_year   
HAVING COUNT(title) > 10;                         
```

```sql
SELECT certification, COUNT(title) AS title_count  
FROM films              
WHERE certification IN ('G', 'PG', 'PG-13')        
GROUP BY certification   
HAVING COUNT(title) > 10 
ORDER BY title_count DESC;                         
LIMIT 3;                 
```

> **Final Order of execution:**  
    - FROM                       
    - WHERE                      
    - GROUP BY                   
    - HAVING                     
    - SELECT                     
    - ORDER BY                   
    - LIMIT

**HAVING vs. WHERE:**
- WHERE filters individual records, HAVING filters groups

```sql
-- Select the country and average_budget from films
SELECT country, AVG(budget) AS average_budget      
FROM films               
-- Group by country      
GROUP BY country         
-- Filter to countries with an average_budget of more than one billion       
HAVING AVG(budget) > 1000000000                    
-- Order by descending order of the aggregated budget                        
ORDER BY average_budget DESC; 
```

## Joining Data in SQL

**INNER JOIN** 
- key field that uniquely identifies each record
- INNER JOIN looks for records in both tables which match on a given field

```sql
--Inner join of presidents and prime_minister, joining on country                    |
SELECT prime_minister.country, prime_minister.continent, prime_minister, president   
FROM presidents         
INNER JOIN prime_minister       
ON presidents.country = prime_minister.country;    
```

> The table.column_name format must be used when selecting columns that exist in both tables to avoid a SQL error.

**Aliasing tables:**
```sql
--Inner join of presidents and prime_ministers, joining on country    
SELECT p2.country, p2.continent, prime_minister, president            
FROM presidents AS p1                       
INNER JOIN prime_ministers AS p2            
ON p1.country = p2.country;                 
```

**USING**
```sql
-- Using USING:
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

**Defining relationships:**  
- **One-to-many relationships:**  
example: Jane Austen -> Persuasion, Pride and Prejudice, Emma
1 author can write many books

- **One-to-one relationships:**  
example: Finger -> fingerprint
1 finger can have 1 fingerprint

- **Many-to-many relationships:**  
examples: German -> Germany, Belgium, French -> Belgium 
many languages can be spoken in many countries


```sql
-- Select country and language names (aliased)          
SELECT c.name AS country, l.name AS language             
-- From countries (aliased)    
FROM countries AS c            
-- Join to languages (aliased) 
INNER JOIN languages AS l      
-- Use code as the joining field with the USING keyword  
USING(code);                   
```

**Multiple joins:**

**Joins on joins**
```sql
SELECT *       
FROM left_table
INNER JOIN right_table                   
ON left_table.id = right_table.id        
INNER JOIN another_table                 
ON left_table.id = another_table.id;     
```

**Joining on multiple keys**
```sql         
SELECT *        
FROM left_table 
INNER JOIN right_table                    
ON left_table.id = right_table.id         
AND left_table.name = right_table.name;   
```

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

```sql
SELECT name, e.year, fertility_rate,unemployment_rate 
FROM countries AS c                
INNER JOIN populations AS p           
ON c.code = p.country_code                      
INNER JOIN economies AS e      
ON c.code = e.code    
    -- Add an additional joining condition such that you are also joining on year 
	AND p.year = e.year; 
```

**Outer Joins, Cross Joins and Self Joins**  
- **Left and Right joins**  

**Left join:**  
    - return all records in the left table, and those records in the right table that match on the joining fields provided

```sql
SELECT p1.country, prime_minister     
FROM presidents AS p1                 
LEFT JOIN prime_ministers AS p2       
USING(country);                       
```

> LEFT JOIN can also be written as LEFT OUTER JOIN.

**Right join:**  
    - return all records in the right table, and those records in the left table that match on the joining fields provided

```sql
SELECT *    
FROM left_table                       
RIGHT JOIN right_table                
ON left_table.id = right_table.id;    
```

> RIGHT JOIN can also be written as RIGHT OUTER JOIN.

```sql
SELECT p1.country, prime_minister     
FROM presidents AS p1                 
RIGHT JOIN prime_ministers AS p2       
USING(country);                       
```

```sql
-- Modify this query to use RIGHT JOIN instead of LEFT JOIN           
SELECT countries.name AS country, languages.name AS language, percent 
FROM languages    
RIGHT JOIN countries                        
USING(code)       
ORDER BY language;
```

**Full join:**
- A full join combines a left join and a right join
- It returns all records when there is a match in either left (table1) or right (table2) table records

```sql
SELECT left_table.id AS L_id, right_table.id AS R_id  
FROM left_table             
FULL JOIN right_table       
ON L_id = R_id;             
```

> FULL JOIN can also be written as FULL OUTER JOIN.

```sql
SELECT p1.country, prime_minister, president   
FROM presidents AS p1
FULL JOIN prime_ministers AS p2                
USING(country);      
LIMIT 10;            
```

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

**Cross join:**
- CROSS JOIN creates all possible combinations of two tables

```sql
SELECT id1, id2    
FROM table1        
CROSS JOIN table2; 
```

```sql 
SELECT prime_minister, president       
FROM prime_ministers AS p1             
CROSS JOIN presidents AS p2;           
WHERE p1.continent IN ('Asia') AND p2.continent IN ('North America'); 
```

**Self join:**
- Self joins are tables joined with themselves
- They can be used to compare parts of the same table

```sql
SELECT p1.country AS country1, p2.country AS country2  
FROM prime_ministers AS p1   
INNER JOIN prime_ministers AS p2                        
ON p1.continent = p2.continent                         
AND p1.country <> p2.country 
LIMIT 10;                    
```

```sql
-- Select aliased fields from populations as p1                       
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015      
FROM populations AS p1                      
-- Join populations as p1 to itself, alias as p2, on country code     
INNER JOIN populations AS p2                
ON p1.country_code = p2.country_code;       
```

```sql
SELECT      
    p1.country_code,                  
    p1.size AS size2010,              
    p2.size AS size2015,         
	p1.country_code,                  
    p1.size AS size2010,              
    p2.size AS size2015               
FROM populations AS p1                
INNER JOIN populations AS p2          
ON p1.country_code = p2.country_code  
WHERE p1.year = 2010                  
-- Filter such that p1.year is always five years before p2.year 
    AND p1.year = p2.year -5;         
```

**INNER JOIN:**  
You sell house and have two tables, listing prices and price_sold.
You want a table with sale prices and listing prices, only if you know both.

**LEFT JOIN:**  
You run a pizza delivery service with loyal clients. You want a table of clients and their weekly orders, with nulls if there are 
no orders.

**FULL JOIN:**  
You want a report of wheter your parents have reached out to you, or you have reached out to them.   
You are fine with nulls either condition.

**Set theory for SQL Joins** => both tables must have the same number of columns
- **UNION**
    - UNION takes two tables as input, and returns all records from both tables

```sql
SELECT *                   
FROM left_table            
UNION                      
SELECT *                   
FROM right_table;          
```

- **UNION ALL**
    - UNION ALL returns all records from both tables, but does not remove duplicates

```sql
SELECT *                   
FROM left_table            
UNION ALL                  
SELECT *                   
FROM right_table;          
```

```sql
SELECT monarch AS leader, country
FROM monarchs
UNION
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;
```

```sql
SELECT monarch AS leader, country
FROM monarchs
UNION ALL
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;
```

> Both queries on the left and right of the set operation must have the same data types. The names of the fields do not need to be the same, as the result will always contain field names from the left query.

```sql
-- Query that determines all pairs of code and year from economies and populations, without duplicates
SELECT code, year
FROM economies
UNION 
SELECT country_code, year
FROM populations
ORDER BY code, year;
```

**INTERSECT:**
- INTERSECT returns all records that are in both tables

```sql
SELECT id, val
FROM left_table
INTERSECT
SELECT id, val
FROM right_table;
```

```sql
-- Countries with prime ministers and president
SELECT country AS intersect_country
FROM prime_ministers
INTERSECT
SELECT country
FROM presidents;
```

```sql
-- Return all cities with the same name as a country
SELECT name
FROM cities
INTERSECT 
SELECT name
FROM countries;
```

**EXCEPT:**
- EXCEPT returns all records from the first table that are not in the second table

```sql
SELECT id, val
FROM left_table
EXCEPT
SELECT id, val
FROM right_table;
```

```sql
-- Return all cities that do not have the same name as a country
SELECT name
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;
```

**Subqueries:**
- Subqueries are queries nested within other queries

**Subquerying with semi joins and anti joins:**  
**Semi join:**
- Semi join returns all records from the left table where a condition is met in the right table
- Return all values from left_table where values of col1 are in right_table

```sql
SELECT president, country, continent
FROM presidents
WHERE country IN
    (SELECT country
     FROM states
     WHERE indep_year < 1800;)
```

**Anti join:**
- Anti join returns all records from the left table where a condition is not met in the right table

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

**Subqueries inside WHERE and SELECT:**
- All semi joins and anti joins can be written as subqueries inside WHERE
- WHERE is the most common place to use subqueries

**Subqueries inside WHERE:**  

```sql
SELECT * 
FROM some_table
WHERE some_field IN 
    (SELECT some_numeric_field
     FROM another_table
     WHERE another_field = 'condition');
```

**Subqueries inside SELECT:**

```sql
SELECT DISTINCT continet,
    (SELECT COUNT(*)
    FROM monarchs
    WHERE states.continent = monarchs.continent) AS monarchs_count
FROM states;
```

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

```sql
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
```

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

**Subqueries inside FROM:**
- Subqueries can be used inside FROM to create a temporary table

```sql
SELECT DICTINCT monarch.continent, sub.most_recent
FROM monarchs,
    (SELECT continent, MAX(indep_year) AS most_recent
     FROM states
     GROUP BY continent) AS sub
WHERE monarch.continent = sub.continent
oRDER BY continent;
```

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

<br>

## Introduction to Relational Databases in SQL

**A relational database:**
- real-life entities become tables -> professors, universities, companies
- reduced redundancy -> only one entry in companies for the bank "Credit Suisse"
- data integrity by relationships -> a professor can work at multiple universities and companis a company can employ multiple professors

```sql
--Have a look at the PostgreSQL database
SELECT table_schema, table_name
FROM information_schema.tables;
```

```sql
--Have a look at the columns of a certain table
SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name = 'pg_config';
```

```sql
-- Query the right table in information_schema to get columns
SELECT column_name, data_type 
FROM information_schema.columns
WHERE table_name = 'university_professors' AND table_schema = 'public';
```

**ERD(Entitiy Relationship Diagram):**
- Square boxes represent tables
- Circle represent relationships

```sql
--Create a new table
CREATE TABLE table_name (
    column_a data_type,
    column_b data_type,
    column_c data_type
);
```

```sql
CREATE TABLE weather (
    clouds text,
    temperature numeric,
    weather_station char(5)
);
```

```sql
-- Add columns to a table
ALTER TABLE table_name
ADD COLUMN column_name data_type;
```

**Update database structure:**
- Only store DICTINCT data in the new tables

```sql
INSERT DICTINCT records INTO the new tables;
```

```sql
INSERT INTO organizations
SELECT DISTINCT organization, organization_sector
FROM university_professors;
```

```sql
The INSERT INTO statement:
INSERT INTO table_name(column_a, column_b)
VALUES ("Value_a", "Value_b");
```

```sql
CREATE TABLE affiliations (
    firstname text,
    lastname text,
    university_shortname text,
    function text,
    organization text
);
```

```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

```sql
--Delete a table
DROP TABLE table_name;
```

**Better data quality with constraints:**  
**Integrity constraints:**
- Attribute constraints, e.g. data types on columns
- Key constraints, e.g. primary keys
- Referential integrity constraints, e.g. foreign keys

**Why constraints?**
- Constraints give the data structure
- Constraints help with consistency thus data quality
- Data quality is a business adavantage/ data science prerequisite
- Enforcing is difficult, but PostgreSQL helps

**Dealing with data types (casting)**
```sql
CREATE TABLE weather(
    temperature integer,
    wind_speed text
);
```

```sql
SELECT temperature * wind_speed AS wind_chill
FROM weather; -- error because wind_speed is text
```

```sql
--Cast the wind_speed column to integer
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill
FROM weather;
```

**Working with data types:**
- Enforced on columns (i.e. attributes)
- Define the so-called "domain" of a column
- Define what operations are allowed on the data
- Enforce consistent storage of values

**The most common types:**
- **text**: character strings of any length
- **varchar (x)**: a maximum of x characters
- **char (x)**: a fixed-length string with a maximum of x characters
- **boolean**: can only take three states, true, false, or null
- **date, time, timestamp**: date, time, both date and time
- **numeric**: arbitrary precision numbers
- **integer**: whole numbers

**Specifying types upon table creation:**

```sql
CREATE TABLE student (
    ssn integer,
    name varchar(64),
    dob date,
    average_grade numeric(3,2) -- e.g. 5.54
    tuition_paid boolean
);
```

```sql
ALTER TABLE students
ALTER COLUMN name TYPE varchar(128);
```

```sql
ALTER TABLE students
ALTER COLUMN average_grade TYPE integer 
USING ROUND(average_grade); -> round the average_grade
```

> If you don't want to reserve too much space for a certain varchar column, you can truncate the values before converting its type.
For this, you can use the following syntax:

```sql
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
USING SUBSTRING(column_name FROM 1 FOR x)
```

```sql
-- Convert the values in firstname to a max. of 16 characters
ALTER TABLE professors 
ALTER COLUMN firstname 
TYPE varchar(16)
USING SUBSTRING(firstname FROM 1 FOR 16)
```

**The not-null and unique constraints:**  
- **The not-null constraint:**
    - Disallow NULL values in a certain column
    - Must hold true for the current state
    - Must hold true for any future state

    **What does NULL mean?**
    - unknown
    - does not exist
    - does not apply
    ... 

```sql
CREATE TABLE students (
    ssn integer NOT NULL,
    lastname VARCHAR(64) NOT NULL,
    home_phone integer,
    office_phone integer
);
```

```sql
ALTER TABLE students
ALTER COLUMN home_phone
SET NOT NULL;
```

```sql
ALTER TABLE students
ALTER COLUMN ssn
DROP NOT NULL;
```

- **The unique constraint:**
    - Disallow duplicate values in a certain column
    - Must hold true for the current state
    - Must hold true for any future state

```sql
CREATE TABLE table_name (
    column_name UNIQUE
);
```

```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name);
```

**Keys and superkeys:**
What is a key?
- Attribute(s) that identify a record uniquely
- As long as attributes can be removed: superkey
- If no more attributes can be removed: minimal superkey or key

SK1 = license_no, serial_no, make, model, year  
SK2 = license_no, serial_no, make, model  
SK3 = license_no, serial_no, year  

K1 = licensed_no  
K2 = serial_no  
K3 = model  
K4 = make, year  

K1 to K3 only consist of one attribute  
Removing either "make" or "year" from K4 would result in duplicate  
Only one candidate key can be the chosen key  

```sql
-- Try out different combinations
SELECT COUNT(DISTINCT(firstname, lastname)) 
FROM professors;
```

**Primary Keys:**
- One primary key per database table, chosen from candidate keys
- Uniquely identifies records, e.g. for referencing in other tables
- Unique and not-null constraints both apply
- Primary keys are time-invariant: choose columns wisely!

```sql
CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
);
```

```sql
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);
```

```sql
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```

```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name);
```

**Surrogate keys:**
- Surrogate keys are artificial primary keys
- Primary keys should be built from as few columns as possible
- Primary keys should never change over time

```sql
-- Adding a surrogate key with serial data type:
ALTER TABLE cars 
ADD COLUMN id serial PRIMARY KEY;
```

```sql
INSERT INTO cars
VALUES ('Volkswagen', 'Blitz', 'black');
```

**Another type of surrogate key:**
```sql
ALTER TABLE table_name
ADD COLUMN column_c VARCHAR(256);
```

```sql
UPDATE table_name
SET column_c = CONCAT(column_a, column_b);
```

```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_c);
```

```sql
-- Count the number of distinct rows with columns make, model
SELECT COUNT(DISTINCT(make, model)) 
FROM cars;
```

```sql
-- Add the id column
ALTER TABLE cars
ADD COLUMN id varchar(128);
```

```sql
-- Update id with make + model
UPDATE cars
SET id = CONCAT(make, model);
```

```sql
-- Make id a primary key
ALTER TABLE cars
ADD CONSTRAINT id_pk PRIMARY KEY(id);
```

```sql
-- Have a look at the table
SELECT * FROM cars;
```

```sql
-- Create the table
CREATE TABLE students (
  last_name VARCHAR(128) NOT NULL,
  ssn INTEGER PRIMARY KEY,
  phone_no CHAR(12)
);
```

**Foreign keys:**
- A foreign key (FK) points to the primary key (PK) of another table
- DOmain of FK must be equal to domain of PK
- Each value of FK must exist in PK of the other table (FK constraint or referential integrity constraint)
- FK are not actual keys, but constraints

```sql
CREATE TABLE manufacturers (
    name VARCHAR(255) PRIMARY KEY
);
```

```sql
INSERT INTO manufacturers
VALUES ('Volkswagen');
```

```sql
CREATE TABLE cars (
    model VARCHAR(128) PRIMARY KEY,
    manufacturer_name VARCHAR(255) REFERENCES manufacturers(name)
);
```

```sql
ALTER TABLE a
ADD CONSTRAINT a_key FOREIGN KEY (b_id) REFERENCES b(id);
```

```sql
-- Rename the university_shortname column
ALTER TABLE professors
RENAME COLUMN university_shortname TO university_id;
```

```sql
-- Add a foreign key on professors referencing universities
ALTER TABLE professors
ADD CONSTRAINT professors_fkey FOREIGN KEY (university_id) REFERENCES universities (id);
```

```sql
-- Select all professors working for universities in the city of Zurich
SELECT professors.lastname, universities.id, universities.university_city
FROM professors
INNER JOIN universities
ON professors.university_id = universities.id
WHERE universities.university_city = 'Zurich';
```

```sql
-- Add a professor_id column
ALTER TABLE affiliations
ADD COLUMN professor_id integer REFERENCES professors (id);
```

```sql
-- Rename the organization column to organization_id    
ALTER TABLE affiliations      
RENAME organization TO organization_id;                 
```

```sql
-- Add a foreign key on organization_id      
ALTER TABLE affiliations                    
ADD CONSTRAINT affiliations_organization_fkey FOREIGN KEY (organization_id) REFERENCES organizations (id);;       
```

```sql
-- Set professor_id to professors.id where firstname, lastname correspond to rows in professors  
UPDATE affiliations            
SET professor_id = professors.id  
FROM professors  
WHERE affiliations.firstname = professors.firstname AND affiliations.lastname = professors.lastname;
```

**Referential integrity:**
- A records referencing another table must refer to an existing record in that table
- Specified between two tables
- Enforced through foreign keys

**Referential integrity violations:**
Referential integrity from table A to table B is violated if:
- if a record in table B that is referenced from a record in table A is deleted.
- if a record in table A referencing a non-existing record from table B is inserted.
- Foreign keys prevent such violations


**Dealing with violations:** 

```sql
CREATE TABLE a (                   
    id integer PRIMARY,KEY,             
    column_a VARCHAR,(64),            
    ...,             
    b_id integer REFERENCES b(id) ON DELETE NO ACTION -- records in A that reference a record in B cannot be deleted 
); 
```

**Dealing with violations:**                 
```sql  
CREATE TABLE a (  
    id integer PRIMARY ,KEY,             
    column_a VARCHAR(64),            
    ...,             
    b_id integer REFERENCES b(id) ON DELETE CASCADE -- delete all records in A that reference the record in B       
); 
```

**ON DELETE**        
- **NO ACTION**: Throw an error                
- **CASCADE**: Delete all records in A that reference the record in B    
- **RESTRICT**: Throw an error                 
- **SET NULL**: Set the foreign key to NULL    
- **SET DEFAULT**: Set the foreign key to its default value              

```sql
-- Identify the correct constraint name                
SELECT constraint_name, table_name, constraint_type    
FROM information_schema.table_constraints              
WHERE constraint_type = 'FOREIGN KEY';                 
```

```sql
-- Identify the correct constraint name                  
SELECT constraint_name, table_name, constraint_type      
FROM information_schema.table_constraints                
WHERE constraint_type = 'FOREIGN KEY';                   
```

```sql
-- Drop the right foreign key constraint               
ALTER TABLE affiliations     
DROP CONSTRAINT affiliations_organization_id_fkey;     
```

```sql
-- Add a new foreign key constraint from affiliations to organizations which cascades deletion
ALTER TABLE affiliations 
ADD CONSTRAINT affiliations_organization_id_fkey FOREIGN KEY (organization_id) REFERENCES organizations (id) ON DELETE CASCADE;
```

```sql
-- Delete an organization   
DELETE FROM organizations   
WHERE id = 'CUREM';         
```

```sql
-- Check that no more affiliations with this organization exist    
SELECT * FROM affiliations               
WHERE organization_id = 'CUREM';         
```

```sql
-- Count the total number of affiliations per university    
SELECT COUNT(*), professors.university_id                   
FROM affiliations                 
JOIN professors                   
ON affiliations.professor_id = professors.id                
-- Group by the university ids of professors                
GROUP BY professors.university_id 
ORDER BY COUNT DESC;              
```

```sql
-- Filter the table and sort it
SELECT COUNT(*), organizations.organization_sector,
professors.id, universities.university_city 
FROM affiliations 
JOIN professors   
ON affiliations.professor_id = professors.id
JOIN organizations
ON affiliations.organization_id = organizations.id 
JOIN universities 
ON professors.university_id = universities.id      
WHERE organizations.organization_sector = 'Media & communication' 
GROUP BY organizations.organization_sector, 
professors.id, universities.university_city 
ORDER BY COUNT DESC;                        
```
<br>

## Database Design

**How should we organize and manage data?**
- Schemas: How should my data be logically organized?
- Normalization: Should my data have minimal dependency and redundancy?
- Views: What joins will be done most often?
- Access control: Should all users of the data have the same level access
- DBMS: How do i pick between al the SQL and noSQL options?

> Its depends on the intended use of the data.

**Approaches to processing data**

- **OLTP(Online Transaction Processing)**
    - oriented around transactions
    - Task example:
        - Find the price of a book
        - Update latest customer transaction
        - Keep track of employee hours
    - Supporting day-to-day operations/daily transactions
    - data: up-to-date
    - size: gigabytes, snapshots
    - queries: simple transactions & frequent updates
    - users: thousands

- **OLAP (Online Analytical Processing)**
    - oriented around analytics
    - Task example:
        - Calculate books with the best profit margin
        - Find most loyal customers
        - Decide employee of the month
    - Vaguer and focus on business decision making
    - Report and analyze data
    - data: consolidated, historical
    - size: archive, terabytes
    - queries: complex, aggregations, joins
    - users: hundreds

**Storing data beyond traditional databases**
- **Traditional databases**
    - For storing real-time relational structured data? OLTP
- **Data warehouses**
    - For analyzing archived structured data? OLAP
    - Optimized for analytics - OLAP
    - Organized for reading/aggregating data
    - usualy read-only
    - Contains data from multiple sources
    - Massively Parallel Processing (MPP)
    - Typically uses a denormalized schema and dimensional modeling
    - Amazon Redshift, Azure SQL Data Warehouse, Google Big Query
- **Data marts**
    - Subset of data warehouses
    - Dedicated to a spesific topic
- **Data lakes**
    - For storing data of all structures = flexibility and Scalability
    - For analyzing big data
    - Store all types of data at a lower cost
    - raw, operational databases, IoT device logs, real-time, relational and non-relational
    - Retains all data and can take up petabytes
    - Schema-on-read as opposed to schema-on-write
    - Need to catalog data otherwise becomes a data swamp
    - Run big data analytics using services such as Apache Spark and Hadoop
    (useful for deep learning and data discovery because activities require so much data)

**ETL**
- Extract, Transform, Load
- more traditional approach for warehousing and smaller-scale analytics  

> Data sources (OLTP, APIs, Files, IoT Logs) - Staging - Data warehouse -  Data marts, BI tools, Machine learning  
Extract, Transform, Load

**ELT**
- Extract, Load, Transform
- more common with big data project

> Data sources (OLTP, APIs, Files, IoT Logs) - Data lake - Data warehouse - Data marts, BI tools, Machine learning  
Extract, Load, Transform

**Database design** 
- Determines how data is logically stored
   - How is data going to be read and updates?
- Uses database models: high-level specifications for database structure
  - Most popular: relational model
  - Other: NoSQL, objct-oriented, graph, network model
- Uses schemas: blueprint of the database
  - defines tables, fields, relationships, indexes, and views
  - When inserting data in relational databases, schemas must be respected

**Data modeling**
- Process of creating a data model for the data to be stored

1. Conceptual data model: describes entities, relationships, and attributes
   Tools: data structure diagrams (entity relationship diagrams), UML
2. Logical data model: describes tables, columns, and relationships
   Tools: database models and schemas (relational mode, star schema)
3. Physical data model: describes physical storage of data
   Tools: partitions, CPUs, indexes, backup system and tablespaces

**Dimensional modeling** 
-  adaptation of the relational model for data warehouse design 
- optimized for OLAP queries: aggregate data, not updating (OLTP)
- built using the star schema
- easy to understand and query

**Elements of dimensional modeling**
1. **Fact tables**
    - decided by business use-case
    - holds records of a metric
    - changes regularly
    - connects to dimensions via foregin keys
2. **Dimension tables**
    - holds descriptive information
    - rarely changes
    - connects to fact tables via foregin keys

**Star schema**  
Dimensional modeling: star schema  
    - **Fact tables:**
        - Holds records of a metric
        - Changes regularly
        - Connect to dimensions via foreign keys
        - Example: - supply books to stores in USA and Canada
                - keep track of book sales  
    - **Dimensions tables:**
        - Holds decriptions of attributes
        - Does not change as often

> Snowflake schema (an extension)

> Star schema: one dimension  
  Snowflake schema: more than one dimension

> Because dimension tables are normalized

**Normalization:**
- Database design technique
- Divides tables into smaller tables and connects them via relationships
- Goal: reduce redundancy and increase data integrity

> Identify repeating groups of data anc create new tables for them

**Book dimension of the star schema**  
dim_book_star -: book_id, title, author, publisher, genre

**Most likely to have repeating values:**
- author
- publisher
- genre

**divides tables:**  
dim_book_sf: book_id, title, author_id, publisher_id, genre_id  
dim_genre_sf: genre_id, genre  
dim_publisher_sf: publisher_id, publisher  
dim_author_sf: author_id, author

```sql
-- Create dim_author with an author column
CREATE TABLE dim_author (
    author VARCHAR(256)  NOT NULL
);
```

```sql
-- Insert distinct authors 
INSERT INTO dim_author
SELECT DISTINCT author FROM dim_book_star;
```

```sql
-- Add a primary key 
ALTER TABLE dim_author ADD COLUMN author_id SERIAL PRIMARY KEY;
```

```sql
-- Output the new table
SELECT * FROM dim_author;
```

**Normalized and denormalized databases**

**Denormalized query:**  
get quantity of all Octavia E.Butler books sold in Vancouver in Q4 of 2018

```sql
SELECT SUM(quantity) FROM fact_booksales
INNER JOIN dim_store_star on fact_booksales.store_id = dim_store_star.store_id
INNER JOIN dim_book_star on fact_booksales.book_id = dim_book_star.book_id
INNER JOIN dim_time_star on fact_booksales.time_id = dim_time_star.time_id
WHERE dim_store_star.city = 'Vancouver' AND dim_book_star.author = 'Octavia E. Butler' AND dim_time_star.quarter = 4 AND dim_time_star.year = 2018;
```

**Normalized query:**  
get quantity of all Octavia E.Butler books sold in Vancouver in Q4 of 2018

```sql
SELECT SUM(fact_booksales.quantity)
FROM fact_booksales
INNER JOIN dim_store_sf ON fact_booksales.store_id = dim_store_sf.store_id
INNER JOIN dim_book_sf ON fact_booksales.book_id = dim_book_sf.book_id
INNER JOIN dim_time_sf ON fact_booksales.time_id = dim_time_sf.time_id
INNER JOIN dim_author_sf ON dim_book_sf.author_id = dim_author_sf.author_id
..........
WHERE dim_city_sf.city = 'Vancouver' AND dim_author_sf.author = 'Octavia E. Butler' AND dim_time_sf.quarter = 4 AND dim_time_sf.year = 2018;
```

```
Normalization saves space
Denormalized databases enable data redundancy
Normalization eliminates data redundancy
```

**Normalization ensures better data integrity**
- Enforces data consistency
- Safer updating, removing, and inserting (less data redundancy = less records to alter)
- Easier to redesign by extending (smaller tables are easier to extend than larger tablea)

**Database Normalization**  
**Advantages:**
- Eliminates data redundancy: save on storage
- Better daya integrity: accurate and consistent data

**Disadvantages:**
- Complex queries require more CPU

**OLTP -> operational databases**
- Typically highly normalized
- Write-intensive
- Prioritize quicker and safer insertion of data

**OLAP -> data warehouses**
- Typically denormalized
- Read-intensive
- Prioritize quicker queries for analytics

```sql
-- Star schema
-- Output each state and their total sales_amount
SELECT dim_store_star.state, SUM(fact_booksales.sales_amount)
FROM fact_booksales
	-- Join to get book information
    JOIN dim_book_star ON fact_booksales.book_id = dim_book_star.book_id
	-- Join to get store information
    JOIN dim_store_star ON fact_booksales.store_id = dim_store_star.store_id
-- Get all books with in the novel genre
WHERE  
    dim_book_star.genre = 'novel'
-- Group results by state
GROUP BY
    dim_store_star.state;
```

```sql
-- Snowflake schema    
-- Output each state and their total sales_amount
SELECT dim_state_sf.state, SUM(fact_booksales.sales_amount)
FROM fact_booksales
    -- Joins for genre
    JOIN dim_book_sf on fact_booksales.book_id = dim_book_sf.book_id
    JOIN dim_genre_sf on dim_book_sf.genre_id = dim_genre_sf.genre_id
    -- Joins for state 
    JOIN dim_store_sf on fact_booksales.store_id = dim_store_sf.store_id 
    JOIN dim_city_sf on dim_store_sf.city_id = dim_city_sf.city_id
	JOIN dim_state_sf on  dim_city_sf.state_id = dim_state_sf.state_id
-- Get all books with in the novel genre and group the results by state
WHERE  
    dim_genre_sf.genre = 'novel'
GROUP BY
    dim_state_sf.state;
```

```sql
-- Add a continent_id column with default value of 1
ALTER TABLE dim_country_sf
ADD COLUMN continent_id int NOT NULL DEFAULT(1);
```

```sql
-- Add the foreign key constraint
ALTER TABLE dim_country_sf ADD CONSTRAINT country_continent
   FOREIGN KEY (continent_id) REFERENCES dim_continent_sf(continent_id);
```

```sql 
-- Output updated table
SELECT * FROM dim_country_sf;
```

**Normal forms**  
**Normalization**: identify repeating groups of data and create new tables for them

- A more formal definition:
    - Be able to characterize the level of redundancy in a relational schema
    - Provide mechanism for transforming schemas in order to remove redundancy

**Normal forms (NF):**  
Ordered from least to most normalized
- First normal form (1NF)
- Second normal form (2NF)
- Third normal form (3NF)
- Elemtary key normal form (EKNF)
- Boyce-Codd normal form (BCNF)
- Fourth normal form (4NF)
- Essential tuple normal form (ETNF)
- Fifth normal form (5NF)
- Domain key normal form (DKNF)
- Sixth normal form (6NF)

**1NF rules:**
```
- each record must be unique - no duplicate rows
- each cell must hold one value

student id - student email - course_completed  
235 - jim@gmail.com - intro to python, intermediate python 

1NF form:
student_id - student_email  
235 - jim@gmail.com

student_id - course_completed
235 - intro to python
235 - intermediate python
```

**2NF rules:**
```
- must satisfy 1NF AND
    - if primary key is one column then automatically satisfies 2NF
    - if there is a composite primary key then each non-key column must be dependent on all the keys

composite primary key: when two or more columns are used as a primary key

student_id(PK) - course_id (PK) - instructor_id - instructor_name - progress  
235 - 2001 - 560- Nick Carchedi - .55

instructor isn't dependent on student_id, only on course_id

2nf form:
student_id(PK) - course_id (PK) - progress
235 - 2001 - .55

course_id (PK) - instructor_id - instructor_name
2001 - 560 - Nick Carchedi
```

**3NF rules:**
```
- satisfies 2NF AND
    - no transitive dependencies
    - non key columns can't depend on other non-key columns

course_id (PK) - instructor_id - instructor_name - tech  
2001 - 560 - Nick Carchedi - python

course_id (PK) - instructor_id - tech 
2001 - 560 - python

instructor_id - instructor_name  
560 - Nick Carchedi
```

**Data anomalies**  
What is risked if we don't normalize enough?

1. Update anomaly
2. Insert anomaly
3. Delete anomaly

**update anomaly**: data inconsistency cause by data redundancy when updating
**insertion anomaly**: unable to add data because of missing data
**delete anomaly**: unintentional loss of data when deleting

The more normalized the database, the less likely these anomalies are to occur

```sql
-- Create a new table to satisfy 2NF
CREATE TABLE cars (
  car_id VARCHAR(256) NULL,
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128),
  condition VARCHAR(128),
  color VARCHAR(128)
);
```

```sql
-- Insert data into the new table
INSERT INTO cars
SELECT DISTINCT
  car_id,
  model,
  manufacturer,
  type_car,
  condition,
  color
FROM customer_rentals;
```

```sql
-- Drop columns in customer_rentals to satisfy 2NF
ALTER TABLE customer_rentals
DROP COLUMN model,
DROP COLUMN manufacturer,
DROP COLUMN type_car, 
DROP COLUMN condition ,
DROP COLUMN color;
```

```sql
-- Create a new table to satisfy 3NF
CREATE TABLE car_model(
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128)
);
```

```sql
-- Drop columns in rental_cars to satisfy 3NF
ALTER TABLE rental_cars
DROP COLUMN model,
DROP COLUMN manufacturer;
```

**Database views** 

**A view** -> the result set of a stored query on the data, which the database users can query just as they would in a persistent database collection object  
- Virtual table that is not oart of the physical schema
- Query, not data is stored in memory
- Data is aggregated from data in tables
- Can be queried like a regular database table
- No need to retype common queries or alter schemas

```sql
CREATE VIEW view_name AS
SELECT col1, col2
FROM table_name
WHERE condition;
```

```sql
-- Viewing views
-- Include system views
SELECT * FROM INFORMATION_SCHEMA.views;
```

```sql
-- Exclude system views:
SELECT * FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');
```

**Benefits of views:**
- Doesnt take up storage
- A form of access control 
- Masks complexity of queries

```sql
-- Create a view for reviews with a score above 9
CREATE VIEW high_scores AS
SELECT * FROM REVIEWS
WHERE score > 9;
```

```sql
-- Count the number of self-released works in high_scores
SELECT COUNT(*) FROM high_scores
INNER JOIN labels ON high_scores.reviewid = labels.reviewid
WHERE label = 'self-released';
```

**Managing views**  
**Granting and revoking access to a view:**

```sql
GRANT privilege(s) or REVOKE privilege(s)
ON object
TO role or FROM role
```
```
Privileges: SELECT, INSERT, UPDATE, DELETE
Objects: table, view, schema, etc
Roles: a database user or a group of database users
```

```sql
GRANT UPDATE ON ratings TO PUBLIC;
REVOKE INSERT ON films FROM db_user;
```

**Updating a view:**
```sql
UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama';
```

```
- Note all views are updatable
- View is made up of one table
- Doesn't use a window or aggregate function
```

**Inserting into a view:**
```sql
INSERT INTO view_name
VALUES (value1, value2, ...);
```

```
- Not all views are insertable
- Avoid modifying data through views
```

**Dropping a view:**
```sql
DROP VIEW view_name [CASCADE | RESTRICT];
```

**Restrict** -> only drop the view if no other objects depend on it
**Cascade** -> drop the view and all objects that depend on it

**Redefining a view:**
```sql
CREATE OR REPLACE VIEW view_name AS new_query;
```

- if a view with view_name exists, it will be replaced
- new_query must have the same number of columns as the original view
- the column output may be different
- new column may be added at the end

**Altering a view:**
```sql
ALTER VIEW [IF EXISTS] view_name ALTER [COLUMN] column_name SET DATA TYPE new_data_type;
ALTER VIEW [ IF EXISTS] view_name RENAME COLUMN column_name TO new_column_name;
```

```sql
-- Revoke everyone's update and insert privileges
REVOKE UPDATE, INSERT ON long_reviews FROM PUBLIC; 
```

```sql
-- Grant the editor update and insert privileges 
GRANT UPDATE, INSERT ON long_reviews TO editor; 
```

```sql
-- Redefine the artist_title view to have a label column
CREATE OR REPLACE VIEW artist_title AS
SELECT reviews.reviewid, reviews.title, artists.artist, labels.label
FROM reviews
INNER JOIN artists
ON artists.reviewid = reviews.reviewid
INNER JOIN labels
ON labels.reviewid = artists.reviewid;
```

```sql
SELECT * FROM artist_title;
```

**Materialized views**  

**Views:** 
- also known as non materialized views
- how we've defined views so far

**Materialized views:**
- physically views stored in memory
- stores the query result not the query
- querying a materialized view means accessing the stored result
- refreshed or rematerialized to update the data when prompted

**When to use materialized views:**
- long running queries
- underlying query result dont change often
- data warehouse because OLAP is not write-intensive

```sql
CREATE MATERIALIZED VIEW view_name AS 
SELECT * FROM table_name;
```

```sql
REFRESH MATERIALIZED VIEW view_name;
```

**Database roles and access control**

**Database roles:**
- Manage database access and permissions
- A database role in an entity that contains information that:
    - Define the role's privileges
      - Can you login?
      - Can you create databases?
      - can you write to tables?
    - Interact with the client authentication system
      - Password
- Roles can be assigned to one or more users
- Roles are global across a database cluster installation

**Create a role:**
```sql
-- Empty role
CREATE ROLE role_name;
```

**Role with some attributes set**
```sql
CREATE ROLE intern WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2020-01-01';
```

```sql
CREATE ROLE admin CREATEDB;
ALTER ROLE admin CREATEROLE;
```

```sql
GRANT UPDATE ON ratings TO data_analyst;
REVOKE UPDATE ON ratings FROM data_analyst;
```

**The available privileges in PostgreSQL are:**
```
- SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, CONNECT, TEMPORARY, EXECUTE, and USAGE
```

**Users and groups (are both roles):**
- A role is an entity that can function as a user and/or a group
    - User roles
    - Group roles

```sql
CREATE ROLE data_analyst;
CREATE ROLE alex WITH PASSWORD 'PasswordForAlex' VALID UNTIL '2020-01-01';
GRANT data_analyst TO alex;
```

```sql
-- Create an admin role
CREATE ROLE admin WITH CREATEDB CREATEROLE;
```

```sql
-- Grant data_scientist update and insert privileges
GRANT UPDATE, INSERT ON long_reviews TO data_scientist;
```

```sql
-- Give Marta's role a password
ALTER ROLE Marta WITH PASSWORD 's3cur3p@ssw0rd';
```

```sql
-- Add Marta to the data scientist group
GRANT data_scientist TO Marta;
```

```sql
-- Remove Marta from the data scientist group
REVOKE data_scientist FROM Marta;
```

**Table partitioning**  
**Why partition?**
- Tables grow (100gb / 1tb / 10tb)

Problem: queries/update become slower  
Because: e.g. indices don't fit memory

**Solution**: split table into smaller tables (partitions)

**Data modeling refresher**
1. Conceptual data model
2. Logical data model
    For partitioning, logical data model is the same
3. Physical data model
    Partitioning is part of the physical data model

**Vertical partitioning:**  
Split table even when fully normalized     
e.g. store long_description on slower medium  

**Horizontal partitioning:**  
Split table over the rows  
e.g. partition the table by the timestamp

```sql
CREATE TABLE sales (
    ....
    timestamp DATE NOT NULL
)
PARTITION BY RANGE (timestamp);
```

```sql
CREATE TABLE sales_2019_q1 PARTITION OF sales
FOR VALUES FROM ('2019-01-01') TO ('2019-04-01');
```

```sql
CREATE INDEX ON sales('timestamp');
```

**Pros:**
- Indices of heavily-used partitions fit in memory
- Move to specific medium: slower vs faster
- Used for both OLTP and OLAP

**Cons:**
- Partitioninng existing table can be a hassle
- Some constraints can not be set

```sql
-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);
```

```sql
-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;
```

```sql
-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);
```

```sql
-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;
```

```sql
-- Drop the descriptions from the original table
ALTER TABLE film
DROP COLUMN long_description;
```

```sql
-- Join to view the original table
SELECT * FROM film_descriptions
JOIN film USING(film_id);
```

```sql
-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY RANGE (release_year);
```

```sql
CREATE TABLE sales (
    id INT,
    sales_date DATE,
    amount DECIMAL
)
PARTITION BY RANGE (YEAR(sales_date)) (
    PARTITION p1 VALUES LESS THAN (2016),
    PARTITION p2 VALUES LESS THAN (2021),
    PARTITION p3 VALUES LESS THAN (2026)
);
```

```sql
-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);
```

```sql
CREATE TABLE sales (
    id INT,
    country VARCHAR(50),
    amount DECIMAL
)
PARTITION BY LIST (country) (
    PARTITION p1 VALUES IN ('Indonesia', 'Malaysia'),
    PARTITION p2 VALUES IN ('USA', 'Canada'),
    PARTITION p3 VALUES IN ('Japan', 'South Korea')
);
```

```sql
-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');

CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');

CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');
```

```sql
-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);
```

```sql
-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');

CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');

CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');
```

```sql
-- Insert the data into film_partitioned
INSERT INTO film_partitioned
SELECT film_id, title, release_year FROM film;
```

```sql
-- View film_partitioned
SELECT * FROM film_partitioned;
```

**Data integration**  
Combines data from different sources, formats, technologies to provide users with a translated and unified view of that data


**Business case examples:**
- 360 degree customer
- acquisition
- legacy Systems

```
Data source 1 (postgresql) ----
Data source 2 (mongodb)  ----- Unified data model (Redshift)
Data source 3 (csv)  ------
```

Choosing a data integration tools:
- flexible
- reliable
- scalable

**Different DBMS types:**

**SQL**: RDBMS  
**NoSQL**: key-value store, document store, columnar database, graph database

<br>

## Data Warehousing Concept

**Data warehouse:** A computer system designed to store and analyze large amounts of data for an organization

**What does a data warehouse do?**
- Gathers data from different areas of an organization
- Integrates and stores the data
- Make it available for analysis

**Why is a data warehouse valuable?**
Organizations implement data warehouse in order to:*
- Support business intelligence activity
- Enable effective organizational analysis and decision-making
- Find ways to innovate based on insights from their data

**Common scenarios:**
- Product sales forecasting
- Governance and regulation adherence
- Insight and growth

**Difference between data warehouses and data lakes:**

- **Database:**
    - Structures data in rows and columns
    - Transactional databases store transactions
- **Data warehouse:**
    - Gather data, integrate, and make available for analysis
    - Many input data sources
    - Store structured data
    - Complex to change
    - Upstream and downstream effects must be considered
    - Typically > 10 gigabytes
    - How quickly query will run on a large amount of data
    - Avoid slowing down transactional database
- **Data marts:**
    - a relational database for analysis
    - Data is focused on one subject area 
    - Few input data sources
    - Typically < 100 GBs

- **Data lakes:**
    - Entire organization store of data
        - Contains data from many departments
        - Many data input sources
        - Typically > 100 gb in size
    - Stores structured and unstructured data
    Example: video, audio, and documents
    - Less complex to make changes
        - Fewer upstream and downstream effects to consider
    - Purpose to store data may not be known
        - Less organized

**Data warehouse support organizational analysis:**

- High-level life cycle data warehouse project  
- Planning -> Business requirements (Analyst, DS), data modelling ( analysts, DS, DE, DB admin)
- Implementation -> ETL design & development (DE, DB admin), BI application development (Analyst, DS)
- Support/ maintenance -> Maintenance (DE), test & deploy (Analyst, DS, DE)

**Warehouse Architectures and Properties**

Layer overview - data staging

```
Data source -> Data source 1 & 2 -> Data staging (ETL) -> Data storage (data warehouse & data mart) -> Data presentation (data analytics, reporting tools, analysis tools)
```

**Data source layer:**
- All data sources for data warehouse
- Example:
    - Transactional database
    - Log files
    - Spreadsheet

**Data staging (ETL)**
- Layer extracts, transform, and clean data through ETL process
- Contains ETL process and storage tables

**ETL process within data staging layer:**
- extracted
- business rules applied and cleaned
- staging database often used
- must be able to extract valid data
- batch / full loading

**Data storage layer**
- Data is stored in warehouse and data marts

Data presentation layer:
- Users interact with stored data
- Users:
    - Use BI (Business Intelligence) tools
    - Use data mining tools
    - Create direct queries

The presentation layer:
- Users interact with the presentation layer
  - Area of constant development

Presentation layer groups:
- Automated reporting/dashboarding tools
    - Goals: 
        - Create reports needed for decision-making
        - Create dashboards using historical data
        - Ex: tableau, looker, SAP

    - Users:
        - analysts
        - Citizen data scientists

- BI/data analytics
    - Goals:
        - Tools for Exploration
        - Looking for patterns
        - Ex: tableau, rapidminer, alteryx, looker, oracle, knime

    - Users:
        - analysts
        - Data scientist

- direct queries
    - Goals:
        - Sophisticated tools for Exploration
        - Ex: R, Azure, Python

    - Users:
        - Data scientists
        - Analysts
        - Data engineers

Data warehouse architecture:
Bill Inmon - top-down approach
Must decide:
- On all data definitions, cleaning, and business rules
- Before any data enters warehouse
Data source -> ETL -> Data warehouse -> Data mart -> BI tools

Pros and cons:
- Pros:
    - Single source of truth for organization
    - Normalization = less storage
    - Easy to change data marts to support reporting changes

- Cons:
    - More joins = slower response time
    - Lengthy upfront work

Kimball - bottom-up approach
Data source -> ETL -> Data mart -> Data warehouse -> BI tools

- Denormalize data 
- Focus on departmental data mart
- Data moves directly from ETL to data marts

Pros and cons:
- Pros:
    - Upfront development speed 
    - Lower startup cost
    - Denormalized = user friendly

- Cons:
    - Increase ETL processing time
    - Greater possibility of duplicate data
    - Ongoing development needed

OLAP and OLTP Systems
-------------------------------
OLAP (Online analytical processing)
- Designed to support analysis of large amounts of data
- Example dimensional organization:
    - country, state, city
    - years, months, days
- OLAP reorganizes data into multidimensional format

OLAP cube:
- OLAP cube key to OLAP system
- Faster processing vs. traditional relational databases
- Hypercubes have more than three dimensions

OLTP (Online transaction processing)
- Designed for processing simple database queries
- Used in source systems to data warehouse

Example for a credit card company:
OLTP: 
- System tracks customer's purchase
- Processes large amounts of simple database updates to account balances
- Rows and columns

OLAP:
- Designed for analyzing purchase data 
- Data organized by multiple dimensions

Data warehouse data modeling
---------------------------------------
Data models
- Bottom-up, kimbal model = star & snowflake schemas
- Denormalized data models

Example:
- Hypothetical publicly traded company
    - Sells home office furniture
    - Fact table =  sales_order_fact -> custid, dateid, productid, unitsold, salesamount, tax
        Fact table (measurements, metrics, or fact about an organization)
        - Links to dimension tables for more detail
    - Dimension table = cust_dim -> custid, custname, custaddress, custcity, custstate, custzip
        - Dimensionns/attribute about a process
        - Holds reference data
        - Dimension tables add more detail to fact table

Star schema:
- A central fact table, with one or more dimensional tables
- Easy for business users

Snowflake schema:
- Dimensional table connected through another dimensional table

Kimball's four step process:
1. Step 1- Select the organizational process
   - Ask questions about a process
   - Kimball bottom-ip approach starts with a business process
   - Example of organizational process:
     - Invoice and billing
     - Product quality monitoring
     - Marketing

2. Step 2 - Declare the grain
    - Grain = level to store fact table
    - A level of data cannot be split further
    - Example: 
      - Music service -> song grain
      - Shipping service -> Line item grain

3. Step 3- Identify the dimensions
    - Choose dimensions that apply to each row
    - Example:
      - Time: year, quarter, and month
      - Location: address, state, and country
      - Users: names and email address
    - How to describe the data?
    - Business users and analysts = valuable feedback

4. Identify the fact
    - Numerical facts for each fact table row
    - What are we answering?
    - Metrics should be true at selected grain
    - Example:
      - Music service: total number of plays, sales revenue a song
      - Ride-sharing: travel distance, time needed

Slowly changing dimensions:

The challenge
Original data:
productid = 12345
description = Tesla-ModelY
category = electric-veh

update category:
currect: electric-veh
new: electric-crossover

Type I: 
- Update value in table 
Category = electric-crossover
- Will lose historical data

Type II:
- Add a row with the updated value
New:
productid = 12345
description = Tesla-ModelY
category = electric-crossover
startdate = 2020-01-01
enddate = 9999-12-31
- The history is retained

Type III:
- Add column to dimension table to track changes
New: 
productid = 12345
description = Tesla-ModelY
category = electric-crossover
pastcategory = electric-veh
- Can view past and current data together
- Can require reporting changes and limited tracking

Row vs. column store data store
-------------------------------------
Why is it important?
- Optimizing queries for speed
- Column store format for data warehouse tables is best for analytic workloads

Basics of computer storage:
- Computer store data in blocks
- Reads the required blosks when retrieving data
- Reading fewer blocks increases the overall speed of the process

Row store:
- Stores data in rows
- Reads all columns for a row
- Good for OLTP systems

Column store:
- Stores data in columns
- Reads only the columns needed for a query
- Good for OLAP systems
- Better data compression

Implementation and Data Prep
-------------------------------------
ETL                 |       ELT
1. Extract         |       1. Extract
2. Transform       |       2. Load
3. Load            |       3. Transform

ETL:
- Data transformed during the moves
- Uses separate system to process data

Pros:
- Lower data storage costs
- Personally identifiable information (PII) security compliance

Cons:
- Transformation errors/changes require new data pulls
- Costs of separate system for process data

ELT:
- Data is loaded, then transformed
- Uses the warehouse to transform the data

Pros:
- No need system to process data
- Transformations can be rerun without impacting source systems
- Works well for near real-time requirements

Cons:
- Increases storage needs from raw data
- Compliance with PII secutiry standards

Data cleaning
----------------------------
Data format cleaning:
- Update values to an expected format
    - date
    - names
    - capitalization
- Ensures output is in a consistent format

Address parsing:
- dividing a street address into its components
- can use tools to validate addresses

Data validation:
- Range check 
    - Is the value within expected range? - a person age
- Type check 
    - Is the value the proper data type? Storing age vs number

Duplicate row elimination
- This process gets rid of duplicate entries

On premise and cloud data warehouse:
-------------------------------------
On premise:
- purchase and install software and hardware
- on the grounds of the organization

pros:
- complete control
- implement custom data governance
- local network speed
- can optimize for workloads

cons:
- upfront hardware and software costs
- Personnel/staff must maintain system
- Must keep up with patches and security

In the cloud:
- Rapid growth
- Forecasted continued growth

Pros:
- No maintaining equipment and infrastructure
- Frees up IT staff
- Can scale storage and computer resources
- No upfront costs

Cons:
- less control
- cannot optimize warehouse workloads
- possible unanticipated costs

Data warehouse design example:
- New startup company
- Photo sharing app

Top-down, or bottom-up approach?
- Consideration:
    - Vital to show business impact quickly
    - Top-down approach has a longer startup process
- Decision:
    - Bottom-up approach
    - Sales data mart must be the priority

Kimball 
Step 1 - Select the organizational process 
Considerations:
- What type of customers purchase large volume of photos?
Decision:
- Develop customer purchase

Step 2 - Declare the grain
Considerations:
- Data should be flexible to answer many questions
- Selecting the lowest grain possible
Decision:
- Tracking customer/photo purchase

Step 3 - Identify the dimensions
Considerations:
- How do users describe the data that results from the business process?
- Customer prioritization
Decision:
- Customer location (country, state)
- Date customer joined
- Default payment method

Step 4 - Identify the fact
Considerations:
- What are answering?
Decision:
- Time spent viewing photo before purchase
- Photo cost and tax
- date of purchase

fact_table:
custid, dateid, productid, unitsold, salesamount, tax

dimension_table:
cust_dim -> custid, custname, custaddress, custcity, custstate, custzip
photo_dim -> photoid, photodesc, phototype, photoprice
date_dim -> dateid, day, month, year

On premise vs. cloud data warehouse:
Considerations:
- we dont want upfront costs
- small team
decision:
- cloud data warehouse

ETL vs. ELT:
Considerations:
- Keep all data
- Cloud implementation allows us to scale compute as needed

Decision:
- ELT

-------------------------------------
Introduction to Snowflake           |
-------------------------------------

Introduction to Snowflake: Architecture, Competitors, and SnowflakeSQL
-----------------------------------------------------------------------
What is Snowflake?
- Cloud data warehouse solution 
- Columnar data storage

Advantages:
- Scalability
- Accessibility
- Cost-efficiency
- Lower management effort

Row vs. Columnar database
Category           | Row                    | Columnar
Data organization    by row                    by column
Data retrieval       complete records          specific columns
Operational          OLTP                       OLAP
Example              Postgre, MySQL, Oracle    Snowflake, Amazon Redshift, Google BigQuery, Vertica
                     SQL Server

Snowflake use cases:
- Business Intelligence
- Data science
- Data Ingestion 
- Data warehousing
- Data sharing

Snowflake Architecture
----------------------
Shared-disk and shared-nothing architecture
- Shared-disk: 
    - Data stored on a central disk
    - All nodes can access the data
    - Data is distributed across all nodes

- Shared-nothing:
    - Data stored on each node
    - Each node has its own disk
    - Data is not shared between nodes

Decoupling storage and compute: (Separating storage and compute)
- Efficient data storage
- Independent data processing
- Components operate without interdependence

Benefits of Decoupling:
- Enhanced scalability 
- faster data processing and response
- Cost-effective operations

Snowflake architecture:
infrastructure manager, optimizer, metadata manager, security -> cloud service layer
virtual warehouse -> compute layer 
data storage -> storage layer

Storage layer:
- Columnar storage format
   - Efficient data retrieval
   - analysis
- Optimized
- Compressed 
- Consists of tables, schemas, and databases

Compute layer:
- Query execution 
- Virtual warehouses

Cloud services layer:
- Infrastructure management
- Query optimization 
- authentication
- access control 
- security

Snowflake competitors:
- Google BigQuery
- Amazon Redshift
- Databricks
- PostgreSQL


Security & data support:
Security:
- Access control
- Encryption 

Semi structured data support:
- JSON
- Avro
- Parquet
- CSV

What makes Snowflake unique?
- Unique architecture - decoupling storage and compute
- Secure data sharing
- High performance
- Multi-cloud support

Why use Snowflake?
- Large-scale data warehousing
- Multi
- Pricing
- Ease of use

-- Count all pizza entries
SELECT COUNT(*) AS count_all_pizzas
FROM pizza_type
-- Apply filter on category for Classic pizza types
WHERE category = 'Classic'
-- Additional condition to filter where name has Cheese in it
AND name LIKE '%Cheese%';


Snowflake SQL and key concepts
--------------------------------
Connecting to Snowflake and DDL commands
Connecting to Snowflake:
- Snowsight: Snowflake web interface (worksheets)
- Drivers & connectors: ODBC (Open Database Connectivity) and JDBC (Java Databace Connectivity) Drivers, Python/Spark 
- SnowSQL: Command-line client, installed on linux, windows, or mac

Staging:
- Temporary location storing data
- Internal stage
- External stage (cloud storage: amazon s3, google cloud storage)
    - Raw data sources: initial, unprocessed data, e.g. CSV files
    - Staging area: temporary storage, loading
    - Table: final data is loaded here

Create stage:
CREATE STAGE my_local_stage

PUT file:///path_to_your_local_file/orders.csv
@my_local_stage -- stage naame prefixed with @

@ -> Prefix to reference stage

DDL commands:
CREATE, ALTER, DROP, RENAME, COMMENT

CREATE TABLE orders_pizza (
    order_id NUMBER,
    order_date DATE,
    time TIME
)

CREATE VIEW orders_pizza_view AS
SELECT order_id, order_date
FROM orders_pizza;

ALTER TABLE IF EXISTS orders_pizza
RENAME TO orders;

ALTER TABLE orders
RENAME COLUMN time TO order_time;

DROP TABLE orders;

Comment:
CREATE TABLE pizza_type(
    pizza_type_id VARCHAR(50) COMMENT 'Unique identifier for pizza type',
    name VARCHAR(100),
    category VARCHAR(50),
    ingridents VARCHAR(500)
)
COMMENT = 'Table that stores information about different type of pizzas, including their names, categories, and ingridents';

postgresql:
COMMENT ON [object type] [object name] IS 'comment';
COMMENT ON TABLE pizza_type IS 'Table with pizza type info';

DML commands:
SHOW, DESCRIBE, INSERT, UPDATE, MERGE, COPY

SHOW databases -> show available databases

SHOW TABLES IN { DATABASE [db_name]}
SHOW TABLES IN DATABASE pizza_sales -> show tables in database

SHOW TABLES [LIKE 'pattern]
            [IN {DATABASE [db_name]}]
SHOW TABLES LIKE '%pizza%' IN DATABASE pizza_sales

SHOW SCHEMAS IN DATABASE pizza_sales
SHOW COLUMNS IN pizza_type

SHOW VIEWS IN DATABASE pizza_sales

DESCRIBE DATABASE pizza_sales
DESCRIBE TABLE pizza_type
DESCRIBE VIEW orders_pizza_view
DESCRIBE STAGE my_local_stage

INSERT INTO orders (order_id, order_date, order_time)
VALUES (1, '2015-01-01', '11:38:36')

INSERT INTO orders_filtered
SELECT * FROM orders
WHERE order_date > '2015-01-02'

UPDATE orders
SET order_time = '17:00:00'
WHERE order_id = '5'

MERGE INTO orders_filtered AS target -- target table
USING orders AS source -- source table
ON target.order_id = source.order_id --Common column
WHEN MATCHED THEN -- When there is a match
    UPDATE SET -- Update order_date and time of target table
    target.order_date = source.order_date
    target.time = source.order_time

Load data from staging to table:
COPY INTO pizza_type FROM @my_local_stage/orders.csv
FILE_FORMAT = (TYPE = 'CSV' SKIP_HEADER=1)

Snowflake data type and data conversion:
Data types:
VARCHAR
NUMERIC
INT
DATE
TIME
TIMESTAMP
VARIANT

VARCHAR Max length -> Snowflake = 16.777.216 , Postgresql = 65.535
Numeric default precision -> 38 and 37
Integer range -> +- 10^37 and 32-bit range

Data type conversion:
CAST ((source_data) AS (target_data_type))
CAST ('80' AS INT)

::
((source_data) :: (target_data_type))
'80'::INT

SELECT CAST(order_date AS timestamp) AS order_timestamp
FROM orders

SELECT TO_DATE('2023-08-16 11:51:00')

-- Convert request_id to VARCHAR aliasing to request_id_string
SELECT CAST(request_id AS VARCHAR) AS request_id_string
FROM uber_request_data

-- Convert request_id to VARCHAR using CAST method and alias to request_id_string

SELECT CAST(request_id AS VARCHAR) AS request_id_string, 
-- Convert request_timestamp to DATE using TO_DATE and alias as request_date
	   TO_DATE(request_timestamp) AS request_date
FROM uber_request_data

-- Convert request_id to VARCHAR using CAST method and alias to request_id_string
SELECT CAST(request_id AS VARCHAR) AS request_id_string, 
-- Convert request_timestamp to DATE using TO_DATE and alias as request_date
	   TO_DATE(request_timestamp) AS request_date,
-- Convert drop_timestamp column to TIME using :: operator and alias to drop_time
       drop_timestamp::TIME AS drop_time
FROM uber_request_data
-- Filter the records where request_date is greater than '2016-06-01' and drop_time is less than 6 AM.
WHERE request_date > '2016-06-01' AND drop_time < '06:00:00'

Functions, sorting, and grouping
----------------------------------------
- Aggregate
AVG(), SUM(), MIN(), MAX(), COUNT()

- string
CONCAT()
SELECT CONCAT(category, ' - Pizza') AS pizza_category
FROM pizza type

UPPER(), LOWER()

- date and time
CURRENT_DATE(), CURRENT_TIME()
EXTRACT() -> EXTRACT(YEAR FROM drop_timestamp) AS YEAR

GROUP BY ALL -> group column not separated

-- Complete the CONCAT function for columns pickup_point and status
SELECT CONCAT('Trip from ', pickup_point, ' was completed with status: ', status) AS trip_comment
FROM uber_request_data

-- Retrieve order_id, pizza_id and sum of quantity
SELECT order_id, pizza_id, SUM(quantity) AS total_quantity
FROM order_details
-- GROUP the orders using group by all having total_quantity greater than 3
GROUP BY ALL
HAVING SUM(quantity) > 3
-- ORDER BY order id and total quantity in descending order
ORDER BY order_id, total_quantity DESC

-- Select the current date, current time
-- Concatenate and convert it to TIMESTAMP
SELECT *,
-- Extract month and alias to concat_month
	EXTRACT(MONTH FROM CONCAT(CURRENT_DATE, ' ', CURRENT_TIME)::TIMESTAMP) AS concat_month
-- Use table uber_request_data where request_timestamp's month is greater than or equal to concat_month
FROM uber_request_data
WHERE EXTRACT(month FROM request_timestamp) >= concat_month

Advance Snowflake SQL Concepts
---------------------------------
Joining on Snowflake
INNER Joins
OUTER Joins
CROSS Joins
SELF Joins
NATURAL Joins
LATERAL Joins

Natural join -> automatically mathing same name column
SELECT * FROM pizzas AS p
NATURAL JOIN pizza_type AS t
WHERE pizza_type_id = 'bbq'

LATERAL -> subquery
SELECT p.pizza_id, lat.name, lat.category
FROM pizzas AS p
LATERAL 
    (SELECT * 
    FROM pizza_type AS t
    WHERE p.pizza_type_id = t.pizza_type_id) AS lat

SELECT 
    pt.category,
	-- Calculate total_revenue 
    SUM(p.price * od.quantity) AS total_revenue
FROM order_details AS od
-- NATURAL JOIN all tables
NATURAL JOIN pizzas AS p 
NATURAL JOIN pizza_type AS pt
-- GROUP the records by category from pizza_type table
GROUP BY pt.category
-- ORDER by total_revenue and limit the records
ORDER BY total_revenue DESC
LIMIT 1

SELECT COUNT(o.order_id) AS total_orders,
        AVG(p.price) AS average_price,
        -- Calculate total revenue
        SUM(p.price * od.quantity) AS total_revenue,
        -- Get the name from pizza_type table
		pt.name AS pizza_name
FROM orders AS o
-- Use appropriate JOIN
LEFT JOIN order_details AS od
ON o.order_id = od.order_id
-- Use appropriate JOIN with pizzas table
RIGHT JOIN pizzas p
ON od.pizza_id = p.pizza_id
-- Natural join pizza_type table
NATURAL JOIN pizza_type AS pt
GROUP BY pt.name, pt.category
ORDER BY total_revenue desc, total_orders desc

Subquerying and common table expressions
-----------------------------------------
WITH cte1 AS ( -- ctename
            SELECT col1, col2
            FROM table1
),
cte2 AS (
    SELECT ...
    FROM ....
)
    

SELECT ...
FROM cte1;

WITH max_price AS (
    SELECT pizza_type_id, MAX(price) AS max_price
    FROM pizzas
    GROUP BY pizza_type_id
)

SELECT pt.name, pz.price, pt.category
FROM pizzas AS pz
JOIN pizza_type AS pt pz.pizza_type_id = pt.pizza_type_id
JOIN max_price AS mp 
ON pt.pizza_type_id = mp.pizza_type_id
WHERE pz.price < mp.max_price

WHy use CTE?
- Managing complex operations
- Readable 
- Modular
- Reusable

SELECT 
    pt.name, 
    pt.category, 
    SUM(od.quantity) AS total_quantity
FROM pizza_type AS pt
-- Join pizzas and order_details table
JOIN pizzas AS pz
    ON pt.pizza_type_id = pz.pizza_type_id
JOIN order_details AS od
    ON pz.pizza_id = od.pizza_id
-- Group by name and category
GROUP BY pt.name, pt.category
HAVING SUM(od.quantity) < (
    -- Calculate AVG of total_quantity 
    SELECT AVG(total_quantity)
    FROM (
        SELECT SUM(od2.quantity) AS total_quantity
        FROM pizzas AS pz2
        JOIN order_details AS od2
            ON pz2.pizza_id = od2.pizza_id
        GROUP BY pz2.pizza_type_id
    ) AS sub
)
-- Order  by total_quantity in ascending order
ORDER BY total_quantity ASC

-- Create a CTE named most_ordered and limit the results 
WITH most_ordered AS (
    SELECT pizza_id, SUM(quantity) AS total_qty 
    FROM order_details GROUP BY pizza_id ORDER BY total_qty DESC
    LIMIT 1
)
-- Create CTE cheapest_pizza where price is equals to min price from pizzas table
, cheapest_pizza AS(
    SELECT pizza_id, price
    FROM pizzas 
    WHERE price = (SELECT MIN(price) FROM pizzas)
    LIMIT 1
)
-- Select pizza_id and total_qty aliased as metric from first cte most_ordered
SELECT pizza_id, 'Most Ordered' AS Description, total_qty AS metric
FROM most_ordered
UNION ALL
-- Select pizza_id and price aliased as metric from second cte cheapest_pizza
SELECT pizza_id, 'Cheapest' AS Description, price AS metric
FROM cheapest_pizza

Snowflake query optimization:
- Transforming into more efficient queries
- Snowflake Cloud Service Layer

Why optimize queries in Snowflake?
- Achieve faster results
- Cost efficiency -> shorter query consumes fewer resources like CPU and memory

Common query problems:
- Exploding joins: Be cautios!
Incorrect:
SELECT * 
FROM order_details AS od
JOIN pizzas AS p -- Missing ON condition leading to exploding joins

- UNION or UNION ALL: Know the Difference
    - UNION remove duplicates, slows down the query
    - UNION ALL is faster if no duplicates

- Handling big data
    - Use filters to narrow down data
    - Apply limits for quicker results

SELECT TOP 10* example:
SELECT TOP 10*
FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF100.CUSTOMER

- Filter Early
    - Use WHERE clause early on 
    - Apply filters before JOIN s

Without early filtering:
SELECT 
FROM 
JOIN
ON 
JOIN
ON 
WHERE

With early filtering:
WITH filtered_orders AS (
    SELECT *
    FROM orders
    WHERE order_date > '2020-01-01' -- Apply filter early
)

SELECT 
FROM filtered_orders
JOIN
ON
JOIN
ON

Query history:
- snowflake.account_usage.query_history

SELECT 
    query_text,
    start_time,
    end_time,
    execution_time
FROM snowflake.account_usage.query_history
WHERE execution_time > 1000

WITH filtered_orders AS (
  -- Select order_id, order_date and filter records where order_date is greater than November 1, 2015.
  SELECT order_id, order_date
  FROM orders 
  WHERE order_date > '2015-11-01'
)

, filtered_pizza_type AS (
  -- Select name, pizza_type_id and filter the pizzas which has Veggie category
  SELECT name, pizza_type_id
  FROM pizza_type 
  WHERE category = 'Veggie'
)

SELECT o.order_id, o.order_date, pt.name, od.quantity
-- Get the details from filtered_orders CTE
FROM filtered_orders AS o
JOIN order_details AS od ON o.order_id = od.order_id
JOIN pizzas AS p ON od.pizza_id = p.pizza_id
-- JOIN CTE filtered_pizza_type on common column
JOIN filtered_pizza_type AS pt ON p.pizza_type_id = pt.pizza_type_id

Handling semi-structured data (JSON)
-----------------------------
JSON:
- Javascript Object Notion 
- Common use: Web APIs, Mobile Apps, Config files
- JSON data structure -> key-value pairs

{
    "name": "John",
    "age": 30,
    "city": "New York"
}

JSON in Snowflake
- Native JSON support
- Flexible for evolving schemas

Comparisons:
- Postgres: Uses JSONB
- Snowflake: Uses VARIANT (Object and Array data )

- Creating a Snowflake table to handle JSON data
CREATE TABLE json_data (
    id NUMBER,
    data VARIANT
)

PARSE_JSON
- expr: JSON data in string format
- Returns: VARIANT type, valid JSON object

SELECT PARSE_JSON(
    '{
        "name": "John",
        "age": 30,
        "city": "New York"
    }' -- enclose JSON data in single quotes
) AS customer_info_json

OBJECT_CONSTRUCT
- Syntax: OBJECT_CONSTRUCT(key1, value1, key2, value2, ...)
- Returns: JSON object

SELECT OBJECT_CONSTRUCT(
    'name', 'John',
    'age', 30,
    'city', 'New York'
) 

Querying JSON data in Snowflake:
- Simple JSON data
SELECT 
    customer_info:cust_age,
    customer_info:cust_name
    customer_info:cust_email
FROM 
    customer_info_json_data;

- Nested JSON data
<column>:<level1_element>:<level2_element>:<level3_element>
<column>:<level1_element>.<level2_element>.<level3_element>

SELECT 
    customer_info:address:street AS street_name
FROM 
    customer_info_json_data;

SELECT 
    customer_info:address.street AS street_name
FROM 
    customer_info_json_data;

SELECT
  name,
  categories,
  -- Select WheelchairAccessible from attributes converting it to STRING
  attributes:WheelchairAccessible::STRING AS wheelchair_accessible,
  -- Select Saturday, Sunday from hours converting it to STRING
  hours:Saturday::STRING IS NOT NULL OR hours:Sunday::STRING IS NOT NULL AS open_on_weekend
FROM
  yelp_business_data

SELECT
  name,
  categories,
  -- Select WheelchairAccessible from attributes converting it to STRING
  attributes:WheelchairAccessible::STRING AS wheelchair_accessible,
  -- Select Saturday, Sunday from hours converting it to STRING
  (hours:Saturday::STRING IS NOT NULL OR hours:Sunday::STRING IS NOT NULL) AS open_on_weekend
FROM
  yelp_business_data
WHERE
  	-- Filter where wheelchair_accessible is 'True' and open_on_weekend is 'true'
    wheelchair_accessible = 'True' AND open_on_weekend = 'true'
    -- Filter further where categories is having Italian in it
    AND categories LIKE '%Italian%'

-- Create CTE dogs_allowed.
WITH dogs_allowed AS (
  SELECT *
  FROM yelp_business_data
  WHERE attributes:DogsAllowed::STRING  NOT ILIKE '%None%'
  -- Filter data where DogsAllowed is True.
  AND attributes:DogsAllowed::STRING = 'True'
)

SELECT business_id, 
  name
FROM dogs_allowed

WITH dogs_allowed AS (
  SELECT * 
  FROM yelp_business_data
  WHERE attributes:DogsAllowed::STRING  NOT ILIKE '%None%'
  AND attributes:DogsAllowed::STRING = 'True' 
)

, touristy_places AS (
  SELECT *
  FROM yelp_business_data
  WHERE attributes:Ambience NOT ILIKE '%None%'
    AND attributes:Ambience IS NOT NULL
    AND attributes:Ambience NOT ILIKE '%u\'%'
    -- Convert Ambience attribute in the attributes columns into valid JSON using PARSE_JSON.
    -- From Valid JSON, fetch the touristy attribute and check if it is true when casted to BOOLEAN.
    AND PARSE_JSON(attributes:Ambience):touristy::BOOLEAN = true
)

SELECT
	d.business_id,
    d.name
FROM dogs_allowed d
JOIN touristy_places t
	ON d.business_id = t.business_id

------------------------------------------------
Understanding Data Visualization               |
------------------------------------------------
Three ways of getting insights:
- Calculating summary statistics -> mean, median, standard deviation
- Running models -> linear and logistic regression 
- Drawing plots -> scatter, bar, histogram

Continuous and categorical variables:
Continuous -> usually number -> heights, temperature, revenues
Categorical -> usually text -> eye colors, countries, Industry
Can be either -> age is continuous, but age group is categorical
                 time is continuous, month of year is categorical

Histogram 
- If you have a single continuous variables
- You want to ask questions about the shape of its distribution 

bins = intervals

Modality: how many peaks?
Unimodal 
Bimodal
Trimodal

Skewness: Is is symmetric?
lef-skewed -> penuh di kanan, outlier di kiri
symmetric
right-skewed -> penuh di kiri, outlier di kanan

Kurtosis: how many extreme values?
leptokurtic -> narrow peak, lots of extreme values
mesokurtic -> normal distribution
platykurtic -> broad peak, few extreme value

Box plot:
- When you have a continuous variable, split by a categorical variable
- When you want to compare the distributions of the continuous variable for each category

line in the middle -> median
line below the middle -> lower quartile (one quarter of the values are below it)
line upper the middle -> upper quartile (three quarters of the values are below it)
low whisker -> nilai minimum 
upper whisker -> nilai maximum

Visualizing two variables
--------------------------
Scatter plot:
- You have two continuous variable
- You want to answer question about the relationship between the two variables
- Correlation: How close are you being able to fit a straight line through points?

garis ke atas kanan  -> positive correlation 
garis ke atas kiri -> negative correlation 
menyebar tapi ke kiri -> weak negative
menyebar tapi ke kanan -> weak positive
berkumpul di tengah -> tidak ada korelasi

Adding smooth trend lines or curve

Line plot:
- You have two continuous variables
- You want to answer questions about their relationship
- Consecutive observation are connected somehow

Bar plot:
- You have a categorical variable
- You want to counts or percentage for each category 
Ocassionaly:
- You want another numeric score for each category, and need to include zero in the plot

Dots plot:
- You have a categorical variable
- You want to display numeric scores for each category on a log scale
- You want to display multiple numeric scores for each category

The color and the shape
--------------------------
Higher dimensions:
- 3D scatter plot

x and y are not the only dimensions:
Points also have these dimensions
- color
- size
- transparency
- shape

Lots of panel

Others dimensions for line plots:
- color
- thickness
- transparency
- line type (solid, dashes, dots)

Using color:
Colorspaces:
- Red-Green-Blue
- Cyan-Magenta-Yellow-Black
- Hue- Chroma-Luminance

Hue (color of the rainbow)
Chroma (intensity of the color)
Luminance (brightness of the color)

Choosing a plotting palette:
- Usually each color should stand out as much as other colors
- The perceptual distance from one color in the next should be constant

Three types of color scale: qualitative
qualitative -> distinguish unordered categories -> hue
sequential -> show ordering -> chroma or luminance
diverging -> show above or below a midpoint -> chroma or luminance, with 2 hues , example: tidak setuju, netral, setuju

viridis => for color blind people

Plotting many variables at once
Pair plot:
- You have up to ten variables (either continuous, categorical, mix)
- You want to see the distribution for each variable
- You want to see the relationship between each pair of variables

When sould you use a correlation heatmap
- You have lots of continuos variables
- You want to simple overview of how each pair of variables is related

When you should use a parallel coordinates plot:
- You have lots of continuous variables
- You want to find patterns accross these variables
- You want to see how each observation is related to each other

Polar coordinates:
Bar plot + polar coordinates = pie plot

When you should use a polar coordinates:
- Almost never
- If you have a variable that is naturally circular (time, compass direction)

Histogram + polar coordinates = rose plot

When you should use a rose plot:
- You have a continuous variable that is naturally circular
- You want to see the distribution of this variable

Axes of evil:
- Nonsense bar length
- y axis does not start at zero
- y axis does not have a consistent scale
- dual axes are misleading
- Better to use multiple panels

Sensory overload:
Measures of a good visualization:
- How many interesting insights can your reader get from the plot?
- How quickly can they get thise insights?

Chartjunk:
Any element of the plot that distracts from reader getting insight:
- pictures
- skueomorphism: reflection ,shadows, etc
- extra dimensions
- ostentatious (norak) colors or lines


Yang mesti ditingkatkan:

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

Data Manipulation in SQL
------------------------------
CASE statement:
- Contains a WHEN, THEN, and ELSE statement, finished with an END

CASE WHEN x = 1 THEN 'a'
    WHEN x = 2 THEN 'b'
    ELSE 'c'
    END AS new_column_name

SELECT id, home_goal, away_goal, 
        CASE WHEN home_goal > away_goal THEN 'Home Team Win'
                WHEN home_goal < away_goal THEN 'Away Team Win'
                ELSE 'Draw'
                END AS match_result
FROM match
WHERE season = '2013/2014';

SELECT
	-- Select the team long name and team API id
	team_long_name,
	team_api_id
FROM teams_germany
-- Only include FC Schalke 04 and FC Bayern Munich
WHERE team_long_name IN ('FC Schalke 04', 'FC Bayern Munich');

SELECT 
	-- Select the date of the match
	date,
	-- Identify home wins, losses, or ties
	CASE WHEN home_goal > away_goal THEN 'Home win!'
        WHEN home_goal < away_goal THEN 'Home loss :(' 
        ELSE 'Tie' END AS outcome
FROM matches_spain;

SELECT 
	m.date,
	t.team_long_name AS opponent,
    -- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Barcelona win!'
        WHEN m.home_goal < m.away_goal THEN 'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
LEFT JOIN teams_spain AS t 
ON m.awayteam_id = t.team_api_id
-- Filter for Barcelona as the home team
WHERE m.hometeam_id = 8634; 

-- Select matches where Barcelona was the away team
SELECT  
	m.date,
	t.team_long_name AS opponent,
	CASE WHEN m.away_goal > m.home_goal THEN 'Barcelona win!'
        WHEN m.away_goal < m.home_goal THEN 'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
-- Join teams_spain to matches_spain
LEFT JOIN teams_spain AS t 
ON m.hometeam_id = t.team_api_id
WHERE m.awayteam_id = 8634;

SELECT 
	date,
	CASE WHEN hometeam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as home,
	CASE WHEN awayteam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as away,
	-- Identify all possible match outcomes
	CASE WHEN home_goal > away_goal AND hometeam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal > away_goal AND hometeam_id = 8633 THEN 'Real Madrid win!'
        WHEN home_goal < away_goal AND awayteam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal < away_goal AND awayteam_id = 8633 THEN 'Real Madrid win!'
        ELSE 'Tie!' END AS outcome
FROM matches_spain
WHERE (awayteam_id = 8634 OR hometeam_id = 8634)
      AND (awayteam_id = 8633 OR hometeam_id = 8633);

-- Select the season and date columns
SELECT 
	season,
	date,
    -- Identify when Bologna won a match
	CASE WHEN hometeam_id = 9857 
        AND home_goal > away_goal 
        THEN 'Bologna Win'
		WHEN awayteam_id = 9857 
        AND away_goal > home_goal 
        THEN 'Bologna Win' 
		END AS outcome
FROM matches_italy;

-- Select the season, date, home_goal, and away_goal columns
SELECT 
	season,
    date,
	home_goal,
	away_goal
FROM matches_italy
WHERE 
-- Exclude games not won by Bologna
	CASE WHEN hometeam_id = 9857 AND home_goal > away_goal THEN 'Bologna Win'
		WHEN awayteam_id = 9857 AND away_goal > home_goal THEN 'Bologna Win' 
		END IS NOT NULL;

SELECT 
	c.name AS country,
    --Count games from the 2012/2013 season
	COUNT(CASE WHEN m.season = '2012/2013' 
        	THEN m.id ELSE NULL END) AS matches_2012_2013
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Count matches in each of the 3 seasons
	COUNT(CASE WHEN m.season = '2012/2013' THEN m.id END) AS matches_2012_2013,
	COUNT(CASE WHEN m.season = '2013/2014' THEN m.id END) AS matches_2013_2014,
	COUNT(CASE WHEN m.season = '2014/2015' THEN m.id END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Sum the total records in each season where the home team won
	SUM(CASE WHEN m.season = '2012/2013' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2012_2013,
 	SUM(CASE WHEN m.season = '2013/2014' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2013_2014,
        SUM(CASE WHEN m.season = '2014/2015' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY c.name;

SELECT 
	c.name AS country,
    -- Calculate the percentage of tied games in each season
	AVG(CASE WHEN m.season='2013/2014' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2013/2014' AND m.home_goal != m.away_goal THEN 0
			END) AS ties_2013_2014,
	AVG(CASE WHEN m.season='2014/2015' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2014/2015' AND m.home_goal != m.away_goal THEN 0
			END) AS ties_2013_2014
FROM country AS c
LEFT JOIN matches AS m
ON c.id = m.country_id
GROUP BY country;

SELECT 
	c.name AS country,
    -- Calculate the percentage of tied games in each season
	ROUND(AVG(CASE WHEN m.season='2013/2014' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2013/2014' AND m.home_goal != m.away_goal THEN 0
			END), 2) AS pct_ties_2013_2014,
	ROUND(AVG(CASE WHEN m.season='2014/2015' AND m.home_goal = m.away_goal THEN 1
			WHEN m.season='2014/2015' AND m.home_goal != m.away_goal THEN 0
			END), 2) AS pct_ties_2014_2015
FROM country AS c
LEFT JOIN matches AS m
ON c.id = m.country_id
GROUP BY country;

Short and Simple Subqueries
----------------------------
Subquery -> A query nested inside another query

SELECT column
FROM (SELECT column FROM table) AS subquery;

- Can be in any part of a query (SELECT, FROM, WHERE, GROUP BY)
- Can return a variety of information 

Why subqueries:
- Comparing groups to summarize values (How did Liverpool compare to English Premier League average performance that year?)
- Reshaping data (What is the highest average of goals scored in the Bundesliga?)
- Combining data that cannot be joined (How do you get both the hoe and away names into a table of match results?)

- Subquery with WHERE
SELECT home_goal
FROM match
WHERE home_goal > (SELECT AVG(home_goal) FROM match);

SELECT date, hometeam_id, awayteam_id, home_goal, away_goal
FROM match
WHERE season = '2012/2013'
    AND home_goal > (SELECT AVG(home_goal) FROM match);

- Subquery with IN
SELECT team_long_name, team_short_name AS abbr
FROM team
WHERE team_api_id IN (SELECT hometeam_id FROM match WHERE country_id = 15722);

-- Select the average of home + away goals, multiplied by 3
SELECT 
	3 * AVG(home_goal + away_goal)
FROM matches_2013_2014;

SELECT 
	-- Select the date, home goals, and away goals scored
    date,
	home_goal,
	away_goal
FROM  matches_2013_2014
-- Filter for matches where total goals exceeds 3x the average
WHERE (home_goal + away_goal) >
       (SELECT 3 * AVG(home_goal + away_goal)
        FROM matches_2013_2014); 

SELECT 
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team 
-- Exclude all values from the subquery
WHERE team_api_id NOT IN 
     (SELECT DISTINCT hometeam_id  FROM match);

SELECT
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team
-- Filter for teams with 8 or more home goals
WHERE team_api_id IN
	  (SELECT hometeam_id 
       FROM match
       WHERE home_goal >= 8);

- Subquery in FROM
SELECT t.team_long_name AS team, AVG(n.home_goal) AS home_avg
FROM match AS m
LEFT JOIN team AS t
ON n.hometeam_id = t.team_api_id
WHERE season = '2011/2012'
GROUP BY team;

SELECT team, home_avg
FROM (SELECT t.team_long_name AS team, AVG(m.home_goal) AS home_avg
        FROM match AS m)
        LEFT JOIN team AS t
        ON n.hometeam_id = t.team_api_id
        WHERE season = '2011/2012'
        GROUP BY team) AS subquery
ORDER BY home_avg DESC
LIMIT 3;

- You can create multiple subqueries in one FROm statement (alias and join them)
- You can join a subquery to a table in FROM (include a joining columns in both tables)

SELECT
	-- Select country name and the count match IDs
    c.name AS country_name,
    COUNT(sub.id) AS matches
FROM country AS c
-- Inner join the subquery onto country
-- Select the country id and match id columns
INNER JOIN (SELECT country_id, id 
           FROM match
           -- Filter the subquery by matches with 10+ goals
           WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;

SELECT
	-- Select country, date, home, and away goals from the subquery
    country,
    date,
    home_goal,
    away_goal
FROM 
	-- Select country name, date, home_goal, away_goal, and total goals in the subquery
	(SELECT c.name AS country, 
     	    m.date, 
     		m.home_goal, 
     		m.away_goal,
           (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN country AS c
    ON m.country_id = c.id) AS subq
-- Filter by total goals scored in the main query
WHERE total_goals >= 10;

- SUbqueries in SELECT
SELECT COUNT(id) FROM match;

SELECT season, COUNT(id) AS matches, 12837 AS total_matches
FROM match
GROUP BY season;

- Subquery
SELECT season, COUNT(id) AS matches, (SELECT COUNT(id) FROM match) AS total_matches
FROM match
GROUP BY season;

Things to remember SELECT subqueries:
- Need to return a SINGLE value -> will generate an error otherwise

SELECT 
	l.name AS league,
    -- Select and round the league's total goals
    ROUND(AVG(home_goal + m.away_goal), 2) AS avg_goals,
    -- Select & round the average total goals for the season
    (SELECT ROUND(AVG(home_goal + away_goal), 2) 
     FROM match
     WHERE season = '2013/2014') AS overall_avg
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Filter for the 2013/2014 season
WHERE m.season = '2013/2014'
GROUP BY league;

SELECT
	-- Select the league name and average goals scored
	l.name AS league,
	ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Subtract the overall average from the league average
	ROUND(AVG(m.home_goal + m.away_goal) -
		(SELECT AVG(home_goal + away_goal)
		 FROM match 
         WHERE season = '2013/2014'),2) AS diff
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Only include 2013/2014 results
WHERE season = '2013/2014'
GROUP BY l.name;

Best practices:
- Format your queries
- Annotate your queries
- Indent your queries
- Properly filter each subquery

SELECT 
	-- Select the stage and average goals for each stage
	m.stage,
    ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Select the average overall goals for the 2012/2013 season
    ROUND((SELECT AVG(home_goal + away_goal) 
           FROM match 
           WHERE season = '2012/2013'),2) AS overall
FROM match AS m
-- Filter for the 2012/2013 season
WHERE season = '2012/2013'
-- Group by stage
GROUP BY stage;

SELECT 
	-- Select the stage and average goals from the subquery
	s.stage,
	ROUND(s.avg_goals,2) AS avg_goals
FROM 
	-- Select the stage and average goals in 2012/2013
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match WHERE season = '2012/2013');

SELECT 
	-- Select the stage and average goals from s
	stage,
    ROUND(s.avg_goals,2) AS avg_goal,
    -- Select the overall average for 2012/2013
    (SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM 
	-- Select the stage and average goals in 2012/2013 from match
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match WHERE season = '2012/2013');


Correlated Queries, Nested Queries, and Common Table Expressions
-------------------------------------------------------------------
Correlated subquery:
- Uses values from the outer query to generate a result
- Re-run for every row generated in the final data set
- Uses for advanced joining, filtering, and evaluating data

SELECT 
	-- Select the stage and average goals from s
	stage,
    ROUND(s.avg_goals,2) AS avg_goal,
    -- Select the overall average for 2012/2013
    (SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM 
	-- Select the stage and average goals in 2012/2013 from match
	(SELECT
		 stage,
         AVG(home_goal + away_goal) AS avg_goals
	 FROM match
	 WHERE season = '2012/2013'
	 GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                    FROM match AS m
                    WHERE s.stage > m.stage);

Simple subquery  Correlated subquery
- Can be run independently from the main query  | Dependent on the main query to EXECUTE
- Evaluated once in the whole query             | Evaluated in loops (running time longer)

SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    date,
    main.home_goal, 
    main.away_goal
FROM match AS main
WHERE 
	-- Filter the main query by the subquery
	(home_goal + away_goal) > 
        (SELECT AVG((sub.home_goal + sub.away_goal) * 3)
         FROM match AS sub
         -- Join the main query to the subquery in WHERE
         WHERE main.country_id = sub.country_id);

SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    main.date,
    main.home_goal,
    main.away_goal
FROM match AS main
WHERE 
	-- Filter for matches with the highest number of goals scored
	(home_goal + away_goal) =
        (SELECT MAX(sub.home_goal + sub.away_goal)
         FROM match AS sub
         WHERE main.country_id = sub.country_id
               AND main.season = sub.season);

Nested subqueries:
- Subquery inside another subquery

SELECT 
    EXTRACT(MONTH FROM date) AS month, 
    SUM(m.home_goal + m.away_goal) AS total_goals, 
    SUM(m.home_goal + m.away_goal) - 
        (SELECT AVG(goals) // Outer query
         FROM (SELECT EXTRACT(MONTH FROM date) AS month,
                SUM(home_goal + away_goal) AS goals
                FROM match
                GROUP BY month)) AS avg_diff // Inner query
FROM match AS m
GROUP BY month;

SELECT 
    c.name AS country,
    (SELECT AVG(home_goal + away_goal)
    FROM match AS m
    WHERE m.country_id = c.id -- Correlates with main query
    AND id IN (SELECT id -- Begin inner subquery
                FROM match
                WHERE season = '2011/2012')) AS avg_goals
FROM country AS c
GROUP BY country;

SELECT
	-- Select the season and max goals scored in a match
	season,
    MAX(home_goal + away_goal) AS max_goals,
    -- Select the overall max goals scored in a match
   (SELECT MAX(home_goal + away_goal) FROM match) AS overall_max_goals,
   -- Select the max number of goals scored in any match in July
   (SELECT MAX(home_goal + away_goal) 
    FROM match
    WHERE id IN (
          SELECT id FROM match WHERE EXTRACT(MONTH FROM date) = 07)) AS july_max_goals
FROM match
GROUP BY season;

-- Select matches where a team scored 5+ goals
SELECT
	country_id,
    season,
	id
FROM match
WHERE home_goal >= 5 OR away_goal >= 5;

-- Count match ids
SELECT
    country_id,
    season,
    COUNT(subquery) AS matches
-- Set up and alias the subquery
FROM (
	SELECT
    	country_id,
    	season,
    	id
	FROM match
	WHERE home_goal >= 5 OR away_goal >= 5 )
    AS subquery
-- Group by country_id and season
GROUP BY country_id, season;

#Gabungan
SELECT
	c.name AS country,
    -- Calculate the average matches per season
	AVG(outer_s.matches) AS avg_seasonal_high_scores
FROM country AS c
-- Left join outer_s to country
LEFT JOIN (
  SELECT country_id, season,
         COUNT(id) AS matches
  FROM (
    SELECT country_id, season, id
	FROM match
	WHERE home_goal >= 5 OR away_goal >= 5) AS inner_s
  -- Close parentheses and alias the subquery
  GROUP BY country_id, season) AS outer_s
ON c.id = outer_s.country_id
GROUP BY country;

Common Table Expressions (CTEs)
-------------------------------
- Table declared before the main query

WITH cte AS (
    SELECT col1, col2
    FROM table
)

SELECT 
    AVG(col1) AS avg_col1
FROM cte;

SELECT 
    c.name AS country,
    COUNT(s.id) AS matches
FROM country AS c
INNER JOIN (
    SELECT country_id , id
    FROM match 
    WHERE (home_goal + away_goal) >= 10) AS s
ON c.id = s.country_id
GROUP BY country;

WITH s AS (
    SELECT country_id, id
    FROM match
    WHERE (home_goal + away_goal) >= 10
)

SELECT 
    c.name AS country,
    COUNT(s.id) AS matches
FROM country AS c
INNER JOIN s
ON c.id = s.country_id
GROUP BY country;

-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id, 
  		id
    FROM match
    WHERE (home_goal + away_goal) >= 10)
-- Select league and count of matches from the CTE
SELECT
    l.name AS league,
    COUNT(match_list.id) AS matches
FROM league AS l
-- Join the CTE to the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;

-- Set up your CTE
WITH match_list AS (
  -- Select the league, date, home, and away goals
    SELECT 
  		l.name AS league, 
     	m.date, 
  		m.home_goal, 
  		m.away_goal,
       (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN league as l ON m.country_id = l.id)
-- Select the league, date, home, and away goals from the CTE
SELECT league, date, home_goal, away_goal
FROM match_list
-- Filter by total goals
WHERE total_goals >= 10;

-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id,
  	   (home_goal + away_goal) AS goals
    FROM match
  	-- Create a list of match IDs to filter data in the CTE
    WHERE id IN (
       SELECT id
       FROM match
       WHERE season = '2013/2014' AND EXTRACT(MONTH FROM date) = 08))
-- Select the league name and average of goals in the CTE
SELECT 
	l.name,
    AVG(match_list.goals)
FROM league AS l
-- Join the CTE onto the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;

Deciding on techniques to use:
- Considerable overlap
- These technique not identical 

Joins:
- Combine 2+ tables
- Simple operations/aggregations

Correlated subqueries:
- Match subqueries & tables 
- Avoid limits of joins
- High processing time

Multiple/nested subqueries:
- Combine multiple subqueries

CTEs:
- Organize subqueries sequentially 
- Can reference each other

Different use cases:
- Joins: 2+ tables (What is the total sales per employee?)
- Correlated subqueries: Who does each employee report to in a company?
- Multiple/nested subqueries: What is the average deal size closed by each sales representative in the quarter?
- CTEs: How did the marketing, sales, growth, engineering teams perform on key metrics in the last quarter?

--Get team names with a subquery
SELECT
	m.date,
    -- Get the home and away team names
    home.hometeam,
    away.awayteam,
    m.home_goal,
    m.away_goal
FROM match AS m
-- Join the home subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS hometeam
  FROM match
  LEFT JOIN team
  ON match.hometeam_id = team.team_api_id) AS home
ON home.id = m.id
-- Join the away subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS awayteam
  FROM match
  LEFT JOIN team
  -- Get the away team ID in the subquery
  ON match.awayteam_id = team.team_api_id) AS away
ON away.id = m.id;

-- Get the team names with Correlated Subqueries
SELECT
    m.date,
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.hometeam_id) AS hometeam,
    -- Connect the team to the match table
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.awayteam_id) AS awayteam,
    -- Select home and away goals
     m.home_goal,
     m.away_goal
FROM match AS m;

-- Get the team names with a CTE
WITH home AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS hometeam, m.home_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.hometeam_id = t.team_api_id),
-- Declare and set up the away CTE
away AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS awayteam, m.away_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.awayteam_id = t.team_api_id)
-- Select date, home_goal, and away_goal
SELECT 
	home.date,
    home.hometeam,
    away.awayteam,
    home.home_goal,
    away.away_goal
-- Join away and home on the id column
FROM home
INNER JOIN away
ON home.id = away.id;


Window Functions
----------------
Working with aggregate values
- Requires you to use GROUP BY with all non-aggregate columns

Window functions:
- Perform calculation on an already generated result set (a window)
- Aggregate calculations 
    - Similar to subqueries in SELECT
    - Running totals, rankings, moving average

-- Subqueries version 
SELECT 
    date, 
    (home_goal + away_goal) AS total_goals,
    (SELECT AVG(home_goal + away_goal) 
     FROM match
     WHERE season = '2013/2014') AS overall_avg
FROM match
WHERE season = '2013/2014';

-- Window function version
SELECT 
    date, 
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER() AS overall_avg
FROM match
WHERE season = '2013/2014';


-- Generate a RANK
SELECT 
    date,
    (home_goal + away_goal) AS total_goals,
    RANK() OVER(ORDER BY home_goal + away_goal DESC) AS goals_rank // Skip next ranking if there is a tie
FROM match
WHERE season = '2013/2014';

Key difference:
- Processed after the main query except for ORDER BY

SELECT 
	-- Select the id, country name, season, home, and away goals
	m.id, 
    c.name AS country, 
    m.season,
	m.home_goal,
	m.away_goal,
    -- Use a window to include the aggregate average in each row
	AVG(m.home_goal + m.away_goal) OVER() AS overall_avg
FROM match AS m
LEFT JOIN country AS c ON m.country_id = c.id;

SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank each league according to the average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank  // DESC -> highest rank first
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;

OVER and PARTITION BY:
- Calculate separate values for different categories
- Calculate different calculations in the same column 

AVG(home_goal) OVER(PARTITION BY season)

- How many goals were scored in each match compared to the average goals scored in that season?
SELECT 
    date,
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER(PARTITION BY season) AS season_avg
FROM match;

PARTITION BY season, country_id
SELECT 
    c.name,
    m.season,
    (home_goal + away_goal) AS total_goals,
    AVG(home_goal + away_goal) OVER(PARTITION BY season, country_id) AS season_country_avg
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id;

RANK() -> ranking, skipping next ranking if there is a tie
DENSE_RANK() -> ranking, no skipping

SELECT
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
		 ELSE 'away' END AS warsaw_location,
    -- Calculate the average goals scored partitioned by season
    AVG(home_goal) OVER(PARTITION BY season) AS season_homeavg,
    AVG(away_goal) OVER(PARTITION BY season) AS season_awayavg
FROM match
-- Filter the data set for Legia Warszawa matches only
WHERE 
	hometeam_id = 8673
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;

SELECT 
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
	-- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(MONTH FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
            EXTRACT(MONTH FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;

-- Sliding window
- Perform calculations relative to the current row
- Calculate moving averages, cumulative sums, and running totals
- Can be partitioned by a column

ROWS BETWEEN <start> AND <end>

PRECEDING -> before the current row
FOLLOWING -> after the current row
UNBOUNDED PRECEDING -> from the first row
UNBOUNDED FOLLOWING -> to the last row
CURRENT ROW -> the current row

SELECT 
    date, 
    home_goal, 
    away_goal, 
    SUM(home_goal) OVER(ORDER BY date ROWS BETWEEN 
    UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total_home
FROM match
WHERE hometeam_id = 8673 AND season = '2013/2014';

SELECT 
    date, 
    home_goal, 
    away_goal, 
    SUM(home_goal) OVER(ORDER BY date ROWS BETWEEN 
    1 AND CURRENT ROW) AS running_total_home
FROM match
WHERE hometeam_id = 8673 AND season = '2013/2014';

SELECT 
	date,
	home_goal,
	away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date  
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
    AVG(home_goal) OVER(ORDER BY date
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM match
WHERE 
	hometeam_id = 9908 
	AND season = '2011/2012';

-- Kebalikan
SELECT 
	-- Select the date, home goal, and away goals
	date,
    home_goal,
    away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
    AVG(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE 
	awayteam_id = 9908 
    AND season = '2011/2012';

Setting up the home team CTE:
SELECT 
	m.id, 
    t.team_long_name,
    -- Identify matches as home/away wins or ties
	CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		WHEN m.home_goal < m.away_goal THEN 'MU Loss'
        ELSE 'Tie' END AS outcome
FROM match AS m
-- Left join team on the home team ID and team API id
LEFT JOIN team AS t 
ON m.hometeam_id = t.team_api_id
WHERE 
	-- Filter for 2014/2015 and Manchester United as the home team
	season = '2014/2015'
	AND t.team_long_name = 'Manchester United';

```sql
-- Set up the home team CTE
WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
-- Select team names, the date and goals
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal,
    m.away_goal
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
      AND (home.team_long_name = 'Manchester United' 
           OR away.team_long_name = 'Manchester United');

```sql
WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
		   WHEN m.home_goal < m.away_goal THEN 'MU Win' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)

-- Select columns and and rank the matches by goal difference
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal, m.away_goal,
    RANK() OVER(ORDER BY ABS(home_goal - away_goal) DESC) as match_rank
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
      AND ((home.team_long_name = 'Manchester United' AND home.outcome = 'MU Loss')
      OR (away.team_long_name = 'Manchester United' AND away.outcome = 'MU Loss'));
```
<br> 

## Leetcode

```sql
SELECT p.product_id, 
       IFNULL(ROUND(SUM(p.price * u.units) / SUM(u.units), 2), 0) AS average_price
FROM 
    Prices AS p
LEFT JOIN 
    UnitsSold AS u
ON 
    p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY 
    p.product_id;
```

```sql
-- Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).
# Write your MySQL query statement below
SELECT w1.id
FROM Weather AS w1
JOIN Weather AS w2
ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature
```

```sql
SELECT a.machine_id, 
       ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a, 
     Activity b
WHERE 
    a.machine_id = b.machine_id
AND 
    a.process_id = b.process_id
AND 
    a.activity_type = 'start'
AND 
    b.activity_type = 'end'
GROUP BY machine_id
```
<br>

## Addition
**ROW_NUMBER()** -> Memberi nomor urut unik ke setiap baris, meskipun nilai sama
**RANK()** -> Memberi peringkat dengan nilai yang sama memiliki peringkat yang sama, tetapi nomor berikutnya akan meloncat  
**DENSE_RANK()** -> Mirip RANK(), tetapi tanpa loncatan  
**NTILE(N)** -> Membagi data menjadi N kelompok yang sama besar

**LAG()** -> Mengambil nilai dari baris sebelumnya  
**LEAD()** -> Mengambil nilai dari baris berikutnya  
**FIRST_VALUE()** -> Mengambil nilai pertama dalam window  
**LAST_VALUE()** -> Mengambil nilai terakhir dalam window  

```sql
SELECT id_karyawan, nama, departemen, gaji,
       ROW_NUMBER() OVER (ORDER BY gaji DESC) AS row_num,
       RANK() OVER (ORDER BY gaji DESC) AS rank_num,
       DENSE_RANK() OVER (ORDER BY gaji DESC) AS dense_rank_num
FROM Karyawan;
```

