2025-06-05 17:05

# 8 - Common Troubleshooting
## DAG wont' run on schedule
### Scheduler not running
- Error will show in Airflow Web UI
- Can fix by running `airflow scheduler`

### Executor not enough free slots
- Change executor type
- Add system resources
- Add more systems
- Change DAG scheduling

## DAG won't load
### Python File not in DAG Folder
- Verify DAG in correct folder
- DAG folder specified in `airflow.cfg`

### Syntax Error in Python
- Run `airflow dags list-import-errors`


# References
