2025-06-05 16:51

# 7 - Airflow Executors
- Executors run tasks
- Different executors handle running the tasks differently
- Can be determined from `airflow.cfg`, the line with "executor="
	- Can check using the command `cat airflow/airflow.cfg | grep "executor = "`
- Can also use `airflow info`

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
