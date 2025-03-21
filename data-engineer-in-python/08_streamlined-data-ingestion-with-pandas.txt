## [Streamlined Data Ingestion with pandas](https://app.datacamp.com/learn/courses/streamlined-data-ingestion-with-pandas)


---------------------------------------------------------------
STREAMLINED DATA INGESTION WITH PANDAS                        |
---------------------------------------------------------------

# Create dataframe of next 500 rows with labeled columns
vt_data_next500 = pd.read_csv("vt_tax_data_2016.csv", 
                       		  nrows=500,
                       		  skiprows=500,
                       		  header=None,
                       		  names=vt_data_first500.columns)

# View the Vermont dataframes to confirm they're different
print(vt_data_first500.head())
print(vt_data_next500.head())

error_bad_lines=False -> skip rows with errors
warn_bad_lines=True -> show warning for bad lines

---------------------------------------------------------------
# Create dict specifying data types for agi_stub and zipcode
data_types = {'agi_stub' : "category",
			  'zipcode': str}

# Load csv using dtype to set correct data types
data = pd.read_csv("vt_tax_data_2016.csv", dtype=data_types)

# Print data types of resulting frame
print(data.dtypes.head())

---------------------------------------------------------------
# Create dict specifying that 0s in zipcode are NA values
null_values = {'zipcode': 0}

# Load csv using na_values keyword argument
data = pd.read_csv("vt_tax_data_2016.csv", 
                   na_values=null_values)

# View rows with NA ZIP codes
print(data[data.zipcode.isna()])
---------------------------------------------------------------
try:
  # Set warn_bad_lines to issue warnings about bad records
  data = pd.read_csv("vt_tax_data_2016_corrupt.csv", 
                     error_bad_lines=False, 
                     warn_bad_lines=True)
  
  # View first 5 records
  print(data.head())
  
except pd.errors.ParserError:
    print("Your data contained rows that could not be parsed.")

---------------------------------------------------------------
# Load pandas as pd
import pandas as pd

# Read spreadsheet and assign it to survey_responses
survey_responses = pd.read_excel('fcc_survey.xlsx')

# View the head of the dataframe
print(survey_responses.head())

---------------------------------------------------------------
# Create string of lettered columns to load
col_string = 'AD, AW:BA'

# Load data with skiprows and usecols set
survey_responses = pd.read_excel("fcc_survey_headers.xlsx", 
                        skiprows=2, 
                        usecols=col_string)

# View the names of the columns selected
print(survey_responses.columns)
---------------------------------------------------------------
# Create df from second worksheet by referencing its position
responses_2017 = pd.read_excel("fcc_survey.xlsx",
                               sheet_name=1)

# Graph where people would like to get a developer job
job_prefs = responses_2017.groupby("JobPref").JobPref.count()
job_prefs.plot.barh()
plt.show()

---------------------------------------------------------------
# Create df from second worksheet by referencing its name
responses_2017 = pd.read_excel("fcc_survey.xlsx",
                               sheet_name='2017')

# Graph where people would like to get a developer job
job_prefs = responses_2017.groupby("JobPref").JobPref.count()
job_prefs.plot.barh()
plt.show()

---------------------------------------------------------------
# Load both the 2016 and 2017 sheets by name
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=['2016', '2017'])

# View the data type of all_survey_data
print(type(all_survey_data))

# Load all sheets in the Excel file
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name = [0, '2017'])

# View the sheet names in all_survey_data
print(all_survey_data.keys())

---------------------------------------------------------------
# Load all sheets in the Excel file
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=None)

# View the sheet names in all_survey_data
print(all_survey_data.keys())

---------------------------------------------------------------
# Create an empty dataframe
all_responses = pd.DataFrame()

# Set up for loop to iterate through values in responses
for df in responses.values():
  # Print the number of rows being added
  print("Adding {} rows".format(df.shape[0]))
  # Concatenate all_responses and df, assign result
  all_responses = pd.concat([all_responses, df])

# Graph employment statuses in sample
counts = all_responses.groupby("EmploymentStatus").EmploymentStatus.count()
counts.plot.barh()
plt.show()
---------------------------------------------------------------
# Load the data
survey_data = pd.read_excel("fcc_survey_subset.xlsx")

# Count NA values in each column
print(survey_data.isna().sum())

# Set dtype to load appropriate column(s) as Boolean data
survey_data = pd.read_excel("fcc_survey_subset.xlsx",
                            dtype={'HasDebt': bool})

# View financial burdens by Boolean group
print(survey_data.groupby('HasDebt').sum())

# Load file with Yes as a True value and No as a False value
survey_subset = pd.read_excel("fcc_survey_yn_data.xlsx",
                              dtype={"HasDebt": bool,
                              "AttendedBootCampYesNo": bool},
                              true_values=['Yes'],
                              false_values=['No'])

# View the data
print(survey_subset.head())

---------------------------------------------------------------
# Load file, with Part1StartTime parsed as datetime data
survey_data = pd.read_excel("fcc_survey.xlsx",
                            parse_dates=['Part1StartTime'])

# Print first few values of Part1StartTime
print(survey_data.Part1StartTime.head(1))

---------------------------------------------------------------
# Create dict of columns to combine into new datetime column
datetime_cols = {"Part2Start": ['Part2StartDate', 'Part2StartTime']}


# Load file, supplying the dict to parse_dates
survey_data = pd.read_excel("fcc_survey_dts.xlsx",
                            parse_dates=datetime_cols)

# View summary statistics about Part2Start
print(survey_data.Part2Start.describe())
---------------------------------------------------------------
# Parse datetimes and assign result back to Part2EndTime
survey_data["Part2EndTime"] = pd.to_datetime(survey_data["Part2EndTime"], 
                                             format="%m%d%Y %H:%M:%S")

# Print first few values of Part2EndTime
print(survey_data['Part2EndTime'].head())

---------------------------------------------------------------
Importing data from database

import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///data.db")
print(engine.table_names())

weather = pd.read_sql("SELECT * FROM wether", engine)

------------------------------------------------------------------------
# Create database engine for data.db
engine = create_engine("sqlite:///data.db")

# Write query to get date, tmax, and tmin from weather
query = """
SELECT date, 
       tmax, 
       tmin
  FROM weather;
"""

# Make a dataframe by passing query and engine to read_sql()
temperatures = pd.read_sql(query, engine)

# View the resulting dataframe
print(temperatures)

---------------------------------------------------------------------------
# Create query to get hpd311calls records about safety
query = """
SELECT *
FROM hpd311calls
WHERE complaint_type LIKE '%SAFETY%';
"""

# Query the database and assign result to safety_calls
safety_calls = pd.read_sql(query, engine)

# Graph the number of safety calls by borough
call_counts = safety_calls.groupby('borough').unique_key.count()
call_counts.plot.barh()
plt.show()
---------------------------------------------------------------------------
# Create query for records with max temps <= 32 or snow >= 1
query = """
SELECT *
  FROM weather
  WHERE tmax <= 32 OR snow >= 1;
"""

# Query database and assign result to wintry_days
wintry_days = pd.read_sql(query, engine)

# View summary stats about the temperatures
print(wintry_days.describe())
---------------------------------------------------------------------------
Importing JSON Data and Working with APIs

import pandas as pd
death_cause = pd.read_json("nyc_death_cause.json", orient="split")

---------------------------------------------------------------------------
# Load pandas as pd
import pandas as pd

# Load the daily report to a dataframe
pop_in_shelters = pd.read_json("dhs_daily_report.json")

# View summary stats about pop_in_shelters
print(pop_in_shelters.describe())

---------------------------------------------------------------------------
try:
    # Load the JSON without keyword arguments
    df = pd.read_json("dhs_report_reformatted.json", orient="split")
    
    # Plot total population in shelters over time
    df["date_of_census"] = pd.to_datetime(df["date_of_census"])
    df.plot(x="date_of_census", 
            y="total_individuals_in_shelter")
    plt.show()
    
except ValueError:
    print("pandas could not parse the JSON.")

---------------------------------------------------------------------------
API

import requests
import pandas as pd

api_url = "https://api.yelp.com/v3businesses/search

#Setup the parameters of the request
params = {"term": "bookstore",
          "location": "NYC"}

# Setup header dictionary w/ API key according to documentation
header = {"Authorization": "Bearer {}".format(api_key)}

# Call the API
response = requests.get(api_url, headers=header, params=params)

data = response.json()
print(data)

bookstrores = pd.DataFrame(data["businesses"])
print(bookstores.head())

---------------------------------------------------------------------------
api_url = "https://api.yelp.com/v3/businesses/search"

# Get data about NYC cafes from the Yelp API
response = requests.get(api_url, 
                headers=headers, 
                params=params)

# Extract JSON data from the response
data = response.json()

# Load data to a dataframe
cafes = pd.DataFrame(data['businesses'])

# View the data's dtypes
print(cafes.dtypes)
---------------------------------------------------------------------------
pandas.io.json => read and write JSON
json_normalize() => flatten nested JSON data -> attribute.nestedattribute


import pandas as pd
import requests
from pandas.io.json import json_normalize

api_url = "https://api.yelp.com/v3/businesses/search"

# Get data about NYC cafes from the Yelp API
response = requests.get(api_url, 
                headers=headers, 
                params=params)

# Extract JSON data from the response
data = response.json()

# Load data to a dataframe
bookstores = json_normalize(data['businesses'], sep="_", 
                            record_path="categories", 
                            meta=["name", 
                                  "rating", 
                                  ["location", "zip_code"]], 
                            meta_prefix="biz_")
print(list(bookstores))
---------------------------------------------------------------------------
Combining multiple datasets

pd.concat([df1, df2, df3], axis=0) -> stack dataframes on top of each other
pd.concat([df1, df2, df3], axis=1) -> stack dataframes side by side

merge() -> combine data on a common column
---------------------------------------------------------------------------
# Add an offset parameter to get cafes 51-100
params = {"term": "cafe", 
          "location": "NYC",
          "sort_by": "rating", 
          "limit": 50,
          "offset": 50}

result = requests.get(api_url, headers=headers, params=params)
next_50_cafes = json_normalize(result.json()["businesses"])

# Concatenate the results, setting ignore_index to renumber rows
cafes = pd.concat([top_50_cafes, next_50_cafes], ignore_index=True)

# Print shape of cafes
print(cafes.shape)

---------------------------------------------------------------------------
# Merge crosswalk into cafes on their zip code fields
cafes_with_pumas = cafes.merge(crosswalk, left_on="location_zip_code", right_on="zipcode")
# Merge pop_data into cafes_with_pumas on puma field
cafes_with_pop = cafes_with_pumas.merge(pop_data, on="puma")

# View the data
print(cafes_with_pop.head())