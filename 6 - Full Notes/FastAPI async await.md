2025-11-02 10:15

Tags: [[fastapi]] [[concurrency]]

# FastAPI async await
## Asynchronous Code
- The programming language has way to tell the computer that at some point in the code, it need to wait for something else to finish somewhere else
- So, during that time, the computer can go and do some other work and comeback later
- When the waiting task finish, computer continue
- In contrast, synchronous code refers to as *sequential* because the computer don't switch to other task
- Asynchronous code sometimes conceptually refer to as concurrency and it is different with parallelism [[Concurrency vs Parallelism]] 

### IO Bound
- The operation that require waiting usually slow I/O operations like
	- waiting for data over network
	- waiting for data from disk
	- waiting for data from db
	- etc
- These are IO bound operations

### Coroutines
- A term referring to the thing that return by `async` function


## Path operation function in FastAPI
FastAPI support asynchronous code by allow path operation function to be `async`
```python
@app.get('/burgers') 
async def read_burgers(): 
	burgers = await get_burgers(2) 
	return burgers
```
- When a path operation function is `def` instead of `async def` it is run in an external thread pool that is then awaited, instead of being called directly
- Same applies to dependencies


# References
[[FastAPI Concurrency and async await]]