2025-06-05 15:32

Tags: [[airflow]] [[pipeline]]

# Airflow Scheduler
## DAG Runs
- Specific instance of a workflow at a point in time
- Can be run manually or via `schedule_interval`
- Maintain state and tasks within
- State:
	- success
	- failed
	- running

## Schedule 
- `start_date` - The date/time to initially schedule the DAG run
- `end_date` - When to stop running (Optional)
- `max_tries` - How many attemtps (Optional)
- `schedule_interval` - How often to run
	- Cron syntax
		- 5 fields - minute, hour, day of month, month, day of week
	- Preset
		- `@hourly`
		- `@daily`
		- `@weekly
		- `@monthly`

### Common Issue
- Airflow use `start_date` as the earliest possible date
- When we use 25 Feb, daily, the first execution will be on 26 Feb

## Sample Schedule Code
```python
# Update the scheduling arguments as defined

default_args = {
  'owner': 'Engineering',
  'start_date': datetime(2023, 11, 1),
  'email': ['airflowresults@datacamp.com'],
  'email_on_failure': False,
  'email_on_retry': False,
  'retries': 3,
  'retry_delay': timedelta(minutes=20)
}

dag = DAG('update_dataflows', 
default_args=default_args, 
schedule_interval='30 12 * * 3') # Every Wed 12:30
```

##  Scheduler not running
- When scheduler not running, DAG cannot be executed
- Error will be shown in web UI
- We can start scheduler using  `airflow scheduler`


# References
[[5 - Airflow Schedule]]
[[Airflow DAG]]