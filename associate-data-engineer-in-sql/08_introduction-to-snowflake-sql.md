## [Introduction to Snowflake SQL](https://app.datacamp.com/learn/courses/introduction-to-snowflake-sql)


### Introduction to Snowflake: Architecture, Competitors, and SnowflakeSQL
```
What is Snowflake?
- Cloud data warehouse solution 
- Columnar data storage

Advantages:
- Scalability
- Accessibility
- Cost-efficiency
- Lower management effort
```

### Row vs. Columnar database
Category | Row | Columnar
-------------------|------------------------|------------------
Data organization  |  by row             | by column
Data retrieval     |  complete records   | specific columns
Operational        |  OLTP               | OLAP
Example            |  Postgre, MySQL, Oracle, SQL Server |  Snowflake, Amazon Redshift, Google BigQuery, Vertica

### Snowflake use cases
```
- Business Intelligence
- Data science
- Data Ingestion 
- Data warehousing
- Data sharing
```

### Snowflake Architecture
```
Shared-disk and shared-nothing architecture
- Shared-disk: 
    - Data stored on a central disk
    - All nodes can access the data
    - Data is distributed across all nodes

- Shared-nothing:
    - Data stored on each node
    - Each node has its own disk
    - Data is not shared between nodes
```

### Decoupling storage and compute -> separating storage and compute
```
- Efficient data storage
- Independent data processing
- Components operate without interdependence

Benefits of Decoupling:
- Enhanced scalability 
- faster data processing and response
- Cost-effective operations
```

### Snowflake architecture
```
infrastructure manager, optimizer, metadata manager, security -> cloud service layer
virtual warehouse -> compute layer 
data storage -> storage layer
```

#### Storage layer
```
- Columnar storage format
   - Efficient data retrieval
   - analysis
- Optimized
- Compressed 
- Consists of tables, schemas, and databases
```

#### Compute layer
```
- Query execution 
- Virtual warehouses
```

#### Cloud services layer
```
- Infrastructure management
- Query optimization 
- authentication
- access control 
- security
```

### Snowflake competitors
```
- Google BigQuery
- Amazon Redshift
- Databricks
- PostgreSQL
```

### Security & data support
```
Security:
- Access control
- Encryption 
```

### Semi structured data support
```
- JSON
- Avro
- Parquet
- CSV
```

### What makes Snowflake unique?
```
- Unique architecture - decoupling storage and compute
- Secure data sharing
- High performance
- Multi-cloud support

Why use Snowflake?
- Large-scale data warehousing
- Multi
- Pricing
- Ease of use
```

### Snowflake SQL
```sql
-- Count all pizza entries
SELECT COUNT(*) AS count_all_pizzas
FROM pizza_type
-- Apply filter on category for Classic pizza types
WHERE category = 'Classic'
-- Additional condition to filter where name has Cheese in it
AND name LIKE '%Cheese%';
```

### Snowflake SQL and key concepts
```
Connecting to Snowflake and DDL commands
Connecting to Snowflake:
- Snowsight: Snowflake web interface (worksheets)
- Drivers & connectors: ODBC (Open Database Connectivity) and JDBC (Java Databace Connectivity) Drivers, Python/Spark 
- SnowSQL: Command-line client, installed on linux, windows, or mac
```

#### Staging
```
- Temporary location storing data
- Internal stage
- External stage (cloud storage: amazon s3, google cloud storage)
    - Raw data sources: initial, unprocessed data, e.g. CSV files
    - Staging area: temporary storage, loading
    - Table: final data is loaded here
```

### Create stage
```sql
CREATE STAGE my_local_stage

PUT file:///path_to_your_local_file/orders.csv
@my_local_stage -- stage name prefixed with @

-- @ -> Prefix to reference stage
```

### DDL commands
```
CREATE, ALTER, DROP, RENAME, COMMENT
```

```sql
CREATE TABLE orders_pizza (
    order_id NUMBER,
    order_date DATE,
    time TIME
)
```

### Create view
```sql
CREATE VIEW orders_pizza_view AS
SELECT order_id, order_date
FROM orders_pizza;
```

### Rename table
```sql
ALTER TABLE IF EXISTS orders_pizza
RENAME TO orders;
```

### Rename column
```sql
ALTER TABLE orders
RENAME COLUMN time TO order_time;
```

### Drop table
```sql
DROP TABLE orders;
```

### Comment
```sql
CREATE TABLE pizza_type(
    pizza_type_id VARCHAR(50) COMMENT 'Unique identifier for pizza type',
    name VARCHAR(100),
    category VARCHAR(50),
    ingridents VARCHAR(500)
)
-- COMMENT = 'Table that stores information about different type of pizzas, including their names, categories, and ingridents';
```

### Postgresql
```sql
COMMENT ON [object type] [object name] IS 'comment';
COMMENT ON TABLE pizza_type IS 'Table with pizza type info';
```

### DML commands
```
SHOW, DESCRIBE, INSERT, UPDATE, MERGE, COPY
```

```sql
SHOW databases -- show available databases

SHOW TABLES IN { DATABASE [db_name]}
SHOW TABLES IN DATABASE pizza_sales -- show tables in database

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
```

### Insert data
```sql
INSERT INTO orders (order_id, order_date, order_time)
VALUES (1, '2015-01-01', '11:38:36')

INSERT INTO orders_filtered
SELECT * FROM orders
WHERE order_date > '2015-01-02'
```

### Update data
```sql
UPDATE orders
SET order_time = '17:00:00'
WHERE order_id = '5'
```

### Merge data
```sql
MERGE INTO orders_filtered AS target -- target table
USING orders AS source -- source table
ON target.order_id = source.order_id --Common column
WHEN MATCHED THEN -- When there is a match
    UPDATE SET -- Update order_date and time of target table
    target.order_date = source.order_date
    target.time = source.order_time
```

### Copy data
```sql
Load data from staging to table:
COPY INTO pizza_type FROM @my_local_stage/orders.csv
FILE_FORMAT = (TYPE = 'CSV' SKIP_HEADER=1)
```

### Snowflake data type and data conversion
```
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
```

### Data type conversion
```sql
CAST ((source_data) AS (target_data_type))
CAST ('80' AS INT)

-- using ::
((source_data) :: (target_data_type))
'80'::INT
```

### Cast and convert data types
```sql
SELECT CAST(order_date AS timestamp) AS order_timestamp
FROM orders

SELECT TO_DATE('2023-08-16 11:51:00')
```

```sql
-- Convert request_id to VARCHAR aliasing to request_id_string
SELECT CAST(request_id AS VARCHAR) AS request_id_string
FROM uber_request_data
```

```sql
-- Convert request_id to VARCHAR using CAST method and alias to request_id_string
SELECT CAST(request_id AS VARCHAR) AS request_id_string, 
-- Convert request_timestamp to DATE using TO_DATE and alias as request_date
	   TO_DATE(request_timestamp) AS request_date
FROM uber_request_data
```

```sql
-- Convert request_id to VARCHAR using CAST method and alias to request_id_string
SELECT CAST(request_id AS VARCHAR) AS request_id_string, 
-- Convert request_timestamp to DATE using TO_DATE and alias as request_date
	   TO_DATE(request_timestamp) AS request_date,
-- Convert drop_timestamp column to TIME using :: operator and alias to drop_time
       drop_timestamp::TIME AS drop_time
FROM uber_request_data
-- Filter the records where request_date is greater than '2016-06-01' and drop_time is less than 6 AM.
WHERE request_date > '2016-06-01' AND drop_time < '06:00:00'
```

### Functions, sorting, and grouping
```
- aggregate
AVG(), SUM(), MIN(), MAX(), COUNT()

- string
CONCAT(), LENGTH(), LOWER(), UPPER(), TRIM(), SUBSTRING(), REPLACE(), POSITION(), SPLIT_PART(), REVERSE()

- date and time
CURRENT_DATE(), CURRENT_TIME()
EXTRACT() -> EXTRACT(YEAR FROM drop_timestamp) AS YEAR

GROUP BY ALL -> group column not separated
```

### Advance Snowflake SQL Concepts
```
Joining on Snowflake:
- INNER Joins
- OUTER Joins
- CROSS Joins
- SELF Joins
- NATURAL Joins
- LATERAL Joins
```


### Natural join
```
automatically mathing same name column
```

```sql
SELECT * FROM pizzas AS p
NATURAL JOIN pizza_type AS t
WHERE pizza_type_id = 'bbq'
```

### LATERAL
```
subquery
```

```sql
SELECT p.pizza_id, lat.name, lat.category
FROM pizzas AS p
```

```sql
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
```

### Subquerying and common table expressions
```sql
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
```

```sql
WITH max_price AS (
    SELECT pizza_type_id, MAX(price) AS max_price
    FROM pizzas
    GROUP BY pizza_type_id
)

SELECT pt.name, pz.price, pt.category
FROM pizzas AS pz
JOIN pizza_type AS pt pz.pizza_type_id = pt.pizza_type_id
JOIN max_price AS mp 
ON pt.pizza_type_id = mp.pizzape_id
WHERE pz.price < mp.max_price
```

### Why use CTE?
```
- Managing complex operations
- Readable 
- Modular
- Reusable
```

### Snowflake query optimization
```sql
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
```

### Multiple CTEs
```sql
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
```

### Snowflake query optimization
```
- Transforming into more efficient queries
- Snowflake Cloud Service Layer

Why optimize queries in Snowflake?
- Achieve faster results
- Cost efficiency -> shorter query consumes fewer resources like CPU and memory
```

### Common query problems
```
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
```

```sql
SELECT TOP 10*
FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF100.CUSTOMER
```

```
- Filter Early
    - Use WHERE clause early on 
    - Apply filters before JOINs

Without early filtering:
SELECT 
FROM 
JOIN
ON 
JOIN
ON 
WHERE
```

### With early filtering
```sql
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
```

### Query history
```sql
SELECT 
    query_text,
    start_time,
    end_time,
    execution_time
FROM snowflake.account_usage.query_history
WHERE execution_time > 1000
```

```sql
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
```

### Handling semi-structured data (JSON)
```
JSON:
- Javascript Object Notion 
- Common use: Web APIs, Mobile Apps, Config files
- JSON data structure -> key-value pairs

{
    "name": "John",
    "age": 30,
    "city": "New York"
}
```

### JSON in Snowflake
```
- Native JSON support
- Flexible for evolving schemas

Comparisons:
- Postgres: Uses JSONB
- Snowflake: Uses VARIANT (Object and Array data )
```

### Creating a Snowflake table to handle JSON data
```sql
CREATE TABLE json_data (
    id NUMBER,
    data VARIANT
)
```

### PARSE_JSON
```sql
- expr: JSON data in string format
- Returns: VARIANT type, valid JSON object
```

```sql
SELECT PARSE_JSON(
    '{
        "name": "John",
        "age": 30,
        "city": "New York"
    }' -- enclose JSON data in single quotes
) AS customer_info_json
```

### OBJECT_CONSTRUCT
```
- Syntax: OBJECT_CONSTRUCT(key1, value1, key2, value2, ...)
- Returns: JSON object
```

```sql
SELECT OBJECT_CONSTRUCT(
    'name', 'John',
    'age', 30,
    'city', 'New York'
) 
```

### Querying JSON data in Snowflake
```sql
-- simple JSON data
SELECT 
    customer_info:cust_age,
    customer_info:cust_name
    customer_info:cust_email
FROM 
    customer_info_json_data;
```

### Nested JSON data
```
<column>:<level1_element>:<level2_element>:<level3_element>
<column>:<level1_element>.<level2_element>.<level3_element>
```

### Get JSON data from nested structure
```sql
SELECT 
    customer_info:address:street AS street_name
FROM 
    customer_info_json_data;
```

```sql
SELECT 
    customer_info:address.street AS street_name
FROM 
    customer_info_json_data;
```

### Get nested JSON data and convert to string
```sql
SELECT
  name,
  categories,
  -- Select WheelchairAccessible from attributes converting it to STRING
  attributes:WheelchairAccessible::STRING AS wheelchair_accessible,
  -- Select Saturday, Sunday from hours converting it to STRING
  hours:Saturday::STRING IS NOT NULL OR hours:Sunday::STRING IS NOT NULL AS open_on_weekend
FROM
  yelp_business_data
```

```sql
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
```

```sql
-- Create CTE dogs_allowed.
WITH dogs_allowed AS (
  SELECT *
  FROM yelp_business_data
  WHERE attributes:DogsAllowed::STRING  NOT ILIKE '%None%'
  -- Filter data where DogsAllowed is True.
  AND attributes:DogsAllowed::STRING = 'True'
)

SELECT business_id, name
FROM dogs_allowed
```

```sql
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
    AND attributes:Ambience NOT ILIKE '%u''%'
    AND PARSE_JSON(attributes:Ambience):touristy::BOOLEAN = True
)

SELECT
	d.business_id,
    d.name
FROM dogs_allowed d
JOIN touristy_places t
	ON d.business_id = t.business_id
```