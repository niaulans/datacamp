
## [Introduction to Apache Airflow in Python](https://app.datacamp.com/learn/courses/introduction-to-apache-airflow-in-python)

### Data Engineering
```
Taking any action involving data and turning it into a reliable, repeatable, and maintainable process.
```

### Workflow
```
- A set of steps to accomplish a given data engineering task
  - Such as: downloading files, copying data, filtering information, writing to a database, etc.
- Varying levels of complexity
- A term with various meanings depending on context

Workflow example:
- Download sales data
- Clean sales data
- Run ML pipeline
- Upload results to web server
- Generate report and email to CEO
```

### Airflow
```
- A platform to program workflows, including:
  - Creation
  - Scheduling
  - Monitoring
- Can implement programs from any language, but workflows are written in Python
- Implements workflows as DAGs: Directed Acyclic Graphs
- Accessed via code, command-line, or web interface/REST API

Other workflow tools:
- Luigi
- SSIS
- Bash scripting
```

### DAGs
```
Directed Acyclic Graph:
- Directed: there is an inherent flow representing dependencies between components
- Acyclic: does not loop/cycle/repeat
- Graph: the actual set of components

- In Airflow, this represents the set of tasks that make up your workflow
- Consists of the tasks and the dependencies between tasks
- Created with various details about the DAG, including the name, start date, owner, etc.
- Seen in Airflow, Apache Spark, dbt
```

### Simple DAG
```python
etl_dag = DAG(
  dag_id='etl_pipeline',
  default_args={"start_date": "2024-01-08"}
)
```

### Running a workflow in Airflow
```bash
airflow tasks test <dag_id> <task_id> [execution_date]
airflow tasks test example_etl download-file 2024-01-10

airflow -h  # to see all the commands
```
