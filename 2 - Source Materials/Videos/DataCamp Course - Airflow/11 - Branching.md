2025-06-05 20:46

# 11 - Branching
- Provides conditional logics
- Using `BranchPythonOperator`
- `from airflow.operators.python import BranchPython`
- Takes a python callable to return the next task ID to be executed
- Defining dependencies is mandatory to allow branching

## Branching Examples
```python 
def branch_test(**kwargs): # must accept kwargs
	if int(kwargs['ds_nodash']) % 2 == 0:
		return 'even_day_task'
	else:
		return 'odd_day_task'

branch_task = BranchPythonOperator(
	task_id = 'branch_task',
	provide_context = True, # to provide macros and runtime variables to the function (must set to True for this operator)
	python_callable = branch_test
)

branch_task >> even_day_task
branch_task >> single_day_task
```


# References
