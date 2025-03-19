## [Intermediate Python for Developers](https://app.datacamp.com/learn/courses/intermediate-python-for-developers)

### Built-in functions:
```
- len()       -> length of a string, list, dictionary, set, tuple
- type()      -> data type of a variable
- max()       -> maximum value in a list
- min()       -> minimum value in a list
- sum()       -> sum of values in a list
- sorted()    -> sort a list
- reversed()  -> reverse a list
- abs()       -> absolute value of a number
- round()     -> round a number to a specified number of decimal places
- all()       -> return True if all elements in an iterable are True
- any()       -> return True if any element in an iterable is True
- zip()       -> combine two lists into a list of tuples
- enumerate() -> return an index and value of an iterable
```

### Modules 
```
- Files ending in .py
- Contain functions and attributes
- Can contain other modules
- Python comes with many built-in modules

os          -> interpreting and interacting with the operating system
collections -> advanced data structure types and functions
string      -> performing string operations
logging     -> to log in information when testing or running software
subprocess  -> to run system commands
```

### Get the current working directory and change it
```python
import os
os.getcwd() # get the current working directory
os.chdir() # change the current working directory
```

### Remember!!
```
Module functions: for task 
Module attributes: for information and values

Don't use parentheses when calling attributes
```

### Importing a single function from a module
```python
from module_name import function_name

from os import getcwd, chdir
```

### String module
```python
# Import the string module
import string 

# Print all ASCII lowercase characters
print(string.ascii_lowercase)

# Print all punctuation
print(string.punctuation)
```

### Date module
```python
# Import date from the datetime module
from datetime import date 

# Create a variable called deadline
deadline = date(2024, 1, 19)

# Check the data type
print(type(deadline))

# Print the deadline
print(deadline)
```

### Packages
```
Packages -> collection of modules

Module = python file
Packages = library
```

### Installing packages
```bash
python3 -m pip install package_name
```

### Pandas module
```python
import pandas as pd

sales = {"item": ["A", "B", "C"], "price": [10, 20, 30]}
sales_df = pd.DataFrame(sales) # function 
sales_df.head() # method 

# Read in sales.csv
sales_df = pd.read_csv("sales.csv")

# Print the mean order_value
print(sales_df["order_value"].mean())

# Print the total value of sales
print(sales_df["order_value"].sum())
```

### Remember!!
```
Function = code that performs a specific task
Method = function that belongs to an object
```

### Defining a custom function
```
Considerations when defining a function:
- Number of lines
- Code complexity
- Frequency of use
```

### Creating a custom function
```python
# Creating a custom function to calculate the average value
def average(values):
    # Calculate the average 
    average_value = sum(values) / len(values)

    # Round the results
    rounded_average = round(average_value, 2)

    return rounded average

sales = [10, 20, 30, 40, 50]
average(sales)
```

### Cleaning data function
```python
# Create the clean_string function
def clean_string(text):
  
  # Replace spaces with underscores
  no_spaces = text.replace(" ", "_")
  
  # Convert to lowercase
  clean_text = no_spaces.lower()
  
  # Return the final text as an output
  return clean_text

converted_text = clean_string("I LoVe dATaCamP!")
print(converted_text)
```

### Password checker function
```python
password = "not_very_secure_2023"

# Define the password_checker function
def password_checker(submission):
  
  # Check that the password variable and the submission match
  if password == submission:
    print("Successful login!")
  
  # Otherwise, print "Incorrect password"
  else:
    print("Incorrect password")

# Call the function    
password_checker("NOT_VERY_SECURE_2023")
```

### Arguments in functions
```
- Arguments are values provided to a function or method
- Function and methods have two types of arguments:
    - Positional arguments
    - Keyword arguments
  
Positional arguments:
- Provide arguments in order, separated by commas
Ex: round(3.14455555, 2)

Keyword arguments:
- Provide arguments by assigning values to keywords
- Useful for interpretation and tracking arguments
- keyword = value
Ex: round(number=3.14455555, ndigits=2)

None == no value/empty
Default argument: way of setting a default value for an argument
```

### Function with default arguments
```python
def average(values, rounded=False):
  if rounded == True:
    average_value = sum(values) / len(values)
    rounded_average = round(average_value, 2)
    return rounded_average
  else:
    average_value = sum(values) / len(values)
    return average_value

sales = [10, 20, 30, 40, 50]
average(sales, rounded=False)
```

### Cleaning text function
```python
# Define clean_text
def clean_text(text, remove=None):
  if remove != None:
    clean_text = text.replace(remove, "")
    clean_text = clean_text.lower()
    return clean_text
  else:
    clean_text = text.lower()
    return clean_text
```

### Convert data structure function
```python
# Create the convert_data_structure function
def convert_data_structure(data, data_type="list"):
  
  # If data_type is "tuple"
  if data_type == "tuple":
    data = tuple(data)
  
  # Else if data_type is set, convert to a set
  elif data_type == "set":
    data = set(data)
  else:
    data = list(data)
  return data

# Call the function to convert to a set
convert_data_structure({"a", 1, "b", 2, "c", 3}, data_type="set")
```

### Docstrings
```
Docstrings: provide information about the function

help(function_name) -> get information about the function
help(round)         -> get information about the round function
round.__doc__       -> get information about the round function

.__doc__ => "under doc" attribute
```

### Docstrings in functions
```python
def average(values, rounded=False):
  """
  Calculate the average of a list of values
  
  Parameters:
  values: list of numerical values
  rounded: boolean, round the average to two decimal places
  
  Returns:
  average_value: float, the average of the values
  """
  if rounded == True:
    average_value = sum(values) / len(values)
    rounded_average = round(average_value, 2)
    return rounded_average
  else:
    average_value = sum(values) / len(values)
    return average_value

```

### Cleaning text function with docstring
```python
def clean_string(text):
  
  # Add a single-line docstring
  """Swap spaces to underscores and convert text to lowercase."""
  
  no_spaces = text.replace(" ", "_")
  clean_text = no_spaces.lower()
  return clean_text

# Access the docstring
print(clean_string.__doc__)
```

### Convert data structure function with docstring
```python
# Create the convert_data_type function
def convert_data_structure(data, data_type="list"):
  # Add a multi-line docstring
  """
  Convert a data structure to a list, tuple, or set.
  
  Args:
  	data (list, tuple, or set): A data structure to be converted.
    data_type (str): String representing the type of structure to convert data to.
    
  Returns:
  	data (list, tuple, or set): Converted data structure.
  """
  if data_type == "tuple":
    data = tuple(data)
  elif data_type == "set":
    data = set(data)
  else:
    data = list(data)
  return data

print(help(convert_data_structure))
```


### Arbitrary arguments
```
Arbitrary arguments-> is a way to pass a variable number of arguments to a function
- Positional arguments
- Keyword arguments

Arbitrary arguments allows you to pass a variable number of arguments to a function

Positional => def average(*args)
* = Convert arguments to a single iterable (tuple)
average(*[15, 29], *[4, 13])

Keyword => def average(**kwargs)
def average(**kwargs):
  average_value = sum(kwargs.values()) / len(kwargs.values())
  rounded_average = round(average_value, 2)
  return rounded_average

average(a=15, b=29, c=4, d=13)
```

### Positional arbitrary arguments
```python
# Define a function called concat
def concat(*args):
  
  # Create an empty string
  result = ""
  
  # Iterate over the Python args tuple
  for arg in args:
    result += " " + arg
  return result

# Call the function
print(concat("Python", "is", "great!"))
```

### Keyword arbitrary arguments
```python
# Define a function called concat
def concat(**kwargs):
  
  # Create an empty string
  result = ""
  
  # Iterate over the Python kwargs
  for kwarg in kwargs.values():
    result += " " + kwarg
  return result

# Call the function
print(concat(start="Python", middle="is", end="great!"))
```

### Lambda functions
```
Lambda functions
- Lambda keyword -> create an anonymous function

lambda arguments(s): expression
- Convention is to use x for a single argument
- The expression is the equivalent of the function body
- No return statement is required

lambda x: sum(x) / len(x)
```

### Lambda functions 
```python
(lambda x: sum(x) / len(x))([10, 20, 30, 40, 50])
```

### Lambda functions(2)
```python
average = lambda x: sum(x) / len(x)
average([10, 20, 30, 40, 50])

(lambda x, y: x**y)(2, 3)
```

### Lambda functions with map()
```python
# map() -> applies a function to all elements in an iterable
names = ["alice", "bob", "charlie"]
capitalize = map(lambda x: x.capitalize(), names)
print(list(capitalize))
```

### Lambda functions with filter()
```python
# filter -> filters elements in an iterable
numbers = [1, 2, 3, 4, 5]
even = filter(lambda x: x % 2 == 0, numbers)
print(list(even))
```

### Lambda functions with reduce()
```python
# reduce -> applies a function to all elements in an iterable
from functools import reduce
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)
print(total)
```

### Implementation of lambda functions
```python
sale_price = 29.99

# Define a lambda function called add_tax
add_tax = lambda x: x * 1.2

# Call the lambda function
print(add_tax(sale_price)))
```

### Implementation of lambda functions(2)
```python
sale_price = 29.99

# Call a lambda function adding 20% to sale_price
print((lambda x: x * 1.2) (sale_price)) 
```

### Implementation of lambda functions with map()
```python
sales_prices = [29.99, 9.95, 14.50, 39.75, 60.00]

# Create add_taxes to add 20% to each item in sales_prices
add_taxes = map(lambda x: x * 1.2, sales_prices)

# Use add_taxes to return a new list with updated values
print(list(add_taxes))
```

### Error handling
```
Error handling -> process of managing errors that occur in a program

What is an error?
- code that violates one or more rules
- Error = exception 
- Cause our code to terminate

TypeError   -> unsupported operation. incorrect data type
"Hello" + 5
ValueError -> incorrect value, the value is not acceptable in an operation
int("Hello")
KeyError  -> key not found in a dictionary

Error-handling:
except, raise, finally
```

### Error handling with try-except
```python
def average(values):
  try:
    average_value = sum(values) / len(values)
    return average_value
  except:
    print("average() accepts a list or set. Please provide a correct data type")
```

### Error handling with raise
```python
def average(values):
  if type(values) in ["list", "set"]:
    average_value = sum(values) / len(values)
    return average_value
  else:
    raise TypeError("average() accepts a list or set. Please provide a correct data type")
```

### Remember!!
```
try-except:
- avoid errors being produced
- still execute subsequent code

raise:
- will produce an error
- avoid executing subsequent code
```

### Implementation of error handling
```python
def snake_case(text):
  # Attempt to clean the text
  try:
    # Swap spaces for underscores
    clean_text = text.replace(" ", "_")
    clean_text = clean_text.lower()
  # Run this code if an error occurs
  except:
    print("The snake_case() function expects a string as an argument, please check the data type provided.")
    
snake_case("User Name 187")
```

### Implementation of error handling with raise
```python
def snake_case(text):
  # Check the data type
  if type(text) == str:
    clean_text = text.replace(" ", "_")
    clean_text = clean_text.lower()
  else:
    # Return a TypeError error if the wrong data type was used
    raise TypeError("The snake_case() function expects a string as an argument, please check the data type provided.")
    
snake_case("User Name 187")
```