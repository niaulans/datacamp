## [Data Warehousing Concepts](https://app.datacamp.com/learn/courses/data-warehousing-concepts)

### Data warehouse
```
A computer system designed to store and analyze large amounts of data for an organization
```

### What does a data warehouse do?
```
- Gathers data from different areas of an organization
- Integrates and stores the data
- Make it available for analysis

Why is a data warehouse valuable?
Organizations implement data warehouse in order to:*
- Support business intelligence activity
- Enable effective organizational analysis and decision-making
- Find ways to innovate based on insights from their data

Common scenarios:
- Product sales forecasting
- Governance and regulation adherence
- Insight and growth
```

### Difference between data warehouses and data lakes

#### Database
```
- Structures data in rows and columns
- Transactional databases store transactions
```

#### Data warehouse
```
- Gather data, integrate, and make available for analysis
- Many input data sources
- Store structured data
- Complex to change
    - Upstream and downstream effects must be considered
- Typically > 10 gigabytes

Why the data warehouse

- How quickly query will run on a large amount of data
- Avoid slowing down transactional database
```

#### Data marts
```
- a relational database for analysis
- Data is focused on one subject area 
- Few input data sources
- Typically < 100 GBs
```

#### Data lakes
```
- Entire organization store of data
    - Contains data from many departments
    - Many data input sources
    - Typically > 100 gb in size
- Stores structured and unstructured data
Example: video, audio, and documents
- Less complex to make changes
    - Fewer upstream and downstream effects to consider
- Purpose to store data may not be known
    - Less organized
```

### Data warehouse support organizational analysis
```
High-level llife cycle data warehouse project
Planning                -> Business requirements (Analyst, DS), data modelling ( analysts, DS, DE, DB admin)
Implementation          -> ETL design & development (DE, DB admin), BI application development (Analyst, DS)
Support/ maintenance    -> Maintenance (DE), test & deploy (Analyst, DS, DE)
```

### Warehouse Architectures and Properties
```
Layer overview - data staging

Data source -> Data source 1 & 2 -> Data staging (ETL) -> Data storage (data warehouse & data mart) -> Data presentation (data analytics, reporting tools, analysis tools)
```

#### Data source layer
```
- All data sources for data warehouse
- Example:
    - Transactional database
    - Log files
    - Spreadsheet
```

#### Data staging (ETL)
```
- Layer extracts, transform, and clean data through ETL process
- Contains ETL process and storage tables

 ETL process within data staging layer:
- extracted
- business rules applied and cleaned
- staging database often used
- must be able to extract valid data
- batch / full loading
```

#### Data storage layer
```
Data is stored in warehouse and data marts
```

#### Data presentation layer
```
- Users interact with stored data
- Users:
    - Use BI (Business Intelligence) tools
    - Use data mining tools
    - Create direct queries

The presentation layer:
- Users interact with the presentation layer
  - Area of constant development

Presentation layer groups:
- Automated reporting/dashboarding tools
    - Goals: 
        - Create reports needed for decision-making
        - Create dashboards using historical data
        - Ex: tableau, looker, SAP

    - Users:
        - analysts
        - Citizen data scientists

- BI/data analytics
    - Goals:
        - Tools for Exploration
        - Looking for patterns
        - Ex: tableau, rapidminer, alteryx, looker, oracle, knime

    - Users:
        - analysts
        - Data scientist

- direct queries
    - Goals:
        - Sophisticated tools for Exploration
        - Ex: R, Azure, Python

    - Users:
        - Data scientists
        - Analysts
        - Data engineers
```

### Data warehouse architecture

#### Bill Inmon - top-down approach
```     
Must decide:
- On all data definitions, cleaning, and business rules
- Before any data enters warehouse
Data source -> ETL -> Data warehouse -> Data mart -> BI tools

Pros and cons:
- Pros:
    - Single source of truth for organization
    - Normalization = less storage
    - Easy to change data marts to support reporting changes

- Cons:
    - More joins = slower response time
    - Lengthy upfront work
```

#### Kimball - bottom-up approach
```
Data source -> ETL -> Data mart -> Data warehouse -> BI tools

- Denormalize data 
- Focus on departmental data mart
- Data moves directly from ETL to data marts

Pros and cons:
- Pros:
    - Upfront development speed 
    - Lower startup cost
    - Denormalized = user friendly

- Cons:
    - Increase ETL processing time
    - Greater possibility of duplicate data
    - Ongoing development needed
```

### OLAP and OLTP Systems

#### OLAP (Online analytical processing)
```
- Designed to support analysis of large amounts of data
- Example dimensional organization:
    - country, state, city
    - years, months, days
- OLAP reorganizes data into multidimensional format

OLAP cube:
- OLAP cube key to OLAP system
- Faster processing vs. traditional relational databases
- Hypercubes have more than three dimensions

OLAP:
- Designed for analyzing purchase data 
- Data organized by multiple dimensions
```

#### OLTP (Online transaction processing)
```
- Designed for processing simple database queries
- Used in source systems to data warehouse

Example for a credit card company:
OLTP: 
- System tracks customer's purchase
- Processes large amounts of simple database updates to account balances
- Rows and columns
```

### Data warehouse data modeling

#### Data models
```
- Bottom-up, kimbal model = star & snowflake schemas
- Denormalized data models

Example:
- Hypothetical publicly traded company
    - Sells home office furniture
    - Fact table =  sales_order_fact -> custid, dateid, productid, unitsold, salesamount, tax
        Fact table (measurements, metrics, or fact about an organization)
        - Links to dimension tables for more detail
    - Dimension table = cust_dim -> custid, custname, custaddress, custcity, custstate, custzip
        - Dimensionns/attribute about a process
        - Holds reference data
        - Dimension tables add more detail to fact table
```

```
Star schema:
- A central fact table, with one or more dimensional tables
- Easy for business users

Snowflake schema:
- Dimensional table connected through another dimensional table
```

#### Kimball's four step process
```
1. Step 1- Select the organizational process
   - Ask questions about a process
   - Kimball bottom-ip approach starts with a business process
   - Example of organizational process:
     - Invoice and billing
     - Product quality monitoring
     - Marketing

2. Step 2 - Declare the grain
    - Grain = level to store fact table
    - A level of data cannot be split further
    - Example: 
      - Music service -> song grain
      - Shipping service -> Line item grain

3. Step 3- Identify the dimensions
    - Choose dimensions that apply to each row
    - Example:
      - Time: year, quarter, and month
      - Location: address, state, and country
      - Users: names and email address
    - How to describe the data?
    - Business users and analysts = valuable feedback

4. Identify the fact
    - Numerical facts for each fact table row
    - What are we answering?
    - Metrics should be true at selected grain
    - Example:
      - Music service: total number of plays, sales revenue a song
      - Ride-sharing: travel distance, time needed

Slowly changing dimensions:

The challenge
Original data:
productid = 12345
description = Tesla-ModelY
category = electric-veh

update category:
currect: electric-veh
new: electric-crossover

Type I: 
- Update value in table 
Category = electric-crossover
- Will lose historical data

Type II:
- Add a row with the updated value
New:
productid = 12345
description = Tesla-ModelY
category = electric-crossover
startdate = 2020-01-01
enddate = 9999-12-31
- The history is retained

Type III:
- Add column to dimension table to track changes
New: 
productid = 12345
description = Tesla-ModelY
category = electric-crossover
pastcategory = electric-veh
- Can view past and current data together
- Can require reporting changes and limited tracking
```

### Row vs. column store data store
```
Why is it important?
- Optimizing queries for speed
- Column store format for data warehouse tables is best for analytic workloads

Basics of computer storage:
- Computer store data in blocks
- Reads the required blosks when retrieving data
- Reading fewer blocks increases the overall speed of the process

Row store:
- Stores data in rows
- Reads all columns for a row
- Good for OLTP systems

Column store:
- Stores data in columns
- Reads only the columns needed for a query
- Good for OLAP systems
- Better data compression
```

### Implementation and Data Prep

ETL                 |       ELT
|-------------------|-------------------
Extract         |       Extract
Transform       |       Load
Load            |       Transform

#### ETL
```
- Data transformed during the moves
- Uses separate system to process data

Pros:
- Lower data storage costs
- Personally identifiable information (PII) security compliance

Cons:
- Transformation errors/changes require new data pulls
- Costs of separate system for process data
```

#### ELT
```
- Data is loaded, then transformed
- Uses the warehouse to transform the data

Pros:
- No need system to process data
- Transformations can be rerun without impacting source systems
- Works well for near real-time requirements

Cons:
- Increases storage needs from raw data
- Compliance with PII secutiry standards
```

### Data cleaning
```
Data format cleaning:
- Update values to an expected format
    - date
    - names
    - capitalization
- Ensures output is in a consistent format

Address parsing:
- dividing a street address into its components
- can use tools to validate addresses
```

#### Data validation
```
- Range check 
    - Is the value within expected range? - a person age
- Type check 
    - Is the value the proper data type? Storing age vs number

Duplicate row elimination
- This process gets rid of duplicate entries
```

### On premise and cloud data warehouse

#### On premise
```
- purchase and install software and hardware
- on the grounds of the organization

pros:
- complete control
- implement custom data governance
- local network speed
- can optimize for workloads

cons:
- upfront hardware and software costs
- Personnel/staff must maintain system
- Must keep up with patches and security
```

#### In the cloud
```
- Rapid growth
- Forecasted continued growth

Pros:
- No maintaining equipment and infrastructure
- Frees up IT staff
- Can scale storage and computer resources
- No upfront costs

Cons:
- less control
- cannot optimize warehouse workloads
- possible unanticipated costs
```

### Data warehouse design example
```
- New startup company
- Photo sharing app

Top-down, or bottom-up approach?
- Consideration:
    - Vital to show business impact quickly
    - Top-down approach has a longer startup process
- Decision:
    - Bottom-up approach
    - Sales data mart must be the priority
```

#### Kimball
``` 
Step 1 - Select the organizational process 
Considerations:
- What type of customers purchase large volume of photos?
Decision:
- Develop customer purchase

Step 2 - Declare the grain
Considerations:
- Data should be flexible to answer many questions
- Selecting the lowest grain possible
Decision:
- Tracking customer/photo purchase

Step 3 - Identify the dimensions
Considerations:
- How do users describe the data that results from the business process?
- Customer prioritization
Decision:
- Customer location (country, state)
- Date customer joined
- Default payment method

Step 4 - Identify the fact
Considerations:
- What are answering?
Decision:
- Time spent viewing photo before purchase
- Photo cost and tax
- date of purchase

fact_table:
custid, dateid, productid, unitsold, salesamount, tax

dimension_table:
cust_dim -> custid, custname, custaddress, custcity, custstate, custzip
photo_dim -> photoid, photodesc, phototype, photoprice
date_dim -> dateid, day, month, year
```

### On premise vs. cloud data warehouse
```
Considerations:
- we dont want upfront costs
- small team
decision:
- cloud data warehouse
```

### ETL vs. ELT
```
Considerations:
- Keep all data
- Cloud implementation allows us to scale compute as needed

Decision:
- ELT
```