2025-08-17 09:17

# 1 - Key Concepts and Terminology
## Purposes of MCP
- Enable AI models to connect with external data sources, tools, and environment
- Allow seamless transfer of information and capabilities between AI systems and digital worlds

## What is MCP
- "USB-C" for AI applications - consistent protocol for linking AI models to external capabilities
	- users enjoy simpler and more consistent experience across AI applications
	- AI application developers gain easy integration
	- tool and data providers need only create a single implementation to work with multiple AI application
	- increased interoperability, innovation, and reduced fragmentation
- without MCP, connecting model and tool create *M x N problem*
![[Attachments/Pasted image 20250817094116.png]]
- MCP transforms this into a M+N problem by providing standard interface
![[Attachments/Pasted image 20250817094158.png]]

## Terminologies in MCP
### Components
- Host 
	- the user-facing AI application that end users interact with directly
	- Examples: Anthropic Claude Desktop, Cursor
	- Initiate connections to MCP servers and orchestrate the overall flow between user requests, LLM processing, external tools
- Client
	- A component within the host application that manages communication with a specific MCP server. 
	- Each client maintain 1:1 connection with server
	- Handle protocol-level details of MCP communication 
- Server
	- External program or service that exposes capabilities via the MCP protocol

### Capabilities
- Tools: Executable functions that AI model can invoke to perform actions or retrieve computed data.
- Resources: read-only data sources that provide context without significant computation
- Prompts: pre-defined templates or workflows that guide interactions between users, AI models, and the available capabilities
- Sampling: Server-initiated requests for the Client/Host to perform LLM interactions, enabling recursive actions where the LLM can review generated content and make further actions


# References
