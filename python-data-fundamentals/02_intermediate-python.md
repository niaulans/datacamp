## [Intermediate Python](https://app.datacamp.com/learn/courses/intermediate-python)

# %% [markdown]
# ## Matplotlib

# %% [markdown]
# - Visualization 
# - Very important in Data Analyis
#     - Explore data (get insights)
#     - Report insights
#     
# - Many visualization package in python 

# %%
import matplotlib.pyplot as plt

# %%
#Insights about world population
year = [1950, 1970, 1990, 2010] #year
pop = [2.519, 3.692, 5.263, 6.792] # population in billion

# %% [markdown]
# ### Line plot

# %%
#Plot using line chart
plt.plot(year, pop) # argumen pertama = x-axis, argumen kedua = y-axis
#plot() function digunain untuk membuat apa yang mau di plot dan gimana cara ngeplot nya
plt.show()
#show() function untuk display plot

# %% [markdown]
# ### Scatter plot

# %%
import matplotlib.pyplot as plt

#Insights about world population
year = [1950, 1970, 1990, 2010] #year
pop = [2.519, 3.692, 5.263, 6.792] # population in billion
plt.scatter(year, pop)
#plt.xscale('log') #digunakan untuk mengubah scale x-axis menjadi logarithmic
plt.show()

# %% [markdown]
# - Scatter plot paling banyak digunakan daripada yang lain

# %% [markdown]
# ### Histogram

# %% [markdown]
# - Explore dataset
# - Get idea about distribution

# %%
import matplotlib.pyplot as plt

# %%
help(plt.hist)

# %%
values = [0, 0.6, 1.4, 1.6, 2.2, 2.5, 2.6, 3.2, 3.5, 3.9, 4.2, 6]

# %%
plt.hist(values, bins = 5)
#bins merupakan pengelompokan 
#jika bins tidak diisi maka default 10

# %%
#plt.clf() digunakan untuk me-refresh plot

# %%
#You're a professor in Data Analytics with Python, and you want to visually assess if longer answers on exam questions lead to higher grades. Which plot do you use?
#The answer is : scatter plot

# %% [markdown]
# ### Customization

# %% [markdown]
# Tantangan dalam membuat plot adalah :
# - Buat plot yang sesuai
# - Buat pesan jadi sejelas mungkin
# 
# Data visualization : 
# - many option 
# - may customizations
# 
# pemilihan berdasarkan : 
# - data
# - cerita yang mau disampaikan

# %%
import matplotlib.pyplot as plt 

#Insights about world population
year = [1950, 1970, 1990, 2010] #year
pop = [2.519, 3.692, 5.263, 6.792] # population in billion

#add historical data untuk memperkuat
year = [1800, 1850, 1900] + year
pop  = [1.0, 1.262, 1.650] + pop

#year for x-axis and pop for y-axis
plt.plot(year, pop)

#X-axis labels
plt.xlabel("Year")

#Y-axis labels
plt.ylabel("Population")

#Title plot
plt.title("World Population Projections")

#Ticks digunakan untuk mengatur value axis
plt.yticks([0, 2, 4, 6, 8, 10], 
           ['0', '2B', '4B', '6B', '8B', '10B'])
#maksud B diatas adalah Billion
plt.show()


# %% [markdown]
# #### Ticks 

# %%
# Scatter plot
plt.scatter(gdp_cap, life_exp)

# Previous customizations
plt.xscale('log') 
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')

# Definition of tick_val and tick_lab
tick_val = [1000, 10000, 100000]
tick_lab = ['1k', '10k', '100k']

# Adapt the ticks on the x-axis
plt.xticks(tick_val, tick_lab)

# After customizing, display the plot
plt.show()

# %% [markdown]
# #### Sizes

# %% [markdown]
# argument s digunakan untuk menentukan size dari ukuran bubble scatter plot

# %% [markdown]
# #### Colors 

# %%
# Specify c and alpha inside plt.scatter()
plt.scatter(x = gdp_cap, y = life_exp, 
            s = np.array(pop) * 2, c = col, alpha = 0.8)

#c digunakan untuk menentukan warna 
#alpha digunakan untuk menentukan opaacity, angkanya dari 0-1, dimana 0 merupakan transparant

# Previous customizations
plt.xscale('log') 
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')
plt.xticks([1000,10000,100000], ['1k','10k','100k'])

# Show the plot
plt.show()

# %% [markdown]
# #### Additional customizations

# %%
# Scatter plot
plt.scatter(x = gdp_cap, y = life_exp, s = np.array(pop) * 2, c = col, alpha = 0.8)

# Previous customizations
plt.xscale('log') 
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')
plt.xticks([1000,10000,100000], ['1k','10k','100k'])

# Additional customizations
#text digunakan untuk menambahkan keterangan pada bubble scatter
plt.text(1550, 71, 'India')
plt.text(5700, 80, 'China')

# Add grid() call
#grid digunakan untuk membuat garis
plt.grid(True)

# Show the plot
plt.show()

# %% [markdown]
# dari plot diatas menunjukkan bahwa gdp berpengaruh terhadap life expectancy penduduknya. Jika gdp negara tinggi, maka life expectancy penduduknya juga tinggi

# %% [markdown]
# ## Dictionaries and Pandas

# %% [markdown]
# ### Dictionaries

# %%
pop = [30.55, 2.77, 39.21]
countries = ["Afghanistan", "Albania", "Algeria"]

# %%
ind_alb = countries.index("Albania")

# %%
ind_alb

# %%
pop[ind_alb]

# %%
world = {"Afghanistan":30.55, "Albania":2.77, "Algeria":39.21}

# %%
world["Albania"]

# %%
countries = ['spain', 'france', 'germany', 'norway']
capitals = ['madrid', 'paris', 'berlin', 'oslo']

ind_ger = countries.index('germany')
capitals[ind_ger]

# %% [markdown]
# Using dictionary 

# %%
# Definition of dictionary
europe = {'spain':'madrid', 'france':'paris', 'germany':'berlin', 'norway':'oslo' }

# Print out the keys in europe
print(europe.keys())
print(europe.values())

# Print out value that belongs to key 'norway'
print(europe['norway'])

# %% [markdown]
# Keys pada dictionary merupakan object immutable

# %%
world["sealand"] = 0.000027

# %%
world

# %%
"sealand" in world

# %%
#Update data sealand di dictionary
world["sealand"] = 0.000028

# %%
world

# %%
del(world["Albania"])

# %%
world

# %% [markdown]
# List vs Dictionary

# %% [markdown]
# List :
# - select, update, remove()
# - indexed by range of numbers
# - collection of values orders matters select entire subsets
# 
# 
# Dictionary : 
# - select, update, remove()
# - indexed by unique keys
# - lookup table with unique keys

# %%
# Definition of dictionary
europe = {'spain':'madrid', 'france':'paris', 'germany':'berlin', 'norway':'oslo' }

# Add italy to europe
europe["italy"] = "rome"

# Print out italy in europe
print('italy' in europe)

# Add poland to europe
europe["poland"] = "warsaw"

# Print europe
print(europe)

# %%
# Definition of dictionary
europe = {'spain':'madrid', 'france':'paris', 'germany':'bonn',
          'norway':'oslo', 'italy':'rome', 'poland':'warsaw',
          'australia':'vienna' }

# Update capital of germany
europe["germany"] = "berlin"

# Remove australia
del(europe["australia"])

# Print europe
print(europe)

# %% [markdown]
# Dictionary dapat mengandung key:value pairs tetapi value juga dapat mengandung dict

# %%
# Dictionary of dictionaries
europe = { 'spain': { 'capital':'madrid', 'population':46.77 },
           'france': { 'capital':'paris', 'population':66.03 },
           'germany': { 'capital':'berlin', 'population':80.62 },
           'norway': { 'capital':'oslo', 'population':5.084 } }

# %%
europe

# %%
# Print out the capital of France
print(europe['france']['population'])

# %%
# Create sub-dictionary data
data = {'capital':'rome', 'population':59.83}

# %%
# Add data to europe under key 'italy'
europe['italy'] = data

# %%
europe

# %% [markdown]
# ### Pandas

# %% [markdown]
# Tabular -> excel data
# 
# Datasets in Python :
# - 2D Numpy array ?
#     - one data type
# - pandas 
#     - high level data manipulation
#     - built on numpy
#     - dataframe

# %% [markdown]
# #### Dataframe from dictionary

# %%
dict = {
    "country":["Brazil", "Rusia", "India", "China", "South Africa"], 
    "capital":["Brasilia", "Moscow"," New Delhi"," Beijing", "Pretoria"],
    "area":[8.516, 7.10, 3.286, 9.597, 1.221], 
    "population":[200.4, 143.5, 1252, 1357, 252.98]
}

# %%
dict

# %% [markdown]
# - keys = columns labels
# - values = data, columns by columns

# %%
import pandas as pd

# %%
brics = pd.DataFrame(dict)

# %%
brics

# %%
brics.index = ["BR", "RU", "IN", "CH", "SA"]

# %%
brics

# %%
brics1 = pd.read_csv("brics.csv", index_col=0)

# %%
brics1

# %%
# Pre-defined lists
names = ['United States', 'Australia', 'Japan', 'India', 'Russia', 'Morocco', 'Egypt']
dr =  [True, False, False, False, True, True, True]
cpc = [809, 731, 588, 18, 200, 70, 45]

# %%
# Import pandas as pd
import pandas as pd

# %%
# Create dictionary my_dict with three key:value pairs: my_dict
my_dict = {"country": names, "drivers_right": dr, "cars_per_cap":cpc}

# %%
my_dict

# %%
# Build a DataFrame cars from my_dict: cars
cars = pd.DataFrame(my_dict)

# %%
cars

# %%
import pandas as pd

# Build cars DataFrame
names = ['United States', 'Australia', 'Japan', 'India', 'Russia', 'Morocco', 'Egypt']
dr =  [True, False, False, False, True, True, True]
cpc = [809, 731, 588, 18, 200, 70, 45]
cars_dict = { 'country':names, 'drives_right':dr, 'cars_per_cap':cpc }
cars = pd.DataFrame(cars_dict)
print(cars)

# Definition of row_labels
row_labels = ['US', 'AUS', 'JPN', 'IN', 'RU', 'MOR', 'EG']

# Specify row labels of cars
cars.index = row_labels

# %%
cars

# %% [markdown]
# #### CSV to DataFrame

# %%
import pandas as pd

cars = pd.read_csv("cars.csv")

# %%
cars

# %%
cars = pd.read_csv("cars.csv", index_col=0)

# %%
cars

# %% [markdown]
# #### Index and select data

# %% [markdown]
# - square brackets
# - advanced methods : loc and iloc

# %%
import pandas as pd

brics = pd.read_csv("brics.csv", index_col=0)

# %%
brics

# %%
brics[["country", "capital"]]

# %%
type(brics["country"])

# %%
type(brics[["country"]])

# %%
brics[["country","capital"]]

# %%
type(brics[["country","capital"]])

# %%
brics[1:4]

# %%
brics[1:4]

# %%
brics[2:4]

# %% [markdown]
# #### Loc and Iloc function

# %% [markdown]
# - loc (label-based)
# - iloc ( integer position-based)

# %% [markdown]
# ##### Loc

# %%
brics

# %%
brics.loc["RU"]

# %%
brics.loc[["RU"]]

# %%
brics.loc[["RU", "IN", "CH"]]

# %%
brics.loc[["RU", "IN", "CH"], ["country", "capital"]]

#Baris dulu baru kolom

# %%
brics.loc[:, ["country", "capital"]]
#get semua baris tapi cuma ambil 2 kolom

# %% [markdown]
# ##### Iloc

# %%
brics.iloc[[1]]

# %%
brics.iloc[[1, 2, 3]]

# %%
brics.iloc[[1, 2, 3], [0, 1]]

# %%
brics.iloc[:, [0, 1]]

# %%
cars = pd.read_csv("cars.csv", index_col=0)

# %%
cars

# %%
cars["country"]

# %%
cars[["country"]]

# %%
cars[["country", "drives_right"]]

# %%
cars[0:3]

# %%
cars[3:7]

# %%
cars

# %%
cars.loc["JAP"]

# %%
cars.loc[["AUS", "EG"]]

# %%
cars.iloc[[1, 6], :]

# %%
cars.iloc[-2, 2]

# %%
cars.iloc[[4, 5, 6], [1, 2]]

# %%
cars.iloc[:, 2]

# %%
cars.loc[:, ["drives_right"]]

# %%
cars.loc[:, ["cars_per_cap", "drives_right"]]

# %% [markdown]
# ## Logic, Control Flow and Filtering

# %% [markdown]
# ### Numeric comparisons

# %%
2 < 3

# %%
2 == 3

# %%
2 <= 3 

# %%
3 <= 3

# %%
x = 2
y = 3
x < y

# %%
"carl" < "chris"

# %%
3 < "chris"

# %%
3 < 4.1

# %%
True == False

# %%
-5 * 15 != 75

# %%
"pyscript" == "PyScript"

# %%
True == 1

# %%
import numpy as np
my_house = np.array([18.0, 20.0, 10.75, 9.50])
your_house = np.array([14.0, 24.0, 14.25, 9.0])

# %%
my_house[my_house >= 18]

# %%
my_house >= 18

# %%
my_house[my_house > your_house]

# %%
my_house > your_house

# %% [markdown]
# ### Boolean operators

# %% [markdown]
# #### AND

# %%
True and True

# %%
False and True

# %%
True and False

# %%
False and False

# %%
x = 12
x > 5 and x < 15

# %% [markdown]
# #### OR

# %%
True or True

# %%
False or True

# %%
True or False

# %%
False or False

# %%
y = 5
y < 7 or y > 13

# %% [markdown]
# #### Not

# %%
not True

# %%
not False

# %% [markdown]
# #### Numpy

# %% [markdown]
# - logical_and()
# - logical_or()
# - logical_not()

# %%
bmi[np.logical_and(bmi > 21, bmi < 22)]

# %%
x = 8
y = 9
not(not(x < 3) and not(y > 14 or y > 10))

# %%
import numpy as np
my_house = np.array([18.0, 20.0, 10.75, 9.50])
your_house = np.array([14.0, 24.0, 14.25, 9.0])

# %%
my_house[np.logical_or(my_house > 18.5, my_house < 10)]

# %%
# my_house greater than 18.5 or smaller than 10
print(np.logical_or(my_house > 18.5, my_house < 10))

# %%
# Both my_house and your_house smaller than 11
print(np.logical_and(my_house < 11, your_house < 11))

# %% [markdown]
# ### if, elif, else

# %% [markdown]
# if condition:
# - expression

# %%
z = 4
if z % 2 == 0:
    print("z is even")

# %%
z = 4
if z % 2 == 0:
    print("z is even")
else:
    print("z is odd")

# %%
z = 15
if z % 2 == 0 :
    print("z is divisible by 2")
elif z % 3 == 0 :
    print("z is divisible by 3")
else :
    print("z is neither divisible by 2 nor by 3")

# %% [markdown]
# ### Filtering pandas DataFrames

# %%
import pandas as pd
brics = pd.read_csv("brics.csv", index_col= 0 )

# %%
brics

# %% [markdown]
# - Select countries with area over 8 million km2
# 
# 3 step : 
# - select area column
# - do comparison on area column
# - use result to select country

# %%
brics[["area"]]
#brics.loc[:, ["area"]]
#brics.iloc[:, [2]]

# %%
brics[["area"]] > 8

# %%
is_huge = brics["area"] > 8
brics[is_huge]

# %%
brics["area"] > 8

# %%
brics[brics["area"] > 8]

# %%
brics[brics["country"] == "Brazil"]

# %%
brics[np.logical_and(brics["area"] > 8, brics["area"] < 18)]

# %%
brics[np.logical_or(brics["population"] > 1000, brics["area"] > 1000)]

# %%
import pandas as pd

cars = pd.read_csv("cars.csv", index_col=0)

# %%
cars

# %%
dr = cars["drives_right"]

# %%
dr

# %%
sel = cars[dr]

# %%
sel

# %%
cars[cars["drives_right"]]

# %%
cars

# %%
cars[cars["cars_per_cap"] > 500]

# %%
#cara panjang
cpc = cars["cars_per_cap"]
many_cars = cpc > 500
car_maniac = cars[many_cars]
car_maniac

# %%
cars[np.logical_and(cars["cars_per_cap"] > 100, cars["cars_per_cap"] < 500)]

# %%
#cara panjang 
cpc = cars["cars_per_cap"]
between = np.logical_and(cpc > 100, cpc < 500)
medium = cars[between]
medium

# %% [markdown]
# ## Loops

# %% [markdown]
# ### While loop

# %% [markdown]
# Bakalan eksekusi expression jka kondisinya benar.
# 
# - while loop = repeated if statement
# - repeating action until condition is met

# %%
error = 50.0

while error > 1: #berhenti
    error = error / 4
    print(error)

# %%
offset = 8

while offset != 0 :
    print("correcting...")
    offset -= 1
    print(offset)

# %%
offset = -6

while offset != 0 :
    print("correcting...")
    if offset > 0:
        offset -= 1
    else :
        offset += 1
    print(offset)

# %% [markdown]
# ### For Loop

# %% [markdown]
# for var in seq :
# - expression

# %%
fam = [1.73, 1.68, 1.71, 1.89]

# %%
fam

# %%
print(fam[0])
print(fam[1])
print(fam[2])
print(fam[3])

# %%
for height in fam:
    print(height)

# %%
for index, height in enumerate(fam):
    print("Index " + str(index) + ": " + str(height))

# %%
for c in "family":
    print(c.capitalize())

# %%
areas = [11.25, 18.0, 20.0, 10.75, 9.50]

# %%
for x in areas:
    print(x)

# %%
for a, room in enumerate(areas):
    print("room " + str(a) + ": " + str(room))

# %%
for a, room in enumerate(areas):
    print("room " + str(a + 1) + ": " + str(room))

# %%
house = [["hallway", 11.25], 
         ["kitchen", 18.0], 
         ["living room", 20.0], 
         ["bedroom", 10.75], 
         ["bathroom", 9.50]]

# %%
for x in house :
    print("the " + x[0] + " is " + str(x[1]) + " sqm")

# %% [markdown]
# ### Loop Data Structures 1

# %%
world1 : {"afghanistan": 30.55, 
     "albania": 2.77,
     "algeria":39.2}

# %%
world1

# %%
#Untuk mencetak perulangan dari dictionary
#menggunakan method items()

for key, value in world.items() :
    print(key + " -- " + str(value))

# %%
import numpy as np

# %%
np_height = np.array([1.73, 1.68, 1.71, 1.89, 1.79])
np_weight = np.array([65.4, 59.2, 63.6, 88.4, 68.7])
bmi = np_weight / np_height ** 2

# %%
for val in bmi:
    print(val)

# %%
np_height = np.array([1.73, 1.68, 1.71, 1.89, 1.79])
np_weight = np.array([65.4, 59.2, 63.6, 88.4, 68.7])
meas = np.array([np_height, np_weight])
for val in meas :
    print(val)

# %%
#Untuk mencetak perulangan dari numpy array menggunakan
#method nditer()
for val in np.nditer(meas):
    print(val)

# %%
europe = {'spain':'madrid', 'france':'paris', 'germany':'berlin',
          'norway':'oslo', 'italy':'rome', 'poland':'warsaw', 'austria':'vienna' }

# %%
for key, value in europe.items():
    print("the capital of " + key + " is " + value)

# %%
import numpy as np

nia = np.array([56, 77, 88, 99, 100])

# %%
for x in np.nditer(nia):
    print(x)

# %% [markdown]
# ### Loop Data Structures 2

# %%
import pandas as pd

brics = pd.read_csv("brics.csv", index_col= 0)
brics

# %%
for val in brics:
    print(val)

# %% [markdown]
# #### Iterrows

# %%
for lab, row in brics.iterrows():
    print(lab)
    print(row)

# %%
for lab, row in brics.iterrows():
    print(lab + " : " + row["capital"] + ", " + str(row["population"]))

# %% [markdown]
# #### Add column

# %%
import pandas as pd

brics = pd.read_csv("brics.csv", index_col=0)

# %%
for lab, row in brics.iterrows():
    # - Creating series on every iteration
    brics.loc[lab, "name_length"] = len(row["country"])

# %%
brics

# %%
for lab, row in brics.iterrows():
    brics.loc[lab, "Total"] = row["area"] * row["population"]

# %%
brics

# %% [markdown]
# #### Apply

# %%
import pandas as pd

brics = pd.read_csv("brics.csv", index_col=0)
brics["name_length"] = brics["country"].apply(len)

# %%
brics

# %%
import pandas as pd

cars = pd.read_csv("cars.csv", index_col=0)

# %%
cars

# %%
for x, y in cars.iterrows():
    print(x)
    print(y)

# %%
for x, y in cars.iterrows():
    print(x + ": " + str(y["cars_per_cap"]))

# %%
for x, y in cars.iterrows():
    cars.loc[x, "COUNTRY"] = str.upper(y["country"])

# %%
cars

# %%
cars["COUNTRY"] = cars["country"].apply(str.upper)

# %%
cars

# %% [markdown]
# ## Case Study: Hacker Statistics

# %% [markdown]
# ### Random Numbers

# %% [markdown]
# How to solve ?
# - Analytical
# - Simulate the process
#     - Hackerstatistics

# %% [markdown]
# #### Random generators

# %%
import numpy as np
#np.random.rand()  # Pseudo- random numbers
#didalam numpy package terdapat random sub-packages random dan didalamnya terdapat fungsi rand
#fungsi rand() menghasilkan angka random antara 0-1

# %%
np.random.seed(123)

# %%
np.random.rand()

# %% [markdown]
# #### Coin toss

# %%
import numpy as np

np.random.seed(123)
coin = np.random.randint(0,2) # ngerandom angka 0 dan 1
print(coin)

if coin == 0:
    print("heads")
else:
    print("tails")

# %%
np.random.randint(1,7)

# %%
import numpy as np

np.random.seed(123)

step = 50
dice = np.random.randint(1,7)

if dice <= 2:
    step = step - 1
elif dice <= 5:
    step = step + 1
else: 
    step = step + np.random.randint(1,7)

# %%
print(dice)
print(step)

# %% [markdown]
# ### Random Walk

# %%
import numpy as np

np.random.seed(123)
outcomes = []

for x in range(10):
    coin = np.random.randint(0,2)
    if coin == 0:
        outcomes.append("heads")
    else:
        outcomes.append("tails")

# %%
outcomes

# %%
import numpy as np

np.random.seed(123)
tails = [0]
for x in range(10):
    coin = np.random.randint(0,2)
    tails.append(tails[x] + coin)

# %%
tails

# %%
import numpy as np

np.random.seed(123)

#Inisialisasi random walk dengan 0
random_walk = [0]

for x in range(100):
    #Set step dimana element terakhir pada list
    step = random_walk[-1]
    
    #Lempar dadu
    dice = np.random.randint(1,7)
    
    #Melangkah
    if dice <= 2:
        step = step - 1
    elif dice <= 5:
        step = step - 2
    else:
        step = step + np.random.randint(1,7)
        
    #Nambahin next_step ke random_walk
    random_walk.append(step)

# %%
random_walk

# %%
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(123)

random_walk = [0]

for x in range(100):
    step = random_walk[-1]
    dice = np.random.randint(1,7)
    
    if dice <=2 :
        step = max(0, step - 1)
    elif dice <=5 :
        step = step + 1
    else : 
        step = step + np.random.randint(1,7)
        
    random_walk.append(step)
    
# print(random_walk)

plt.plot(random_walk)
plt.show()

# %% [markdown]
# ### Distribution

# %%
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(123)
final_tails = []

for x in range(10000):
    tails = [0]
    for x in range(10):
        coin = np.random.randint(0,2)
        tails.append(tails[x] + coin)
    final_tails.append(tails[-1])

plt.hist(final_tails, bins = 10)
plt.show()

# %%
for x in range(2):
    print(x)
    for y in range(5):
        print(y)

# %%
import numpy as np

np.random.seed(123)

all_walks = []

for i in range(10):
    
    random_walk = [0]
    for x in range(100):
        step = random_walk[-1]
        dice = np.random.randint(1,7)
    
        if dice <=2 :
            step = max(0, step - 1)
        elif dice <=5 :
            step = step + 1
        else : 
            step = step + np.random.randint(1,7)
        
        random_walk.append(step)
    all_walks.append(random_walk)
print(all_walks)

# %%
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(123)

# initialize and populate all_walks
all_walks = []
for i in range(10) :
    random_walk = [0]
    for x in range(100) :
        step = random_walk[-1]
        dice = np.random.randint(1,7)
        if dice <= 2:
            step = max(0, step - 1)
        elif dice <= 5:
            step = step + 1
        else:
            step = step + np.random.randint(1,7)
        random_walk.append(step)
    all_walks.append(random_walk)

# Convert all_walks to Numpy array: np_aw
np_aw = np.array(all_walks)

# Plot np_aw and show
plt.plot(np_aw)
plt.show()

# Clear the figure
plt.clf()

# Transpose np_aw: np_aw_t
np_aw_t = np.transpose(np_aw)

# Plot np_aw_t and show
plt.plot(np_aw_t)
plt.show()

# %%
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(123)

# Simulate random walk 250 times
all_walks = []
for i in range(250) :
    random_walk = [0]
    for x in range(100) :
        step = random_walk[-1]
        dice = np.random.randint(1,7)
        if dice <= 2:
            step = max(0, step - 1)
        elif dice <= 5:
            step = step + 1
        else:
            step = step + np.random.randint(1,7)

        # Implement clumsiness
        if np.random.rand() <= 0.001 :
            step = 0

        random_walk.append(step)
    all_walks.append(random_walk)

# Create and plot np_aw_t
np_aw_t = np.transpose(np.array(all_walks))
plt.plot(np_aw_t)
plt.show()

# %%
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(123)

# Simulate random walk 500 times
all_walks = []
for i in range(500) :
    random_walk = [0]
    for x in range(100) :
        step = random_walk[-1]
        dice = np.random.randint(1,7)
        if dice <= 2:
            step = max(0, step - 1)
        elif dice <= 5:
            step = step + 1
        else:
            step = step + np.random.randint(1,7)
        if np.random.rand() <= 0.001 :
            step = 0
        random_walk.append(step)
    all_walks.append(random_walk)

# Create and plot np_aw_t
np_aw_t = np.transpose(np.array(all_walks))

# Select last row from np_aw_t: ends
ends = np_aw_t[-1,:]

# Plot histogram of ends, display plot
plt.hist(ends)
plt.show()


# %%
np.mean(ends > 30)

# %%
nia = "nia"
for letter in nia:
    print(letter)

# %% [markdown]
# ## Practice

# %% [markdown]
# - apply() digunakan agar lebih efisien dan code lebih mudah dibaca
# - Python akan set numbers of bins ke 10 jika tidak diinisialisasi pada function hist()
# - plot scatter digunakan untuk mengetahui korelasi antara 2 variabel

# %%
practice = {'python': 100, "r": 30, "sql": 10}
for course, number in practice.items():
    print(course + " practice has " + str(number) + " items")

# %%
fruits = {
    'nuts': 230,
    'lemons': 200,
    'olives': 250
}

print(fruits['lemons'])

# %%
device = ['MOBILE', 'DESKTOP']
active = ['yes', 'no']
login_id = ['12347', '11021']

# %%
dict = {'DEVICE': device, 'active': active, 'login_id': login_id }

# %%
import pandas as pd

data = pd.DataFrame(dict)

# %%
data

# %%
data['device'] = data['DEVICE'].apply(str.capitalize)

# %%
data

# %%
import numpy as np

x = np.array([[9, 2, 2], [9, 8, 7]])

for i in np.nditer(x):
    print(i)

# %%
width = [8.36, 15.11, 19.92, 8.25]
for index, x in enumerate(width):
    print('Room ' + str(index + 1) + ' -' + str(x) )


