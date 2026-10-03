2025-06-05 15:32

Tags: [[airflow]] [[pipeline]]

# Airflow Scheduler
- A DAG run is one execution of an [[Airflow DAG]], with its own task instances and state.
- The scheduler creates scheduled runs and identifies tasks whose dependencies permit execution.
- A schedule is not a guarantee of immediate execution: capacity and dependencies still matter.

## Airflow 3 Scheduling Controls
- `schedule`: a cron expression, preset, timedelta, or supported timetable.
- `start_date`: the earliest boundary considered by the timetable, not simply an instruction to execute immediately.
- `end_date`: an optional scheduling boundary.
- `catchup`: whether missed scheduled intervals should be generated.
- `retries` and `retry_delay`: task/operator settings, often supplied through `default_args`; `max_tries` is not a DAG scheduling argument.
- Older `schedule_interval` examples need migration to `schedule`.

Cron has five fields: minute, hour, day of month, month, and day of week. Presets include `@hourly`, `@daily`, `@weekly`, and `@monthly`.

## Data Intervals and First Runs
- A data-interval timetable commonly schedules a run after its interval ends.
- “Start February 25, first run February 26” depends on the timetable, alignment, time zone, and catchup configuration.
- Do not apply that rule to every timetable. Airflow 3 changes the default cron timetable configuration.

## Example
```python
from datetime import timedelta
import pendulum
from airflow.sdk import DAG

dag = DAG(
    dag_id="update_dataflows",
    start_date=pendulum.datetime(2026, 1, 1, tz="UTC"),
    schedule="30 12 * * 3",
    catchup=False,
    default_args={
        "owner": "Engineering",
        "retries": 3,
        "retry_delay": timedelta(minutes=20),
    },
)
```

The cron expression selects Wednesday at 12:30 in the DAG's time zone.

## Scheduler Troubleshooting
- Check scheduler health, DAG import errors, paused state, and task capacity.
- `airflow scheduler` starts the scheduler component; a complete deployment also needs its other configured components.

# References
[[5 - Airflow Schedule]]
[[Airflow DAG]]
[Airflow 3 scheduling changes](https://airflow.apache.org/docs/apache-airflow/3.1.7/installation/upgrading_to_airflow3.html)
