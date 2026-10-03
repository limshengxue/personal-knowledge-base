2025-06-05 15:24

Tags: [[pipeline]] [[airflow]]

# Airflow DAG
- A workflow is a set of steps that accomplishes a task, such as downloading, transforming, and loading data.
- A directed acyclic graph (DAG) expresses task dependencies without cycles.
- Airflow defines workflows in Python; individual tasks can run programs in other languages.
- [[Airflow Operators]] define work, and [[Airflow Scheduler]] determines when runs and runnable tasks are scheduled.

## Airflow 3 Example
This example requires Airflow 3 and the standard provider:

```python
import pendulum
from airflow.sdk import DAG
from airflow.providers.standard.operators.empty import EmptyOperator

with DAG(
    dag_id="etl_workflow",
    start_date=pendulum.datetime(2026, 1, 1, tz="UTC"),
    schedule="@daily",
    catchup=False,
    default_args={"owner": "jdoe", "retries": 1},
) as etl_dag:
    begin = EmptyOperator(task_id="begin")
    finish = EmptyOperator(task_id="finish")
    begin >> finish
```

## Discovery and CLI
- DAGs must be discoverable through the configured DAG folder or bundle.
- `airflow dags list` lists discovered DAGs.
- `airflow dags list-import-errors` identifies loading failures.
- `airflow dags trigger etl_workflow` creates a manual run.
- Check the installed CLI's `--help` before adding logical-date options; old examples using `-e` are version-specific.

## Reporting
- Operator defaults can include `email_on_failure` and `email_on_retry` when email is configured.
- `email_on_success` is not a standard operator default. Use a supported success callback or notifier.
- Task retries, DAG-run monitoring, and deadline alerts solve different problems; see [[Airflow Tasks]] and [[Airflow SLA]].

# References
[[2 - Airflow DAG]]
[[8 - Common Troubleshooting]]
[[9 - SLA and Reporting in Airflow]]
[[1 - Introduction to Airflow]]
[Airflow CLI](https://airflow.apache.org/docs/apache-airflow/stable/cli-and-env-variables-ref.html)
