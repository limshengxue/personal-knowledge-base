2025-09-28 12:47

# 7 - Gradio MCP Server
## Gradio MCP
- Gradio provides straightforward way to create MCP servers
	- Automatic convert function into MCP tools
	- Maps input components to tool argument schema
	- Determine response format from output component
	- Setup JSON-RPC over HTTP+SSE
	- Create both web interface and MCP server endpoint

### Code Sample
```python
import json
import gradio as gr
from textblob import TextBlob

def sentiment_analysis(text: str) -> str:
    """
    Analyze the sentiment of the given text.

    Args:
        text (str): The text to analyze

    Returns:
        str: A JSON string containing polarity, subjectivity, and assessment
    """
    blob = TextBlob(text)
    sentiment = blob.sentiment
    
    result = {
        "polarity": round(sentiment.polarity, 2),  # -1 (negative) to 1 (positive)
        "subjectivity": round(sentiment.subjectivity, 2),  # 0 (objective) to 1 (subjective)
        "assessment": "positive" if sentiment.polarity > 0 else "negative" if sentiment.polarity < 0 else "neutral"
    }

    return json.dumps(result)

# Create the Gradio interface
demo = gr.Interface(
    fn=sentiment_analysis,
    inputs=gr.Textbox(placeholder="Enter text to analyze..."),
    outputs=gr.Textbox(),  # Changed from gr.JSON() to gr.Textbox()
    title="Text Sentiment Analysis",
    description="Analyze the sentiment of text using TextBlob"
)

# Launch the interface and MCP server
if __name__ == "__main__":
    demo.launch(mcp_server=True)
```

Start the server by `python app.py`

Test the Web Interface by http://localhost:7860

Test the MCP by http://localhost:7860/gradio_api/mcp/schema


### Troubleshooting
- Type Hints and Docstring
	- Provide type hints for parameters and return avalue
	- Include a docstring with an Args block
- String input:
	- Accept input argument as `str` when in doubt
	- Convert them to desired type inside the function
- SSE Connection
	- Some client doesnt support SSE
	- use `mcp-remote` instead
```json
{
  "mcpServers": {
    "gradio": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "http://localhost:7860/gradio_api/mcp/sse"
      ]
    }
  }
}
```


# References
