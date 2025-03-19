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

### Implementation of list, loop, and conditional statements
```python
# Create an empty list
authors_below_twenty_five = []

# Loop through the authors dictionary
for key, value in authors.items():
  
  # Check for values less than 25
  if value < 25:
    
    # Append the author to the list
    authors_below_twenty_five.append(key)
    
print(authors_below_twenty_five)
```

### Implementation of list, loop, and conditional statements (2)
```python
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
```

### Implementation of list, loop, and conditional statements (3)
```python
# Loop through the dictionary
for genre, sale in genre_sales.items():
  
  # Check if genre is Horror or Mystery
  if genre == "Horror" or genre == "Mystery":
    print(genre, sale)
  
  # Check if genre is Thriller and sale is at least 3 million
  elif genre == "Thriller" and sale >= 3000000:
    print(genre, sale)
```