## [Introduction to Shell](https://app.datacamp.com/learn/courses/introduction-to-shell)

--------------------------------------------------------------------
INTRODUCTION TO SHELL
--------------------------------------------------------------------
Manipulating files and directories

pwd - print working directory -> shows the current directory
ls - list -> shows the files and directories in the current directory

/home/repl/seasonal/winter.csv - absolute path - specifies the location of a file or directory from the root directory
winter.csv - relative path - specifies the location of a file or directory from the current directory
with / -> absolute path
without / -> relative path

cd - change directory -> changes the current directory
cd /home/repl/seasonal -> changes the current directory to /home/repl/seasonal
cd .. -> changes the current directory to the parent directory
cd ~ -> changes the current directory to the home directory
cd ~/../. -> The path means 'home directory', 'up a level', 'here'.

cp original.txt duplicate.txt -> copies a file to another file
cp seasonal/autumn.csv seasonal/winter.csv backup/ -> copies a file to a directory backup

mv autumn.csv winter.csv .. -> moves a file to another directory one level up (..)
mv course.txt old-course.txt -> renames a file

rm thesis.txt -> removes a file
rm -r thesis -> removes a directory and all its contents
rmdir thesis -> removes a directory if it is empty

mkdir thesis -> creates a new directory

Linux directories:
Direktori	Fungsi
/ (root)	Direktori utama (root) yang menjadi awal dari semua direktori lain.
/home	Menyimpan direktori pengguna (misalnya /home/user).
/root	Direktori home untuk pengguna root (administrator).
/bin	Berisi binaries (program eksekusi) dasar seperti ls, cp, mv, dll.
/sbin	Seperti /bin, tetapi untuk perintah administratif (hanya root).
/usr	Berisi aplikasi dan file tambahan (misalnya /usr/bin/, /usr/lib/).
/var	Berisi file variabel seperti log (/var/log/), data aplikasi, dll.
/tmp	Direktori sementara yang digunakan oleh sistem dan aplikasi.
/etc	Berisi file konfigurasi sistem dan aplikasi.
/dev	Berisi file perangkat (device) seperti disk, USB, dll.
/mnt dan /media	Tempat mounting (penyambungan) sistem file tambahan seperti hard disk eksternal, flash drive, dll.
/opt	Menyimpan perangkat lunak tambahan yang tidak termasuk dalam sistem standar.
/proc	Berisi informasi tentang proses yang sedang berjalan dan sistem kernel.
/sys	Menyediakan informasi tentang perangkat keras sistem.
/lib	Berisi pustaka (library) yang diperlukan oleh sistem dan program dalam /bin dan /sbin.

--------------------------------------------------------------------
Manipulating data

cat file.txt -> displays the contents of a file
less file.txt -> displays the contents of a file one page at a time
    spacebar -> next page
    :q -> quit
    :n -> next file 

head file.txt -> first few lines of a file (10)
use tab to autocomplete
head f -> tab -> head file.txt
head -n 3 file.txt -> first 3 lines of a file

tail file.txt -> last few lines of a file (10)
tail -n 5 file.txt -> last 5 lines of a file
tail -n +7 file.txt -> all lines starting from the 7th line until the end (1-6 are skipped)

ls -R -> lists all files in the current directory and all its subdirectories
ls -R -F -> lists all files in the current directory and all its subdirectories with / for directories and * for executables

man ls -> manual for ls command

cut -f 2-5,8 -d , values.csv -> extracts columns 2-5 and 8 with delimiter , from values.csv
- adding space after flag is optional

!head -> runs the last command starting with head
!2 -> runs the second command in the history
history -> shows the history of commands

head, tail -> select rows
cut -> select columns
grep -> select lines according to what they contain

grep
    -c -> count of matching lines
    -h -> do not print filenames
    -i -> case-insensitive
    -l -> print only the names of files with matching lines
    -n -> print line numbers of matching lines
    -v -> invert match, i.e., only show lines that don't match

grep molar seasonal/autumn.csv -> shows lines containing molar in autumn.csv
grep -v -n molar seasonal/autumn.csv -> shows lines not containing molar in autumn.csv with line numbers

--------------------------------------------------------------------
Combining tools

head -n 5 seasonal/summer.csv > top.csv -> writes the first 5 lines of summer.csv to top.csv
head -n 5 seasonal/summer.csv | tail -n 3 -> first 5 lines of summer.csv to tail command to get the last 3 lines
cut -f 2 -d , seasonal/winter.csv | grep -v Tooth -> extracts the second column from winter.csv and shows lines not containing Tooth
cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | head -n 1 -> extracts the second column from winter.csv, shows lines not containing Tooth, and shows the first line

wc 
    -l -> count lines
    -w -> count words
    -c -> count characters 
wc -l seasonal/summer.csv -> counts the number of lines in summer.csv
grep 2017-07 seasonal/winter.csv | wc -l -> counts the number of lines containing 2017-07 in winter.csv

wildcards 
* -> match zero or more characters
? -> match a single character
[...] -> match any one of the characters inside the square brackets
[!...] -> match any character not inside the square brackets
{...} -> match any of the comma-separated patterns inside the curly brackets

head -n3 seasonal/s*.csv -> shows the first 3 lines of all files in seasonal starting with s and ending with .csv
[singh, johel]{*.csv, *.txt} -> shows all files starting with singh or johel and ending with .csv or .txt

sort 
    -n -> sort numerically
    -r -> reverse order
    -b -> ignore leading blanks
    -f -> fold case
    -o -> output to file

cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | sort -r -> extracts the second column from winter.csv, shows lines not containing Tooth, and sorts in reverse order

uniq 
    -c -> count occurrences
    -i -> ignore case
    -d -> only show duplicate lines
    -u -> only show unique lines

cut -f 2 -d , seasonal/winter.csv | grep -v Tooth | sort | uniq -c -> extracts the second column from winter.csv, shows lines not containing Tooth, sorts, and counts unique lines

cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt -> extracts the second column from all csv files in seasonal, shows lines not containing Tooth, and writes to teeth-only.txt

wc -l seasonal/*.csv | grep -v total | sort -n | head -n 1
- counts the number of lines in all csv files in seasonal 
- shows lines not containing total
- sorts numerically
- shows the first line

--------------------------------------------------------------------
Batch processing

echo $USER -> shows the value of the USER environment variable
-> repl

testing=seasonal/winter.csv
head -n 1 $testing -> shows the first line of winter.csv

for filetype in gif jpg png; do echo $filetype; done
- prints gif, jpg, png

for filetype in docx odt pdf; do echo $filetype; done
- prints docx, odt, pdf

for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done

for f in seasonal/*.csv; do echo $f; head -n 2 $f | tail -n 1; done
seasonal/autumn.csv
2017-01-05,canine
seasonal/spring.csv
2017-01-25,wisdom
seasonal/summer.csv
2017-01-11,canine
seasonal/winter.csv
2017-01-03,bicuspid

--------------------------------------------------------------------
Creating new tools

ctrl+k -> delete a line
ctrl+u -> un-delete a line
ctrl+o -> save the file
ctrl+x -> exit the editor


cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq (all-dates.sh)
bash all-dates.sh > dates.out -> writes the output of all-dates.sh to dates.out

tail -q -n +2 $@ | wc -l -> counts the number of lines in all files except the first line
bash count-records.sh seasonal/*.csv > num-records.out -> counts the number of lines in all csv files in seasonal except the first line and writes to num-records.out

cut -d , -f $2 $1 -> extracts the column specified by the second argument from the file specified by the first argument
$2 -> file
$1 -> column

bash get-field.sh seasonal/summer.csv 4 2
- filename 1
- number of row to extract 2
- number of column to extract 3

wc -l $@ | grep -v total | sort -n | head -n 1 -> shortest
wc -l $@ | grep -v total | sort -n -r | head -n 1 > longest