2025-06-05 20:54

# 12 - Creating a production pipeline
## Execute Task / Pipeline
To run a specific task
`airflow tasks test <dag_id> <task_id> <date>`

To run a full DAG
`airflow dags trigger -e <date> <dag>`

Set `<date>` to -1 for instant execution


# References
