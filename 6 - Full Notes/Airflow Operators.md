2025-06-05 21:33

Tags: [[airflow]] [[pipeline]]

# Airflow Operators
Operators define reusable task behavior; instantiating one inside a [[Airflow DAG]] creates a task. Tasks may execute on different workers, so do not rely on local files or in-process variables to share data. Use small XCom values or external storage deliberately.

## Bash and Python Tasks
This Airflow 3 example requires the standard provider. The Python function receives arguments through `op_kwargs`.

```python
import time

import pendulum
from airflow.sdk import DAG
from airflow.providers.standard.operators.bash import BashOperator
from airflow.providers.standard.operators.python import PythonOperator


def pause(length_of_time: int) -> None:
    time.sleep(length_of_time)


with DAG(
    dag_id="operator_examples",
    start_date=pendulum.datetime(2026, 1, 1, tz="UTC"),
    schedule=None,
    catchup=False,
) as dag:
    greeting = BashOperator(
        task_id="greeting",
        bash_command="echo 'Hello from Bash'",
    )
    wait_task = PythonOperator(
        task_id="pause",
        python_callable=pause,
        op_kwargs={"length_of_time": 5},
    )
    greeting >> wait_task
```

BashOperator uses a temporary working directory when no `cwd` is supplied. A referenced script must be available in the execution environment; the example uses an inline command instead.

## Email Tasks
Install the SMTP provider and configure its connection before using EmailOperator. This fragment belongs inside a DAG and requires the attachment to exist on the executing worker:

```python
from airflow.providers.smtp.operators.smtp import EmailOperator

email_task = EmailOperator(
    task_id="email_sales_report",
    conn_id="smtp_default",
    to="sales_manager@example.com",
    subject="Automated Sales Report",
    html_content="The report is attached.",
    files=["latest_sales.xlsx"],
)
```

Airflow 2 examples commonly use `airflow.operators.bash` and `airflow.operators.python`; current standard-provider imports are different. See [[Airflow Tasks]] for dependencies, [[Airflow Sensors]] for waiting, and [[Airflow Executor]] for execution placement.

# References
[[4 - Airflow Operators and Tasks]]
[Standard provider operators](https://airflow.apache.org/docs/apache-airflow-providers-standard/stable/operators/index.html)
[SMTP EmailOperator](https://airflow.apache.org/docs/apache-airflow-providers-smtp/stable/_api/airflow/providers/smtp/operators/smtp/index.html)
