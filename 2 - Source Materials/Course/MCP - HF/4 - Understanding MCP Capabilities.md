2025-09-28 10:23

# 4 - Understanding MCP Capabilities
## Tools
- Executable functions that the AI model can invoke
	- Control: Model-controlled, LLM decide when to call them
	- Safety: Usually require explicit user approval as side effects can be dangerous

## Resources
- Provide read-only access to data sources
- Allow retrieve context without executing complex logic

## Prompts
- Predefined templates or workflows that guide the interaction between user, AI model, and server capabilities
- Often presented as options in *host* application UI

## Sampling
- Allows servers to request Client (specifically the Host application) to perform LLM interactions
- Allow recursive or multi-step interactions
Follow steps
1. Server sends a `sampling` request to the Client
2. Client reviews the request and can modify it
3. Client samples from an LLM
4. Client review the completion
5. Client return the result to Server

## Summary
![[Attachments/Pasted image 20250928102919.png]]

## Discovery Process
- Dynamic capability discovery. Allow MCP server to list its capabilities to Client


# References
