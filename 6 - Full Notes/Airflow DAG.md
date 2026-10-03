2025-06-05 15:24

Tags: [[pipeline]] [[airflow]]

# Airflow DAG
## What is DAG
- Directed, there are inherent flow representing dependencies between tasks
- Acyclic, does not loop/cycle
- Graph, the actual set of components

## DAG in Airflow
- Written in Python (components can be written in other language)
- Are made up of components (usually tasks)
- Contain dependencies defined explicitly or implicitly
- DAG must be placed in DAG folder, which can be found in `airflow.cfg` or `airflow info`

## Define a DAG In Python
```python
from airflow import DAG
from datetime import datetime


default_arguments = {
	'owner': 'jdoe',
	'start_date' : datetime(2020, 1, 20)
}

with DAG('etl_workflow' , default_args=default_arguments) as etl_dag:

```

## CLI Tool
- Useful for troubleshooting or determine information 
- Show DAG - `airflow dags list`
- Show error in DAG - `airflow dags list-import-errors`
- To run a DAG - `airflow dags trigger -e <date> <dag>` (set date to -1)
 
## DAG Reporting
- For reporting purpose, we can configure alert according to different status of the DAG
```python
default_args = {
	'email' : [''],
	'email_on_failure' : True,
	'email_on_retry' : False,
	'email_on_success' : True,
}
```

# References
[[2 - Airflow DAG]]
[[8 - Common Troubleshooting]]
[[9 - SLA and Reporting in Airflow]]