2025-11-02 09:36

Tags: [[gen ai]] [[mcp]]

# MCP Prompt
- Allow user to use high quality, well-tested prompt instead of prompting on their own
- Defines a set of User and Assistant messages that can be used by the client
- *User-controlled* - execution decide by user

## MCP Server
```python
@mcp.prompt(
    name = "format",
    description="Rewrites the content of the document in Markdown format.",
)
def format_document(
    doc_id: str = Field(description = "Id of the document to format"),
) -> list[base.Message]:
    prompt = f"""
    You are a document formatting assistant.
    Your task is to rewrite the content of the document in Markdown format.
    The id of the document you need to reformat is
    <doc_id>
    {doc_id}
    </doc_id>
    Add in headers, bulleted lists, numbered lists, and other formatting as appropriate.
    Use the 'edit_documnet' tool to make the changes. After the document is formatted.
    """

    return [
        base.UserMessage(content=prompt)
    ]
```

## MCP Client
```python
    async def list_prompts(self) -> list[types.Prompt]:
        result = await self.session().list_prompts()
        return result.prompts

    async def get_prompt(self, prompt_name, args: dict[str, str]):
        result = await self.session().get_prompt(prompt_name, args)
        return result
```


# References
[[Prompt]]