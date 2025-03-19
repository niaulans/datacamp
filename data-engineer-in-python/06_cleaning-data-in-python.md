## [Cleaning Data in Python](https://app.datacamp.com/learn/courses/cleaning-data-in-python)

### Sequence of cleaning data in Python
```
- Data type constraints                           
- Data range constraints                          
- Uniqueness constraints                          
- Consistency constraints                         
- Completeness constraints
```

### Data type constraints
```
Text data   : first name, last name, address
Integers    : age, number of items, products sold
Decimals    : temperature, exchange rate
Binary      : is married, is employed, is present
Dates       : Order dates, ship dates
Categories  : marriage status, employment status, product type
```

### Check data type constraints and convert to correct data type
```python
# Check the data type of each column
sales.dtypes

# Remove $ from Revenue column
sales['Revenue'] = sales['Revenue'].str.strip('$')

# Convert data types (string to integers)
sales['Revenue'] = sales['Revenue'].astype('int')

# Convert numerical data to categorical data
sales['Product'] = sales['Product'].astype('category')

# Verify data type
assert sales['Revenue'].dtype == 'int'
assert sales['Product'].dtype == 'category'  

# Check info of the DataFrame
print(sales.info())

# Check statistics of the DataFrame
print(sales.describe())
```


### Data range constraints
```
How to deal with out of range data:
- Dropping data (depending the size of the dataset), drop data when it is not significant
- Setting custom minimums and maximums
- Treat as missing and impute
- Setting custome value depending on business assumptions
```

### Check data range constraints and convert to correct data range
```python
# Drop values using .drop()
movies.drop(movies[movies['avg_rating'] > 5].index, inplace=True)
assert movies['avg_rating'].max() <= 5

# Convert avg_rating > 5 to 5
movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5

# Change the subscription date to today's date
user_signups['subsription_date'] = pd.to_datetime(user_signups['subsription_date']).dt.date

# Drop values using .drop()
user_signups.drop(user_signups[user_signups['subscription_date'] > today_date].index, inplace=True)
```

```python
# Convert tire_sizes to integer
ride_sharing['tire_sizes'] = ride_sharing['tire_sizes'].astype('int')

# Set all values above 27 to 27
ride_sharing.loc[ride_sharing['tire_sizes'] > 27, 'tire_sizes'] = 27

# Reconvert tire_sizes back to categorical
ride_sharing['tire_sizes'] = ride_sharing['tire_sizes'].astype('category')

# Print tire size description
print(ride_sharing['tire_sizes'].describe())

# Convert ride_date to date
ride_sharing['ride_dt'] = pd.to_datetime(ride_sharing['ride_date']).dt.date

# Save today's date
today = dt.date.today()

# Set all in the future to today's date
ride_sharing.loc[ride_sharing['ride_dt'] > today, 'ride_dt'] = today

# Print maximum of ride_dt column
print(ride_sharing['ride_dt'].max())
```

### Uniqueness constraints

```python
# Get duplicates across all columns
duplicates = height_weight.duplicated()
print(duplicates)

# Get duplicate rows
duplicates = height_weight.duplicated()
height_weight[duplicates]
```

```
The .duplicated() method:
- subset -> column labels to consider
- keep -> whether to keep first, last, or all duplicates
- inplace -> whether to drop duplicates in place
```

### Check uniqueness constraints and drop duplicates
```python
column_names = ['first_name', 'last_name', 'address']
duplicates = height_weight.duplicated(subset=column_names, keep=False) # keep=False to mark all duplicates as True
height_weight[duplicates]

height_weight[duplicates].sort_values(by = 'first_name')
height_weight.drop_duplicates(subset=column_names, inplace=True)
```

# Group by column names and produce statistical summaries
column_names = ['first_name', 'last_name', 'address']
summaries = {'height': 'max', 'weight': 'mean'}
height_weight = height_weight.groupby(column_names).agg(summaries).reset_index()
duplicates = height_weight.duplicated(subset = column_names, keep = False)
height_weight[duplicates].sort_values(by = 'first_name')

---------------------------------------------------------------
# Find duplicates
duplicates = ride_sharing.duplicated(subset = 'ride_id', keep=False)

# Sort your duplicated rides
duplicated_rides = ride_sharing[duplicates].sort_values('ride_id')

# Print relevant columns of duplicated_rides
print(duplicated_rides[['ride_id','duration','user_birth_year']])

# Drop complete duplicates from ride_sharing
ride_dup = ride_sharing.drop_duplicates()

# Create statistics dictionary for aggregation function
statistics = {'user_birth_year': 'min', 'duration': 'mean'}

# Group by ride_id and compute new statistics
ride_unique = ride_dup.groupby('ride_id').agg(statistics).reset_index()

# Find duplicated values again
duplicates = ride_unique.duplicated(subset = 'ride_id', keep = False)
duplicated_rides = ride_unique[duplicates == True]

# Assert duplicates are processed
assert duplicated_rides.shape[0] == 0
---------------------------------------------------------------
Finding inconsistent categories:

inconsistent_categories = set(study_data['blood_type']).difference(categories['blood_type'])
print(inconsistent_categories)

inconsistent_rows = study_data['blood_type'].isin(inconsistent_categories)
study_data[inconsistent_rows]

# Drop inconsistent categories and get consistent data
consistent_data = study_data[~inconsistent_rows]

---------------------------------------------------------------
# Print categories DataFrame
print(categories)

# Print unique values of survey columns in airlines
print('Cleanliness: ', airlines['cleanliness'].unique(), "\n")
print('Safety: ', airlines['safety'].unique(), "\n")
print('Satisfaction: ', airlines['satisfaction'].unique, "\n")

# Find the cleanliness category in airlines not in categories
cat_clean = set(airlines['cleanliness']).difference(categories['cleanliness'])

# Find rows with that category
cat_clean_rows = airlines['cleanliness'].isin(cat_clean)

# Print rows with inconsistent category
print(airlines[cat_clean_rows])

# Print rows with consistent categories only
print(airlines[~cat_clean_rows])
---------------------------------------------------------------
What type of errors could we have?
1. Value inconsistency
2. Collapsign too many categories
3. making sure data is of type category

1. Value inconsistency:
# Get marriage status column
marriage_status = demographics['marriage_status']
marriage_status.value_counts()

# Get value counts on DataFrame
marriage_status.groupby('marriage_status').count()

# Capitalize 
marriage_status['marriage_status'] = marriage_status['marriage_status'].str.upper()
marriage_status['marriage_status'].value_counts()

# lowercase
marriage_status['marriage_status'] = marriage_status['marriage_status'].str.lower()
marriage_status['marriage_status'].value_counts()

# Trailing spaces
marriage_status = demographics['marriage_status']
marriage_status.value_counts()

demographics['marriage_status'] = demographics['marriage_status'].str.strip()
demographics['marriage_status'].value_counts()
---------------------------------------------------------------
2. Collapsing too many categories:
# Using qcut()
import pandas as pd
group_names = ['0-200k', '200k-500k', '500k-1m', '1m+']
demographics['income_group'] = pd.qcut(demographics['household_income'], q=4, labels=group_names)

demographics[['income_group', 'household_income']]

# Using cut()
import pandas as pd
ranges = [0, 200000, 500000, np.inf]
group_names = ['0-200k', '200k-500k', '500k+']
demographics['income_group'] = pd.cut(demographics['household_income'], bins=ranges, labels=group_names)
demographics[['income_group', 'household_income']]

# Mapping categories to fewer oranges
operating systm column is: Microsoft, MacOS, IOS, Android, Linux
operating_system column should become DEsktopOS and MobileOS

mapping = {'Microsoft': 'DesktopOS', 'MacOS': 'DesktopOS', 'Linux': 'DesktopOS', 'IOS': 'MobileOS', 'Android': 'MobileOS'}
devices['operating_system'] = devices['operating_system'].replace(mapping)
devices['operating_system'].unique()
---------------------------------------------------------------
# Print unique values of both columns
print(airlines['dest_region'].unique())
print(airlines['dest_size'].unique())

# Lower dest_region column and then replace "eur" with "europe"
airlines['dest_region'] = airlines['dest_region'].str.lower() 
airlines['dest_region'] = airlines['dest_region'].replace({'eur':'europe'})

# Remove white spaces from `dest_size`
airlines['dest_size'] = airlines['dest_size'].str.strip()

# Verify changes have been effected
print(airlines['dest_region'].unique())
print(airlines['dest_size'].unique())

---------------------------------------------------------------
# Create ranges for categories
label_ranges = [0, 60, 180, np.inf]
label_names = ['short', 'medium', 'long']

# Create wait_type column
airlines['wait_type'] = pd.cut(airlines['wait_min'], bins = label_ranges, 
                                labels = label_names)

# Create mappings and replace
mappings = {'Monday':'weekday', 'Tuesday':'weekday', 'Wednesday': 'weekday', 
            'Thursday': 'weekday', 'Friday': 'weekday', 
            'Saturday': 'weekend', 'Sunday': 'weekend'}

airlines['day_week'] = airlines['day'].replace(mappings)

---------------------------------------------------------------
Cleaning text data

Common text data problems:
- Data inconsistency
  +6282981201 or 0921218
- Fixed length violations
- Typos

# Fixing the phone number column
# Replace "+" with "00"
phone["Phone number"] = phone["Phone number"].str.replace("+", "00")
phones

# Replace "-" with ""
phone["Phone number"] = phone["Phone number"].str.replace("-", "")
phones

# Replace phone numbers with lower than 10 digits with np.nan
digits = phone["Phone number"].str.len()
phone.loc[digits < 10, "Phone number"] = np.nan
phone

# Find length of each phone number
sanity_check = phone["Phone number"].str.len()
assert sanity_check.min() >= 10

# Assert all numbers do not have "+" or "-"
assert phone["Phone number"].str.contains("+|-").any() == False

Assert returns nothing if the condition is true

---------------------------------------------------------------
# Regex to clean text data

# Replace letters with nothing
phone["Phone number"] = phone["Phone number"].str.replace(r'\D+', '')
phone.head()

---------------------------------------------------------------
# Replace "Dr." with empty string ""
airlines['full_name'] = airlines['full_name'].str.replace("Dr.", "")

# Replace "Mr." with empty string ""
airlines['full_name'] = airlines['full_name'].str.replace("Mr.", "")

# Replace "Miss" with empty string ""
airlines['full_name'] = airlines['full_name'].str.replace("Miss", "")

# Replace "Ms." with empty string ""
airlines['full_name'] = airlines['full_name'].str.replace("Ms.", "")

# Assert that full_name has no honorifics
assert airlines['full_name'].str.contains('Ms.|Mr.|Miss|Dr.').any() == False

# Store length of each row in survey_response column
resp_length = airlines['survey_response'].str.len()

# Find rows in airlines where resp_length > 40
airlines_survey = airlines[resp_length > 40]

# Assert minimum survey_response length is > 40
assert airlines_survey['survey_response'].str.len().min() > 40

# Print new survey_response column
print(airlines_survey['survey_response'])
---------------------------------------------------------------
Advanced data problems

1. Uniformity
# Check for uniformity
import matplotlib.pyplot as plt
plt.scatter(x='Date', y='Temperature', data=temperature)
plt.title('Temperature in Celcius March 2019 - NYC')
plt.xlabel('Dates')
plt.ylabel('Temperature in Celcius')
plt.show()

c = (F-32) * 5/9
temp_fah = remperatures.loc[temperatures['Temperature'] > 40, 'Temperature]
temp_cels = (temp_fah - 32) * 59
temperatures.loc[temperatures['Temperature'] > 40, 'Temperature'] = temp_cels

assert temperatures['Temperature].max() < 40

---------------------------------------------------------------
Datetime formatting

pandas.to_datetime() -> convert string to datetime
birthdays['Birthday'] = pd.to_datetime(birthdays['Birthday'], infer_datetime_format=True, errors='coerce')

birthdays['Birthday'] = birthdays['Birthday'].dt.strftime("%d-%m-%Y)

Treating ambigous date data:
Is 2019-03-08 in August or March?
- Convert to NA and treat accordingly
- Infer format by understanding data source

---------------------------------------------------------------
# Find values of acct_cur that are equal to 'euro'
acct_eu = banking['acct_cur'] == 'euro'

# Convert acct_amount where it is in euro to dollars
banking.loc[acct_eu, 'acct_amount'] = banking.loc[acct_eu, 'acct_amount'] * 1.1

# Unify acct_cur column by changing 'euro' values to 'dollar'
banking.loc[acct_eu, 'acct_cur'] = 'dollar'

# Assert that only dollar currency remains
assert banking['acct_cur'].unique() == 'dollar'
---------------------------------------------------------------
# Print the header of account_opend
print(banking['account_opened'].head())

# Convert account_opened to datetime
banking['account_opened'] = pd.to_datetime(banking['account_opened'],
                                           # Infer datetime format
                                           infer_datetime_format = True,
                                           # Return missing value for error
                                           errors = 'coerce') 

# Get year of account opened
banking['acct_year'] = banking['account_opened'].dt.strftime('%Y')

# Print acct_year
print(banking['acct_year'])
---------------------------------------------------------------
Cross field validation
-> The use of multiple fields in a dataset to danity check data integrity

sum_class = flight[['economy_class', 'business_class', 'first_class']].sum(axis=1)
passenger_equ = sum_classes == flight['total_passengers']

inconsistent_pass = flight[~passenger_equ]
consistent_pass = flight[passenger_equ]

# Cross validation age and birth date

import pandas as pd
import datetime as dt

users['Birthday'] = pd.to_datetime(users['Birthday'])
today = dt.date.today()

age_manual = today.year - users['Birthday'].dt.year
age_equ = age_manual == users['Age']
inconsistent_age = users[~age_equ]
consistent_age = users[age_equ]

What to do when we catch inconsistencies?
- Dropping data
- Set to missing and impute
- Apply rules from domain knowledge
---------------------------------------------------------------
# Store fund columns to sum against
fund_columns = ['fund_A', 'fund_B', 'fund_C', 'fund_D']

# Find rows where fund_columns row sum == inv_amount
inv_equ = banking[fund_columns].sum(axis=1) == banking['inv_amount']

# Store consistent and inconsistent data
consistent_inv = banking[inv_equ]
inconsistent_inv = banking[~inv_equ]

# Store consistent and inconsistent data
print("Number of inconsistent investments: ", inconsistent_inv.shape[0])

# Store today's date and find ages
today = dt.date.today()
ages_manual = today.year - banking['birth_date'].dt.year

# Find rows where age column == ages_manual
age_equ = ages_manual == banking['age']

# Store consistent and inconsistent data
consistent_ages = banking[age_equ]
inconsistent_ages = banking[~age_equ]

# Store consistent and inconsistent data
print("Number of inconsistent ages: ", inconsistent_ages.shape[0])
---------------------------------------------------------------
Completeness constraints

Missing data: 
- Missing data can be:
  - NaN
  - NA
  - 0
  - Unknown
  - Not applicable
- Technical or human error

# Return missing values
airquality.isna()
airquality.isna().sum()

Missingno package:

import missingno as msno
import matplotlib.pyplot as plt
msno.matrix(airquality)
plt.show()

# Isolate missing and complete values aside
missing = airquality[airquality['CO2'].isna()]
complete = airquality[~airquality['CO2'].isna()]

sorted_airquality = airquality.sort_values('Temperature')
msno.matrix(sorted_airquality)
plt.show()

Missingness types:
- Missing completely a random (MCAR) -> no relationship between missing data and any values, missing data is random, data entry error
- Missing at random (MAR) -> Systemic relationship between missing data and observed data, missing ozone data for high temperature
- Missing not at random (MNAR) -> Systematic relationship between missing data and unobserved data, missing temperature values for high temperature

How to deal with missing data?
- Drop missing data
- Impute with statistical measures (mean, median, mode)
- Impute using an algorithmic approach, machine learning models

# Drop missing values
airquality_dropped =airquality.dropna(subset=['CO2'])

# Replacing with statistical measures
co2_mean = airquality['CO2'].mean()
airquality_imputed = airquality.fillna({'CO2':co2_mean})

---------------------------------------------------------------
# Print number of missing values in banking
print(banking.isna().sum())

# Visualize missingness matrix
msno.matrix(banking)
plt.show()

# Isolate missing and non missing values of inv_amount
missing_investors = banking[banking['inv_amount'].isna()]
investors = banking[~banking['inv_amount'].isna()]

# Sort banking by age and visualize
banking_sorted = banking.sort_values('age')
msno.matrix(banking_sorted)
plt.show()

# Drop missing values of cust_id
banking_fullid = banking.dropna(subset = ['cust_id'])

# Compute estimated acct_amount
acct_imp = banking_fullid['inv_amount'] * 5

# Impute missing acct_amount with corresponding acct_imp
banking_imputed = banking_fullid.fillna({'acct_amount':acct_imp})

# Print number of missing values
print(banking_imputed.isna().sum())

---------------------------------------------------------------
Record linkage

Comparing strings:
- Minimum edit distance -> Least possible amount of steps needed to transition from one string to another

+ inserting
+ deleting
+ substituting
+ transposing

algorithm:
- Damerau-Levenshtein distance = insertion, deletion, substitution, transposition
- Levenshtein distance = insertion, deletion, substitution
- Hamming distance = substitution
- Jaro distance = transposition

Possible packages: nltk, thefuzz, textdistance

from thefuzz import fuzz
fuzz.WRatio('Reeding', 'Reading')
-> 86

from thefuzz import process
string = "Houston Rockets vs Los Angeles Lakers"
choices = ["Houston Rockets vs Los Angeles Clippers", "Chicago Bulls vs Miami Heat", "Los Angeles Lakers vs Houston Rockets"]
process.extract(string, choices, limit=2)
-> [('Houston Rockets vs Los Angeles Clippers', 95), ('Los Angeles Lakers vs Houston Rockets', 95)]

# For each correct category
for state in categories['state']:
  # Find potential mathes in states with typoes
  matches = process.extract(state, survey['state'], limit = survey.shape[0])

  # for each potential match match 
  for potential_match in matches:
    if potential_match[1] >= 80:
      survey.loc[survey['state'] == potential_match[0], 'state'] = state
  
---------------------------------------------------------------
# Import process from thefuzz
from thefuzz import process

# Store the unique values of cuisine_type in unique_types
unique_types = restaurants['cuisine_type'].unique()

# Calculate similarity of 'asian' to all values of unique_types
print(process.extract('asian', unique_types, limit = len(unique_types)))

# Calculate similarity of 'american' to all values of unique_types
print(process.extract('american', unique_types, limit = len(unique_types)))

# Calculate similarity of 'italian' to all values of unique_types
print(process.extract('italian', unique_types, limit = len(unique_types)))

---------------------------------------------------------------
# Create a list of matches, comparing 'italian' with the cuisine_type column
matches = process.extract('italian', restaurants['cuisine_type'], limit = restaurants.shape[0])

# Inspect the first 5 matches
print(matches[0:5])

# Create a list of matches, comparing 'italian' with the cuisine_type column
matches = process.extract('italian', restaurants['cuisine_type'], limit=len(restaurants.cuisine_type))

# Iterate through the list of matches to italian
for match in matches:
  # Check whether the similarity score is greater than or equal to 80
  if match[1] >= 80 :
    # Select all rows where the cuisine_type is spelled this way, and set them to the correct cuisine
    restaurants.loc[restaurants['cuisine_type'] == match[0], 'cuisine_type'] = 'italian'

# Iterate through categories
for cuisine in categories:  
  # Create a list of matches, comparing cuisine with the cuisine_type column
  matches = process.extract(cuisine, restaurants['cuisine_type'], limit=len(restaurants.cuisine_type))

  # Iterate through the list of matches
  for match in matches:
     # Check whether the similarity score is greater than or equal to 80
    if match[1] >= 80:
      # If it is, select all rows where the cuisine_type is spelled this way, and set them to the correct cuisine
      restaurants.loc[restaurants['cuisine_type'] == match[0]] = cuisine
      
# Inspect the final result
print(restaurants['cuisine_type'].unique())
---------------------------------------------------------------
Generating pairs

Blocking = grouping similar records together

import recordlinkage
indexer = recordlinkage.Index()

# Generate pairs blocked on state
indexer.block('state')
pairs = indexer.index(census_A, census_B)

# Comparing the DataFrames
pairs = indexer.index(census_A, census_B)
compare_cl = recordlinkage.Compare()

compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
compare_cl.exact('state', 'state', label='state')

compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
compare_cl.string('address', 'address', threshold=0.85, label='address')

potential_matches = compare_cl.compute(pairs, census_A, census_B)
potential_matches[potential_matches.sum(axis=1) >= 2]

---------------------------------------------------------------
# Create an indexer and object and find possible pairs
indexer = recordlinkage.Index()

# Block pairing on cuisine_type
indexer.block('cuisine_type')

# Generate pairs
pairs = indexer.index(restaurants, restaurants_new)

# Create a comparison object
comp_cl = recordlinkage.Compare()

# Find exact matches on city, cuisine_types 
comp_cl.exact('city', 'city', label='city')
comp_cl.exact('cuisine_type', 'cuisine_type', label = 'cuisine_type')

# Find similar matches of rest_name
comp_cl.string('rest_name', 'rest_name', label='name', threshold = 0.8) 

# Get potential matches and print
potential_matches = comp_cl.compute(pairs, restaurants, restaurants_new)
print(potential_matches)

---------------------------------------------------------------
Linking DataFrames

matches.index
duplicate_rows = matches.index.get_level_values(1)
print(census_B_index)

census_B_duplicates = census_B[census_B.index.isin(duplicate_rows)]
census_B_new = census_B[~census_B.index.isin(duplicate_rows)]

full_census = census_A.append(census_B_new)

---------------------------------------------------------------
# Isolate potential matches with row sum >=3
matches = potential_matches[potential_matches.sum(axis = 1) >= 3]

# Get values of second column index of matches
matching_indices = matches.index.get_level_values(1)

# Subset restaurants_new based on non-duplicate values
non_dup = restaurants_new[~restaurants_new.index.isin(matching_indices)]

# Append non_dup to restaurants
full_restaurants = restaurants.append(non_dup)
print(full_restaurants)

---------------------------------------------------------
Urutan:
1. Data type constraints
2. Data range constraints
3. Uniqueness constraints
4. Consistency constraints
5. Completeness constraints

# Check dataframe
df.head()
df.info()
df.describe()

1. Data type constraints
Data types:
- text
- integer
- float
- date
- boolean
- categories

# Check data types
df.dtypes

# Change data types
df['column_name'] = df['column_name'].astype('data_type')

# Verify 
assert df['column_name'].dtype == 'data_type'

# Strip white spaces
df['column_name'] = df['column_name'].str.strip()

2. Data range constraints
How to deal with out of range data:
- Dropping data (depending the size of the dataset), drop data when it is not significant
- Setting custom minimums and maximums
- Treat as missing and impute
- Setting custome value depending on business assumptions

# Check data range
df.describe()

# Check for outliers
df['column_name'].plot(kind='box')

# Outlier detection
Q1 = df['column_name'].quantile(0.25)
Q3 = df['column_name'].quantile(0.75)
IQR = Q3 - Q1
min_value = Q1 - 1.5 * IQR
max_value = Q3 + 1.5 * IQR

# Drop data that is out of range
df = df[(df['column_name'] >= min_value) & (df['column_name'] <= max_value)]
df.drop(df[df['column_name'] < min_value].index, inplace=True)
df.drop(df[df['column_name'] > max_value].index, inplace=True)

# Convert max to spesific value
df.loc[df['column_name'] > max_value, 'column_name'] = max_value

3. Uniqueness constraints
How to deal with duplicate data:
- Drop duplicates
- Mark duplicates
- Combine duplicates
- Setting custom value depending on business assumptions

# Check duplicates
df.duplicated().sum()

# Drop duplicates
df.drop_duplicates(inplace=True)
df.drop_duplicates(subset=['column_name'], inplace=True)
df.drop_duplicates(subset=['column_name'], keep='last', inplace=True)

# Mark duplicates
df['is_duplicated'] = df.duplicated()

4. Consistency constraints
How to deal with inconsistent data:
- Standardize data
- Correct data
- Setting custom value depending on business assumptions

# Check consistency
df['column_name'].unique()

# Standardize data
df['column_name'] = df['column_name'].str.lower()
df['column_name'] = df['column_name'].str.upper()

# Correct data
df['column_name'] = df['column_name'].replace('incorrect_value', 'correct_value')

inconsistent_data = set(df['column_name']).difference(standard_data)
df.loc[df['column_name'].isin(inconsistent_data), 'column_name'] = 'correct_value'

5. Completeness constraints
How to deal with missing data:
- Drop data
- Fill missing data
- Setting custom value depending on business assumptions

# Check missing data
df.isnull().sum()

# Fill with statistical value
df['column_name'].fillna(df['column_name'].mean(), inplace=True)
df['column_name'].fillna(df['column_name'].median(), inplace=True)
df['column_name'].fillna(df['column_name'].mode()[0], inplace=True)

# Fill with spesific value
df['column_name'].fillna('spesific_value', inplace=True)

# Drop missing data
df.dropna(inplace=True)

# Drop missing data based on column
df.dropna(subset=['column_name'], inplace=True)

# Drop missing data based on threshold
df.dropna(thresh=2, inplace=True)


axis
✅ "0" untuk ke bawah (vertikal, baris akan diproses)
✅ "1" untuk ke samping (horizontal, kolom akan diproses)

.str.slice(start=n)	Hapus n karakter pertama
.str[n:]	Alternatif .slice(), lebih simpel
.str.lstrip("X")	Hapus karakter spesifik "X" dari kiri

Do reset_index() after:
- dropna()
- fillna()
- drop_duplicates()
- drop()
- groupby()

