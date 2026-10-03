2025-06-05 17:50

Tags: [[airflow]] [[pipeline]] [[3 - Tags/kubernetes]] [[runner]]

# Airflow Executor
- Executors run tasks
- Different executors handle running the tasks differently
- Can be determined from `airflow.cfg`, the line with "executor="
	- Can check using the command `cat airflow/airflow.cfg | grep "executor = "`
- Can also use `airflow info`
- Poorly configured executor can resulted in DAG unable to run on schedule
	- Lack of resources
	- Blocking task slot (using Sequential together with blocking  [[Airflow Sensors]])

## Sequential Executors
- The default executor for Airflow
- Runs one task at a time
- Useful for debugging
- Not recommended for production

## Local Executor
- Runs on a single system
- Treat tasks as processes
- Parallelism: Start tasks concurrently as many as possible or as defined by the user
- Can utilize all resources of a given host system

## Kubernetes Executor
- Multiple Airflow systems as workers
- Kubernetes as task manager
- More difficult to setup and configure


# References
[[7 - Airflow Executors]]
[[8 - Common Troubleshooting]]