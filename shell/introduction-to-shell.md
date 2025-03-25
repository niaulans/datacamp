## [Introduction to Shell](https://app.datacamp.com/learn/courses/introduction-to-shell)

### Linux directories
| Directory             | Function                                                                               |
|-----------------------|----------------------------------------------------------------------------------------|
| `/` (root)            | The main (root) directory that serves as the starting point for all other directories. |
| `/home`               | Stores user directories (e.g., `/home/user`).                                          |
| `/root`               | The home directory for the root (administrator) user.                                  |
| `/bin`                | Contains basic binaries (executable programs) like `ls`, `cp`, `mv`, etc.              |
| `/sbin`               | Similar to `/bin`, but for administrative commands (root-only).                        |
| `/usr`                | Contains applications and additional files (e.g., `/usr/bin/`, `/usr/lib/`).           |
| `/var`                | Stores variable files such as logs (`/var/log/`), application data, etc.               |
| `/tmp`                | Temporary directory used by the system and applications.                               |
| `/etc`                | Contains system and application configuration files.                                   |
| `/dev`                | Contains device files (e.g., disks, USB drives, etc.).                                 |
| `/mnt` and `/media`   | Mount points for additional file systems like external hard drives, flash drives, etc. |
| `/opt`                | Stores additional software that is not included in the standard system.                |
| `/proc`               | Contains information about running processes and the system kernel.                    |
| `/sys`                | Provides information about system hardware.                                            |
| `/lib`                | Stores libraries required by the system and programs in `/bin` and `/sbin`.            |

### Check working directory
```bash
man <command>  # shows the manual for
man ls         # shows the manual for ls command

pwd            # shows the current working directory

ls             # shows the files and directories in the current directory
ls -R          # shows the files and directories in the current directory and all its subdirectories
ls -R -F       # shows the files and directories in the current directory and all its subdirectories with / for directories

!head         # runs the last command starting with head
!2 -          # runs the second command from the history 
history       # shows the history of commands
```

### Absolute vs relative paths
```
/home/repl/seasonal/winter.csv  -> absolute path - specifies the location of a file or directory from the root directory
winter.csv                      -> relative path - specifies the location of a file or directory from the current directory

with /      -> absolute path
without /   -> relative path
```

### Change directory
```bash
cd <path>               # changes the current directory
cd seasonal             # changes the current directory to seasonal
cd /home/repl/seasonal  # changes the current directory to /home/repl/seasonal

cd ..                   # changes the current directory to the parent directory
cd ~                    # changes the current directory to the home directory
cd ~/../.               # The path means 'home directory', 'up a level', 'here'.
```

### Create and remove files and directories
```bash
touch <file_name>       # creates a new file
touch draft.txt         # creates draft.txt file

mkdir <folder_name>     # creates a new directory
mkdir thesis            # creates thesis directory

rm <file_name>          # removes a file
rm thesis.txt           # removes thesis.txt file

rmdir <folder_name>     # removes an empty directory
rmdir thesis            # removes thesis directory (empty)

rm -r <folder_name>     # removes a directory and all its contents
rm -r thesis            # removes thesis directory and all its contents
```

### Copy and move files
```bash
cp <source> <destination>                           # copies a file to another file
cp original.txt duplicate.txt                       # copies original.txt to duplicate.txt
cp seasonal/autumn.csv seasonal/winter.csv backup/  # copies autumn.csv and winter.csv to backup directory

mv <source> <destination>                           # moves a file to another file
mv autumn.csv winter.csv ..                         # moves autumn.csv and winter.csv to the parent directory

mv <old> <new>                                      # renames a file
mv course.txt old-course.txt                        # renames course.txt to old-course.txt
```

### Show file contents
```bash
# head, tail   -> select rows
# cut          -> select columns
# grep         -> select lines according to what they contain
# wc           -> count lines, words, characters

cat <file_name>         # displays the contents of a file
cat file.txt            # displays the contents of file.txt

less <file_name>        # displays the contents of a file one page at a time
                        # spacebar -> next page
                        # :q -> quit
                        # :n -> next file
less file.txt           # displays the contents of file.txt

head <file_name>        # displays the first few lines of a file (10 by default)
head file.txt           # displays the first few lines of file.txt
head -n 3 file.txt      # displays the first 3 lines of file.txt

tail <file_name>        # displays the last few lines of a file (10 by default)
tail file.txt           # displays the last few lines of file.txt
tail -n 5 file.txt      # displays the last 5 lines of file.txt
tail -n +7 file.txt     # displays all lines starting from the 7th line until the end (1-6 are skipped)

cut -f <column_number> -d <delimiter> <file_name>  # extracts a column from a file
cut -f 2 -d , seasonal/winter.csv                  # extracts the second column from winter.csv with , as delimiter
cut -f 2-5,8 -d , values.csv                       # extracts the second to fifth and eighth columns from values.csv with , as delimiter

grep -flag <pattern> <file_name>                   # searches for a pattern in a file
grep
    -c # count occurrences
    -h # don't print filenames
    -i # ignore case
    -l # print filenames
    -n # print line numbers
    -v # invert match, show non-matching lines

grep molar seasonal/autumn.csv                    # shows lines containing molar in autumn.csv
grep -v -n molar seasonal/autumn.csv              # shows lines not containing molar in autumn.csv with line numbers
grep -c molar seasonal/autumn.csv                 # counts the number of lines containing molar in autumn.csv

wc -flag <file_name>                              # counts the number of lines, words, and characters in a file
wc 
    -l # count lines
    -w # count words
    -c # count characters

wc file.txt                                       # counts the number of lines, words, and characters in file.txt
wc -l file.txt                                    # counts the number of lines in file.txt
```

### Combining all
```
wildcards:
*       -> match zero or more characters
?       -> match a single character
[...]   -> match any one of the characters inside the square brackets
[!...]  -> match any character not inside the square brackets
{...}   -> match any of the comma-separated patterns inside the curly brackets
```

```bash
head -n 5 seasonal/summer.csv > top.csv          # extracts the first 5 lines of summer.csv and writes to top.csv
head -n 5 seasonal/summer.csv | tail -n 3        # extracts the first 5 lines of summer.csv and shows the last 3 lines
cut -f 2 -d , seasonal/winter.csv | grep -v Tooth  # extracts the second column from winter.csv and shows lines not containing Tooth
cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | head -n 1 # extracts the second column from winter.csv, shows lines not containing Tooth, and shows the first line
grep 2017-07 seasonal/winter.csv | wc -l ->      # counts the number of lines containing 2017-07 in winter.csv 

head -n 3 seasonal/s*.csv        # shows the first 3 lines of all csv files starting with s in seasonal
[singh, johel]{*.csv, *.txt}     # shows all csv and txt files with singh or johel in the name
```

### Sorting and unique lines
```bash
sort -flag <file_name>                           # sorts the lines in a file
sort 
    -n # sort numerically
    -r # sort in reverse order
    -b # ignore leading blanks
    -f # fold case
    -o # write output to file

cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | sort -r # extracts the second column from winter.csv, shows lines not containing Tooth, and sorts in reverse order

uniq -flag <file_name>                           # removes adjacent duplicate lines in a file
uniq 
    -c # count occurrences
    -i # ignore case
    -d # only show duplicate lines
    -u # only show unique lines

cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | sort | uniq -c # extracts the second column from winter.csv, shows lines not containing Tooth, sorts, and counts occurrences
```

### Batch processing
```sh
echo $USER                                     # shows the current user -> repl

testing=seasonal/winter.csv                    # assigns seasonal/winter.csv to testing
head -n 1 $testing                             # shows the first line of the file assigned to testing
```

```sh
for filetype in gif jpg png; 
    do echo $filetype; # prints gif, jpg, png
done  
```

```sh
for filetype in docx odt pdf; 
    do echo $filetype; # prints docx, odt, pdf
done  
```

```sh
for file in seasonal/*.csv;  # loops through all csv files in seasonal
    do head -n 2 $file | tail -n 1 # shows the second line of each file
done
```

```sh
for f in seasonal/*.csv; 
    do echo $f; head -n 2 $f | tail -n 1;  # shows the second line of each csv file in seasonal
done
```

### Shell shortcuts
```bash
ctrl+k # delete from cursor to end of line
ctrl+u # delete from cursor to start of line
ctrl+o # save and run the command
ctrl+x # cancel the command
```

### Shell scripts
```sh
#!/bin/bash
# all-dates.sh
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq # extracts the first column from all csv files in seasonal, shows lines not containing Date, sorts, and removes duplicates
```

```bash
all-dates.sh > dates.out        # writes the output of all-dates.sh to dates.out
```

```sh
#!/bin/bash
tail -q -n +2 $@ | wc -l        # extracts all lines except the first line from all files specified by the arguments and counts the number of lines
```

```bash
count-records.sh seasonal/*.csv > num-records.out       # counts the number of lines in all csv files in seasonal except the first line and writes to num-records.out
```

## Arguments
```sh
#!/bin/bash

# $# -> number of arguments
cut -d , -f $2 $1 # extracts the column specified by the second argument from the file specified by the first argument
```

```bash
get-field.sh seasonal/summer.csv 4 2 # extracts the second column from summer.csv
```