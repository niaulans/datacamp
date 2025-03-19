## [Introduction to Python for Developers](https://app.datacamp.com/learn/courses/introduction-to-python-for-developers)

### Python function
```python
str.replace(text_to_replace, replacement_text)
str.upper() -> convert to uppercase
str.lower() -> convert to lowercase
"""text"""  -> multiline string
```

### List
```
- Store multiple values in a single variable
- Can contain different data types
- Mutable
- Ordered
- Can use indexing to access elements
- Counts from 0

Accessing elements in a list:
- List = []
- Subsetting = a_list[index]

a_list = [1, 2, 3, 4, 5]
a_list[0]     -> 1
a_list[-1]    -> 5
a_list[1:3]   -> [2, 3] -> 3rd element is not included
a_list[:3]    -> [1, 2, 3] -> 4th element is not included
a_list[3:]    -> [4, 5] -> 3rd element is included
a_list[::2]   -> [1, 3, 5] -> step size of 2
a_list[1::3]  -> [2, 5] -> start from 2nd element and step size of 3
```

### Dictionary
```
- Store key-value pairs
- Mutable
- Ordered
- Can contain different data types

a_dict = {'key1': 'value1', 'key2': 'value2'}

Accessing elements in a dictionary:
- a_dict['key1']            -> 'value1'
- get all values            -> a_dict.values()
- get all keys              -> a_dict.keys()
- get all key-value pairs   -> a_dict.items()
- add a new key-value pair  -> a_dict['key3'] = 'value3'
```

### Set
```
- Store unique values
- Unordered
- Mutable
- Ideal to identify unique elements

Accessing elements in a set:
- a_set = {1, 2, 3, 4, 5}
- convert a list to a set -> set(a_list)
- sorting a set -> sorted(a_set) -> return a list
```

### Tuples
```
- Immutable - cannot change values
- No adding values
- No removing values
- No changing values
- Can subset values by indexing

tuple = (1, 2, 3, 4, 5)
convert a list to a tuple -> tuple(a_list)
tuple[0] -> 1
```

Data structure    | Syntax        | Immutable | Allow duplicates | Ordered | Subset with []
| --- | --- | --- | --- | --- | --- |
List             | [1, 2, 3]      | No        | Yes              | Yes     | Yes
Dictionary       | {key:value}    | No        | Yes              | Yes     | Yes
Set              | {1, 2, 3}      | No        | No               | No      | No
Tuple            | (1, 2, 3)      | Yes       | Yes              | Yes     | Yes

### Conditional statements
```python
# Check if September inflation is less than August inflation
if inflation_september < inflation_august:
  print("Inflation decreased")

# Check if September inflation is more than August inflation
elif inflation_september > inflation_august:
  print("Inflation increased")
  
# Confirm that they are equal
else:
  print("Inflation remained stable")
```

### Loop
```python 
for value in sequence:
    action
```

```
- sequence = list, dictionary, set, tuple
- value = variable that represents the value in the sequence, iterator
```

### Loop through a list
```python
prices = [10, 20, 30, 40, 50]
for price in prices:
    print(price)

for price in prices:
    if price > 10:
        print("More than 10")
    elif price == 10:
        print("Equal to 10")
    else:
        print("Less than 10")
```

### Loop through a string
```python
username = "Datacamp"
for letter in username:
    print(letter)
```

### Loop through a dictionary
```python
a_dict = {'key1': 'value1', 'key2': 'value2'}
for key, value in a_dict.items():
    print(key, value)

for key in a_dict.keys():
    print(key)

for value in a_dict.values():
    print(value)
```

### Remember!!
```
range(start, end + 1, step)
start = inclusive
end = exclusive
```

### Loop through a range
```python
for i in range(1, 6):
    print(i)
-> 1
   2
   3
   4
   5
```

### Loop through a range with a step size
```python
visits = 0
for i in range(1, 11):
    visits += 1 # Add 1 to visits during each iteration
print(visits)
```

### Implementation of a loop
```python
# Create the tickets_sold variable
tickets_sold = 0

# Create the max_capacity variable
max_capacity = 30

# Loop through a range up to and including max_capacity's value
for tickets in range(1, max_capacity + 1):
  
  # Add one to tickets_sold in each iteration
  tickets_sold += 1
  
print("Sold out:", tickets_sold, "tickets sold!")
```

### Implementation of a loop (2)
```python
# Loop through the dictionary's keys and values
for key, value in courses.items():
  
  # Check if the value is "AI"
  if value == "AI":
    print(key, "is an AI course")
  
  # Check if the value is "Programming"
  elif value == "Programming":
    print(key, "is a Programming course")
  
  # Otherwise, print that it is a "Data Engineering" course
  else:
    print(key, "is a Data Engineering course")
```

### While loop
```python
while condition:
    action
```

### Implementation of a while loop
```python
stock = 10
num_purchases = 0

while num_purchases < stock:
    num_purchases += 1
    print("Purchase made")
```

### Remember!!
```
while runs until the condition is False
```

### Implementation of a while loop (2) with a break
```python
while num_purchases < stock:
    print(stock - num_purchases, "items left")

    break -> exit the loop
```

### Implementation of a while loop (3) with if-elif-else
```python
while num_purchases < stock:
    num_purchases += 1
    if stock - num_purchases == 7:
        print("Plenty of items left")
    elif stock - num_purchases == 3:
        print("Only a few items left")
    elif stock - num_purchases == 1:
        print("Last item left")
    else:
        print("No items left")
  ```

### Implementation of a while loop (4) 
```python
release_date = 26
current_date = 22

# Create a conditional loop while current_date is less than or equal to the release_date
while current_date <= release_date
  
  # Increment current_date by one
  current_date += 1
  
  # Promote purchases
  if current_date <= 24:
    print("Purchase before the 25th for early access")
  
  # Check if the date is equal to the 25th
  elif current_date == 25:
    print("Coming soon!")
  else:
    print("Available now!")
```

# Create an empty list
authors_below_twenty_five = []

# Loop through the authors dictionary
for key, value in authors.items():
  
  # Check for values less than 25
  if value < 25:
    
    # Append the author to the list
    authors_below_twenty_five.append(key)
    
print(authors_below_twenty_five)

-------------------------------------------------------------
for genre, average_sale in genre_sales.items():
    
    # Filter for the maximum sales value
    if average_sale == 5166000000:
      
      # Print the genre
      print("Most popular genre: ", genre)
      print("Average sales: ", average_sale)
    
    # Filter for the minimum sales value
    elif average_sale == 80000000:
      
      # Print the genre
      print("Least popular genre: ", genre)
      print("Average sales: ", average_sale)

-------------------------------------------------------------
# Loop through the dictionary
for genre, sale in genre_sales.items():
  
  # Check if genre is Horror or Mystery
  if genre == "Horror" or genre == "Mystery":
    print(genre, sale)
  
  # Check if genre is Thriller and sale is at least 3 million
  elif genre == "Thriller" and sale >= 3000000:
    print(genre, sale)
-------------------------------------------------------------

-------------------------------------------------------------
INTERMEDIATE PYTHON FOR DEVELOPERS                          |
-------------------------------------------------------------

built-in functions:
- len() -> length of a string, list, dictionary, set, tuple
- type() -> data type of a variable
- max() -> maximum value in a list
- min() -> minimum value in a list
- sum() -> sum of values in a list
- sorted() -> sort a list
- reversed() -> reverse a list
- abs() -> absolute value of a number
- round() -> round a number to a specified number of decimal places
- all() -> return True if all elements in an iterable are True
- any() -> return True if any element in an iterable is True
- zip() -> combine two lists into a list of tuples
- enumerate() -> return an index and value of an iterable

Modules:
- Files ending in .py
- Contain functions and attributes
- Can contain other modules
- Python comes with many built-in modules

os -> interpreting and interacting with the operating system
collections -> advanced data structure types and functions
string -> performing string operations
logging -> to log in information when testing or running software
subprocess -> to run system commands

import os
os.getcwd() -> get the current working directory
os.chdir() -> change the current working directory

Module functions: for task 
Module attributes: for information and values

os.environ -> get environment variables
Don't use parentheses when calling attributes

Importing a single function from a module:
from module_name import function_name

from os import getcwd, chdir

-------------------------------------------------------------
# Import the string module
import string 

# Print all ASCII lowercase characters
print(string.ascii_lowercase)

# Print all punctuation
print(string.punctuation)
-------------------------------------------------------------
# Import date from the datetime module
from datetime import date 

# Create a variable called deadline
deadline = date(2024, 1, 19)

# Check the data type
print(type(deadline))

# Print the deadline
print(deadline)

-------------------------------------------------------------
Packages: collection of modules

Module = python file
Packages = library

python3 -m pip install package_name

import pandas as pd
sales = {"item": ["A", "B", "C"], "price": [10, 20, 30]}
sales_df = pd.DataFrame(sales) -> function 
sales_df.head() -> method 

Function = code that performs a specific task
Method = function that belongs to an object

# Read in sales.csv
sales_df = pd.read_csv("sales.csv")

# Print the mean order_value
print(sales_df["order_value"].mean())

# Print the total value of sales
print(sales_df["order_value"].sum())

-------------------------------------------------------------
Defining a custom function:
Considerations when defining a function:
- Number of lines
- Code complexity
- Frequency of use

# Creating a custom function to calculate the average value
def average(values):
    # Calculate the average 
    average_value = sum(values) / len(values)

    # Round the results
    rounded_average = round(average_value, 2)

    return rounded average

sales = [10, 20, 30, 40, 50]
average(sales)

-------------------------------------------------------------
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

-------------------------------------------------------------
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

-------------------------------------------------------------
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

-------------------------------------------------------------
# Define clean_text
def clean_text(text, lower=True):
  if lower == False:
    clean_text = text.replace(" ", "_")
    return clean_text
  else:
    clean_text = text.replace(" ", "_")
    clean_text = clean_text.lower()
    return clean_text

-------------------------------------------------------------
# Define clean_text
def clean_text(text, remove=None):
  if remove != None:
    clean_text = text.replace(remove, "")
    clean_text = clean_text.lower()
    return clean_text
  else:
    clean_text = text.lower()
    return clean_text

-------------------------------------------------------------
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

-------------------------------------------------------------
Docstrings: provide information about the function
help(function_name) -> get information about the function
help(round) -> get information about the round function
round.__doc__ -> get information about the round function

.__doc__ => "dunder doc" attribute

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

-------------------------------------------------------------
def clean_string(text):
  
  # Add a single-line docstring
  """Swap spaces to underscores and convert text to lowercase."""
  
  no_spaces = text.replace(" ", "_")
  clean_text = no_spaces.lower()
  return clean_text

# Access the docstring
print(clean_string.__doc__)

-------------------------------------------------------------
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

-------------------------------------------------------------
Arbitrary arguments-> is a way to pass a variable number of arguments to a function
- Positional arguments
- Keyword arguments

Arbitrary arguments allows you to pass a variable number of arguments to a function

Positional => def average(*args):

* = Convert arguments to a single iterable (tuple)
average(*[15, 29], *[4, 13])

Keyword => def average(**kwargs):

def average(**kwargs):
  average_value = sum(kwargs.values()) / len(kwargs.values())
  rounded_average = round(average_value, 2)
  return rounded_average

average(a=15, b=29, c=4, d=13)

-------------------------------------------------------------
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

-------------------------------------------------------------
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

-------------------------------------------------------------
Lambda functions and error-handling

Lambda functions:
- Lambda keyword -> create an anonymous function

lambda arguments(s): expression
- Convention is to use x for a single argument
- The expression is the equivalent of the function body
- No return statement is required

lambda x: sum(x) / len(x)

- Using lambda functions:
(lambda x: sum(x) / len(x))([10, 20, 30, 40, 50])

average = lambda x: sum(x) / len(x)
average([10, 20, 30, 40, 50])

(lambda x, y: x**y)(2, 3)

Lambda functions with iterables:
- map() -> applies a function to all elements in an iterable
names = ["alice", "bob", "charlie"]
capitalize = map(lambda x: x.capitalize(), names)
print(list(capitalize))

- filter -> filters elements in an iterable
numbers = [1, 2, 3, 4, 5]
even = filter(lambda x: x % 2 == 0, numbers)
print(list(even))

- reduce -> applies a function to all elements in an iterable
from functools import reduce
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)
print(total)

-------------------------------------------------------------
sale_price = 29.99

# Define a lambda function called add_tax
add_tax = lambda x: x * 1.2

# Call the lambda function
print(add_tax(sale_price)))
-------------------------------------------------------------
sale_price = 29.99

# Call a lambda function adding 20% to sale_price
print((lambda x: x * 1.2) (sale_price)) 
-------------------------------------------------------------
sales_prices = [29.99, 9.95, 14.50, 39.75, 60.00]

# Create add_taxes to add 20% to each item in sales_prices
add_taxes = map(lambda x: x * 1.2, sales_prices)

# Use add_taxes to return a new list with updated values
print(list(add_taxes))
-------------------------------------------------------------
Introduction to errors:

What is an error?
- code that violates one or more rules
- Error = exception 
- Cause our code to terminate

TypeError -> unsupported operation. incorrect data type
"Hello" + 5
ValueError -> incorrect value, the value is not acceptable in an operation
int("Hello")
KeyError -> key not found in a dictionary

Error-handling:
except, raise, finally

def average(values):
  try:
    average_value = sum(values) / len(values)
    return average_value
  except:
    print("average() accepts a list or set. Please provide a correct data type")


def average(values):
  if type(values) in ["list", "set"]:
    average_value = sum(values) / len(values)
    return average_value
  else:
    raise TypeError("average() accepts a list or set. Please provide a correct data type")

try-except:
- avoid errors being produced
- still execute subsequent code

raise:
- will produce an error
- avoid executing subsequent code

-------------------------------------------------------------
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

-------------------------------------------------------------
def snake_case(text):
  # Check the data type
  if type(text) == str:
    clean_text = text.replace(" ", "_")
    clean_text = clean_text.lower()
  else:
    # Return a TypeError error if the wrong data type was used
    raise TypeError("The snake_case() function expects a string as an argument, please check the data type provided.")
    
snake_case("User Name 187")