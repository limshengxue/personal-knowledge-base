2026-10-03 15:43

Tags: [[mcp]] [[gen ai]]

# Gradio MCP Servers
- Gradio can expose application functions as MCP tools while also providing a browser interface.
- Document the function's purpose and parameters so clients can discover usable tool schemas.
- This is an implementation option for [[MCP Tool]], not a different protocol.

## Minimal Example
Install the MCP dependencies with `pip install "gradio[mcp]"`, then save this example in an application project:

```python
import gradio as gr


def word_count(text: str) -> int:
    """Count whitespace-separated words.

    Args:
        text (str): Text to count.
    """
    return len(text.split())


demo = gr.Interface(
    fn=word_count,
    inputs=gr.Textbox(),
    outputs=gr.Number(),
    title="Word Count",
)

if __name__ == "__main__":
    demo.launch(mcp_server=True)
```

## Connect and Inspect
- Run `python app.py` when the example is saved as `app.py`.
- Use the server URL printed at startup; the default local endpoint is typically `http://localhost:7860/gradio_api/mcp/`.
- Inspect `/gradio_api/mcp/schema` or the app's API page for generated tools.
- Copy configuration appropriate to the chosen client rather than assuming the source's older SSE endpoint is required. [Gradio MCP guide](https://gradio.app/guides/building-mcp-server-with-gradio).

## Practical Checks
- Confirm parameter names, types, descriptions, and return values.
- Test both the web interface and a harmless MCP call.
- Check installed-version transport support before adding a bridge.
- Keep the server local unless remote exposure and authentication are intentional.
- Review tool side effects before granting broad access through [[MCP Client Configuration and Integration]].

# References
[[2 - Source Materials/Course/MCP - HF/7 - Gradio MCP Server|7 - Gradio MCP Server]]

