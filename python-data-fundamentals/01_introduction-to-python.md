## [Introduction to Python](https://app.datacamp.com/learn/courses/intro-to-python-for-data-science)

# %% [markdown]
# ## Python Basics

# %% [markdown]
# - Python ditemukan **Guido Van Rossum**
# - General purpose : build anything
# - Open source ! Free
# - Dapat digunakan untuk perhitungan seperti   tambah, kurang, kali, bagi, modulus, eksponensial, dsb.

# %% [markdown]
# ### Calculator

# %%
#subtraction 
print(7 - 3)
print(5 - 2)

# %%
#multiplication 
print(800 * 40000)

# %%
#division
print(80000 / 40)

# %%
#modulo 
print(1920 % 3)

# %%
#exponentiation
print(15 ** 2)

# %% [markdown]
# Contoh kasus: 
# Misalnya kamu punya uang 100 dollar. Diinvestasikan dengan bunga sebesar 10 % tiap tahun. Jika setelah 1 tahun uang kamu jadi 100 * 1.1 = 110 dollar, kemudian setelah 2 tahun uang kamu jadi 100 * 1.1 * 1.1 = 121 dollar. Maka setelah 7 tahun uang kamu jadi berapa ???

# %%
#Penyelesaian
print(100 * (1.1 ** 7))

# %% [markdown]
# ### Variable

# %% [markdown]
# - Spesific
# - Case-sensitive through variable name

# %%
height = 1.79 
weight = 74.2

# %%
68.7 / 1.79 ** 2

# %%
weight / height ** 2

# %%
#store in new variable 
bmi = weight / height ** 2
print(bmi)

# %% [markdown]
# ### Type

# %% [markdown]
# - Tipe data dalam python ada : int, float, string, dsb
# - Tipe data string tidak dapat dijumlahkan   dengan tipe data lainnya, maka dari itu     data lainnya tersebut harus dikonversi   terlebih dahulu.

# %%
a = 2 + 3
type(a)
#type() digunakan untuk mengetahui tipe data 

# %%
c = 'ab' + 'cd'
type(c)

# %%
savings = 100
growth_multiplier = 1.1
desc = "compound interest"

# %%
year1 = savings * growth_multiplier

# %%
year1

# %%
doubledesc = desc + desc
doubledesc

# %%
# Type conversion 
savings = 100
result = 100 * 1.10 ** 7

print("I started with $" + savings + " and now have $" + result + ".Awesome!")

# %% [markdown]
# Code diatas error karena **strings tidak bisa digabungkan dengan tipe data lain seperti int dan float** , maka dari itu tipe data dari variable savings dan result harus di **conversi**.

# %%
savings = 100
result = 100 * 1.10 ** 7

print("I started with $", str(savings), "and now have $", str(result), ". Awesome!")

# %%
"I can add integers, like " + str(5) + " to strings."

# %% [markdown]
# ## Python Lists

# %% [markdown]
# Python data types : 
# - float - real numbers
# - int - integer numbers
# - str - string, text
# - bool - True, False
# 
# ---------------------------------------------------------------
# List = [a, b, c]
# - List dapat mengandung tipe data apapun bahkan mengandung list juga didalamnya

# %% [markdown]
# ### Create a list

# %%
hall = 11.25
kit = 18.0
liv = 20.0
bed = 10.75
bath = 9.50

# %%
areas = [hall, kit, liv, bed, bath]

# %%
areas

# %%
areas = ["hallway", hall, "kitchen", kit, "living room", liv, "bedroom", bed, "bathroom", bath]

# %%
areas

# %% [markdown]
# ### List of lists (list dalam list)

# %%
house = [["hallway", hall], 
         ["kitchen", kit], 
         ["living room", liv], 
         ["bedroom", bed],
         ["bathroom", bath]]

# %%
house

# %%
type(house)

# %% [markdown]
# ### Subsetting Lists

# %% [markdown]
# Element pertama dalam list mempunyai index 0
# Element kedua dalam list mempunyai index 1 
# dst

# %%
house

# %% [markdown]
# [start : end]

# %%
house[0]
#ngambil data list yang pertama

# %%
house[-1]
#ngambil data list yang paling akhir

# %%
house[0:2]
#ngambil data list dari index 0 - 1

# %%
house[:3]
#ngambil 3 data pertama, index 0-2

# %%
house[2:]
#ngambil data dari index 2 sampai akhir 

# %% [markdown]
# ### Slicing and dicing 

# %%
areas = ["hallway", 11.25, "kitchen", 18.0, "living room", 20.0, "bedroom", 10.75, "bathroom", 9.50]

# %%
#Print 6 element pertama list , index 0 - 5
areas[:6]

# %%
# Print 4 element terakhir list, index 6 - akhir
areas[6:]

# %% [markdown]
# ### Manipulating lists

# %% [markdown]
# - Change list element
# - Add list elements
# - Remove list elements

# %%
fam = ["Liz", 1.73, "emma", 1.68, "mom", 1.71, "dad", 1.89]

# %%
fam

# %%
#Changing list elements
fam[7] = 1.86
fam

# %%
fam[0:2] = ["lis", 1.74]
fam

# %% [markdown]
# #### Adding and removing elements

# %%
fam + ["me", 1.79]

# %%
fam_ext = fam + ["me", 1.79]

# %% [markdown]
# Removing

# %%
del(fam[2])

# %%
fam

# %%
x = ["a", "b", "c"]
y = x

# %%
y

# %%
y[1] = "z"

# %%
y

# %%
x

# %%
#Copy value, bukan sekalian reference 
x = ["a", "b", "c"]
y = list(x)
y = x[:]

# %%
y

# %%
y[1] = "i"

# %%
y 

# %%
x

# %% [markdown]
# #### Replace list elements

# %%
areas = ["hallway", 11.25, "kitchen", 18.0, "living room", 20.0, "bedroom", 10.75, "bathroom", 9.50]

# %%
areas[-1] = 10.50
areas[-6] = "chill zone"

# %%
areas

# %% [markdown]
# #### Extend a list

# %%
#Add poolhouse data to areas
areas_1 = areas + ["poolhouse", 24.5]

# %%
# Add garage data to areas
areas_2 = areas_1 + ["garage", 15.45]

# %%
areas_2

# %% [markdown]
# ## Functions and Packages

# %% [markdown]
# ### Functions

# %% [markdown]
# - Nothing new !
# - type() merupakan salah satu function 
# - piece of reusable code
# - call function instead writing code yourself
# - built-in function = function yang sudah disediakan python

# %%
fam = [1.73, 1.68, 1.71, 1.89]
fam

# %%
max(fam)
#mencari nilai tertinggi dalam list

# %%
tallest = max(fam)
tallest

# %%
round(1.68, 1)
#membulatkan angka menjadi 1 angka dibelakang coma

# %%
round(1.51)
#otomatis membulatkan bilangan ke integer terdekat

# %%
help(round)

# %%
var1 = [1, 2, 3, 4]
var2 = True

# %%
print(type(var1))

# %%
print(len(var1))
#

# %%
out2 = int(var2)

# %%
print(type(out2))

# %%
help(complex)

# %% [markdown]
# #### Multiple Arguments

# %%
first = [11.25, 18.0, 20.0]
second = [10.75, 9.50]

# %%
full = first + second 
full

# %%
full_sorted = sorted(full, key=None, reverse=True)
#Fungsi sorted digunakan untuk mengurutkan. 
#Parameter reverse= True digunakan untuk mengurutkan dari yang terbesar ke terkecil

# %%
full_sorted

# %% [markdown]
# ### Methods

# %% [markdown]
# sister = "liz"
# "liz" merupakan object yang bertipe data str.

# %% [markdown]
# Method : Functions that belong to objects

# %% [markdown]
# object yang bertipe data juga mempunyai methods.
# 
# - str   - capitalize(), replace().
# - float - bit_length(), conjugate().
# - list  - index(), count()

# %% [markdown]
# - Everything = object 
# - Object memiliki methods yang berhubungan, tergantung pada tipe data

# %%
fam

# %%
fam.index(1.68)

# %%
fam.count(1.71)

# %% [markdown]
# #### str methods

# %%
sister = "Lis"

# %%
sister.capitalize()

# %%
sister.replace("s", "z")

# %%
fam = ['lis', 1.73, "emma", 1.68, "mom", 1.71, "dad", 1.89]

# %%
fam.append("me")
#menambahkan element baru ke list

# %%
fam

# %%
fam.append(1.65)

# %%
fam

# %%
place = "poolhouse"

# %%
place_up = place.upper()
#digunakan untuk merubah string menjadi uppercase
place_up

# %%
print(place.count('o'))

# %% [markdown]
# #### List methods

# %%
areas = [11.25, 18.0, 20.0, 10.75, 9.50]

# %%
print(areas.index(20.0))
#print dimana element 20.0 berada

# %%
print(areas.count(9.50))
#menghitung seberapa banyak 9.50 muncul dalam list

# %%
areas.append(24.5)
areas.append(15.45)

# %%
areas

# %%
areas.reverse()
#membalikkan urutan list

# %%
areas

# %% [markdown]
# ### Packages

# %% [markdown]
# pkg/
# - mod1.py
# - mod2.py

# %% [markdown]
# - Directory of python scripts
# - each scipt = module 
# - specify functions, methods, types 
# - thousands of package available
#         - numpy
#         - matplotlib
#         - scikit-learn

# %% [markdown]
# #### Import packages

# %%
import numpy
numpy.array([1, 2, 3])

# %%
numpy.array([1, 2, 3])

# %%
import numpy as np
np.array([1, 2, 3])
#array merupakan salah satu contoh function dalam package numpy

# %%
#Hanya import array function pada numpy package
from numpy import array
array([1, 2, 3])

# %%
import math

r = 0.43 
keliling_lingkaran = 2 * math.pi * r
luas_lingkaran = math.pi * r ** 2

# %%
print("Keliling lingkaran : " + str(keliling_lingkaran))

# %%
print("Luas lingkaran : " + str(luas_lingkaran))

# %%
from math import radians

r = 192500
phi = radians(12)
dist = r * phi

# %%
dist

# %%
from scipy.linalg import inv as my_inv

# %% [markdown]
# - scipy  = package
# - linalg = sub-package
# - inv    = function
# - my_inv = alias dari inv function

# %% [markdown]
# ## Numpy

# %% [markdown]
# - Alternative dari python list = Numpy Array
# - Numpy array dapat digunakan untuk kalkulasi keseluruhan array
# - Lebih cepat dan mudah

# %%
import numpy as np 

height = [1.73, 1.68, 1.71, 1.89]
np_height = np.array(height)

weight = [65.4, 63.6, 88.4, 68.7]
np_weight = np.array(weight)

# %%
np_height

# %%
for index,x in enumerate(np_height):
    print(index + 1, x)

# %%
np_weight

# %%
type(np_height)

# %%
bmi = np_weight / np_height ** 2
bmi

# %% [markdown]
# Numpy array hanya bisa mengandung satu jenis tipe data, jika terdapat tipe data yang berbeda, maka akan dikonversi ke str.

# %%
python_list = [1, 2, 3]
numpy_array = np.array([1, 2, 3])

# %%
python_list + python_list

# %%
numpy_array + numpy_array

# %% [markdown]
# ### Numpy subsetting

# %%
bmi

# %%
bmi[1]

# %%
bmi > 20

# %%
bmi[bmi > 20]

# %%
baseball = [180, 215, 210, 210, 188, 176, 209, 200]

# %%
import numpy as np

np_baseball = np.array(baseball)
np_baseball

# %%
import pandas as pd

# %%
data = pd.read_csv("baseball.csv")

# %%
data

# %%
# height is available as a regular list

# Import numpy
import numpy as np

# Create a numpy array from height_in: np_height_in
np_height_in = np.array(height_in)

# Print out np_height_in
print(np_height_in)

# Convert np_height_in to m: np_height_m
np_height_m = np_height_in * 0.0254

# Print np_height_m
print(np_height_m)

# %%
# height and weight are available as regular lists

# Import numpy
import numpy as np

# Create array from height_in with metric units: np_height_m
np_height_m = np.array(height_in) * 0.0254

# Create array from weight_lb with metric units: np_weight_kg
np_weight_kg = np.array(weight_lb) * 0.453592

# Calculate the BMI: bmi
bmi = np_weight_kg / np_height_m ** 2

# Print out bmi
print(bmi)

# %%
# height and weight are available as a regular lists

# Import numpy
import numpy as np

# Calculate the BMI: bmi
np_height_m = np.array(height_in) * 0.0254
np_weight_kg = np.array(weight_lb) * 0.453592
bmi = np_weight_kg / np_height_m ** 2

# Create the light array
light = np.array(bmi < 21)

# Print out light
print(light)

# Print out BMIs of all baseball players whose BMI is below 21
print(bmi[bmi < 21])

# %%
np.array([True, 1, 2]) + np.array([3, 4, False])

# %%
x = np.array([4, 3, 0]) + np.array([0, 2, 2])

# %%
# height and weight are available as a regular lists

# Import numpy
import numpy as np

# Store weight and height lists as numpy arrays
np_weight_lb = np.array(weight_lb)
np_height_in = np.array(height_in)

# Print out the weight at index 50
print(np.array(np_weight_lb[50]))

# Print out sub-array of np_height_in: index 100 up to and including index 110
print(np.array(np_height_in[100: 111]))

# %% [markdown]
# ### 2D Numpy Arrays

# %%
np_2d = np.array([[1.73, 1.68, 1.71, 1.89, 1.79], 
                 [65.4, 59.2, 63.6, 88.4, 68.7]])
np_2d

# %%
np_2d.shape
# 2 baris 5 kolom

# %%
np_2d[0][2]

# %%
np_2d[0,4]

# %%
np_2d[:, 1:3]
#ambil semua baris namun hanya kolom index 1-2

# %%
np_2d[1, :]

# %%
baseball = [[180, 78.4],
            [215, 102.7],
            [210, 98.5],
            [188, 75.2]]

# %%
np_baseball = np.array(baseball)

# %%
np_baseball

# %%
np_baseball.shape

# %%
# baseball is available as a regular list of lists

# Import numpy package
import numpy as np

# Create np_baseball (2 cols)(kolom 1 = tinggi, kolom 2 = berat)
np_baseball = np.array(baseball)

# Print out the 50th row of np_baseball
print(np_baseball[49, :])

# Select the entire second column of np_baseball: np_weight_lb
np_weight_lb = np_baseball[:, 1]

# Print out height of 124th player
print(np_baseball[123, 0])

# %% [markdown]
# ### Numpy : Basic Statistics

# %%
np.mean(np_city[:, 0])
#mencari mean dari data kolom 1

np.median(np_city[:, 0])
#mencari median dari data kolom 1

np.corrcoef(np_city[:, 0], np_city[:, 1])
#mencari apakah kolom 1 dan kolom 2 berkorelasi

np.std(np_city[:, 0])
#mencari standar deviasi kolom 1

sort()
sum()

np.random.normal()

# %%
# heights and positions are available as lists

# Import numpy
import numpy as np

# Convert positions and heights to numpy arrays: np_positions, np_heights
np_positions = np.array(positions)
np_heights = np.array(heights)


# Heights of the goalkeepers: gk_heights
gk_heights = np_heights[np_heights == 'GK']

# Heights of the other players: other_heights
other_heights = np_heights[np_heights != 'GK']

# Print out the median height of goalkeepers. Replace 'None'
print("Median height of goalkeepers: " + str(np.median(gk_heights)))

# Print out the median height of other players. Replace 'None'
print("Median height of other players: " + str(np.median(other_heights)))

# %% [markdown]
# ## Practice

# %% [markdown]
# - Teknik yang digunakan untuk subset list = Square brackets
# - Method yang mengubah list jika merekea dipanggil = append(), remove(), reverse()

# %%
import numpy as np

x = np.random.normal()

# %%
p ="True"
print(type(p))

# %%
import numpy as np

np_2nd = np.array([[2, 3], [4, 5]])
print(np_2nd[-2, 1])

# %%
import numpy as np

store = np.array(["X", "Z", "Z", "Z"])
cost = np.array([2, 6, 5, 2])
select_cost = cost[store == "X"]
print(select_cost)

# %%
x = np.array([[2,3], [4,5]])
y = np.array([2, 0])

# %%
x.shape

# %%
y.shape


