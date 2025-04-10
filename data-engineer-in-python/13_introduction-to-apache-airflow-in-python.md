## [Introduction to Apache Airflow in Python](https://app.datacamp.com/learn/courses/introduction-to-apache-airflow-in-python)


### Data engineering
```
Taking any action invloving data and turning it into a reliable, repeatable, and maintainable process
```

### Workflow
```
- A set of steps to accomplish a given data engineering task
  - Such as : downloading files, copying data, filtering information, writing to a database, exact
- Of varying level of complexity
- A term with various meaning depending on context

Workflow example:
- Download sales data
- Clean sales data
- Run ML pipeline
- Upload results to webserver
- Generate report and email to CEO
```

### Airflow
```
- A platform to program workflows, including:
  - Creation
  - Scheduling 
  - Monitoring
- Can implement programs from any language, but workflow are written in python 
- Implements workflows as DAGs: Directed Acylic Graphs
- Accessed via code, command-line, or via web interface/REST API

Other workflow tools:
- Luigi
- SSIS
- Bash scripting
```

### DAGs
```
Directed Acylic Graph
- Directed, there is an inherent flow representing dependencies between components
- Acylic, does not loop/cycle/repeat
- Graph, the actual set of components

- In Airflow, this represents the set of tasks that make up your workflow
- Consists of the tasks and the dependencies between tasks
- Created with various details about the DAG, including the name, start date, owner, etc.
- Seen in Airflow, Apache Spark, dbt
```

### Simple DAG
```python
etl_dag = DAG(
  dag_id='etl_pipeline',
  default_args={"start_date":"2024-01-08"}
)
```

### Running a workflow in Airflow
```bash
airflow tasks test <dag_id> <task_id> [execution_date]
airflow tasks test example_etl download-file 2024-01-10

airflow -h # to see all the commands
```

### DAG in Airflow
```
- Are written in Python (but can use components written in other languages)
- Are made up of components (typically tasks) to be exceuted, suah as operators, sensors, etc.
- Contain dependencies defines explicitly or implicitly
  - Copy the file to the server before trying to import it to the database service.
```

### DAGs example in Airflow (1)
```python
from airflow import DAG
from datetime import datetime

default_arguments = {
  'owner': 'jdoe',
  'email': 'jdoe@datacamp.com',
  'start_date': datetime(2020, 1, 20)
}

with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
```

```bash
airflow -h          # for descriptions
airflow dags list   # to show all recognized DAGs.
```

```
Command line to:
- Start airflow processes
- Manually run DAGs/Tasks
- Get logging information from Airflow

Python to:
- Create a DAG
```

### DAGs example in Airflow (2)
```python
# Import the DAG object
from airflow import DAG

# Define the default_args dictionary
default_args = {
  'owner': 'dsmith',
  'start_date': datetime(2023, 1, 14),
  'retries': 2
}

# Instantiate the DAG object
with DAG('example_etl', default_args=default_args) as etl_dag:
  pass
```

```bash
airflow <subcommand> -h    # to get help on a specific subcommand
airflow webserver -p 9090  # to start the webserver on port 9090
```

### Implementing Airflow DAGs
```
Airflow operators:
- Represents a single task in a workflow
- Run independently (usually)
- generally do no tshane information 
- Various operators to perform diffenrent tasks

EmptyOperator(task_id='example') -> for troubleshooting
```

### BashOperator
```
- Exceute a given Bash command or script.
- Runs the command in a temporary directory.
- Can specify environment variables for the command.

BashOperator(
  task_id='bash_example',
  bash_command='echo "Example!"',
  dag=dag # old version
)
```

### BashOperator example
```python
from airflow.operators.bash import BashOperator

example_task = BashOperator(task_id='bash_ex', bash_command='echo 1')
bash_task = BashOperator(
  task_id='clean_addresses',
  bash_command='cat addresses.txt | awk "NF==10" > cleaned.txt',
)
```

```
Operator gotchas
- Not guaranteed to run in the same location/environment.
- May require extansive use of Environment variables.
- Can be difficult to run tasks with elevated privileges.
```

### More than one BashOperator example
```python
# Import the BashOperator
from airflow.operators.bash import BashOperator

with DAG(dag_id="test_dag", default_args={"start_date": "2024-01-01"}) as analytics_dag:
  # Define the BashOperator 
  cleanup = BashOperator(
      task_id='cleanup_task',
      # Define the bash_command
      bash_command="cleanup.sh",
  )

  # Define a second operator to run the `consolidate_data.sh` script
  consolidate = BashOperator(
      task_id='consolidate_task',
      bash_command="consolidate_data.sh"
      )

  # Define a final operator to execute the `push_data.sh` script
  push_data = BashOperator(
      task_id="pushdata_task",
      bash_command="push_data.sh"
      )
```

### Tasks
```
- Instances of operators
- Usually assigned to a variable in Python 
  example_task = BashOperator(task_id='example', bash_command='echo 1')
- Referred to by the task_id within the Airflow tools

Task dependencies:
- Define a given order of task completion 
- Are not required for a given workflow, but usually present in most
- Are referred to as upstream or downstream tasks
- >> upstream operator -> before
- << downstream operator -> after

Simple task dendency
task1 = BashOperator(task_id='first_task,
                      bash_command='echo 1')
task2 = BashOperator(task_id='second_task',
                      bash_command='echo 2')

# Set first_task to run before second_task
task1 >> task2 or task2 << task1

# Mixed dependencies
task1 >> task2 << task3
```

### Task dependencies example
```python
# Define a new pull_sales task
pull_sales = BashOperator(
    task_id='pullsales_task',
    bash_command="wget https://salestracking/latestinfo?json"
)

# Set pull_sales to run prior to cleanup
pull_sales >> cleanup

# Configure consolidate to run after cleanup
consolidate << cleanup

# Set push_data to run last
consolidate >> push_data
```

### PythonOperator
```
PythonOperator
- Executes a python function/callable
- Operates similarly to the BashOperator, with more options.
- Can pass in arguments to the python code
```

### PythonOperator example
```python
from airflow.operators.python import PythonOperator
def printme():
  print("This goes in the logs!")
python_task = PythonOperator(
    task_id='simple_print',
    python_callable=printme
)
```

```
Arguments:
- Support arguments to tasks (positional, keyword)
- Use the op_kwargs dictionary.
```

### PythonOperator example with arguments
```python
def sleep(length_of_time):
  time.sleep(length_of_time)

sleep_task = PythonOperator(
  task_id='sleep',
  python_callable=sleep,
  op_kwargs={'length_of_time':5}
)
```

### More than one PythonOperator example
```python
def pull_file(URL, savepath):
    r = requests.get(URL)
    with open(savepath, 'wb') as f:
        f.write(r.content)   
    # Use the print method for logging
    print(f"File pulled from {URL} and saved to {savepath}")

from airflow.operators.python import PythonOperator

# Create the task
pull_file_task = PythonOperator(
    task_id='pull_file',
    # Add the callable
    python_callable=pull_file,
    # Define the arguments
    op_kwargs={'URL':'http://dataserver/sales.json', 'savepath':'latestsales.json'}
)

# Add another Python task
parse_file_task = PythonOperator(
    task_id='parse_file',
    # Set the function to call
    python_callable=parse_file,
    # Add the arguments
    op_kwargs={'inputfile':'latestsales.json', 'outputfile':'parsedfile.json'},
)
```

### EmailOperator
```
- Found in the airflow.operators library
- Sends an email
- Can contain typical components  
  - HTML contents
  - Attachments
- Does require the Airflow system to be configured with email server details
```

### EmailOperator example
```python
email_task = EmailOperator(
  task_id='email_sales_report',
  to='sales_manager@example.com',
  subject='Automated Sales Report',
  html_content='Attached is the latest sales report',
  files='latest_sales.xlsx'
)
```

### EmailOperator example with attachments
```python
# Import the Operator
from airflow.operators.email import EmailOperator

# Define the task
email_manager_task = EmailOperator(
    task_id='email_manager',
    to='manager@datacamp.com',
    subject='Latest sales JSON',
    html_content='Attached is the latest sales JSON file as requested.',
    files='parsedfile.json',
    dag=process_sales_dag
)

# Set the order of tasks
pull_file_task >> parse_file_task >> email_manager_task
```

### Airflow scheduling
```
DAG Runs = spesific instance of a workflow at a point in time
- Can be run manually or via schedule_interval
- Maintain state for each workflow and the tasks within 
  - running
  - failed
  - success

Schedule details:
When scheduling a DAG, there are several attributes of note:
- start_date: The date / time to initially schedule the DAG run
- end_date: Optional attribute for when to stop running new DAG instance
- max_tries: Optional attribute for how many attempts to make
- schedule_interval: How often to run 

schedule_interval: 
- How often to schedule the DAG
- Between the start_date and end_date
- Can be defined via cron style syntax or via built-in presets.
```

### Cron syntax
```
* * * * * 
minute(0-59)
hour(0-23)
day of the month(1-31)
month(1-12)
day of the week(0-6) (Sunday to saturday), 7 is also Sunday on some system
0 Sunday
1 Monday
2 Tuesday
3 Wednesday
4 Thursday
5 Friday
6 Saturday

- is pulled from the unix cron format
- consists of 5 fields separated by a space

0 12 * * *          -> Run daily at noon 
* * 25 2 *          -> Run once per minute on February 25
0,15,30,45 * * * *  -> Run every 15 minutes
``` 

```
Airflow scheduler presets
Preset        Cron equivalent
@hourly       0 * * * *
@daily        0 0 * * * 
@weekly       0 0 * * 0
@monthly
@once
None

Special presets:
- None -> Don't schedule ever, used for manually triggered DAGs
- @once -> Schedule only once

When scheduling a DAG, Airflow will:
- Use the start_date as the earlist possible value
- Schedule the task at start_date + schedule_interval

'start_date': datetime(2020, 2, 5),
'schedule_interval': @daily
```

### Scheduling example
```python
# Update the scheduling arguments as defined
default_args = {
  'owner': 'Engineering',
  'start_date': datetime(2023, 11, 1),
  'email': ['airflowresults@datacamp.com'],
  'email_on_failure': False,
  'email_on_retry': False,
  'retries': 3,
  'retry_delay': timedelta(minutes=20)
}

dag = DAG('update_dataflows', default_args=default_args, schedule_interval='30 12 * * 3')
```

### Maintaining and monitoring Airflow workflows
```
Airflow sensors:
- An operator that waits for a certain condition to be true
  - Creation of a file
  - Upload of a database record
  - Certain response from a web request
- Can define how often to check for the condition to be true
- Are assigned to tasks

Sensor details:
- Derived from airflow.sensors.base_sensor_operator
- Sensor arguments:
  - mode -> how to poke for the condition
    mode='poke' - The default, run repeatedly
    mode='reschedule' - Give up task slot and try again later
  - poke_interval - How often to wait between checks
  - timeout - How long to wait before failing the task
```

### File sensor
```
- Is part of the airflow.sensors library
- Checks for the existence of a file at a certain location
- Can also check if any files exist within a directory
```

### File sensor example
```python
from airflow.sensors.filesystem import FileSensor

file_sensor_task = FileSensor(
  task_id='file_sense',
  filepath='salesdata.csv',
  poke_interval=300,
  dag-sales_report_dag
)

init_sales_cleanup >> file_sensor_task >> generate_report
```

### Other sensors
```
- ExternalTaskSensor      -> wait for a task in another DAG to complete
- HttpSensor              -> Request a web URL and check for content
- SqlSensor               -> Run a SQL wuery to check for content
```

```python
airflow.sensors 
airflow.providers.*.sensors
```

```
Why sensors?
- Uncertain when it will be true
- If failure not immediately desired
```

### 
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from airflow.providers.http.operators.http import SimpleHttpOperator
from airflow.sensors.filesystem import FileSensor

dag = DAG(
   dag_id = 'update_state',
   default_args={"start_date": "2023-10-01"}
)

precheck = FileSensor(
   task_id='check_for_datafile',
   filepath='salesdata_ready.csv',
   dag=dag)

part1 = BashOperator(
   task_id='generate_random_number',
   bash_command='echo $RANDOM',
   dag=dag
)

import sys
def python_version():
   return sys.version

part2 = PythonOperator(
   task_id='get_python_version',
   python_callable=python_version,
   dag=dag)
   
part3 = SimpleHttpOperator(
   task_id='query_server_for_external_ip',
   endpoint='https://api.ipify.org',
   method='GET',
   dag=dag)
   
precheck >> part3 >> part2
---------------------------------------------------------------------------
Airflow executor:
- Executors run tasks
- Different executr=ors handle running the tasks diffferently
- Example:
  - SequentialExecutor
  - LocalExcecutor
  - KubernetesExecutor

SequentialExecutor:
- The default Airflow executor
- Runs one task at a time
- Useful for debugging
- While functional, not really recommended for production 

LocalExecutor:
- Runs on a single system 
- Treats tasks as processes
- Parallelism defined by the user
- Can utilize all resources of a given host system 

KubernetsExecutor:
- Uses a Kubernetes as task manager
- Multiple worker systems can be defined
- Is significantly more difficult to setup & configure
- Extremely powerful method for organization with extensive workflows

Determien your executor:
- Via airlfow.cf file
- Look for the executor= line 
cat airlofw/airflow.cfg | grep "executor = "executor = SequentialExecutor
- airflow info 

---------------------------------------------------------------------------
Debugging and troubleshooting in Airflow

Typical issues:
- DAG won't run on schedule
- DAG won't load
- Syntax errors

DAG won't run on schedule:
- Check if scheduler is running 
- Fix by running airflow scheduler from the command-line 
- At least one schedule_interval hasn't passed.
  - Modify the attributes to meet your requirements
- Not enough tasks free within the executor to run.
  - Change executor type.
  - Add system resources.
  - Add more systems.
  - Change DAG scheduling 

DAG won't load:
- DAG not in web UI
- DAG not in airflow dags list
Possible solutions:
- Verify DAG file is in correct folder.
- Determine the DAGs folder via airflow.cfg
- Note, the folder must be an absolute path 

Syntax errors:
- The most common reason a DAG file won't appear
- Sometimes difficult to find errors in DAG
- Two quick methods:
  - Run airflow dags list-import-errors
  - Run the python interpreter 
    python3 dagfile.py

---------------------------------------------------------------------------
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.sensors.filesystem import FileSensor
from datetime import datetime

report_dag = DAG(
    dag_id = 'execute_report',
    schedule_interval = "0 0 * * *"
)

precheck = FileSensor(
    task_id='check_for_datafile',
    filepath='salesdata_ready.csv',
    start_date=datetime(2023,2,20),
    mode='poke',
    dag=report_dag)

generate_report_task = BashOperator(
    task_id='generate_report',
    bash_command='generate_report.sh',
    start_date=datetime(2023,2,20),
    dag=report_dag
)

precheck >> generate_report_task
---------------------------------------------------------------------------
SLAs and reporting in Airflow

SLAs -> Service Level Agreement
- Within Airflow, the amount of time a task or a DAG should be require to run 
- An SLA Miss is any time the task / DAG does not meet the expected timing 
- If an SLA is missed, an email is sent out and a log is stored

SLA Misses:
- Found under Browse: SLA Misses (UI)

Defining SLAs:
- Using the 'sla' argument on the task 
task1 = BashOperator(
    task_id='sla_task',
    sla=timedelta(seconds=30),
    dag=dag
)

- On the default_args dictionary
default_args={
  'sla'=timedelta(minutes=20),
  'start_date':datetime(2023, 2, 20)
}
dag = DAG('sla_dag', default_args=default_args)

timedelta object 
- In the datetime library
- Accessed via from datetime imprt timedelta
timedelta(seconds=30)
timedelta(weeks=2)
timedelta(days=4, hours=10, minutes=20, seconds=30)

General reporting:
- Options for success/failure/error
- Keys in the default_args dictionary
default_args={
  'email':['airflowalerts@datacamp.com'],
  'email_on_failure':True,
  'email_on_retry:False,
  'email_on_success':True,
  .....
}

---------------------------------------------------------------------------
# Import the timedelta object
from datetime import timedelta

# Create the dictionary entry
default_args = {
  'start_date': datetime(2024, 1, 20),
  'sla': timedelta(minutes=30)
}

# Add to the DAG
test_dag = DAG('test_workflow', default_args=default_args, schedule_interval=None)

---------------------------------------------------------------------------
# Import the timedelta object
from datetime import timedelta

test_dag = DAG('test_workflow', start_date=datetime(2024,1,20), schedule_interval=None)

# Create the task with the SLA
task1 = BashOperator(task_id='first_task',
                     sla=timedelta(hours=3),
                     bash_command='initialize_data.sh',
                     dag=test_dag)

---------------------------------------------------------------------------
# Define the email task
email_report = EmailOperator(
        task_id='email_report',
        to='airflow@datacamp.com',
        subject='Airflow Monthly Report',
        html_content="""Attached is your monthly workflow report - please refer to it for more detail""",
        files=['monthly_report.pdf'],
        dag=report_dag
)

# Set the email task to run after the report is generated
email_report << generate_report
---------------------------------------------------------------------------
'from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.sensors.filesystem import FileSensor
from datetime import datetime

default_args={
    'email': ['airflowalerts@datacamp.com', 'airflowadmin@datacamp.com'],
    'email_on_failure': True,
    'email_on_success': True
}

report_dag = DAG(
    dag_id = 'execute_report',
    schedule_interval = "0 0 * * *",
    default_args=default_args
)

precheck = FileSensor(
    task_id='check_for_datafile',
    filepath='salesdata_ready.csv',
    start_date=datetime(2023,2,20),
    mode='reschedule',
    dag=report_dag)

generate_report_task = BashOperator(
    task_id='generate_report',
    bash_command='generate_report.sh',
    start_date=datetime(2023,2,20),
    dag=report_dag
)

precheck >> generate_report_task
---------------------------------------------------------------------------
Building production pipelines in Airflow

Working with templates:
- Template allow substituting information during a DAG run 
- Provide added flexibility when defining tasks
- Are created using the Jinja templating language

Non-Templated BashOperator example
Create a task to echo a list of files:
t1 = BashOperator(
  task_id='first_task',
  bash_command='echo "Reading file1.txt"',
  dag=dag
)

t2 = BashOperator(
  task_id='first_task',
  bash_command='echo "Reading file2.txt"',
  dag=dag
)

Templated BashOperator example:
template_command="""
  echo "Reading {{params.filename}}
"""

t1 = BashOperator{
  task_id='template_task',
  bash_command=template_command,
  params={'filename':'file1.txt'},
  dag=example_dag
}

---------------------------------------------------------------------------
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

default_args = {
  'start_date': datetime(2023, 4, 15),
}

cleandata_dag = DAG('cleandata',
                    default_args=default_args,
                    schedule_interval='@daily')

# Create a templated command to execute
# 'bash cleandata.sh datestring'
templated_command = """
bash cleandata.sh {{ ds_nodash }}
"""

# Modify clean_task to use the templated command
clean_task = BashOperator(task_id='cleandata_task',
                          bash_command=templated_command,
                          dag=cleandata_dag)
---------------------------------------------------------------------------
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

default_args = {
  'start_date': datetime(2023, 4, 15),
}

cleandata_dag = DAG('cleandata',
                    default_args=default_args,
                    schedule_interval='@daily')

# Modify the templated command to handle a
# second argument called filename.
templated_command = """
  bash cleandata.sh {{ ds_nodash }} {{params.filename}}
"""

# Modify clean_task to pass the new argument
clean_task = BashOperator(task_id='cleandata_task',
                          bash_command=templated_command,
                          params={'filename': 'salesdata.txt'},
                          dag=cleandata_dag)

# Create a new BashOperator clean_task2
clean_task2 = BashOperator(task_id='cleandata_task2',
                           bash_command=templated_command,
                          params={'filename': 'supportdata.txt'},
                          dag=cleandata_dag)
                           
# Set the operator dependencies
clean_task >> clean_task2

---------------------------------------------------------------------------
More templates:
template_command="""
{% for filename in params.filenams %}
  echo "Reading {{filename}}"
{% endfor %}
"""

t1 = BashOperator(
  task_id='template_task',
  bash_command=template_command,
  params={'filenames:['file1.txt','file2.txt']},
  dag=example_dag
)
---------------------------------------------------------------------------
Variables
- Airflow built-in runtime variables
- Provides assorted information about DAG runs, tasks, and even the system configuration

Execution Data: {{ds}} -> 2023-04-15
Execution Data, no dashes: {{ds_nodash  }} -> 20230415
Previous execution date: {{prev_ds}} -> 2023-04-14
Previous execution date, no dashes: {{prev_ds_nodash}} -> 20230414
DAG object: {{dag}}
Airflow config object: {{conf}}
{{macros.datetime}} -> datetime.datetime object 
{{macros.ds.add('2020-04-15, 5)}} -> Modify days from a date, this example returns 2020-04-20

---------------------------------------------------------------------------
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

filelist = [f'file{x}.txt' for x in range(30)]

default_args = {
  'start_date': datetime(2020, 4, 15),
}

cleandata_dag = DAG('cleandata',
                    default_args=default_args,
                    schedule_interval='@daily')

# Modify the template to handle multiple files in a 
# single run.
templated_command = """
  <% for filename in params.filenames %>
  bash cleandata.sh {{ ds_nodash }} {{ filename }};
  <% endfor %>
"""

# Modify clean_task to use the templated command
clean_task = BashOperator(task_id='cleandata_task',
                          bash_command=templated_command,
                          params={'filenames': filelist},
                          dag=cleandata_dag)
---------------------------------------------------------------------------
from airflow import DAG
from airflow.operators.email import EmailOperator
from datetime import datetime

# Create the string representing the html email content
html_email_str = """
Date: {{ds}}
Username: {{params.username}}
"""

email_dag = DAG('template_email_test',
                default_args={'start_date': datetime(2023, 4, 15)},
                schedule_interval='@weekly')
                
email_task = EmailOperator(task_id='email_task',
                           to='testuser@datacamp.com',
                           subject="{{ macros.uuid.uuid4() }}",
                           html_content=html_email_str,
                           params={'username': 'testemailuser'},
                           dag=email_dag)
---------------------------------------------------------------------------
Branching 

Branching in Airflow:
- Provides conditional logic
- Using BranchPythonOperator
- from airflow.operators.python import BranchPythonOperator

def branch_test(**kwargs):
  if int(kwargs['ds_nodash']) % 2 == 0:
    return 'even_day_task'
  else:
    return 'odd_day_task'

branch_task = BranchPythonOperator(
  task_id='branch_task',
  dag=dag,
  provide_context=True,
  python_callable=branch_test
)

start_task >> branch_task >> even_day_task >> even_day_task2
branch_task >> odd_day_task >> odd_day_task2

---------------------------------------------------------------------------
# Create a function to determine if years are different
def year_check(**kwargs):
    current_year = int(kwargs['ds_nodash'][0:4])
    previous_year = int(kwargs['prev_ds_nodash'][0:4])
    if current_year == previous_year:
        return 'current_year_task'
    else:
        return 'new_year_task'

# Define the BranchPythonOperator
branch_task = BranchPythonOperator(task_id='branch_task', dag=branch_dag,
                                   python_callable=year_check, provide_context=True)
# Define the dependencies
branch_task >> current_year_task
branch_task >> new_year_task

---------------------------------------------------------------------------
Creating a production pipeline 

- Running DAGs & Tasks
airflow tasks test <dag_id> <task_id> [execution_date]

- RUn a full DAG
airflow dags trigger -e <date> <dag_id>

- Operators reminder
  - BashOperator -> expects a bash_command
  - PythonOperator  -> expects a python_callable
  - EmailOperator
  - FileSensor -> requires filepath argument and might need mode or poke_interval attributes
  - BranchPythonOperator - requires a python_callable and provide_context=True. The callable must accept **kwargs

- Template reminders 

1. Open python3 interpreter
2. Import necessary libraries
3. At prompt, run help(Airflow object) -> help(BashOperator)
4. Look for a line that referencing the template_fields attribute

---------------------------------------------------------------------------
from airflow import DAG
from airflow.sensors.filesystem import FileSensor

# Import the needed operators
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from datetime import date, datetime

def process_data(**context):
  file = open('/home/repl/workspace/processed_data.tmp', 'w')
  file.write(f'Data processed on {date.today()}')
  file.close()

    
dag = DAG(dag_id='etl_update', default_args={'start_date': datetime(2023,4,1)})

sensor = FileSensor(task_id='sense_file', 
                    filepath='/home/repl/workspace/startprocess.txt',
                    poke_interval=5,
                    timeout=15,
                    dag=dag)

bash_task = BashOperator(task_id='cleanup_tempfiles', 
                         bash_command='rm -f /home/repl/*.tmp',
                         dag=dag)

python_task = PythonOperator(task_id='run_processing', 
                             python_callable=process_data,
                             dag=dag)

sensor >> bash_task >> python_task

---------------------------------------------------------------------------
from airflow import DAG
from airflow.sensors.filesystem import FileSensor
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from dags.process import process_data
from datetime import timedelta, datetime

# Update the default arguments and apply them to the DAG
default_args = {
  'start_date': datetime(2023,1,1),
  'sla':timedelta(minutes=90)
}

dag = DAG(dag_id='etl_update', default_args=default_args)

sensor = FileSensor(task_id='sense_file', 
                    filepath='/home/repl/workspace/startprocess.txt',
                    poke_interval=45,
                    dag=dag)

bash_task = BashOperator(task_id='cleanup_tempfiles', 
                         bash_command='rm -f /home/repl/*.tmp',
                         dag=dag)

python_task = PythonOperator(task_id='run_processing', 
                             python_callable=process_data,
                             provide_context=True,
                             dag=dag)

sensor >> bash_task >> python_task

---------------------------------------------------------------------------
from airflow import DAG
from airflow.sensors.filesystem import FileSensor
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from airflow.operators.python import BranchPythonOperator
from airflow.operators.empty import EmptyOperator
from airflow.operators.email import EmailOperator
from dags.process import process_data
from datetime import datetime, timedelta

# Update the default arguments and apply them to the DAG.

default_args = {
  'start_date': datetime(2023,1,1),
  'sla': timedelta(minutes=90)
}
    
dag = DAG(dag_id='etl_update', default_args=default_args)

sensor = FileSensor(task_id='sense_file', 
                    filepath='/home/repl/workspace/startprocess.txt',
                    poke_interval=45,
                    dag=dag)

bash_task = BashOperator(task_id='cleanup_tempfiles', 
                         bash_command='rm -f /home/repl/*.tmp',
                         dag=dag)

python_task = PythonOperator(task_id='run_processing', 
                             python_callable=process_data,
                             provide_context=True,
                             dag=dag)

email_subject="""
  Email report for {{ params.department }} on {{ ds_nodash }}
"""

email_report_task = EmailOperator(task_id='email_report_task',
                                  to='sales@mycompany.com',
                                  subject=email_subject,
                                  html_content='',
                                  params={'department': 'Data subscription services'},
                                  dag=dag)

no_email_task = EmptyOperator(task_id='no_email_task', dag=dag)

def check_weekend(**kwargs):
    dt = datetime.strptime(kwargs['execution_date'],"%Y-%m-%d")
    # If dt.weekday() is 0-4, it's Monday - Friday. If 5 or 6, it's Sat / Sun.
    if (dt.weekday() < 5):
        return 'email_report_task'
    else:
        return 'no_email_task'
    
branch_task = BranchPythonOperator(task_id='check_if_weekend',
                                   python_callable=check_weekend,
                                   provide_context=True,                                
                                   dag=dag)

sensor >> bash_task >> python_task

python_task >> branch_task >> [email_report_task, no_email_task]
---------------------------------------------------------------------------
Pelajari yg lain:
- XCom
- Connections
- Refer to the Airflow documentation


------------------------------------
YOUTUBE
Apache airflow:
Open source tool which we can create, schedule, and monitor workflows.

https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html

docker compose up airflow-init
-------------------------------------------------------------------------
What is Airflow?
- Starts in Airbnb 2014
- Manage complex workflows
- Top-level Apache project 2019
- Workflow management platform 
- Written in Python 

Manage workflows:
Workflow -> DAG (Directed Acylic Graph)

A -> C -> E
A -> B -> D

- Directed, there is an inherent flow representing dependencies between components
- Acylic, does not loop/cycle/repeat
- Graph, the actual set of components
- Seen in Airflow, Apache Spark, dbt

Task -> unit of work within a DAG
A, B, C, D, E -> tasks

- >> upstream operator -> before
- << downstream operator -> after

Operator -> defines the work to be done
- BashOperator
- PythonOperator
- EmailOperator
- SimpleHttpOperator
- ...

Operator determines what is going to be done in the task
In the DAG, we define the order of the tasks

operator -> task -> DAG

Execution date -> logical date and time which the DAG is running
Task instance -> specific occurrence of a task
DAG run -> instance of a DAG that runs at a specific execution date

-------------------------------------------------------------------------
Task lifecycle:
- When a DAG is triggered. Its tasks are going to be executed one after another according to their dependencies.
8 stages of a task:
- queued
- running
- success
- failed 
- up_for_retry
- up_for_reschedule
- upstream_failed 
- skipped
- scheduled
- no_status

no_status -> scheduler created empty task instance
schedules -> scheduler determined task instance needs to run 
upstream_failed -> the task's updtream task failed
skipped -> the task's condition was not met

no status -> (scheduler) scheduled -> (executor) queued -> (worker) running -> success, failed, shutdown -> if max retries not reached, up_for_retry

-------------------------------------------------------------------------
import pendulum

local_tz = pendulum.timezone("Asia/Jakarta")  # Ubah ke zona waktu lokal
start_date=datetime(2024, 3, 13, 9, 0, tzinfo=local_tz)  # 13 Maret 2024, 09:00 WIB

-------------------------------------------------------------------------
Airflow XCom:
- Cross-communication between tasks
- Share small amount of data between tasks
- XCom push and pull
- XComArg
- XComPull
- XComPush

Kamu set schedule_interval='@daily', yang artinya DAG akan dijadwalkan setiap hari pada awal hari (00:00 UTC).

- Kalau catchup=False, maka DAG akan dijalankan hanya untuk tanggal eksekusi terbaru.
Misal startdate 2025-3-10, sekarang tanggal 14 Maret 2025, maka DAG akan dijalankan hanya untuk tanggal 13 Maret 2025. dan next schedule akan dijalankan untuk tanggal 14 Maret 2025.
Jadi jika ingin dijalankan juga dari tgl 10 maka gunakan backfill untuk mengisi tanggal-tanggal yang terlewat.

docker exec -it <airflow-scheduler-container-id> bash
airflow dags backfill -s 2025-03-01 -e 2025-03-12 dag_with_catchup_and_backfill_v02

-------------------------------------------------------------------------
Schedule interval -> datetime.timedelta or cron expression

None -> no schedule
@once -> run once
@hourly -> run every hour -> 0 * * * *
@daily -> run every day -> 0 0 * * *
@weekly -> run every week -> 0 0 * * 0
@monthly -> run every month -> 0 0 1 * *
@yearly -> run every year -> 0 0 1 1 *

-------------------------------------------------------------------------
docker compose up -d --no-deps --build postgres -> rebuild postgres container

Two ways to install python dependencies to airflow docker container:
- Image extending 
- Image customising

Extending:
docker build . --tag extending_airflow:latest
docker compose up -d --no-deps --build airflow-webserver airflow-scheduler

-------------------------------------------------------------------------
Sensor 
- Special type of operator which waits for something to occur
- Use case:
    - don't know exact time when the file exists
    
airflow connections add 'minio_conn' \
    --conn-type 's3' \
    --conn-extra '{"aws_access_key_id":"ROOTNAME","aws_secret_access_key":"CHANGEME123", "host":"http://host.docker.internal:9000"}'

docker run -it \
   -p 9000:9000 \
   -p 9001:9001 \
   --name minio \
   --network airflow_tutorial_default \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=minioadmin" \
   -e "MINIO_ROOT_PASSWORD=minioadmin123" \
   quay.io/minio/minio server /data --console-address ":9001"





















