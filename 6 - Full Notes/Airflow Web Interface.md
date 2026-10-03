2025-06-05 15:26

Tags: [[pipeline]] [[airflow]]

# Airflow Web Interface
- The web interface helps inspect [[Airflow DAG]], [[Airflow Scheduler|scheduled runs]], and [[Airflow Tasks]].
- Airflow 3 serves the interface through the API server:

```bash
airflow api-server --port 9090
```

This starts one component, not an entire production deployment. Configure authentication and access before exposing it.

## Useful Views
- DAG list: ownership, schedule, pause state, and recent runs.
- Graph/grid and task details: dependencies, task states, attempts, and logs.
- Run history: inspect failures and execution timing.
- Audit information and available code views depend on release and permissions.

## Legacy Airflow 2
- `airflow webserver -p 9090` is an older webserver command.
- Names such as Tree view are release-specific; do not assume they match the current interface.

# References
[[3 - Airflow Web Interface]]
[Airflow CLI](https://airflow.apache.org/docs/apache-airflow/stable/cli-and-env-variables-ref.html)
