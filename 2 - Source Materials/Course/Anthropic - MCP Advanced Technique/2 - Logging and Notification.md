2025-10-26 08:50

# 2 - Logging and Notification
- Notifications emitted from server to help the client track the status of long-running tasks
- In Python MCP SDK, logging and progress are done through the Context arguments provided to tool function of the MCP Server
	- `info` - for logging
	- `report_progress` - for progress update
- Client then implement 2 callbacks to handle logging and progress update

## Implementation
Tool Call use the Context argument to log and update progress
```python
@mcp.tool()
async def add(a: int, b: int, ctx: Context) -> int:
    await ctx.info("Preparing to add...")
    await ctx.report_progress(20, 100)

  
    await asyncio.sleep(2)

    await ctx.info("OK, adding...")
    await ctx.report_progress(80, 100)

    return a + b
```

Client define the callbacks to handle the logging and progress update from server
```python
async def logging_callback(params: LoggingMessageNotificationParams):
    print(params.data)

  
async def print_progress_callback(
    progress: float, total: float | None, message: str | None
):
    if total is not None:
        percentage = (progress / total) * 100
        print(f"Progress: {progress}/{total} ({percentage:.1f}%)")
    else:
        print(f"Progress: {progress}")

  
async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read, write, logging_callback=logging_callback
        ) as session:

            await session.initialize()

            await session.call_tool(
                name="add",
                arguments={"a": 1, "b": 3},
                progress_callback=print_progress_callback,
            )
```



# References
