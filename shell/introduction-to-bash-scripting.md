## [Introduction to Bash Scripting](https://app.datacamp.com/learn/courses/introduction-to-bash-scripting)


### Bash 
```
Bourne Again Shell
- Develop in the 80's but a very popular shell today. Default in many Unix systems, Macs
- Unix is the internet! (Running ML models, data pipelines).
```

### Why Bash scripting?
```
- Ease of execution of shell commands 
- Powerfurl programming constructs
```

```bash
# open book.txt, search for Sydney Carton or Charles Darnay, count the lines
cat book.txt | grep -E "Sydney Carton|Charles Darnay" | wc -l 

# counts the number of occurrences of each animal in animals.txt
cat animals.txt | cut -d " " -f 2 | sort | uniq -c

# counts the number of occurrences of each team in soccer_scores.csv
cat soccer_scores.csv | cut -d "," -f 2 | tail -n +2 | sort | uniq -c
```

### Bash scripting
```
Anatomy:
- Begins with #!/usr/bash (shebang)
- Middle line contain code 
- File extension .sh
- Run with bash filename.sh or ./filename.sh
```

**example.sh**
```bash
#!/bin/bash
echo "Hello World"
echo "Goodby World"
```

```bash
bash ./example.sh # run the script

# Output:
# Hello World
# Goodby World
```

```bash 
# sed (stream editor)
    # g (global) -> replace all occurrences
    # gi (case insensitive)
    # p (print) -> print only lines that have been changed
    # w (write) -> write the output to a file

sed 's/error/warning/g' file.txt                # change "error" to "warning" in file.txt
sed 's/error/warning/gi' file.txt               # change "error" to "warning" in file.txt case insensitive
sed -n 's/error/warning/p' file.txt             # print only lines that have been changed
sed 's/error/warning/w output.txt' file.txt     # write the output to output.txt
```

```sh
#!/bin/bash

# replaces Cherno with Cherno City and Arda with Arda United in soccer_scores.csv and writes to soccer_scores_edited.csv
cat soccer_scores.csv | sed 's/Cherno/Cherno City/g' | sed 's/Arda/Arda United/g' > soccer_scores_edited.csv
```

### Standard streams and arguments
```
STDIN  -> Standard Input  
STDOUT -> Standard Output  
STDERR -> Standard Error  

STDIN vs ARGV
- STDIN                                         -> cat file.txt | cut -d " " -f 2
- ARGV used for passing arguments to the script -> bash script.sh arg1 arg2

$@          -> all arguments
$*          -> all arguments as a single string
$#          -> number of arguments
$1, $2, ... -> first, second, ... arguments
```

### Bash script with arguments
**args.sh**
```bash
#!/bin/bash
echo $1
echo $2
echo $@
echo "There are " $# " arguments"
```

```bash
bash ./args.sh one two three four five

# Output:
# one
# two
# one two three four five
# There are 5 arguments
```

```bash
# Echo the first ARGV argument
echo $1

# Cat all the files
# Then pipe to grep using the first ARGV argument
# Then write out to a named csv using the first ARGV argument
cat hire_data/* | grep "$1" > "$1".csv
```

### Basic variables in Bash 
```bash
var1='Moon'
echo $var1

# Output:
# Moon
```

```bash
firstname='Nia'
lastname='Ulan Sari'
echo "Hi there $firstname $lastname"

# Output:
# Hi there Nia Ulan Sari
```

```
Beware of adding spaces in variable assignment
var1 = "Moon" -> error

single quotation marks -> literal string
double quotation marks -> variable substitution
backticks -> command substitution STDOUT
```

```bash
now_var='NOW'
now_var_singlequote='@now_var'
echo $now_var_singlequote

# Output:
# @now_var
```

```bash
now_var='NOW'
now_var_doublequote="@now_var"
echo $now_var_doublequote

# Output:
# NOW
```

```bash
now_var='NOW'
now_var_backticks="The data is `date`" # same as $(date)
echo $now_var_backticks

# Output:
# The data is Thu 15 Jul 2021 10:00:00 AM WIB
```

```bash
# Create the required variable
yourname="Sam"

# Print out the assigned name (Help fix this error!)
echo "Hi there "$yourname", welcome to the website!"

# Output:
# Hi there Sam, welcome to the website!
```

### Number variables in Bash
```bash
expr 1 + 1 
# Output:
# 2

# bc -> arbitrary precision calculator
echo "5 + 7.5" | bc 
# Output:
# 12.5

echo "scale=3; 5 / 3" | bc
# Output:
# 1.666

expr 5 + 7
# Output:
# 12

echo $((5 + 7)) 
# Output:
# 12
```

```bash
model1=57.54
model2=57.87
echo "The total score is $(echo "$model1 + $modell2" | bc)"
echo "The total score is $((model1 + model2))"

# Output:
# The total score is 115.41
# The total score is 115.41
```

```bash
temp_f=$1

temp_f2=$(echo "scale=2; $temp_f - 32" | bc)
temp_c=$(echo "scale=2; $temp_f2 * 5 / 9" | bc)

echo $temp_c

# Output:
# 37.22
```

### Arrays in Bash 
```
Two types array in Bash
- Indexed array
- Associative array
```

### Indexed array
```
- Declare with ()
- Access with []
- Assign with =()
```

```bash
arr=(1 2 3 4 5)     # no commas
echo ${arr[0]}      
echo ${arr[@]}      
echo ${#arr[@]}     
echo ${arr[@]:1:2}  

# Output:
# 1
# 1 2 3 4 5
# 5
# 2 3 (from index 1 to 2)

arr+=(6) # add 6 to the end of the array
echo ${arr[@]} 

# Output:
# 1 2 3 4 5 6
```

### Associative array
```
- Declare with -A
- Access with []
- Assign with =()
```

```bash
declare -A arr
arr[fruit]=apple
arr[veggie]=carrot
echo ${arr[fruit]}

# Output:
# apple
```

```bash
declare -A city_details
city_details=([city]="Tokyo" [population]=10000000 [city_code]=03)
echo ${city_details[city]} 
echo ${!city_details[@]}  # keys
echo ${city_details[@]}   # values

# Output:
# Tokyo
# city population city_code
# Tokyo 10000000 03
```

```bash
# Create a normal array with the mentioned elements
capital_cities=("Sydney" "Albany" "Paris")
```

```bash
# Create a normal array with the mentioned elements using the declare method
declare -a capital_cities

# Add (append) the elements
capital_cities+=("Sydney")
capital_cities+=("Albany")
capital_cities+=("Paris")
```

```bash
# The array has been created for you
capital_cities=("Sydney" "Albany" "Paris")

# Print out the entire array
echo ${capital_cities[@]}

# Print out the array length
echo ${#capital_cities[@]}

# Output:
# Sydney Albany Paris
# 3
```

```bash
# Create empty associative array
declare -A model_metrics

# Add the key-value pairs
model_metrics[model_accuracy]=98
model_metrics[model_name]="knn"
model_metrics[model_f1]=0.82

# Declare associative array with key-value pairs on one line
declare -A model_metrics=([model_accuracy]=98 [model_name]="knn" [model_f1]=0.82)

# Print out the entire array
echo ${model_metrics[@]}

# Output:
# 98 knn 0.82
```

```bash
# Create variables from the temperature data files
temp_b="$(cat temps/region_B)"
temp_c="$(cat temps/region_C)"

# Create an array with these variables as elements
region_temps=($temp_b $temp_c)

# Call an external program to get average temperature
average_temp=$(echo "scale=2; (${region_temps[0]} + ${region_temps[1]}) / 2" | bc)

# Append average temp to the array
region_temps+=($average_temp)

# Print out the whole array
echo ${region_temps[@]}
```

### Control statements in Bash scripting
```bash
if [ condition ]; then
    # code
elif [ condition ]; then
    # code
else
    # code
fi
```

```bash
x="Queen"
if [$x == "King"]; then
    echo "$x is a King!"
else 
    echo "$x is not a King!"
fi

# Output:
# Queen is not a King!
```

```bash
x=10
if ($x > 5); then
    echo "$x is greater than 5"
else
    echo "$x is not greater than 5"
fi

# Output:
# 10 is greater than 5
```

```
-eq -> equal
-ne -> not equal
-gt -> greater than
-ge -> greater than or equal
-lt -> less than
-le -> less than or equal

-e -> file exists
-s -> file exists and is not empty
-r -> file exists and is readable
-w -> file exists and is writable

&& -> and
|| -> or
```

```bash
# Extract Accuracy from first ARGV element
accuracy=$(grep Accuracy $1 | sed 's/.* //')

# Conditionally move into good_models folder
if [ $accuracy -ge 90 ]; then
    mv $1 good_models/
fi

# Conditionally move into bad_models folder
if [ $accuracy -lt 90 ]; then
    mv $1 bad_models/
fi
```

```bash
# Create variable from first ARGV element
sfile=$1

# Create an IF statement on sfile's contents
if grep -q 'SRVM_' $sfile && grep -q 'vpt' $sfile; then
	
    # Move file if matched
	mv $sfile good_logs/
    
fi
```

### For loop and while loop in Bash
```bash
for x in 1 2 3
do 
    echo $x
done

# Output:
# 1
# 2
# 3
```

```bash
for x in {1..5..2} # start end increment
do 
    echo $x
done

# Output:
# 1
# 3
# 5
```

```bash
for ((x=2;x<=4;x++))
do 
    echo $x
done
```

```bash
for book in books/*
do 
    echo $book
done
```

```bash

for book in $(ls book/ | grep -i 'air)
do 
    echo $book
done
```

```bash
x=1
while [ $x -le 3 ];
do 
    echo $x
    ((x+=1))
done

# Output:
# 1
# 2
```

```bash
# Create a FOR statement on files in directory
for file in robs_files/*.py
do 
    # Create IF statement using grep
    if grep -q 'RandomForestClassifier' $file ; then
        # Move wanted files to to_keep/ folder
        mv $file to_keep/
    fi
done
```

```bash
# Use a FOR loop on files in directory
for file in inherited_folder/*.R
do 
    # Echo out each file
    echo $file
done
```

### CASE statement in Bash
```
if a file contains sydney       -> move to sydney folder
if a file contains melbourne    -> delete the file
if a file contains canberra then rename it to IMPORTANT_filename where filename was the original filename
```

```bash
case 'SRINGVAR' in 
    PATTERN1)
    COMMAND1;;
    PATTERN2)
    COMMAND2;;
*)
DEFAULT COMMAND;;
esac
```

### Multiple if
```bash
if grep -q 'sydney $1; then 
    mv $1 sydney/
fi

if grep -q 'melbourne' $1; then 
    rm $1
fi

if grep -q 'canberra' $1; then 
    mv $1 IMPORTANT_$1
fi
```

### CASE statement
```bash
case $(cat $1) in 
    *sydney*)
    mv $1 sydney/ ;;
    *melbourne*|*brisbane*)
    rm $1 ;;
    *canbera*)
    mv $1 "IMPORTANT_$1" ;;
*)
echo "No cities found";;
esac
```

```bash
# Create a CASE statement matching the first ARGV element
case $1 in
  # Match on all weekdays
  Monday|Tuesday|Wednesday|Thursday|Friday)
  echo "It is a Weekday!" ;;
  # Match on all weekend days
  Saturday|Sunday)
  echo "It is a Weekend!" ;;
  # Create a default
  *) 
  echo "Not a day!" ;;
esac
```

```bash
# Use a FOR loop for each file in 'model_out/'
for file in model_out/*
do
    # Create a CASE statement for each file's contents
    case $(cat $file) in
      # Match on tree and non-tree models
      *"Random Forest"*|*GBM*|*XGBoost*)
      mv $file tree_models/ ;;
      *KNN*|*Logistic*)
      rm $file ;;
      # Create a default
      *) 
      echo "Unknown model in $file" ;;
    esac
done
```

### Function and Automation in Bash
```bash
function_name() {
    # code
    return #something
}
```

```bash
function print_hello () {
    echo "Hello World"  
}
print_hello

# Output:
# Hello World
```

```bash
temp_f=30
function convert_temp() {
    temp_c=$(echo "scale=2; ($temp_f - 32) * 5 / 9" | bc)
    echo $temp_c
}
```

### Create function
```bash
function upload_to_cloud () {
  # Loop through files with glob expansion
  for file in output_dir/*results*
  do
    # Echo that they are being uploaded
    echo "Uploading $file to cloud"
  done
}

# Call the function
upload_to_cloud
```

```bash
# Create function
function what_day_is_it {

  # Parse the results of date
  current_day=$(date | cut -d " " -f1)

  # Echo the result
  echo $current_day
}

# Call the function
what_day_is_it
```

### Global and local variables
```bash
function print_filename {
    first_filename=$1 # global
}
print_filename "file1.txt"
echo $first_filename
-
# Output:
# file1.txt
```

```bash
function print_filename {
    local first_filename=$1 # local
}
print_filename "file1.txt"
echo $first_filename

# Output:
# Error
```

```bash
function convert_temp () {
    echo $(echo "scale=2; ($1 - 32) * 5 / 9" | bc)
}
converted=$(convert_temp 30)
echo $converted

# Output:
# 1.11
```

```bash
# Create a function 
function return_percentage () {

  # Calculate the percentage using bc
  percent=$(echo "scale=2; 100 * $1 / $2" | bc)

  # Return the calculated percentage
  echo $percent
}

# Call the function with 456 and 632 and echo the result
return_test=$(return_percentage 456 632)
echo "456 out of 632 as a percent is $return_test%"

# Output:
# 456 out of 632 as a percent is 72.15%
```

```bash
# Create a function
function get_number_wins () {

  # Filter aggregate results by argument
  win_stats=$(cat soccer_scores.csv | cut -d "," -f2 | egrep -v 'Winner'| sort | uniq -c | egrep "$1")

}

# Call the function with specified argument
get_number_wins "Etar"

# Print out the global variable
echo "The aggregated stats are: $win_stats"
```

```bash
# Create a function with a local base variable
function sum_array () {
  local sum=0
  # Loop through, adding to base variable
  for number in "$@"
  do
    sum=$(echo "$sum + $number" | bc)
  done
  # Echo back the result
  echo $sum
  }
# Call function with array
test_array=(14 12 23.5 16 19.34)
total=$(sum_array "${test_array[@]}")
echo "The total sum of the test array is $total"

# Output:
# The total sum of the test array is 84.84
```

### Cronjob
```
Crontab -> file that contains the schedule of cron jobs
Crontab 
    -l -> list cron jobs
    -e -> edit cron jobs
    -r -> remove cron jobs

* * * * * command 
- minute (0-59)
- hour (0-23)
- day of month (1-31)
- month (1-12)
- day of week (0-7) (0-6 -> Sunday-Saturday)/ (7 -> Sunday)
```

```bash
5 1 * * * bash myscript.sh          # run myscript.sh at 1:05 AM every day
15 14 * * 7 bash myscript.sh        # run myscript.sh at 2:15 PM every Sunday
15,30,45 * * * * bash myscript.sh   # run myscript.sh at 15, 30, 45 minutes past the hour every day
*/15 * * * * bash myscript.sh       # run myscript.sh every 15 minutes every day
0 0 1 * * bash myscript.sh          # run myscript.sh at midnight on the first of every month
```

```bash
# Create a schedule for 30 minutes past 2am every day
30 2  * * * bash script1.sh

# Create a schedule for every 15, 30 and 45 minutes past the hour
15,30,45 * * * *  bash script2.sh

# Create a schedule for 11.30pm on Sunday evening, every week
30 23 * * 0 bash script3.sh
```