## [ETL and ELT in Python](https://app.datacamp.com/learn/courses/etl-and-elt-in-python)


---------------------------------------------------------------------------
ETL & ELT IN PYTHON                                                       |
---------------------------------------------------------------------------
Data pipelines -> are responsible for moving data from a source to a destination, and transforming it somewhere along the way.

ETL -> Extract, Transform, Load
- Traditional data pipeline design pattern
- Sources may be tabular or non-tabular
- Leverage python with pandas

ELT -> Extract, Load, Transform
- More recent pattern
- Data warehouses
- Typically tabular data
---------------------------------------------------------------------------
ETL 
def load(data_frame, target_table):
  # Some custom-built python logic to load data to SQL
  data_frame.to_sql(name=target_table, con=POSTGRES_CONNECTION)
  print(f"Loading data to the {target_table} table")

# Now, run the data pipeline
extracted_data = extract(file_name="raw_data.csv")
transformed_data = transform(data_frame=extracted_data)
load(data_frame=transformed_data, target_table="clean_data")

---------------------------------------------------------------------------
ELT
def transform(source_table, target_table):
  data_warehouse.run_sql("""
    CREATE TABLE {target_table} AS
      SELECT 
        columne1 column2
      FROM {source_table};
  """)

# Similar to ETL pipeline, but with the transform step moved to the data warehouse
extracted_data = extract(file_name="raw_data.csv")
load(data_frame=extracted_data, table_name="raw_data")
transform(source_table="raw_data", target_table="clean_data")

---------------------------------------------------------------------------
-ETL
def extract(file_name):
    print(f"Extracting data from {file_name}")
    return pd.read_csv(file_name)

# Extract data from the raw_data.csv file
extracted_data = extract(file_name="raw_data.csv")

# Transform the extracted_data
transformed_data = transform(data_frame=extracted_data)

# Load the transformed_data to cleaned_data.csv
load(data_frame=transformed_data, target_table="cleaned_data")

---------------------------------------------------------------------------
- ELT
# Extract data from the raw_data.csv file
raw_data = extract(file_name="raw_data.csv")

# Load the extracted_data to the raw_data table
load(data_frame=raw_data, table_name="raw_data")

# Transform data in the raw_data table
transform(
  source_table="raw_data", 
  target_table="cleaned_data"
)

---------------------------------------------------------------------------
Building ETL and ELT Pipelines

# Extract data from a csv file
import pandas as pd

# Read in the csv file to a dataframe
data_frame = pd.read_csv("raw_data.csv")

# Display the first few rows of the dataframe
print(data_frame.head())

# First, filter by rows
data_frame.loc[data_frame["name"] == "Apparel", :]

# Then by columns
data_frame.loc[:, ["name", "num_firms"]]

# Write a dataframe to a csv
data_frame.to_csv("cleaned_data.csv")

# Other option -> to_sql(), to_json(), to_excel()

# Running SQL queries
data_warehouse.execute( 
  """
  CREATE TABLE total_sales AS
    SELECT 
      ds, 
      SUM(sales)
    FROM raw_sales_data
    GROUP BY ds;
  """
)

---------------------------------------------------------------------------
# Define extract(), transform(), load() functions
...

def transform(data_frame, value):
  return data_frame.loc[data_frame["name"] == value, ["name", "num_firms]]

# Extract
extracted_data = extract(file_name="raw_data.csv")

# Transform
transformed_data = transform(data_frame=extracted_data, value="Apparel")

# Load
load(data_frame=transformed_data, target_table="cleaned_data")

---------------------------------------------------------------------------
def load(data_frame, file_name):
  # Write cleaned_data to a CSV using file_name
  data_frame.to_csv(file_name)
  print(f"Successfully loaded data to {file_name}")

extracted_data = extract(file_name="raw_data.csv")

# Transform extracted_data using transform() function
transformed_data = transform(data_frame=extracted_data)

# Load transformed_data to the file transformed_data.csv
load(data_frame=transformed_data, file_name="transformed_data.csv")

---------------------------------------------------------------------------
# Complete building the transform() function
def transform(source_table, target_table):
  data_warehouse.execute(f"""
  CREATE TABLE {target_table} AS
      SELECT
          CONCAT("Product ID: ", product_id),
          quantity * price
      FROM {source_table};
  """)

extracted_data = extract(file_name="raw_sales_data.csv")
load(data_frame=extracted_data, table_name="raw_sales_data")

# Populate total_sales by transforming raw_sales_data
transform(source_table="raw_sales_data", target_table="total_sales")

---------------------------------------------------------------------------
def extract(file_name):
  return pd.read_csv(file_name)

def transform(data_frame):
  return data_frame.loc[:, ["industry_name", "number_of_firms"]]

def load(data_frame, file_name):
  data_frame.to_csv(file_name)
  
extracted_data = extract(file_name="raw_industry_data.csv")
transformed_data = transform(data_frame=extracted_data)

# Pass the transformed_data DataFrame to the load() function
load(data_frame=transformed_data, file_name="number_of_firms.csv")

---------------------------------------------------------------------------
source of data:
- csv files
- parquet files
- json files
- SQL databases
- APIs
- Data Lakes
- Data Warehouses
- Web scraping
- Streaming data
-...

- Reading in parquet files
import pandas as pd
raw_stock_data = pd.read_parquet("raw_stock_data.parquet", engine="fastparquet)

- Connecting to SQL databases
import pandas as pd
import sqlalchemy

connection_uri = "postgresql+psycompg2://repl:password@localhost:5432/market # schema_identifier://username:password@host:port/database
db_engine = sqlalchemy.create_engine(connection_uri)

raw_stock_data = pd.read_sql("SELECT * FROM raw_stock_data LIMIT 10", db_engine)

---------------------------------------------------------------------------
Modularity

def extract_from_sql(connection_uri, query):
  db_engine = sqlalchemy.create_engine(connection_uri)
  return pd.read_sql(query, db_engine)


extract_from_sql("postgresql+psycompg2://repl:password@localhost:5432/market", "SELECT * FROM raw_stock_data LIMIT 10")

---------------------------------------------------------------------------
import pandas as pd

# Read the sales data into a DataFrame
sales_data = pd.read_parquet("sales_data.parquet", engine="fastparquet")

# Check the data type of the columns of the DataFrames
print(sales_data.dtypes)

# Print the shape of the DataFrame, as well as the head
print(sales_data.shape)
print(sales_data.head())

import sqlalchemy

# Create a connection to the sales database
db_engine = sqlalchemy.create_engine("postgresql+psycopg2://repl:password@localhost:5432/sales")

# Query the sales table
raw_sales_data = pd.read_sql("SELECT * FROM sales", db_engine)
print(raw_sales_data)
---------------------------------------------------------------------------
# Extract
def extract():
    connection_uri = "postgresql+psycopg2://repl:password@localhost:5432/sales"
    db_engine = sqlalchemy.create_engine(connection_uri)
    raw_data = pd.read_sql("SELECT * FROM sales WHERE quantity_ordered = 1", db_engine)
    
    # Print the head of the DataFrame
    print(raw_data.head())
    
    # Return the extracted DataFrame
    return raw_data
    
# Call the extract() function
raw_sales_data = extract()

# Transform
# Filtering records with .loc[]

# Keep only non-zero entries
cleaned = raw_stock_data.loc[raw_stock_data["open"] > 0, :]

# Remove excess columns
cleaned = raw_stock_data.loc[:, ["timestamps", "open", "close"]]

# Combine both steps
cleaned = raw_stock_data.loc[raw_stock_data["open"] > 0, ["timestamps", "open", "close"]]

cleaned = raw_stock_data.iloc[[0:50], [0, 1, 2]]

cleaned["timestamps"] = pd.to_datetime(cleaned["timestamps"], format="%Y%m%d%H%M%S")

---------------------------------------------------------------------------
# Extract data from the sales_data.parquet path
raw_sales_data = extract("sales_data.parquet")

def transform(raw_data):
  	# Only keep rows with `Quantity Ordered` greater than 1
    clean_data = raw_data.loc[raw_data["Quantity Ordered"] > 1, :]
    
    # Only keep columns "Order Date", "Quantity Ordered", and "Purchase Address"
    clean_data = clean_data.loc[:, ["Order Date", "Quantity Ordered", "Purchase Address"]]
    
    # Return the filtered DataFrame
    return clean_data
    
transform(raw_sales_data)

---------------------------------------------------------------------------
raw_sales_data = extract("sales_data.csv")

def transform(raw_data):
    # Convert the "Order Date" column to type datetime
    raw_data["Order Date"] = pd.to_datetime(raw_data["Order Date"], format="%m/%d/%y %H:%M")
    
    # Only keep items under ten dollars
    clean_data = raw_data.loc[raw_data["Price Each"] < 10, :]
    return clean_data

clean_sales_data = transform(raw_sales_data)

# Check the data types of each column
print(clean_sales_data.dtypes)
---------------------------------------------------------------------------
def extract(file_path):
    raw_data = pd.read_parquet(file_path)
    return raw_data

raw_sales_data = extract("sales_data.parquet")

def transform(raw_data):
  	# Filter rows and columns
    clean_data = raw_data.loc[raw_data["Quantity Ordered"] == 1, ["Order ID", "Price Each", "Quantity Ordered"]]
    return clean_data

# Transform the raw_sales_data
clean_sales_data = transform(raw_sales_data)

clean_sales_data.nlargest(1, "Price Each")
---------------------------------------------------------------------------
def transform(raw_data):
	# Find the items prices less than 25 dollars
	return raw_data.loc[raw_data["Price Each"] < 25, ["Order ID", "Product", "Price Each", "Order Date"]]

def load(clean_data):
	# Write the data to a CSV file without the index column
	clean_data.to_csv("transformed_sales_data.csv", index=False)


clean_sales_data = transform(raw_sales_data)

# Call the load function on the cleaned DataFrame
load(clean_sales_data)

---------------------------------------------------------------------------
# Import the os library
import os

# Load the data to a csv file with the index, no header and pipe separated
def load(clean_data, path_to_write):
	clean_data.to_csv(path_to_write, header=False, sep="|")

load(clean_sales_data, "clean_sales_data.csv")

# Check that the file is present.
file_exists = os.path.exists("clean_sales_data.csv")
print(file_exists)
---------------------------------------------------------------------------
def load(clean_data, file_path):
    # Write the data to a file
    clean_data.to_csv(file_path, header=False, index=False)

    # Check to make sure the file exists
    file_exists = os.path.exists(file_path)
    if not file_exists:
        raise Exception(f"File does NOT exists at path {file_path}")

# Load the transformed data to the provided file path
load(clean_sales_data, "transformed_sales_data.csv")
---------------------------------------------------------------------------
Monitoring a data pipeline

Data pipelines should be monitored for changes to data and failures in execution.
- Missing data
- Shifting data types
- Packages deprecation or functionality change

Logging data pipeline performance
- Document performance at excecution 
- Provides a starting point when a solution failures

import logging
logging.basicConfig(format='%(levelname)s: %(message)s', level=LEVEL.DEBUG)

# Create different types of logs
logging.debug(f"variable has value {path}")
logging.info("Data has been transformed and will now be loaded.")
logging.warning("Unexpected number of rows detected.")
logging.error("{ke} arose in execution.")

---------------------------------------------------------------------------
Handling with try and except

try:
  clean_stock_data = transform(raw_stock_data)
  logging.info("Successfully filtered DataFrame by 'price_change'")

except KeyError as ke:
  logging.warning(f"{ke}: Cannot filter DataFrame by 'price_change'")
  raw_stock_data["price_change"] = raw_stock_data["close"] - raw_stock_data["open"]
  clean_stock_data = transform(raw_stock_data)

---------------------------------------------------------------------------
def transform(raw_data):
    raw_data["Order Date"] = pd.to_datetime(raw_data["Order Date"], format="%m/%d/%y %H:%M")
    clean_data = raw_data.loc[raw_data["Price Each"] < 10, :]
    
    # Create an info log regarding transformation
    logging.info("Transformed 'Order Date' column to type 'datetime'.")
    
    # Create debug-level logs for the DataFrame before and after filtering
    logging.debug(f"Shape of the DataFrame before filtering: {raw_data.shape}")
    logging.debug(f"Shape of the DataFrame after filtering: {clean_data.shape}")
    
    return clean_data
  
clean_sales_data = transform(raw_sales_data)

---------------------------------------------------------------------------
def extract(file_path):
    return pd.read_parquet(file_path)

# Update the pipeline to include a try block
try:
	# Attempt to read in the file
    raw_sales_data = extract("sales_data.parquet")
	
# Catch the FileNotFoundError
except FileNotFoundError as file_not_found:
	# Write an error-level log
	logging.error(file_not_found)

---------------------------------------------------------------------------
def transform(raw_data):
	return raw_data.loc[raw_data["Total Price"] > 1000, :]

try:
	clean_sales_data = transform(raw_sales_data)
	logging.info("Successfully filtered DataFrame by 'Total Price'")

except KeyError as ke:
	logging.warning(f"{ke}: Cannot filter DataFrame by 'Total Price'")
	
	# Create the "Total Price" column, transform the updated DataFrame
	raw_sales_data["Total Price"] = raw_sales_data["Price Each"] * raw_sales_data["Quantity Ordered"]
	clean_sales_data = transform(raw_sales_data)

---------------------------------------------------------------------------
Advanced ETL Techniques

Types of non-tabular data:
- Text
- Audio
- Image
- Video
- Spatial
- IoT
- APIs

# Reading JSON files with pandas
raw_stock_data = pd.read_json("raw_stock_data.json", orient="columns")

Nested or unstructured JSON data
- Nested objects
- Varyung "schema"

import json 
with open("raw_stcok_data.json", "r") as file:
  raw_stock_data = json.load(file)

print(type(raw_stock_data))

---------------------------------------------------------------------------
def extract(file_path):
  # Read the JSON file into a DataFrame
  return pd.read_json(file_path, orient="records")

# Call the extract function with the appropriate path, assign to raw_testing_scores
raw_testing_scores = extract("testing_scores.json")

# Output the head of the DataFrame
print(raw_testing_scores.head())

---------------------------------------------------------------------------
def extract(file_path):
  	# Read the JSON file into a DataFrame, orient by index
	return pd.read_json(file_path, orient="index")

# Call the extract function, pass in the desired file_path
raw_testing_scores = extract("nested_scores.json")
print(raw_testing_scores.head())

---------------------------------------------------------------------------
# Import the json library
import json

def extract(file_path):
    with open(file_path, "r") as json_file:
        # Load the data from the JSON file
        raw_data = json.load(json_file)
    return raw_data

raw_testing_scores = extract("nested_scores.json")

# Print the raw_testing_scores
print(raw_testing_scores)

---------------------------------------------------------------------------
# Parse data from dictionary using .get()
volume = entry.get("volume")

# Call .get() twice to parse nested dictionary
price = entry.get("price").get("regular")

---------------------------------------------------------------------------
# Creating a dataframe from a list of lists

raw_data = pd.DataFrame(flattened_rows)
raw_data.columns = ["name", "age", "city", "state"]

# Set an index using .set_index()
raw_data.set_index("name", inplace=True)

---------------------------------------------------------------------------
parsed_stock_data = []

for timestamp, ticker_info in raw_stock_data.items():
  parsed_stock_data.append([
    timestamp, 
    ticker_info.get("price", {}).get("open", 0),
    ticker_info.get("price", {}).get("close", 0),
    ticker_info.get("volume", 0)
  ])

transformed_stock_data = pd.DataFrame(parsed_stock_data)
transformed_stock_data.columns = ["timestamp", "open", "close", "volume"]
transformed_stock_data.set_index("timestamp")

---------------------------------------------------------------------------
# Keys
raw_testing_scores_keys = []

# Iterate through the keys of the raw_testing_scores dictionary
for school_id in raw_testing_scores.keys():
  	# Append each key to the raw_testing_scores_keys list
	raw_testing_scores_keys.append(school_id)
    
print(raw_testing_scores_keys[0:3])

# Values
raw_testing_scores_values = []

# Iterate through the values of the raw_testing_scores dictionary
for school_info in raw_testing_scores.values():
	raw_testing_scores_values.append(school_info)
    
print(raw_testing_scores_values[0:3])

# Items
raw_testing_scores_keys = []
raw_testing_scores_values = []

# Iterate through the values of the raw_testing_scores dictionary
for school_id, school_info in raw_testing_scores.items():
	raw_testing_scores_keys.append(school_id)
	raw_testing_scores_values.append(school_info)

print(raw_testing_scores_keys[0:3])
print(raw_testing_scores_values[0:3])

---------------------------------------------------------------------------
# Parse the street_address from the dictionary
street_address = school.get("street_address")

# Parse the scores dictionary
scores = school.get("scores")

# Try to parse the math, reading and writing values from scores
math_score = scores.get("math", 0)
reading_score = scores.get("reading", 0)
writing_score = scores.get("writing", 0)

print(f"Street Address: {street_address}")
print(f"Math: {math_score}, Reading: {reading_score}, Writing: {writing_score}")

---------------------------------------------------------------------------
normalized_testing_scores = []

# Loop through each of the dictionary key-value pairs
for school_id, school_info in raw_testing_scores.items():
	normalized_testing_scores.append([
    	school_id,
    	school_info.get("street_address"),  # Pull the "street_address"
    	school_info.get("city"),
    	school_info.get("scores").get("math", 0),
    	school_info.get("scores").get("reading", 0),
    	school_info.get("scores").get("writing", 0),
    ])

print(normalized_testing_scores)

# Create a DataFrame from the normalized_testing_scores list
normalized_data = pd.DataFrame(normalized_testing_scores)

# Set the column names
normalized_data.columns = ["school_id", "street_address", "city", "avg_score_math", "avg_score_reading", "avg_score_writing"]

normalized_data = normalized_data.set_index("school_id")
print(normalized_data.head())
---------------------------------------------------------------------------
Advanced transformation with pandas

- Filling missing values with pandas
# Fill all NaN value with 0
clean_stock_data = raw_stock_data.fillna(0)

# Fill NaN values with spesific value for each column
clean_stock_data = raw_stock_data.fillna(value={"open":0,"close":.5}, axis=1)

# Fill NaN values using other columns 
raw_stock_data["open"].fillna(raw_stock_data["close"], inplace=True)

# using python to group by data by ticker, find the mean of the reaminign columns
grouped_stock_data = raw_stock_data.groupby(by=["ticker"], axis=0).mean()

---------------------------------------------------------------------------
def classify_change(row):
  change = row["close"] - row["open"]
  if change > 0:
    return "Increase"
  else:
    return "Decrease"

# Apply method
raw_stock_data["change"] = raw_stock_data.apply(classify_change, axis=1)

axis=0, mean dihitung ke bawah untuk setiap kolom
axis=1, mean dihitung melintasi kolom di setiap baris

---------------------------------------------------------------------------
# Fill NaN values with the average from that column
raw_testing_scores["math_score"] = raw_testing_scores["math_score"].fillna(raw_testing_scores["math_score"].mean())

# Print the head of the raw_testing_scores DataFrame
print(raw_testing_scores.head())

---------------------------------------------------------------------------
def transform(raw_data):
	raw_data.fillna(
    	value={
			# Fill NaN values with column mean
			"math_score": raw_data["math_score"].mean(),
			"reading_score": raw_data["reading_score"].mean(),
			"writing_score": raw_data["writing_score"].mean()
		}, inplace=True
	)
	return raw_data

clean_testing_scores = transform(raw_testing_scores)

# Print the head of the clean_testing_scores DataFrame
print(clean_testing_scores.head())
---------------------------------------------------------------------------
def transform(raw_data):
	# Use .loc[] to only return the needed columns
	raw_data = raw_data.loc[:, ["city", "math_score", "reading_score", "writing_score"]]
	
    # Group the data by city, return the grouped DataFrame
	grouped_data = raw_data.groupby(by=["city"], axis=0).mean()
	return grouped_data

# Transform the data, print the head of the DataFrame
grouped_testing_scores = transform(raw_testing_scores)
print(grouped_testing_scores.head())

---------------------------------------------------------------------------
def transform(raw_data):
	# Use the apply function to extract the street_name from the street_address
    raw_data["street_name"] = raw_data.apply(
   		# Pass the correct function to the apply method
        find_street_name,
        axis=1
    )
    return raw_data

# Transform the raw_testing_scores DataFrame
cleaned_testing_scores = transform(raw_testing_scores)

# Print the head of the cleaned_testing_scores DataFrame
print(cleaned_testing_scores.head())

---------------------------------------------------------------------------
Loading data to a SQL database with pandas

# Create connection object
connection_uri = "postgresql+psycopg://repl:password@localhoost:5432/market"
db_engine = sqlalchemy.create_engine(connection_uri)

# Use the to_sql() method to write the data to the table
clean_stock_data.to_sql(
  name="filtered_stock_data",
  con=db_engine,
  if_exists="append",
  index=True,
  index_label="timestamps"
)

---------------------------------------------------------------------------
# Update the connection string, create the connection object to the schools database
db_engine = sqlalchemy.create_engine("postgresql+psycopg2://repl:password@localhost:5432/schools")

# Write the DataFrame to the scores table
cleaned_testing_scores.to_sql(
	name="scores",
	con=db_engine,
	index=False,
	if_exists="replace"
)

---------------------------------------------------------------------------
def load(clean_data, con_engine):
	# Store the data in the schools database
    clean_data.to_sql(
    	name="scores_by_city",
		con=con_engine,
		if_exists="replace",  # Make sure to replace existing data
		index=True,
		index_label="school_id"
    )

# Call the load function, passing in the cleaned DataFrame
load(cleaned_testing_scores, db_engine)

# Call query the data in the scores_by_city table, check the head of the DataFrame
to_validate = pd.read_sql("SELECT * FROM scores_by_city", con=db_engine)
print(to_validate.head())

---------------------------------------------------------------------------
Deploying and Maintaining a Data Pipeline

Manually testing a data pipeline:
- Validate that data is extracted, transformed, and loaded as expected

Validating pipelines limits maintenance efforts after deployment:
- Identify and fix data quality issues
- Improves data reliability

Tools and techniques to test data pipelines:
- End-to-end testing
- Validating data at "checkpoints"

End-to-end testing:
- Confirm that pipeline runs on repeated attempts
- Validate data at pipeline checkpoints

Validating pipeline checkpoints
# ETL pipeline
.........

# Check data 
loaded_data = pd.read_sql("SELECT * FROM clean_stock_data", con=db_engine)
print(loaded_data.shape)

---------------------------------------------------------------------------
raw_tax_data = extract("raw_tax_data.csv")
clean_tax_data = transform(raw_tax_data)
load(clean_tax_data, "clean_tax_data.parquet")

print(f"Shape of raw_tax_data: {raw_tax_data.shape}")
print(f"Shape of clean_tax_data: {clean_tax_data.shape}")

to_validate = pd.read_parquet("clean_tax_data.parquet")
print(clean_tax_data.head(3))
print(to_validate.head(3))

# Check that the DataFrames are equal
print(to_validate.equals(clean_tax_data))
---------------------------------------------------------------------------
# Trigger the data pipeline to run three times
for attempt in range(0, 3):
	print(f"Attempt: {attempt}")
	raw_tax_data = extract("raw_tax_data.csv")
	clean_tax_data = transform(raw_tax_data)
	load(clean_tax_data, "clean_tax_data.parquet")
	
	# Print the shape of the cleaned_tax_data DataFrame
	print(f"Shape of clean_tax_data: {clean_tax_data.shape}")
    
# Read in the loaded data, check the shape
to_validate = pd.read_parquet("clean_tax_data.parquet")
print(f"Final shape of cleaned data: {to_validate.shape}")

---------------------------------------------------------------------------
Unit-testing a data pipeline

Unit-test:
- Commonly used in software development
- Ensure code works as expected
- Help to validate data

pytest for unit testing 

from pipeline import extract, transform, load

def test_trasnformed_data():
  raw_stok_data = extract("raw_stock_data.csv")
  clean_stock_data = transform(raw_data)
  assert isinstance(clean_stock_data, pd.DataFrame)

execute:
python -m pytest test_pipeline.py

pipeline_type = "ETL"

# Check if pipeline_type is an instance of a string
isinstance(pipeline_type, str)

# Assert that the pipeline does indeed take value "ETL"
assert pipeline_type == "ETL"

assert isinstance(pipeline_type, str)

---------------------------------------------------------------------------
# Mocking data pipeline components with fixtures

import pytest

@pytest.fixture
def clean_data():
  raw_stock_data = extract("raw_stock_data.csv")
  clean_stock_data = transform(raw_data)
  return clean_stock_data

def test_transformed_data(clean_data):
  assert isinstance(clean_data, pd.DataFrame)
  aseert len(clean_data.columns) == 4
  assert clean_data["open].min() >= 0
  assert clean_data["open"].min() >= 0 and clean_data["open"].max() <= 1000

---------------------------------------------------------------------------
raw_tax_data = extract("raw_tax_data.csv")
clean_tax_data = transform(raw_tax_data)

# Validate the number of columns in the DataFrame
assert len(clean_tax_data.columns) == 5

# Determine if the clean_tax_data DataFrames take type pd.DataFrame
assert isinstance(clean_tax_data, pd.DataFrame)

# Assert that clean_tax_data takes is an instance of a string
try:
	assert isinstance(clean_tax_data, str)
except Exception as e:
	print(e)

---------------------------------------------------------------------------
import pytest

def test_transformed_data():
    raw_tax_data = extract("raw_tax_data.csv")
    clean_tax_data = transform(raw_tax_data)
    
    # Assert that the transform function returns a pd.DataFrame
    assert isinstance(clean_tax_data, pd.DataFrame)
    
    # Assert that the clean_tax_data DataFrame has more columns than the raw_tax_data DataFrame
    assert len(clean_tax_data.columns) > len(raw_tax_data.columns)

---------------------------------------------------------------------------
# Import pytest
import pytest

@pytest.fixture()
def clean_tax_data():
    raw_data = pd.read_csv("raw_tax_data.csv")
    clean_data = transform(raw_data)
    return clean_data

# Pass the fixture to the function
def test_tax_rate(clean_tax_data):
    # Assert values are within the expected range
    assert clean_tax_data["tax_rate"].max() <= 1 and clean_tax_data["tax_rate"].min() >= 0

---------------------------------------------------------------------------
Running a data pipeline in production 

Data pipeline architecture patterns:
- Separate file from the execution logic

from pipeline_utils import extract, transform, load

raw_stock_data = extract("raw_stock_data.csv")
clean_stock_data = transform(raw_stock_data)
load(clean_stock_data, "clean_stock_data.csv")

---------------------------------------------------------------------------
# Running a data pipeline end-to-end

import logging
from pipeline_utils import extract, transform, load

logging.basicConfig(format='%(levelname)s : %(message)s', level=logging.DEBUG)

try: 
  raw_stock_data = extract("raw_stock_data.csv")
  clean_stock_data = transform(raw_stock_data)
  load(clean_stock_data)

  logging.info("Successfully extracted, transformed, and loaded data.")

except Exception as e:
  logging.error(f"Pipeline failed with error: {e}")

---------------------------------------------------------------------------
import logging
from pipeline_utils import extract, transform, load

logging.basicConfig(format='%(levelname)s: %(message)s', level=logging.DEBUG)

try:
	raw_tax_data = extract("raw_tax_data.csv")
	clean_tax_data = transform(raw_tax_data)
	load(clean_tax_data, "clean_tax_data.parquet")
    
	logging.info("Successfully extracted, transformed and loaded data.")  # Log a success message.
    
except Exception as e:
	logging.error(f"Pipeline failed with error: {e}")  # Log failure message

