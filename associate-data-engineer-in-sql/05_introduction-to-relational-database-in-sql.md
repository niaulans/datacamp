## [Introduction to Relational Databases in SQL](https://app.datacamp.com/learn/courses/introduction-to-relational-databases-in-sql)


### Relational database
```
- Real-life entities become tables  -> professors, universities, companies
- Reduced redundancy                -> only one entry in companies for the bank "Credit Suisse"
- Data integrity by relationships   -> a professor can work at multiple universities and companis a company can employ multiple professors
```

### Have a look at the PostgreSQL database
```sql
SELECT table_schema, table_name
FROM information_schema.tables;
```

### Have a look at the columns of a certain table
```sql
SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name = 'pg_config';

SELECT column_name, data_type 
FROM information_schema.columns
WHERE table_name = 'university_professors' AND table_schema = 'public';
```

### ERD(Entitiy Relationship Diagram)
```
- Square boxes represent tables
- Circle represent relationships
```

### Create a new table
```sql
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

CREATE TABLE affiliations (
    firstname text,
    lastname text,
    university_shortname text,
    function text,
    organization text
);
```

### Add columns to a table
```sql
ALTER TABLE table_name
ADD COLUMN column_name data_type;
```

### Insert DISTINCT data from one table into another
```sql
INSERT INTO organizations
SELECT DISTINCT organization, organization_sector
FROM university_professors;
```

### Insert data into a table
```sql
INSERT INTO table_name(column_a, column_b)
VALUES ("Value_a", "Value_b");
```

### Rename a table
```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

### Delete a column
```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

### Delete a table
```sql
DROP TABLE table_name;
```

### Better data quality with constraints
```
Integrity constraints:
- Attribute constraints, e.g. data types on columns
- Key constraints, e.g. primary keys
- Referential integrity constraints, e.g. foreign keys

Why constraints?
- Constraints give the data structure
- Constraints help with consistency thus data quality
- Data quality is a business adavantage/ data science prerequisite
- Enforcing is difficult, but PostgreSQL helps
```

### Dealing with data types (casting)
```sql
CREATE TABLE weather(
    temperature integer,
    wind_speed text
);

SELECT temperature * wind_speed AS wind_chill
FROM weather; -- error because wind_speed is text

-- Cast the wind_speed column to integer
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill
FROM weather;
```

### Working with data types
```
- Enforced on columns (i.e. attributes)
- Define the so-called "domain" of a column
- Define what operations are allowed on the data
- Enforce consistent storage of values
```

### The most common data types
Data type | Description
--- | ---
text | character strings of any length
varchar (x) | a maximum of x characters
char (x) | a fixed-length string with a maximum of x characters
boolean | can only take three states, true, false, or null
date, time, timestamp | date, time, both date and time
numeric | arbitrary precision numbers
integer | whole numbers


### Specifying types upon table creation
```sql
CREATE TABLE student (
    ssn integer,
    name varchar(64),
    dob date,
    average_grade numeric(3,2) -- e.g. 5.54
    tuition_paid boolean
);
```

### Altering the data type of a column
```sql
ALTER TABLE students
ALTER COLUMN name TYPE varchar(128);
```

### Altering the data type of a column and rounding
```sql
ALTER TABLE students
ALTER COLUMN average_grade TYPE integer 
USING ROUND(average_grade); -- round the average_grade
```
### REMEMBER!!!
```
If you don't want to reserve too much space for a certain varchar column, you can truncate the values before converting its type.
```

### Restrict the values in a column
```sql
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
USING SUBSTRING(column_name FROM 1 FOR x)
```

### Convert the values in firstname to a max. of 16 characters
```sql
ALTER TABLE professors 
ALTER COLUMN firstname 
TYPE varchar(16)
USING SUBSTRING(firstname FROM 1 FOR 16)
```

### The not-null and unique constraints
```
The not-null constraint:
- Disallow NULL values in a certain column
- Must hold true for the current state
- Must hold true for any future state

What does NULL mean?
- unknown
- does not exist
- does not apply
...
```

### Create table with not-null constraint
```sql
CREATE TABLE students (
    ssn integer NOT NULL,
    lastname VARCHAR(64) NOT NULL,
    home_phone integer,
    office_phone integer
);

-- Add a not-null constraint
ALTER TABLE students
ALTER COLUMN home_phone
SET NOT NULL;

-- Drop a not-null constraint
ALTER TABLE students
ALTER COLUMN ssn
DROP NOT NULL;
```

### The unique constraint
```
- Disallow duplicate values in a certain column
- Must hold true for the current state
- Must hold true for any future state
```

### Create table with unique constraint
```sql
CREATE TABLE table_name (
    column_name UNIQUE
);

-- Add a unique constraint
ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name);
```

### Keys and superkeys
```
What is a key?
- Attribute(s) that identify a record uniquely
- As long as attributes can be removed: superkey
- If no more attributes can be removed: minimal superkey or key
```

```
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
```

### Primary keys
```
- One primary key per database table, chosen from candidate keys
- Uniquely identifies records, e.g. for referencing in other tables
- Unique and not-null constraints both apply
- Primary keys are time-invariant: choose columns wisely!
```

### Create a table with a unique key
```sql
CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
);
```

### Create a table with a primary key
```sql
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);
```

### Create a table with a composite primary key
```sql
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```

### Add a primary key
```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name);
```

### Surrogate keys
```
- Surrogate keys are artificial primary keys
- Primary keys should be built from as few columns as possible
- Primary keys should never change over time
```

### Adding a surrogate key with serial data type
```sql
ALTER TABLE cars 
ADD COLUMN id serial PRIMARY KEY;

-- Insert data into table
INSERT INTO cars
VALUES ('Volkswagen', 'Blitz', 'black');

-- Look the surrogate key candidates
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
```

### Foreign keys
```
- A foreign key (FK) points to the primary key (PK) of another table
- Domain of FK must be equal to domain of PK
- Each value of FK must exist in PK of the other table (FK constraint or referential integrity constraint)
- FK are not actual keys, but constraints
```
### Add a foreign key
```sql
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
```

### Referential integrity
```
- A records referencing another table must refer to an existing record in that table
- Specified between two tables
- Enforced through foreign keys
```

### Referential integrity violations
```
Referential integrity from table A to table B is violated if:
- if a record in table B that is referenced from a record in table A is deleted.
- if a record in table A referencing a non-existing record from table B is inserted.
- Foreign keys prevent such violations
```

### Dealing with violations
```sql 
CREATE TABLE a (           
    id integer PRIMARY KEY,
    column_a VARCHAR(64),  
    ...,                   
    b_id integer REFERENCES b(id) ON DELETE NO ACTION -- records in A that reference a record in B cannot be deleted  
);
```                

```sql
CREATE TABLE a (     
    id integer PRIMAR
    column_a VARCHAR(
    ...,             
    b_id integer REFERENCES b(id) ON DELETE CASCADE -- delete all records in A that reference the record in B  
);                   
```

```
- CASCADE: Delete all records in A that refer
- SET DEFAULT: Set the foreign key to its default value          
```

### Identify the correct constraint name   
```sql             
SELECT constraint_name, table_name, constraint_type    
FROM information_schema.table_constraints              
WHERE constraint_type = 'FOREIGN KEY';                 
```

### Identify the correct constraint name   
```sql               
SELECT constraint_name, table_name, constraint_type      
FROM information_schema.table_constraints                
WHERE constraint_type = 'FOREIGN KEY';                   
```

### Drop the right
```sql
DROP CONSTRAINT affiliations_organization_id_fkey;  
```

### Add a new foreign key cons          
```sql
ALTER TABLE affiliations              
ADD CONSTRAINT affiliations_organization_id_fkey FOREIGN KEY (organization_id) REFERENCES organizations (id) ON DELETE CASC FROM organizations   
WHERE id = 'CUREM';         
```