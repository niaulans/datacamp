## [Database Design](https://app.datacamp.com/learn/courses/database-design)

------------------------------
Database Design              |
------------------------------

How should we organize and manage data?
- Schemas: How should my data be logically organized?
- Normalization: Should my data have minimal dependency and redundancy?
- Views: What joins will be done most often?
- Access control: Should all users of the data have the same level access
- DBMS: How do i pick between al the SQL and noSQL options?

Its depends on the intended use of the data.

Approaches to processing data

OLTP(Online Transaction Processing)
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

OLAP (Online Analytical Processing)
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

Storing data beyond traditional databases
- Traditional databases
    - For storing real-time relational structured data? OLTP
- Data warehouses
    - For analyzing archived structured data? OLAP
- Data lakes
    - For storing data of all structures = flexibility and Scalability
    - For analyzing big data

Data warehouses
- Optimized for analytics - OLAP
  - Organized for reading/aggregating data
  - usualy read-only
- Contains data from multiple sources
- Massively Parallel Processing (MPP)
- Typically uses a denormalized schema and dimensional modeling

Amazon Redshift
Azure SQL Data Warehouse
Google Big Query

Data marts
- Subset of data warehouses
- Dedicated to a spesific topic

Data lakes
- Store all types of data at a lower cost
    - raw, operational databases, IoT device logs, real-time, relational and non-relational
- Retains all data and can take up petabytes
- Schema-on-read as opposed to schema-on-write
- Need to catalog data otherwise becomes a data swamp
- Run big data analytics using services such as Apache Spark and Hadoop
 (useful for deep learning and data discovery because activities require so much data)

ETL
- Extract, Transform, Load
- more traditional approach for warehousing and smaller-scale analytics
Data sources (OLTP, APIs, Files, IoT Logs) - Staging - Data warehouse - Data marts, BI tools, Machine learning
                                          Extract, Transform, Load

ELT
- Extract, Load, Transform
- more common with big data project
Data sources (OLTP, APIs, Files, IoT Logs) - Data lake - Data warehouse - Data marts, BI tools, Machine learning
                                          Extract, Load, Transform

Database design 
- Determines how data is logically stored
   - How is data going to be read and updates?
- Uses database models: high-level specifications for database structure
  - Most popular: relational model
  - Other: NoSQL, objct-oriented, graph, network model
- Uses schemas: blueprint of the database
  - defines tables, fields, relationships, indexes, and views
  - When inserting data in relational databases, schemas must be respected

Data modeling
- Process of creating a data model for the data to be stored

1. Conceptual data model: describes entities, relationships, and attributes
   Tools: data structure diagrams (entity relationship diagrams), UML
2. Logical data model: describes tables, columns, and relationships
   Tools: database models and schemas (relational mode, star schema)
3. Physical data model: describes physical storage of data
   Tools: partitions, CPUs, indexes, backup system and tablespaces

Dimensional modeling 
= adaptation of the relational model for data warehouse design 
- optimized for OLAP queries: aggregate data, not updating (OLTP)
- built using the star schema
- easy to understand and query

Elements of dimensional modeling
1. Fact tables
    - decided by business use-case
    - holds records of a metric
    - changes regularly
    - connects to dimensions via foregin keys
2. Dimension tables
    - holds descriptive information
    - rarely changes
    - connects to fact tables via foregin keys

Star schema
Dimensional modeling: star schema

Fact tables:
- Holds records of a metric
- Changes regularly
- Connect to dimensions via foreign keys
- Example: - supply books to stores in USA and Canada
           - keep track of book sales

Dimensions tables:
- Holds decriptions of attributes
- Does not change as often

Snowflake schema (an extension)

Star schema: one dimension
Snowflake schema: more than one dimension

Because dimension tables are normalized

Normalization:
- Database design technique
- Divides tables into smaller tables and connects them via relationships
- Goal: reduce redundancy and increase data integrity

Identify repeating groups of data anc create new tables for them

Book dimension of the star schema
dim_book_star -: book_id, title, author, publisher, genre

Most likely to have repeating values:
- author
- publisher
- genre

divides tables:
dim_book_sf: book_id, title, author_id, publisher_id, genre_id
dim_genre_sf: genre_id, genre
dim_publisher_sf: publisher_id, publisher
dim_author_sf: author_id, author

-- Create dim_author with an author column
CREATE TABLE dim_author_sf (
    author VARCHAR(256)  NOT NULL
);

-- Insert distinct authors 
INSERT INTO dim_author_sf
SELECT DISTINCT author FROM dim_book_star;

-- Add a primary key 
ALTER TABLE dim_author ADD COLUMN author_id SERIAL PRIMARY KEY;

-- Output the new table
SELECT * FROM dim_author_sf;

Normalized and denormalized databases

Denormalized query:
get quantity of all Octavia E.Butler books sold in Vancouver in Q4 of 2018

SELECT SUM(quantity) FROM fact_booksales
INNER JOIN dim_store_star on fact_booksales.store_id = dim_store_star.store_id
INNER JOIN dim_book_star on fact_booksales.book_id = dim_book_star.book_id
INNER JOIN dim_time_star on fact_booksales.time_id = dim_time_star.time_id
WHERE dim_store_star.city = 'Vancouver' AND dim_book_star.author = 'Octavia E. Butler' AND dim_time_star.quarter = 4 AND dim_time_star.year = 2018;

Normalized query:
get quantity of all Octavia E.Butler books sold in Vancouver in Q4 of 2018

SELECT SUM(fact_booksales.quantity)
FROM fact_booksales
INNER JOIN dim_store_sf ON fact_booksales.store_id = dim_store_sf.store_id
INNER JOIN dim_book_sf ON fact_booksales.book_id = dim_book_sf.book_id
INNER JOIN dim_time_sf ON fact_booksales.time_id = dim_time_sf.time_id
INNER JOIN dim_author_sf ON dim_book_sf.author_id = dim_author_sf.author_id
..........
WHERE dim_city_sf.city = 'Vancouver' AND dim_author_sf.author = 'Octavia E. Butler' AND dim_time_sf.quarter = 4 AND dim_time_sf.year = 2018;

Normalization saves space
Denormalized databases enable data redundancy
Normalization eliminates data redundancy

Normalization ensures better data integrity
- Enforces data consistency
- Safer updating, removing, and inserting (less data redundancy = less records to alter)
- Easier to redesign by extending (smaller tables are easier to extend than larger tablea)

Database Normalization
Advantages:
- Eliminates data redundancy: save on storage
- Better daya integrity: accurate and consistent data

Disadvantages:
- Complex queries require more CPU

OLTP -> operational databases
- Typically highly normalized
- Write-intensive
- Prioritize quicker and safer insertion of data

OLAP -> data warehouses
- Typically denormalized
- Read-intensive
- Prioritize quicker queries for analytics


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

-- Add a continent_id column with default value of 1
ALTER TABLE dim_country_sf
ADD COLUMN continent_id int NOT NULL DEFAULT(1);

-- Add the foreign key constraint
ALTER TABLE dim_country_sf ADD CONSTRAINT country_continent
   FOREIGN KEY (continent_id) REFERENCES dim_continent_sf(continent_id);
   
-- Output updated table
SELECT * FROM dim_country_sf;

Normal forms
Normalization: identify repeating groups of data and create new tables for them

A more formal definition:
- Be able to characterize the level of redundancy in a relational schema
- Provide mechanism for transforming schemas in order to remove redundancy

Normal forms (NF):
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

1NF rules:
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

2NF rules:
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

3NF rules:
- satisfies 2NF AND
    - no transitive dependencies
    - non key columns can't depend on other non-key columns

course_id (PK) - instructor_id - instructor_name - tech
2001 - 560 - Nick Carchedi - python

course_id (PK) - instructor_id - tech
2001 - 560 - python

instructor_id - instructor_name
560 - Nick Carchedi

Data anomalies
What is risked if we don't normalize enough?

1. Update anomaly
2. Insert anomaly
3. Delete anomaly

update anomaly: data inconsistency cause by data redundancy when updating
insertion anomaly: unable to add data because of missing data
delete anomaly: unintentional loss of data when deleting

The mre normalized the database, the less likely these anomalies are to occur

-- Create a new table to satisfy 2NF
CREATE TABLE cars (
  car_id VARCHAR(256) NULL,
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128),
  condition VARCHAR(128),
  color VARCHAR(128)
);

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

-- Drop columns in customer_rentals to satisfy 2NF
ALTER TABLE customer_rentals
DROP COLUMN model,
DROP COLUMN manufacturer,
DROP COLUMN type_car, 
DROP COLUMN condition ,
DROP COLUMN color;

-- Create a new table to satisfy 3NF
CREATE TABLE car_model(
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128)
);

-- Drop columns in rental_cars to satisfy 3NF
ALTER TABLE rental_cars
DROP COLUMN model,
DROP COLUMN manufacturer;

Database views 
------------------------------
A view -> the result set of a stored query on the data, which the database users can query just as they would in a persistent database collection object
Virtual table that is not oart of the physical schema

- Query, not data is stored in memory
- Data is aggregated from data in tables
- Can be queried like a regular database table
- No need to retype common queries or alter schemas

CREATE VIEW view_name AS
SELECT col1, col2
FROM table_name
WHERE condition;

Viewing views:
Include system views:
SELECT * FROM INFORMATION_SCHEMA.views;

Exclude system views:
SELECT * FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');

Benefits of views:
- Doesnt take up storage
- A form of access control 
- Masks complexity of queries

-- Create a view for reviews with a score above 9
CREATE VIEW high_scores AS
SELECT * FROM REVIEWS
WHERE score > 9;

-- Count the number of self-released works in high_scores
SELECT COUNT(*) FROM high_scores
INNER JOIN labels ON high_scores.reviewid = labels.reviewid
WHERE label = 'self-released';

Managing views
Granting and revoking access to a view:

GRANT privilege(s) or REVOKE privilege(s)
ON object
TO role or FROM role

Privileges: SELECT, INSERT, UPDATE, DELETE
Objects: table, view, schema, etc
Roles: a database user or a group of database users

GRANT UPDATE ON ratings TO PUBLIC;
REVOKE INSERT ON films FROM db_user;

Updating a view:
UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama';

- Note all views are updatable
- View is made up of one table
- Doesn't use a window or aggregate function

Inserting into a view:
INSERT INTO view_name
VALUES (value1, value2, ...);

- Not all views are insertable
- Avoid modifying data through views

Dropping a view:
DROP VIEW view_name [CASCADE | RESTRICT];

Restrict -> only drop the view if no other objects depend on it
Cascade -> drop the view and all objects that depend on it

Redefining a view:
CREATE OR REPLACE VIEW view_name AS new_query;

- if a view with view_name exists, it will be replaced
- new_query must have the same number of columns as the original view
- the column output may be different
- new column may be added at the end

Altering a view:
ALTER VIEW [IF EXISTS] view_name ALTER [COLUMN] column_name SET DATA TYPE new_data_type;
ALTER VIEW [ IF EXISTS] view_name RENAME COLUMN column_name TO new_column_name;

-- Revoke everyone's update and insert privileges
REVOKE UPDATE, INSERT ON long_reviews FROM PUBLIC; 

-- Grant the editor update and insert privileges 
GRANT UPDATE, INSERT ON long_reviews TO editor; 

-- Redefine the artist_title view to have a label column
CREATE OR REPLACE VIEW artist_title AS
SELECT reviews.reviewid, reviews.title, artists.artist, labels.label
FROM reviews
INNER JOIN artists
ON artists.reviewid = reviews.reviewid
INNER JOIN labels
ON labels.reviewid = artists.reviewid;

SELECT * FROM artist_title;

Materialized views
Two types of views:
Views: 
- also known as non materialized views
- how we've defined views so far

Materialized views:
- physically views stored in memory
- stores the query result not the query
- querying a materialized view means accessing the stored result
- refreshed or rematerialized to update the data when prompted

when to use materialized views:
- long running queries
- underlying query result dont change often
- data warehouse because OLAP is not write-intensive

CREATE MATERIALIZED VIEW view_name AS SELECT * FROM table_name;

REFRESH MATERIALIZED VIEW view_name;

Database roles and access control

Database roles:
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

Create a role:
- Empty role
CREATE ROLE role_name;

- Role with some attributes set
CREATE ROLE intern WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2020-01-01';

CREATE ROLE admin CREATEDB;
ALTER ROLE admin CREATEROLE;

GRANT UPDATE ON ratings TO data_analyst;
REVOKE UPDATE ON ratings FROM data_analyst;

The available privileges in PostgreSQL are:
- SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, CONNECT, TEMPORARY, EXECUTE, and USAGE

Users and groups (are both roles):
- A role is an entity that can function as a user and/or a group
    - User roles
    - Group roles

CREATE ROLE data_analyst;
CREATE ROLE alex WITH PASSWORD 'PasswordForAlex' VALID UNTIL '2020-01-01';
GRANT data_analyst TO alex;

-- Create an admin role
CREATE ROLE admin WITH CREATEDB CREATEROLE;

-- Grant data_scientist update and insert privileges
GRANT UPDATE, INSERT ON long_reviews TO data_scientist;

-- Give Marta's role a password
ALTER ROLE Marta WITH PASSWORD 's3cur3p@ssw0rd';

-- Add Marta to the data scientist group
GRANT data_scientist TO Marta;

-- Celebrate! You hired data scientists.

-- Remove Marta from the data scientist group
REVOKE data_scientist FROM Marta;

Table partitioning
Why partition?
Tables grow (100gb / 1tb / 10tb)

Problem: queries/update become slower 
Because: e.g. indices don't fit memory

Solution: split table into smaller tables (partitions)

Data modeling refresher
1. Conceptual data model
2. Logical data model
    For partitioning, logical data model is the same
3. Physical data model
    Partitioning is part of the physical data model

Vertical partitioning:
Split table even when fully normalized
e.g. store long_description on slower medium

horizontal partitioning:
Split table over the rows
e.g. partition the table by the timestamp

CREATE TABLE sales (
    ....
    timestamp DATE NOT NULL
)
PARTITION BY RANGE (timestamp);

CREATE TABLE sales_2019_q1 PARTITION OF sales
FOR VALUES FROM ('2019-01-01') TO ('2019-04-01');

CREATE INDEX ON sales('timestamp');

Pros:
- Indices of heavily-used partitions fit in memory
- Move to specific medium: slower vs faster
- Used for both OLTP and OLAP

Cons:
- Partitioninng existing table can be a hassle
- Some constraints can not be set

-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);

-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;

-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);

-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;
    
-- Drop the descriptions from the original table
ALTER TABLE film
DROP COLUMN long_description;

-- Join to view the original table
SELECT * FROM film_descriptions
JOIN film USING(film_id);

-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY RANGE (release_year);

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

-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);

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

-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');
    
CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');
    
CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');

-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);

-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');

CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');

CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');

-- Insert the data into film_partitioned
INSERT INTO film_partitioned
SELECT film_id, title, release_year FROM film;

-- View film_partitioned
SELECT * FROM film_partitioned;

Data integration
Combines data from different sources, formats, technologies to provide users with a translated and unified view of that data


Business cas examples:
- 360 degree customer
- acquisition
- legacy Systems

Data source 1 (postgresql) ----
Data source 2 (mongodb)  ----- Unified data model (Redshift)
Data source 3 (csv)  ------

Choosing a data integration toola:
flexible
reliable
scalable

different DBMS types:

SQL: RDBMS
NoSQL: key-value store, document store, columnar database, graph database