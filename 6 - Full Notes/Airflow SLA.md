2025-06-05 17:59

Tags: [[airflow]] [[pipeline]] [[sla]]

# Airflow SLA
- Timing alerts help detect a workflow that fails to meet its expected completion boundary.
- Distinguish an alert from a task timeout: sending a notification does not necessarily stop execution.
- See [[Airflow Scheduler]] for scheduling boundaries and [[Airflow Tasks]] for execution behavior.

## Airflow 3 Deadline Alerts
- Classic Airflow SLAs were removed in Airflow 3.
- Deadline Alerts were introduced in 3.1; the documented feature remains experimental and may change between releases.
- A deadline specifies a reference point, an interval, and a callback.
- Possible reference points include DAG-run queue time and supported logical-date references.
- Confirm the installed release's callback classes and API before copying an example.
- Deadline Alerts are not a drop-in reproduction of every legacy SLA behavior.

## Legacy Airflow 2 SLAs
- Older notes use an operator's `sla` argument, sometimes supplied through `default_args`.
- Legacy SLA timing relates to the logical schedule, not merely elapsed runtime after a task starts.
- Keep those examples only when maintaining a compatible Airflow 2 deployment.
- Use execution timeout or DAG-run timeout when the requirement is to fail work after a configured duration.

# References
[[9 - SLA and Reporting in Airflow]]
[Deadline Alerts](https://airflow.apache.org/docs/apache-airflow/stable/howto/deadline-alerts.html)
