2025-06-05 14:38

# 4 - Airflow Operators and Tasks

## Operators

- Represent a single task in a workflow
- Run independently
- Generally do not share information

### Bash Operator

- Executes a given Bash command or script
- Runs the command in a temporary directory
- Can specify environment variables

```python
# Import the BashOperator
from airflow.operators.bash import BashOperator

with DAG(dag_id="test_dag", default_args={"start_date": "2024-01-01"}) as analytics_dag:
  # Define the BashOperator 
  cleanup = BashOperator(
      task_id='cleanup_task',
      # Define the bash_command
      bash_command='cleanup.sh',
  )
```

### Python Operator
- Executes a Python function / callable
- Operates like Bash Operator with more option
```python
from airflow.operators.python import PythonOperator

def printme():
	print("Hello World")
	
python_task = PythonOperator(
	task_id = 'simple_print', 
	python_callable = printme
)
```

Passing arguments using `op_kwargs`
```python
def sleep(length_of_time):
	time.sleep(length_of_time)

sleep_task = PythonOperator(
	task_id = 'sleep',
	python_callable = sleep,
	op_kwargs = {'length_of_time' : 5}
)
```

### Email Operator
```python
from airflow.operators.email import EmailOperator

email_task = EmailOperator(
	task_id = 'email_sales_report',
	to = 'sales_manager@example.com',
	subject = 'Automated Sales Report',
	html_content = 'test email',
	files = 'latest_sales.xlsx'
)
```


### Operators Common Problem
- Not guaranteed to run in same location/environment
- May require extensive use of Environment variables
- Can be difficult to run tasks with elevated privileges


## Task
- Instances of operator
- Referred to using task ID in Airflow

### Task Dependencies
- Define a given order of task completion
- Are not required for a given workflow, but usually present
- When not given, Airflow auto-decide with no guarantee in order of execution
- Are referred to as upstream or downstream tasks
- Defined using bit-shift operator
	- `>>` - upstream
	- `<<` - downstream
- Upstream = before
- Downstream = after
- `task_1 >> task_2`


# References
