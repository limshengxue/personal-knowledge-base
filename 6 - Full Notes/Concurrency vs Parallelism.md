2025-11-02 10:17

Tags: [[concurrency]] [[parallelism]]

# Concurrency vs Parallelism
## Concurrency vs Parallelism
- The idea of asynchronous code is sometimes called "concurrency" and it is different from parallelism
- Parallelism means to execute a tasks with multiple works, but still if IO Bound operation is there, the worker has to wait if the code is synchronous
- Parallelism should be used to handle CPU bound operations (tasks that need many working instead of waiting)
- Example of CPU bound operations
	- Audio/Image processing
	- Machine Learning/Deep Learning


# References
[[2 - Source Materials/Articles/FastAPI Concurrency and async await|FastAPI Concurrency and async await]]