2025-11-02 09:37

Tags: [[gen ai]] [[mcp]]

# MCP Sampling
MCP Server need to access LLM
- For example, when building a "research tool", we might collect some articles from the web and use LLM to synthesis a final report
- This add complexity to MCP server with code to call the LLM API
- With sampling, MCP Server ask the MCP Client to run a prompt on its behalf
- MCP Client already have connection to LLM, with little complexity added (adding a `sampling_callback` to handle the sampling message from server)
- Sampling is useful for *publicly available MCP Server*

![[Attachments/Pasted image 20251026082917.png]]

## Implementation
Initiating Sampling at the Server
```python
@mcp.tool()

async def summarize(text_to_summarize: str, ctx: Context):
    prompt = f"""
        Please summarize the following text:
        {text_to_summarize}
    """

    result = await ctx.session.create_message(
        messages=[
            SamplingMessage(
                role="user", content=TextContent(type="text", text=prompt)
            )
        ],
        max_tokens=4000,
        system_prompt="You are a helpful research assistant.",
    )

    if result.content.type == "text":
        return result.content.text
    else:
        raise ValueError("Sampling failed")
```


Adding a callback method to handle the sampling message at the client
```python
async def sampling_callback(
    context: RequestContext, params: CreateMessageRequestParams
):

    # Call Claude using the Anthropic SDK
    text = await chat(params.messages)

    return CreateMessageResult(
        role="assistant",
        model=model,
   content=TextContent(type="text", text=text),

    )
```

Connect the Callback to ClientSession
```python
async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read, write, sampling_callback=sampling_callback
        ) as session:

            await session.initialize()

            result = await session.call_tool(
                name="summarize",
                arguments={"text_to_summarize": "lots of text"},
            )
            print(result.content)
```

Might need formatting to ensure the MCP message is compatible with the LLM SDK
```python
async def chat(input_messages: list[SamplingMessage], max_tokens=4000):
    messages = []
    for msg in input_messages:
        if msg.role == "user" and msg.content.type == "text":
            content = (
                msg.content.text
                if hasattr(msg.content, "text")
                else str(msg.content)
            )

          messages.append({"role": "user", "content": content})

        elif msg.role == "assistant" and msg.content.type == "text":
            content = (
                msg.content.text
                if hasattr(msg.content, "text")
                else str(msg.content)
            )
           messages.append({"role": "assistant", "content": content})
```



# References
[[1 - Sampling]]
