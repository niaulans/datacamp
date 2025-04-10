## [Writing Efficient Python Code](https://app.datacamp.com/learn/courses/writing-efficient-python-code)

### Definition efficient
```
- Fast runtime (minimal completion time)
- Small memory footprint (Minimal resource consumption)
- Focus on readability
- Using Python constructs as intended
```

### Create list using the Non-Pythonic approach
```python
i = 0
new_list= []
while i < len(names):
    if len(names[i]) >= 6:
        new_list.append(names[i])
    i += 1
print(new_list)
```

### Create list by looping over the contents of names
```python
better_list = []
for name in names:
    if len(name) >= 6:
        better_list.append(name)
print(better_list)
```

### Create list by using list comprehension
```python
best_list = [name for name in names if len(name) >= 6]
print(best_list)
```

### The Python Standard library
```
Built-in types:
- list, tuple, set, dict, and others

Built-in functions:
- print(), len(), range(), round(), enumerate(), map(), zip(), and others.

Built-in modules:
- os, sys, ietrtools, collections, math, and others.
```

### range()
```python
# range with start and end
nums = range(1, 11)
num_list = list(nums)
print(num_list)

# Output:
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# range with just end
nums = range(11)
num_list = list(nums)
print(num_list)

# Output:
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# range with start, end, and step
even_nums = range(2, 11, 2) # start from 2, end at 11, step 2
even_nums_list = list(even_nums)
print(even_nums_list)

# Output:
# [2, 4, 6, 8, 10]
```

### enumerate()
```python
letters = ['a', 'b', 'c', 'd']
indexed_letters = enumerate(letters)
indexed_letters_list = list(indexed_letters)
print(indexed_letters_list)

# Output:
# [(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]

# enumerate with start index
letters = ['a', 'b', 'c', 'd']
indexed_letters = enumerate(letters, start=5)
indexed_letters_list = list(indexed_letters)
print(indexed_letters_list)

# Output:
# [(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
```

### maps()
```python
nums = [1.5, 2.3, 3.4, 4.6]
rnd_nums = map(round, nums)
print(list(rnd_nums))

# Output:
# [2, 2, 3, 5]

nums = [1, 2, 3, 4]
sqrd_numms = map(lambda x: x ** 2, nums)
print(list(sqrd_numms))

# Output:
# [1, 4, 9, 16]
```

### Unpacking range object to list
```python
# Create a range object that goes from 0 to 5
nums = range(0, 6)
print(type(nums))

# Convert nums to a list
nums_list = list(nums)
print(nums_list)

# Create a new list of odd numbers from 1 to 11 by unpacking a range object
nums_list2 = [*range(1, 12, 2)] # unpacking range object to list
print(nums_list2)
```
---------------------------------------------------------------
# Rewrite the for loop to use enumerate
indexed_names = []
for i,name in enumerate(names):
    index_name = (i,name)
    indexed_names.append(index_name) 
print(indexed_names)

# Rewrite the above for loop using list comprehension
indexed_names_comp = [(i, name) for i,name in enumerate(names)]
print(indexed_names_comp)

# Unpack an enumerate object with a starting index of one
indexed_names_unpack = [*enumerate(names, 1)]
print(indexed_names_unpack)

---------------------------------------------------------------
# Use map to apply str.upper to each element in names
names_map  = map(lambda x: x.upper(), names)

# Print the type of the names_map
print(type(names_map))

# Unpack names_map into a list
names_uppercase = [*map(lambda x: x.upper(), names)]

# Print the list created above
print(names_uppercase)
---------------------------------------------------------------
Numpy arrays:

nums_list = list(range(5))

import numpy as np
nums_np = np.array(range(5))
nums_np.dtype

numpy arrays: homogeneous data types

---------------------------------------------------------------
sqrd_nums = []
for nums in nums:
  sqrd_numms.append(num ** 2)
print(sqrd_nums)

sqrd_nums = [num ** 2 for num in nums]
print(sqrd_nums)

nums_np = np.array([-2, -1, 0, 1, 2])
nums_np ** 2

nums_np > 0
-> array([False, False, False, True, True])

nums_np[nums_np > 0]
-> array([1, 2])

---------------------------------------------------------------
# Print second row of nums
print(np.array(nums[1,:]))

# Print all elements of nums that are greater than six
print(nums[nums > 6])

# Double every element of nums
nums_dbl = np.array(nums) * 2
print(nums_dbl)

# Replace the third column of nums
nums[:,2] = nums[:, 2] + 1
print(nums)
---------------------------------------------------------------
# Create a list of arrival times
arrival_times = [*range(10,60,10)]

# Convert arrival_times to an array and update the times
arrival_times_np = np.array(arrival_times)
new_times = arrival_times_np - 3

# Use list comprehension and enumerate to pair guests to new times
guest_arrivals = [(names[i],time) for i,time in enumerate(new_times)]

# Map the welcome_guest function to each (guest,time) pair
welcome_map = map(lambda x: welcome_guest(x), guest_arrivals)

guest_welcomes = [*welcome_map]
print(*guest_welcomes, sep='\n')
---------------------------------------------------------------
Timing and profiling code

Examining runtime:
Calculate runtime with IPyhton magic comans %timeit

%lsmagic -> list all available magic commands

import numpy as np
rand_nums = np.random.rand(1000)
%timeit rand_nums = np.random.rand(1000)

# Set numbers of runs to 2 (-r2)
# Set number of loops to 10 (-n10)

%timeit -r2 -n10 rand_nums = np.random.rand(1000)

%%timeit -> time the entire cell
times = %timeit -o rand_nums = np.random.rand(1000)

times.timings -> list of all runtimes
times.best -> best runtime
times.worst -> worst runtime
times.average -> average runtime
times.stdev -> standard deviation of runtimes

---------------------------------------------------------------
Code profiling for runtime
- Detailed stats on frequency and duration of function calls
- Line by line analysis of code runtime
- Package used for profiling: line_profiler

pip install line_profiler

def convert_units(heroes, heights, weights):
  new_hts = [ht * 0.39370 for ht in heights]
  new_wts = [wt * 2.20462 for wt in weights]

  hero_data = {}

  for i, hero in enumerate(heroes):
    hero_data[hero] = (new_hts[i], new_wts[i])

  return hero_data

%load_ext line_profiler
%lprun -f convert_units convert_units(heroes, hts, wts)

---------------------------------------------------------------
Code profiling for memory usage

import sys 

num_list = [*range(1000)]
sys.getsizeof(num_list) -> get memory usage of num_list
-> 9112

nums_np = np.array(range(1000))
sys.getsizeof(nums_np) -> get memory usage of nums_np
-> 8096

memory profiler package:

from hero_funcs import convert_units

%load_ext memory_profiler
%mprun -f convert_units convert_units(heroes, hts, wts)

---------------------------------------------------------------
def calc_bmi_lists(sample_indices, hts, wts):

    # Gather sample heights and weights as lists
    s_hts = [hts[i] for i in sample_indices]
    s_wts = [wts[i] for i in sample_indices]

    # Convert heights from cm to m and square with list comprehension
    s_hts_m_sqr = [(ht / 100) ** 2 for ht in s_hts]

    # Calculate BMIs as a list with list comprehension
    bmis = [s_wts[i] / s_hts_m_sqr[i] for i in range(len(sample_indices))]

    return bmis

---------------------------------------------------------------
def get_publisher_heroes(heroes, publishers, desired_publisher):

    desired_heroes = []

    for i,pub in enumerate(publishers):
        if pub == desired_publisher:
            desired_heroes.append(heroes[i])

    return desired_heroes
def get_publisher_heroes_np(heroes, publishers, desired_publisher):

    heroes_np = np.array(heroes)
    pubs_np = np.array(publishers)

    desired_heroes = heroes_np[pubs_np == desired_publisher]

    return desired_heroes

# Use get_publisher_heroes() to gather Star Wars heroes
star_wars_heroes = get_publisher_heroes(heroes, publishers, 'George Lucas')

print(star_wars_heroes)
print(type(star_wars_heroes))

# Use get_publisher_heroes_np() to gather Star Wars heroes
star_wars_heroes_np = get_publisher_heroes_np(heroes, publishers, 'George Lucas')

print(star_wars_heroes_np)
print(type(star_wars_heroes_np))

---------------------------------------------------------------
Gaining efficiencies

names = ['Bulbasaur', 'Charmander', 'Squirtle']
hps = [45, 39, 44]

combines = []

for i, pokemon in enumerate(names):
  combines.append((pokemon, hps[i]))
print(combines)
-> [('Bulbasaur', 45), ('Charmander', 39), ('Squirtle', 44)]

combined_zip = zip(names, hps)
combines_zip_list = [*combined_zip]
print(combines_zip_list)

Collection module:
- namedtuple -> create tuple subclasses with named fields
- deque -> list-like container with fast appends and pops on either end
- Counter -> dict subclass for counting hashable objects
- OrderedDict -> dict subclass that remembers the order entries were added
- defaultdict -> dict subclass that calls a factory function to supply missing values

Counting with loop:
poke_types = ['Grass', 'Dark', 'Fire', 'Water', 'Fire', 'Bug', ...]
type_counts = {}
for poke_type in poke_types:
  if poke_type not in type_counts:
    type_counts[poke_type] = 1
  else:
    type_counts[poke_type] += 1
print(type_counts)


Counting with Counter:
poke_types = ['Grass', 'Dark', 'Fire', 'Water', 'Fire', 'Bug', ...]

from collections import Counter
type_counts = Counter(poke_types)


Itertools module:
- Infinite iterators: count(), cycle(), repeat()
- Finite iterators: accumulate(), chain(), zip_longest()
- Combination generators: product(), permutations(), combinations()

Combination with loop:
poke_types = ['Grass', 'Dark', 'Fire', 'Water', 'Fire', 'Bug']
combos = []

for x in poke_types:
  for y in poke_types:
    if x == y:
      continue
    if((x, y) not in combos) & ((y, x) not in combos):
      combos.append((x, y))
print(combos)

Combination with itertools:
poke_types = ['Grass', 'Dark', 'Fire', 'Water', 'Fire', 'Bug']
from itertools import combinations
combos_obj = combinations(poke_types, 2)
combos = [*combos_obj]
print(combos)

---------------------------------------------------------------
# Combine names and primary_types
names_type1 = [*zip(names, primary_types)]

print(*names_type1[:5], sep='\n')

# Combine all three lists together
names_types = [*zip(names, primary_types, secondary_types)]

print(*names_types[:5], sep='\n')

# Combine five items from names and three items from primary_types
differing_lengths = [*zip(names[:5], primary_types[:3])]

print(*differing_lengths, sep='\n')

---------------------------------------------------------------
# Collect the count of primary types
type_count = Counter(primary_types)
print(type_count, '\n')

# Collect the count of generations
gen_count = Counter(generations)
print(gen_count, '\n')

# Use list comprehension to get each Pokémon's starting letter
starting_letters = [name[0] for name in names]

# Collect the count of Pokémon for each starting_letter
starting_letters_count = Counter(starting_letters)
print(starting_letters_count)
---------------------------------------------------------------
# Import combinations from itertools
from itertools import combinations

# Create a combination object with pairs of Pokémon
combos_obj = combinations(pokemon, 2)
print(type(combos_obj), '\n')

# Convert combos_obj to a list by unpacking
combos_2 = [*combos_obj]
print(combos_2, '\n')

# Collect all possible combinations of 4 Pokémon directly into a list
combos_4 = [*combinations(pokemon, 4)]
print(combos_4)

---------------------------------------------------------------
Set theory

- intersection() -> elements in both sets
- difference() -> elements in one set but not the other
- symmetric_difference() -> all elements in exactly one set
- union() -> all elements in both sets

list_a = ['Bulbasaur', 'Charmander', 'Squirtle']
list_b = ['Caterpie', 'Pidgey', 'Squirtle']

Using loops:
in_common = []
for pokemon_a in list_a:
  for pokemon_b in list_b:
    if pokemon_a == pokemon_b:
      in_common.append(pokemon_a)

print(in_common)

Using sets:
set_a = set(list_a)
set_b = set(list_b)

in_common = set_a.intersection(set_b) -> 'Squirtle'
difference_a = set_a.difference(set_b) -> 'Bulbasaur', 'Charmander'
difference_b = set_b.difference(set_a) -> 'Caterpie', 'Pidgey'
all_pokemon = set_a.union(set_b) -> 'Bulbasaur', 'Charmander', 'Squirtle', 'Caterpie', 'Pidgey'
---------------------------------------------------------------
# Convert both lists to sets
ash_set = set(ash_pokedex)
misty_set = set(misty_pokedex)

# Find the Pokémon that exist in both sets
both = ash_set.intersection(misty_set)
print(both)

# Find the Pokémon that Ash has and Misty does not have
ash_only = ash_set.difference(misty_set)
print(ash_only)

# Find the Pokémon that are in only one set (not both)
unique_to_set = ash_set.symmetric_difference(misty_set)
print(unique_to_set)
---------------------------------------------------------------
Eliminating loops:

for loop : interate over sequence piece by piece
while loop : repeat until condition is met
nested loop : loop inside another loop

Flat is better than nested!!

poke_stats = [
  [90, 92, 75],
  [25, 20, 15],
  [65, 130, 60]
]

# Using for loop
totals = []
for row in poke_stats:
  totals.append(sum(row))

# Using list comprehension
totals_comp = [sum(row) for row in poke_stats]

# Using map
totals_map = [*map(sum, poke_stats)]

avgs_np = poke_stats.mean(axis=1)

---------------------------------------------------------------
# Collect Pokémon that belong to generation 1 or generation 2
gen1_gen2_pokemon = [name for name,gen in zip(poke_names, poke_gens) if gen < 3]

# Create a map object that stores the name lengths
name_lengths_map = map(len, gen1_gen2_pokemon)

# Combine gen1_gen2_pokemon and name_lengths_map into a list
gen1_gen2_name_lengths = [*zip(gen1_gen2_pokemon, name_lengths_map)]

print(gen1_gen2_name_lengths_loop[:5])
print(gen1_gen2_name_lengths[:5])

---------------------------------------------------------------
# Create a total stats array
total_stats_np = stats.sum(axis=1)

# Create an average stats array
avg_stats_np = stats.mean(axis=1)

# Combine names, total_stats_np, and avg_stats_np into a list
poke_list_np = [*zip(names, total_stats_np, avg_stats_np)]

print(poke_list_np == poke_list, '\n')
print(poke_list_np[:3])
print(poke_list[:3], '\n')
top_3 = sorted(poke_list_np, key=lambda x: x[1], reverse=True)[:3]
print('3 strongest Pokémon:\n{}'.format(top_3))

---------------------------------------------------------------
Writing better loops:
- Understanding what is being done with each loop iteration
- Move one-time calculations outside(above) the loop
- Use holistic conversions outside the loop
- Anything that is done once should be done outside the loop

import numpy as np

names = ['Abomasnow', 'Abra', 'Absol', 'Accelgor', 'Aerodactyl']
attacks = np.array([92, 20, 130, 70, 105])
for pokemon, attack in zip(names, attacks):
  total_attack_avg = attacks.mean()
  if attack > total_attack_avg:
    print(
      "{}'s attack: {} > average: {}!"
      .format(pokemon, attack, total_attack_avg)
    )

# Mean above or below the loop 
total_attack_avg = attacks.mean()
for pokemon, attack in zip(names, attacks):
  if attack > total_attack_avg:
    print(
      "{}'s attack: {} > average: {}!"
      .format(pokemon, attack, total_attack_avg)
    )

# Using holistic conversion
names = ['Abomasnow', 'Abra', 'Absol', 'Accelgor', 'Aerodactyl']
legend_status = [False, False, False, False, True]
generations = [4, 1, 3, 5, 1]

poke_data = []
for poke_tuple in zip(names, legend_status, generations):
  poke_list = list(poke_tuple)
  poke_data.append(poke_list)
print(poke_data)

poke_data_tuples = []
for poke_tuple in zip(names, legend_status, generations):
  poke_data_tuples.append(poke_tuple)
poke_data = [*map(list, poke_data_tuples)]

---------------------------------------------------------------

for gen,count in gen_counts.items():
    total_count = len(generations)
    gen_percent = round(count / total_count * 100, 2)
    print(
      'generation {}: count = {:3} percentage = {}'
      .format(gen, count, gen_percent)
    )

# Import Counter
from collections import Counter

# Collect the count of each generation
gen_counts = Counter(generations)

# Improve for loop by moving one calculation above the loop
total_count = []

for gen,count in gen_counts.items():
    gen_percent = round(count / total_count * 100, 2)
    print('generation {}: count = {:3} percentage = {}'
          .format(gen, count, gen_percent))
---------------------------------------------------------------
# Import Counter
from collections import Counter

# Collect the count of each generation
gen_counts = Counter(generations)

# Improve for loop by moving one calculation above the loop
total_count = len(generations)

for gen,count in gen_counts.items():
    gen_percent = round(count / total_count * 100, 2)
    print('generation {}: count = {:3} percentage = {}'
          .format(gen, count, gen_percent))
---------------------------------------------------------------
# Collect all possible pairs using combinations()
possible_pairs = [*combinations(pokemon_types, 2)]

# Create an empty list called enumerated_tuples
enumerated_tuples = []

# Append each enumerated_pair_tuple to the empty list above
for i,pair in enumerate(possible_pairs, 1):
    enumerated_pair_tuple = (i,) + pair
    enumerated_tuples.append(enumerated_pair_tuple)

# Convert all tuples in enumerated_tuples to a list
enumerated_pairs = [*map(list, enumerated_tuples)]
print(enumerated_pairs)

---------------------------------------------------------------
# Calculate the total HP avg and total HP standard deviation
hp_avg = hps.mean()
hp_std = hps.std()

# Use NumPy to eliminate the previous for loop
z_scores = (hps - hp_avg)/hp_std

# Combine names, hps, and z_scores
poke_zscores2 = [*zip(names, hps, z_scores)]
print(*poke_zscores2[:3], sep='\n')

# Calculate the total HP avg and total HP standard deviation
hp_avg = hps.mean()
hp_std = hps.std()

# Use NumPy to eliminate the previous for loop
z_scores = (hps - hp_avg)/hp_std

# Combine names, hps, and z_scores
poke_zscores2 = [*zip(names, hps, z_scores)]
print(*poke_zscores2[:3], sep='\n')

# Use list comprehension with the same logic as the highest_hp_pokemon code block
highest_hp_pokemon2 = [(name, hp, z_score) for name,hp,z_score in poke_zscores2 if z_score > 2]
print(*highest_hp_pokemon2, sep='\n')
---------------------------------------------------------------
Intro to pandas DataFrame iteration

impot numpy as np
def calc_win_per(wins, games_played):
  win_perc = wins / games_played
  return np.round(win_perc, 2)

win_perc_list = []
for in in range(len(baseball_df)):
  row = baseball_df.iloc[i]
  wins = row['W']
  games_played = row['G']
  win_perc = calc_win_per(wins, games_played)
  win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list

---------------------------------------------------------------
win_perc_list = []

for i, row in baseball_df.iterrows():
  wins = row['W']
  games_played = row['G']
  win_perc = calc_win_per(wins, games_played)
  win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list

---------------------------------------------------------------
# Iterate over pit_df and print each row
for i,row in pit_df.iterrows():
    print(row)

# Iterate over pit_df and print each index variable, row, and row type
for i,row in pit_df.iterrows():
    print(i)
    print(row)
    print(type(row))

# Use one variable instead of two to store the result of .iterrows()
for row_tuple in pit_df.iterrows():
    print(row_tuple)

# Create an empty list to store run differentials
run_diffs = []

# Write a for loop and collect runs allowed and runs scored for each row
for i,row in giants_df.iterrows():
    runs_scored = row['RS']
    runs_allowed = row['RA']
    
    # Use the provided function to calculate run_diff for each row
    run_diff = calc_run_diff(runs_scored, runs_allowed)
    
    # Append each run differential to the output list
    run_diffs.append(run_diff)

giants_df['RD'] = run_diffs
print(giants_df)

---------------------------------------------------------------
.itertuples()

for row_namedtuple in pit_df.itertuples():
    print(row_namedtuple.Team)

---------------------------------------------------------------
# Loop over the DataFrame and print each row's Index, Year and Wins (W)
for row in rangers_df.itertuples():
  i = row.Index
  year = row.Year
  wins = row.W
  
  # Check if rangers made Playoffs (1 means yes; 0 means no)
  if row.Playoffs in rangers_df.itertuples() == 1:
    print(i, year, wins)

---------------------------------------------------------------
run_diffs = []

# Loop over the DataFrame and calculate each row's run differential
for row in yankees_df.itertuples():
    
    runs_scored = row.RS
    runs_allowed = row.RA

    run_diff = calc_run_diff(runs_scored, runs_allowed)
    
    run_diffs.append(run_diff)

# Append new column
yankees_df['RD'] = run_diffs
print(yankees_df)

---------------------------------------------------------------
Pandas alternatives to looping

.apply() -> apply a function along an axis of the DataFrame
axis = 0 -> apply function to each column, vertical
axis = 1 -> apply function to each row, horizontal

✅ "0" untuk ke bawah (vertikal, baris akan diproses)
✅ "1" untuk ke samping (horizontal, kolom akan diproses)

run_diff_apply = baseball_df.apply(
  lambda row: calc_run_diff(row['RS'], row['RA']),
  axis = 1
)
baseball_df['RD'] = run_diff_apply

---------------------------------------------------------------
# Gather sum of all columns
stat_totals = rays_df.apply(sum, axis=0)
print(stat_totals)

# Gather total runs scored in all games per year
total_runs_scored = rays_df[['RS', 'RA']].apply(sum, axis=1)
print(total_runs_scored)

# Convert numeric playoffs to text by applying text_playoffs()
textual_playoffs = rays_df.apply(lambda row: text_playoffs(row['Playoffs']), axis=1)
print(textual_playoffs)

---------------------------------------------------------------
# Display the first five rows of the DataFrame
print(dbacks_df.head())

# Create a win percentage Series 
win_percs = dbacks_df.apply(lambda row: calc_win_perc(row['W'], row['G']), axis=1)
print(win_percs, '\n')

# Append a new column to dbacks_df
dbacks_df['WP'] = win_percs
print(dbacks_df, '\n')

# Display dbacks_df where WP is greater than 0.50
print(dbacks_df[dbacks_df['WP'] >= 0.50])
---------------------------------------------------------------
Optimal pandas iterating

baseball_df['RS'].values - baseball_df['RA'].values

win_perc_preds_loop = []

# Use a loop and .itertuples() to collect each row's predicted win percentage
for row in baseball_df.itertuples():
    runs_scored = row.RS
    runs_allowed = row.RA
    win_perc_pred = predict_win_perc(runs_scored, runs_allowed)
    win_perc_preds_loop.append(win_perc_pred)

# Apply predict_win_perc to each row of the DataFrame
win_perc_preds_apply = baseball_df.apply(lambda row: predict_win_perc(row['RS'], row['RA']), axis=1)

# Calculate the win percentage predictions using NumPy arrays
win_perc_preds_np = predict_win_perc(baseball_df['RS'].values, baseball_df['RA'].values)
baseball_df['WP_preds'] = win_perc_preds_np
print(baseball_df.head())

urutan teroptimal:
- numpy arrays
- .itertuples()
- .apply()