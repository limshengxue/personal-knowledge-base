2025-06-05 21:31

Tags: [[airflow]] [[pipeline]]

# Airflow Sensors
- A special operator that waits for a certain condition to be True
	- Eg. creation of a file, upload of a database record
- Can define how often to check for the condition
- Are assigned to tasks
- Use the sensor base class and concrete provider supported by the installed release; old `base_sensor_operator` imports are legacy.
- Sensor arguments:
- mode - How to check for the condition
	- poke - run repeatedly (default)
	- reschedule - Give up task slot and try again (refer to [[Airflow Executor]])
- poke interval - how often between checks
- timeout - how long to wait before failing task

## File Sensor
- Check for existence of a file
```python
from airflow.providers.standard.sensors.filesystem import FileSensor

file_sensor_task = FileSensor(task_id = 'file_sense',  filepath = 'sales_data.csv', poke_interval = 300)
```

## Other Sensors
- `ExternalTaskSensor` - wait for a task in another DAG
- `HttpSensor` - Request web URL and check for content
- `SQLSensor` - Issue SQL statement and check for content

## Why use Sensor
- Uncertain when a condition will be met
- Check for a condition continuously instead of failing the DAG
- Add task repetition without loops


## Version and Capacity
- The FileSensor import above targets the standard provider used with Airflow 3.
- `poke` occupies a worker slot while waiting; `reschedule` releases it between checks.
- Deferrable sensors can use a triggerer when the provider and deployment support them; this is different from reschedule mode.
- Provider names include `SqlSensor`, not universally `SQLSensor`.

# References
[[6 - Airflow Sensor]]
