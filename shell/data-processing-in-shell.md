## [Data Processing in Shell](https://app.datacamp.com/learn/courses/data-processing-in-shell)

### Downloading data using curl
```
- curl                      -> client for URL
- man curl                  -> manual for curl
```

```bash
curl [option flags] [URL] 
    -o # output to file with specified name
    -O # output to file with the same name as the URL
    -L # follow redirects
    -C # resume download
    -s # silent mode (no progress bar)
    -I # show headers only
    -H # add headers
    -d # data to send in POST request
```

```bash
curl -o <file_name> <URL> # download file from URL with the specified file name
curl -O https://websitename.com/datafilename*.txt
curl -O https://websitename.com/datafilename[1-5].txt

# Download and rename the file in the same step
curl -o Spotify201812.zip -L https://assets.datacamp.com/production/repositories/4180/datasets/eb1d6a36fa3039e4e00064797e1a1600d267b135/201812SpotifyData.zip

# Download all 100 data files
curl -O https://s3.amazonaws.com/assets.datacamp.com/production/repositories/4180/datasets/files/datafile[001-100].txt

# Print all downloaded files to directory
ls datafile*.txt
```

### Downloading data using wget
```
- wget - non-interactive network downloader
- World Wide Web, get
- which wget -> check wget installation
- better than curl for downloading files in bulk
- sudo apt-get install wget
```

```bash
wget [option flags] [URL]
    -o # output to file with specified name
    -O # output to file with the same name as the URL
    -q # quiet mode
    -c # continue download
    -b # background download
    -r # recursive download
    -np # no parent directories
    -nd # no directories
    -nc # no clobber (don't overwrite)
    -q # quiet mode
    -P # directory prefix
    -A # accept list
    -R # reject list
```

```bash
wget -bqc URL # download file from URL in background and continue download

# fill in the two option flags 
wget -c -b https://assets.datacamp.com/production/repositories/4180/datasets/eb1d6a36fa3039e4e00064797e1a1600d267b135/201812SpotifyData.zip

# Verify that the Spotify file has been downloaded
ls 

# Preview the log file 
cat wget-log
```

### Advanced downloading using wget
```bash
# -i -> input file
# download files listed in file.txt
wget -i file.txt                            

# download file with specified rate
wget --limit-rate={rate}k {file_location}   
wget --limit-rate=200k -i file.txt

# wait for specified seconds before download
wget --wait={seconds} {file_location}       
wget --wait=5 -i file.txt

# view url_list.txt to verify content
cat url_list.txt

# create a mandatory 1 second pause between downloading all files in url_list.txt
wget --wait=1 -i url_list.txt

# Take a look at all files downloaded
ls
```

```bash
# use curl, download and rename a single file from URL
curl -o Spotify201812.zip -L https://assets.datacamp.com/production/repositories/4180/datasets/eb1d6a36fa3039e4e00064797e1a1600d267b135/201812SpotifyData.zip

# unzip, delete, then re-name to Spotify201812.csv
unzip Spotify201812.zip && rm Spotify201812.zip
mv 201812SpotifyData.csv Spotify201812.csv

# view url_list.txt to verify content
cat url_list.txt

# use Wget, limit the download rate to 2500 KB/s, download all files in url_list.txt
wget --limit-rate=2500k -i url_list.txt

# take a look at all files downloaded
ls
```

### Data Cleaning and Munging on the Command Line
```
Getting started with csvkit:
- offers data processing and cleaning capabilities on CSV files 
- pip install csvkit
- in2csv -> convert file to CSV
- csvlook -> view CSV file
```

```bash
in2csv SpotifyData.xlxs > SpotifyData.csv                       # convert first sheet in the file
in2csv -n SpotifyData.xlxs > SpotifyData.csv                    # print all sheets in the file
in2csv SpotifyData.xlxs --sheet "sheet_name" > SpotifyData.csv  # convert specific sheet in the file

csvlook SpotifyData.csv # view the CSV file
csvstat SpotifyData.csv # view the statistics of the CSV file
```

### Filtering data using csvkit
```
- csvcut -> select columns
- csvgrep -> select rows through pattern matching
```

```bash
csvcut -n SpotifyData.csv                           # print all columns in the file
csvcut -c 1 SpotifyData.csv                         # select the first column in the file
csvcut -c "track_name" SpotifyData.csv              # select the column with the name track_name in the file
csvcut -c "track_name,track_id" SpotifyData.csv     # select the columns with the names track_name and track_id in the file
```

```bash
csvgrep 
    -m # match
    -i # case-insensitive
    -r # regular expression
    -f # file containing patterns

csvgrep -c "track_id" -m "TRAAABD128F429CF47" SpotifyData.csv # select rows with track_id TRAAABD128F429CF47

# print a list of column headers in the data 
csvcut -n Spotify_MusicAttributes.csv

# print the track id, song duration, and loudness, by name 
csvcut -c "track_id","duration_ms","loudness" Spotify_MusicAttributes.csv
```

### Stacking data and chaining commands with csvkit 
```
csvstack   -> stack multiple CSV files
```

```bash
csvstack file1.csv file2.csv > file3.csv # stack file1.csv and file2.csv to file3.csv

csvstck -g "Rank6","Rank7" file1.csv file2.csv > file3.csv      # stack file1.csv and file2.csv to file3.csv with columns Rank6 and Rank7

csvstack -g "Rank6","Rank7" -n "source file1.csv file2.csv" > file3.csv # stack file1.csv and file2.csv to file3.csv with columns Rank6 and Rank7 and source
```

```
;       -> chain commands
&& -    -> chain commands and run the next command only if the previous command was successful
|       -> pipe commands
```

```bash
# view the CSV file and view the statistics of the CSV file
csvlook SpotifyData.csv; csvstat SpotifyData.csv  

# select columns and view the CSV file
csvcut -c "track_id","track_name" SpotifyData.csv | csvlook 

# sort the CSV file by track_id, select columns, and view the CSV file
csvsort -c "track_id" SpotifyData.csv | csvcut -c "track_id","track_name" | csvlook 

# take top 15 rows from sorted output and save to new file
csvsort -c 2 Spotify_Popularity.csv | head -n 15 > Spotify_Popularity_Top15.csv

# preview the new file 
csvlook Spotify_Popularity_Top15.csv
```

```bash
# convert the Spotify201809 tab into its own csv file 
in2csv Spotify_201809_201810.xlsx --sheet "Spotify201809" > Spotify201809.csv

# check to confirm name and location of data file
ls

# preview file preview using a csvkit function
csvlook Spotify201809.csv

# create a new csv with 2 columns: track_id and popularity
csvcut -c "track_id","popularity" Spotify201809.csv > Spotify201809_subset.csv

# while stacking the 2 files, create a data source column
csvstack -g "Sep2018","Oct2018" Spotify201809_subset.csv Spotify201810_subset.csv > Spotify_all_rankings.csv
```

### Database operations on the command line
```
sql2csv -> execute SQL queries on a database and output the results as CSV
```

```bash
sql2csv --db "sqlite:///SpotifyDatabase.db" \
        --query "SELECT * FROM Spotif_popularity" \
        > Spotify_popularity.csv

# postgres://
# mysql://

# verify database name 
ls

# query first 5 rows of Spotify_Popularity and print in log
sql2csv --db "sqlite:///SpotifyDatabase.db" \
        --query "SELECT * FROM Spotify_popularity LIMIT 5" \
        | csvlook         
```

```bash
csvsql # execute SQL queries on a CSV file and output the results as CSV

csvsql --query "SELECT * FROM Spotify_popularity" Spotify_popularity.csv > Spotify_popularity.csv
csvsql --query "SELECT * FROM file_a JOIN file_b ON file_a.id = file_b.id" file_a.csv file_b.csv > file_c.csv

# preview CSV file
ls

# store SQL query as shell variable
sqlquery="SELECT * FROM Spotify_MusicAttributes ORDER BY duration_ms LIMIT 1"

# apply SQL query to Spotify_MusicAttributes.csv
csvsql --query "$sqlquery" Spotify_MusicAttributes.csv

# store SQL query as shell variable
sql_query="SELECT ma.*, p.popularity FROM Spotify_MusicAttributes ma INNER JOIN Spotify_Popularity p ON ma.track_id = p.track_id"

# join 2 local csvs into a new csv using the saved SQL
csvsql --query "$sql_query" Spotify_MusicAttributes.csv Spotify_Popularity.csv > Spotify_FullData.csv

# Preview newly created file
csvstat Spotify_FullData.csv
```

### Pushing data back to database
```bash
cvsql 
    --insert 
    --db "sqlite:///SpotifyDatabase.db"
    --no-inference
    --no-constraints
```

```bash
csvsql --db "sqlite:///SpotifyDatabase.db" \
        --insert Spotify_MusicAttributes.csv
        --no-inference --no-constraints

# preview file
ls
```

```bash
# upload Spotify_MusicAttributes.csv to database
csvsql --db "sqlite:///SpotifyDatabase.db" --insert Spotify_MusicAttributes.csv

# store SQL query as shell variable
sqlquery="SELECT * FROM Spotify_MusicAttributes"
```

```bash
# apply SQL query to re-pull new table in database
sql2csv --db "sqlite:///SpotifyDatabase.db" --query "$sqlquery" 

# Store SQL for querying from SQLite database 
sqlquery_pull="SELECT * FROM SpotifyMostRecentData"

# Apply SQL to save table as local file 
sql2csv --db "sqlite:///SpotifyDatabase.db" --query "$sqlquery_pull" > SpotifyMostRecentData.csv

# Store SQL for UNION of the two local CSV files
sqlquery_union="SELECT * FROM SpotifyMostRecentData UNION ALL SELECT * FROM Spotify201812"

# Apply SQL to union the two local CSV files and save as local file
csvsql 	--query "$sqlquery_union" SpotifyMostRecentData.csv Spotify201812.csv > UnionedSpotifyData.csv

# Push UnionedSpotifyData.csv to database as a new table
csvsql --db "sqlite:///SpotifyDatabase.db" --insert UnionedSpotifyData.csv
```

### Data pipeline in Bash
```
pip install -r requirements.txt -> install all packages in requirements.txt

crontab -l -> list cron jobs
crontab -e -> edit cron jobs
```

```bash
# Preview both Python script and requirements text file
cat create_model.py
cat requirements.txt

# Pip install Python dependencies in requirements file
pip install -r requirements.txt

# Run Python script on command line
python create_model.py

# Add CRON job that runs create_model.py every minute
echo "* * * * * python create_model.py" | crontab

# Verify that the CRON job has been scheduled via CRONTAB
crontab -l

chmod +x run_etl.sh # make the script executable
```
