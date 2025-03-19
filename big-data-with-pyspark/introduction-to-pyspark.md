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
getOrCreate()   -> creates or retrieves a session 
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

### Create a new columns from existing columns
```python
df = df.withColumn("new_column", df["old_column"] + 1)
```

### Rename columns
```python
df = df.withColumnRenames("old_column", "new_column")
```

### Drop columns
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
df = df.withCOlumn("name_struct", struct("first_name", "last_name"))
```
