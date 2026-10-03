2025-11-02 10:17

Tags: [[concurrency]] [[parallelism]]

# Concurrency vs Parallelism
- Concurrency means multiple tasks can make progress over overlapping periods.
- Parallelism means work executes simultaneously on multiple execution resources.
- Asynchronous programming is one way to express concurrency, not its definition.

## Choosing an Approach
- I/O-heavy workloads can benefit from concurrency while operations wait for external results.
- CPU-heavy workloads may benefit from parallel execution when the runtime and hardware permit it.
- Threads, processes, coroutines, and distributed workers have different costs and constraints.
- `await` does not by itself start several operations concurrently; the program must arrange overlapping work.
- Neither approach removes dependencies, shared-state hazards, or the need to measure the workload.

# References
[[2 - Source Materials/Articles/FastAPI Concurrency and async await|FastAPI Concurrency and async await]]
[FastAPI concurrency](https://fastapi.tiangolo.com/async/)
