## [Introduction to SQL](https://app.datacamp.com/learn/courses/introduction-to-sql)

-------------------------------------------------------------
INTRODUCTION TO SQL                                         |
-------------------------------------------------------------

Database advantages:
- More storage than spreadsheet applications
- Storage is more secure

SQL (Structured Query Language)
- The most widely used programming language for databases

Tables:
- organized into rows and columns
- rows are records, and columns are fields

Good table naming:
- be lowercase
- have no spaces - use underscores instead
- refer to a collective group or be plural

a record is a row that holds data on an individual observation
a field is a column that holds one piece of information about all records

fields names should:
- be lowercase
- have no spaces
- be singular
- be different from other field names
- be different from the table name

Assigned seats:
- unique identifiers are used to identify records in a table
- they are unique and often numbers

SQL data types:
- Different types of data are stored differently and take up different space
- Some operations only apply to certain data types

- A string is a sequence of characters such as letters or punctutation 
- VARCHAR is a flexible and popular string data type in SQL

- Integers store whole numbers
- INT is a flexible and popular

- Float store numbers thet include a fractional part
- NUMERIC is a flexible and popular Float data type in SQL

- Schemas are often referred to as "blueprints" of databases
- A schema show a database's design such as table, relationships, and constraints

Database storage:
- Servers are centralized computers that perform services via requests made over a network

- Use SQL queries to uncover trends in website traffic, customer reviews, and produc sales

Keywords:
- Keywords are reserved words for operations
- Common keywords: SELECT, FROM

---------------------------------------------
#select all names from the patrons table    |
SELECT name                                 |
FROM patrons;                               |
---------------------------------------------

- Query results often called result set

--------------------------------------------------
#select card_num and name from the patrons table  |
#multiple select                                  |
SELECT card_num, name                             |
FROM  patrons;                                    |
--------------------------------------------------

---------------------------------------------
#select all columns from the patrons table  |
#select all                                 |
SELECT *                                    |
FROM patrons;                               |
---------------------------------------------

Aliasing -> use aliasing to rename columns
--------------------------------------------------------------------
#select name as first_name and year_hired from the employees table  |
SELECT name AS first_name, year_hired                               |
FROM employees;                                                     |
--------------------------------------------------------------------

Selecting distinct records:
-----------------------------------------------------
#select distinct year_hired from the employees table |
SELECT DISTINCT year_hired                           |
FROM employees;                                      |
-----------------------------------------------------

VIEWS:
- a view is a virtual table that is the result of a saved SQL SELECT statement
- When accesed, views automatically update in response to updates in the underlying data

----------------------------------------------------------------------------
#buat view dengan nama employee_hire_years yang isinya id, name, year_hired |
CREATE VIEW employee_hire_years AS                                          |
SELECT id, name, year_hired                                                 |
FROM employees;                                                             |
----------------------------------------------------------------------------

----------------------------------------
CREATE VIEW library_authors AS          |
SELECT DISTINCT author AS unique_author |
FROM books;                             |
----------------------------------------

SQL flavors:
- Both free and paid
- All used with relational databases
- Vast majority of keywords are the same
- All must follow universal standards
- Only the additions on top of there standards make flavors different

PostgreSQL:
- Free and open-source relational database system
- Created at university of california, berkeley
- Research and fund by DARPA 
- "PostgreSQL" referens to both the postgresql databse system and its associated SQL flavor

SQL server:
- Has free and paid versions
- Created by microsoft
- T-SQL is microsoft SQL flavor, used with SQL Server databases

PostgreSQL vs. SQL Server
PostgreSQL:
-----------------
SELECT id, name  |
FROM employees   |
LIMIT 2;         |
-----------------

SQL Server:
-----------------------
SELECT TOP(2) id, name |
FROM employees;        |
-----------------------