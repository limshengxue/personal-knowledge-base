2025-06-05 14:00

# 1 - Introduction to Airflow
## Workflow
- A set of steps to accomplish a given data engineering task
	- Download file, copy data
- Of varying levels of complexity
- Carries various meaning depending on context

## Airflow
- Platform to program workflow
	- Create, schedule, and monitor
- Can implement programs from any language, but workflows are written in Python
- Workflow implemented as DAG: Directed Acyclic Graphs
- Accessed via code, CMD, web interface

## DAG
- Directed Acyclic Graph
- Set of tasks that make up the workflow
- Consists of tasks and dependencies between tasks
- Carries attributes like name, start date, etc

## Basic Commands
### Test Run
`airflow tasks test <dag_id> <task_id> <start_date>`
Sample:
`airflow tasks test etl_pipeline download_file 2023-01-08`

### Check Command Available
`airflow`


# References




