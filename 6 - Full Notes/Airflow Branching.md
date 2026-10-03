2025-06-05 21:27

Tags: [[airflow]] [[pipeline]] 

# Airflow Branching
- Branching chooses which downstream [[Airflow Tasks]] should run.
- Use `BranchPythonOperator` or a supported branching decorator from [[Airflow Operators]].
- Return a downstream task ID or supported collection of IDs; other branches are skipped.
- A join after branching needs an appropriate trigger rule, because skipped branches can otherwise prevent it from running.

## Airflow 3 Example
Requires the standard provider. Context can be requested by parameter name; `**kwargs` and the old `provide_context=True` flag are not mandatory.

```python
import pendulum
from airflow.sdk import DAG
from airflow.providers.standard.operators.empty import EmptyOperator
from airflow.providers.standard.operators.python import BranchPythonOperator

def choose_branch(ds_nodash):
    return "even_day_task" if int(ds_nodash) % 2 == 0 else "odd_day_task"

with DAG(
    dag_id="branch_example",
    start_date=pendulum.datetime(2026, 1, 1, tz="UTC"),
    schedule="@daily",
    catchup=False,
):
    branch = BranchPythonOperator(
        task_id="branch_task",
        python_callable=choose_branch,
    )
    even_day = EmptyOperator(task_id="even_day_task")
    odd_day = EmptyOperator(task_id="odd_day_task")
    branch >> [even_day, odd_day]
```

Returned IDs must match the actual downstream tasks.

# References
[[11 - Branching]]
[[Airflow Tasks]]
[Python and branching operators](https://airflow.apache.org/docs/apache-airflow-providers-standard/stable/operators/python.html)
