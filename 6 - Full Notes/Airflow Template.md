2025-06-05 21:25

Tags: [[airflow]] [[pipeline]] [[jinja]]

# Airflow Template
- Allow substituting information during a DAG run
- Provide added flexibility when defining tasks
- Created using Jinja templating language
- Several fields of the operators can support like the `subject` in `EmailOperator`, `bash_command` in `BashOperator`
- Can run `help(<operator>)` like `help(BashOperator)` to decide which fields can accept template

## Simple Example Use with Bash Operator (Echo Filename)
```python
templated_command = """
	echo "Reading {{params.filename}}"
"""

t1 = BashOperator(
	task_id = "template_task",
	bash_command = templated_command,
	params = {'filename' : 'file1.txt'},
	dag = example_dag
)
```

## Advanced Jinja Use - Loop
```python
templated_command = """
{% for filename in params.filenames %}
	echo "Reading {{filename}}"
{% endfor %}
"""

t1 = BashOperator(
	task_id = "template_task",
	bash_command = templated_command,
	params = {'filenames' : ['file1.txt', 'file2.txt']},
	dag = example_dag
)

```
- However using a list of params will result in the commands success and failed together in a single task
- Using separate tasks could yield better monitor and scheduling with parallelism


## Variables
- Built-in runtime variables provided by Airflow that can be used in the template
- Provides information about DAG runs, tasks
- Examples include: `ds` , `ds_nodash`, 
```python
templated_command = """
	echo "Reading {{params.filename}} {{ds_nodash}}"
"""

t1 = BashOperator(
	task_id = "template_task",
	bash_command = templated_command,
	params = {'filename' : 'file1.txt'},
	dag = example_dag
)
```

### Macros Variable
- Reference to Airflow macros package which provides various useful objects/methods for Airflow templates
- `{{macros.datetime}}` - The datetime object
- `{{macros.uuid}}` - The uuid object in Python


# References
[[10 - Airflow Template]]
[[Airflow Operators]]
