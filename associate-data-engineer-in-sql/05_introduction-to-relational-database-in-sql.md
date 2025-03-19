## [Introduction to Relational Databases in SQL](https://app.datacamp.com/learn/courses/introduction-to-relational-databases-in-sql)

-------------------------------------------------------------
Introduction to Relational Databases in SQL                 |
-------------------------------------------------------------

A relational database:
- real-life entities become tables -> professors, universities, companies
- reduced redundancy -> only one entry in companies for the bank "Credit Suisse"
- data integrity by relationships -> a professor can work at multiple universities and companis a company can employ multiple professors

--Have a look at the PostgreSQL database
SELECT table_schema, table_name
FROM information_schema.tables;

--Have a look at the columns of a certain table
SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name = 'pg_config';

-- Query the right table in information_schema to get columns
SELECT column_name, data_type 
FROM information_schema.columns
WHERE table_name = 'university_professors' AND table_schema = 'public';

ERD(Entitiy Relationship Diagram):
- Square boxes represent tables
- Circle represent relationships

--Create a new table
CREATE TABLE table_name (
    column_a data_type,
    column_b data_type,
    column_c data_type
);

CREATE TABLE weather (
    clouds text,
    temperature numeric,
    weather_station char(5)
);

--Add columns to a table
ALTER TABLE table_name
ADD COLUMN column_name data_type;

Update database structure:
- Only store DICTINCT data in the new tables

INSERT DICTINCT records INTO the new tables:

INSERT INTO organizations
SELECT DISTINCT organization, organization_sector
FROM university_professors;

The INSERT INTO statement:
INSERT INTO table_name(column_a, column_b)
VALUES ("Value_a", "Value_b");

CREATE TABLE affiliations (
    firstname text,
    lastname text,
    university_shortname text,
    function text,
    organization text
);

ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;

ALTER TABLE table_name
DROP COLUMN column_name;

--Delete a table
DROP TABLE table_name;

Better data quality with constraints:
Integrity constraints:
- Attribute constraints, e.g. data types on columns
- Key constraints, e.g. primary keys
- Referential integrity constraints, e.g. foreign keys

Why constraints?
- Constraints give the data structure
- Constraints help with consistency thus data quality
- Data quality is a business adavantage/ data science prerequisite
- Enforcing is difficult, but PostgreSQL helps

Dealing with data types (casting)
CREATE TABLE weather(
    temperature integer,
    wind_speed text
);

SELECT temperature * wind_speed AS wind_chill
FROM weather; -> error because wind_speed is text

--Cast the wind_speed column to integer
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill
FROM weather;

Working with data types:
- Enforced on columns (i.e. attributes)
- Define the so-called "domain" of a column
- Define what operations are allowed on the data
- Enforce consistent storage of values

The most common types:
- text: character strings of any length
- varchar (x): a maximum of x characters
- char (x): a fixed-length string with a maximum of x characters
- boolean: can only take three states, true, false, or null
- date, time, timestamp: date, time, both date and time
- numeric: arbitrary precision numbers
- integer: whole numbers

Specifying types upon table creation:
CREATE TABLE student (
    ssn integer,
    name varchar(64),
    dob date,
    average_grade numeric(3,2) -- e.g. 5.54
    tuition_paid boolean
);

ALTER TABLE students
ALTER COLUMN name TYPE varchar(128);

ALTER TABLE students
ALTER COLUMN average_grade TYPE integer 
USING ROUND(average_grade); -> round the average_grade

If you don't want to reserve too much space for a certain varchar column, you can truncate the values before converting its type.
For this, you can use the following syntax:

ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
USING SUBSTRING(column_name FROM 1 FOR x)

-- Convert the values in firstname to a max. of 16 characters
ALTER TABLE professors 
ALTER COLUMN firstname 
TYPE varchar(16)
USING SUBSTRING(firstname FROM 1 FOR 16)

The not-null and unique constraints:
The not-null constraint:
- Disallow NULL values in a certain column
- Must hold true for the current state
- Must hold true for any future state

What does NULL mean?
- unknown
- does not exist
- does not apply
...

CREATE TABLE students (
    ssn integer NOT NULL,
    lastname VARCHAR(64) NOT NULL,
    home_phone integer,
    office_phone integer
);

ALTER TABLE students
ALTER COLUMN home_phone
SET NOT NULL;

ALTER TABLE students
ALTER COLUMN ssn
DROP NOT NULL;

The unique constraint:
- Disallow duplicate values in a certain column
- Must hold true for the current state
- Must hold true for any future state

CREATE TABLE table_name (
    column_name UNIQUE
);

ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name);

Keys and superkeys:
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

-- Try out different combinations
SELECT COUNT(DISTINCT(firstname, lastname)) 
FROM professors;

Primary Keys:
- One primary key per database table, chosen from candidate keys
- Uniquely identifies records, e.g. for referencing in other tables
- Unique and not-null constraints both apply
- Primary keys are time-invariant: choose columns wisely!

CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
);

CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);

CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);

ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name);

Surrogate keys:
- Surrogate keys are artificial primary keys
- Primary keys should be built from as few columns as possible
- Primary keys should never change over time

Adding a surrogate key with serial data type:
ALTER TABLE cars 
ADD COLUMN id serial PRIMARY KEY;

INSERT INTO cars
VALUES ('Volkswagen', 'Blitz', 'black');

Another type of surrogate key:
ALTER TABLE table_name
ADD COLUMN column_c VARCHAR(256);

UPDATE table_name
SET column_c = CONCAT(column_a, column_b);

ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_c);

-- Count the number of distinct rows with columns make, model
SELECT COUNT(DISTINCT(make, model)) 
FROM cars;

-- Add the id column
ALTER TABLE cars
ADD COLUMN id varchar(128);

-- Update id with make + model
UPDATE cars
SET id = CONCAT(make, model);

-- Make id a primary key
ALTER TABLE cars
ADD CONSTRAINT id_pk PRIMARY KEY(id);

-- Have a look at the table
SELECT * FROM cars;

-- Create the table
CREATE TABLE students (
  last_name VARCHAR(128) NOT NULL,
  ssn INTEGER PRIMARY KEY,
  phone_no CHAR(12)
);

Foreign keys:
- A foreign key (FK) points to the primary key (PK) of another table
- Domain of FK must be equal to domain of PK
- Each value of FK must exist in PK of the other table (FK constraint or referential integrity constraint)
- FK are not actual keys, but constraints

CREATE TABLE manufacturers (
    name VARCHAR(255) PRIMARY KEY
);

INSERT INTO manufacturers
VALUES ('Volkswagen');

CREATE TABLE cars (
    model VARCHAR(128) PRIMARY KEY,
    manufacturer_name VARCHAR(255) REFERENCES manufacturers(name)
);

ALTER TABLE a
ADD CONSTRAINT a_key FOREIGN KEY (b_id) REFERENCES b(id);

-- Rename the university_shortname column
ALTER TABLE professors
RENAME COLUMN university_shortname TO university_id;

-- Add a foreign key on professors referencing universities
ALTER TABLE professors
ADD CONSTRAINT professors_fkey FOREIGN KEY (university_id) REFERENCES universities (id);

-- Select all professors working for universities in the city of Zurich
SELECT professors.lastname, universities.id, universities.university_city
FROM professors
INNER JOIN universities
ON professors.university_id = universities.id
WHERE universities.university_city = 'Zurich';

--------------------------------------------------------------
-- Add a professor_id column
ALTER TABLE affiliations
ADD COLUMN professor_id integer REFERENCES professors (id);

--------------------------------------------------------
-- Rename the organization column to organization_id    |
ALTER TABLE affiliations                                |
RENAME organization TO organization_id;                 |
--------------------------------------------------------

------------------------------------------------------------------------------------------------------------------
-- Add a foreign key on organization_id                                                                           |
ALTER TABLE affiliations                                                                                          |
ADD CONSTRAINT affiliations_organization_fkey FOREIGN KEY (organization_id) REFERENCES organizations (id);;       |
------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------------------------
-- Set professor_id to professors.id where firstname, lastname correspond to rows in professors             |
UPDATE affiliations                                                                                         |
SET professor_id = professors.id                                                                            |
FROM professors                                                                                             |
WHERE affiliations.firstname = professors.firstname AND affiliations.lastname = professors.lastname;        |
------------------------------------------------------------------------------------------------------------

Referential integrity:
- A records referencing another table must refer to an existing record in that table
- Specified between two tables
- Enforced through foreign keys

Referential integrity violations:
Referential integrity from table A to table B is violated if:
- if a record in table B that is referenced from a record in table A is deleted.
- if a record in table A referencing a non-existing record from table B is inserted.
- Foreign keys prevent such violations

---------------------------------------------------------------------------------------------------------------------------
Dealing with violations:                                                                                                  |
CREATE TABLE a (                                                                                                          |
    id integer PRIMARY KEY,                                                                                               |
    column_a VARCHAR(64),                                                                                                 |
    ...,                                                                                                                  |
    b_id integer REFERENCES b(id) ON DELETE NO ACTION -- records in A that reference a record in B cannot be deleted      |
);                                                                                                                        |
---------------------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------------------------------------
Dealing with violations:                                                                                            |
CREATE TABLE a (                                                                                                    |
    id integer PRIMARY KEY,                                                                                         |
    column_a VARCHAR(64),                                                                                           |
    ...,                                                                                                            |
    b_id integer REFERENCES b(id) ON DELETE CASCADE -- delete all records in A that reference the record in B       |
);                                                                                                                  |
--------------------------------------------------------------------------------------------------------------------

---------------------------------------------------------------------
ON DELETE                                                            |
- NO ACTION: Throw an error                                          |
- CASCADE: Delete all records in A that reference the record in B    |
- RESTRICT: Throw an error                                           |
- SET NULL: Set the foreign key to NULL                              |
- SET DEFAULT: Set the foreign key to its default value              |
---------------------------------------------------------------------

-------------------------------------------------------
-- Identify the correct constraint name                |
SELECT constraint_name, table_name, constraint_type    |
FROM information_schema.table_constraints              |
WHERE constraint_type = 'FOREIGN KEY';                 |
-------------------------------------------------------

---------------------------------------------------------
-- Identify the correct constraint name                  |
SELECT constraint_name, table_name, constraint_type      |
FROM information_schema.table_constraints                |
WHERE constraint_type = 'FOREIGN KEY';                   |
---------------------------------------------------------

--------------------------------------------------------
-- Drop the right foreign key constraint               |
ALTER TABLE affiliations                               |
DROP CONSTRAINT affiliations_organization_id_fkey;     |
--------------------------------------------------------

-------------------------------------------------------------------------------------------------------------------------------------
-- Add a new foreign key constraint from affiliations to organizations which cascades deletion                                       |
ALTER TABLE affiliations                                                                                                             |
ADD CONSTRAINT affiliations_organization_id_fkey FOREIGN KEY (organization_id) REFERENCES organizations (id) ON DELETE CASCADE;      |
-------------------------------------------------------------------------------------------------------------------------------------

----------------------------
-- Delete an organization   |
DELETE FROM organizations   |
WHERE id = 'CUREM';         |
----------------------------

-------------------------------------------------------------------
-- Check that no more affiliations with this organization exist    |
SELECT * FROM affiliations                                         |
WHERE organization_id = 'CUREM';                                   |
-------------------------------------------------------------------

------------------------------------------------------------
-- Count the total number of affiliations per university    |
SELECT COUNT(*), professors.university_id                   |
FROM affiliations                                           |
JOIN professors                                             |
ON affiliations.professor_id = professors.id                |
-- Group by the university ids of professors                |
GROUP BY professors.university_id                           |
ORDER BY COUNT DESC;                                        |
------------------------------------------------------------

In this last exercise, your task is to find the university city of the professor with the most affiliations in the sector "Media & communication".
For this,
you need to join all the tables,
group by some column,
and then use selection criteria to get only the rows in the correct sector.
-----------------------------------------------------------------------------
-- Filter the table and sort it                                             |
SELECT COUNT(*), organizations.organization_sector,                         |
professors.id, universities.university_city                                 |
FROM affiliations                                                           |
JOIN professors                                                             |
ON affiliations.professor_id = professors.id                                |
JOIN organizations                                                          |
ON affiliations.organization_id = organizations.id                          |
JOIN universities                                                           |
ON professors.university_id = universities.id                               |
WHERE organizations.organization_sector = 'Media & communication'           |
GROUP BY organizations.organization_sector,                                 |
professors.id, universities.university_city                                 |
ORDER BY COUNT DESC;                                                        |
-----------------------------------------------------------------------------