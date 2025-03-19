## [Introduction to Importing Data in Python](https://app.datacamp.com/learn/courses/introduction-to-importing-data-in-python)

### Importing flat files
```python
filename = 'huck_fin.txt'
file = open(filename, mode='r') # r is to read
text = file.read()
file.close()
print(text)
```

### Writing to a file
```python
file = open(filename, mode='w') # w is to write
file.write("I am writing a new line")
file.close()
```

### Importing flat files using context managers
```python
with open(filename, mode='r') as file:
  print(file.read())
```

### Importing flat files using context managers (2)
```python
# Open a file as read-only and bind it to file
with open('moby_dick.txt', 'r') as file:
  	# Print it
    print(file.read())
```

### Read few first lines in flat file
```python
# Read & print the first 3 lines
with open('moby_dick.txt', 'r') as file:
    print(file.readline())
    print(file.readline())
    print(file.readline())
```

### The Zen of Python, by Tim Peters
```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

### Importing flat files using NumPy
```python
import numpy as np
filename = 'MNIST.txt'
data = np.loadtxt(filename, delimiter=',')
data
```

### Customizing NumPy import
```python
import numpy as np
filename = 'MNIST.txt'
data = np.loadtxt(filename, delimiter=',', skiprows=1, dtype=str)
data
```

### Importing csv files using NumPy and plotting
```python
# Import packages
import numpy as np

# Assign filename to variable: file
file = 'digits.csv'

# Load file as array: digits
digits = np.loadtxt(file, delimiter=',')

# Print datatype of digits
print(type(digits))

# Select and reshape a row
im = digits[21, 1:]
im_sq = np.reshape(im, (28, 28))

# Plot reshaped data (matplotlib.pyplot already loaded as plt)
plt.imshow(im_sq, cmap='Greys', interpolation='nearest')
plt.show()
```

### Importing flat files using NumPy
```python
# Import numpy
import numpy as np

# Assign the filename: file
file = 'digits_header.txt'

# Load the data: data
data = np.loadtxt(file, delimiter='\t', skiprows=1, usecols=[0, 2])

# Print data
print(data)
```

### Importing flat file using Numpy and skipping rows
```python
---------------------------------------------------------------
# Assign filename: file
file = 'seaslug.txt'

# Import file: data
data = np.loadtxt(file, delimiter='\t', dtype=str)

# Print the first element of data
print(data[0])

# Import file as floats and skip the first row: data_float
data_float = np.loadtxt(file, delimiter='\t', dtype=float, skiprows=1)

# Print the 10th element of data_float
print(data_float[9])

# Plot a scatterplot of the data
plt.scatter(data_float[:, 0], data_float[:, 1])
plt.xlabel('time (min.)')
plt.ylabel('percentage of larvae')
plt.show()
```

### Importing csv using Pandas
```python
import pandas as pd
filename = 'titanic.csv'
data = pd.read_csv(filename)
data.head()

# Convert to numpy array
data_array = data.to_numpy()
```

### Customizing Pandas import
```python
# Assign the filename: file
file = 'digits.csv'

# Read the first 5 rows of the file into a DataFrame: data
data = pd.read_csv(file, header=None, nrows=5)

# Build a numpy array from the DataFrame: data_array
data_array = data.to_numpy()

# Print the datatype of data_array to the shell
print(type(data_array))
```

### Importing flat file using Pandas (na_values)
```python
# Import matplotlib.pyplot as plt
import matplotlib.pyplot as plt

# Assign filename: file
file = 'titanic_corrupt.txt'

# Import file: data
data = pd.read_csv(file, sep='\t', comment='#', na_values='Nothing')

# Print the head of the DataFrame
print(data.head())

# Plot 'Age' variable in a histogram
pd.DataFrame.hist(data[['Age']])
plt.xlabel('Age (years)')
plt.ylabel('count')
plt.show()
```

### Other file types
```
- Excel
- SAS
- Stata
- HDF5
- MATLAB
- SQL
- Pickled files

Pickle -> Python-specific data format
Pickled files are serialized 
Serialized -> convert object to bytestream
```

### Importing Pickled files
```python
import pickle
with open('pickled_fruit.pkl', 'rb') as file: # rb is read binary
  data = pickle.load(file)
print(data)
```

### Importing Excel files using Pandas
```python
import pandas as pd
file = 'urbanpop.xlsx'
data = pd.ExcelFile(file)
print(data.sheet_names)

df1 = data.parse('1960-1966') # sheet name as a string
df2 = data.parse(0) # sheet index as an integer
```

### Importing Excel files using Pandas (2) -> choose sheet by name, index
```python
---------------------------------------------------------------
# Import pandas
import pandas as pd

# Assign spreadsheet filename: file
file = 'battledeath.xlsx'

# Load spreadsheet: xls
xls = pd.ExcelFile(file)

# Print sheet names
print(xls.sheet_names)

# Load a sheet into a DataFrame by name: df1
df1 = xls.parse('2004')

# Load a sheet into a DataFrame by index: df2
df2 = xls.parse(0)

# Parse the first sheet and rename the columns: df1
df1 = xls.parse(0, skiprows="0", names=['Country', 'AAM due to War (2002)'])
# Parse the first column of the second sheet and rename the column: df2
df2 = xls.parse(1, usecols=[0], skiprows="0", names=['Country'])
```

### Importing SAS and Stata files using Pandas
```
SAS and Statafiles
- SAS   -> statistical analysis system
- Stata -> statistics and data science
```
```python
# SAS files
import pandas as pd
# Import sas7bdat package
from sas7bdat import SAS7BDAT

# Save file to a DataFrame: df_sas
with SAS7BDAT('sales.sas7bdat') as file:
    df_sas = file.to_data_frame()

# Print head of DataFrame
print(df_sas.head())

# Plot histogram of DataFrame features (pandas and pyplot already imported)
pd.DataFrame.hist(df_sas[['P']])
plt.ylabel('count')
plt.show()
```

```python
# Stata files
import pandas as pd

# Load Stata file into a pandas DataFrame: df
df = pd.read_stata('disarea.dta')

# Print the head of the DataFrame df
print(df.head())

# Plot histogram of one column of the DataFrame
pd.DataFrame.hist(df[['disa10']])
plt.xlabel('Extent of disease')
plt.ylabel('Number of countries')
plt.show()
```

### Importing HDF5 files
```
HDF5 files
- Hierarchical Data Format version 5
- Standard for storing large quantities of numerical data
```

```python
import h5py
filename = 'LIGO_data.hdf5'
data = h5py.File(filename, 'r')
print(type(data))

for key in data.keys():
  print(key)

for key in data['meta].keys():
  print(key)

print(np.array(data['meta']['Description']))
```

```python
# Import packages
import numpy as np
import h5py

# Assign filename: file
file = 'LIGO_data.hdf5'

# Load file: data
data = h5py.File(file, 'r')

# Print the datatype of the loaded file
print(type(data))

# Print the keys of the file
for key in data.keys():
    print(key)

# Get the HDF5 group: group
group = data['strain']

# Check out keys of group
for key in group.keys():
    print(key)

# Set variable equal to time series data: strain
strain = np.array(data['strain']['Strain'])

# Set number of time points to sample: num_samples
num_samples = 10000

# Set time vector
time = np.arange(0, 1, 1/num_samples)

# Plot data
plt.plot(time, strain[:num_samples])
plt.xlabel('GPS Time (s)')
plt.ylabel('strain')
plt.show()
```

### Importing MATLAB files
```
MATLAB files
- Matrix Laboratory
- Numerical computing environment

scipy.io.loadmat() -> read .mat files
scipy.io.savemat() -> write .mat files
```

```python
import scipy.io
filename = 'workspace.mat'
mat = scipy.io.loadmat(filename)
print(type(mat))

keys = matlab variable names
values = objects assigned to variables

# Import package
import scipy.io

# Load MATLAB file: mat
mat = scipy.io.loadmat('albeck_gene_expression.mat')

# Print the datatype type of mat
print(type(mat))

# Print the keys of the MATLAB dictionary
print(mat.keys())

# Print the type of the value corresponding to the key 'CYratioCyt'
print(type(mat['CYratioCyt']))

# Print the shape of the value corresponding to the key 'CYratioCyt'
print(np.shape(mat['CYratioCyt']))

# Subset the array and plot it
data = mat['CYratioCyt'][25, 5:]
fig = plt.figure()
plt.plot(data)
plt.xlabel('time (min.)')
plt.ylabel('normalized fluorescence (measure of expression)')
plt.show()
```


### Working with relational databases in Python
```
Creating a database engine
- SQLite database
- SQLAlchemy -> SQL toolkit and Object-Relational Mapping (ORM) library
```

```python
# Import necessary module
from sqlalchemy import create_engine

# Create engine: engine
engine = create_engine('sqlite:///Chinook.sqlite')

# Save the table names to a list: table_names
table_names = engine.table_names()

# Print the table names to the shell
print(table_names)
```

### Querying relational databases in Python (select *)
```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Northwind.sqlite')
con = engine.connect()

rs = con.execute('SELECT * FROM Orders')
df = pd.DataFrame(rs.fetchall())
df.columns = rs.keys()
con.close()
```

### Querying relational databases in Python (select few rows)
```python
from sqlalchemy import create_engine
import pandas as pd
engine = create_engine('sqlite:///Northwind.sqlite')

with engine.connect() as con:
  rs = con.execute("SELECT OrderID, OrderDate, ShipName FROM Orders")
  df = pd.DataFrame(rs.fetchmany(size=5))
  df.columns = rs.keys()
```

### Querying relational databases in Python (filtering)
```python
# Import packages
from sqlalchemy import create_engine
import pandas as pd

# Create engine: engine
engine = create_engine('sqlite:///Chinook.sqlite')

# Execute query and store records in DataFrame: df
df = pd.read_sql_query("SELECT * FROM Album", engine)

# Print head of DataFrame
print(df.head())

# Open engine in context manager and store query result in df1
with engine.connect() as con:
    rs = con.execute("SELECT * FROM Album")
    df1 = pd.DataFrame(rs.fetchall())
    df1.columns = rs.keys()

# Confirm that both methods yield the same result
print(df.equals(df1))
```

### Querying relational databases in Python (join)
```python
with engine.connect() as con:
    rs = con.execute("SELECT Title, Name FROM Album INNER JOIN Artist ON Album.ArtistID = Artist.ArtistID")
    df = pd.DataFrame(rs.fetchall())
    df.columns = rs.keys()

# Print head of DataFrame df
print(df.head())
```
