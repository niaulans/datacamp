## [Introduction to PySpark](https://app.datacamp.com/learn/courses/introduction-to-pyspark)

### Apache Spark
```
- Open source, distributed computing systen designed for fast processing of large-scale data
- Support various data formats including CSV, Parquet, and JSON
- SQL integration allows querying of data using both Python and SQL syntax
```

### PySpark
```
PySpark -> Python API for Apache Spark

Useful for:
- Big data analytics
- Distributed data processing
- Real-time data streaming
- Machine learning on large datasets
- ETL and ELT pipelines
- Working with diverse data sources:
    - CSV
    - JSON
    - Parquet
```

### PySpark Ecosystem
```
Key component:
- Spark cluster     -> group of computers/nodes that collaboratively process large datasets
- Master node       -> manages the cluster, coordinates tasks, and schedules jobs
- Worker nodes      -> execute the tasks assigned by the master, responsible for executing the actual computations and storing data in memory or disk 

                [ Driver Program ]
                        |
                        v
                [ Master Node ]
                /      |      \
            v          v       v
    [ Worker 1 ] [ Worker 2 ] [ Worker 3 ]

Driver Program  = client
Master Node     = project manager
Worker Node     = team members
Task            = project

``` 

### SparkSession
```
Allow you to access your Spark cluster and are critical for using PySpark
```

### Initializing a SparkSession
```python
# Import SparkSession
from pyspark.sql import SparkSession

# Initialize a SparkSession
spark = SparkSession.builder.appName("MySparkApp").getOrCreate()
```

```
.builder()      -> sets up a session
.appName()      -> helps manage multiple sessions
.getOrCreate()  -> creates or retrieves a session 
```

### PySpark dataframe
```python
# Import SparkSession
from pyspark.sql import SparkSession

# Initialize a SparkSession
spark = SparkSession.builder.appName("MySparkApp").getOrCreate()

# Create DataFrame by reading a CSV file
salaries_df = spark.read.csv("datasets/salaries.csv", ["work_year", "experience_level", "employment_type", "job_title", "salary"])

# Show the DataFrame
salaries_df.show()
```

### PySpark dataframe operations -> count() and groupBy()
```python
# Create a DataFrame by reading a CSV file
salaries_df = spark.read.csv('path/to/file', header=True, inferSchema=True)

# .count() will return the total row numbers in the DataFrame
row_count = salaries_df.count()
print(f"Number of rows: {row_count}")

# groupby () allows the use of sql-like aggregation functions
from pyspark.sql.functions import avg

salaries_df.groupBy('experience_level') \
    .agg(
       round(avg('salary'), 2).alias('average_salary'),
       round(avg('work_year'), 2).alias('average_work_year')
    ) \
    .show()
```

```
.select()   -> allows you to select specific columns from a DataFrame
.filter()   -> allows you to filter rows based on a condition
.where()    -> allows you to filter rows based on a condition
.groupBy()  -> allows you to group rows together based on a column
.agg()      -> allows you to perform aggregate functions on a DataFrame
.sort()     -> allows you to sort the rows in a DataFrame
.na.drop()  -> allows you to drop rows with missing values
```

```python
df.select("name", "age").show()
df.filter(df["age"] > 21).show()
df.where(df["age"] > 21).show()
df.sort(df["age"].desc()).show()
df.na.drop().show()
```

### PySpark dataframe operations -> filter()
```python
# Average salary for entry level in Canada
CA_jobs = salaries_df.filter(salaries_df['company_location'] == "CA").filter(salaries_df['experience_level'] == "EN").groupBy().avg("salary_in_usd")

# Show the result
CA_jobs.show()
```

```
spark.read.csv("path/to/file")      -> read a CSV file
spark.read.json("path/to/file")     -> read a JSON file
spark.read.parquet("path/to/file")  -> read a Parquet file

- Spark can infer schemas from data with inferSchema=True
- Manually define schema for better control
```

### Data types in PySpark
```
- IntegerType               -> 1, 34545, -345
- LongType                  -> 1821728121
- FloatType and DoubleType  -> 1.0, 3.14, 1.0e10
- StringType                -> "Hello, World!"
```

### PySpark schema
```python
# Import the necessary type as classes 
from pyspark.sql.types import (StructType, 
                               StructField, 
                               IntegerType, StringType, ArrayType)

# Construct the schema
schema = StructType([
    StructField("id", IntegerType(), True),
    StructField("name", StringType(), True),
    StructField("scores", ArrayType(IntegerType()), True)
])

# Set the schema
df = spark.createDataFrame(data, schema=schema)
df.show()
```

### Drop rows with missing values
```python
df_cleaned = df.na.drop()
```

### Filter out nulls
```python
df_cleaned = df.where(col("columnName").isNotNull())
```

### Fill missing values 
```python
df_filled = df.na.fill({"columnName": 0})
```

### Create a new column from existing column
```python
df = df.withColumn("new_column", df["old_column"] + 1)
```

### Rename column
```python
df = df.withColumnRenamed("old_column", "new_column")
```

### Drop column
```python
df = df.drop("column_name")
```

### Joining on id column using an inner join 
```python
df_joined = df1.join(df2, on='id', how='inner')
```

### Joining on columns with different names
```python
df_joined = df1. join(df2, df1.Id == df2.Name, how='inner')
```

### Union operations
```python
df_union = df1.union(df2)
```

### Arrays and Maps
```
Arrays: Useful for storing lists within columns 
```

```python
from pyspark.sql.functions import array, struct, lit

# Create an array column
df = df.withColumn("scores, array(lit(85), lit(90), lit(78)))

- Maps: key-value pairs
schema = StructType([
    StructField('name', StringType(), True),
    StructField('properties', MapType(StringType(), StringType()), True)
])
```

### Working with structs
```python
df = df.withColumn("name_struct", struct("first_name", "last_name"))
```

### Query
```python
adults_by_marriage = spark.sql("SELECT * FROM people WHERE age > 18 AND married = true")

pd_counts = adults_by_marriage.toPandas()

print(pd_counts.head())
```

### User define function (UDF)
``` 
UDF -> custom function to work with data using PySpark dataframes
Advantages of UDFs:
- Reuse and repeat common tasks
- Registered directly with Spark and can be shared
- PySpark DataFrames (for smaller datasets)
- pandas UDFs (for larger datasets)
- All PySpark UDFs need to be registered via the udf() function.
```

```python
from pyspark.sql.functions import udf

# Define the function
def to_uppercase(s):
    return s.upper() if s else None

# Register the function 
to_uppercase_udf = udf(to_uppercase, StringType())

# Apply the UDF to the DataFrame
df = df.withColumn("name_upper", to_uppercase_udf(df["name"]))

# See the result
df.show()
```

### pandas UDF
```
- Eliminates costly conversions of code and data
- Does not need to be registered to the SparkSession
- Uses pandas capabilities on extremely large datasets

```python
from pyspark.sql.functions import pandas_udf

@pandas_udf("float")
def fahrenheit_to_celcius_pandas(temp_f):
    return (temp_f - 32) * 5.0/9.0
```

| PySpark UDF | pandas UDF |
|------------|-----------|
| Simple for relatively small datasets | Relatively large datasets |
| Simple transformations like data cleaning | Complex operations beyond simple data cleaning |
| Changes occur at the columnar level, not the row level | Specific row level changes over column level |
| Must be registered  to a Spark Session with udf() | Can be called ouside the Spark Session |

```python
# Register the function age_category as a UDF
age_category_udf = udf(age_category, StringType())

# Apply your udf to the DataFrame
age_category_df_2 = age_category_df.withColumn("category", age_category_udf(age_category_df["age"]))

# Show df
age_category_df_2.show()
```

```python
# Define a Pandas UDF that adds 10 to each element in a vectorized way
@pandas_udf(DoubleType())
def add_ten_pandas(column):
    return column + 10

# Apply the UDF and show the result
df.withColumn("10_plus", add_ten_pandas(df['value']))
df.show()
```

### Resilient distributed datasets (RDD) in PySpark
```
Parallelization in PySpark:
- Automatically parallelizing data and computations across multiple nodes in a cluster
- Distributed processing of large datasets across multiple nodes
- Worker nodes process data in parallel, combining at the end of the task
- Faster processinf at scale (think gigabytes or even terabytes)

Understanding RDDs:
- Distributed data collections across a cluster with automatic recovery from node failures
- Good for large scale data
- Immutable and can be transformed using operations like map() or filter(), with actions like collect() or paralelize() to retrieve results or create RDDs
- Map() : method applies functions across a dataset like: rdd.map(lambda x: x * 2)
```

### Creating an RDD
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("RDDExample").getOrCreate()

# Create a DataFrame from csv
census_df = spark.read.csv("/census.csv")

# Convert DataFrame to RDD
census_rdd = census_df.rdd

#Show the RDD using collect()
census_rdd.collect()
```

```python
# Collect the entire DataFrame into a local Pythoon list of row objects
data_collected = df.collect()

# Print the collected data
for row in data_collected:
    print(row)

# Example output:
# Row(name='John', age=30, city='New York')
# Row(name='Alice', age=25, city='Los Angeles')
```

### RDDs vs. DataFrames
| DataFrames | RDDs |
|------------|------|
| High-level: optimized for ease of use | Low-level: More flexible but requiring more lines of code for complex operations |
| SQL Like operations: Work with SQL-like queries and perform complex operations with less code | Type Safety: Preserve data types but don't have the optimization benefits of DataFrames |
| Schema information: Contain columns and types line an SQL table | No schema: Harder to work with structured data like SQL or relational data |
| - | Large scaling |
| - | very very verbose compared to DataFrames and poor at analytics |


```python
# Create a DataFrame
df = spark.read.csv("salaries.csv", header=True, inferSchema=True)

# Convert DataFrame to RDD
rdd = df.rdd

# Show the RDD's contents
rdd.collect()
print(rdd)
```

```python
# Create an RDD from the df_salaries
rdd_salaries = df_salaries.rdd

# Collect and print the results
print(rdd_salaries.collect())

# Group by the experience level and calculate the maximum salary
dataframe_results = df_salaries.groupby("experience_level").agg({"salary_in_usd": 'max'})

# Show the results
dataframe_results.show()
```

### Spark SQL
```
- Module in Apache Spark for structured data processing
- Allows us to run SQL queries alongside data processing tasks
- Seamless combination of Python and SQL in one application 
- DataFrame interfacing: provides programmatic access to structured data
- To work with SQL in PySpark, you need to create a SparkSession and register your DataFrame as a temporary view
```

```python
from spark.sql import SparkSession 

spark = SparkSession.builder.appName("Spark SQL Example").getOrCreate()

data = [("Alice", "HR", 30), ("Bob", "IT", 40), ("Cathy", "HR", 28)]
columns = ["Name", "Department, "Age"]

df = spark.createDataFrame(data, schema=columns)

# Register DataFrame as a temporary view
df.createOrReplaceTempView("people")

# Query using SQL
result = spark.sql("SELECT Name, Age, FROM people WHERE Age > 30")
result.show()
```

### Temp views
```
- Temp views protect the underlying data while doing analytics
- Loading from a csv uses methods we already know
df = spark.read.csv("path/to/file", header=True, inferSchema=True)
df.createORReplaceTempView("employees")
```

### Combining SQL and DataFrames
```python
# SQL query result 
query_result = spark.sql("SELECT Name, Salary FROM employees WHERE Salary > 3000")

# DataFrame transformation
high_earners = query_result.withCOlumn("Bonus", query_result.Salary * 0.1)
high_earners.show()
```

```python
# Register as a view
df.createOrReplaceTempView("data_view")

# Advanced SQL query: Calculate total salary by Position
result = spark.sql("""
    SELECT Position, SUM(Salary) AS Total_Salary
    FROM data_view
    GROUP BY Position
    ORDER BY Total_Salary DESC
    """
)
result.show()
```

```python
# Create a temporary table "people"
df.createOrReplaceTempView("people")

# Select the names from the temporary table people
query = """SELECT name FROM people"""

# Assign the result of Spark's query to people_df_names
people_df_names = spark.sql(query)

# Print the top 10 names of the people
people_df_names.show(10)
```

```python
# Create a temporary view of salaries_table
salaries_df.createOrReplaceTempView('salaries_table')

# Construct the "query"
query = '''SELECT job_title, salary_in_usd FROM salaries_table WHERE company_location == "CA"'''

# Apply the SQL "query"
canada_titles = spark.sql(query)

# Generate basic statistics
canada_titles.describe().show()
```

### PySpark aggregations
```python
# Combining SQL and DataFrames
filtered_df = df.filter(df.Salary > 3000)

filtered_df.createOrReplaceTempView("filtered_employees")

spark.sql("""
    SELECt Department, COUNT(*) AS Employee_Count
    FROM filtered_employees
    GROUP BY Department
    """).show()
```

### Handling data types in aggregations PySpark
```python
# Example of type casting
data = [("Alice", 3000), ("Bob", 4000), ("Cathy", 5000)]
columns = ["Name", "Salary"]
df = createDataframe(data, schema=columns)

df = df.withColumn("Salary", df["Salary"].cast("int"))

df.groupBy("Department").sum("Salary").show()
```

### Handling data types in aggregations RDD
```python
rdd = df.rdd.map(lambda row: (row["Department"], row["Salary"])
rdd_aggregated = rdd.reduceByKey(lambda x, y:x + y))
rdd_aggregated.collect()
```

### Best practices for PySpark aggregations
```
- Filter early: reduce data size before performing aggregations
- Handle data types: ensure data is clean and correctly typed
- Avoid operations that use the entire dataset: minimize operations like groupBy()
- Choose the right interface: prefer dataframes for most tasks due to their optimizations
- Monitor performance: use explain() to inspect the execution plan and optimize queries
```

```python
# PySpark DataFrame
# Find the minimum salaries for small companies
salaries_df.filter(salaries_df.company_size == "S").groupBy().min("salary_in_usd").show()

# Find the maximum salaries for large companies
salaries_df.filter(salaries_df.company_size == "L").groupBy().max("salary_in_usd").show()
```

```python
# PySpark RDD
# DataFrame Creation
data = [("HR", "3000"), ("IT", "4000"), ("Finance", "3500")]
columns = ["Department", "Salary"]
df = spark.createDataFrame(data, schema=columns)

# Map the DataFrame to an RDD
rdd = df.rdd.map(lambda row: (row["Department"], row["Salary"]))

# Apply a lambda function to get the sum of the DataFrame
rdd_aggregated = rdd.reduceByKey(lambda x, y: x + y)

# Show the collected Results
print(rdd_aggregated.collect())
```

```python
# Average salaries at large us companies
large_companies=salaries_df.filter(salaries_df.company_size == "L").filter(salaries_df.company_location == "US").groupBy().avg("salary_in_usd")

#set a large companies variable for other analytics
large_companies=salaries_df.filter(salaries_df.company_size == "L").filter(salaries_df.company_location == "US")

# Total salaries in usd
large_companies.groupBy().sum("salary_in_usd").show()
```

### PySpark at scale
```
- PySpark works effectively with gigabytes and terabytes of data
- Using PySpark, speed and efficient processing the the goal
- Understanding PySpark execution gets even more efficiencies
- Use broadcast to manage the whole cluster
```

```python
joined_df = large_df.join(broadcast(smal_df), on="id", how="inner")
joined_df.show()
```

### Execution plan
```python
# Using explain() to view the execution plan
df.filter(df.Age > 40).select("Name").explain()

# Output:
# == Physical Plan ==
# *(1) Project [Name#0]
# +- *(1) Filter (isnotnull(Age#1) AND (Age#1 > 40))
#    +- *(1) Scan ExistingRDD[Name#0, Age#1]
```

### Caching and persisting DataFrames
```
- Caching: Stores data in memory, for faster access for smaller datasets
- Persisting: Stores data in different storage levels for larger datasets
```
```python
df = spark.read.csv("large_dataset.csv", header=True, inferSchema=True)

# Cache the DataFrame
df.cache()

# Perform multiple operations on the cached DataFrame
df.filter(df["column1"] > 50).show()
df.groupBy("column2").count().show()
```

### Persisting DataFrames with different storage levels
```python
from pyspark import StorageLevel

df.persist(StorageLevel.MEMOORY_AND_DISK)

result = df.groupBy("Column1").agg({"column4": "sum"})

# Unpersist the DataFrame when done
df.unpersist()
```

```python
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.appName("final_spark").getOrCreate()

# Print my_spark
print(my_spark)

# Load dataset into a DataFrame
df = my_spark.createDataFrame(data, schema=columns)

df.show()

# Cache the DataFrame
df.cache()

# Perform aggregation
agg_result = df.groupBy("Department").sum("Salary")
agg_result.show()

# Analyze the execution plan
agg_result.explain()

# Uncache the DataFrame
df.unpersist()
```










