## [Introduction to PySpark](https://app.datacamp.com/learn/courses/introduction-to-pyspark)

-----------------------------------------------------------------------------
INTRODUCTION TO PYSPARK
-----------------------------------------------------------------------------
Apache Spark -> opem-source, distributed computing systen designed for fast processing of large-scale data
- Support various data formats including CSV, Parquet, and JSON
- SQL integration allows querying of data using both Python and SQL syntax

PySpark -> Python API for Apache Spark
- Big data analytics
- Distributed data processing
- Real-time data streaming
- Machine learning on large datasets
- ETL and ELT pipelines
- Working with diverse data sources:
    - CSV
    - JSON
    - Parquet

Key component:
- Spark cluster -> group of computers/nodes that collaboratively process large datasets
- Master node -> manages the cluster, coordinates tasks, and schedules jobs
- Worker nodes -> execute the tasks assigned by the master, responsible for executing the actual computations and storing data in memory or disk 

                [ Driver Program ]
                        |
                        v
                [ Master Node ]
                /      |      \
            v          v       v
    [ Worker 1 ] [ Worker 2 ] [ Worker 3 ]

Master Node = Manajer Proyek
Worker Node = Tim Pekerja
Task = Job-job kecil yang dikerjakan tim
Driver Program = Klien yang pesan proyek

SparkSession:
- Allow you to access your Spark cluster and are critical for using PySpark

# Import SparkSession
from pyspark.sql import SparkSession

# Initialize a SparkSession
spark = SparkSession.builder.appName("MySparkApp").getOrCreate()

.builder() -> sets up a session
getOrCreate() -> creates or retrieves a session 
.appName() -> helps manage multiple sessions

---------------------------------------------------------------------------
PySpark DataFrame

- Similar to other DataFrames but 
- Optimized for PySpark

# Import SparkSession
from pyspark.sql import SparkSession

# Initialize a SparkSession
spark = SparkSession.builder.appName("MySparkApp").getOrCreate()

# Create DataFrame
salaries_df = spark.read.csv("datasets/salaries.csv", ["work_year", "experience_level", "employment_type", "job_title", "salary"])
salaries_df.show()

# Read in the CSV
census_adult = spark.read.csv("adult_reduced.csv")

# Show the DataFrame
census_adult.show()

---------------------------------------------------------------------------
# Create a DataFrame from filestores
census_df = spark.read.csv('path/to/file', header=True, inferSchema=True)

# .count() will return the total row numbers in the DataFrame
row_count = census_df.count()
print(f"Number of rows: {row_count}")

# groupby () allows the use of sql-like aggregation functions
census_df.groupBy('gender').agg({'salary_usd': 'avg'}).show()

salaries_df.groupBy('experience_level') \
    .agg(
       round(avg('salary'), 2).alias('average_salary'),
       round(avg('work_year'), 2).alias('average_work_year')
    ) \
    .show()

.select() -> allows you to select specific columns from a DataFrame
.filter() -> allows you to filter rows based on a condition
.groupBy() -> allows you to group rows together based on a column
.agg() -> allows you to perform aggregate functions on a DataFrame

# Average salary for entry level in Canada
CA_jobs = salaries_df.filter(salaries_df['company_location'] == "CA").filter(salaries_df['experience_level'] == "EN").groupBy().avg("salary_in_usd")

# Show the result
CA_jobs.show()

---------------------------------------------------------------------------
spark.read.csv("path/to/file") -> read a CSV file
spark.read.json("path/to/file") -> read a JSON file
spark.read.parquet("path/to/file") -> read a Parquet file

- Spark can infer schemas from data with inferSchema=True
- Manually define schema for better control

Datatypes in PySpark DataFrame:
- IntegerType -> 1, 34545, -345
- LongType -> 1821728121
- FloatType and DoubleType -> 1.0, 3.14, 1.0e10
- StringType -> "Hello, World!"


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

.select() -> allows you to select specific columns from a DataFrame
.filter() -> allows you to filter rows based on a condition
.where() -> allows you to filter rows based on a condition
.sort() -> allows you to sort the rows in a DataFrame
.na.drop() -> allows you to drop rows with missing values

df.select("name", "age").show()
df.filter(df["age"] > 21).show()
df.where(df["age"] > 21).show()
df.sort(df["age"].desc()).show()
df.na.drop().show()
---------------------------------------------------------------------------
# Load the dataframe
census_df = spark.read.json("adults.json")

# Filter rows based on salary condition
salary_filtered_census = census_df.filter(census_df['age'] > 40)

# Show the result
salary_filtered_census.show()

---------------------------------------------------------------------------
Handling missing data

- Drop rows with missing values
.na.drop() 
df_cleaned = df.na.drop()

- Filter out nulls
df_cleaned = df.where(col("columnName").isNotNull())

- Fill missing values
df_filled = df.na.fill({"columnName": 0})

---------------------------------------------------------------------------
Column operations

- Create a new columns from existing columns
df = df.withColumn("new_column", df["old_column"] + 1)

- Rename columns
df = df.withColumnRenames("old_column", "new_column")

- Drop columns
df = df.drop("column_name")

---------------------------------------------------------------------------
Joins in PySpark

- Joining on id column using an inner join 
df_joined = df1.join(df2, on='id', how='inner')

- Joining on columns with different names
df_joined = df1. join(df2, df1.Id == df2.Name, how='inner')

Union operations
df_union = df1.union(df2)

---------------------------------------------------------------------------
Working with Arrays and Maps

- Arrays: Usefule for storing lists within columns 

from pyspark.sql.functions import array, struct, lit

# Create an array column
df = df.withColumn("scores, array(lit(85), lit(90), lit(78)))

- Maps: key-value pairs
schema = StructType([
    StructField('name', StringType(), True),
    StructField('properties', MapType(StringType(), StringType()), True)
])

Working with structs

df = df.withCOlumn("name_struct", struct("first_name", "last_name"))

