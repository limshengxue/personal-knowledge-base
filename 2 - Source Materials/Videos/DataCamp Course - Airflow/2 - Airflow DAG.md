2025-06-05 14:04

# 2 - Airflow DAG
## What is DAG
- Directed, there are inherent flow representing dependencies between tasks
- Acyclic, does not loop/cycle
- Graph, the actual set of components

## DAG in Airflow
- Written in Python (components can be written in other language)
- Are made up of components (usually tasks)
- Contain dependencies defined explicitly or implicitly

## Define a DAG
### In Python
```python
from airflow import DAG
from datetime import datetime


default_arguments = {
	'owner': 'jdoe',
	'start_date' : datetime(2020, 1, 20)
}

with DAG('etl_workflow' , default_args=default_arguments) as etl_dag:

```

### Using CMD
Show DAG
`airflow dags list`
 
# References
