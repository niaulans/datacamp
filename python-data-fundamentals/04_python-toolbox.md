## [Python Toolbox](https://app.datacamp.com/learn/courses/python-toolbox)

# %% [markdown]
# ## Writing your own functions

# %% [markdown]
# ### User-defined functions

# %% [markdown]
# - Define functions without parameters
# - Define functions with one parameter
# - Define functions that return a value
# - Later : multiple arguments, multiple return values

# %%
# Built-in functions

x = str(5)

# %%
x

# %%
type(x)

# %% [markdown]
# #### Defining a function

# %%
def square():   # <- Function header
    new_value = 4 ** 2   # <- Function body
    print(new_value)

# %%
square()

# %% [markdown]
# #### Function parameters

# %%
def square(value):
    new_value = value ** 2
    print(new_value)
    
square(4)

# %%
square(10)

# %% [markdown]
# #### Return values from functions

# %% [markdown]
# - Return a value from a function using return

# %%
def square(value):
    new_value = value ** 2
    return new_value

num = square(11)

# %%
num

# %% [markdown]
# #### Docstring

# %% [markdown]
# - Docstring menjelaskan apa yang dikerjakan oleh function
# - Sebagai documentation fungsi

# %%
def square(value):
    """Komentar"""
    new_value = value ** 2
    return new_value

# %%
#Simple function

def shout():
    shout_word = 'congratulations' + '!!!'
    print(shout_word)
    
shout()

# %%
#Function dengan satu parameter

def shout(word):
    shout_word = word + '!!!'
    print(shout_word)
    
shout('congratulations')

# %%
#Function dengan single return value

def shout(word):
    shout_word = word + '!!!'
    return shout_word
    
yell = shout('Selamat')
print(yell)

# %%
type(yell)

# %% [markdown]
# ### Multiple parameters and return values

# %%
def raise_to_power(value1, value2):
    """Raise value1 to the power of value2."""
    new_value = value1 ** value2
    return new_value

result = raise_to_power(20, 2)

# %%
result

# %% [markdown]
# #### Return multiple values with Tuples

# %% [markdown]
# Tuples : 
# - seperti list - dapat mengandung banyak nilai 
# - immutable - nilainya tidak dapat dimodifikasi
# - menggunakan ()

# %%
even_nums = (2, 4, 6)

print(type(even_nums))

# %% [markdown]
# #### Unpacking tuples

# %%
even_nums = (2, 4, 6)

even_nums = a, b, c

# %%
for x in even_nums:
    print(x)

# %%
even_nums[2]

# %%
#Unpack tuple
nums = (3, 4, 5)

num1, num2, num3 = nums
even_nums = (2, num2, num3)

# %%
even_nums

# %%
def raise_both(value1, value2):
    """Raise valeue1 to the power of value2 and vice verse"""
    
    new_value1 = value1 ** value2
    new_value2 = value2 ** value1
    
    new_tuple = (new_value1, new_value2)
    
    return new_tuple

raise_both(2,3)

# %%
def shout(word1, word2):
    
    shout1 = word1 + '!!!'
    shout2 = word2 + '!!!'
    new_shout = shout1 + shout2
    
    return new_shout

yell = shout('Selamat ', ' Nia ')
print(yell)

# %%
def shout_all(word1, word2):
    
    shout1 = word1 + '!!!'
    shout2 = word2 + '!!!'
    shout_words = (shout1, shout2)
    
    return shout_words

yell1, yell2 = shout_all('Selamat', 'Nia')

# %%
print(yell1)

# %% [markdown]
# #### Exercise 

# %%
#Tanpa function
import pandas as pd

df = pd.read_csv("tweets.csv")

langs_count = {}
col = df['lang']

for entry in col:
    if entry in langs_count.keys():
        langs_count[entry] += 1
    else :
        langs_count[entry] = 1
        
print(langs_count)

# %%
#Dengan function
import pandas as pd 

tweets_df = pd.read_csv('tweets.csv')

def count_countries(df, col_name):
    """Return a dict with counts of occurrences as value for each key."""
    
    langs_count = {}
    col = df[col_name]
    
    for entry in col:
        if entry in langs_count.keys():
            langs_count[entry] += 1
        else:
            langs_count[entry] = 1
    
    return langs_count

result = count_countries(tweets_df, 'lang')
print(result)

# %% [markdown]
# ## Default arguments, variable-length arguments and scope

# %% [markdown]
# ### Scope and user-defined functions

# %% [markdown]
# - Tidak semua object dapat diakses di keseluruhan script
# - Scope = bagian program dimana sebuah object dan variabel dapat diakses
#     - Global scope : dedifinisikan dalam body script
#     - Local scope : didefinisikan di dalam function
#     - Built-in scope : didefinisikan di dalam pre-defined built-ins module

# %% [markdown]
# ### Global vs local scope

# %%
def square(value):
    """Return the square of a number"""
    new_val = value ** 2 #local scope, tidak global
    return new_val

square(3)

# %%
new_val

# %%
new_val = 10 #Global scope

def square(value):
    """Return the square fof a number"""
    new_val = value ** 2 #local scope, tidak global
    return new_val

square(3)

# %%
new_val

# %%
new_val = 10 #Global scope

def square(value):
    """Return the square fof a number"""
    new_val2 = new_val ** 2 #local scope, tidak global
    return new_val2

square(3)

# %%
new_val = 10 #Global scope

def square(value):
    """Return the square fof a number"""
    global new_val
    new_val = new_val ** 2 #local scope, tidak global
    return new_val

square(3)

# %%
new_val

# %%
num = 5
def func1():
    num = 3
    print(num)
func1()

# %%
num = 5

def func2():
    global num
    double_num = num * 2
    num = 6
    print(double_num)
func2()

# %%
num

# %%
team = "teen titans"

def change_team():
    
    global team
    team = "justice league"

print(team)

# %%
change_team()
team

# %% [markdown]
# ### Python's built-in scope

# %%
import builtins

# %%
'tuple' in dir(builtins)

# %% [markdown]
# ### Nested Functions

# %%
def outer(...):
    """..."""
    x = ...
    
    def inner(...):
        """..."""
        y = x ** 2
    return ...

# %%
def mod2plus5(x1, x2, x3):
    """Return the remainder plus 5 of three values."""
    
    new_x1 = x1 % 2 + 5
    new_x2 = x2 % 2 + 5
    new_x3 = x3 % 2 + 5
    
    return (new_x1, new_x2, new_x3)

# %% [markdown]
# Code diatas tidak efisien jika kita ingin menjalankan komputasi berulang kali

# %%
def mod2plus5(x1, x2, x3):
    """Returns the remainder plus 5 of three values."""
    
    def inner(x):
        """Returns the remainder plus 5 of a value."""
        return x % 2 + 5
    
    return (inner(x1), inner(x2), inner(x3))

# %%
mod2plus5(1, 2, 3)

# %%
def raise_val(n):
    """Return the inner function"""
    
    def inner(x):
        """Raise x to the power of n."""
        raised = x ** n
        return raised
    
    return (inner(n))

# %%
raise_val(2)

# %%
square = raise_val(2) #isi parameter ini = n
cube = raise_val(3)
print(square(2), cube(2)) #isi parameter ini x

# %%
def outer():
    """Prints the value of n."""
    n = 1
    
    def inner():
        nonlocal n
        n = 2
        print(n)
        
    inner()
    print(n)

# %%
outer()

# %% [markdown]
# #### Scopes searched

# %% [markdown]
# - Local scope
# - Enclosing functions
# - Global 
# - Built-in

# %%
def three_shouts(word1, word2, word3):
    """Returns a tuple of strings concatened with !!!"""
    
    def inner(word):
        """Returns a strings concatenend with !!!"""
        return word + "!!!"
    
    return(inner(word1), inner(word2), inner(word3))

# %%
print(three_shouts("Nia ","Ulan ", "Sari"))

# %%
def echo(n):
    
    def inner_echo(word):
        echo_word = word * n
        return echo_word
    
    return inner_echo

twice = echo(2)
thrice = echo(3)

# %%
twice("Hello ")

# %%
# DEefine echo_shout()

def echo_shout(word):
    """Change the value of a nonlocal variable"""

    #Concatenate word with itself : echo_word
    echo_word = word + word

    #Print eco_word 
    print(echo_word)

    #Define inner function shout()
    def shout():
        """Alter a variable in the enclosing scope"""
        #Use echo_word in ninlocal scope
        nonlocal echo_word

        #Change echo_word to echo_word concatenated with '!!!'
        echo_word = echo_word + '!!!'

    shout()
    print(echo_word)

echo_shout('Hello')


# %% [markdown]
# ### Default and flexible arguments

# %%
def power(number, pow=1):
    """raise number to the power of pow."""
    new_value = number ** pow
    return new_value

print(power(9, 2)) # kalau kita mengganti nilai parameter kedua disini maka function akan menggunakan nilai yang
#disini, bukan di atas
print(power(9)) # Jika parameer kedua kita kosongkan, maka function akan memanggil default nilai parameter kedua yang telah di inisialisasi di awal

# %%
#Parameter *args digunakan untuk membaca semua parameter di function
def tambahin_semuanya(*args):

    total_semua = 0

    for angka in args:
        total_semua += angka

    return total_semua

tambahin_semuanya(1, 3, 4, 5)

# %%
def print_all(**kwargs):
    """Print out_key-value pairs in **kwargs."""

    #Print out the key-value pairs
    for key, value in kwargs.items():
        print(key, ":", value)

# %%
print_all(name = "Nia", job = "Student")

# %%
def shout_echo(word1, echo=1):
    
    echo_word = word1 * echo
    shout_word = echo_word + '!!!'

    return shout_word

no_echo = shout_echo("Hey")
with_echo = shout_echo("Hey", echo=5)

print(no_echo)
print(with_echo)

# %%
def shout_echo(word1, echo=1, intense=False):

    echo_word = word1 * echo

    if intense is True:
        echo_word_new = echo_word.upper() + '!!!'
    else:
        echo_word_new = echo_word + '!!!'

    return echo_word_new
        
with_big_echo = shout_echo("Hey", 5, True)
big_no_echo = shout_echo("Hey", True)

print(with_big_echo)
print(big_no_echo)

# %%
def gibberish(*args):

    hodgepodge = ""

    for word in args:
        hodgepodge += word

    return hodgepodge

one_word = gibberish("luke")
many_words = gibberish("luke", "leia", "han", "obi", "darth")

print(one_word)
print(many_words)


# %%
# Define report_status
def report_status(**kwargs):
    """Print out the status of a movie character."""

    print("\nBEGIN: REPORT")
    print("---------------")

    # Iterate over the key-value pairs of kwargs
    for x, y in kwargs.items():
        # Print out the keys and values, separated by a colon ':'
        print(x + ": " + y)

    print("END REPORT")
    print("---------------")
    

# First call to report_status()
report_status(name="luke", affiliation="jedi", status="missing")

# Second call to report_status()
report_status(name="anakin", affiliation="sith lord", status="deceased")

# %%
import pandas as pd

tweets_df = pd.read_csv("tweets.csv")

def count_entries(df, col_name):

    cols_count = {}
    col = df[col_name]

    for entry in col:
        if entry in cols_count.keys():
            cols_count[entry] += 1
        else:
            cols_count[entry] = 1

    return cols_count

result1 = count_entries(tweets_df, 'lang')
result2 = count_entries(tweets_df, 'source')

print(result1)
print(result2)

# %%
import pandas as pd

tweets_df = pd.read_csv("tweets.csv")

# Define count_entries()
def count_entries(df, *args):
    """Return a dictionary with counts of
    occurrences as value for each key."""
    
    #Initialize an empty dictionary: cols_count
    cols_count = {}
    
    # Iterate over column names in args
    for col_name in args:
    
        # Extract column from DataFrame: col
        col = df[col_name]
    
        # Iterate over the column in DataFrame
        for entry in col:
    
            # If entry is in cols_count, add 1
            if entry in cols_count.keys():
                cols_count[entry] += 1
    
            # Else add the entry to cols_count, set the value to 1
            else:
                cols_count[entry] = 1

    # Return the cols_count dictionary
    return cols_count

# Call count_entries(): result1
result1 = count_entries(tweets_df, 'lang')

# Call count_entries(): result2
result2 = count_entries(tweets_df, 'lang', 'source')

# Print result1 and result2
print(result1)
print(result2)

# %% [markdown]
# ## Lambda functions and error-handling

# %% [markdown]
# ### Lambda functions

# %% [markdown]
# Lambda function merupakan cara tercepat untuk mmebuat function dengan menggunakan keyword lambda

# %%
raise_to_power = lambda x, y : x ** y

raise_to_power(2, 3)

# %% [markdown]
# #### Map functions / anonymous functions

# %% [markdown]
# - Function map mempunyai 2 argument , map (func, sqeq)
# - map() mengaplikasikan function ke semua elemen dalam sequence

# %%
nums = [48, 6, 9, 21, 1]

square_all = map(lambda num: num ** 2, nums)

print(square_all)
print(list(square_all))

# %%
add_bangs = lambda a: a + '!!!'

add_bangs("hello")

# %%
echo_word = lambda word1, echo : word1 * echo

result = echo_word('hey', 5)
print(result)

# %%
spells = ["protego", "accio", "expecto patronum", "legilimens"]

shout_spells = map(lambda item: item + '!!!', spells)

shout_spells_list = list(shout_spells)

print(shout_spells_list)

# %% [markdown]
# #### Filter functions

# %% [markdown]
# - Filter functions digunakan untuk memfilter element dari list yang tidak mengikuti kriteria

# %%
fellowship = ['frodo', 'samwise', 'merry', 'pippin', 'aragorn', 'boromir', 'legolas', 'gimli', 'gandalf']

result = filter(lambda item : len(item) > 6, fellowship)

result_list = list(result)

print(result_list)


# %% [markdown]
# #### Reduce functions

# %% [markdown]
# - Reduce functions digunakan untuk komputasi pada list.

# %%
from functools import reduce

stark = ['robb', 'sansa', 'arya', 'brandon', 'rickon']

result = reduce(lambda item1, item2 : item1 + item2, stark)

print(result)

# %% [markdown]
# ### Error handling

# %%
float(5)

# %%
float('3.5')

# %%
float('hello')

# %%
def sqrt(x):
    """Returns the square root of a number."""
    return x ** (0.5)

sqrt(4)

# %%
sqrt('hello')

# %% [markdown]
# #### Errors and Exceptions

# %% [markdown]
# - Exceptions - ditangap selama eksekusi
# - Tangkap exceptions dengan tr-except clause

# %%
def sqrt(x):
    """Returns the square root of a number."""
    try:
        return x ** 0.5
    except:
        print("x must be an int or float")

# %%
sqrt(5)

# %%
sqrt('nia')

# %%
def sqrt(x):
    """Returns the square root of a number."""
    try:
        return x ** 0.5
    except TypeError:
        print("x must be an int or float")

# %%
sqrt('nia')

# %%
sqrt(-9)

# %%
def sqrt(x):
    """returns the square root of a number."""
    if x < 0:
        raise ValueError("x must be non-negative")
    try:
        return x ** 0.5
    except TypeError:
        print("s must be an int or float")

# %%
sqrt(-9)

# %%
print(len('There is a beast in every man and it stirs when you put a sword in his hand.'))
print(len(['robb', 'sansa', 'arya', 'eddard', 'jon']))
print(len(525600))
print(len(('jaime', 'cersei', 'tywin', 'tyrion', 'joffrey')))

# %%
# Define shout_echo
def shout_echo(word1, echo=1):
    """Concatenate echo copies of word1 and three
    exclamation marks at the end of the string."""

    # Initialize empty strings: echo_word, shout_words
    echo_word = ""
    shout_words = ""


    # Add exception handling with try-except
    try:
        # Concatenate echo copies of word1 using *: echo_word
        echo_word = word1 * echo

        # Concatenate '!!!' to echo_word: shout_words
        shout_words = echo_word + '!!!'
    except:
        # Print error message
        print("word1 must be a string and echo must be an integer.")

    # Return shout_words
    return shout_words

# Call shout_echo
shout_echo("particle", echo="accelerator")

# %%
# Define shout_echo
def shout_echo(word1, echo=1):
    """Concatenate echo copies of word1 and three
    exclamation marks at the end of the string."""

    # Raise an error with raise
    if echo < 0:
        raise ValueError("echo must be greater than or equal to 0")

    # Concatenate echo copies of word1 using *: echo_word
    echo_word = word1 * echo

    # Concatenate '!!!' to echo_word: shout_word
    shout_word = echo_word + '!!!'

    # Return shout_word
    return shout_word

# Call shout_echo
shout_echo("particle", echo=5)

# %%
import pandas as pd

tweets_df = pd.read_csv("tweets.csv")

# Select retweets from the Twitter DataFrame: result
result = filter(lambda x : x[0:2] == 'RT' , tweets_df['text'])

# Create list from filter object result: res_list
res_list = list(result)

# Print all retweets in res_list
for tweet in res_list:
    print(tweet)

# %%
import pandas as pd

tweets_df = pd.read_csv("tweets.csv")

# Define count_entries()
def count_entries(df, col_name='lang'):
    """Return a dictionary with counts of
    occurrences as value for each key."""

    # Initialize an empty dictionary: cols_count
    cols_count = {}

    # Add try block
    try:
        # Extract column from DataFrame: col
        col = df[col_name]
        
        # Iterate over the column in dataframe
        for entry in col:
    
            # If entry is in cols_count, add 1
            if entry in cols_count.keys():
                cols_count[entry] += 1
            # Else add the entry to cols_count, set the value to 1
            else:
                cols_count[entry] = 1
    
        # Return the cols_count dictionary
        return cols_count

    # Add except block
    except:
        print('The DataFrame does not have a ' + col_name + ' column.')

# Call count_entries(): result1
result1 = count_entries(tweets_df, 'lang')

# Print result1
print(result1)

# %%
import pandas as pd

tweets_df = pd.read_csv("tweets.csv")

# Define count_entries()
def count_entries(df, col_name='lang'):
    """Return a dictionary with counts of
    occurrences as value for each key."""
    
    # Raise a ValueError if col_name is NOT in DataFrame
    if col_name not in df.columns:
        raise ValueError('The DataFrame does not have a ' + col_name + ' column.')

    # Initialize an empty dictionary: cols_count
    cols_count = {}
    
    # Extract column from DataFrame: col
    col = df[col_name]
    
    # Iterate over the column in DataFrame
    for entry in col:

        # If entry is in cols_count, add 1
        if entry in cols_count.keys():
            cols_count[entry] += 1
            # Else add the entry to cols_count, set the value to 1
        else:
            cols_count[entry] = 1
        
        # Return the cols_count dictionary
    return cols_count

# Call count_entries(): result1
result1 = count_entries(tweets_df, 'lang')

# Print result1
print(result1)

# %% [markdown]
# ## Practice

# %%
x = ['aPPle', 'baNANa']

def caps(li):
    """Returns a list, with all elements capitalized"""
    def inner(w):
        """Returns a capitalized word"""
        return w.capitalize()
    return ([inner(li[0]), inner(li[1])])

print(caps(x))

# %%
def sorted_elements(x, desc=True, n=2):
    new_x = sorted(x, reverse=desc)[0:n]
    return new_x

# %%
a = [8, 1, 4, 1, 4, 3]
print(sorted_elements(a, n = 4))

# %%
a = [8, 1, 4, 1, 4, 3]
print(sorted_elements(a, desc = False, n = 3))


# %% [markdown]
# ## 1. Using iterators in PythonLand

# %% [markdown]
# ### Iterators

# %% [markdown]
# - Iterating with a for loop
# - Iterable
#     - list, string, dict, file connections
#     - object associated iter() method
#     - applying iter() to an iterable iterator
# 
# - Iterator
#     - produce next value with next()

# %%
employees = ['Nick', 'Lore', 'Hugo']

for employee in employees:
    print(employee)

# %%
for i in range(4):
    print(i)

# %% [markdown]
# #### Iterating over iterables : next() - string

# %%
word = 'Da'
it = iter(word)
next(it)

# %%
next(it)

# %%
next(it)

# %%
word = 'Data'
it = iter(word)
print(*it)

# %%
print(*it)

# %%
flash = ['jay garrick', 'barry allen', 'wally west', 'bart allen']

# for person in flash:
#     print(person)

supehero = iter(flash)

print(next(supehero))
print(next(supehero))
print(next(supehero))
print(next(supehero))

# %%
small_value = iter(range(3))

print(next(small_value))
print(next(small_value))
print(next(small_value))

# for num in range(3):
#     print(num)

googol = iter(range(10 ** 100))

print(next(googol))
print(next(googol))
print(next(googol))
print(next(googol))
print(next(googol))

# %% [markdown]
# #### Iterating over dictionaries

# %%
pythonistas = {'hugo':'bowne-anderson', 'francis':'castro'}

for key, value in pythonistas.items():
    print(key, value)

# %% [markdown]
# #### Iterating over file connections

# %%
file = open('file.txt')
it = iter(file)
print(next(it))

# %% [markdown]
# #### Iterators as function arguments

# %%
values = range(10, 21)
print(values)

value_list = list(values)
print(value_list)

value_sum = sum(values)
print(value_sum)

# %% [markdown]
# ### Playing with iterators

# %% [markdown]
# #### Using enumerate()

# %%
avengers = ['hawkeye', 'iron man', 'thor', 'quicksilver']
e = enumerate(avengers)

# %%
e_list = list(e)
print(e_list)

# %% [markdown]
# #### enumerate() and unpack

# %%
avengers = ['hawkeye', 'iron man', 'thor', 'quicksilver']
for index, value in enumerate(avengers):
    print(index + 1, value)

print()

for index, value in enumerate(avengers, start=10):
    print(index, value)

# %% [markdown]
# #### Using zip()

# %%
avengers = ['hawkeye', 'iron man', 'thor', 'quicksilver']
names = ['barton', 'stark', 'odinson', 'maximoff']

z = zip(avengers, names)
print(type(z))

# %%
z_list = list(z)
print(z_list)

# %%
mutants = ['charles xavier', 
            'bobby drake', 
            'kurt wagner', 
            'max eisenhardt', 
            'kitty pryde']

mutant_list = list(enumerate(mutants))
print(mutant_list)

for index1, value1 in enumerate(mutants):
    print(index1, value1)

for index2, value2 in enumerate(mutants, start=1):
    print(index2, value2)

# %%
x = ['nia', 'ulan', 'sari']
y = ['arief', 'budi', 'mulia']
z = ['pipi', 'susanti']

nama = list(zip(x, y, z))
nama_zip = zip(x, y, z)
nama_zip

for nilai1, nilai2, nilai3 in nama_zip:
    print(nilai1, nilai2, nilai3)

# %%
x = ('nia', 'ulan', 'sari')
y = ('arief', 'budi', 'mulia')

#Zip
z1 = zip(x, y)
print(*z1)

#Unzip
z1 = zip(x, y)
result1, result2 = zip(*z1)

print(result1)
print(result2)
print(result1 == x)
print(result2 == y)

# %% [markdown]
# ### Using iterators to load large files into memory

# %% [markdown]
# - Jika ada data yang terlalu besar, maka pengerjaannya dapat dibagi menjadi beberapa bagian kecil menggunakan iterator

# %%
import pandas as pd

counts_dict = {}

for chunk in pd.read_csv('tweets.csv', chunksize=10):

    for entry in chunk['lang']:
        if entry in counts_dict.keys():
            counts_dict[entry] += 1
        else:
            counts_dict[entry] = 1

print(counts_dict)

# %%
def count_countries(csv_file, c_size, colname):
    counts_dict = {}

    for chunk in pd.read_csv(csv_file, chunksize=c_size):
        for entry in chunk[colname]:
            if entry in counts_dict.keys():
                counts_dict[entry] += 1
            else:
                counts_dict[entry] = 1
    return counts_dict

result_counts = count_countries('tweets.csv', 10, 'lang')
print(result_counts)

# %% [markdown]
# ## 2. List comprehensions and generators

# %% [markdown]
# ### List comprehensions

# %% [markdown]
# - Mempersingkat for loop menjadi single line

# %%
#For loop example
nums = [12, 8, 21, 3, 16]
new_nums = []
for nums in nums:
    new_nums.append(nums + 2)
print(new_nums)

# %%
#List comprehension example
nums = [12, 8, 21, 3, 16]
new_nums = [nums + 2 for nums in nums]
print(new_nums)

# %%
result = [num * 2 for num in range(11)]
print(result)

# %%
pairs_1 = []
for num1 in range(0, 2):
    for num2 in range(6, 8):
        pairs_1.append(num1, num2)
print(pairs_1)

# %%
pairs_2 = [(num1, num2) for num1 in range(0, 2) for num2 in range(6, 8)]
print(pairs_2)

# %%
doctor = ['house', 'cuddy', 'chase', 'thirteen', 'wilson']
z = [doc[0] for doc in doctor]
print(z)

# %%
squares = [i ** 2 for i in range(0, 10)]
print(squares)

# %%
# Create a 5 x 5 matrix using a list of lists: matrix
matrix = [[col for col in range(5)] for row in range(2)]

# Print the matrix
for row in matrix:
    print(row)

# %%
for col in range(5):
    for row in range(2):
        print(col, row)


# %% [markdown]
# ### Advanced comprehensions

# %%
[num ** 2 for num in range(10) if num % 2 == 0]

# %%
[num ** 2 if num % 2 == 0 else 0 for num in range(10)]

# %% [markdown]
# #### Dict comprehensions

# %%
pos_neg = {num + 1: num * 2 for num in range(9)}

# %%
pos_neg

# %%
fellowship = ['frodo', 'samwise', 'merry', 'aragorn', 'legolas', 'boromir', 'gimli']

new_fellowship = [member for member in fellowship if len(member) >= 7]
print(new_fellowship)

# %%
fellowship = ['frodo', 'samwise', 'merry', 'aragorn', 'legolas', 'boromir', 'gimli']

new_fellowship = [member if len(member ) >= 7 else '' for member in fellowship]

# %%
new_fellowship

# %%
#Dict Comprehension

fellowship = ['frodo', 'samwise', 'merry', 'aragorn', 'legolas', 'boromir', 'gimli']

new_fellowhip = {member : len(member) for member in fellowship}


# %%
new_fellowhip

# %% [markdown]
# ### Generator Expressions

# %% [markdown]
# #### Recall list comprehension

# %%
[2 * num for num in range(10)]

# %%
(2 * num for num in range(10))

# %% [markdown]
# List comprehensions vs. generators
# 
# - List comprehension - returns a list
# - Generators - returns a generator object
# - Both can be iterated over

# %%
result = (num for num in range(6))
for num in result:
    print(num)

# %%
result = (num for num in range(6))
print(list(result))

# %%
result = (num for num in range(6))

print(next(result))
print(next(result))
print(next(result))
print(next(result))
print(next(result))

# %% [markdown]
# #### Generator Functions

# %% [markdown]
# - Produces generator objects when called
# - Defined like a regular function def
# - Yields a sequence of values instead of returning a single value
# - Generates a value with yields keyword

# %%
def num_sequence(n):
    """"Generate values from 0 to n"""

    i = 0
    while i < n:
        yield i
        i +=1

# %%
result = num_sequence(5)
print(type(result))

# %%
for item in result:
    print(item)

# %%
result = (num for num in range(31))

print(next(result))
print(next(result))
print(next(result))
print(next(result))
print(next(result))

for value in result:
    print(value)

# %%
lannister = ['cersei', 'jaime', 'tywin', 'tyrion', 'joffrey']

lengths = (len(person) for person in lannister)

for value in lengths:
    print(value)

# %%
lannister = ['cersei', 'jaime', 'tywin', 'tyrion', 'joffrey']

def get_lengths(input_list):
    """Generator function thaat yields the length of the srings in input_list"""

    for person in input_list:
        yield len(person)

for value in get_lengths(lannister):
    print(value)

# %%
d = {
    'one': 1,
    'two': 2,
    'three': 3,
    'four': 4
}

# %% [markdown]
# ### Wrapping up comprehensions and generators

# %% [markdown]
# Re-cap : list comprehensions: 
#  
#     - Basic 
#     [output expression for interator variable in iterable]
# 
#     - Advanced
#     [output expression + conditional on output for iterator variable in iterable + conditional on iterable]
#     

# %%
import pandas as pd

df = pd.read_csv('tweets.csv')


# %%
tweet_time = df['created_at']

# %%
tweet_clock_time = [entry[11:19] for entry in tweet_time]

# %%
print(tweet_clock_time)

# %%
tweet_time = df['created_at']

# %%
tweet_clock_time = [entry[11:19] for entry in tweet_time if entry[17:19] == '19']

# %%
print(tweet_clock_time)

# %% [markdown]
# ## 3. Bringing it all together!

# %% [markdown]
# ### Welcome to the case study!

# %% [markdown]
# World bank data
# 
# - Data on world economies for over half a century
# - Indicators 
#     - Population
#     - Electricity consumption
#     - CO2 emissions
#     - Literacy rates
#     - Unemployment
#     - Mortality rates

# %%
avengers = ['hawkeye', 'iron man', 'thor', 'quicksilver']
names = ['barton', 'stark', 'odinson', 'maximoff']

z = zip(avengers, names)
print(type(z))

# %%
list(z)

# %%
def raise_both(value1, value2):
    """Raise value1 to the power of value2 and vice versa"""

    new_value1 = value1 ** value2
    new_value2 = value2 ** value1
    new_tuple = (new_value1, new_value2)
    return new_tuple

# %%
raise_both(3, 2)

# %%
#Dataset dari bigger dataset file of world development indicators from the World Bank.

zipped_lists = zip(features_name, row_vals)
rs_dict = dict(zipped_lists)
print(rs_dict)

# %%
def lists2dict(list1, list2):
    """Return a dictionary where list1 provides
    the keys and list2 provides the values."""

    zipped_lists = zip(list1, list2)
    rs_dict = dict(zipped_lists)

    return rs_dict

rs_fxn = lists2dict(feature_names, row_vals)

print(rs_fxn)

# %%
print(row_lists[0])
print(row_lists[1])

list_of_dicts = [lists2dict(feature_names, sublist) for sublist in row_lists]

print(list_of_dicts[0])
print(list_of_dicts[1])

# %%
import pandas as pd

list_of_dicts = [lists2dict(feature_names, sublist) for sublist in row_lists]

df = pd.DataFrame(list_of_dicts)

print(df.head())

# %% [markdown]
# ### Using Python generators for streaming data

# %% [markdown]
# Generators for large data limit : 
# 
#     - Use a generator to load a file line by line 
#     - Works on streaming data
#     - Read and process the file until all lines are exhausted

# %%
#Open a connection to the file
with open('world_dev_ind.csv') as file:

    #Skip the column names
    file.readline()

    #Initialize an empty dict
    counts_dict = {}

    #Process only the first 1000 rows
    for j in range(0, 1000):
        #Split the current line into a list = line
        line = file.readline().split(',')

        #Get the value for the first column : first_col
        first_col = line[0]

        #Get the value is in the dict, increment its value
        if first_col in counts_dict.keys():
            counts_dict[first_col] += 1
        else:
            counts_dict[first_col] = 1

print(counts_dict)

# %%
def read_large_file(file_object):
    """A generator function to read a large file lazily"""

    while True:

        data = file_object.readline()
        if not data:
            break 
        yield data

with open('world_dev_ind.csv') as file:

    gen_file = read_large_file(file)

    print(next(gen_file))
    print(next(gen_file))
    print(next(gen_file))

# %%
counts_dict = {}

with open('world_dev_ind.csv') as file:

    for line in read_large_file(file):

        row = line.split(",")
        first_col = row[0]

        if first_col in counts_dict.keys():
            counts_dict[first_col] += 1
        else:
            counts_dict[first_col] = 1

print(counts_dict)

# %% [markdown]
# ### Using pandas 'read_csv' iterator for streaming data

# %% [markdown]
# Reading file in chunks : 
# 
# - read_csv() function and chunksize argument
# - look at spesific indicators in spesific countries
# 

# %%
import pandas as pd

df_reader = pd.read_csv('ind_pop.csv', chunksize=10)

print(next(df_reader)) #Cetak 10 row data
print(next(df_reader)) #Cetak 10 row data berikutnya


# %%
import pandas as pd

urb_pop_reader = pd.read_csv('ind_pop_data.csv', chunksize=1000)

df_urb_pop = next(urb_pop_reader)
print(df_urb_pop.head())

df_pop_ceb = df_urb_pop[df_urb_pop['CountryCode'] == 'CEB']
pops = zip(df_pop_ceb['Total Population'], 
           df_pop_ceb['Urban population (% of total)'])
pops_list = list(pops)

print(pops_list)

# %%
import pandas as pd
import matplotlib.pyplot as plt

urb_pop_reader = pd.read_csv('ind_pop_data.csv', chunksize=1000)
df_urb_pop = next(urb_pop_reader)
df_pop_ceb = df_urb_pop[df_urb_pop['CountryCode'] == 'CEB']
pops = zip(df_pop_ceb['Total Population'], 
           df_pop_ceb['Urban population (% of total)'])
pops_list = list(pops)

df_pop_ceb['Total Urban Population'] = [int(tup[0] * tup[1] * 0.01)  for tup in pops_list]

df_pop_ceb.plot(kind='scatter', x='Year', y='Total Urban Population')
plt.show()

# %%
# Initialize reader object: urb_pop_reader
urb_pop_reader = pd.read_csv('ind_pop_data.csv', chunksize=1000)

# Initialize empty DataFrame: data
data = pd.DataFrame()

# Iterate over each DataFrame chunk
for df_urb_pop in urb_pop_reader:

    # Check out specific country: df_pop_ceb
    df_pop_ceb = df_urb_pop[df_urb_pop['CountryCode'] == 'CEB']

    # Zip DataFrame columns of interest: pops
    pops = zip(df_pop_ceb['Total Population'],
                df_pop_ceb['Urban population (% of total)'])

    # Turn zip object into list: pops_list
    pops_list = list(pops)

    # Use list comprehension to create new DataFrame column 'Total Urban Population'
    df_pop_ceb['Total Urban Population'] = [int(tup[0] * tup[1] * 0.01) for tup in pops_list]
    
    # Append DataFrame chunk to data: data
    data = data.append(df_pop_ceb)

# Plot urban population data
data.plot(kind='scatter', x='Year', y='Total Urban Population')
plt.show()


# %%
# Define plot_pop()
def plot_pop(filename, country_code):

    # Initialize reader object: urb_pop_reader
    urb_pop_reader = pd.read_csv(filename, chunksize=1000)

    # Initialize empty DataFrame: data
    data = pd.DataFrame()
    
    # Iterate over each DataFrame chunk
    for df_urb_pop in urb_pop_reader:
        # Check out specific country: df_pop_ceb
        df_pop_ceb = df_urb_pop[df_urb_pop['CountryCode'] == country_code]

        # Zip DataFrame columns of interest: pops
        pops = zip(df_pop_ceb['Total Population'],
                    df_pop_ceb['Urban population (% of total)'])

        # Turn zip object into list: pops_list
        pops_list = list(pops)

        # Use list comprehension to create new DataFrame column 'Total Urban Population'
        df_pop_ceb['Total Urban Population'] = [int(tup[0] * tup[1] * 0.01) for tup in pops_list]
    
        # Append DataFrame chunk to data: data
        data = data.append(df_pop_ceb)

    # Plot urban population data
    data.plot(kind='scatter', x='Year', y='Total Urban Population')
    plt.show()

# Set the filename: fn
fn = 'ind_pop_data.csv'

# Call plot_pop for country code 'CEB'
plot_pop(fn, 'CEB')

# Call plot_pop for country code 'ARB'
plot_pop(fn, 'ARB')



