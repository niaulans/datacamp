## [Understanding Data Engineering](https://app.datacamp.com/learn/courses/understanding-data-engineering)

### Data workflow
```
1. Data collection and storage (from web traffic, surveys, media consumption)
2. Data preparation (cleaning data, converting data into a more organized format)
3. Exploration and visualization
4. Experimentation and prediction (prediksi harga saham)

Data engineer responsible for the first two steps
```

### Data engineer deliver
```
- the correct data
- in the right format
- to the right people
```

### Data engineer's responsibilities
```
- ingest data from different sources
- optimize databases for analysis
- remove corrupted data
- develop, construct, test, and maintain architectures
```

### The five Vs
```                    
- Volume (how much?)              
- Variety (what kind?)            
- Velocity (how frequent?)        
- Veracity (how accurate?)        
- Value (how useful?)             
```

### Data Engineer VS Data Scientist
| Data Engineer | Data Scientist  |
|---------------|-----------------|
ingest and store data | exploit data
setup databases | access databases
build data pipelines | use pipeline outputs
strong software skills | strong analytical skills


### Data engineering
```
- ingest
- process
- store
- need pipelines
- automate flow from one station to the next
- provide up-to-date, accurate, relevant data
```

### Data pipeline
```
Data pipeline ensure an efficient flow of the data

- Move data from one system to another
- May follow ETL
- Data may not be transformed
- Data may be directly loaded in applications

automate:
- extracting
- transforming
- combining
- validating
- loading

reduce:
- human intervention
- errors
- time it takes data to flow
```

### Data pipeline example
```
Example data pipeline generating a weekly playlist for each user based on their taste:

1. Extract the songs Julian listened to the most over the past month
2. Find other user who listened to these same songs a lot as well
3. Load only the 10 top songs these users listened to the most over the past week into a table called "Similar profiles"
4. Extract only songs these other users listen to that are of the same genre as the ones in Julian's listening sessions. These are our recommendations
5. Load the recommendations song into a new table. That's Julian's weekly playlist.
```

### Data pipeline frameworks
```
ETL 
- Popular frameworks for designing data pipelines 
- Extract data, Transform extracted data, Load transformed data to another databases
```

### Data structure
```
- Structured data 
   - easy to search and organized
   - consisten model, rows, and columns
   - define types
   - can be grouped to form relations
   - stored in relational databases
   - about 20% of the data is structured
   - created and queried using SQL

- Semi-structured data
    - relatively easy to search and organize
    - consistent model, less-rigid implementation: different observations have different sizes
    - different types
    - can be grouped, but needs more work
    - NoSQL databases: JSON, XML, YAML

- Unstructured data
    - doesn't follow a model, can't be contained in rows and columns
    - difficult to search and organize
    - usually text, sound, pictures or video
    - usually stored in data lakes, can appear in data warehouses  or databases
    - most of the data is unstructured
    - can be extremely valuable
```

### SQL databases
```
- Structured Query Language
- Industry standard for Relational Database Management Systems (RDBMS)
- Allows you to access many records at once, and group, filter, or aggregate them
- Close to written English, easy to write and understand
- DE use SQL to create and maintain databases
```

### Create a table in SQL
```sql
    CREATE TABLE employees(    
    employee_id INT,           
    first_name VARCHAR(255),   
    last_name VARCHAR(255),    
    role VARCHAR(255),         
    team VARCHAR(255),         
    full_time BOOLEAN,         
    office VARCHAR(255)        
);                             
```

### Database schema
```
- Databases are made of tables
- The database schema governs how tables are related

Several implementations of SQL:
- SQLite
- MySQL
- PostgreSQL
- Oracle SQL
- SQL Server
```

### Data warehouses and data lakes
```
Data lakes
- Stores all the raw data (unprocessed and messy)
- Can be petabytes in size (1 million GBs)
- Stores all data structures
- Cost-effective
- Difficult to analyze
- Requires an up-to-date data catalog
- Used by data scientists
- Big data, real-time analytics

Data warehouses
- Specific data for specific use
- Relatively small in size
- Stores mainly structured data
- More costly to update
- Optimized for data analysis
- Ad-hoc, read-only queries

Data catalog for data lakes:
- What is the source of this data?
- Where is this data used?
- Who is the owner of the data?
- How often is this data updated?
- Good practice in terms of data governance
- Ensures reproducibility
- No catalog --> data swamp

Good practice for any data storage:
- Reliability
- Autonomy
- Scalability
- Speed
```

### Database vs. data warehouse
```
Database:
- General term
- Loosely defined as organized data stored and accessed on a computer

Data warehouse:
- type of database
```


### Processing data
```
Converting raw data into meaningful information.

Data processing value:
- Remove unwanted data
- Optimize memory, process, and network costs
- Convert data from one type to another
- Organized data
- To fit into a schema/structure
- Increase productivity

Example at Spotflix:
- No long term need for testing feature data
- Can't afford to store and stream files this big
- Convert songs from .flac to .ogg
- Reorganized data from the data lake to data warehouses
- Employee table example
- Enable data scientists
```

### How data engineers process data
```
- Data manipulation, cleaning, and tidying tasks
  - that can be automated
  - that will always need to be done
- Store data in a sanely structured database
- Create views on top of the database tables
- Optimizing the performance of the database

Example:
- Rejecting corrupt song files
- Deciding what happens with missing metadata
- Separate artists and albums tables
- ..but provide view combining them
- indexing
```

### Scheduling data (Apache Airflow)
```
- Can apply to any task listed in data processing
- Scheduling is the glue of your system
- Holds each piece and organize how they work together
- Run tasks in a specific order and resolves all dependencies
```

### Manual, time, and sensor scheduling
```
- Manually (manually update the employee tables)
- Automatically run at a specific time (update the employee table at 6 AM)
- Automatically run if a specific condition is met 
    - sensor scheduling (update the departement tables if a new employee was added)
```

### Batch and streams
```
Batch 
- Group records at intervals (song uploaded by artists every 24 hours, employee table update, revenue table)
- Often cheaper

Streams
- Send individual records right away (new users signing in, online vs. offline)
```

### Parallel computing
```
- Basis of modern data processing tools
- Necessary :
    - mainly because of memory
    - also for processing power
- How it works:
    - Breaks down tasks into smaller tasks
    - Distributes these tasks to multiple computers
    - Combines the results

Benefit and risks of parallel computing:
- Employees = processing unit
- Adv: 
    - Extra processing power
    - Reduced memory footprint
- Disadv:
    - Moving data incurs a cost
    - Communication time
```

### Cloud Computing
```
Servers on premises:
- Bought
- Need space
- Electrical and maintenance costs
- Enough power for peak moments
- Processing power unused at quieter times

Servers on the cloud:
- Rented
- Don't need space
- Use just the resources when we need them
- The closer to the user the better

Cloud computing for data storage:
- Database reliability : data replication
- Risk with sensitive data
```

```
AWS             -> AWS S3 -> AWS EC2 -> AWS RDS
Microsoft Azure -> Azure Blob Storage -> Azure Virtual Machines -> Azure SQL Database
Google Cloud    -> Google Cloud Storage -> Google Compute Engine -> Google Cloud SQL
```

```
Spotflix use AWS:
- S3 to store cover albums
- EC2 to process songs
- RDS to store employee data
```

### Multicloud
```
Pros:
- Reducing reliance on a single vendor
- Cost-efficiencies
- Local laws requiring certain data to be physically present within the country
- Mitigating againts disasters

Cons:
- Cloud providers try to lock in consumers
- Incompatibility
- Security and governance
```