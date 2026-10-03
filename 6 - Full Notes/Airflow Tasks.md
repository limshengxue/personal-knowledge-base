2025-06-05 15:28

Tags: [[pipeline]] [[airflow]]

# Airflow Tasks
- Components that made up a [[Airflow DAG]] workflow
- Usually an instance of an [[Airflow Operators]] or [[Airflow Sensors]]
- Referred to using task ID in Airflow
- To run a task in CLI `airflow tasks test <dag_id> <task_id> <date>`

## Task Dependencies
- Define a given order of task execution
- Are not required for a given workflow, but usually present
- When not given, Airflow auto-decide with no guarantee in order of execution
- Are referred to as upstream or downstream tasks
- Define edges using bit-shift operators: `task_1 >> task_2` means task_1 is upstream and task_2 is downstream. `task_2 << task_1` expresses the same edge.
- Upstream = before
- Downstream = after
- With the default `all_success` trigger rule, `task_2` waits for successful upstream completion. Other trigger rules can change that requirement.
- Throw error when cycle is detected
- Easily visualize in graph view of [[Airflow Web Interface]]


# References
[[4 - Airflow Operators and Tasks]]
[[Airflow DAG]]
