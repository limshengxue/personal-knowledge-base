2025-06-05 17:59

Tags: [[airflow]] [[pipeline]] [[sla]]

# Airflow SLA
SLA: Service Level Agreement
- In Airflow, the amount of time a task or a DAG should run
- An *SLA Miss* is any time the task/DAG does not meet the expected timing
- When there is a *SLA Miss*
	- Email will be sent out according to config
	- Log
	- Can view in web UI

### Defining SLA
Specify at task level
```python
task1 = BashOperator(
	sla = timedelta(seconds = 30),
	...
)
```

Specify at DAG level (apply to all tasks of the DAG)
```python
default_args = {
	'sla' : timedelta(minutes = 20)
}

dag = DAG('sla_dag', default_args = default_args)



```

# References
[[9 - SLA and Reporting in Airflow]]