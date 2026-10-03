2025-06-05 17:50

Tags: [[airflow]] [[pipeline]] [[3 - Tags/kubernetes]] [[runner]]

# Airflow Executor
- An executor determines how runnable [[Airflow Tasks]] are executed after [[Airflow Scheduler]] queues them.
- Inspect the configured executor with `airflow config get-value core executor` or `airflow info`.
- Insufficient execution capacity and blocking [[Airflow Sensors]] can delay a workflow.

## LocalExecutor
- Runs task processes on the local execution host.
- Concurrency is constrained by Airflow configuration and available host resources.
- It is the default executor in Airflow 3, not a promise that every deployment uses it.

## KubernetesExecutor
- Launches an individual [[Kubernetes Pod]] for each task instance.
- It does not require a separate complete Airflow system per task.
- Requires compatible provider configuration, cluster credentials, pod configuration, and access to task code.
- Pod startup and scheduling affect latency.

## Legacy SequentialExecutor
- Older Airflow releases included an executor running one task at a time.
- Airflow 3 removes SequentialExecutor; use the executor supported by the installed release.

# References
[[7 - Airflow Executors]]
[[8 - Common Troubleshooting]]
[Airflow 3 migration](https://airflow.apache.org/docs/apache-airflow/3.1.7/installation/upgrading_to_airflow3.html)
