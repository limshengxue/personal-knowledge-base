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
- Defined using bit-shift operator
	- `>>` - upstream
	- `<<` - downstream
- Upstream = before
- Downstream = after
- `task_1 >> task_2` -  execute and complete `task1` before executing `task2`
- Throw error when cycle is detected
- Easily visualize in graph view of [[Airflow Web Interface]]


# References
[[4 - Airflow Operators and Tasks]]
[[Airflow DAG]]
